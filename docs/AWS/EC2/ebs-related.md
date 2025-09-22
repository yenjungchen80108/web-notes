---
sidebar_position: 2
---

# EBS Related

## EC2 + EBS

在 AWS 的世界裡，EC2 ＋ EBS 是「計算（Compute）」和「儲存（Storage）」兩個不同層面的角色，你必須把它們組合起來，才能得到一台既能跑程式、又能持久存資料的虛擬伺服器。以下幾點說明為什麼 EBS 要「附加（attach）」到 EC2 Instance：

⸻

**1. EBS 本質上只是「裸硬碟」**

- EBS Volume 是網路區塊儲存（network‐attached block storage），就像一顆可持久化的硬碟，
- 自身並不包含任何「執行環境」── 它不會自行運算，也不會自行回應 I/O 請求，
- 必須要有一個「主機」來當 Host，把它 mount 為 block device，才能讀寫。

▶️ 類比：EBS 就像放在資料中心裡的 SAN 磁碟陣列；EC2 Instance 就像連上去的伺服器主機。

⸻

**2. EC2 才是「跑程式」的地方**

- EC2 Instance 提供 CPU、記憶體、網卡、OS… 等「運算環境」，
- 你所有的服務程式、資料庫軟體都必須跑在 EC2 上，才能呼叫系統 API、載入驅動程式與 EBS 溝通。

⸻

**3. 為何不能「同 AZ 就能共用」？**

- EBS 放在同一個 AZ 裡，只是保證了網路延遲低、可靠性高，卻不會自己主動給任何主機用。
- 必須明確「把 EBS attach 到某一台 EC2 上」，OS 才會在 /dev/xvdf（Linux）或 D:（Windows）底下看到它，並能格式化、掛載、讀寫。

⸻

**4. 叢集模式（Multi-Attach）＆ Cluster-Aware File System**

- io1/io2 支援「Multi-Attach」：同一顆 volume 最多可以 attach 給 16 台 EC2。
- 但多台一起讀寫同一份 block，如果沒有智慧型檔案系統（cluster-aware FS），會有資料競爭、破壞 Metadata 的風險。
- 你得在每台 EC2 上安裝像 OCFS2、GFS2 之類「叢集檔案系統」，它們會協調鎖（lock）、同步元資料，確保多機 安全共用。

⸻

**5. 性能差異：EBS vs. EC2 Local**

| 特性            | EC2 Local NVMe / Instance Store | EBS Volume                       |
| --------------- | ------------------------------- | -------------------------------- |
| 延遲（Latency） | < 1 ms（直接 PCIe 附加）        | ~1–2 ms（透過網路掛載）          |
| IOPS/吞吐量     | 極高（依機型而定）              | 視選型號（gp3/io2…）可調整       |
| 持久性          | 臨時（重啟或停止即丟）          | 持久（除非 DeleteOnTermination） |
| 共用性          | 只能在單台本機                  | 可 Multi-Attach 給多台           |

- 一般情況下，把 EBS 當「持久化硬碟」掛到 EC2，用於存放資料庫資料、log、big data …
- 把 Local NVMe 當「暫存磁碟」用於快取、高速 scratch space。

⸻

**總結**

1. EBS：持久化、可快照的「網路硬碟」，不會自己跑程式，必須 attach 到 EC2 才能使用。
2. EC2：提供運算與 I/O 驅動的執行環境，附加 EBS 後才知道要把資料寫哪顆磁碟。
3. Multi-Attach + Cluster-Aware FS：當你要做多台同時掛載同一顆 EBS（io1/io2），要配合叢集檔案系統才能安全讀寫。

這樣的設計，把「算力」和「儲存」解耦，讓你可以各自彈性調整規格和數量：高運算就 scale EC2，多資料就 scale EBS。

## 為什麼 EBS 可以這麼可靠？

AWS 在設計 EBS 時，就把它當成「網路硬碟」——只是把儲存層從伺服器本地抽出，放到一個專門的分散式儲存系統裡面。為了保證資料不會因為底層硬體故障或網路中斷而遺失，他們採用了幾項關鍵機制：

⸻

**1. 同 AZ 內的冗餘複製**

「當你建立一個 EBS 卷，AWS 會自動在同一 Availability Zone 的多台儲存伺服器之間同步複製該磁碟的所有區塊。」

- 每一個寫入操作（write）只有在成功寫入到多份副本之後，才會向使用者回應成功。
- 這樣即便某台儲存伺服器突然故障，或者其中一份複本所在的底層硬碟壞掉，也不會造成任何資料缺失。
- 這個過程對使用者完全透明，故障副本會在背景自動被替換，使用者根本察覺不到。

⸻

**2. 嚴格的確認與日誌（Journaling）**

