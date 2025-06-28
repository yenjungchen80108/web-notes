---
sidebar_position: 1
---

# Benchmarking Next.js 15 with Serverless vs SSR: Architecture, Cold Start, and Real Load Test Insights

## Part 1: The Architectural Shift in Next.js 15

Before v15, Next.js developers constantly had to navigate ambiguities and limitations:

First, there are blurred boundaries between Server and Client. It wasn’t clear whether a component would execute server-side or in the browser. Second, SSR, SSG, and ISR are confusing; A wrong choice may lead to caching bugs or slow performance. Finally, cache or revalidate settings are complicated, and each rendering mode has its own rules that developers often misconfigure.

Example: You want the homepage to revalidate every 10 seconds, but mistakenly configure ISR with no fallback, resulting in stale content being cached indefinitely.

Then it comes to Next.js 15, which finally addresses these issues:

1. Explicit Server and Client Components
   You can now clearly define where a component runs.
2. Streaming + Suspense + Fetch integration
   Enables granular, partial data fetching — closely aligned with Serverless on-demand execution.
3. Improved Deployment Flexibility
   Easily deploy to platforms like Vercel, SST, or AWS Lambda with better SSR support. These improvements make Next.js 15 a natural fit for Serverless.

## Part 1.5: What Is Serverless, Really?

Serverless doesn’t mean there’s no server. It means developers focus only on writing logic (functions) without provisioning, scaling, or maintaining infrastructure.

<figure>
    <img src="/img/serverless/faas_intro.png" alt="Serverless process diagram" width="500" />
</figure>

fig1. Serverless flow (general version)

As illustrated above, when a user sends a request, the load balancer forwards it to a request handler. The instance manager then checks whether a warm container for the function already exists. If none is available, a new container is spun up, the function code is executed, and finally the container is torn down.

This pay-per-use model is highly cost-efficient: you only pay for the exact number of invocations and compute time you consume, with no standing fees. Lambda automatically scales out under load, ensuring you have sufficient capacity to handle traffic spikes without overprovisioning. Furthermore, since each execution runs in its own function module, deployments and maintenance remain granular and easy to manage.

Cloud function or AWS Lambda is a function in serverless computing. An anatomy of a Lambda function looks like this:

```js
export const handler = async (event, context) => {
  return {
    statusCode: 200,
    body: JSON.stringify({ message: "Hello from Lambda" }),
  };
};
```

```
handler: entry point
event: incoming data
context: metadata (timeout, memory)
```

Lambda can be separated into init phase and invoke phase, as the init phase, like loading global dependencies, will only execute once when the container starts; Whereas the invoke phase, like the handler function itself will execute every time by request.

Understanding the Execution Flow: Cold Start vs. Warm Start

<figure>
    <img src="/img/serverless/cold_warm_start_full.png" alt="Serverless process diagram" width="500" />
</figure>

fig2. Serverless process diagram — cold and warm start (handcrafted detailed version)
Let’s walk through the execution flow of a serverless function and break down the difference between a cold start and a warm start.

When a user triggers a request, the system first checks whether a function instance already exists. If it does, it proceeds with a warm start. If not, it initiates a cold start, which is where things get a bit slower.

A cold start can take several hundred milliseconds to a few seconds, since it involves spinning up a brand-new lightweight virtual machine (like a Firecracker microVM), downloading and deploying your code package, and initializing the network interface for the function.

Once the VM is up, it checks whether the function code has already been cached. If not, it downloads and deploys it from scratch. This phase — ranging anywhere from 10ms to multiple seconds — is part of what’s known as the custom runtime lifecycle. During this phase, you can define how your function initializes, including what libraries or configurations it requires.

(In the architecture diagram, this corresponds to the yellow section.)

Now here’s where SST comes in handy.

SST is an open-source framework that makes it easy to build, deploy, and manage serverless applications on AWS. With SST, deploying your code to AWS becomes much easier. It automatically handles code packaging, Lambda function creation, and production deployment. Once deployed, the network setup begins: SST pre-establishes the Elastic Network Interface (ENI) and initializes the VPC. This process helps reduce the networking overhead of your Lambda function.

In contrast, when a warm start occurs (i.e. an instance already exists), the system reuses that instance. This typically takes only a few milliseconds. Warm instances scale automatically and quickly, making them ideal for bursts of traffic.

