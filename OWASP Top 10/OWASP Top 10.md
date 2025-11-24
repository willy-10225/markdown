# OWASP Top 10 – 完整安全指南 (Markdown 版本)

本文件整理自你之前與我討論的所有 OWASP Top 10 主題，包含：

- 中文名稱
- 英文名稱
- 攻擊原理
- 攻擊方式（前端 → 後端流程）
- 包含的子攻擊種類
- 防禦方式（全局概念）
- C# / Python 程式碼示範 (局部代碼)

---

# 🛡️ OWASP Top 10 — Web 應用程式安全風險完整說明

---

## 1. **存取控制破壞 (Broken Access Control)**

### 🔍 介紹

攻擊者利用後端未正確驗證「身分是否有權限」的漏洞，直接操作 URL、修改參數、或繞過前端 UI，存取本不該存取的後端 API 或檔案。

### 💥 常見攻擊種類

- 水平權限提升：普通使用者讀取別人資料
- 垂直權限提升：一般帳戶操作管理員功能
- URL 直接存取
- 強制瀏覽（Force Browsing）
- 修改 Cookie 或 Session 權限

### 🛡 防禦方式

- 所有敏感頁面後端必須檢查權限（不要依賴前端）
- API 層檢查 session / token / role
- 限制目錄瀏覽
- 鎖定 ID 參數與帳戶關係

### 🧩 C#（ASP.NET Web Forms）

```csharp
if (Session["role"]?.ToString() != "admin")
{
    Response.StatusCode = 403;
    Response.End();
}
```

### 🧩 Python（Flask）

```python
if session.get("role") != "admin":
    return "Forbidden", 403
```

---

## 2. **加密失效 (Cryptographic Failures)**

### 🔍 介紹

使用弱加密、錯誤加密方式、或直接明文儲存敏感資料（密碼、手機、Email、身分資訊）。

### 💥 子攻擊種類

- 密碼明文儲存
- 使用 SHA-1、MD5
- 未加鹽的 SHA256
- 明文傳輸資料
- 私密金鑰洩漏

### 🛡 防禦方式

- 密碼使用 PBKDF2 / bcrypt / Argon2
- 敏感資料加密後存 DB
- HTTPS 強制啟用
- 秘密金鑰存於環境變數

### 🧩 C# PBKDF2 密碼儲存

```csharp
using var rng = RandomNumberGenerator.Create();
var salt = new byte[16];
rng.GetBytes(salt);

var pbkdf2 = new Rfc2898DeriveBytes(password, salt, 10000);
var hash = pbkdf2.GetBytes(32);

return $"{10000}.{Convert.ToBase64String(salt)}.{Convert.ToBase64String(hash)}";
```

---

## 3. **注入攻擊 (Injection)**

### 🔍 介紹

攻擊者透過未清洗的輸入，把惡意指令注入你的 SQL、OS 指令、XPath、LDAP、NoSQL 中。

### 💥 子攻擊種類

- SQL Injection
- OS Command Injection
- LDAP Injection
- NoSQL Injection
- ORM Injection

### 🛡 防禦方式

- 統一使用 Prepared Statement
- 禁止字串拼接 SQL
- ORM 使用參數化查詢
- Validate Input

### 🧩 C# 防 SQL Injection

```csharp
var cmd = new SqlCommand("SELECT * FROM Users WHERE id=@id", con);
cmd.Parameters.AddWithValue("@id", userId);
```

---

## 4. **不安全設計 (Insecure Design)**

### 🔍 介紹

根本性的設計漏洞，例如：

- 沒有限制登入錯誤次數
- 沒有權限分級
- 壞的 Session 設計
- 未考慮攻擊流程

### 🛡 防禦方式

- Threat Modeling
- 強制 RBAC 權限模型
- 設計階段就加入安全性需求

---

## 5. **安全設定錯誤 (Security Misconfiguration)**

### 🔍 介紹

伺服器、DB、IIS、Nginx、CORS、Headers 設得太鬆導致可被攻擊。

### 💥 子攻擊

- Directory Listing
- 預設帳密未修改
- Headers 缺失（X-Frame、X-XSS、防 MIME sniffing）
- 錯誤訊息顯示 StackTrace

### 🛡 防禦方式

- 關閉 Directory Listing
- 自訂 error pages
- 強化 HTTP Security Headers

---

## 6. **易受攻擊與舊元件 (Vulnerable & Outdated Components)**

### 🔍 介紹

你使用的 DLL、NuGet、Python packages 版本太舊且含漏洞。

### 🛡 防禦方式

- pip / NuGet 定期升級
- 不使用 EOL 的框架
- 啟用自動安全更新
- 用 GitHub Dependabot

---

## 7. **身分驗證與 Session 管理破壞 (Broken Authentication & Session Management)**

### 🔍 介紹

攻擊者盜取或偽造 SessionID 進入你的帳號。

### 💥 子攻擊

- Session Fixation
- SessionID 可猜測
- Cookie 可竄改
- 密碼重置無限次
- 永不過期的 Session

### 🛡 防禦方式

- 登入後重新產生 SessionID
- Cookie 使用 HttpOnly + Secure
- 限制 Session timeout
- 加上第二組 Token（Double Submit Cookie）

### 🧩 C#（登入後重新產生 SessionID）

```csharp
SessionIDManager manager = new SessionIDManager();
string newID = manager.CreateSessionID(Context);
manager.SaveSessionID(Context, newID, out _, out _);
```

---

## 8. **軟體與資料完整性失效 (Software Integrity Failures)**

### 🔍 介紹

攻擊者修改你的更新檔、程式碼、JSON、CDN 外部程式庫。

### 🛡 防禦方式

- 確保更新來源可信（如 HTTPS）
- CDN 檔案使用 SRI（Subresource Integrity）
- 版本簽名檢查

---

## 9. **安全記錄與監控不足 (Security Logging & Monitoring Failures)**

### 🔍 介紹

你沒有 Log、Log 沒寫錯誤、防駭事件無法追查。

### 🛡 防禦方式

- 重要事件記錄 Log（登入失敗、異常行為）
- 使用 NLog / Serilog
- 監控 500 / 403 事件

### 🧩 C# NLog Example

```csharp
logger.Error(ex, "Login error!");
```

---

## 10. **伺服器端請求偽造（SSRF） (Server-Side Request Forgery)**

### 🔍 介紹

攻擊者誘導你的後端去抓「他指定的網址」，達到：

- 掃描內網 IP
- 讀取 Metadata（AWS / Azure）
- 變相當 Proxy

### 🛡 防禣方式

- 後端對外部 URL 做白名單限制
- 禁止讀內網（127.0.0.1、169.254.169.254）
- 禁止自由轉發 URL

### 🧩 C# SSRF 防禦

```csharp
if (!url.StartsWith("https://trusted.com"))
    throw new Exception("Blocked SSRF attempt");
```

---

# ✔ 完整版 OWASP Top10 整理完畢

如需：

- 加入圖示版
- 加入流程圖
- 加入各攻擊的「真實範例」
- 區分 WebForms / MVC / .NET Core 防禦

我也可以繼續擴充。
