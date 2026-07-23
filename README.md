# spec-dev

**规范驱动开发** — 一个 Claude Code 技能，实现 Spec → Architecture → Tasks → TDD Code → 回归验证 的完整开发流程。

## 安装

**克隆到 Claude Code 技能目录：**

```bash
# macOS / Linux
git clone https://github.com/libo1248/spec-dev.git \
  ~/.claude/skills/spec-dev

# Windows (PowerShell)
git clone https://github.com/libo1248/spec-dev.git `
  "$env:USERPROFILE\.claude\skills\spec-dev"
```

## 使用

在 Claude Code 中调用：

```
/spec-dev
```

或显式指定技能（路径指向 SKILL.md）：

```
spec-dev: 开始一个新的功能开发
```

## 核心流程（5 个 Phase）

| Phase | 产出 | 关键点 |
|-------|------|--------|
| **1. 需求规格** | `requirements.md` | User Stories、边界条件、非功能需求、AC；**Pause 1 等待确认** |
| **2. 架构设计** | `design.md` | 模块依赖、数据结构、API 契约、流程图；**Pause 2 等待确认** |
| **3. 任务拆解** | `tasks.md` | 原子化 Checklist `- [ ] Task X: ...`；**Pause 3 等待确认** |
| **4. 编码-测试-自愈** | 代码 + 测试通过 | 单线程循环，**每 Task 跑测试**；失败自愈 ≤3 次，超限**熔断上报** |
| **5. 收尾** | 全量回归通过 | 运行全量测试套件、更新变更日志 |

## Spec 目录结构（运行时生成）

运行 `/spec-dev` 后，会在项目的 `docs/specs/` 下按 **日期+功能名** 创建目录：

```
docs/specs/
└── 20260723-用户登录/        # YYYYMMDD-功能名
    ├── requirements.md       # Phase 1：需求规格
    ├── design.md             # Phase 2：架构设计
    └── tasks.md              # Phase 3/4/5：任务清单与执行记录
```

> **约定**：所有 Phase 产出的文件**统一放在此目录**，不分散在 `docs/specs/` 根目录。

## License

MIT License - see [LICENSE](LICENSE) file.