# auto_zampto 续期 建议每天运行一次


你需要做以下修改：


## 1、设置环境变量，环境变量格式如下：

- PRIVATE_REPO_TOKEN （用于读取私库的 token，可跟其他库共用）

- ZAMPTO_BATCH （zampto 的账号环境变量）

```
ZAMPTO_BATCH=a1@example.com,pass1,218445
a2@example.com,pass2,998877,123456:AAxxxxxx,123456789
```


### 格式为:
 email,password,server_id,tg_bot_token,tg_chat_id


每行一套数据：

1、不发 TG：email,password,server_id

2、发 TG：email,password,server_id,tg_bot_token,tg_chat_id

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

GITHUB_TOKEN 不用你手动创建 —— 只要你的 workflow 在 GitHub Actions 里跑起来，GitHub 会自动为每次运行生成一个临时的 secrets.GITHUB_TOKEN，在 workflow 里直接用就行（你已经在用 ${{ secrets.GITHUB_TOKEN }} 了）。

### 你需要做的通常是 给它权限、以及确认 checkout 会把它用在 push 上。

1) 你不需要创建：它默认就存在,在 workflow 里这样写就能用：
```
env:
  GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

```

这个 secrets.GITHUB_TOKEN 是 GitHub 自动注入的，不会出现在 "Secrets" 列表里让你手动建。


2) 你需要设置的地方：给它写权限

到仓库：

### Settings → Actions → General → Workflow permissions

选择：

✅ Read and write permissions

## 3、修改定时任务执行时间。
### 去zampto-AutoRenew.yml里面修改你的定时任务的执行时间（建议每天运行一次）
