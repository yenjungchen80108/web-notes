---
title: CS585 課外連結總覽
description: USC CS585 (Spring 2026) 教授精選課外閱讀，涵蓋 AI Agent、資料庫、AI 安全與產業動態
sidebar_label: CS585 課外連結
tags: [AI, database, agents, industry]
---

# CS585 課外連結總覽

> USC CS585 — Data, Agents & the AI Era (Spring 2026)
> 教授精選課外閱讀，共約 70 則，持續更新中。

:::tip 期末考提醒
教授明確說明：**下方內容有可能出現在期末考題中**，請務必瀏覽。
:::

---

## 🦞 一、OpenClaw / Moltbot / Clawdbot 風波

> 本學期課程中出現次數最多的「即時案例」，代表 AI Agent 從概念走入現實的歷史時刻。

### 起源與改名始末

| 名稱     | 時間       | 原因                                |
| -------- | ---------- | ----------------------------------- |
| Clawdbot | 2025/11    | 奧地利開發者 Peter Steinberger 發布 |
| Moltbot  | 2026/01/27 | Anthropic 商標警告（太像 Claude）   |
| OpenClaw | 2026/01/30 | 名字「唸起來不順」再改              |

**[Clawdbot → Moltbot：CNET 完整報導](https://www.cnet.com/tech/services-and-software/from-clawdbot-to-moltbot-how-this-ai-agent-went-viral-and-changed-identities-in-72-hours/)**
72 小時內爆紅、改名、再改名的完整紀錄。Karpathy、David Sacks 等人紛紛按讚，GitHub ⭐ 在 24 小時內突破 9,000，最終衝上 24 萬。

**[Moltbot 官方文件](https://docs.molt.bot/start/showcase)**
官方使用案例展示，包括寄信、訂機票、管行事曆、寫程式等自主任務。

**[CACM：OpenClaw 到處都是，是場等待發生的災難](https://cacm.acm.org/blogcacm/openclaw-a-k-a-moltbot-is-everywhere-all-at-once-and-a-disaster-waiting-to-happen/)**
ACM 通訊的評論，點名安全漏洞、身分冒用、Prompt Injection 等風險，以及中國政府已限制政府機構使用。

**[課堂影片：Clawdbot 示範片段](https://bytes.usc.edu/cs585/s26_DAgTA/extras/clips/Clawdbot_vgvvK.mp4)**
教授提供的 Clawdbot 原始示範影片。

### Agent 生態延伸

**[NewStack：Claude Dispatch vs OpenClaw](https://thenewstack.io/claude-dispatch-versus-openclaw/)**
Anthropic 的 Claude Code 與開源 OpenClaw 的定位比較——前者專注程式碼開發，後者是通用個人助理。

**[NewStack：Claude Code 原始碼外洩](https://thenewstack.io/claude-code-source-leak/)**
Claude Code 系統提示（System Prompt）遭外洩事件，引發 AI 公司透明度討論。

**[NewStack：TNS Daily 2026/04/01](https://info.thenewstack.io/tns-daily-april-1-2026)**
包含 OpenClaw 與 Hermes 相關動態的每日 AI 快訊。

**[GitHub：OpenClaw API 整合清單](https://github.com/cporter202/openclaw-api-list)**
社群維護的 OpenClaw 第三方 API 整合目錄，記錄 Agent 生態擴張軌跡。

**[NewStack：IDE vs 桌面 Agent](https://thenewstack.io/ide-vs-desktop-agent/)**
傳統 IDE（VS Code + Copilot）與桌面 Agent（OpenClaw 類）的工作模式差異比較，哪個將成為未來工程師主力工具？

---

## 🏭 二、AI 行業重大動態

### Anthropic

**[Anthropic Project Glasswing（官方）](https://www.anthropic.com/glasswing)**
Anthropic 的未公開新模型 **Claude Mythos Preview** 太會找漏洞，因此拒絕公開發布，改以 Project Glasswing 名義：讓 AWS、Apple、Google、Microsoft、NVIDIA、JPMorganChase 等 50+ 組織先悄悄修補。已發現數千個高危零日漏洞，包括一個藏了 17 年的 FreeBSD 遠端執行漏洞。承諾 1 億美元使用額度 + 400 萬美元捐贈開源安全組織。

**[TechBrew：Pentagon 與 Anthropic 截止期限](https://www.techbrew.com/stories/2026/02/25/pentagon-deadline-anthropic)**
美國國防部對 Anthropic 設定的合作或政策合規截止期限，涉及 AI 軍事應用邊界。

**[eWeek：Anthropic 偵測蒸餾攻擊](https://www.eweek.com/security/anthropic-deepseek-moonshot-minimax-targeted-claude/)**
「蒸餾攻擊」：駭客大量呼叫 Claude API 以提取知識訓練自己的廉價模型，DeepSeek、Moonshot、MiniMax 均被點名。

**[Anthropic：偵測與防禦蒸餾攻擊（技術文）](https://www.anthropic.com/news/detecting-and-preventing-distillation-attacks)**
Anthropic 的防禦機制技術說明，對應上方 eWeek 報導。

**[AI Supremacy：BigAI 資料中心與瓶頸](https://www.ai-supremacy.com/p/mythos-bigai-datacenters-and-bottlenecks-anthropic-2026)**
深度分析 AI 資料中心成本結構、能源瓶頸，以及 Anthropic 2026 年擴張計畫。

**[YouTube：Anthropic 相關影片](https://www.youtube.com/watch?v=aFcVKzfkJPk)**
（具體內容需點入查看）

### OpenAI / Sam Altman

**[Sam Altman 個人部落格：無標題文章](https://blog.samaltman.com/2279512)**
凌晨 3:45 有人向 Sam Altman 住家投汽油彈（無人受傷）後，他貼出家人照片、首度坦率回顧 OpenAI 十年歷程、自身決策失誤，以及對 AGI 不能由單一實體掌控的核心信念。

**[Medium：Microsoft-OpenAI 的「2,500 億美元背叛」](https://medium.com/illumination/the-microsoft-openai-divorce-is-official-inside-the-250-billion-betrayal-822073eba64e)**
兩者關係破裂的深度報導，分析 Altman 復職後 OpenAI 走向獨立的內部角力。

**[Gary Marcus：Sam Altman 與 Nvidia 的隕落時刻](https://garymarcus.substack.com/p/sam-altman-and-the-day-nvidias-meteoric)**
AI 著名批評者 Gary Marcus 質疑 Altman 的決策與 Nvidia 市值的合理性。

### Google

**[Google Blog：Gemma 4 發布](https://blog.google/innovation-and-ai/technology/developers-tools/gemma-4/)**
Google 推出開源模型系列 Gemma 4，可在本地運行，是大型封閉模型的開源替代。

**[CodingBeauty：Gemma 4 深度介紹](https://codingbeautydev.com/blog/new-google-gemma-4)**
技術層面的 Gemma 4 能力說明與使用指南。

**[Google Blog：Universal Commerce Protocol (UCP)](https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/)**
Google 提出的通用商務協議，讓 AI Agent 能跨平台執行電商交易的標準化 API 規範。

### 其他公司動態

**[Futurism：Palantir AI 取代人工](https://futurism.com/future-society/palantir-ai-labor-hands)**
Palantir 的 AI 系統如何被定位為「取代」而非「輔助」工人，CEO Alex Karp 的激進言論。

**[Futurism：Jack Dorsey / Block 的 AI 困境](https://futurism.com/artificial-intelligence/jack-dorsey-block-falling-apart-ai)**
Block（前 Square）因 Jack Dorsey 強推 AI 策略而內部分裂，大量員工離職。

**[Kimi K2.5 展示（YouTube）](https://www.youtube.com/watch?v=eQyAzZboDbw)**
中國 Moonshot AI 的 Kimi K2.5 能力展示。教授備注：「看前 12 分鐘，開 2 倍速 :)」

---

## 💸 三、AI 資金與產業規模

**[Yahoo Finance：大型科技公司 6,500 億美元 AI 支出](https://finance.yahoo.com/news/big-tech-spend-650-billion-012716850.html)**
2026 年 Microsoft、Google、Meta、Amazon 等在 AI 基礎設施的總資本支出預估，數字令人震撼。

**[Apricitas：美國的 1 兆美元 AI 豪賭](https://www.apricitas.io/p/americas-1t-ai-gamble)**
AI 基礎設施投資是否過度——泡沫還是轉型的必要代價？

**[Fortune：「有什麼大事正在發生」](https://fortune.com/2026/02/11/something-big-is-happening-ai-february-2020-moment-matt-shumer/)**
將 2026 年 2 月的 AI 時刻比作 2020 年 2 月（新冠疫情爆發前）：我們站在歷史轉折點，大多數人尚未意識到。

**[Archive：AI 崩潰？（存檔文章一）](https://archive.ph/FB947)**
討論 AI 行業是否進入泡沫化或技術停滯的文章存檔。

**[Archive：AI 崩潰？（存檔文章二）](https://archive.ph/zqxuF)**
同主題的補充觀點存檔。

---

## ⚠️ 四、AI 的社會衝擊與倫理

**[The Guardian：印度女工被迫觀看暴力內容訓練 AI](https://www.theguardian.com/global-development/2026/feb/05/in-the-end-you-feel-blank-indias-female-workers-watching-hours-of-abusive-content-to-train-ai)**
揭露印度低薪女性工人被迫反覆觀看仇恨、暴力內容以標記訓練資料的真實處境。

**[TIME：肯亞 OpenAI 訓練標記工人](https://time.com/6247678/openai-chatgpt-kenya-workers/)**
OpenAI 外包 RLHF 標記任務至肯亞，工人低薪且須接觸心理創傷性內容。兩篇文章合看，揭示 AI 訓練的隱藏人力成本。

**[Gary Marcus：人類的緊急警報](https://garymarcus.substack.com/p/code-red-for-humanity)**
AI 發展速度對人類社會構成緊迫威脅，監管嚴重落後於技術，疾呼全球回應。

**[Gary Marcus：大規模 AI 停機事件](https://garymarcus.substack.com/p/a-spate-of-outages-including-incidents)**
記錄 AI 系統在生產環境中的一系列重大故障，說明可靠性問題遠未解決。

**[404 Media：RFK Jr 的 AI 營養機器人鬧劇](https://www.404media.co/rfk-jrs-nutrition-chatbot-recommends-best-foods-to-insert-into-your-rectum/)**
RFK Jr（美衛生部長）的 AI 營養建議機器人，因 AI 幻覺建議用戶「將食物插入直腸」。標準的高風險情境 AI 失誤案例。

**[Prof Bachman：地基正在位移](https://profbachman.substack.com/p/the-ground-is-shifting-under-our)**
AI 對高等教育（尤其商學院）根本模式的衝擊，以及院校如何在 AI 時代重新定位自身價值。

**[Zachwills.net：以思維速度構建](https://zachwills.net/building-at-the-speed-of-thought/)**
開發者分享使用 AI 工具後，開發節奏從「打字速度」升級為「思維速度」的親身體驗。

**[YouTube：某深度思考影片](https://www.youtube.com/watch?v=9v2eZKsEJMI)**
（教授備注「看前 12 分鐘，開 2 倍速」）

**[Archive：AI 時代的「試煉」](https://archive.ph/yDAYN)**
存檔文章，標籤 'gauntlet'（試煉），具體內容需點入查看。

**[Archive：存檔文章](https://archive.ph/h46DF)**
教授標注「Ooh」，具體內容需點入查看。

---

## 🛡️ 五、AI 安全與技術攻防

**[Futurism：Project Nepenthes++ — 毒泉計畫](https://futurism.com/artificial-intelligence/poison-fountain-ai)**
Nepenthes 是開源「焦油坑」工具：讓 AI 爬蟲陷入無限假網頁迴圈，並注入 Markov 亂碼污染訓練資料。「++」版本功能更強。開發者稱這是對無視 robots.txt 的 AI 公司的「反抗」。

**[LessWrong：AI 模型的奇特吸引子狀態](https://www.lesswrong.com/posts/mgjtEHeLgkhZZ3cEx/models-have-some-pretty-funny-attractor-states)**
實驗記錄 AI 模型在特定情境下反覆收斂到固定輸出模式（「吸引子」），有時滑稽，有時令人擔憂。

**[Neuroscience News：Humanity's Last Exam 基準測試](https://neurosciencenews.com/humanity-last-exam-ai-benchmark-30191/)**
設計來「難倒 AI」的超難題庫，由各學科頂尖專家出題，初版 AI 正確率接近 0%。作為 AI 能力前沿的指標。

**[The Deep View：AI 模型輕量化競賽](https://archive.thedeepview.com/p/ai-giants-race-to-build-lighter-models)**
各大 AI 公司競相壓縮模型大小——更小的模型、更低的推論成本，是 AI 普及的關鍵戰場。

---

## 🗄️ 六、資料庫與分散式系統

:::note 課程核心
以下為 CS585 的核心學術主題，與課堂內容直接呼應。
:::

**[Codd 1970 年關聯式資料庫原始論文（PDF）](https://bytes.usc.edu/cs585/s26_DAgTA/extras/docs/Codd_1970.pdf)**
Edgar Codd 提出關聯式模型的劃時代論文。教授附言：「一切的起點」，並附上 Larry Ellison（Oracle 創辦人，USC 校友）照片——他靠這篇論文建立了 Oracle 帝國。

**[Martin Kleppmann：CAP 定理的陷阱與業界迷思](https://martin.kleppmann.com/2019/06/27/hydra-interview.html#pitfalls-of-cap-theorem-and-other-industry-mistakes)**
《Designing Data-Intensive Applications》作者談 CAP 定理被普遍誤解與濫用，以及常見分散式系統迷思。

**[DBMS Musings：NewSQL 的失敗](https://dbmsmusings.blogspot.com/2018/09/newsql-database-systems-are-failing-to.html)**
NewSQL 承諾兼顧 ACID 與水平擴展，但作者論證大多數 NewSQL 系統只是換包裝，未解決根本問題。

**[BuildTechCareer：CAP 定理與 Gen Z 工程師](https://buildtechcareer.substack.com/p/cap-theorem-genzs-dont-do-onsite)**
現代年輕工程師對 CAP 定理的認知現狀，以及面試中這個話題的演變。

**[3DS：Virtual Twin 數位分身](https://www.3ds.com/virtual-twin)**
達梭系統介紹虛擬分身技術——用精確數位模型模擬現實系統（工廠、飛機、人體），現在與 AI 深度結合。

**[Epsio：Streaming SQL 串流查詢（文件）](https://docs.epsio.io/)**
對不斷更新的資料流用標準 SQL 語法做即時查詢，不需要學 Kafka Streams 或 Flink。

**[GitHub：FalkorDB QueryWeaver](https://github.com/FalkorDB/QueryWeaver)**
圖形資料庫 FalkorDB 的 AI 輔助查詢生成工具——用自然語言寫 Cypher 查詢。

**[Microsoft Research：Project Silica — 玻璃儲存](https://www.microsoft.com/en-us/research/blog/project-silicas-advances-in-glass-storage-technology/)**
用雷射在石英玻璃中燒刻資料，可保存 1 萬年以上。Microsoft 已用它儲存 Warner Bros. 電影庫備份。

**[Nature：AI 科學突破論文](https://www.nature.com/articles/s41586-025-10042-w)**
（具體內容需點入查看）

**[USC：互遞歸（Mutual Recursion）即時示範](https://bytes.usc.edu/~saty/tools/xem/run.html?x=mutrec)**
教授自製的線上 Scheme 執行環境，搭配課堂說明互遞歸概念。

---

## 🛠️ 七、開源工具與技術專案

**[GitHub：PythonRobotics](https://github.com/AtsushiSakai/PythonRobotics)**
最受歡迎的機器人算法 Python 實作庫，涵蓋路徑規劃、SLAM、機械臂控制，配動畫說明。

**[GitHub：llmfit](https://github.com/AlexsJones/llmfit)**
評估與比較不同 LLM 對特定任務適用性的輕量工具，幫助選擇最合適的模型。

**[GitHub：VectifyAI / OpenKB](https://github.com/VectifyAI/OpenKB?tab=readme-ov-file)**
用 LLM 把原始文件（PDF、Word、Markdown 等）編譯成結構化 wiki 知識庫。靈感來自 Karpathy：傳統 RAG 每次從頭找，OpenKB 讓知識隨時間累積。

**[GitHub：Coolify](https://github.com/coollabsio/coolify?tab=readme-ov-file)**
開源自架 PaaS 平台，相當於免費版 Vercel/Heroku/Netlify。只需 SSH 連線，即可在自己的伺服器上一鍵部署網站、資料庫、280+ 服務。無供應商鎖定。

**[GitHub：Rowboat（YC Backed）](https://github.com/rowboatlabs/rowboat)**
開源多 Agent 建構框架。可視覺化設計多 Agent 工作流程，用自然語言描述需求，AI Copilot 自動搭建系統。支援 MCP 伺服器整合，適合客服、企業自動化等場景。

**[Hilbert（infplane.com）](https://hilbert.infplane.com/pages/hilbert)**
圍繞 Hilbert 計畫的 AI 工具或思想實驗。

**[Tiiny AI](https://tiiny.ai/)**
輕量 AI 工具平台（與 Hilbert 並列提到）。

**[Accio.com / Work](https://www.accio.com/work)**
AI 工作助理平台，定位為辦公室任務的 AI 代理人。

**[YouTube：某影片片段（from 1:26:01）](https://youtu.be/AOZJ5axPPUk?t=5161)**
（需點入查看具體內容）

**[USC Lunair 示範頁](https://bytes.usc.edu/~saty/courses/snipps/lunair-feb26/)**
USC 課程相關 AI 工具示範。

---

## 🧠 八、AI 工程思想與未來程式語言

**[為 AI 設計的程式語言——多模型對答實驗](https://earlywarning.news/files/2025-11-28-18-49-02-chatgpt-programming_language_for_ai.html)**
讓 GPT、Gemini、Claude、Grok、DeepSeek、Qwen、Kimi 等多個 AI 同答：「若主要使用者是 AI 而非人類，程式語言應長什麼樣？」各模型一致認為：應保留形式驗證、強型別、明確語義；應拿掉語法糖、可讀命名、自然語言風格關鍵字。

**[Hacker News：AI 程式語言討論串](https://news.ycombinator.com/item?id=46571166)**
工程師社群對上文「AI-first 程式語言」的批評、補充與辯論。

**[Karpathy：Wiki RAG 概念（Twitter/X）](https://x.com/karpathy/status/2039805659525644595?s=20)**
Andrej Karpathy（前 Tesla AI 總監）分享以 Wikipedia 為知識庫的 RAG 系統想法，啟發了 OpenKB 等多個開源工具。

**[Wiki RAG 示範影片（YouTube）](https://www.youtube.com/watch?v=sboNwYmH3AY)**
Karpathy 概念的實際示範。

**[Archive：某技術文章](https://archive.ph/LJY5T)**
（具體內容需點入查看）

**[Archive：某技術文章](https://archive.ph/1GJ5x)**
（具體內容需點入查看）

**[YouTube：笑一個（Lol）](https://www.youtube.com/watch?v=0vvVo0Um1HY)**
教授標注「Lol」的影片（具體內容需點入查看）。

---

## 💼 九、職涯與個人

**[YouTube：職涯建議](https://www.youtube.com/watch?v=smA932BkHYw)**
教授推薦的職涯建議影片。

**[Epstein 人脈網路視覺化](https://epsteinexposed.com/network)**
Jeffrey Epstein 的人際關係圖形視覺化。在課程語境下可能作為「圖資料庫」或「社會網路分析」的技術示例。

**[Sam Altman 部落格](https://blog.samaltman.com/2279512)**
（見第二節 OpenAI / Sam Altman）

**[Audio Plugin Deals 2025 備忘表](https://audioplugin.deals/apd-shop-cheat-sheet-2025)**
音頻插件購買指南——教授個人分享，非課程相關。

---

## 🖼️ 十、圖片與補充資料

| 項目                                                                                                    | 說明                                  |
| ------------------------------------------------------------------------------------------------------- | ------------------------------------- |
| [Codd 1970 論文（PDF）](https://bytes.usc.edu/cs585/s26_DAgTA/extras/docs/Codd_1970.pdf)                | 關聯式資料庫原始論文                  |
| [Larry Ellison @ USC 2021](https://bytes.usc.edu/cs585/s26_DAgTA/extras/pics/Ellison_USC_2021.png)      | Oracle 創辦人 USC 演講圖              |
| [Big Tech $650B 支出圖](https://bytes.usc.edu/cs585/s26_DAgTA/extras/pics/hyperscalers-650B-hgtr.png)   | 科技公司 AI 資本支出圖表              |
| [GaryM：AI 對工作的影響](https://bytes.usc.edu/cs585/s26_DAgTA/extras/pics/GaryM_AIJobs_Apr26_gh54.png) | Gary Marcus 製作的 AI 就業趨勢圖      |
| [Nvidia 沒有芯片](https://bytes.usc.edu/cs585/s26_DAgTA/extras/pics/NV_nochips.png)                     | Nvidia 芯片短缺示意圖                 |
| [Hermes vs OpenClaw](https://bytes.usc.edu/cs585/s26_DAgTA/extras/pics/Hermes-vs-Claw.png)              | Anthropic Hermes 與 OpenClaw 競爭對比 |
| [Anthropic 的回應（圖）](https://bytes.usc.edu/cs585/s26_DAgTA/extras/pics/Anth_yesbut_bg3.jpg)         | Anthropic 對蒸餾攻擊事件的回應圖      |

---

_最後更新：2026 年 4 月 16 日_
