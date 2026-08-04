# fix_<desc>.md 设计

`fix_<desc>.md` 是一批测试、审查或人工反馈的闭环文件。`desc` 必须是语义描述，例如 `fix_audit-batch.md`、`fix_mobile-layout.md`。

## 为什么需要

反馈如果直接塞进 TODO，会和原计划混在一起；如果只留在聊天里，会丢失来源和收敛状态。`fix_<desc>.md` 给每批缺陷反馈一个独立生命周期。它不承接 plan 范围新增或修订；这类内容走 `fix-plan_<desc>.md`。

`fix_<desc>.md` 也是外部 agent 异步介入项目的入口之一。人类可以随时让其他 agent 审查项目方向、实现内容或文档一致性，并由该 agent 创建 `fix_<desc>.md`、`fix-plan_<desc>.md` 或其他按需元文件。主 agent 不需要停工等待这类审查；它在每次进入项目或继续执行前按同步协议检查架构、未归档 fix、fix-plan 和 TODO，就能识别新增变动、调整优先级，并把外部输入纳入连续工作流。

## 创建条件

仅在两类情况下创建：

- 用户要求代码审查、安全审计或计划复核。
- 用户提供具体测试反馈、E2E 失败、人工验收 bug 或外部 agent 报告。

代理不得把临时自查的小问题擅自写入 fix，除非用户明确要求自审。

## 应写内容

- 批次内容、时间、来源、范围。
- 每条反馈的现象、期望、状态。
- 对应 TODO 条目。
- 是否需要转 `HUMAN.md`。
- 归档前对 `plan.md` 的影响。

## 生命周期

1. 创建语义命名的 `fix_<desc>.md`。
2. 把可执行事项落到 `TODO.md`。
3. 代理完成能做的修复。
4. 人工事项转入 `HUMAN.md`。
5. 全部收敛后回写 `plan.md` 的影响。
6. 归档为 `.cairn/archive/fix_desc-YYYYMMDD.md`。

## 与主 agent 连续工作的关系

- 外部 agent 可以在主 agent 工作期间新增或修改 `fix_<desc>.md`，记录审查、测试或方向复核结论。
- 若外部 agent 的反馈实际改变 plan 范围，应进入 `fix-plan_<desc>.md`，而不是塞进 fix。
- 主 agent 每次同步时先读未归档 fix 和 fix-plan，再读 TODO；因此它能发现外部 agent 新增的反馈、计划修订或阻塞项。
- 主 agent 发现新增 fix 后，应先把其中可执行项映射到 TODO，再继续普通 TODO；不需要丢失当前上下文或重启项目流程。
