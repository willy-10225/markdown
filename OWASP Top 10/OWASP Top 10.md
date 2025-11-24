# 🛡️ OWASP Top 10 — Security Guide for C# WebForms & Python  
**Author:** Willy Lin (林俊宇)  
**Company:** 虎門科技  
**Version:** 2025  
**Project Type:** Web Security | .NET WebForms | Python | 教育競賽系統（EulerCup / UGM / FloBook）

---

## 📛 Badges  
![Language](https://img.shields.io/badge/language-C%23-blue.svg)
![Language](https://img.shields.io/badge/language-Python-yellow.svg)
![Security](https://img.shields.io/badge/Security-OWASP%20Top%2010-critical.svg)
![Framework](https://img.shields.io/badge/.NET-WebForms-green.svg)
![License](https://img.shields.io/badge/license-MIT-lightgrey.svg)
![Status](https://img.shields.io/badge/Status-Active-brightgreen.svg)

---

# 📘 OWASP Top 10 – 完整資安指南（含 C# WebForms + Python）

本文件是針對 **台灣常見企業專案 / 校園競賽平台 / 教育系統 / WebForms 遺留系統**  
所設計的 **完整 OWASP Top 10 安全指南**。

內容包含：

- 攻擊介紹（白話＋實際 WebForms 例子）
- 涉及攻擊手法（含 CSRF / XSS / Session Fixation）
- 防禦方式（技術＋流程＋架構）
- C# WebForms 程式碼
- Python Flask 程式碼
- 可直接放在 GitHub 的完整 Markdown

---

# 📑 目錄（自動 TOC）

- [A01 – 權限控制失效（Broken Access Control）](#a01--權限控制失效broken-access-control)
- [A02 – 加密機制失效（Cryptographic Failures）](#a02--加密機制失效cryptographic-failures)
- [A03 – 注入攻擊（Injection）](#a03--注入攻擊injection)
- [A04 – 不安全設計（Insecure Design）](#a04--不安全設計insecure-design)
- [A05 – 安全設定錯誤（Security Misconfiguration）](#a05--安全設定錯誤security-misconfiguration)
- [A06 – 過時組件（Vulnerable and Outdated Components）](#a06--過時組件vulnerable-and-outdated-components)
- [A07 – 身分驗證失效（Identification & Authentication Failures）](#a07--身分驗證失效identification--authentication-failures)
- [A08 – 軟體與資料完整性問題（Software & Data Integrity Failures）](#a08--軟體與資料完整性問題software--data-integrity-failures)
- [A09 – 記錄與監控不足（Security Logging and Monitoring Failures）](#a09--記錄與監控不足security-logging-and-monitoring-failures)
- [A10 – SSRF（Server-Side Request Forgery）](#a10--ssrfserver-side-request-forgery)

---

# A01 – 權限控制失效（Broken Access Control）

## 📘 攻擊介紹
後端未檢查使用者的身份與角色 → 攻擊者能：

- 看他人資料
- 修改他人資料
- 一般使用者變管理員
- 直接推 URL 看內容（IDOR）
- 未登入也能存取

## 🔥 涉及攻擊
- IDOR（Insecure Direct Object Reference）
- 水平越權 / 垂直越權
- CSRF 結合越權攻擊
- Session Fixation

## 🛡 防禦方式
- 所有修改頁面都要驗證 Session["role"]
- URL 不可作為身份依據
- 更新 Session ID（防 Fixation）

## 🧩 C# WebForms
```csharp
if (Session["user"] == null)
    Response.Redirect("~/login.aspx");

if (Session["role"].ToString() != "judge")
{
    Response.StatusCode = 403;
    Response.End();
}
