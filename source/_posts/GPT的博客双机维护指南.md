---
title: 博客维护指南
date: 2018-01-05 00:00:00
updated: 2018-01-05 00:00:00
tags:
  - blog
categories:
  - Misc
keywords:
description: 全部由GPT5.1生成
top_img:
cover:
toc:
toc_number:
toc_style_simple:
copyright:
copyright_author:
copyright_author_href:
copyright_url:
copyright_info:
---



我把 **最正确、最稳、不踩坑的双机维护方法** 给你整理一下，让你在两台电脑上随时写作、同步、预览、部署都没有问题。

---

# ✅ 总体结构

你的博客现在结构如下：

* **仓库 A：hexo-blog**
  👉 存放 Hexo 源码、主题配置、文章等
  👉 包含 `themes/butterfly` 的 Git submodule

* **仓库 B：phlzy.github.io**
  👉 GitHub Actions 自动部署结果（静态文件），无需手动维护

你只需要在两台电脑上维护 **hexo-blog** 即可。

---

# 🧱 🔧 两台电脑如何初始化 hexo-blog（含 submodule）？

## ① 克隆 hexo-blog 仓库（重点：带 submodule）

不要直接 clone，需要加参数：

```bash
git clone --recursive https://github.com/phlzy/hexo-blog.git
cd hexo-blog
```

如果你忘记加 `--recursive`，也没关系，用下面的补救命令：

```bash
git submodule update --init --recursive
```

这一步会把 Butterfly 主题的文件完整拉下来。

---

# 🧱 🔧 ② 安装依赖（两台电脑都需要执行一次）

```bash
npm install
```

Hexo、主题所需 renderer 插件都会自动安装。

---

# 🧱 🔧 ③ 本地预览

```bash
hexo clean
hexo g
hexo s
```

访问：

```
http://localhost:4000/
```

---

# 🚀 双机写作同步流程（每天都用到）

## ⭐ 在任意电脑开始工作前必须做：

```bash
git pull --recurse-submodules
```

（`--recurse-submodules` 确保主题也同步）

⚠ 如果你不写这个参数，有时候 submodule 不会自动更新。

---

## ⭐ 写完文章后推送：

```bash
git add .
git commit -m "Add new post"
git push
```

GitHub Actions 会自动部署到 `phlzy.github.io`

---

# 🧠 如何确认 submodule 状态正常？

每次切换电脑后，可以检查 submodule 指向的 commit 是否正确：

```bash
git submodule status
```

如果正确，会显示主题仓库的 commit hash。

---

# 🧱 💡 你要特别注意的 **submodule 不要直接修改主题文件**！

如果你编辑了 `themes/butterfly` 里的文件（例如 CSS、布局），Git 会认为：

* “这个子模块被改动了”
* 你必须提交 submodule 的新指针（不推荐）

除非你非常熟悉 submodule 的机制，否则不要动主题代码。

### ✔ 如果你想修改样式，推荐使用：

```
source/_data/styles.styl
source/_data/override.styl
```

我可以告诉你如何正确添加额外 CSS。

---

# 🔧 如果你确实改了 submodule（例如主题配置文件）

正常修改 `themes/butterfly/_config.yml` 是完全可以的，**它不属于 submodule 管理文件**，不会造成仓库指针变化。

只有主题目录中包含在 submodule 仓库中的文件才会导致“submodule pointer changed”。

---

# 🎯 避免 submodule 冲突的小技巧

在任何电脑上：

```bash
git pull --recurse-submodules
git submodule update --remote
```

后者可以让主题自动跟随上游更新（如果你愿意自动更新）。