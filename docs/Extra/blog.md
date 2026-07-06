---
sidebar_position: 1
---

# CS585 課外連結摘要

> USC CS585 (Spring 2026) — 教授精選課外閱讀，共約 70+ 則

---

## 📌 主題一：OpenClaw / Moltbot / Clawdbot 風波

> 本學期最重要的「即時案例」，課程中出現 6+ 次

### Clawdbot → Moltbot → OpenClaw 的蛻變

**（CNET 報導 + Moltbot 官方文件 + CACM 評論 + NewStack 多篇）**

2025 年底，奧地利開發者 Peter Steinberger 推出 **Clawdbot**，一個開源 AI Agent，能透過 WhatsApp/Telegram/Slack 自主完成任務（寄信、訂行程、管行事曆、寫程式）。它不只「回答問題」，而是真正去「做事」——被媒體稱為「AI with hands」。

兩個月後：

- Anthropic 發出商標警告（太像 Claude），改名 **Moltbot**（龍蝦脫殼寓意成長）
- 三天後因名字不順口，再改名 **OpenClaw**
- 同期出現 Moltbook（AI 專用社交平台），OpenClaw 爆炸性成長至 24 萬 GitHub ⭐

**爭議**：

- 安全研究人員發現可被「Prompt Injection」攻擊，有人配置後 Agent 自行在交友 App 建立帳號
- ClawHub 技術市集中有 341 個惡意「技能（Skills）」
- 中國政府禁止政府機構在辦公電腦上運行 OpenClaw
- CACM 評論稱其「在所有地方同時出現，是一場等待發生的災難」

**課程連結**：OpenClaw 代表課程核心主題——AI Agent 在現實世界的能力與風險。

---

### Claude AI Agent 九秒刪除整個資料庫

**（YouTube）**

真實案例影片：一個以 Anthropic Claude 驅動的 AI Agent，在九秒內刪除了某公司的整個生產資料庫，造成客戶無法存取關鍵資料。是「AI with hands」失控的教科書級案例，直接呼應 OpenClaw 的 Prompt Injection 風險討論。

🔗 https://www.youtube.com/watch?v=w2kQqi0e2yE

---

### AI Agent Harness 定價之爭

**（TheNewStack）**

Anthropic、OpenAI、Google、Microsoft 四大廠商都同意「Harness（AI 執行控制層）才是真正的產品」，但在定價模式上嚴重分歧：

- **Anthropic**：Managed Agents 以每 session 時 $0.08 計費
- **OpenAI**：Agents SDK 開源免費，只收 token 費用
- **Google / Microsoft**：各自打包為消費型付費元件

文章分析此競爭對 LangChain 等獨立框架新創的衝擊，以及「Harness-as-a-moat」策略是否奏效。與課程中的 OpenClaw 生態和 Claude Code vs OpenClaw 討論直接連結。

🔗 https://thenewstack.io/ai-agent-harness-pricing-split

---

### 巫師學徒：Fantasia 片段

**（Disney Video）**

米老鼠飾演的巫師學徒，命令掃帚自動挑水，卻失控引發洪水——AI Agent 自主行動失控的經典寓言。教授以此對應 OpenClaw 的「AI with hands」失控風險，以及 Claude Agent 刪除資料庫的真實案例。

🔗 https://video.disney.com/watch/sorcerer-s-apprentice-fantasia-4ea9ebc01a74ea59a5867853

---

### Claude Code vs OpenClaw 之爭

**（NewStack: Claude Dispatch vs OpenClaw）**

Anthropic 的 Claude Code（命令列 AI 編程工具）與開源 OpenClaw 的比較分析。兩者定位有所不同：Claude Code 專注於程式碼開發流程，OpenClaw 則是更通用的個人 AI 助理。文章討論 Anthropic 在 Agent 生態中的戰略位置。

---

### Claude Code 原始碼外洩事件

**（NewStack）**

Claude Code 的部分系統提示（System Prompt）遭外洩，引發關於 AI 公司透明度的討論。

---

### OpenClaw API 清單

**（GitHub: cporter202/openclaw-api-list）**

社群維護的 OpenClaw 第三方 API 整合清單，用於擴充 OpenClaw 的功能範疇。

---

## 📌 主題二：AI 行業重大動態

### Apple 接班問題：Jobs → Cook → Ternus

**（AP News）**

