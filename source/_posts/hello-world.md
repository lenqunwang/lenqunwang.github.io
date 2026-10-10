---
title: 我的第一篇博客
date: 2026-10-10 21:00:00
tags:
  - Hexo
  - 建站
categories:
  - 折腾记录
---

欢迎来到我的博客。这篇文章既是第一篇，也是一份写文章的速查手册。

## 怎么发一篇新文章

在博客目录下打开终端，执行：

```bash
hexo new "文章标题"
```

Hexo 会在 `source/_posts/` 下生成一个 Markdown 文件，用编辑器打开就能写。文件开头被 `---` 包住的那段叫 front-matter，用来声明标题、日期、标签和分类。

## 常用命令

| 命令 | 作用 |
| --- | --- |
| `hexo new "标题"` | 新建文章 |
| `hexo server` | 本地预览，默认地址 http://localhost:4000 |
| `hexo generate` | 把文章生成静态文件到 public/ |
| `hexo clean` | 清空缓存和 public/ |
| `hexo deploy` | 部署到线上 |

## 正文写法

正文就是标准 Markdown。可以写列表：

- 无序列表第一项
- **加粗**、*斜体*、`行内代码`
- [外部链接](https://hexo.io)

也可以引用一段话：

> 写作是把想法从脑子里搬到纸上的过程。

代码块会自动高亮：

```javascript
function hello(name) {
  console.log(`Hello, ${name}!`);
}
hello('Hexo');
```

## 图片放在哪

把图片丢进 `source/images/` 目录，然后在文章里用相对网站根目录的路径引用：

```markdown
![图片说明](/images/图片文件名.png)
```

写完了执行 `hexo server`，浏览器打开 http://localhost:4000 就能看到效果。
