# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

基于 Hugo + HugoBlox Academic CV 模板的个人学术主页。当前内容为模板 demo 数据，后续会替换为真实个人信息。

## 常用命令

```bash
# 开发服务器（热重载）
pnpm dev

# 构建生产版本（含搜索索引）
pnpm build

# 仅生成搜索索引
pnpm pagefind
```

需要先安装 Hugo extended 版本（v0.156.0）和 Go（用于 Hugo Modules）。

## 技术栈

- **静态生成器**: Hugo 0.156.0 (extended)
- **主题**: HugoBlox Kit (通过 Hugo Modules 引入，见 `config/_default/module.yaml`)
- **样式**: Tailwind CSS v4
- **搜索**: Pagefind (客户端静态搜索)
- **包管理**: pnpm
- **部署**: GitHub Pages（可切换，见 `hugoblox.yaml` 中 `host` 字段）

## 架构要点

### 配置体系
所有 Hugo 配置在 `config/_default/` 下，按职责拆分：
- `hugo.yaml` — Hugo 核心设置（语言、分页、taxonomy、输出格式）
- `params.yaml` — 主题参数（品牌、配色、字体、导航栏、SEO、安全策略）
- `module.yaml` — Hugo Modules 依赖（blox、netlify、slides）
- `menus.yaml` — 导航菜单
- `languages.yaml` — 多语言

### 内容组织
`content/` 下按内容类型分目录，每个条目为 Markdown + YAML front matter：
- `authors/` — 作者资料（页面），对应 `data/authors/me.yaml`（结构化数据）
- `publications/` — 论文（journal-article / conference-paper / preprint）
- `blog/` — 博客文章
- `projects/` — 项目展示
- `events/` — 演讲/活动
- `courses/` — 课程内容

### 首页构建
`content/_index.md` 使用 block 系统组合页面区块（resume-biography-3、markdown、collection、cta-card 等），通过 `sections` 数组声明式配置。

### 自定义扩展
`layouts/` 目录放置自定义 Hugo 模板，会覆盖主题同名文件。当前仅有 `_partials/hooks/head-end/github-button.html`。

### CI/CD
`.github/workflows/` 包含：
- `build.yml` — PR 构建验证
- `deploy.yml` — 主分支自动部署
- `upgrade.yml` — 模块自动升级
- `import-publications.yml` — 论文引用导入
