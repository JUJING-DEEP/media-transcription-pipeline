---
name: media-transcription-pipeline
description: 音视频转录整理完整工作流 — 接收音频/视频/播客/会议录制/飞书妙记/链接，判断来源，下载/转录，清洗整理，交付飞书文档。核心交付物为「分享式提纯版原文」，忠实保留原意、语气、论证链和重要原话，不压缩成摘要。
trigger: "转录,整理稿,音视频转文字,播客整理,会议记录整理,飞书妙记,字幕提取,ASR,转写"
category: media
---

# 媒体转录整理工作流（media-transcription-pipeline）

接收音频、视频、播客、会议录制、飞书妙记、网页链接或本地媒体文件 → 完成转录 → 清洗 → 整理 → 交付飞书文档。

---

## 素材接收与来源判断

**支持的素材类型：**
- 音频文件（mp3/m4a/wav/ogg，本地或 URL）
- 视频文件（mp4/mov/avi，本地或 URL）
- 播客链接（小宇宙、Apple Podcasts、喜马拉雅等）
- 视频平台链接（B 站、YouTube、抖音等）
- 飞书妙记转录结果
- 会议录制文件（腾讯会议、Zoom、Teams 等）
- 其他可访问的网页媒体

**收到素材后先判断：**
1. 素材来源平台 → 判断是否可直接取字幕或逐字稿
2. 素材是否可公开访问，或需要登录/验证码
3. 是否有官方字幕 / shownotes / 章节信息可用
4. 是否需要下载 + ASR

**优先级（降序）：**
1. 官方字幕 / 飞书妙记逐字稿 → 直取
2. 平台隐藏字幕（如 YouTube/B 站）→ 尝试提取
3. 无字幕 → 下载音频 → ASR 转录
4. 无法下载 → 明确阻塞原因，告知用户需要的权限

---

## 字幕 / 逐字稿获取

| 来源 | 获取方式 |
|------|---------|
| YouTube | `video-content-extraction` skill：yt-dlp 提取字幕 |
| B 站 | `video-content-extraction` skill：提取弹幕 + 字幕 |
| 飞书妙记 | 直接粘贴文本，或通过 `lark-doc` 相关 skill |
| 小宇宙 | 尝试解析 RSS 或直接下载音频 |
| 本地文件 | 检查格式和时长，准备 ASR |

---

## ASR 转录（无字幕时）

**使用工具：**
- Whisper（本地优先）：`whisper <audio_file> --language zh --model medium`
- 或用户配置的 ASR API（需确认已授权）

**音频预处理：**
- 检查格式（mp3/m4a 最通用）
- 检查时长（过长可分段）
- 检查可读性（音频质量是否可用）

**ASR 后处理：**
- 修正常见错词：人名、公司名、产品名、技术术语优先校准
- 去掉明显 ASR 噪音（无意义字符、乱码）
- 不补全残句，忠实保留原始断句

---

## 整理稿标准：分享式提纯版原文

**这是核心交付物，不是摘要。**

### 必须做到：
- ✅ 去掉所有时间戳
- ✅ 去掉说话人标签（如「主讲人：」「听众：」）
- ✅ 去掉寒暄、口水话、重复句、设备噪音
- ✅ 修正常见 ASR 错词（人名/公司名/产品名/技术术语优先）
- ✅ 忠实保留原意、语气、关键判断、案例、数字、类比、论证链
- ✅ 保留重要原话（加引号或加粗）
- ✅ 按少量大主题 + 二级小标题组织

### 禁止：
- ❌ 不新增事实，不外推，不替原文下结论
- ❌ 不压缩成「核心要点 / 一句话总结 / 启发 / 结论」
- ❌ 不写「其他有效观点」「补充说明」等垃圾桶标题
- ❌ 标题不来自真实内容

### 标题规则：
- 标题从真实内容出，具体描述主题
- 避免「概述」「总结」「其他」类标题
- 二级标题不超过 5-7 个，控制颗粒度

### 格式：
- 重要判断、金句、用户强相关内容 **适度加粗**
- 长引用用引号标出
- 数字、案例、年份忠实保留

---

## 交付方式

**长文（超过 2000 字）：默认创建飞书文档交付，不在聊天窗口刷屏。**

短文可直接在聊天窗口输出，但需符合上述格式标准。

