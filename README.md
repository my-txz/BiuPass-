# BiuPass  快速局域网传输程序。

> **Closed Source Software. All Rights Reserved.**  
> 闭源软件，版权所有。

### 目前作者不打算开源，但不排除将来开源的可能性。

![Python](https://img.shields.io/badge/Python-3.11+-blue.svg)
![Flask](https://img.shields.io/badge/Flask-Latest-green.svg)
![License](https://img.shields.io/badge/License-Closed%20Source-red.svg)

## Introduction / 简介

BiuPass is a lightweight, secure file transfer system designed for enterprise LAN environments. It focuses on privacy, ease of use, and broad compatibility without requiring complex server setups.  
BiuPass 是一个专为的企业局域网设计的轻量级安全文件传输系统。专注于隐私保护、易用性以及广泛的兼容性，无需复杂的服务器配置即可运行。

Built with Python 3.11+ and Flask, it runs smoothly on Windows, Linux, and macOS. Whether you are in a dormitory, office, or lab, just start the script and share files instantly.  
基于 Python 3.11+ 和 Flask 构建，可在 Windows、Linux 和 macOS 上流畅运行。无论你在宿舍、办公室还是实验室，只需启动脚本即可即时共享文件。

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

-   **Cross-Platform Compatibility:** Runs on any device with Python support.  
    **跨平台兼容：** 在任何支持 Python 的设备上均可运行。
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

### 3. Access / 访问
Open your browser and go to `http://<your-ip>:5000`. The default PIN is `1234`.  
打开浏览器访问 `http://<你的 IP>:5000`。默认 PIN 码为 `1234`。

## Configuration / 配置说明

The `config.json` file handles all settings. You don't need to edit it manually usually, but here are the key parameters:  
`config.json` 文件处理所有设置。通常无需手动编辑，但以下是关键参数：

-   `server.port`: Listening port (Default: 5000).  
    监听端口（默认：5000）。
-   `security.default_pin`: Access password. Change this immediately after first login.  
    访问密码。首次登录后请立即修改。
-   `storage.upload_folder`: Where files are saved (Default: `./uploads`).  
    文件保存位置（默认：`./uploads`）。
-   `transfer.chunk_size_kb`: Buffer size for transfer stability.  
    传输缓冲区大小。

## Technical Details / 技术细节

For those interested in the underlying logic:  
对于关心底层逻辑的朋友：

-   **Encryption:** Uses a custom `StreamCipher` based on SHA256 hashing for lightweight obfuscation.  
    **加密：** 使用基于 SHA256 哈希的自定义 `StreamCipher` 进行轻量级混淆。
-   **Security:** CSRF protection enabled by default. Download tokens expire after 5 minutes (TTL).  
    **安全：** 默认启用 CSRF 保护。下载 Token 5 分钟后过期。
-   **Concurrency:** Multi-threaded file handling to prevent blocking during uploads.  
    **并发：** 多线程文件处理，防止上传时阻塞。
-   **Device Info:** Frontend detects OS and Browser info for compatibility logging.  
    **设备信息：** 前端检测操作系统和浏览器信息以记录兼容性。

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

