# 博客免费部署方案完全指南

当 Netlify 额度用尽、Vercel 访问不稳定时，这里有多个优质的免费部署替代方案。

## 一、Cloudflare Pages（最推荐）

### 优势
- ✅ 完全免费，无限带宽
- ✅ 全球 CDN，国内访问速度优秀
- ✅ 每月 500 次构建
- ✅ 自动 HTTPS
- ✅ 支持自定义域名

### 访问限制
- 单个文件最大 25MB
- 单次部署最多 20,000 个文件
- 每月 500 次构建（对个人博客完全够用）

### 部署步骤

#### 1. 准备工作
```bash
# 确保代码已推送到 GitHub
git add .
git commit -m "准备部署到 Cloudflare Pages"
git push
```

#### 2. 注册并创建项目
1. 访问 [Cloudflare Pages](https://pages.cloudflare.com/)
2. 使用邮箱注册账号（或直接用 GitHub 登录）
3. 点击 "创建项目" → "连接到 Git"
4. 授权 Cloudflare 访问你的 GitHub 仓库
5. 选择你的博客仓库

#### 3. 配置构建设置
在项目配置页面填写：

| 配置项 | 值 |
|--------|-----|
| 框架预设 | 选择 "VitePress" 或 "None" |
| 构建命令 | `pnpm build` |
| 构建输出目录 | `docs/.vitepress/dist` |
| Node 版本 | 20 |

环境变量（可选）：
```
PNPM_VERSION=8
```

#### 4. 部署
点击 "保存并部署"，等待 2-3 分钟，首次构建完成。

#### 5. 访问网站
- 默认域名：`your-project.pages.dev`
- 可以立即访问

### 配置自定义域名

#### 方法一：域名在 Cloudflare
1. 进入项目设置 → "自定义域"
2. 点击 "设置自定义域"
3. 输入你的域名（如：`blog.yourdomain.com`）
4. Cloudflare 会自动添加 DNS 记录
5. 等待几分钟生效

#### 方法二：域名在其他服务商
1. 在 Cloudflare Pages 添加自定义域名
2. 记录 Cloudflare 提供的 CNAME 记录
3. 去你的域名服务商添加 DNS 记录：
   - 类型：`CNAME`
   - 名称：`@` 或 `blog`
   - 值：`your-project.pages.dev`
4. 等待 DNS 生效（几分钟到 24 小时）

---

## 二、GitHub Pages

### 优势
- ✅ 完全免费
- ✅ 与 GitHub 深度集成
- ✅ 国内访问相对稳定
- ✅ 简单可靠

### 访问限制
- 100GB/月 带宽
- 100GB 存储空间
- 每小时 10 次构建
- 单个文件不超过 100MB
- 仓库大小建议不超过 1GB

### 部署步骤

#### 方法一：使用 GitHub Actions（推荐）

1. **创建部署配置文件**

在项目根目录创建 `.github/workflows/deploy.yml`：

```yaml
name: Deploy VitePress site to Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Setup pnpm
        uses: pnpm/action-setup@v2
        with:
          version: 8

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: pnpm

      - name: Setup Pages
        uses: actions/configure-pages@v4

      - name: Install dependencies
        run: pnpm install

      - name: Build with VitePress
        run: pnpm build

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: docs/.vitepress/dist

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    needs: build
    runs-on: ubuntu-latest
    name: Deploy
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

2. **启用 GitHub Pages**
   - 进入仓库 Settings → Pages
   - Source 选择 "GitHub Actions"
   - 保存

3. **推送代码触发部署**
```bash
git add .github/workflows/deploy.yml
git commit -m "添加 GitHub Actions 部署配置"
git push
```

4. **查看部署状态**
   - 进入仓库的 "Actions" 标签页
   - 查看工作流运行状态
   - 部署成功后，访问 `https://yourusername.github.io/repository-name/`

#### 方法二：使用部署脚本

在 `package.json` 添加脚本：
```json
{
  "scripts": {
    "deploy": "bash deploy.sh"
  }
}
```

你的 `deploy.sh` 已经存在，确保内容类似：
```bash
#!/usr/bin/env sh

set -e
pnpm build

cd docs/.vitepress/dist

git init
git add -A
git commit -m 'deploy'

# 推送到 gh-pages 分支
git push -f git@github.com:yourusername/yourrepo.git master:gh-pages

cd -
```

运行部署：
```bash
bash deploy.sh
```

### 配置自定义域名

1. **在仓库根目录创建 CNAME 文件**（你已有）
```bash
echo 'yourdomain.com' > CNAME
```

2. **在 GitHub 仓库设置**
   - Settings → Pages → Custom domain
   - 输入你的域名
   - 勾选 "Enforce HTTPS"

3. **配置 DNS 记录**

在域名服务商添加：

**使用子域名（blog.yourdomain.com）：**
- 类型：`CNAME`
- 名称：`blog`
- 值：`yourusername.github.io`

**使用根域名（yourdomain.com）：**
- 类型：`A`
- 名称：`@`
- 值：添加以下 4 个 IP：
  ```
  185.199.108.153
  185.199.109.153
  185.199.110.153
  185.199.111.153
  ```

4. 等待 DNS 生效（通常 10 分钟到 1 小时）

---

## 三、Render

### 优势
- ✅ 免费套餐慷慨
- ✅ 自动 SSL
- ✅ 国内访问较稳定
- ✅ 配置简单

### 访问限制
- 100GB/月 带宽（免费套餐）
- 静态网站完全免费
- 自动休眠（静态站点不受影响）

### 部署步骤

1. **注册账号**
   - 访问 [Render](https://render.com/)
   - 使用 GitHub 登录

2. **创建静态网站**
   - Dashboard → New → Static Site
   - 连接 GitHub 仓库
   - 选择你的博客仓库

3. **配置构建设置**
   - Name：自定义项目名称
   - Build Command：`pnpm install && pnpm build`
   - Publish Directory：`docs/.vitepress/dist`

4. **环境变量**（可选）
```
NODE_VERSION=20
PNPM_VERSION=8
```

5. **创建并部署**
   - 点击 "Create Static Site"
   - 等待构建完成（首次约 3-5 分钟）

6. **访问**
   - 默认域名：`your-project.onrender.com`

### 配置自定义域名

1. **在 Render 添加域名**
   - 进入项目 Settings → Custom Domain
   - 点击 "Add Custom Domain"
   - 输入域名（如 `blog.yourdomain.com`）

2. **配置 DNS**
   - Render 会提供 CNAME 记录
   - 去域名服务商添加：
     - 类型：`CNAME`
     - 名称：`blog` 或 `@`
     - 值：Render 提供的地址

3. **等待验证**
   - DNS 生效后，Render 自动配置 SSL
   - 通常 10-30 分钟完成

---

## 四、Surge.sh（命令行部署）

### 优势
- ✅ 极简部署，一条命令
- ✅ 完全免费
- ✅ 适合快速测试

### 访问限制
- 免费版有 Surge 品牌标识
- 自定义域名需付费

### 部署步骤

1. **安装 Surge CLI**
```bash
npm install -g surge
```

2. **构建项目**
```bash
pnpm build
```

3. **部署**
```bash
cd docs/.vitepress/dist
surge
```

首次使用需要：
- 输入邮箱
- 设置密码
- 确认域名（或使用随机域名）

4. **后续部署**
```bash
surge docs/.vitepress/dist
```

5. **访问**
   - 默认域名：`random-name.surge.sh`

### 自定义域名（付费功能）
Surge 免费版不支持自定义域名，需要升级到付费计划。

---

## 五、GitLab Pages

### 优势
- ✅ 完全免费
- ✅ 无限私有仓库
- ✅ 配置灵活

### 访问限制
- 10GB 存储
- 流量无限制
- 每月 400 CI/CD 分钟（免费版）

### 部署步骤

1. **将代码推送到 GitLab**
```bash
git remote add gitlab git@gitlab.com:yourusername/yourrepo.git
git push gitlab main
```

2. **创建 `.gitlab-ci.yml`**

在项目根目录创建：
```yaml
image: node:20

pages:
  cache:
    paths:
      - node_modules/

  before_script:
    - npm install -g pnpm@8
    - pnpm install

  script:
    - pnpm build
    - mv docs/.vitepress/dist public

  artifacts:
    paths:
      - public

  only:
    - main
```

3. **推送配置**
```bash
git add .gitlab-ci.yml
git commit -m "添加 GitLab Pages 配置"
git push gitlab main
```

4. **等待部署**
   - CI/CD → Pipelines 查看构建状态
   - 首次构建约 3-5 分钟

5. **访问**
   - 默认域名：`yourusername.gitlab.io/yourrepo`

### 配置自定义域名

1. **在 GitLab 设置**
   - Settings → Pages → New Domain
   - 输入域名

2. **配置 DNS**
   - 类型：`CNAME`
   - 名称：`blog`
   - 值：`yourusername.gitlab.io`

3. **验证域名**
   - 添加 TXT 记录验证所有权
   - 验证通过后自动启用 HTTPS

---

## 六、Railway

### 优势
- ✅ 现代化界面
- ✅ 支持多种项目类型
- ✅ 自动 SSL

### 访问限制
- 每月 $5 免费额度
- 静态网站消耗极少，足够使用
- 500小时运行时间/月

### 部署步骤

1. **注册账号**
   - 访问 [Railway](https://railway.app/)
   - 使用 GitHub 登录

2. **创建项目**
   - New Project → Deploy from GitHub repo
   - 选择你的博客仓库

3. **配置**
   - Railway 会自动检测 VitePress
   - Build Command：`pnpm install && pnpm build`
   - Start Command：`pnpm serve`（或使用 serve 包）

4. **部署**
   - 自动构建和部署
   - 等待完成

5. **访问**
   - 默认域名：`your-project.up.railway.app`

### 配置自定义域名

1. **添加域名**
   - Settings → Domains → Custom Domain
   - 输入域名

2. **配置 DNS**
   - 添加 Railway 提供的 CNAME 记录

---

## 对比总结

| 平台 | 推荐指数 | 国内访问 | 带宽限制 | 构建次数 | 难度 | 最适合 |
|------|---------|---------|---------|---------|------|--------|
| **Cloudflare Pages** | ⭐⭐⭐⭐⭐ | 优秀 | 无限 | 500次/月 | 简单 | 所有人，首选 |
| **GitHub Pages** | ⭐⭐⭐⭐ | 良好 | 100GB/月 | 10次/时 | 简单 | 开源项目 |
| **Render** | ⭐⭐⭐⭐ | 良好 | 100GB/月 | 无限 | 简单 | 备选方案 |
| **Surge.sh** | ⭐⭐⭐ | 一般 | 无限 | 无限 | 极简 | 快速测试 |
| **GitLab Pages** | ⭐⭐⭐ | 一般 | 无限 | 400分钟/月 | 中等 | GitLab 用户 |
| **Railway** | ⭐⭐⭐ | 良好 | $5额度/月 | 无限 | 简单 | 全栈项目 |

## 推荐方案

### 最佳组合策略

1. **主站点：Cloudflare Pages**
   - 速度快，稳定性好
   - 国内访问优秀

2. **备份站点：GitHub Pages**
   - 可靠性高
   - 作为备份访问

3. **测试环境：Surge.sh**
   - 快速预览
   - 测试新功能

## 快速决策指南

- **追求速度和稳定** → Cloudflare Pages
- **重视可靠性** → GitHub Pages
- **需要快速测试** → Surge.sh
- **使用 GitLab** → GitLab Pages
- **想要现代界面** → Railway

## 常见问题

### 1. 可以同时部署到多个平台吗？
可以！建议同时部署到 Cloudflare Pages（主）和 GitHub Pages（备）。

### 2. 如何选择域名配置？
- **有自己域名**：优先 Cloudflare Pages，配置最简单
- **没有域名**：GitHub Pages 提供的 `.github.io` 域名很专业

### 3. 构建失败怎么办？
检查：
- Node 版本是否匹配（建议 20）
- 构建命令是否正确
- 输出目录路径是否准确
- 查看构建日志排查错误

### 4. 国内访问速度优化
- 使用 Cloudflare Pages（自带 CDN）
- 图片等静态资源使用 CDN
- 启用 HTTPS
- 域名使用 Cloudflare DNS

### 5. 如何实现自动部署？
- Cloudflare Pages：推送到 GitHub 自动部署
- GitHub Pages：使用 GitHub Actions
- 其他平台：都支持 Git 推送自动部署

---

## 总结

从 Netlify 迁移到其他平台很简单，**Cloudflare Pages** 是最佳选择，结合 **GitHub Pages** 作为备份，可以确保你的博客始终在线且访问快速。

选择适合自己的方案，开始部署吧！
