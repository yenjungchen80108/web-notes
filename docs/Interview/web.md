1. 簡述 CORS 流程：何時會 preflight？三個核心回應標頭分別控制什麼？

2. XSS vs CSRF 差異與防護（CSP、HttpOnly/SameSite、CSRF token）。

3. Cookie 屬性：Secure, HttpOnly, SameSite（Lax/Strict/None）與第三方 Cookie 的變化。

4. HTTP/1.1 與 HTTP/2/3 的差異：HOL blocking、Multiplexing、連線數的策略。

5. WebSocket 握手如何透過 HTTP 升級？在 LB 後面需要注意什麼（黏性、超時）？

6. REST 的資源設計與版本策略；PUT/PATCH 的語義差異；什麼是 Idempotency Key？

7. OAuth2/OIDC：PKCE 為什麼對 SPA 必要？Authorization Code + PKCE 流程。

8. 什麼是 CSP？舉三個常用 directive 與一個 report-only 的使用情境。

**1. CORS 流程：何時 preflight？三個核心回應標頭是？**

一句話：瀏覽器遇到跨網域請求時，先看是否屬於 Simple Request；不是就先發 Preflight(OPTIONS) 來問伺服器「可不可以」，拿到允許後才送真正請求。

- 什麼算 Simple Request（免 preflight）

  - 方法：GET / HEAD / POST
  - Content-Type：application/x-www-form-urlencoded、multipart/form-data、text/plain
  - 不帶自訂標頭、且不帶 credentials（或同源 cookie 情境）

- 常見會 preflight
  - 方法是 PUT / PATCH / DELETE
  - 帶自訂標頭（如 Authorization）
  - Content-Type 不是三種簡單型（如 application/json）
  - 帶跨站 credentials（cookie）且條件不符

三個「核心回應標頭」

- `Access-Control-Allow-Origin`：允許的來源（可回 https://foo.com 或 _；有 credentials 時不能用 _）
- `Access-Control-Allow-Credentials`：是否允許帶 cookie/認證（true/不回）
- `Access-Control-Allow-Headers/Methods`（preflight 回覆用）：允許哪些標頭/方法
- 另外常用：`Access-Control-Expose-Headers`（讓前端 JS 讀到非簡單回應標頭）、`Access-Control-Max-Age`（preflight 緩存）、`Vary: Origin`。

**2. XSS vs CSRF：差異與防護**

一句話：

- XSS：把惡意腳本「塞進你的頁面執行」
- CSRF：誘使「已登入」的使用者在你的站發送他不想要的請求

- XSS 防護

  - 輸出編碼（HTML/URL/JS/Attr 上下文相容的 encode）
  - 避免 `innerHTML` / `dangerouslySetInnerHTML`；需要時用 DOMPurify 類庫
  - CSP（限制 script 來源、用 nonce/hash 允許白名單）
  - HttpOnly Cookie（就算有 XSS 也讀不到 cookie）

- CSRF 防護
  - `SameSite` Cookie（Lax/Strict）
  - `CSRF Token`（Synchronizer Token / Double-Submit）
  - 驗 `Origin`/`Referer`、重要操作要求 重新驗證

注意：HttpOnly 防 XSS 竊 cookie，但不能防 CSRF；CSRF 用的是瀏覽器自動帶 cookie 的特性。

**3. Cookie 屬性：Secure / HttpOnly / SameSite 與第三方 Cookie**

- `Secure`：只在 HTTPS 傳送
- `HttpOnly`：前端 JS 不能讀（document.cookie 看不到）
- `SameSite`：
- `Strict`：任何跨站情境都不送
- `Lax`：頂層導覽的 GET 會送（常見預設），但跨站子資源/POST 不送
- `None`：跨站都送，但必須加 Secure
- 第三方 Cookie 變化：各瀏覽器逐步限制/淘汰第三方 Cookie；常見替代包含 `SameSite` 策略、`Partitioned`/`CHIPS`（分區 Cookie）、`Storage Access API` 等。

**4. HTTP/1.1 vs HTTP/2/3：HOL、Multiplexing、連線策略**

