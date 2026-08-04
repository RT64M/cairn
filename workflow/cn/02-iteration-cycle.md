# 工作流 2：日常迭代

> 场景：项目已有元文件，用户要求新增功能、修 bug、改文档或继续推进。

## 代理进入项目

按 `AGENTS.md` 默认读取顺序：

1. 根目录 `AGENTS.md`
2. `.cairn/NICKNAME.md`（若存在）
3. `.cairn/ARCHITECTURE.md`
4. 所有未归档 `.cairn/fix_<desc>.md`
5. 所有未归档 `.cairn/fix-plan_<desc>.md`
6. `.cairn/HUMAN.md`（若存在）
7. `.cairn/TODO.md`
8. `.cairn/plan.md`

## 执行循环

1. 先处理未归档 `fix_*` 中的代理可执行事项；其次处理未归档 `fix-plan_*` 的 plan 改写闭环（写入 `plan.md`、标注来源、归档 fix-plan）。
2. 确认当前工作在 `TODO.md` 中有对应条目。
3. 若没有，先回到来源层处理：已有计划内工作从 `plan.md` 派生 TODO；缺陷、测试或 review 反馈先创建 / 更新 `fix_<desc>.md` 再映射 TODO；plan 范围新增或修订先走 `fix-plan_<desc>.md`，写入并归档后再从新版 `plan.md` 拆 TODO。用户原始命令本身不得作为 TODO 来源。
4. 根据是否涉及接口、架构、测试、术语决定是否读取补充文件。
5. 实施改动。
6. 验证通过后更新 TODO 状态。
7. 满足归档条件的 TODO 步骤必须移入 `.cairn/archive/`，TODO 原位留面包屑；归档是完成闭环的一部分，不是可选清理。

## 常见分支

- 涉及对外接口签名：同步 `INTERFACE.md`，并在 TODO 写明"已同步 INTERFACE.md"。
- 项目结构、文件职责、信息流或新代码找不到合理归属：先更新 `ARCHITECTURE.md`，再继续。
- 用户要求测试：读取或创建 `.cairn/TEST.md`，按其中方法执行。
- 出现代理无法完成事项，或需要人类决策且会影响方向、范围、架构或验收标准的问题：转 `HUMAN.md`；若阻塞具体执行，先确认该执行项已从 `plan.md` 或 `fix_<desc>.md` 进入 TODO，再把阻塞指向 `HUMAN.md`。
- 用户给出测试反馈或 review：创建语义命名 `fix_<desc>.md`，再落 TODO。
- 用户提出 plan 之外的新功能、新模块，或要求修订既有 plan 条目：创建语义命名 `fix-plan_<desc>.md`，按 [05-plan-revision.md](05-plan-revision.md) 流程走完讨论与最终确认，再写入 plan、标注来源、优先归档 fix-plan，最后拆 TODO。

## 完成标准

不能只说“完成了”。对应 TODO 必须收敛为 `[x]` 或 `[~]`，验证结果或阻塞原因必须写清；若步骤已满足归档条件，必须完成归档并留下面包屑。
