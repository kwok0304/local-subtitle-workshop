# 本地字幕工坊

Android 本地字幕工具。可选择手机里的视频，也可直接粘贴抖音公开视频分享链接：

`WebView 原生解析 → 手机下载视频/音频 → FFmpeg 合并 MP4 → 本地语音识别`

不需要电脑后端、Tailscale 或额外 VPN。GitHub Actions 会克隆固定版本的上游
Android 源码，应用中文化及 [`patches/douyin-native-import.patch`](patches/douyin-native-import.patch)，
然后仅上传 `arm64-v8a` APK。
