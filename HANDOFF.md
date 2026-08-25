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

## 2026-08-25 字幕 App 清理与国内模型源
- 本地字幕工坊恢复为纯字幕识别 App，不包含抖音下载入口；抖音下载仍是另一个独立 App。
- 字幕 App 版本提升为 `versionCode 9` / `1.7-zh-clean-cnmodels`，用于覆盖此前装过的集成测试版。
- Whisper 模型优先从国内 ModelScope 仓库下载；若镜像连接失败、缺文件或下载不完整，会自动续传并回退 Hugging Face 官方源。
- Vosk 模型仍使用上游目录中的官方 alphacephei.com 地址，未找到覆盖完整目录且可校验的统一国内镜像。
- 国内模型源补丁：`patches/subtitle-cn-model-source.patch`。

## 2026-08-25 独立抖音下载助手
- 已用 `https://v.douyin.com/kJY6N2TxtJw/` 验证作品可下载；作品 ID `7677567300928654598`，成品 MP4 为 25:11.67、1080×1920、H.264/AAC。
- 用户不希望开启 Tailscale（会占用 Android VPN 槽并影响 V2Ray），因此已放弃 Windows 后端方案，也删除了计划任务与服务。
- 隐藏 WebView 在两次真机测试中均无法触发视频资源，因此不再把下载器嵌入字幕工坊。
- 当前采用独立 App：显示完整抖音 WebView，提供登录/清除登录入口；捕获视频和音频请求、手机直接下载并用 FFmpeg 合并，保存到 `下载/DouyinDownloader`。
- 同一作品已验证在未登录网页会话下也能取得视频与音频资源。
- Android 构建补丁：`patches/douyin-downloader-app.patch`。
- 下载助手 1.1 增加“导出下载日志”：使用系统文件选择器保存文本日志，记录资源识别、HTTP 下载、FFmpeg 合并及 MediaStore 写入步骤；不记录 Cookie 值或完整媒体签名参数。
- 下载助手 1.2 将视频保存位置从不被部分系统允许的 `Download` 改为 `Movies/DouyinDownloader`；真机错误已证明解析、下载和 FFmpeg 合并成功，旧版仅失败于 MediaStore 最终写入。

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