**飞书文档创建流程：**
1. 确认用户飞书绑定状态
2. 创建文档（标题 = 素材名称或自动生成）
3. 写入整理稿内容
4. 设置适当权限
5. 交付链接给用户

---

## 扩展交付：飞书文档 → 微信公众号

用户有时需要把整理稿从飞书文档转成公众号可直接发布的格式（带 HTML 样式），而非纯 markdown。

### 已知的 Markdown → 公众号工具

**① md-wechat**（lydiasong/md-wechat）⭐96
GitHub: https://github.com/lydiasong/md-wechat
功能：将 Markdown 内容转换为微信公众号可用的 HTML 格式。单向转换，不生成内容，纯格式转换。

**② wechat-format**（lydiasong/wechat-format）⭐2.4k
GitHub: https://github.com/lydiasong/wechat-format
功能：微信公众号样式生成器，在线编辑器，专注排版美化，不做内容生成。

**③ docs-publish**（woai3c/docs-publish）⭐17
GitHub: https://github.com/woai3c/docs-publish
功能：通用文档发布管道，思路可参考。

### 当前完整流水线

```
视频/音频链接
  ↓
YouTube/B站/播客 → 字幕/逐字稿 或 ASR 转录
  ↓
整理稿（分享式提纯版原文）
  ↓
飞书文档交付  ← 当前 skill 覆盖范围到此
  ↓
md-wechat（future extension）：Markdown → 公众号 HTML
  ↓
公众号后台粘贴发布
```

目前 pipeline 覆盖到飞书文档交付为止。md-wechat 转换是下一步扩展方向，如有需要可研究集成。

### 配图生成完整流程（含审核步骤）

**场景：** 公众号图文教程需要配图时，按以下流程操作：

**Step 1：生成图片**
- 工具：dreamina CLI（见下方命令）
- 默认生成 4 张，16:9 竖屏或横屏看内容需求
- prompt 风格：手绘笔记本涂鸦风 + 浅蓝横格稿纸底 + 简笔火柴人卡通 + 红蓝手绘分割边框 + 童趣简笔图标 + 平涂低饱和色彩

**Step 2：下载到本地**
```bash
mkdir -p ~/Desktop/配图文件夹
curl -sL "<image_url>" -o "~/Desktop/配图文件夹/图1.jpg"
```
- 图片默认下载到桌面一个文件夹，不要直接发压缩包

**Step 3：发微信预览**
- 通过 send_message 发给用户审核
- 一次一张，图后加简短说明（"图1：封面/开头用"）
- 注意：微信发送有频率限制，多张图分开发送，中间间隔几秒

**Step 4：用户确认后才插入文档**
- 用户说"可以"或"写入" → 才插入飞书文档
- 不要在用户审核前写入文档，避免返工

### 配图生成：dreamina CLI 优先于 image_generate API

**dreamina CLI**（`/Users/apple/.local/bin/dreamina`）
- 已登录，积分充足（当前 5808+，maestro 级别）
- 生成命令：`dreamina text2image --prompt "..." --ratio 16:9 --model_version 5.0 --resolution_type 2k --poll 60`
- 每张图消耗 3 积分（2k 分辨率）
- 手绘草稿风 prompt 参考：浅蓝横格稿纸底 + 简笔火柴人 + 红蓝手绘分割边框 + 平涂低饱和色彩 + 童趣简笔图标（金币/存钱罐）

**image_generate API（备用，会员失效）**
- FalClient key 不稳定，偶发 `invalid key credentials`
- 失败时回退 dreamina CLI 即可，无需追踪

---

## 无法完成的阻塞情况

明确告知用户：
1. 阻塞原因（平台限制、无字幕、版权问题、授权缺失等）
2. 所需权限或信息
3. 下一步建议

---

## 支持文件

- `references/asr-error-patterns.md` — 常见 ASR 错词模式与校正参考
- `references/platform-extraction-guide.md` — 各平台字幕/音频获取方式详解
- `references/lark-doc-setup.md` — 飞书文档 API 配置与授权
- `references/session-20260616-feishu-youtube-transcription.md` — 2026-06-16 实战笔记：飞书授权两步走 + YouTube 国内IP提取 + Whisper模型选择 + 公众号格式
- `references/session-20260616-wechat-article-illustration.md` — 公众号图文教程制作：配图生成（dreamina CLI）+ 插入公众号后台方式 + 积分记录
