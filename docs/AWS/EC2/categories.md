---
sidebar_position: 1
---

# Categories

🧊 1. 【EBS 類型】可持久保存、可掛載、可分離磁碟

| 類型 | 全名                   | 用途           | 性能指標                   | IOPS     | 吞吐量         | 成本   | 適用情境           |
| ---- | ---------------------- | -------------- | -------------------------- | -------- | -------------- | ------ | ------------------ |
| gp2  | General Purpose SSD 2  | 通用型 SSD     | 舊版通用型                 | 最高 16K | 250 MB/s       | 低     | 開發測試           |
| gp3  | General Purpose SSD 3  | 通用型 SSD     | 支援獨立調整 IOPS & 吞吐量 | 最多 16K | 最多 1000 MB/s | 更便宜 | 替代 gp2，建議使用 |
| io1  | Provisioned IOPS SSD 1 | 高性能 SSD     | 可調整 IOPS                | 最多 64K | 1000 MB/s      | 貴     | 需要高 IOPS（DB）  |
| io2  | Provisioned IOPS SSD 2 | 更高耐用度 SSD | 同 io1，但 durability 提升 | 最多 64K | 1000 MB/s      | 更貴   | 關鍵 DB（Oracle）  |

⸻

🧱 2. 【HDD 類型 EBS】容量型便宜但慢（非 SSD）不支援啟動磁碟 (boot volume)

| 類型 | 全名                     | 用途         | 性能特徵         | 吞吐量    | 成本   | 適用情境      |
| ---- | ------------------------ | ------------ | ---------------- | --------- | ------ | ------------- |
| st1  | Throughput Optimized HDD | 吞吐量型硬碟 | 大檔案讀寫最佳化 | ~500 MB/s | 非常低 | 大數據 / 日誌 |
| sc1  | Cold HDD                 | 冷資料硬碟   | 最便宜、最慢     | ~250 MB/s | 最低   | 歸檔、冷備份  |

⸻

⚡ 3. 【Instance Store 類型】臨時磁碟，重開機即清除

這不是 EBS，是 EC2 實體機的本地儲存裝置。

| 類型     | 名稱                        | 特性           | 優點           | 缺點           |
| -------- | --------------------------- | -------------- | -------------- | -------------- |
| NVMe SSD | 本地 SSD（如 i3、c5d、m5d） | 超高速本地存取 | 快！不用過網路 | 關機資料就沒了 |
| HDD      | 本地 HDD（如 d2）           | 大容量但較慢   | 省錢           | 不可持久       |

這些類型由 EC2 instance type 決定（如 c5d 表示有 local NVMe disk）。

⸻

🧠 快速記憶法：

💾 EBS 類型（可持久保存）： - gp2, gp3：通用型 SSD（用 gp3 就對了） - io1, io2：IOPS 可調整的高性能 SSD（資料庫用） - st1, sc1：便宜的 HDD（大檔案用）

🧨 Instance Store 類型（用完即丟）： - i3, c5d, m5d → 有本地 NVMe SSD - 關機即清 → 用來當快取/temp disk 不儲存資料

⸻

✅ 使用建議（按用途）

| 用途                          | 建議磁碟      |
| ----------------------------- | ------------- |
| 通用 EC2、Web App             | gp3           |
| MySQL/Postgres DB             | io1 or io2    |
| Big Data / Log Storage        | st1           |
| 歸檔 / infrequent access      | sc1           |
| 高速暫存空間（如 Spark temp） | i3 的本地 SSD |
| 小實驗測試機                  | gp3（最划算） |

⸻

🧪 額外補充：gp3 vs gp2 為什麼推薦換？

| 類型 | 每 GB 對應 IOPS           | 吞吐量              | 成本              | 彈性                    |
| ---- | ------------------------- | ------------------- | ----------------- | ----------------------- |
| gp2  | 3 IOPS/GB（最多 16,000）  | 自動，最多 250 MB/s | $0.10/GB          | ❌ 不彈性               |
| gp3  | 可自由設定（最高 16,000） | 最多 1,000 MB/s     | $0.08/GB 更便宜！ | ✅ 超彈性：便宜但高效能 |

→ 現在已不建議新用戶再用 gp2，可直接換 gp3！

**1. EBS (Elastic Block Store)：**

- 就像一顆「網路上的硬碟」，是一種 block‐level 的網路附加儲存（Network Attached Storage）。
- 它是持久化的，只要你不刻意刪除或把它設定為隨實例終止而刪除（DeleteOnTermination），即便 EC2 關機或重開，資料都還在。

**2. AMI (Amazon Machine Image)：**

- 是一個「磁碟映像檔」（Disk Image Template），裡面包含了 root volume（也就是 EBS snapshot）上的作業系統、軟體、設定等。
- 你可以把它想像成一個「光碟映像檔 (.iso)」或「硬碟快照」，用來快速複製出多個一模一樣的 EBS root volume。

**3. EC2 Instance：**

- 真正啟動起來的「虛擬伺服器」，包含了
- CPU、RAM（記憶體）
- 根磁碟 (root volume)：通常是 EBS
- 本地暫存磁碟 (instance store)（如果該機型支援的話）
- RAM（記憶體） 跟你講得一樣，關機就沒了；instance store 也是臨時的、關機或終止就會清空。
- EBS 根磁碟則是持久化的（除非你顯式設定要刪掉）。

⸻

為什麼 Root EBS 預設會跟著 Instance 刪除？

- 在 AMI 定義裡，root volume 的 DeleteOnTermination 屬性預設為 true，代表「這顆 EBS 只要實例終止就自動刪除」，避免殘留大量空白 OS 磁碟、節省成本。
- 你後來手動從快照或其他 volume 複製出來並掛載的 EBS，系統預設它們的 DeleteOnTermination = false，所以終止實例時不會自動刪除，保護你的資料。

⸻

總結對照

| 元件             | 類似概念                 | 生命週期                                                 |
| ---------------- | ------------------------ | -------------------------------------------------------- |
| RAM (EC2 記憶體) | 電腦記憶體               | 關機即失                                                 |
| Instance Store   | 電腦內建硬碟（臨時）     | 終止/停止實例即失                                        |
| EBS Volume       | 網路硬碟／網路 USB       | 除非設定刪除或手動刪除，資料永久存在                     |
| AMI              | 磁碟映像檔 (.iso)        | 模板，不直接儲存資料                                     |
| EC2 Instance     | 虛擬機器（CPU+RAM+儲存） | 關機後 CPU/RAM/instance-store 清空，EBS 視設定保留或刪除 |