The Runtime API: The Brain Behind the Handler
Both cold and warm starts rely on the Runtime API to execute your function’s handler and initialize its environment, like database connections, SDKs, or other async setup logic.

The Runtime API runs in a loop, constantly waiting for new events. It checks for incoming GET /invocation/next calls — regardless of whether they come from a cold or warm instance — and once an event arrives, it invokes the function and sends back a POST /invocation/response or …/error.

This loop handles the entire execution lifecycle.

Optimizing the Cold Start
Cold starts are slower, but they’re not hopeless. Several optimizations help reduce latency. In the diagram, these are primarily highlighted in blue:

1. Faster VM Startup
   Using lightweight VMs like Firecracker, you can bring up an environment in around 125ms, far quicker than traditional VMs that require full OS booting.
2. Code Caching
   Downloading Lambda packages from S3 takes time. To avoid repeated downloads, platforms cache common functions in memory, improving start-up speeds dramatically.
3. ENI Caching & VPC Lattice
   Initializing the VPC takes time, but caching the Elastic Network Interface (ENI) or using VPC Lattice can streamline and accelerate the process.

SST Warmer: Keeping SSR Lambdas Warm
What about real-world SSR use cases?

<figure>
    <img src="/img/serverless/ssr_warmer.webp" alt="Serverless process diagram" width="500" />
</figure>
fig3. ssr warmer directory snapshot
SST provides a built-in SSR warmer mechanism. It regularly pings your Lambda functions (usually every 5 minutes) to keep them in a warm state — especially useful for pages that rely on Server-Side Rendering.

Without warming, users might experience a noticeable lag the first time they hit a cold Lambda. But with SST’s warmer in place, first-byte latency is significantly reduced.

## Part 2: Project Walkthrough — Comparing Serverless vs. Traditional SSR

In this section, I’ll walk you through a simple experiment I conducted to compare performance and user experience between two deployment strategies for the same Next.js application: traditional SSR and Serverless architecture.

<figure>
    <img src="/img/serverless/demo_page.png" alt="Serverless process diagram" width="500" />
</figure>
fig4. Image Uploader page snapshot
I built a minimal image upload feature where:

- Users can upload an image to an S3 bucket.
- After uploading, the app opens a new tab to immediately preview the image.

This feature was deployed using two different methods:

<figure>
    <img src="/img/serverless/ssr_serverless.png" alt="Serverless process diagram" width="500" />
</figure>
fig5. SSR vs. Serverless deployment approach
To measure performance differences, I used load testing tool K6 to simulate multiple users uploading images concurrently and analyzed the system behavior under stress.

Here’s a simplified view of the two architectures:

<figure>
    <img src="/img/serverless/proj_flow.png" alt="Serverless process diagram" width="500" />
</figure>
fig6. SSR vs. Serverless process diagram
SSR (Left Side of the Diagram):
The user uploads an image through the frontend.
The request goes through the Next.js server (hosted on Vercel).
The server handles the S3 upload (usually by generating a presigned URL).
After the image is uploaded, the URL is returned to the frontend for preview.
Serverless (Right Side of the Diagram):
The user clicks the upload button.
The frontend fetches a presigned URL from an API endpoint.
The image is directly uploaded to S3 from the client.
An AWS Lambda function handles additional logging or returns metadata.
Although both architectures use presigned URLs, Serverless fully bypasses the intermediate server — directly leveraging AWS infrastructure — which theoretically results in lower latency and better scalability.

Introducing SST (Serverless Stack Toolkit)
We’ve mentioned SST before, so why choose SST?

SST is a framework that simplifies deploying modern serverless apps. It offers one-command deployment, with support for Next.js, React, and Vue, enables hot reload for local lambda emulation, automates generation of preview URLs and CloudFront Distributions, and provides seamless integration with AWS resources like S3, Lambda, and API Gateway.

In short, SST removes the knitty-gritty part of configuring IAM, CI/CD pipelines, or infrastructure YAMLs. You can define everything in a single TS file (sst.config.ts)

```js
export default $config({
  app(input) {
    return {
      name: "aws-nextjs",
      removal: input?.stage === "production" ? "retain" : "remove",
      home: "aws",
    };
  },
  async run() {
    const bucket = new sst.aws.Bucket("MyBucket", { access: "public" });
    new sst.aws.Nextjs("MyWeb", { link: [bucket] });
  },
});
```

This configuration sets the app name and controls resource removal based on deployment stage, creates a public S3 bucket and wraps the Next.js app as a lambda function, deployed to CloudFront + S3.

