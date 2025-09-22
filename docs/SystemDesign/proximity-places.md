---
sidebar_position: 2
---

# Proximity Places

## 0. Overview

A service that can find places near a given location.

## 1. Requirements

## 2. High-level design

## 3. Detailed design

問題整理

    1.	每個分片（shard）有備份（replica），主節點故障時讓備用節點接手 ⇒ 這複製過程如何保證正確？
    2.	如果多主（multi-leader）出現，怎麼避免資料寫入衝突？
    3.	Kafka 類似使用 commit id 的方式來保證一致性，這種機制是否也可應用在分片複製？

⸻

🔸 1. 分片備援的複製一致性問題（主從複製）

在典型的 主從（leader–follower）架構中，會出現：

| 問題場景                | 描述                                                                                         |
| ----------------------- | -------------------------------------------------------------------------------------------- |
| ❗ follower 延遲同步    | follower 複製資料比 leader 慢，若此時 leader 壞掉，follower 接手時資料可能不完整（資料丟失） |
| ❗ 複數 follower 不一致 | 多個 follower 同步程度不同，要選哪一個接替？資料差異怎麼處理？                               |

解法：依賴「複製日誌 + commit 確認機制」

- 資料寫入 leader 後，不會馬上標記為「成功」
- 直到 某個 quorum 的 follower 也成功寫入後，才回應 client「OK」

這種機制稱為：

✅ write-ahead log（WAL）+ quorum-based acknowledgement

🔸 2. Kafka 如何解決複製與 failover 問題？

Kafka 的高可用與一致性策略其實非常經典，DDIA 也詳細描述過（第 11–12 章）。

| Kafka 機制            | 說明                                                  |
| --------------------- | ----------------------------------------------------- |
| Partition             | 資料流以 partition 為單位切分（類似 shard）           |
| Leader + Replica      | 每個 partition 有一個 leader，其餘為 follower（備份） |
| ISR (In-Sync Replica) | Kafka 記錄有哪些 follower 是同步跟上的（in-sync）     |
| ACK 策略              | acks=all 表示 leader 要等 ISR 全部寫入後，才算成功    |
| Offset + commit log   | 每條訊息有遞增的 offset，確保 follower 可以按順序複製 |
| Zookeeper / Raft      | 負責 leader election，當主節點故障時自動選出新主      |

➡️ 所以即使 leader 掛了，Kafka 可以從 ISR 裡選出最近跟上的 replica 當新 leader，確保：

- ❌ 不會讓舊的資料回到系統（避免 leader regression）
- ✅ 資料 offset 單調遞增 → 不會混淆訊息順序
- ✅ Producer 不會重複送出已被 commit 的訊息

🔸 3. 那一般的資料庫 sharding + replication 呢？

這就要看你用的資料庫是什麼類型（MySQL、PostgreSQL、MongoDB、Cassandra…），不同系統有不同策略。

以下是幾個常見複製一致性等級：

| 資料庫系統            | 機制／特性                                                                |
| --------------------- | ------------------------------------------------------------------------- |
| MySQL (主從)          | 傳統 async 複製，有資料丟失風險。可搭配 semi-sync                         |
| MongoDB Replica Set   | Primary + secondary，同步複製 + election                                  |
| Cassandra             | Multi-leader（eventual consistency），用「consistency level」定義寫入策略 |
| CockroachDB / Spanner | 分布式一致性，透過 Raft 或 TrueTime 保證 linearizability                  |

大部分支援「同步複製 + quorum commit」的系統，也能達到像 Kafka 那樣的保證：

- ❌ 不會讓資料倒退（即便 leader 掛了）
- ✅ 多副本之間順序一致
- ✅ 可透過 log offset、timestamp、version vector 等方式追蹤寫入狀態

🔹 你的核心問題回答

❓「主節點發生故障時，備用節點是否能根據某種 commit id 機制來確保資料一致性？」

✅ 是的，Kafka 是這樣做的；其他系統要達到這目標也需類似設計：

