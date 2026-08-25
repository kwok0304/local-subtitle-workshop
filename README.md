# 本地字幕工坊

本仓库构建两个互相独立的 Android App：

- **抖音下载助手**：可见 WebView 登录/验证 → 下载并合并 → 保存到 `Movies/DouyinDownloader`；支持导出关键步骤日志。
- **本地字幕工坊**：只负责从手机选择视频、本地语音识别与字幕导出；不含抖音下载入口。

不需要电脑后端、Tailscale 或额外 VPN。下载助手的登录 Cookie 仅保存在 Android
WebView 的 App 私有数据中。GitHub Actions 会克隆固定版本上游、应用补丁，并分别上传
两个 `arm64-v8a` APK。字幕工坊下载 Whisper 模型时优先使用国内 ModelScope
镜像，镜像不可用或缺少文件时自动回退到 Hugging Face 官方源；Vosk 模型继续使用
上游提供的官方地址。模型列表会优先显示 57M 的“中文/多语言 Whisper Base Q5_1”；
选择服务器级中文 Vosk 大模型时，也会提示改用这个更适合手机的国内源模型。