- AWS 的儲存系統會在回寫前先把變更記錄到一個持久化的「寫前日誌（write-ahead log）」裡，確保即使遇到系統 crash，也能從日誌回放完成未完成的寫入。
- 這種設計跟桌機的檔案系統（ext4、NTFS）很類似，只不過後端是分散式的叢集。

⸻

**3. 基於 S3 的快照（Snapshots）**

- EBS 快照會把資料刷到 Amazon S3，S3 本身又有跨多個 AZ、數百萬顆硬碟冗餘儲存的機制。
- 快照能讓你在整個 AZ 當機或更大範圍故障時，隨時還原到某個時間點的版本。

⸻

**4. 處理網路中斷或延遲**

- 網路層面：AWS 自家高速骨幹網路＋光纖通道，流量路徑上至少有兩條以上備援路由，避免中間路段單點故障。
- 傳輸協定：儲存層的協定有錯誤檢查（checksum）、重傳機制、TLS/加密，保證資料完整不會在網路上被丟失或破壞。
- 重試邏輯：EC2 driver 內建對 EBS I/O 的重試（retry）與背壓（back-off），遇到短暫網路波動也能自動再試。

⸻

**小結**

1. 多副本同 AZ 同步寫入 → 防硬體故障
2. 寫前日誌 + Replica 自動修復 → 防系統 Crash
3. 快照存到 S3 → 防 AZ/Region 故障
4. 企業級網路＋重試機制 → 防網路抖動

透過這些分散式儲存與傳輸層的嚴謹設計，AWS 才能把「EBS 就像本地硬碟」這個體驗，以 99.999% 的可靠度呈現給使用者，而不用擔心雲端環境下資料寫不進去或遺失。

## EBS 跟資料庫的關係

WAL（Write-Ahead Log）跟 CDC（Change Data Capture）在傳統資料庫裡都是為了保證資料一致性與捕捉變更而設計的，AWS 在它的存儲與資料庫服務底層，其實也有類似的概念，只是落在不同的層級：

⸻

**1. EBS / 底層 block storage 的「日誌式寫入＋多副本複製」**

- Block-level 日誌 (Journaling)
  EBS Volume 接收寫入時，背後會先把這些 block 變更做類似寫前日誌 (WAL) 的機制，確保即便遇到伺服器 crash 或網路抖動，也能從日誌回放完成那些還沒真正刷到多副本的寫入。

- 同步複製多副本
  EBS 在同一 AZ 內會把每個 block 寫入到至少 2–3 個底層儲存伺服器，並等到多份副本都完成後才回應成功。這跟資料庫「先寫日誌再落盤」+「把日誌／資料複製到備援伺服器」是一樣的耐久性思路，只不過對使用者呈現的介面是「一顆網路硬碟」。

⸻

**2. RDS／Aurora 裡的 WAL 與分散式儲存**

- RDS for PostgreSQL / MySQL
  - PostgreSQL 就用自己原生的 WAL 來做主從複寫，Multi-AZ 下主實例的 WAL 會同步到另一區的 standby，再由 standby replay 保持完全一致。
  - MySQL 則是 binlog 透過 semi-sync 或 async 複製到備援。
- Amazon Aurora
  - Aurora 更進一步把儲存層完全拆成「分散式儲存服務」，每次寫入，WAL 變更會直接在底層多個節點間寫入並多副本保存，讀/寫都透過這個共用的分散式 log 進行，把資料庫的日誌當作存儲協定的一部分，大幅降低延遲並提高可用性。

⸻

**3. CDC／Change Data Capture in AWS**

- Database Migration Service (DMS)
  AWS DMS 可以對 RDS、DynamoDB、MongoDB 等來源做 CDC，讀取 WAL、binlog、oplog 來捕捉資料變更，推送到 Redshift、Elasticsearch、Kinesis、S3 等。
- DynamoDB Streams
  DynamoDB 內建的 Change Stream 就是把每一次寫入、更新、刪除都送到 Kinesis-style 的日誌串流，供下游消費。

⸻

**小結**

1. EBS／底層儲存：用「block-level 日誌＋多副本同步複製」來保證 Durability，概念就像 WAL，只是寫在儲存系統底層。
2. RDS/Aurora：直接把 DB 的 WAL／binlog 當作儲存及複製協定，用 Multi-AZ、分散式日誌完成高可用。
3. CDC 服務：DMS、DynamoDB Streams 等把資料庫日誌暴露出去，做異動複製到其他系統。

因此，雲端存儲跟傳統資料庫在耐久性和複製的設計理念上是同源的——只不過 AWS 把它們拆成「儲存層」「資料庫層」「CDC 工具」三個可彈性組合的服務，而不是把所有東西塞在一顆 monolithic 的 DB 裡。