| 技術元件                    | 角色                             |
| --------------------------- | -------------------------------- |
| Log offset / version vector | 確保副本間資料順序一致           |
| Write-ahead log（WAL）      | 寫入先進入日誌，日誌同步後才提交 |
| Quorum 機制                 | 等待一定數量副本完成寫入才算成功 |
| 自動 leader election 機制   | 當主節點故障時快速選出備援       |

Kafka 是比較標準的 stream 系統，但即使是傳統 OLTP 資料庫，如果支援同步複製 + 多副本選舉（如 MongoDB、Spanner、CockroachDB）也具備類似特性。

⸻

✅ 小結

| 問題                                          | 回答                                                              |
| --------------------------------------------- | ----------------------------------------------------------------- |
| 主從複製怎麼避免資料丟失？                    | 使用同步複製 + quorum 寫入確認                                    |
| Kafka 怎麼做到 leader 壞掉後不丟資料？        | 使用 ISR + offset 記錄每筆訊息 + Zookeeper 做 leader 選舉         |
| 傳統資料庫可以做到類似 Kafka 的複製一致性嗎？ | 若具備 WAL + 同步複製 + leader election，理論上可做到             |
| 如果用 shard + replica，要注意什麼？          | 要記錄每 shard 的複本狀態，並設計好 failover 機制（避免資料倒退） |

## 一致性模型（Consistency Models） 與 分散式系統在多資料源中一致性對齊的難題。

✅ 問題總結： 1. 不同資料庫有不同一致性層級（Read Uncommitted → Serializable） 2. Kafka 是一種 log-based 系統，寫入副本時是 streaming、非同步，但又強調高一致性 3. 那麼 Kafka 寫入副本後，如果背後每個資料庫（MongoDB、Cassandra、MySQL）都有不同一致性模型，那怎麼確保一致性仍被維持？

⸻

🧠 一致性模型快速總覽

| 層級                      | 說明                                 | 特點                         |
| ------------------------- | ------------------------------------ | ---------------------------- |
| Read Uncommitted          | 可以讀取尚未提交的資料               | 快速，但可能「髒讀」         |
| Read Committed            | 只能讀已提交資料                     | 可避免髒讀                   |
| Repeatable Read           | 一次查詢內，多次讀相同資料，結果相同 | 可避免不可重複讀             |
| Serializable              | 強一致，資料彷彿序列執行             | 可避免所有 anomaly，效能最低 |
| Linearizability（線性化） | 每次寫入都立即對全系統可見           | 分散式中最強但難實作         |

AWS 強調 strong consistency，它通常是指近似 Linearizability（比如 S3、DynamoDB 在某些操作是線性化的）。

⸻

🔁 Kafka 的一致性模型是什麼？

Kafka 本身 不是資料庫，但它作為中介訊息流處理系統，其 一致性強度由以下因素決定：

| 元件              | 一致性保證                                 |
| ----------------- | ------------------------------------------ |
| Producer → Broker | acks=all：寫入成功需同步副本都完成寫入     |
| Broker → Replica  | 使用 log offset 確保順序，ISR 保證同步程度 |
| Consumer 讀資料   | 資料只會從 Leader 分區讀，且 offset 遞增   |

所以：

- Kafka 本身的 log 是強一致的（只要設定得當）
- 但若你要「把 Kafka 的 log 寫入資料庫」，則一致性取決於資料庫本身！

⸻

🔧 核心問題：Kafka 的一致性會不會被資料庫破壞？

答案是：

✅ Kafka 可以保證寫入順序與 delivery once 的語意，但它無法控制資料寫入資料庫後的隔離等級或一致性強度，這完全取決於你用的資料庫，以及你是怎麼設計 Sink 處理程序的。

⸻

📌 Kafka → 資料庫，要怎麼維持一致性？

使用 Kafka Connect + Sink connector + 兩階段提交（Exactly-once support）

例如你要把 Kafka 的資料 sink 到：

- PostgreSQL（支援 Serializable）
- Cassandra（最終一致性）
- MongoDB（Replica Set，預設是 Read Committed）

這時：

1. Kafka Connect 支援 Exactly-once delivery semantics
2. 使用 transactional id
3. 讓 sink 端處理具有原子性（與 Kafka topic 一起提交）
4. e.g., Kafka Connect JDBC Sink → PostgreSQL（設定為串流式 transaction）

