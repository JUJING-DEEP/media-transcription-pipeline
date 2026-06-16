# media-transcription-pipeline

Hermes Agent 音视频转录整理 Skill — 接收音视频/播客/会议录制/飞书妙记/链接，输出「分享式提纯版原文」并交付飞书文档。

## 功能

- 接收素材：视频链接（B站、YouTube）、播客、本地音视频、飞书妙记、会议录制
- 自动判断：优先取字幕/逐字稿，无字幕则 ASR 转录
- 核心交付物：「分享式提纯版原文」— 不压缩、不摘要，忠实保留原意、语气、论证链
- 交付方式：默认创建飞书文档，长文不在聊天窗口刷屏

## 完整流水线

```
视频/音频链接
  ↓
YouTube/B站/播客 → 字幕/逐字稿 或 ASR 转录（faster-whisper）
  ↓
整理稿（分享式提纯版原文）
  ↓
飞书文档交付
  ↓
（扩展）md-wechat：Markdown → 公众号 HTML
```

## 目录结构

```
media-transcription-pipeline/
├── SKILL.md                                             # 核心 Skill 定义
├── references/
│   ├── session-20260616-feishu-youtube-transcription.md  # 实战笔记：飞书授权 + YouTube 提取 + Whisper
│   └── session-20260616-wechat-article-illustration.md  # 实战笔记：配图生成 + 公众号格式
```

## 核心概念：分享式提纯版原文

不是摘要！要求：
- 去掉时间戳、说话人标签、寒暄、口水话、重复、残句
- 忠实保留原意、语气、关键判断、案例、数字、类比、论证链
- 不新增事实、不外推、不替原文做过度解释
- 不写成「核心要点 / 一句话总结 / 启发 / 结论」

## 快速开始

1. 安装 Skill（放入 `~/.hermes/skills/media-transcription-pipeline/`）
2. 配置飞书 API（见 `references/lark-doc-setup.md`）
3. 给 Agent 发一个视频/音频链接，坐等飞书文档

## md-wechat 扩展

飞书文档 → 微信公众号：使用 [md-wechat](https://github.com/lydiasong/md-wechat) 将 Markdown 转为公众号 HTML 格式。
