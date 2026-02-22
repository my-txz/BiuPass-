# Security Policy

> **Last Updated:** Feb 22, 2026  
> **Version:** v2.1  
> **Status:** Active | Closed Source

## 🔒 Overview / 概述

BiuPass is designed for **trusted Local Area Networks (LAN)** only. While we implement strong encryption (ChaCha20) and HTTPS by default, this tool is **NOT** intended to be exposed directly to the public internet without additional protection layers.

BiuPass 专为**受信任的局域网**设计。虽然默认启用了强加密 (ChaCha20) 和 HTTPS，但本工具**不建议**在无反向代理等额外保护措施的情况下直接暴露于公网。

---

## 🛡️ Supported Versions / 支持版本

We only provide security updates for the latest stable release. Older versions are deprecated and may contain known vulnerabilities.
我们仅为最新稳定版提供安全更新。旧版本已弃用，可能包含已知漏洞。

| Version | Supported          | Notes |
| ------- | ------------------ | ----- |
| 2.1.x   | :white_check_mark: | Current Stable (HTTPS + ChaCha20) |
| 2.0.x   | :white_check_mark:               | Deprecated (No file encryption) |


---

## 🔐 Security Architecture / 安全架构

### 1. Encryption at Rest & Transit
*   **Transit:** All traffic is forced over **HTTPS** using self-signed RSA-2048 certificates. No plain-text passwords or files travel over the wire.
    **传输加密：** 所有流量强制通过 **HTTPS** (自签名 RSA-2048 证书)。密码和文件绝不以明文传输。
*   **Rest:** Files uploaded with the toggle enabled are encrypted using **ChaCha20** stream cipher before hitting the disk. The format is `[Nonce][Ciphertext]`. Without the `server_key` in `config.json`, these files are unreadable garbage.
    **存储加密：** 勾选开关上传的文件在落盘前使用 **ChaCha20** 流密码加密。格式为 `[Nonce][密文]`。若无 `config.json` 中的 `server_key`，这些文件是无法读取的乱码。

### 2. Access Control
*   **PIN Authentication:** Simple but effective PIN-based login.
*   **Brute-Force Protection:** Automatic temporary ban after **5 failed attempts** within 5 minutes per IP.
    **暴力破解防护：** 单 IP 在 5 分钟内失败 **5 次** 自动临时封禁。
*   **CSRF Protection:** All state-changing requests (upload, delete, config change) require a valid CSRF token.
    **CSRF 防护：** 所有状态变更请求均需有效 CSRF Token。
*   **IP Blacklisting:** Admins can instantly ban malicious IPs via the dashboard.
    **IP 黑名单：** 管理员可通过面板即时封禁恶意 IP。

### 3. Headers & Hardening
Every response includes strict security headers:
*   `X-Content-Type-Options: nosniff`
*   `X-Frame-Options: SAMEORIGIN`
*   `X-XSS-Protection: 1; mode=block`
*   `Referrer-Policy: strict-origin-when-cross-origin`

---

## ⚠️ Known Limitations & Risks / 已知限制与风险

1.  **Self-Signed Certificates:**
    Browsers will show a "Not Secure" warning because we generate certs locally. **This is expected.** You must manually trust the cert on each client. Do not ignore this warning if you are on an untrusted network (e.g., public coffee shop WiFi).
    **自签名证书：** 浏览器会提示“不安全”，因为证书是本地生成的。**这是正常的。** 你必须在每个客户端手动信任证书。如果在不可信网络（如公共 WiFi），请勿忽略此警告。

2.  **Config File Sensitivity:**
    The `config.json` file contains the `server_key`. **If you lose this file, all encrypted data is lost forever.** If someone steals this file, they can decrypt your stored files. Keep it safe.
    **配置文件敏感性：** `config.json` 包含 `server_key`。**丢失此文件意味着所有加密数据永久丢失。** 若被窃取，攻击者可解密文件。请妥善保管。

3.  **LAN Only Design:**
    We do not implement complex user management, audit logs, or DDoS protection found in enterprise tools. This is a lightweight utility. Exposing port 5000 directly to the internet is **strongly discouraged**.
    **仅限局域网设计：** 我们没有企业级工具的用户管理、审计日志或 DDoS 防护。这是一个轻量级工具。**强烈不建议**将 5000 端口直接暴露在公网。

---

## 🐛 Reporting a Vulnerability / 报告漏洞

Found a bug? Think you found a security hole?
发现 Bug？觉得有安全漏洞？

1.  **DO NOT** open a public Issue on GitHub describing the vulnerability. This allows attackers to exploit it before we fix it.
    **切勿**在 GitHub Issue 中公开描述漏洞细节。这会让攻击者在修复前利用它。
2.  **Email us directly** or use the "Private Vulnerability Reporting" feature on GitHub (if enabled for our repo).
    **直接发送邮件**给我们，或使用 GitHub 的“私有漏洞报告”功能（如果仓库已启用）。
3.  Include:
    *   Version number.
    *   Steps to reproduce.
    *   Potential impact.
    *   Your suggested fix (optional).

📧 **Contact:** [Insert Your Email Here or Link to Private GH Feature]
*(Since this is a closed source project by a single dev, checking Issues regularly is the best bet if no email is listed).*

---

## 🚫 What We Don't Support / 不支持的情况

*   Running BiuPass on public clouds (AWS/Azure) without a reverse proxy (Nginx/Caddy) and firewall rules.
    在没有反向代理 (Nginx/Caddy) 和防火墙规则的情况下，在公有云 (AWS/Azure) 运行 BiuPass。
*   Recovering data if `config.json` is deleted.
    `config.json` 被删除后的数据恢复。
*   Protecting against physical access attacks (if someone has your hard drive AND your config file, game over).
    防范物理攻击（如果有人同时拥有你的硬盘和配置文件，那就完了）。

---

## 💡 Best Practices for Users / 用户最佳实践

1.  **Change Default PIN:** Immediately change `1234` after first run.
    **修改默认 PIN：** 首次运行后立即修改 `1234`。
2.  **Backup Config:** Copy `config.json` to a USB drive or secure location.
    **备份配置：** 将 `config.json` 复制到 U 盘或安全位置。
3.  **Firewall:** Ensure Windows Firewall only allows connections from "Private" networks, not "Public".
    **防火墙：** 确保 Windows 防火墙仅允许“专用”网络连接，禁止“公用”。
4.  **Verify Cert:** When connecting first time, check the certificate fingerprint matches what's shown in the console (advanced users).
    **验证证书：** 首次连接时，检查证书指纹是否与控制台显示一致（高级用户）。

---

*Built by Pofengzhe. Stay safe.*  
*Security is a process, not a product.*
