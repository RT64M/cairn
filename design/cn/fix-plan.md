# fix-plan_<desc>.md 设计

`fix-plan_<desc>.md` 是 plan 新增、扩展或修订的闭环文件。它只处理计划本身的变化，不处理已实现内容的缺陷反馈。

## 为什么需要

如果用户提出的新功能、范围调整或既有 plan 条目修订直接写入 `TODO.md`，项目大纲会绕过确认流程；如果直接改 `plan.md`，又会让稳定设计在普通会话中漂移。`fix-plan_<desc>.md` 把 plan 修改拆成一个可确认、可追溯、可快速归档的独立闭环。

## 创建条件

仅在用户明确提出以下事项时创建：

- plan 之外的新功能或新模块。
- 既有 `plan.md` 条目的修改、替换或废除。
- 验收标准、功能范围或项目边界的扩展性调整。

缺陷修复、测试反馈和 review 问题不进入 `fix-plan_<desc>.md`，而是进入 `fix_<desc>.md`。

## 应写内容

- 批次内容、时间、来源。
- 对 `plan.md` 的预期影响范围。
- 用户明确要求的 plan 修改。
- 经用户逐项许可的代理建议。
- 最终确认状态。

## 不写内容

- 已实现内容的 bug 或 review 条目，属于 `fix_<desc>.md`。
- 普通执行步骤，属于 `TODO.md`。
- 接口参数细节，属于 `INTERFACE.md`。
- 实现完成后的历史，属于 `.cairn/archive/`。

## 生命周期

1. 创建语义命名的 `fix-plan_<desc>.md`。
2. 与用户讨论，按“用户明确需求优先、代理建议逐项许可”收敛草稿。
3. 用户最终确认后，立即把通过内容写入 `plan.md`。
4. 在 `plan.md` 的新增或修订条目旁标注来源和修改日期，以单一标记记录——只替换、不追加，最多保留初次来源加最近一次修订。
5. 若影响已完成 TODO，先查 `TODO.md`；若已归档，再查对应 `.cairn/archive/done-YYYYMMDD.md`，决定是否重新审核、补充说明或重开 TODO。
6. `plan.md` 更新和来源标注完成后，优先归档为 `.cairn/archive/fix-plan_desc-YYYYMMDD.md`。
7. 从新版 `plan.md` 派生或调整 `TODO.md`，再进入实现与验证。

## 与其他文件的关系

- 与 `fix_<desc>.md`：`fix` 修正实现与 plan 描述的偏差；`fix-plan` 改变 plan 本身。
- 与 `plan.md`：`fix-plan` 是新增、扩展或修订 plan 内容的入口；普通会话不得绕过它直接改 plan。
- 与 `TODO.md`：TODO 从已确认并写入的新版 plan 派生，不替代 fix-plan 的确认和归档。
- 与 `.cairn/archive/`：fix-plan 归档不等待后续实现完成；它只负责关闭 plan 改写闭环。
