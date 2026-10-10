# 我的博客（Hexo 7 + Butterfly 主题）

## 三步上手

**博客目录**：`C:\Users\ASUS\Desktop\hexo-blog` —— 就在桌面上，双击 `hexo-blog` 文件夹即可打开。

**怎么执行命令**：打开这个文件夹 → 在窗口顶部的**地址栏**里输入 `cmd` 然后回车 → 弹出的黑窗口已经自动定位在博客目录，直接把下面的命令粘进去回车即可。

| 我想做的事 | 命令 |
| --- | --- |
| 本地看效果 | `hexo server` （浏览器打开 http://localhost:4000 ，按 Ctrl+C 停止） |
| 写新文章 | `hexo new "文章标题"` ，然后到 `source\_posts\` 里编辑生成的 md 文件 |
| 发布上线 | `git add -A && git commit -m "更新" && git push` |

**插图**：把图片拖进 `source\img\` 文件夹，文章里写 `![说明](/img/文件名.png)`。

## 环境

- Node.js：`C:\Users\ASUS\nodejs`（已写入用户级 PATH，新开终端可直接用 `node -v`）
- npm 默认源：`https://registry.npmmirror.com`（国内镜像）
  - 想换回官方源：`npm config set registry https://registry.npmjs.org`
- 当前主题：Butterfly 5.7.0（装在 `node_modules/hexo-theme-butterfly`）

## 日常命令

全部在博客根目录（本文件所在目录）下执行：

| 命令 | 作用 |
| --- | --- |
| `hexo new "文章标题"` | 新建文章，生成 `source/_posts/文章标题.md` |
| `hexo server` | 本地预览，浏览器打开 http://localhost:4000 ，Ctrl+C 停止 |
| `hexo generate` | 生成静态文件到 `public/` |
| `hexo clean` | 清空缓存和 `public/`（改了主题配置后建议先执行） |
| `git add -A && git commit -m "更新" && git push` | 推送到 GitHub，自动触发线上构建 |

> 本地预览必须用 `hexo server`。直接双击 `public/index.html` 会因为站点使用以 `/` 开头的绝对路径，导致样式和脚本全部加载失败。

## 目录说明

| 路径 | 说明 |
| --- | --- |
| `_config.yml` | 站点配置：标题、副标题、作者、语言、网址、当前主题等 |
| `_config.butterfly.yml` | **主题配置**：菜单、代码块、灯箱、搜索、CDN 等（日常主要改这个） |
| `source/_posts/` | 文章，一篇一个 Markdown 文件 |
| `source/img/` | 站内图片，文章里用 `/img/xxx.jpg` 引用 |
| `source/about/index.md` | 「关于」页面 |
| `source/tags/index.md` | 「标签」页面（`type: tags` 这行不要删） |
| `source/categories/index.md` | 「分类」页面（`type: categories` 这行不要删） |
| `.github/workflows/pages.yml` | 自动部署到 GitHub Pages 的工作流 |

## 主题配置怎么改

`_config.butterfly.yml` 里**只写要覆盖默认值的项**，其余自动沿用主题默认值（Hexo 会把两份配置做深合并）。

- 全部可配置项 + 默认值：`node_modules/hexo-theme-butterfly/_config.yml`
- 官方中文文档：https://butterfly.js.org/
- 改法：去上面那个文件里找到键名，原样抄到 `_config.butterfly.yml` 里再改值
- 改完执行 `hexo clean && hexo server` 看效果

目前已覆盖的项：导航栏、菜单、代码块、首页副标题、文章目录、深色模式、图片灯箱、图片懒加载、本地搜索、CDN。

## 展示插画

图片统一放在 `source/img/` 文件夹里（拖进去就行），文章里用 `/img/文件名.扩展名` 引用。这套写法已实测通过，单图和 `{% gallery %}` 网格都正常。

两个注意点：