5. 如果你的 DB 本身只支援 Read Committed（例如 MongoDB），那你仍然可以保證：

⸻

📘 實務比喻：

假設 Kafka 是你公司內部的物流系統：

- Kafka 保證每一件包裹會依順序、正確送達
- 但每個「分店（資料庫）」怎麼收貨、入庫，就看該分店的處理流程了：
- A 分店（PostgreSQL）會逐件核對、清點（Serializable）
- B 分店（MongoDB）看到包裹就塞倉庫（Read Committed）
- C 分店（Cassandra）收貨後先寫備忘錄，晚點再統整（eventual consistency）

⸻

🧩 Kafka 實務技巧（要確保一致性）：

| 技術                  | 說明                                                                               |
| --------------------- | ---------------------------------------------------------------------------------- |
| Transaction API       | Kafka 支援 producer + consumer + sink 一起進行 transactional commit                |
| Sink connector config | 可以設定 exactly-once 模式（需 Kafka >= 2.5）                                      |
| 資料庫選擇與設計      | 若你真的需要強一致性，就選用支援 ACID 的資料庫（如 PostgreSQL）並設為 Serializable |

⸻

✅ 最後重點整理

| 問題                                | 解法                                                                 |
| ----------------------------------- | -------------------------------------------------------------------- |
| Kafka 可保證副本一致嗎？            | 可以，只要使用 ISR + acks=all                                        |
| Kafka Sink 到資料庫會破壞一致性嗎？ | 看資料庫本身支援的隔離等級與 Sink 實作方式                           |
| 若想確保整體一致性怎麼做？          | 使用 Kafka transaction + Sink exactly-once delivery + 選擇正確資料庫 |
| Kafka 可配合哪些一致性模型？        | 本身近似 linearizable，但 downstream 資料庫的語意需自行管理          |

## Sync vs. Sink

| 名稱 | 拼字         | 中文意思             | 在 Kafka 中的意思                                                                      |
| ---- | ------------ | -------------------- | -------------------------------------------------------------------------------------- |
| Sync | Sync（同步） | 同步、等待回應再繼續 | Kafka Producer/Consumer 的同步傳輸方式（如 acks=all）                                  |
| Sink | Sink（水槽） | 資料匯出端（目標端） | Kafka Connect 裡面的 資料接收端插件，把資料「寫出」到 DB、S3、Elasticsearch 等外部系統 |

🔄 什麼是 Kafka 的 Sink？

在 Kafka Connect 架構中，一個資料流程大致是：

資料來源 (Source) → Kafka Topic → Sink Connector → 外部資料庫/存儲

🔧 Kafka Sink 是誰？

- Kafka Sink 是 Kafka Connect 架構中的一個「插件」
- 功能是將 Kafka 的資料「寫出」到外部儲存系統
- 常見 Sink：
  - Kafka → PostgreSQL（JDBC Sink）
  - Kafka → Elasticsearch（ES Sink）
  - Kafka → S3（S3 Sink）
  - Kafka → MongoDB（MongoDB Sink）

🔄 Kafka 中的 Sync（同步）

這就比較像是通訊協定層級的行為，例如：

- acks=all：Producer 發送訊息，等到所有同步副本都寫入成功才回應成功（是一種同步確認）
- flush()：Producer 的同步寫入方法
- Consumer commit offset：可以是同步（blocking）或非同步（background）

這邊的「Sync」表示：需要等待對方回應才繼續下一步，是通訊上的一致性機制。

⸻

✅ 結論對照表

| 用語           | Kafka 中意義                              | 關鍵差異               |
| -------------- | ----------------------------------------- | ---------------------- |
| Sync（同步）   | 等待副本或 consumer 處理完才確認          | 是一種「通訊模式」     |
| Sink（匯出端） | 把 Kafka Topic 的資料寫到外部資料庫的插件 | 是一種「資料流程角色」 |

如你有在研究 Kafka Connect，可以深入玩一下：

- sink.task.flush.timeout.ms
- delivery.guarantee=exactly_once（Kafka 2.5+ 的 JDBC Sink 支援）
