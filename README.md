# 本地字幕工坊

本仓库构建两个互相独立的 Android App：

- **抖音下载助手**：可见 WebView 登录/验证 → 下载并合并 → 保存到系统下载目录；支持导出关键步骤日志。
- **本地字幕工坊**：从手机选择下载好的视频 → 本地语音识别与字幕导出。

不需要电脑后端、Tailscale 或额外 VPN。下载助手的登录 Cookie 仅保存在 Android
WebView 的 App 私有数据中。GitHub Actions 会克隆固定版本上游、应用补丁，并分别上传
两个 `arm64-v8a` APK。