- **文件名请用英文**，例如 `art-01.jpg`。别用中文或空格——中文名在网址里会被转义成长串 `%E5%9B%BE...`，容易出错。
- **单张图片控制在 500KB 以内**。GitHub 单个文件上限 100MB、仓库建议不超过 1GB，插画一多很容易撑爆。可以用 [Squoosh](https://squoosh.app/) 之类的在线工具压缩。

### 1. 单张图片

把图片放进 `source/img/`，文章里直接写：

~~~markdown
![发型参考](/img/hair-01.jpg)
~~~

点击会自动放大（fancybox 灯箱，可左右翻页、可下载原图）。

### 2. 图画网格（相册）

用 Butterfly 内置的 `gallery` 标签，图片会自动排成网格，点击进灯箱：

~~~markdown
{% gallery %}
![插图一](/img/art-01.jpg)
![插图二](/img/art-02.jpg)
![插图三](/img/art-03.jpg)
{% endgallery %}
~~~

想一开始只显示 6 张、点「展开」再看全部，把第一行写成 `{% gallery false,6,6 %}`。

### 3. 相册分组封面

~~~markdown
{% galleryGroup 场景插画 日常与通勤 /albums/daily /img/cover-daily.jpg %}
~~~

### 4. 首页 / 归档页顶部大图（横幅）

在 `_config.butterfly.yml` 里取消注释并改成你的图片：

~~~yaml
index_img: /img/banner.jpg
archive_img: /img/archive.jpg
~~~

> 注意：`source/img/` 里不要使用 `404.jpg`、`error-page.png`、`favicon.ico`、`friend_404.gif`、`butterfly-icon.png` 这几个文件名，主题自带了同名文件。

## 写代码 / 项目笔记

代码块已配好 macOS 窗口样式、语言标签、复制按钮，超过 500px 高度会自动折叠。直接写标准 Markdown 代码块即可：

~~~markdown
```javascript
function hello(name) {
  console.log(`Hello, ${name}!`);
}
```
~~~

另外 Butterfly 还支持 `{% note %}`、`{% tabs %}`、`{% timeline %}`、`{% label %}` 等标签，见官方文档的「标签插件」页面。

## CDN 设置（重要）

主题默认从 **jsdelivr** 加载第三方资源（Font Awesome 图标、fancybox 灯箱、图片懒加载、本地搜索），但这个域名在国内基本连不上，会导致图标全部消失、灯箱和搜索失效。

本博客已在 `_config.butterfly.yml` 里改成 **staticfile（七牛，国内节点）**：

~~~yaml
CDN:
  internal_provider: local
  third_party_provider: custom
  version: true
  custom_format: https://cdn.staticfile.net/${cdnjs_name}/${version}/${min_cdnjs_file}
~~~

如果读者主要在海外，把 `third_party_provider: custom` 改成 `third_party_provider: cdnjs` 即可（此时 `custom_format` 会被忽略）。

### 不要开启这几个特效

`canvas_ribbon`、`canvas_nest`、`click_heart`、`fireworks`、`activate_power_mode`、`sharejs` 依赖 npm 包 `butterfly-extsrc`，而该包在 cdnjs 和 staticfile 上都**不存在**（已实测 404）。在当前 CDN 方案下开启会报错。想用的话，需要自己把这几个 js 下载到 `source/js/`，再在 `CDN.option` 里逐个指向本地文件。

## 换主题 / 换回旧主题

改 `_config.yml` 里的 `theme:` 一行即可：

- `theme: butterfly` —— 当前使用
- `theme: next` —— 换回 NexT（对应配置是 `_config.next.yml`，文件和依赖都还在，可以直接切回）

改完执行 `hexo clean && hexo server`。

## 部署到 GitHub Pages

本博客使用 **GitHub Actions 自动部署**：代码推到 `main`，GitHub 自动构建并发布。
好处是不需要 `hexo-deployer-git` 插件、不需要个人访问令牌（PAT）、源码和产物也不会混在同一个分支里。

### 首次上线（只需做一次）

1. 在 GitHub 上新建一个**公开**仓库，仓库名填 `lenqunwang.github.io`。
   用这个仓库名，站点地址就是 `https://lenqunwang.github.io`（与 `_config.yml` 里的 `url` 一致）。
   若想用别的仓库名（例如 `blog`），站点地址会变成 `https://lenqunwang.github.io/blog/`，
   此时必须把 `_config.yml` 的 `url` 改成 `https://lenqunwang.github.io/blog`，并执行
   `git remote set-url origin https://github.com/lenqunwang/blog.git`。

2. 推送代码（远端本仓库已配好，直接推即可）：

   ~~~bash
   git push -u origin main
   ~~~

3. 仓库 **Settings → Pages**，把 **Source** 改成 **GitHub Actions**。

4. 等 1～2 分钟，访问 https://lenqunwang.github.io 。

### 之后每次发布

~~~bash
git add -A && git commit -m "更新" && git push
~~~

推上去就自动构建发布，不需要 `hexo generate`，也不需要 `hexo deploy`。

### 备选：hexo-deployer-git 经典方式

如果你更喜欢教程里那种 `hexo d` 一键发布，需要额外做这些事（二选一，不要和 Actions 同时用）：

1. 安装插件：`npm install hexo-deployer-git --save`
2. 在 `_config.yml` 里填 `deploy:`（文件里有注释好的模板，取消注释即可），
   注意**源码和产物必须放不同分支**：源码留在 `main`，产物推到 `gh-pages`
3. 把仓库 **Settings → Pages → Source** 改成 `gh-pages` 分支
4. 发布命令：`hexo clean && hexo deploy`

注意事项：HTTPS 推送需要凭据（Windows 凭据管理器里要有你的 PAT）；另外改用这条路之后，
`.github/workflows/pages.yml` 会因为 Pages 已切到分支模式而每次都构建失败，需要把该文件删掉。

### 别忘了改 url

`_config.yml` 里的 `url` 目前是占位的 `https://lenqunwang.github.io`，务必改成你自己的地址，否则分享链接和站点地图会指向错误的地方。

### 如果 git push 卡住或报 Connection was reset

本机直连 GitHub 不稳定。可选方案：

- 给 Git 配代理（端口换成你代理软件的端口）：

  ~~~bash
  git config --global http.proxy http://127.0.0.1:7890
  git config --global https.proxy http://127.0.0.1:7890
  # 取消：git config --global --unset http.proxy
  ~~~

- 或改用 SSH：`git remote set-url origin git@github.com:用户名/仓库名.git`

## 备份

博客源文件（含文章）就是这个 Git 仓库，推送后线上就有完整备份，换电脑时 `git clone` 即可继续写。
