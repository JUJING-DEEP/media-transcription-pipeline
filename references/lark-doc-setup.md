# 飞书文档 API 配置与授权

---

## 1. 创建飞书应用

1. 打开 [open.feishu.cn](https://open.feishu.cn) → 开发者后台
2. 点击「创建应用」→ 填写应用名称和描述
3. 进入应用 → 凭证与基础信息 → 拿到 **App ID** 和 **App Secret**

---

## 2. 写入 Hermes 配置

```bash
echo -e "\nFEISHU_APP_ID=cli_你的AppID\nFEISHU_APP_SECRET=你的AppSecret" >> ~/.hermes/.env
```

验证：
```bash
grep FEISHU ~/.hermes/.env
```

---

## 3. OAuth 授权（两步走，避免阻塞）

**第一步：** 立即获取 device_code
```bash
lark-cli auth login --no-wait --recommend --json
```
输出中会有 `verification_url`，复制到浏览器打开并授权。

**第二步：** 授权完成后完成验证
```bash
lark-cli auth login --device-code <device_code>
```
看到「授权成功」即可。

**注意：** 每次重启进程需要重新授权，device_code 有效期 10 分钟。

---

## 4. 常用 lark-cli 命令

```bash
# 创建文档
cat content.md | lark-cli docs +create --title "文档标题" --markdown -

# 列出文档
lark-cli docs list

# 读取文档
lark-cli docs read <doc_id>
```

---

## 5. 飞书文档权限

创建文档后默认是私人文档，需要在飞书后台设置文档权限（分享链接或指定协作者）。
