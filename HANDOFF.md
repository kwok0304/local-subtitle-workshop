# 桌面端 Codex 交接

## 仓库
https://github.com/kwok0304/local-subtitle-workshop

## 当前成果
- Android APK 通过 GitHub Actions 云编译。
- 产物仅上传 `arm64-v8a`，适用于 Snapdragon 8 Gen 2，保留 7 天。
- 应用名：本地字幕工坊。
- UI 已做中文化；翻译、模型选择、VAD、导出等功能保留。
- 主工作流：`.github/workflows/build-apk.yml`。
- 注意：此仓库的工作流会克隆上游 `Serkali-sudo/auto-subtitle-generator` 源码，再在构建时应用中文化补丁；仓库当前不是完整 Android 源码镜像。

## 2026-08-25 抖音原生导入
- 已用 `https://v.douyin.com/kJY6N2TxtJw/` 验证作品可下载；作品 ID `7677567300928654598`，成品 MP4 为 25:11.67、1080×1920、H.264/AAC。
- 用户不希望开启 Tailscale（会占用 Android VPN 槽并影响 V2Ray），因此已放弃 Windows 后端方案，也删除了计划任务与服务。
- 当前采用 Android 原生方案：隐藏 WebView 执行抖音网页自身逻辑，从 Performance Resource Timing 取得 `media-video-avc1` 与 `media-audio-mp4a`，手机直接下载并用 APK 已内置的 FFmpeg 合并。
- 同一作品已验证在未登录网页会话下也能取得视频与音频资源。
- Android 构建补丁：`patches/douyin-native-import.patch`。

## 用户真实目标（后续优先级）
用户要的不是手动导入视频，而是：
```
粘贴抖音公开视频分享链接
→ 下载/解析视频
→ 提取音频并转写中文
→ 导出 MD / JSON
→ 供 AI 分析
```

## 已放弃的旧方案
曾验证 `jiji262/douyin-downloader` 的 Windows REST API 路线可行，但它要求手机通过
Tailscale 访问电脑。Android 同时只能使用一个 VPN 槽，会与用户的 V2Ray 冲突，因此不再采用。
Windows 上的测试 API 与开机计划任务均已停用和删除。

## 重要限制
- 不读取或破解抖音 App 的缓存文件。
- 后端需要面对抖音接口变动；优先复用该开源项目并以其更新为准。