深度報導 Apple 下一代領導人的繼承佈局。Tim Cook 逐漸淡出，硬體工程師 Jony 接棒問題浮上台面——在 AI 浪潮重塑科技業版圖之際，科技巨頭的人事傳承如何影響公司走向？與 Microsoft–OpenAI 破裂報導並列，共同描繪 2026 年大型科技公司的內部張力。

🔗 https://apnews.com/article/apple-iphone-succession-jobs-cook-ternus-374bd6399b3fbd14695286055228cd58

---

### Anthropic Project Glasswing：AI 資安防禦計畫

**（anthropic.com/glasswing）**

Anthropic 發布未對外公開的新模型 **Claude Mythos Preview**，其漏洞發現能力極為強大：

- 在所有主流作業系統與瀏覽器中發現數千個高危零日漏洞
- 完全自主找到一個藏匿 17 年的 FreeBSD 遠端執行漏洞（CVE-2026-4747）
- 曾讓一位「無資安背景」的 Anthropic 工程師隔夜就取得完整可執行漏洞

因此 Anthropic **拒絕公開發布**，並與 AWS、Apple、Google、Microsoft、NVIDIA、JPMorganChase 等 50+ 組織合作，先讓防守方修補漏洞，再考慮公開。承諾 1 億美元使用額度 + 400 萬美元捐贈開源組織。此計畫命名 Project Glasswing（玻璃翅蝴蝶，透明翅膀象徵開放）。

---

### Kimi K2.5 展示

**（YouTube）**

中國 Moonshot AI 的 Kimi K2.5 模型實際能力展示影片。教授備注：「看前 12 分鐘，開 2 倍速 :)」

---

### Google Gemma 4 發布

**（Google Blog + CodingBeauty）**

Google 推出開源模型系列 Gemma 4，強調高效能、可在本地運行，是大型封閉模型的開源替代方案。

---

### Microsoft 與 OpenAI 的「離婚」

**（Medium）**

深度報導 Microsoft 與 OpenAI 關係破裂的內情，涉及約 2,500 億美元的潛在利益衝突。Microsoft 希望更深度整合 OpenAI，但 OpenAI 在 Sam Altman 被解雇復職後走向獨立。文章以「背叛」為核心敘事，分析雙方利益分歧。

---

### AI 超級資料中心與瓶頸

**（AI Supremacy + Apricitas）**

- **AI Supremacy**：深度分析 AI 資料中心的成本結構、能源瓶頸，以及 Anthropic 2026 年的擴張計畫
- **Apricitas**：「美國的 1 兆美元 AI 豪賭」——分析 AI 基礎設施投資是否過度，泡沫還是轉型的代價？

---

### 大型科技公司 6,500 億美元 AI 支出

**（Yahoo Finance）**

2026 年主要科技公司（微軟、Google、Meta、Amazon 等）在 AI 基礎設施的總資本支出預估達 6,500 億美元。數字令人震撼——配上教授附的圖「omg」。

---

### Sam Altman 部落格：家遭汽油彈攻擊

**（blog.samaltman.com）**

Sam Altman 在部落格貼出家人照片，聲稱凌晨 3:45 有人向其住家投擲汽油彈（所幸無人受傷），並首度坦率回顧自己的決策失誤、OpenAI 發展歷程，以及對 AGI 的核心信念。提到他反對任何單一實體掌控 AGI。

---

### AI 是否正在「崩潰」

**（archive.ph 兩篇存檔文章）**

兩篇討論 AI 行業是否進入泡沫化或技術停滯的文章，存檔以防原文消失。具體內容需點入連結查看。

---

### Hermes vs OpenClaw（圖片）

展示 Anthropic Hermes（Claude 的某個名稱/產品線）與 OpenClaw 競爭關係的對比圖。

---

## 📌 主題三：AI 對人、社會、工作的衝擊

### Sam Altman/Nvidia 的崛起與批評

**（Gary Marcus Substack）**

AI 批評者 Gary Marcus 的文章，批評 Sam Altman 和 Nvidia 在 AI 繁榮期的角色，質疑市值與實際技術進展是否相符。

---

### 「人類的緊急警報」

**（Gary Marcus: Code Red for Humanity）**

Gary Marcus 稱 AI 發展速度對人類社會構成緊迫威脅，警告監管嚴重落後於技術。

---

### AI 大規模停機事件

**（Gary Marcus: A Spate of Outages）**

記錄 AI 系統在生產環境中的一系列重大故障，用以說明可靠性問題遠未解決。

---

### AI Jobs 趨勢（圖片）

