# wechat-article

> 面向微信公众号文章处理的 Claude Skills 集合，支持从内容获取、处理到发布的全流程 AI 协作。

[![GitHub](https://img.shields.io/badge/GitHub-weiolivia056--ops-blue)](https://github.com/weiolivia056-ops/wechat-article)

## 项目概述

本项目旨在沉淀并分发面向微信公众号文章处理的 Claude Skills。围绕文章的获取、处理与发布，构建完整的工作流支持。

### 技能体系

我们的技能覆盖微信公众号文章处理的核心工作场景：

1. **内容获取** - 从多种来源收集和转换研究资料
2. **内容处理** - 格式转换、媒体处理，为写作做好准备
3. **专业应用** - 文章分析、内容生成等专业技能

### 核心特点

- **全流程覆盖**：从内容获取到处理发布的完整工作流
- **独立自包含**：每个技能都是完整的模块，可单独使用或组合使用
- **文档完善**：每个技能配备决策记录、任务跟踪、变更日志
- **跨平台支持**：全面支持 Windows、macOS 和 Linux

## 技能列表

| 技能 | 说明 | 许可证 |
|------|------|--------|
| *待添加* | — | — |

> 技能开发中，欢迎贡献！

## 协作规范

本项目遵循 [AGENTS.md](AGENTS.md) 定义的协作规范：

- **技能导向**：每个技能独立成树，根目录包含 SKILL.md 和配套文档
- **文档即上下文**：关键决策、任务、变更记录在文档中
- **透明变更**：所有修改写入 CHANGELOG.md，遵循版本号规范
- **保留证据**：输出引用可回溯，缺失信息明确标注

## 安装方法

### 方式一：通过 Claude Code Plugin Marketplace（推荐）

在 Claude Code 中使用以下命令安装：

```shell
# 添加插件市场源
/plugin marketplace add weiolivia056-ops/wechat-article

# 安装技能集合
/plugin install wechat-article
```

### 方式二：下载压缩包

下载本项目压缩包，解压后将技能目录复制到 Claude Code 技能目录：

```shell
# macOS / Linux
cp -r wechat-article/skills/* ~/.claude/skills/

# Windows
xcopy wechat-article\skills\* %USERPROFILE%\.claude\skills\ /E /I
```
