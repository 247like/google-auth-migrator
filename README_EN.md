# 🛡️ GAuth Migrator Pro (Google Authenticator Migration Tool)

<div align="center">

[中文](./README.md) | English

![License](https://img.shields.io/badge/license-MIT-green.svg)
![Offline](https://img.shields.io/badge/security-Offline%20%26%20Safe-blue.svg)
![Platform](https://img.shields.io/badge/platform-Web%20%7C%20Mobile-lightgrey.svg)

A **pure static, offline** web tool for converting **Google Authenticator** export data (Protobuf format QR codes) into universal formats supported by password managers like **Bitwarden**, **1Password**, **KeePass**, etc.

> 🔴 **Problem Solved**: Google Authenticator's export QR codes use a proprietary Protobuf protocol and contain multiple accounts, making them unreadable by standard QR scanners. This tool is specifically designed to solve this problem.

</div>

---

## ✨ Key Features

* 🔒 **Privacy First**: Code runs entirely in the browser, **never uploads** any images or key data to servers. Supports offline use.
* 🚀 **Triple-Engine Decoding**:
    * Integrates **BarcodeDetector API** (native), **ZXing**, and **jsQR** - three recognition engines with mutual fallback.
    * Built-in **8 image enhancement algorithms** (auto-binarization, Otsu adaptive threshold, sharpening, denoising, high contrast, inversion, 2x scaling) to handle blurry/noisy screenshots.
* 📂 **Batch Processing**: Supports drag-and-drop upload of multiple QR code screenshots at once, with automatic merging, deduplication, and 2-attempt retry on failure.
* 💾 **Smart Storage**:
    * Uses **IndexedDB** for local storage - data persists across page refreshes.
    * **15-minute auto-expiry** mechanism to protect sensitive information.
    * One-click clear function to immediately delete all data after migration.
* 🎨 **Modern Interface**:
    * Clean UI design with **bilingual Chinese/English support** (auto-detects browser language).
    * Real-time progress feedback and friendly error messages.
* 📤 **Multiple Export Formats**:
    * **Bitwarden JSON**: Perfect support for Vaultwarden/Bitwarden import.
    * **Universal CSV**: Supports 1Password, KeePassXC, Enpass, LastPass, etc.
    * **Plain URI Text**: Standard `otpauth://` links, supports Aegis, 2FAS, or regenerating QR codes.
    * **🆕 QR Code Package (ZIP)**: Generates individual QR code images for each key, packaged for download.
* 🔑 **QR Code Viewer**: Each account card can display and download its QR code individually for quick import to other devices.
* 🛠️ **Smart Fixes**:
    * Auto-corrects digit definition issues in Google's protocol (1 -> 6 digits, 2 -> 8 digits).
    * Supports both TOTP and HOTP modes.
    * Auto-matches icons for common services (GitHub, AWS, Google, etc.).

## 🖼️ Interface Preview

![Interface Preview](./screenshot.png)

## 📖 Usage Guide

### Method 1: Import from QR Code Images

#### Step 1: Export from Google Authenticator
1. Open the Google Authenticator app.
2. Tap the menu (top right) -> **Transfer accounts** -> **Export accounts**.
3. Select accounts to export - QR codes will be displayed on screen.
4. **Take screenshots** of these QR codes (if multiple, screenshot each one).

#### Step 2: Parse and Migrate
1. Open this tool's webpage (or run `index.html` locally).
2. Click **"Select Images"** or drag screenshots into the upload area (supports batch upload).
3. The tool will automatically parse and display the account list.
4. Click **"Export Data"** in the top right, choose your desired format (recommended: **Bitwarden JSON** or **QR Code Package**).

### Method 2: Manual Link Parsing

1. Use your phone's camera or WeChat to scan the Google Authenticator export QR code.
2. Copy the resulting `otpauth-migration://...` long link.
3. Click the **"Parse Link"** button in the tool.
4. Paste the link and confirm parsing.

### Step 3: Import to Password Manager
* **Bitwarden/Vaultwarden**: Log in to web version -> Tools -> Import Data -> Select **Bitwarden (json)**.
* **1Password/KeePass/Enpass**: Choose CSV import.
* **Aegis/2FAS**: Import TXT file or scan the generated QR codes.

## 🔐 Security Features

### Data Privacy Protection
- **No Server Communication**: All processing is done locally in the browser, zero network requests
- **Auto-Expiry Mechanism**: Data in IndexedDB is automatically deleted after 15 minutes
- **Manual Clear**: One-click clear button to immediately delete all sensitive data after migration
- **No Tracking/Analytics**: No cookies, statistics, or third-party services used

### Security Best Practices
1. ⚠️ **Must** click the "Clear" button to delete data after migration
2. 🔒 Only use this tool on trusted personal devices
3. 🚫 Never share screenshots or export files
4. ✅ Immediately delete downloaded export files after importing to target app
5. 💡 Recommended to use in offline mode (download and open offline)

## 🛠️ Tech Stack

* **Core Logic**: Vanilla JavaScript (ES6+)
* **UI Framework**: HTML5 + CSS3 (Flexbox/Grid, modern design)
* **QR Code Recognition**:
    * [`BarcodeDetector API`](https://developer.mozilla.org/en-US/docs/Web/API/BarcodeDetector) (native engine, fastest and most accurate)
    * [`zxing-js/library`](https://github.com/zxing-js/library) (primary engine)
    * [`cozmo/jsQR`](https://github.com/cozmo/jsQR) (fallback engine)
* **QR Code Generation**: [`qrcode-generator`](https://github.com/kazuhikoarase/qrcode-generator)
* **Data Storage**: IndexedDB (with auto-expiry mechanism)
* **File Compression**: [`JSZip`](https://github.com/Stuk/jszip)
* **Protocol Parsing**: Custom Protobuf decoder (Base32 encoding, UTF-8 text processing)
* **Internationalization**: Built-in Chinese/English support with auto-detection

## 🌐 Online Version

If you don't mind, you can use my deployed version (pure static hosting):
👉 **[https://g-auth.beiai.de](https://g-auth.beiai.de)**

## 📦 Local Deployment (Recommended)

For maximum security, it's recommended to download and run locally:

1. Clone the repository:
   ```bash
   git clone https://github.com/247like/google-auth-migrator.git
   cd google-auth-migrator
   ```

2. Open `index.html` directly in your browser (no build required).

3. *(Optional)* For a completely clean environment, disconnect from the internet before use.

## 🌍 Browser Compatibility

| Browser | Version | Support |
|---------|---------|---------|
| Chrome  | 88+     | ✅ Full Support |
| Edge    | 88+     | ✅ Full Support |
| Firefox | 78+     | ✅ Full Support |
| Safari  | 14+     | ⚠️ Partial Support (no BarcodeDetector) |
| Opera   | 74+     | ✅ Full Support |

## 📋 Supported Export Formats

### Bitwarden JSON
Fully compatible with Bitwarden and Vaultwarden, contains complete TOTP URI information.

### Universal CSV
Standard CSV format with account name, secret, issuer, algorithm, etc. - supported by most password managers.

### Plain URI Text
One standard `otpauth://` link per line, can be used for:
- Aegis Authenticator batch import
- 2FAS Auth import
- Regenerating QR codes

### QR Code Package ZIP
Generates high-quality QR code images (PNG format) for each key, useful for:
- Printed backups
- Multi-device scanning and import
- Sharing with family (use with caution)

## 🔬 Advanced Features

### Intelligent Image Processing Strategy

The tool tries the following strategies in sequence until successful recognition:

1. **Native API**: BarcodeDetector (if browser supports, fastest)
2. **Raw Image**: jsQR + ZXing
3. **Sharpen Filter**: Edge enhancement + jsQR
4. **Otsu Adaptive Binarization**: Auto-calculate optimal threshold + jsQR
5. **Fixed Threshold Binarization**: Threshold 128 + jsQR
6. **Median Denoising**: Remove salt-and-pepper noise + jsQR
7. **High Contrast Enhancement**: jsQR + ZXing
8. **Color Inversion**: Handle black background white code + jsQR
9. **2x Upscaling**: Improve small image recognition + jsQR

### Protobuf Protocol Parsing

Correctly handles Google Authenticator migration data format:
- Base64URL decoding (auto-padding)
- UTF-8 text field decoding (account name, issuer)
- Binary secret to Base32 encoding conversion
- Supports TOTP (time-based) and HOTP (counter-based)
- Multiple algorithm support (SHA1, SHA256, SHA512, MD5)

## 🤝 Contributing

Pull Requests and Issues are welcome!

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## ⚠️ Disclaimer

This tool is designed to help users migrate personal data.

1. **Security Warning**: Exported JSON/CSV/TXT files contain all your 2FA secrets (TOTP Secret). **Please keep them safe and immediately delete export files after importing**.
2. After migration, please verify that verification codes work correctly on the new device. Do not delete 2FA configuration on the original device until confirmed working.
3. The author is not responsible for any data loss or leakage caused by using this tool.

## 📄 License

This project is licensed under the [MIT License](LICENSE).

## 🙏 Acknowledgments

- [jsQR](https://github.com/cozmo/jsQR) - QR code parsing library
- [ZXing](https://github.com/zxing-js/library) - Backup QR parsing engine
- [qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator) - QR code generation library
- [JSZip](https://github.com/Stuk/jszip) - ZIP file creation library

---

<div align="center">

If you find this tool useful, please give it a ⭐ star!

[Report Bug](https://github.com/247like/google-auth-migrator/issues) · [Request Feature](https://github.com/247like/google-auth-migrator/issues)

</div>
