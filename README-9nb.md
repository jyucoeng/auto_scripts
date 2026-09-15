# 9NB Auto Check-in（README-9nb）

9NB.DE 论坛每日自动签到，GitHub Actions 定时执行，纯 HTTP 请求无需浏览器。

## 功能

- 签到方式：`fixed` 固定 +5 积分（默认）/ `random` 试试手气
- 自动提取签到前/后积分、连续天数、累计天数与累计积分
- 结果通过 Telegram 推送（HTML 格式，用户名完整显示）
- Cookie 用 Fernet 强加密（AES-128-CBC + HMAC-SHA256）持久化，密钥由用户自供
- 可选 sing-box 代理（支持 vless / vmess / trojan / ss / hy2 / tuic / anytls / socks5）
- 登录态失效自动重新登录并覆盖 Cookie 数据库
- 多账号支持（分号 `;` 分隔）

## 环境变量 / GitHub Secrets

| 环境变量 | 用途 | GitHub Secret 名 |
|----------|------|-----------------|
| `NB_BATCH` | 账号列表，每个账号用分号分隔：`username,password,tg_token,tg_chat;username,password,tg_token,tg_chat` | `NB_BATCH` |
| `NB_COOKIE_ENCODE_KEY` | Cookie 加密密钥 | `NB_COOKIE_ENCODE_KEY` |
| `PROXY_CONTENT` | 代理链接（留空则直连） | `HY2_PROXY_URL` |
| `SOCKS_PORT` | sing-box 本地监听端口（默认 `10808`） | `SOCKS_PORT`（可选） |


## 部署步骤

1. 在 GitHub Actions Secrets 中配置：
   - `NB_BATCH`：`username,password,tg_token,tg_chat`（多账号用 `;` 分隔）
   - `NB_COOKIE_ENCODE_KEY`：见下方密钥生成
   - `HY2_PROXY_URL`：代理链接（可选，跳过则直连；支持 vless/vmess/trojan/ss/hy2/tuic/anytls/socks5）
   - `SOCKS_PORT`：可选，默认 `10808`
   - `PRIVATE_REPO_TOKEN`：检出私有代码仓库的 Token（`repo` 权限）
2. 生成加密密钥：

   ```bash
   python3 -c "import secrets; print(secrets.token_hex(32))"
   ```

   > 密钥只用于加密持久化的登录 Cookie，不会出现在日志中。
   > 更换密钥后缓存的 Cookie 失效，会自动重新登录覆盖。
   > 未配置密钥也可运行：每次重新登录，Cookie 不落盘。

## 代理说明

- 配置 `HY2_PROXY_URL` 后，工作流会下载 sing-box 并在本地启动，所有站点请求走代理，TG 推送保持直连
- 启动失败或代理不可达：自动重试 3 次，仍失败则给所有账号发送「⚠️ 9NB 代理连接失败」并退出
- 代理链接示例（与 loli 项目一致）：
  - vless：`vless://uuid@host:443?security=tls&sni=...`
  - hy2：`hysteria2://pass@host:port?insecure=1`
  - ss：`ss://method:password@host:port`
  - 其余协议同理，`socks5://user:pass@host:port` 支持
