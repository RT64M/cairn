# 工作流 6：架构、接口、测试与术语

> 场景：需求触发 `.cairn/ARCHITECTURE.md`、`.cairn/INTERFACE.md`、`.cairn/TEST.md`、`.cairn/NICKNAME.md` 的维护。

## 架构调整

触发条件只有两种：

- 项目结构、文件职责、信息流或新代码在现有架构下找不到合理归属。
- 用户明确要求调整架构。

动作：更新 `.cairn/ARCHITECTURE.md`，并在对应 TODO 写明“架构已同步更新”。

## 接口变更

当路径、方法、参数、返回值、错误码、CLI 参数或 SDK 公开方法签名变化时：

1. 同步 `.cairn/INTERFACE.md`。
2. 在当前 TODO 写明“已同步 INTERFACE.md”。
3. 若是破坏性变更，新增“通知调用方”TODO 子项。

## 用户要求测试

当项目复杂且用户明确要求测试：

1. 若没有 `.cairn/TEST.md`，创建它。
2. 记录测试分类、命令、数据准备和环境依赖。
3. 执行测试。
4. 测试发现的问题进入 `fix_<desc>.md`（缺陷反馈），不要写进 `TEST.md`。

## 术语增长

当项目专有术语达到 5 个：

1. 创建 `.cairn/NICKNAME.md`。
2. 从 `AGENTS.md` 或现有文档迁入术语。
3. 代理后续进入项目时先读根目录 `AGENTS.md`，随后读 `.cairn/NICKNAME.md`。
4. 新术语出现时追加；不确定含义时先问用户。
