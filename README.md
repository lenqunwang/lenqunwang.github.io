# 我的博客（Hexo 7 + Butterfly 主题）

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

1. 在 GitHub 上新建一个**公开**仓库。想让站点地址是 `https://用户名.github.io`，仓库名必须写成 `用户名.github.io`；否则地址是 `https://用户名.github.io/仓库名/`。
2. 本地关联远端并推送：

   ~~~bash
   git remote add origin https://github.com/用户名/仓库名.git
   git push -u origin main
   ~~~

3. 仓库 **Settings → Pages**，把 **Source** 改成 **GitHub Actions**。
4. 之后每次推送到 `main`，Actions 会自动构建并发布。

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