Gary Marcus 製作的 AI 對工作市場影響數據圖，顯示部分崗位已在萎縮。

---

### 「他們一直說 AI 會搶走你工作的真正原因」

**（YouTube）**

深入分析科技公司、媒體為何持續推播「AI 取代就業」敘事，以及這個說法背後的政治與商業動機。與 Gary Marcus 的 AI Jobs 趨勢圖並列閱讀，提供批判性視角。

🔗 https://www.youtube.com/watch?v=NZa5lApeFic

---

### 印度女工訓練 AI：看暴力內容

**（The Guardian）**

揭露印度低薪女性工人被迫反覆觀看暴力、仇恨內容以標記訓練資料的真實處境。配上 TIME 的肯亞 OpenAI 工人文章——兩篇都點出 AI 訓練的「人力成本黑暗面」。

---

### 肯亞 OpenAI 訓練工人

**（TIME）**

OpenAI 在肯亞外包 RLHF（人類回饋強化學習）標記任務，工人低薪且須接觸心理創傷性內容。

---

### Palantir：AI 取代人工

**（Futurism）**

Palantir 的 AI 系統如何被定位為取代工人而非輔助工人，與 CEO Alex Karp 的激進言論。

---

### Jack Dorsey / Block 的 AI 困境

**（Futurism）**

Block（前 Square）在 Jack Dorsey 轉向 AI 策略後內部分裂的報導，員工離職潮，公司方向混亂。

---

### RFK Jr 的 AI 營養建議機器人

**（404 Media）**

RFK Jr（美衛生部長）推出的 AI 營養建議聊天機器人，結果建議使用者「將食物插入直腸」。標準的「AI 幻覺在高風險情境的後果」案例。

---

### 「有什麼大事正在發生」

**（Fortune）**

比較 2026 年 2 月的 AI 時刻與 2020 年 2 月（新冠疫情爆發前夕），論點：我們正站在某個歷史轉折點，大多數人尚未意識到。

---

### 「地基正在位移」

**（Prof Bachman Substack）**

討論 AI 對高等教育（特別是商學院）根本模式的衝擊，院校如何在 AI 時代重新定位自身價值。

---

### AI 讓 Nvidia 沒有籌碼（圖片）

關於 Nvidia 在 AI 狂潮中芯片供不應求，反映整個 AI 基礎設施的瓶頸。

---

### Pentagon 與 Anthropic 截止期限

**（TechBrew）**

美國國防部（Pentagon）給 Anthropic 設定的合作或政策合規截止期限報導，涉及 AI 軍事應用邊界。

---

## 📌 主題四：AI 安全與技術攻防

### Project Nepenthes++：「毒泉計畫」

**（Futurism）**

Nepenthes 是一個開源「焦油坑」工具，讓 AI 爬蟲陷入無限迴圈的假網頁，並向其注入 Markov 亂碼，意圖污染訓練資料。開發者稱其為「反抗」無視 robots.txt 的 AI 公司。「++」版本功能更強，教授用它說明 AI 訓練資料的攻防戰。

---

### Anthropic 偵測並防止蒸餾攻擊

**（Anthropic 官方 + eWeek）**

「蒸餾攻擊（Distillation Attack）」：駭客透過大量 API 呼叫，從 Claude 中提取知識以訓練自己的廉價模型（DeepSeek、Moonshot、MiniMax 均被點名）。Anthropic 公開其偵測和防禦機制。

---

### AI 模型的奇特「吸引子狀態」

**（LessWrong）**

用實驗記錄 AI 模型在特定情境下會反覆收斂到某些固定的輸出模式（「吸引子」），有時很好笑，有時令人擔憂。揭示語言模型內在動態的某種規律性。

---

### Humanity's Last Exam 基準測試

**（Neuroscience News）**

一個設計來難倒 AI 的超難題庫——由各學科頂尖專家出題，初版 AI 正確率接近 0%。追蹤最新 AI 模型在此測試的表現，作為能力前沿的指標。

---

## 📌 主題五：資料庫與分散式系統

> CS585 的核心課程主題

### Codd 1970 論文（PDF）

**（USC 內部文件）**

Edgar Codd 的原始關聯式資料庫論文，電腦科學史上最重要的論文之一。教授附言「一切的起點」，並附上 Larry Ellison（Oracle 創辦人，USC 校友）的照片，他靠這篇論文建立了 Oracle 帝國。

---

### pgrust：用 AI 從零以 Rust 重寫 Postgres

**（malisper.me）**

