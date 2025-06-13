---
sidebar_position: 1
---

# What is Serverless?

<figure>
  <img src="/img/serverless/cold_start.png" alt="Serverless" width="700" height="auto" />
  <figcaption style={{ textAlign: "center" }}>Cold Start</figcaption>
</figure>

## SST introduction

Create a file at `sst.config.ts`:

```jsx title="src/pages/my-react-page.js"
export default $config({
  app(input) {
    return {
      name: "aws-nextjs",
      removal: input?.stage === "production" ? "retain" : "remove",
      home: "aws",
    };
  },
  async run() {
    const bucket = new sst.aws.Bucket("MyBucket", {
      access: "public",
    });
    new sst.aws.Nextjs("MyWeb", {
      link: [bucket],
    });
  },
});
```

A new page is now available at [sst-next-v2.vercel.app](sst-next-v2.vercel.app).
