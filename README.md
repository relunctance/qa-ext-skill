# QA Ext Skill

[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Platforms](https://img.shields.io/badge/platforms-hermes-blue.svg)](#)
[![Version](https://img.shields.io/badge/version-1.0.0-green.svg)](SKILL.md)
[![Skills](https://img.shields.io/badge/skills-10-orange.svg)](references/)
[![Category](https://img.shields.io/badge/category-gql--bots-blue.svg)](#)

> QA 角色技能索引仓库 - 智能推荐合适的测试 skill

## 概述

`qa-ext-skill` 是 GQL-BOT 团队 QA 角色的技能索引，用于 `kanban create --skill qa-ext-skill` 时智能路由到合适的测试 skill。

## 快速开始

### 安装

```bash
# 克隆仓库
git clone https://github.com/relunctance/qa-ext-skill.git

# 查看可用 skills
cat SKILL.md
```

### 使用

```bash
# Web 测试
hermes chat -p qa -s webapp-testing -q "测试 contact 区块"

# API 测试
hermes chat -p qa -s api-documenter -q "验证 OpenAPI 文档"

# 性能测试
hermes chat -p qa -s performance-engineer -q "执行负载测试"
```

## 技能地图

| 场景 | 推荐 Skill | 级别 |
|------|-----------|------|
| QA 评审 | bmad-qa | P0 |
| Web 测试 | webapp-testing | P0 |
| 完成前验证 | verification-before-completion | P0 |
| 浏览器自动化 | playwright-cli | P0 |
| 测试自动化 | test-automator | P0 |
| 性能测试 | performance-engineer | P1 |
| API 文档 | api-documenter | P1 |
| 测试运行 | test-runner | P1 |
| API Mock | api-mock | P2 |
| 变更测试 | run-tests-after-changes | P2 |

## 工作流

```
接到测试任务
  │
  ├─→ bmad-qa → 执行 QA 评审规范
  ├─→ verification-before-completion → 完成前验证
  ├─→ webapp-testing → Web 功能测试
  │     └─→ playwright-cli → 快速调试
  ├─→ test-automator → 测试自动化
  └─→ performance-engineer → 性能测试
```

## 目录结构

```
qa-ext-skill/
├── SKILL.md              # 技能索引
├── README.md             # 本文件
├── LICENSE               # MIT License
├── update_readme.md      # 升级方案
├── learns/               # 踩坑记录
│   └── README.md
└── references/           # Skill 文件副本
    ├── bmad-qa.md
    ├── webapp-testing.md
    └── ...
```

## 升级

详见 [update_readme.md](update_readme.md)

## 作者

- Author: relunctance
- Organization: GQL-BOT Team
