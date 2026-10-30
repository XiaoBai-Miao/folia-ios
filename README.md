# Folia iOS（正规 Capacitor 8 适配）

将 Folia（folia-major / folia-android 0.7.22 的完整 Web 功能）以 **Capacitor 8 + WKWebView** 正规适配到 iOS。

- 无逆向 hack：使用官方 Capacitor iOS 平台，音乐平台网络请求经 **CapacitorHttp**（原生 HTTP，绕过 CORS）
- Android 专属桥（nativeBridge）在 iOS 上自动跳过
- 已配置后台音频播放（`UIBackgroundModes: audio` + AVAudioSession）
- 产物为 **unsigned IPA**，可用 Sideloadly / AltStore 侧载安装

## 构建（GitHub Actions）

仓库根目录有 `.github/workflows/build-ios.yml`，在 Actions 手动触发 `Build Folia iOS`，
macOS runner 会：
1. 把 `dist/` 同步到 `ios/App/App/public`
2. `xcodebuild archive`（unsigned）
3. `ldid` 签名 + 打包 `folia-ios.ipa`
4. 上传 artifact 并发布到 Releases

## 侧载

下载 Release 中的 `folia-ios.ipa`，用 Sideloadly / AltStore 用你的 Apple ID 签名安装。
首次打开在 设置 → 通用 → VPN 与设备管理 → 信任开发者。

## 目录

- `dist/` — Folia Web 构建产物（由 folia-android `pnpm build:capacitor` 生成）
- `ios/` — Capacitor iOS 工程（AppDelegate 已配置后台音频；Info.plist 已配置 ATS/相册权限）
- `capacitor.config.json` — Capacitor 配置（iOS 使用 `ios/App/App/capacitor.config.json`，已启用 CapacitorHttp）