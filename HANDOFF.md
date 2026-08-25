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

## 用户真实目标（后续优先级）
用户要的不是手动导入视频，而是：
```
粘贴抖音公开视频分享链接
→ 下载/解析视频
→ 提取音频并转写中文
→ 导出 MD / JSON
→ 供 AI 分析
```

## 已选现成组件
采用 GitHub 项目：
https://github.com/jiji262/douyin-downloader

理由：
- MIT 许可证。
- 支持 `v.douyin.com` 短链接、单视频下载、重试、浏览器兜底。
- 支持 REST API 服务器模式（`--serve --serve-port 8000`）。
- 可作为 Android App 的“解析/下载后端”，不要自己维护抖音解析规则。

## 明天在 Windows 电脑上做
1. 克隆 `jiji262/douyin-downloader`。
2. 使用 Docker 或 Python 在 Windows 上运行它的 REST API。
3. 用用户的抖音分享链接验证：链接 → 可下载视频文件。
4. 使用 Tailscale 将电脑上的 API 端口暴露给用户手机。
5. 再改 Android App：新增“粘贴链接”入口，调用这个本地 API；拿到文件后复用现有字幕流程。

## 重要限制
- 不读取或破解抖音 App 的缓存文件。
- 后端需要面对抖音接口变动；优先复用该开源项目并以其更新为准。
