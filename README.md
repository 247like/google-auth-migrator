# 🛡️ GAuth Migrator Pro (Google Authenticator 迁移助手)

<div align="center">

中文 | [English](./README_EN.md)

![License](https://img.shields.io/badge/license-MIT-green.svg)
![Offline](https://img.shields.io/badge/security-Offline%20%26%20Safe-blue.svg)
![Platform](https://img.shields.io/badge/platform-Web%20%7C%20Mobile-lightgrey.svg)

一个**纯静态、离线运行**的网页工具，用于将 **Google Authenticator (Google 验证器)** 的导出数据（Protobuf 格式二维码）转换为 **Bitwarden**、**1Password**、**KeePass** 等密码管理器支持的通用格式。

> 🔴 **痛点解决**：Google Authenticator 的导出二维码使用私有 Protobuf 协议，且包含多个账户，普通扫码器无法识别。本工具专为解决此问题而生。

</div>

---

## ✨ 核心特性

* 🔒 **隐私优先**：代码完全在浏览器端运行，**绝不上传**任何图片或密钥数据到服务器。支持断网使用。
* 🚀 **三引擎暴力解析**：
    * 集成 **BarcodeDetector API**（原生）、**ZXing** 和 **jsQR** 三重识别引擎，互为兜底。
    * 内置**8种图像增强算法**（自动二值化、Otsu自适应阈值、锐化、去噪、高对比度、反色、2倍放大），专治微信/截图导致的图片模糊、噪点问题。
* 📂 **批量处理**：支持一次性拖拽上传多张二维码截图，自动合并与排重，失败自动重试2次。
* 💾 **智能存储**：
    * 使用 **IndexedDB** 本地存储，页面刷新不丢失数据。
    * **15分钟自动过期**机制，保护敏感信息安全。
    * 一键清空功能，迁移完成后立即删除所有数据。
* 🎨 **现代化界面**：
    * 清爽的 UI 设计，支持**中英文双语切换**（自动检测浏览器语言）。
    * 实时进度反馈、友好的错误提示。
* 📤 **多格式导出**：
    * **Bitwarden JSON**：完美支持 Vaultwarden/Bitwarden 导入。
    * **通用 CSV**：支持 1Password, KeePassXC, Enpass, LastPass 等。
    * **纯文本 URI**：标准 `otpauth://` 链接，支持 Aegis、2FAS 或再次生成二维码。
    * **🆕 二维码打包 (ZIP)**：为每个密钥生成独立的二维码图片，打包下载。
* 🔑 **QR 码查看**：每个账户卡片可单独查看并下载二维码，方便快速导入其他设备。
* 🛠️ **智能修复**：
    * 自动修正 Google 协议中的位数定义问题（1 -> 6位, 2 -> 8位）。
    * 支持 TOTP 和 HOTP 两种模式。
    * 自动匹配常见服务（GitHub、AWS、Google 等）的图标。

## 🖼️ 界面预览

![界面预览](./screenshot.png)

## 📖 使用指南

### 方法一：从二维码图片导入

#### 第一步：从 Google Authenticator 导出
1.  打开 Google Authenticator App。
2.  点击右上角菜单 -> **转移账号** -> **导出账号**。
3.  选择要导出的账号，屏幕会显示**二维码**。
4.  **截图**保存这些二维码（如果有多个二维码，请依次截图）。

#### 第二步：解析与迁移
1.  打开本工具网页（或本地运行 `index.html`）。
2.  点击 **"选择图片"** 或直接将截图拖入上传区（支持批量）。
3.  工具会自动解析并显示账户列表。
4.  点击右上角的 **"导出数据"**，选择你需要的格式（推荐 **Bitwarden JSON** 或 **二维码打包**）。

### 方法二：从链接手动解析

1.  使用手机系统相机或微信扫描 Google Authenticator 导出的二维码。
2.  复制获得的 `otpauth-migration://...` 长链接。
3.  点击工具上的 **"链接解析"** 按钮。
4.  粘贴链接并确认解析。

### 第三步：导入密码管理器
* **Bitwarden/Vaultwarden**：登录网页版 -> 工具 -> 导入数据 -> 选择 **Bitwarden (json)**。
* **1Password/KeePass/Enpass**：选择 CSV 导入。
* **Aegis/2FAS**：导入 TXT 文件或扫描生成的二维码。

## 🔐 安全功能

### 数据隐私保护
- **无服务器通信**：所有处理均在浏览器本地完成，零网络请求
- **自动过期机制**：IndexedDB 中的数据在 15 分钟后自动清除
- **手动清空**：提供一键清空按钮，迁移完成后立即删除所有敏感数据
- **无追踪分析**：不使用任何 Cookie、统计代码或第三方服务

### 安全使用建议
1. ⚠️ **务必**在迁移完成后点击"清空"按钮删除数据
2. 🔒 仅在可信任的个人设备上使用本工具
3. 🚫 切勿分享截图或导出文件
4. ✅ 导入到目标应用后，立即删除下载的导出文件
5. 💡 建议在无网络环境下使用（下载后离线打开）

## 🛠️ 技术栈

* **核心逻辑**: Vanilla JavaScript (ES6+)
* **UI 框架**: HTML5 + CSS3 (Flexbox/Grid、现代化设计)
* **二维码识别**:
    * [`BarcodeDetector API`](https://developer.mozilla.org/en-US/docs/Web/API/BarcodeDetector) (原生引擎，最快最准)
    * [`zxing-js/library`](https://github.com/zxing-js/library) (主力引擎)
    * [`cozmo/jsQR`](https://github.com/cozmo/jsQR) (兜底引擎)
* **二维码生成**: [`qrcode-generator`](https://github.com/kazuhikoarase/qrcode-generator)
* **数据存储**: IndexedDB (自动过期机制)
* **文件压缩**: [`JSZip`](https://github.com/Stuk/jszip)
* **协议解析**: 手写 Protobuf 解码器 (Base32 编码、UTF-8 文本处理)
* **国际化**: 内置中英双语支持，自动检测浏览器语言

## 🌐 在线使用

如果不介意，可直接用我部署的版本（纯静态托管）：
👉 **[https://g-auth.beiai.de](https://g-auth.beiai.de)**

## 📥 本地离线部署 (推荐)

为了最大程度保证安全，防止任何数据泄露风险，建议使用**全离线单文件版本**：

1.  前往 [Releases 页面](https://github.com/247like/google-auth-migrator/releases) 下载最新的 `google-auth-offline.html` 文件。
2.  **断开网络连接** (拔掉网线或关闭 Wi-Fi)。
3.  直接双击打开下载的 `google-auth-offline.html`。
    * *注：此版本已内置所有依赖库（JS/CSS），无需联网即可完整运行。*

---
*(如果你是开发者，也可以 clone 仓库并运行脚本自行构建离线版)*

## 🌍 浏览器兼容性

| 浏览器 | 版本 | 支持情况 |
|--------|------|----------|
| Chrome | 88+  | ✅ 完全支持 |
| Edge   | 88+  | ✅ 完全支持 |
| Firefox| 78+  | ✅ 完全支持 |
| Safari | 14+  | ⚠️ 部分支持 (无 BarcodeDetector) |
| Opera  | 74+  | ✅ 完全支持 |

## 📋 支持的导出格式

### Bitwarden JSON
完美兼容 Bitwarden 和 Vaultwarden，包含完整的 TOTP URI 信息。

### 通用 CSV
标准 CSV 格式，包含账户名、密钥、发行者、算法等信息，支持多数密码管理器。

### 纯 URI 文本
每行一个标准 `otpauth://` 链接，可用于：
- Aegis Authenticator 批量导入
- 2FAS Auth 导入
- 重新生成二维码

### 二维码打包 ZIP
为每个密钥生成高清二维码图片（PNG 格式），方便：
- 打印备份
- 多设备扫码导入
- 分享给家人（谨慎使用）

## 🔬 高级特性

### 智能图像处理策略

工具会依次尝试以下策略，直到成功识别：

1. **原生 API**：BarcodeDetector（如果浏览器支持，速度最快）
2. **原始图像**：jsQR + ZXing
3. **锐化滤镜**：增强边缘 + jsQR
4. **Otsu 自适应二值化**：自动计算最佳阈值 + jsQR
5. **固定阈值二值化**：128 阈值 + jsQR
6. **中值去噪**：消除椒盐噪点 + jsQR
7. **高对比度增强**：jsQR + ZXing
8. **颜色反转**：处理黑底白码 + jsQR
9. **2倍放大**：提升小图片识别率 + jsQR

### Protobuf 协议解析

正确处理 Google Authenticator 迁移数据格式：
- Base64URL 解码（自动补齐 padding）
- UTF-8 文本字段解码（账户名、发行者）
- 二进制密钥转 Base32 编码
- 支持 TOTP（时间）和 HOTP（计数器）
- 多种算法支持（SHA1、SHA256、SHA512、MD5）

## 🤝 贡献指南

欢迎提交 Pull Request 或 Issue！

1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启 Pull Request

## ⚠️ 免责声明

本工具旨在帮助用户迁移个人数据。

1.  **安全警告**：导出的 JSON/CSV/TXT 文件包含您所有的 2FA 密钥（TOTP Secret），**请务必妥善保管，导入完成后立即彻底删除导出文件**。
2.  迁移后请务必验证新设备上的验证码是否正常工作，在确认无误前不要删除原设备上的 2FA 配置。
3.  作者不对因使用本工具导致的数据丢失或泄露承担任何责任。

## 📄 开源协议

本项目采用 [MIT License](LICENSE) 开源。

## 🙏 致谢

- [jsQR](https://github.com/cozmo/jsQR) - QR 码解析库
- [ZXing](https://github.com/zxing-js/library) - 备用 QR 解析引擎
- [qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator) - 二维码生成库
- [JSZip](https://github.com/Stuk/jszip) - ZIP 文件创建库

---

<div align="center">

如果您觉得这个工具好用，请给一颗 ⭐ 星！

[报告问题](https://github.com/247like/google-auth-migrator/issues) · [功能建议](https://github.com/247like/google-auth-migrator/issues)

</div>
