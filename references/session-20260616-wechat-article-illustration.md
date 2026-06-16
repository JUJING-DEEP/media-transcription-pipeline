# 公众号图文教程制作实战笔记（2026-06-16）

## 背景
用户有一篇关于「打造全自动音视频转录 Agent」的公众号文章需要配图。原始素材：用户发来的公众号文章 + 飞书文档《小D复制部署说明》，需要合并互补写成一篇完整教程。

## 工作流程

### 1. 写稿流程（关键）
- **先写完整草稿** → 发用户审核 → 用户确认后才写入飞书文档
- 本次教训：中间写过一版直接写入，用户要求大幅调整 → 返工
- 正确流程：草稿 → 用户审核 → 修改 → 写入文档 → 生成配图

### 2. 飞书文档写入
- 工具：`lark-cli docs +create --title "标题" --markdown -`
- 积分消耗：0（文字不收费）
- 文档 URL 格式：`https://www.feishu.cn/docx/<doc_id>`

### 3. 配图生成流程
**Step 1：** 用 dreamina CLI 生成（见下方成功 prompt）
**Step 2：** 下载到桌面 `~/Desktop/转录助手配图/`
**Step 3：** 发微信预览（一次一张，有频率限制）
**Step 4：** 用户确认后才插入文档

## 配图生成（dreamina CLI）

**工具路径：** `/Users/apple/.local/bin/dreamina`
**积分查询：** `dreamina user_credit`
**当前积分：** 5796（maestro）
**消耗：** 4张 × 3积分 = 12积分

### 成功 prompt 模板

风格：手绘笔记本涂鸦风，浅蓝横格稿纸底，简笔火柴人卡通速写，红蓝手绘分割边框，童趣简笔图标（金币、油表、存钱罐），平涂低饱和色彩，技术科普信息图，手账备忘草稿质感，线条随性钢笔手绘，无写实光影

**图1-封面/问题引入：**
```
手绘笔记本涂鸦风格，浅蓝横格稿纸底，简笔火柴人卡通速写，红蓝手绘分割边框，童趣简笔图标，a cartoon stick figure person sitting at laptop with question marks above head surrounded by messy video and podcast icons, flat low-saturation watercolor colors, casual pen strokes, light blue lined paper background, journal memo draft texture, no realistic shadows
```

**图2-工作流/流程图：**
```
手绘笔记本涂鸦风格，浅蓝横格稿纸底，简笔火柴人卡通速写，红蓝手绘分割边框，童趣简笔图标，cartoon stick figure pointing at a big flowchart showing: YouTube link → AI Agent → Whisper transcription → Feishu doc, flat low-saturation watercolor colors, casual pen strokes, journal memo draft texture
```

**图3-成果/庆祝：**
```
手绘笔记本涂鸦风格，浅蓝横格稿纸底，简笔火柴人卡通速写，红蓝手绘分割边框，童趣简笔图标，a cute cartoon stick figure celebrating with a big Chinese Feishu document icon in hands, confetti and stars around, flat low-saturation watercolor colors, casual pen strokes, journal memo draft texture
```

**图4-工具墙/清单：**
```
手绘笔记本涂鸦风格，浅蓝横格稿纸底，简笔火柴人卡通速写，红蓝手绘分割边框，童趣简笔图标（金币、油表、存钱罐），工具展示墙：lark-cli图标、yt-dlp图标、whisper图标、ffmpeg图标整齐排列，手绘标注，flat low-saturation watercolor colors, casual pen strokes, journal memo draft texture, doodle style infographic
```

### 生成命令
```bash
dreamina text2image \
  --prompt="<prompt>" \
  --ratio=16:9 \
  --resolution_type=2k \
  --model_version=5.0 \
  --poll=120
```

### 下载命令
```bash
mkdir -p ~/Desktop/转录助手配图
curl -sL "<image_url>" -o "~/Desktop/转录助手配图/图1.jpg"
```

## 关键教训
1. **先审核再写入**：草稿必须先发用户审核，用户确认后才写入飞书文档，避免返工
2. **dreamina CLI 优先**：image_generate API（FalClient）key 不稳定，优先用 dreamina CLI
3. **微信发图有频率限制**：多张图分批发，中间间隔几秒，避免触发 rate limit
4. **配图下载后发微信**：不要直接发压缩包，发单张图片 + 简短说明让用户审核

## 最终成品
- 文档：https://www.feishu.cn/docx/Yj3bdlrjZoOM5MxB5iscCUa8ngf
- 配图：~/Desktop/转录助手配图/（4张）
