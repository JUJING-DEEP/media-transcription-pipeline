# 各平台字幕 / 音频获取方式详解

---

## YouTube

### 有字幕的视频
```bash
# 需要代理 + Chrome Cookie（国内 IP 必须同时满足）
HTTP_PROXY="http://127.0.0.1:7890" \
  yt-dlp --cookies-from-browser chrome \
  --list-subs "https://youtu.be/VIDEO_ID"
```

### 下载字幕
```bash
HTTP_PROXY="http://127.0.0.1:7890" \
  yt-dlp --cookies-from-browser chrome \
  --write-sub --sub-lang zh-Hans,zh-Hant,en \
  -o "/tmp/yt_subs.%(ext)s" \
  "https://youtu.be/VIDEO_ID"
```

### 下载音频（无字幕时）
```bash
HTTP_PROXY="http://127.0.0.1:7890" \
  yt-dlp -f "bestaudio[ext=m4a]" \
  -o "/tmp/yt_audio.m4a" \
  "https://youtu.be/VIDEO_ID"
```

**阻塞情况：** 无字幕 + 国内 IP → 走 ASR 流程（faster-whisper small 模型）

---

## B 站（Bilibili）

### 提取字幕
```bash
# 需要 cookie（登录态）
yt-dlp --cookies-from-browser chrome \
  --write-sub --sub-lang zh-Hans \
  -o "/tmp/bilibili.%(ext)s" \
  "https://www.bilibili.com/video/BVxxx"
```

### 下载音频
```bash
yt-dlp -f "m4a" \
  -o "/tmp/bilibili_audio.m4a" \
  "https://www.bilibili.com/video/BVxxx"
```

---

## 小宇宙（播客）

### 获取 RSS
```bash
curl -s "https://api.xiaoyuzhoufm.com/v1/episode/byfeed?feed_url=https://RSS_URL&page=1&offset=0&filter=0&type=episode"
```

### 下载音频
```bash
yt-dlp -f "bestaudio" -o "/tmp/podcast.m4a" "音频直链URL"
```

---

## 飞书妙记

直接粘贴文字内容，或通过 `lark-doc` skill 读取飞书文档逐字稿。

---

## 腾讯会议 / Zoom / Teams

- 平台自带导出功能：会议录制 → 导出字幕/逐字稿
- 或下载会议录音，通过 faster-whisper 转录
- Teams 录制文件通常是 M4A，直接 ASR 即可
