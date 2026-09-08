# 澳洲·越南自由行手册 · 上线说明

单文件网页 `index.html`（零外部依赖、离线可用、公开网址任何人点开即看）。
部署方案：**Cloudflare Workers（免费）直连 GitHub，push 即自动上线**。

> 🖥 = **这一步必须你在电脑上（浏览器里）亲手做**，我没法代你登录授权。
> 无标记的步骤我已经做好，或你可以在任意终端执行。

---

## 一、一次性 setup（大约 10 分钟，做完就不用再管）

### 1 🖥 登录 GitHub 并新建空仓库
- 打开 https://github.com → 用**Google 账号**登录（你 GitHub 是 Google SSO）
- 右上角 `+` → **New repository**
- Repository name 填：`au-trip-handbook`
- 选 **Public**（公开仓库最省事；Private 也支持，不影响 Cloudflare 拉取）
- **不要**勾 "Add a README file"（本地已有文件，勾了会产生冲突）
- 点 **Create repository**
- 建好后会看到一串地址，形如 `https://github.com/<你的用户名>/au-trip-handbook.git`，记下备用

### 2 本地 git 初始化 + 首次推送（我已建好文件，你执行下面几行）

在本项目目录执行：

```bash
git init
git add .
git commit -m "init: 澳洲越南自由行手册"
git branch -M main
git remote add origin https://github.com/<你的用户名>/au-trip-handbook.git
git push -u origin main
```

首次 push 时 GitHub 会要求登录 → **用浏览器完成 Google SSO 授权**即可（这一步也算 🖥）。

> 如果 `git push` 报 403 / 密码错误：GitHub 早已不支持账号密码推送。
> 解决：🖥 GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic) → Generate new token，勾 `repo`，生成的 token 当作密码使用（或用 SSH key）。

### 3 🖥 注册 / 登录 Cloudflare
- 打开 https://dash.cloudflare.com
- 建议**同样用 Google 账号**注册（你 Cloudflare 也是 Google SSO），省一个密码
- 免费计划（Free）就够，不需要绑卡

### 4 🖥 Cloudflare 连接 GitHub，开启自动部署（核心一步）
- Cloudflare 控制台 → 左侧 **Workers & Pages**
- 右上角 **Create**
- 切到 **Pages** 标签（或直接 Workers → **Get started** → 选择导入仓库）
- 点 **Connect to Git** / **Import an existing Git repository**
- 第一次会跳转 GitHub 授权 → 点 **Authorize Cloudflare Workers and Pages** 🖥
- 选中刚才的 `au-trip-handbook` 仓库 → **Begin setup**

### 5 🖥 填写构建配置（照抄，别改）
| 字段 | 填什么 |
|---|---|
| Project name | `au-trip-handbook` |
| Production branch | `main` |
| Framework preset | **None** |
| Build command | **留空**（纯静态，无构建） |
| Build output directory | **留空** 或填 `.` |
| Root path | 留空 |

点 **Save and Deploy**。

### 6 🖥 拿到网址
部署完成后会给出：
`https://au-trip-handbook.<随机串>.workers.dev`（或 `*.pages.dev`）

**这个网址任何人点开就能看，不用登录、不用装 App —— 发给同行伙伴即可。**

---

## 二、日常更新（"我 push 你就上"）

以后改内容只需要：

```bash
git add .
git commit -m "update"
git push
```

Cloudflare 会在几十秒内自动重新上线，**网址不变**。

> 网站的自动重部署由 Cloudflare 完成，不需要我（WorkBuddy）在线或介入。
> 你要我改内容 → 我改 `index.html` → 你 push → 自动上线。

---

## 三、可选的锦上添花（非必需）

### 绑定自己的域名
🖥 Cloudflare → 该项目 → **Custom domains** → 添加域名。
若域名不在 Cloudflare 托管，需先把 DNS 迁到 Cloudflare（在域名注册商处改 NS）。

### 加密码保护（如果想小范围分享）
免费 Workers 静态资源本身没有密码功能。若需要：
1. 改用 Cloudflare **Access**（Zero Trust），可设置邮箱一次性密码 —— 但访客验证会麻烦；
2. 或者更简单：把网址改成一个难猜的项目名，不对外公开即可。

---

## 四、重要提醒

- **网址是公开的**：页面里**绝不出现**确认号、证件号、房间号。当前页面已按此规则编写，请后续补信息时也遵守。
- 行程万一调整，请让我**重排整个 schedule**（以已出票航班时间为锚），不要手工塞时间点。
- 公开网址请勿放护照照片、机票二维码、订单截图。
