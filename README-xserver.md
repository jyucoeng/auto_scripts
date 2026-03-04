
# auto_xserver_public   自动续期脚本


## 1、你需要在仓库 Settings → Secrets and variables → Actions → Secrets 里新增：

- PRIVATE_REPO_TOKEN （用于读取私库的 token，可共用）

- XSERVER_GAME_EMAIL （XServer GAME 脚本需要的账号）

- XSERVER_GAME_PASSWORD （XServer GAME 脚本需要的密码）

- XSERVER_GAME_TELEGRAM_BOT_TOKEN （可选，用于 Telegram 通知）

- XSERVER_GAME_TELEGRAM_CHAT_ID （可选，用于 Telegram 通知）

- HY2_PROXY_URL （可选，Hysteria2 代理 URL）

```
HY2_PROXY_URL='hysteria2://[auth]@[host]:[port]/?sni=xxx&insecure=1&alpn=h3'
```
### 如果不设置此环境变量，脚本将使用直连模式

- SOCKS_PORT （可选，SOCKS5 端口，默认 51080）

```
SOCKS_PORT='51080'
```
### 仅在设置了 HY2_PROXY_URL 时生效


## 2、开放自动写time.txt的文件权限。
### 这个主要是用于规避github 默认60天的仓库代码没有任何变动就会自动给你发邮件并且自动禁用action的定时任务。

GITHUB_TOKEN 不用你手动创建 —— 只要你的 workflow 在 GitHub Actions 里跑起来，GitHub 会自动为每次运行生成一个临时的 token，在 workflow 里直接用就行。

### 你需要做的通常是 给它权限、以及确认 checkout 会把它用在 push 上。

1) 你需要设置的地方：给它写权限

到仓库：

### Settings → Actions → General → Workflow permissions

选择：

✅ Read and write permissions

然后保存

## 3、关闭网站里的账号敏感通知。
3.1） 打开小日子游戏机网页登录地址进行登录，然后按照下图设置
<img width="1023" height="723" alt="CleanShot 2026-03-04 at 12 54 33" src="https://github.com/user-attachments/assets/a157bfb6-74e6-4f3e-ae26-5a815eb53daf" />


## 4、修改定时任务执行时间。
### main.yml里面修改你的定时任务的执行时间（建议根据你自己下面的到期时间去调整定时。）

<img width="629" height="128" alt="CleanShot 2026-03-04 at 12 56 50" src="https://github.com/user-attachments/assets/cd68b91a-c4bc-4740-9322-baaf194e8e0e" />












