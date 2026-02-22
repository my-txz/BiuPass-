# BiuPass  快速局域网传输程序。

> **Closed Source Software. All Rights Reserved.**  
> 闭源软件，版权所有。

## 快速 、 方便 、 兼容性广 。

目前作者不打算开源，但不排除将来开源的可能性。

![Python](https://img.shields.io/badge/Python-3.11+-blue.svg)
![Flask](https://img.shields.io/badge/Flask-Latest-green.svg)
![Web](https://img.shields.io/badge/web_client-Any%20Browser-lightgrey)
![License](https://img.shields.io/badge/License-Closed%20Source-red.svg)

## Introduction / 简介

BiuPass is a lightweight, secure file transfer system designed for enterprise LAN environments. It focuses on privacy, ease of use, and broad compatibility without requiring complex server setups.  
BiuPass 是一个专为的企业局域网设计的轻量级安全文件传输系统。专注于隐私保护、易用性以及广泛的兼容性，无需复杂的服务器配置即可运行。

Built with Python 3.11+ and Flask, it runs smoothly on Windows, Linux, and macOS. Whether you are in a dormitory, office, or lab, just start the script and share files instantly.  
基于 Python 3.11+ 和 Flask 构建，可在 Windows 上流畅运行。无论你在宿舍、办公室还是实验室，只需启动脚本即可即时共享文件。

## ⚠️ Important Notice / 重要提示

**This project is Closed Source.**  
**本项目为闭源软件。**

We do not plan to open-source the codebase. The compiled binaries are available for download, but the source code is proprietary. Please respect the intellectual property.  
我们不打算开源代码库。仅提供编译后的二进制文件下载，源代码为专有财产。请尊重知识产权。

-   **No Source Code Request:** Please do not ask for the source code in issues.  
    **勿索要源码：** 请不要在 Issues 中索要源代码。
-   **Security:** Do not deploy this on public networks. It is designed for trusted LANs only.  
    **安全性：** 请勿部署在公网。本系统仅设计用于受信任的局域网。

## Features / 特性
-   **Secure Transfer:** Uses stream cipher encryption and token-based download links.  
    **安全传输：** 使用流密码加密和基于 Token 的下载链接。
-   **Admin Control:** Host can approve password change requests from clients.  
    **管理员控制：** 主机可审批客户端的密码修改申请。
-   **Responsive UI:** Supports Dark Mode and Invert Mode automatically.  
    **响应式界面：** 自动支持深色模式和反色模式。
-   **Large File Support:** Configurable chunk size for stable transfers.  
    **大文件支持：** 可配置分块大小以保证传输稳定。

## Quick Start / 快速开始

### 1. Download / 下载
Get the latest release from the Releases page.  
从 Releases 页面获取最新版本。

👉 [Download Here / 点击下载](https://github.com/my-txz/BiuPass-/releases)

### 2. Install Dependencies / 安装依赖
文件已打包好，不需要安装格外依赖
如果遇到文件打不开的情况，请下载 Visual Studio 2022 ，下载链接：https://visualstudio.microsoft.com/zh-hans/downloads/

The files have been packaged and do not require installation of additional dependencies.
If you encounter issues opening the file, please download Visual Studio 2022. Download link:
https://visualstudio.microsoft.com/zh-hans/downloads/

## 🚀 How to Use




### 1. Download & Run (Host)
Go to the [Releases](https://github.com/my-txz/BiuPass-/releases) page and grab the latest Releases.

*   **Double click** the exe file.
*   That's it. No `pip install`, no python env setup. It's bundled.
*   Wait a second. You'll see a black window showing:
    ```text
    Server IP: 192.168.1.55
    Password: 1234
    Security: HTTPS Enabled (Self-Signed)
    ```
*   **Note:** The first time you run it, it generates SSL certificates (`cert.pem`, `key.pem`). This might take a few seconds depending on your CPU.

### 2. Access from Clients
Pick up your phone or another laptop connected to the **same WiFi/LAN**.

*   Open Chrome, Safari, Edge, whatever.
*   Type the address shown in the host window: `https://192.168.1.55:5000`
*   **Warning:** Your browser will scream "Your connection is not private" or "Risk detected". **This is normal.** It's because we use a self-signed certificate.
    *   Click **Advanced** -> **Proceed to ... (unsafe)**.
    *   On iPhone Safari: Tap "Show Details" -> "visit this website".
*   Login with the PIN (default `1234`).

### 3. Uploading & Downloading
*   **Upload:** Drag files into the box. Toggle the "Enable Encryption" switch if you want the file stored encrypted.
*   **Download:** Click the green button. Links expire in 5 mins for safety.
*   **Delete:** You can delete your own files. Admins can delete anything.

---

## ⚙️ Configuration

A `config.json` file appears in the same folder after first run. You can tweak things there. Restart the exe to apply changes.

```json
{
  "server": {
    "port": 5000,          // Change port if 5000 is busy
    "use_https": true,     // Set false if you really don't want HTTPS (not recomended)
    "ssl_cert": "cert.pem",// Cert file path
    "ssl_key": "key.pem"   // Key file path
  },
  "security": {
    "default_pin": "1234", // CHANGE THIS!
    "encryption_enabled_default": true, // Default state of the encrypt switch
    "max_login_attempts": 5 // Brute-force protection limit
  },
  "storage": {
    "upload_folder": "./uploads", // Where files go
    "temp_folder": "./temp_chunks"// Temp space for processing
  }
}
```

**Pro tip:** If you lose your password, just delete `config.json`. It resets to default.

---

## 🛠 Technical Stuff (for nerds)

*   **Crypto:** We use `cryptography` lib internally. ChaCha20 for data, RSA 2048 for cert generation. Nonce is stored with the file header.
*   **Concurrency:** Pure threaded model. Config file writes are locked to prevent corruption if multiple admins click at once.
*   **Memory:** File downloads use `yield` generators. Even if you have 1GB RAM, you can transfer 100GB files smoothly.
*   **Security Headers:** HSTS ready (though not forced yet to avoid lockout), X-Frame-Options, XSS protection enabled.
*   **Rate Limiting:** Login attempts are tracked per IP. 5 fails = 5 min ban.

---

## ⚠️ Important Notes

1.  **Windows Only for Host:** Right now, the `.exe` is compiled for Windows. We might do Linux/Mac later but no promises. Don't ask for source to compile it yourself.
2.  **Self-Signed Certs:** Browsers hate self-signed certs. You *must* manually accept the warning every time you visit a new IP or after cert regeneration. It's safe though, promise.
3.  **LAN Only:** This is NOT for public internet. Do not port forward this to the web unless you know exactly what you are doing. It's designed for trusted local networks.
4.  **Firewall:** If others can't connect, check your Windows Firewall. Allow `BiuPass_v2.1.exe` through private networks.
5.  **Closed Source:** Stop asking for the code. It's closed. Forever. Use the binary or don't use it.

---

## 🐛 Known Issues & Compatiblity

*   **IE Browser:** Might look ugly. Use Chrome or Edge plz.
*   **Huge Files:** If uploading >10GB, ensure your disk has space. The temp folder cleans up but better safe than sorry.
*   **Mobile Safari:** Sometimes the "Add to Home Screen" breaks the drag-drop zone. Just tap the box to select files instead.
*   **Antivirus:** Some aggressive AVs might flag the `.exe` because it creates network sockets and encrypts stuff. It's a false positive. Add an exception if needed.

---


## Support & Issues / 支持与问题

If you encounter bugs, please check the Wiki first. If it's a genuine bug, submit an issue.  
如果遇到 Bug，请先查看 Wiki。如果是真正的 Bug，请提交 Issue。

-   📖 **Usage Wiki / 使用教程:** [https://github.com/my-txz/BiuPass-/wiki](https://github.com/my-txz/BiuPass-/wiki)
-   🐛 **Bug Report / 问题反馈:** [https://github.com/my-txz/BiuPass-/issues](https://github.com/my-txz/BiuPass-/issues)

---

**Dev Note:**  
I made this tool primarily for my own lab and dorm usage. It's stable enough for daily tasks. Don't expect enterprise-grade SLA, but it won't let you down for quick transfers.  
**开发者注：**  
我做这个工具主要是为了自己的实验室和宿舍使用。对于日常任务来说足够稳定。不要指望企业级的 SLA，但对于快速传输来说不会让你失望。