If you’re developing locally, run sst dev to launch a local preview server with Lambda logs and real-time debugging. If in production mode, run sst deploy --stage to deploy a CloudFront-hosted production app in seconds.

For complete installation with Next.js, please refer to this link.

Auto-Generated Lambda Functions
After deployment, we can observe that SST automatically created the following Lambda functions in AWS Console:

<figure>
    <img src="/img/serverless/convert_func.png" alt="Serverless process diagram" width="500" />
</figure>
fig7. SST auto-generated func meanings
For more complex apps (e.g., live stock analysis or push notification systems), SST automatically scales the number and role of Lambda functions according to use cases.

This setup allows us to compare identical user-facing functionality under two deployment models and prepare the groundwork for load testing and performance metrics analysis in the next section.

## Part 3: Load Testing & Performance Analysis — Serverless vs. SSR

After building and deploying our two versions of the image upload feature, it was time to put them to the test.

Our main goal:
Can a Serverless architecture outperform traditional SSR in high-concurrency scenarios?
Or will cold starts and resource scaling limits become the bottlenecks?

Load Testing Setup
I use Grafana K6 to simulate a growing number of concurrent users uploading images to each site. Below is the test config:

<figure>
    <img src="/img/serverless/test_config.png" alt="Serverless process diagram" width="500" />
</figure>
fig8. K6 testing setup (\*VU: virtual user)
Here is the K6 testing script:

```js
import http from "k6/http";
import { check } from "k6";

export const options = {
stages: Array.from({ length: 20 }, (\_, i) => ({
duration: "30s",
target: (i + 1) \* 5, // 5, 10, ..., 100 VUs
})),
thresholds: {
http_req_failed: ["rate<0.01"], // failure rate <= 1%
http_req_duration: ["p(95)<1500"], // 95% req time < 1500ms
},
};
```

```js
export default function () {
  const res = http.get("your-testing-url", { timeout: "10s" });
  check(res, {
    "res code 200": (r) => r.status === 200,
  });
}
```

The test simulates a flash crowd uploading images simultaneously to determine if the system can handle the load or breaks under pressure.

Voila! Test Results

<figure>
    <img src="/img/serverless/ssr_perf.png" alt="Serverless process diagram" width="500" />
</figure>
fig9–1. SSR
<figure>
    <img src="/img/serverless/sst_perf.png" alt="Serverless process diagram" width="500" />
</figure>
fig9–2. SST (Serverless)
Throughput (Requests Made & RPS)

Serverless achieved a higher total request volume and peak requests per second (RPS) compared to SSR (1.3M > 970K req, 4218 > 3405 peak RPS). This confirms Serverless's strength: Elastic scalability — Lambda functions scale quickly to handle bursts of load.

Failure Rate (HTTP Failures)

However, Serverless also encountered a higher number of HTTP failures under peak load (1.2M > 738K). Possible reasons include:

- Cold start delays
- Functions not scaling up fast enough
- Lambda concurrency limits temporarily exceeded
- API Gateway throttling or timeouts

While Serverless scales elastically, there’s a ramp-up curve, and during that window, requests can fail or timeout more frequently.

Latency (P95 Response Time)

Interestingly, SSR had a lower 95th percentile latency (44ms) than Serverless (52ms), suggesting that always-on servers are better at maintaining consistent, low-latency response times, especially when no cold start penalty exists.

Real-World Theory Comparison
These findings match the conclusions from Chapter 12 of Serverless Architecture Deep Dive:

- SSR servers perform consistently under load with low latency.
- Serverless functions can scale faster and handle surges better, but may suffer from cold start penalties and scale lag.

A metaphor to explain this is you can see SSR as a car with a warm engine, it is ready to drive immediately, and Serverless is a rental car, it starts only when needed, but takes a moment to drive.

When Should You Use Serverless?
It depends on your application’s traffic pattern and performance needs. Serverless is ideal for burst-heavy workloads, cost-sensitive apps, or startups needing rapid iteration without infrastructure overhead. In contrast, SSR is a better fit for low-latency, always-on systems that rely on backends like SEO-optimized pages or dashboards.

In conclusion, with the growing adoption of Next.js 15 App router, developers now have access to fine-grained SSR control. Combined with the advantages of serverless architecture, low-cost, event-driven scalability. This pairing offers a high-performance foundation for modern web apps powered by an intelligent, auto-scaling backend.
