# 我的博客（Hexo + NexT）

## 环境

- Node.js：`C:\Users\ASUS\nodejs`（已写入用户级 PATH，新开终端可直接用 `node -v`）
- npm 默认源：`https://registry.npmmirror.com`（国内镜像）
  - 想换回官方源：`npm config set registry https://registry.npmjs.org`

## 日常命令

全部在博客根目录（本文件所在目录）下执行：

| 命令 | 作用 |
| --- | --- |
| `hexo new "文章标题"` | 新建文章，生成 `source/_posts/文章标题.md` |
| `hexo server` | 本地预览，浏览器打开 http://localhost:4000 ，Ctrl+C 停止 |
| `hexo generate` | 生成静态文件到 `public/` |
| `hexo clean` | 清空缓存和 `public/` |
| `git add -A && git commit -m "更新" && git push` | 推送到 GitHub，自动触发线上构建 |

> 注意：本地预览必须用 `hexo server`。直接双击 `public/index.html` 会因为站点用的是以 `/` 开头的绝对路径，导致样式和脚本加载失败。

## 目录说明

| 路径 | 说明 |
| --- | --- |
| `_config.yml` | 站点配置：标题、副标题、作者、语言、网址、主题等 |
| `_config.next.yml` | 主题配置：配色方案、菜单、目录、搜索等 |
| `source/_posts/` | 文章，一篇一个 Markdown 文件 |
| `source/about/index.md` | 「关于」页面 |
| `source/images/` | 站内图片，文章里用 `/images/xxx.png` 引用 |
| `scaffolds/` | `hexo new` 生成新文件时的模板 |
| `.github/workflows/pages.yml` | 自动部署到 GitHub Pages 的工作流 |

## 常改的几处

- 站点标题 / 副标题 / 作者 / 简介：`_config.yml` 顶部的 `# Site` 段
- 网址：`_config.yml` 的 `url`，部署到 `https://用户名.github.io` 时必须改成这个
- 主题外观：`_config.next.yml` 里的 `scheme`，可填 `Muse` / `Mist` / `Pisces` / `Gemini`
- 顶部菜单：`_config.next.yml` 里的 `menu`

改完执行 `hexo clean && hexo server` 看效果。

## 部署到 GitHub Pages

1. 在 GitHub 上新建一个**公开**仓库。如果想让站点地址是 `https://用户名.github.io`，仓库名必须写成 `用户名.github.io`；否则站点地址是 `https://用户名.github.io/仓库名/`。
2. 本地关联远端并推送：

   ```bash
   git remote add origin https://github.com/用户名/仓库名.git
   git push -u origin main
   ```

3. 打开仓库的 **Settings → Pages**，把 **Source** 改成 **GitHub Actions**。
4. 推送到 `main` 分支后，Actions 会自动构建并发布，地址在仓库的 Actions 面板或 Pages 设置里可以看到。

### 如果 `git push` 卡住或报 `Connection was reset`

本机直连 GitHub 不稳定（搭建时 `hexo init` 的 GitHub 克隆就被重置过）。可选方案：

- 给 Git 配代理（把端口换成你自己代理软件的端口）：

  ```bash
  git config --global http.proxy http://127.0.0.1:7890
  git config --global https.proxy http://127.0.0.1:7890
  # 取消：git config --global --unset http.proxy
  ```

- 或者改用 SSH：`git remote set-url origin git@github.com:用户名/仓库名.git`

## 备份

博客源文件（含文章）就是这个 Git 仓库，推送后线上就有完整备份，换电脑时 `git clone` 即可继续写。