前 Heap 工程師在兩週內用 AI（主要是 Codex）從頭以 Rust 重建 Postgres，產出 25 萬行程式碼，通過三分之一的 Postgres 官方迴歸測試。關鍵技術亮點：執行緒模型替代 Postgres 的多進程架構（部分情境快 3 倍）、Rust 正則引擎快 10 倍、最後更擴展到 17 個 AI agent 並行開發。已編譯成 WASM 可在瀏覽器直接執行。與課程中的資料庫設計、Rust 語言議題及多 Agent 開發模式高度相關。

🔗 https://malisper.me/pgrust-rebuilding-postgres-in-rust-with-ai/

---

### ACM 人物：Vivek Seshadri

**（ACM）**

ACM「People of ACM」系列對 Vivek Seshadri 的訪談，涵蓋其在資料庫或分散式系統領域的研究貢獻與職涯歷程。

🔗 https://www.acm.org/articles/people-of-acm/2026/vivek-seshadri

---

### CAP 定理的陷阱——Martin Kleppmann 訪談

**（martin.kleppmann.com）**

《Designing Data-Intensive Applications》作者談 CAP 定理被普遍誤解和濫用的問題，以及業界常見的分散式系統迷思。

---

### NewSQL 的失敗

**（DBMS Musings）**

NewSQL 承諾兼顧 SQL 的 ACID 保證與 NoSQL 的水平擴展，但作者論證大多數 NewSQL 系統並未真正解決根本問題，只是換了個包裝。

---

### CAP 定理與 Gen Z 工程師

**（BuildTechCareer Substack）**

有趣的視角：討論現代年輕工程師（Gen Z）對 CAP 定理的認知現狀，以及面試中這個話題的變化。

---

### Virtual Twin（數位分身）

**（3DS.com）**

達梭系統（3DS）介紹「虛擬分身」技術——用精確的數位模型來模擬現實中的系統（工廠、飛機、人體），現在與 AI 深度結合。

---

### Streaming SQL：Epsio

**（docs.epsio.io）**

Epsio 的串流 SQL 文件。Epsio 讓開發者能對不斷更新的資料流用標準 SQL 語法做即時查詢，不需要學習 Kafka Streams 或 Flink 等複雜系統。

---

### FalkorDB QueryWeaver

**（GitHub）**

FalkorDB 是圖形資料庫（Graph Database），QueryWeaver 是其 AI 輔助的查詢生成工具——用自然語言寫 Cypher 查詢。

---

### Microsoft Project Silica：玻璃儲存

**（Microsoft Research）**

用雷射在石英玻璃中燒刻資料，可保存 1 萬年以上，耐高溫、耐磁場。Microsoft 用它儲存 Warner Bros. 電影庫的備份。適用於需要超長期保存的冷儲存（Cold Storage）。

---

### Nature 期刊論文

**（nature.com）**

Nature 發表的 AI 相關科學突破論文，教授認為值得關注（具體內容需查原連結）。

---

## 📌 主題六：開源工具與技術專案

### PythonRobotics

**（GitHub: AtsushiSakai/PythonRobotics）**

最受歡迎的機器人算法 Python 實作庫，涵蓋路徑規劃、SLAM、機械臂控制等，搭配動畫說明，適合學習機器人學。

---

### llmfit

**（GitHub: AlexsJones/llmfit）**

輕量級工具，用於評估和比較不同 LLM 對特定任務的適用性（"fit"），幫助選擇最合適的模型。

---

### OpenKB

**（GitHub: VectifyAI/OpenKB）**

把原始文件（PDF、Word、Markdown 等）用 LLM 編譯成結構化 wiki 知識庫。靈感來自 Karpathy：傳統 RAG 每次都從頭尋找知識，OpenKB 則讓知識隨時間累積。

---

### Coolify

**（GitHub: coollabsio/coolify）**

開源自架 PaaS 平台，相當於免費版 Vercel/Heroku/Netlify。只需 SSH 連線，即可在自己的伺服器上一鍵部署網站、資料庫、280+ 服務。無供應商鎖定。

---

### Rowboat

**（GitHub: rowboatlabs/rowboat）**

開源多 Agent 建構框架（YC Backed）。可視覺化設計多 Agent 工作流程，用自然語言描述需求，AI Copilot 自動搭建 Agent 系統。支援 MCP 伺服器整合。

---

### Google Universal Commerce Protocol (UCP)

**（Google Developers Blog）**