- `HTTP/1.1`：文字協定、一條連線一次一個請求；為避免等待，瀏覽器會開多條（常見 ~6/域名）。有 HOL（Head-of-line）阻塞。
- `HTTP/2`：二進位分幀、多工（multiplexing）、HPACK 壓縮、（曾有）Server Push；仍在單一 TCP 上，多工時 TCP 層仍可能因丟包出現 HOL。
- `HTTP/3`：基於 QUIC(UDP)，真正多工無 TCP-HOL、更快的握手（0-RTT/1-RTT）、連線可遷移（換網仍不斷）。
- 前端策略：在 H2/H3 下不再需要雪碧圖、域名分片、過度合併檔案；通常少量連線即可充分利用多工。

**5. WebSocket：如何由 HTTP 升級？LB 後面要注意什麼？**

握手（HTTP/1.1 Upgrade）

- 客戶端請求：

```http
GET /chat HTTP/1.1
Host: example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Version: 13
Sec-WebSocket-Key: <base64 nonce>
```

- 伺服器回覆：

```http
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: <base64 of SHA1(key+GUID)>
```

LB 注意事項

- 連線長時間存活：調高 空閒逾時（`idle timeout`）
- 黏性/狀態：若後端有連線狀態，需 Sticky session 或把狀態外部化
- 協定支援：確保 LB/反代支援 WS 透傳（ALB/Nginx/Envoy 都可）
- 伸縮/上限：最大連線數、突發、`backpressure`
- TLS：在 LB 終結 TLS，後端走內網

備註：傳統 WS 走 `HTTP/1.1 Upgrade`；也有 `WebSocket over HTTP/2 (Extended CONNECT)` 的新方案，端到端支援度要確認。

**6. REST：資源設計/版本策略；PUT vs PATCH；Idempotency Key**

- 資源設計：名詞、複數、層次清晰：`/users/{id}/posts`；過濾、排序、分頁用查詢參數。
- 版本策略：
- URL：/v1/...（最直接）
- Header/媒體型：Accept: application/vnd.company.v2+json
- 向後相容為原則；破壞性變更才升版
  - PUT vs PATCH：
  - PUT = 整體替換（遺漏欄位通常視為刪除），冪等
  - PATCH = 部分更新（只改變提供的欄位），不一定冪等（建議設計為冪等）
- Idempotency Key：給非冪等操作（如 POST /payments）加一個唯一鍵（Header：Idempotency-Key），伺服器以此去重，重試不重複扣款。

**7. OAuth2 / OIDC：為何 SPA 需要 PKCE？Code + PKCE 流程**

為何：SPA 沒法安全保存 `client secret`，授權碼在瀏覽器路上可能被攔（攔到就能換 token）。PKCE 用動態一次性秘密（`code_verifier`）保護授權碼交換。

流程

1. 客戶端產生 `code_verifier`（高熵隨機）與 `code_challenge` = `BASE64URL(SHA256(verifier))`
2. 導向授權端點：`response_type=code&code_challenge=...&code_challenge_method=S256`
3. 使用者登入並同意 → 回跳帶 code
4. 客戶端以 `code` + `code_verifier` 換取 access token（+ id token, refresh token）
5. 伺服器驗證：`code_challenge` 是否對應 `code_verifier`，正確才發 token

OIDC：在 OAuth2 上加 身份層，回一顆 ID Token (JWT)，含使用者相關 claims（`sub`, `email`, `iss`, `aud`, `exp`…）。

**8. CSP（Content Security Policy）：是什麼？常用 directives？Report-Only 用法？**

一句話：CSP 是瀏覽器的內容載入白名單策略，用標頭限制 script/style/img 來源與執行方式，大幅降低 XSS 風險。

- 常用 directives
  - `default-src`：預設資源來源白名單
  - `script-src`：腳本來源（可用 `'nonce-<rand>'` 或 `'sha256-...'` 允許特定 inline）
  - `style-src`：樣式來源（同上；避免 unsafe-inline）
  - 其他：`img-src`, `connect-src`（XHR/WS/fetch）、`frame-ancestors`（點擊劫持防護）、`object-src`（建議 `'none'`）

Report-Only

- 標頭：`Content-Security-Policy-Report-Only`
- 作用：只回報不封鎖，先觀察有哪些違規（上報到 `report-uri`/`report-to`），逐步收緊策略不會把線上打爆。
