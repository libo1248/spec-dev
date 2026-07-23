---
name: spec-dev
description: 规范驱动开发流程（Spec -> Architecture -> Tasks -> TDD Code -> 回归验证）
---

# Spec-Driven Development (SOP)

当用户调用此 Skill 时，你必须**严格按以下 Phase 顺序**执行。不得跨越阶段，遇 Pause 点必须等待用户显式指令。

---

## Phase 1: 需求规格定义 (Requirements)
1. **生成 Spec 存放目录**：根据用户需求描述，在 `docs/specs/` 下创建当前日期加功能名的目录，格式为 `docs/specs/YYYYMMDD-功能名/`（如 `docs/specs/20230815-用户登录/`）。**后续所有 Spec 文件均放在此目录下**。
2. 在该目录下生成或更新 `requirements.md`。
3. 明确描述：User Stories、业务边界条件、非功能需求及 Acceptance Criteria (AC)。
4. **⏸️ 暂停点 (Pause 1)**：向用户汇报：“`requirements.md` 已生成，请确认或提意见。” **等待用户回复“确认”或“继续”后方可进入下一阶段。**

---

## Phase 2: 架构与 API 设计 (Architecture)
1. 基于已确认的 `requirements.md`，在 **同一 Spec 目录**（Phase 1 创建的 `docs/specs/YYYYMMDD-功能名/`）下生成 `design.md`。
2. 包含：模块依赖、数据结构/ Schema、API 契约以及关键逻辑流程图。
3. **⏸️ 暂停点 (Pause 2)**：向用户汇报：“`design.md` 已生成，请确认架构设计。” **等待用户回复“确认”或“继续”后方可进入下一阶段。**

---

## Phase 3: 任务拆解 (Task Decomposition)
1. 在 **同一 Spec 目录** 下生成 `tasks.md`。
2. 将架构拆解为粒度极小、原子化（Atomic）的 Checklist 任务：
   - 格式统一为 `- [ ] Task X: [描述]`。
   - 每个 Task 对应的代码改动量应尽量保持在单次可验证范围内。
3. **⏸️ 暂停点 (Pause 3)**：向用户汇报：“`tasks.md` 已生成，请确认任务拆解。” **等待用户回复“确认”或“继续”后方可进入下一阶段。**
---

## Phase 4: 循环编码与自动化自愈 (Code-Test-Repair Loop)

对 `tasks.md` 中**未勾选的 Task** 进行单线程循环处理。**每次只处理一个 Task**：

### 1. 编码 (Implementation)
- 读取当前 Task 的目标与上下文，按 `design.md` 规范编写/修改代码。

### 2. 测试验证 (Test Verification)
- 运行项目的单元测试命令（如 `npm test` / `pytest` / `go test` / `go build`）。
- **如果测试通过**：
  1. 必须将 `tasks.md` 中当前 Task 标记为已完成 `[x]`，不能只修改内存标记。
  2. 继续处理下一个未完成的 Task。

### 3. 自愈循环 (Repair Loop - 最多尝试 3 次)
- **如果测试失败**：
  - 读取测试失败日志，分析报错原因。
  - 针对报错修复代码，再次运行测试。
  - **计数器控制**：
    - 修复尝试次数 $\le 3$：继续迭代修复。
    - 修复尝试次数 $> 3$：**触发紧急熔断**。停止自动修改，保存当前代码，向用户汇报报错信息与分析结论：“Task X 在 3 次自愈尝试后仍未通过测试，请人工介入干预。”

---

## Phase 5: 最终收尾 (Completion)
1. 所有 Tasks 均打勾 `[x]` 后，运行全量测试套件进行回归验证。
2. 自动更新 `README.md` 或相关变更日志（如适用）。