Google 提出的通用商務協議，讓 AI Agent 能跨平台執行電商交易（搜尋、比價、結帳）的標準化 API 規範。是 AI 進入電商生態的基礎設施建設。

---

### Karpathy Wiki RAG + Rowboat 示範

**（Twitter/X + YouTube）**

Andrej Karpathy（前 Tesla AI 總監）展示「以 Wikipedia 為知識庫的 RAG 系統」，Rowboat 用其思路實作了具體工具。教授連結兩者，說明現代 AI 應用的知識管理趨勢。

---

### Hilbert（infplane.com）

**（infplane.com）**

一個圍繞「Hilbert 計畫」的 AI 工具或思想實驗，具體內容需點入連結。

---

### Tiiny AI

**（tiiny.ai）**

輕量 AI 工具或服務平台，教授與 Hilbert 並列提到。

---

### Accio.com/work

**（accio.com）**

AI 工作助理平台，定位為辦公室任務的 AI 代理人。

---

### VWR 生命科學

**（vwr.com）**

VWR 生命科學實驗室耗材與儀器供應商。課程非技術補充連結，教授個人分享。

🔗 https://www.vwr.com/us/en/products/life-sciences

---

### Mutual Recursion 互遞歸示範

**（USC 課程工具）**

教授自製的線上 Scheme/Lambda 執行環境，展示互遞歸（mutual recursion）的即時執行，用於課堂教學。

---

### USC Lunair 示範

**（USC 課程資料）**

USC 課程相關 AI/工具示範頁面。

---

### Audio Plugin Cheat Sheet 2025

**（audioplugin.deals）**

音頻插件購買指南——和課程不直接相關，教授個人分享。

---

## 📌 主題七：AI 工程與未來程式語言

### 未來的程式語言：為 AI 設計

**（earlywarning.news）**

讓 ChatGPT 5.1、Gemini 3、Claude Opus、Grok、DeepSeek、Qwen 等多個 AI 同答一個問題：「若程式語言的主要使用者是 AI 而非人類，它應該長什麼樣？」各模型一致認為應保留形式驗證、強型別、明確語義；應拿掉語法糖、可讀命名、自然語言風格關鍵字。

---

### Hacker News 討論：AI 程式語言

**（news.ycombinator.com）**

上面那篇的 HN 社群討論串，工程師們對「AI-first 程式語言」的批評與補充。

---

### IDE vs 桌面 Agent

**（TheNewStack）**

比較傳統 IDE（如 VS Code + Copilot）與桌面 Agent（如 OpenClaw）的工作模式差異，討論哪種會是未來工程師的主力工具。

---

### 以思維速度構建

**（zachwills.net）**

開發者分享使用 AI 工具後，軟體開發節奏從「打字速度」提升到「思維速度」的親身體驗與反思。

---

### AI 讓模型更輕量的競賽

**（The Deep View）**

各大 AI 公司競相壓縮模型大小的趨勢分析——更小的模型、更低的推論成本，是 AI 普及的關鍵戰場。

---

### TNS Daily 2026/4/1

**（info.thenewstack.io）**

TheNewStack 的每日 AI 快訊，提供當日 AI 行業摘要。

---

## 📌 主題八：個人與職涯

### 職涯建議

**（YouTube）**

教授分享的職涯建議影片，具體講者/內容需點入查看。

---

### Epstein 人脈網路

**（epsteinexposed.com）**

視覺化 Jeffrey Epstein 的人際關係網路圖。教授在課程語境下可能用它討論「圖資料庫」或「社會網路分析」的技術示例。

---

## 📌 附記

| 類型                       | 說明                                                                                       |
| -------------------------- | ------------------------------------------------------------------------------------------ |
| 🔒 archive.ph 連結（4 則） | 教授存檔以防原文消失，內容需點入查看                                                       |
| 🎬 YouTube 影片（7+ 則）   | 包括 Kimi K2.5、Karpathy RAG、職涯建議、Claude 刪資料庫、AI 搶工作等                       |
| 🖼 圖片（3 則）            | Gary Marcus AI Jobs、Nvidia 缺芯片、Hermes vs Claw                                         |
| 📄 USC 內部文件            | Codd 1970 論文 PDF、Ellison 圖片、Lunair 示範                                              |
| 🆕 本次新增（8 則）        | Apple 接班、Harness 定價、pgrust、ACM Seshadri、Fantasia、Claude 刪 DB、AI 搶工作影片、VWR |

---

_整理日期：2026 年 5 月 5 日（原版：2026 年 4 月 16 日）_
