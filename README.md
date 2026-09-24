# GangComment

让多个 AI 会话接力维护代码上下文、设计理由、问题记录和 ADR。

GangComment 解决一个常见问题：AI 在当前会话里知道自己为什么这样写，但换一个会话、模型或维护者后，这些上下文就丢了。它要求 AI 把少量、可核验的思考成果写进代码注释，并在后续修改时重新读取、核对和更新。

## 功能

- 在代码旁记录实现意图、关键上下文、取舍和未验证点。
- 用固定标记记录运行中发现的问题，帮助后续 AI 防止回归。
- 跨会话扫描受影响代码、注释、测试和 ADR，恢复必要背景。
- 对影响多个模块、共享接口或迁移承诺的决定生成 ADR。
- 代码变化后同步检查过期注释、问题编号和 ADR 引用。

## 注释格式

问题使用编号，意图使用花括号。两种标记可以单独使用，也可以组合。

```python
# #1「窗口关闭后，后台回调访问已销毁的界面对象会报错」
# {目标：窗口销毁后丢弃回调；因为界面对象已经不存在；取舍：少显示一次结果，换取不崩溃}
```

```python
# {目标：复用连接；因为每次重建连接开销更大；待验证：服务端断开后是否需要重新建连}
```

JavaScript、Go、Rust 等使用语言自己的注释符号：

```javascript
// #1「旧请求后返回时会覆盖最新搜索结果」{目标：只接收当前请求的结果；因为旧响应可能晚于新响应到达；取舍：丢弃过期结果}
```

`{意图}` 应包含尽量短的思考摘要，例如：

- 想解决什么问题；
- 为什么选择当前方案；
- 关键约束和上下文是什么；
- 放弃了什么替代方案；
- 哪些地方还没有验证。

它保存的是设计思考摘要，不是完整的内部思考流水账，也不能把未经验证的猜测写成事实。

## 跨会话接力

新会话开始改代码时，不假设自己记得上次的讨论：

1. 确定本次涉及的文件、功能或问题范围。
2. 查看工作区状态和当前差异，保护未提交修改。
3. 搜索 `#1「`、`{目标：`、`{意图：`、`@ADR-`、`TODO`、问题链接和相关测试。
4. 读取命中的代码上下文、测试和 ADR，重建当前问题、意图、约束、取舍和未验证点。
5. 把这些线索与当前代码和需求核对，再开始修改。
6. 修改后同步更新受影响的标记、直接引用和测试。

默认只扫描受影响范围。共享接口、跨模块迁移或 ADR 移动/删除时，再扩大搜索范围。

## ADR

当决定影响共享接口、数据格式、存储方案、部署方式、安全边界或迁移承诺，并且重新选择会产生实质成本时，使用 ADR。局部实现细节只需要短注释。

如果项目已有 ADR 规范，优先遵循项目规范。没有规范时使用 `docs/decisions/NNNN-简短名称.md` 和仓库内的模板。

提案阶段使用 `Proposed`，不能因此废止现行决定。只有新决定被接受并明确取代旧决定时，旧记录才标记为 `Superseded`。

## 安装

将整个目录放到 Codex 的技能目录：

```text
%CODEX_HOME%\skills\gang-comment
```

如果没有设置 `CODEX_HOME`，通常是：

```text
C:\Users\<用户名>\.codex\skills\gang-comment
```

安装后，在新会话中使用 `$gang-comment`，或让 Codex 根据任务自动调用该 Skill。

## 边界

GangComment 不要求给每一行代码加标签，也不负责后台自动同步。它只在确实有设计理由、运行时问题、约束或长期决策时留下上下文；普通格式化、机械改名和明显的局部修复不应制造额外文档。

---

# GangComment

GangComment lets multiple AI sessions hand off and maintain code context, design rationale, issue notes, and ADRs.

It addresses a common failure mode: one AI session knows why it chose an implementation, but the context disappears when another session, model, or maintainer continues the work. GangComment asks the AI to write a small, verifiable summary of the useful reasoning next to the code, then re-read and validate it during later changes.

## Features

- Preserve implementation intent, important context, trade-offs, and open questions next to code.
- Track runtime problems with stable numbered markers to prevent regressions.
- Restore context across sessions by scanning affected code, comments, tests, and ADRs.
- Create ADRs for decisions that affect modules, shared interfaces, or migration commitments.
- Keep comments, issue markers, tests, and ADR references aligned after changes.

## Comment syntax

Use a numbered marker for a known problem and braces for implementation intent. They may be used separately or together.

```python
# #1「A callback may touch a destroyed UI object after the window closes」
# {Goal: drop callbacks after destruction; reason: the UI object no longer exists; trade-off: one result may be discarded to avoid a crash}
```

```javascript
// #1「An older response can overwrite the latest search result」{Goal: accept only the current request; reason: stale responses may arrive later; trade-off: discard obsolete results}
```

The `{intent}` summary should stay short and may capture:

- the problem being solved;
- why the chosen approach was used;
- important constraints and context;
- a rejected alternative and its trade-off;
- an assumption that still needs verification.

This is a compact design summary, not a transcript of hidden reasoning. Unverified guesses must be labeled as such.

## Cross-session handoff

When a new session starts changing code, it should not assume that it remembers the previous discussion:

1. Identify the files, feature, or problem in scope.
2. Check the working-tree state and current diff.
3. Search for `#1「`, intent markers, `@ADR-`, `TODO`, issue links, and relevant tests.
4. Read the matching code context, tests, and ADRs.
5. Reconcile those clues with the current implementation and requirements.
6. Update affected markers, direct references, and tests after the change.

Scan the affected scope by default. Expand the search for shared interfaces, cross-module migrations, or moved/deleted ADRs.

## ADRs

Use an ADR when a decision affects a shared interface, data format, storage, deployment, security boundary, or migration commitment and changing it later would have a material cost. Local implementation details normally need only a short comment.

Follow an existing repository convention when one exists. Otherwise use `docs/decisions/NNNN-short-name.md` and the repository template.

Use `Proposed` for an unsettled proposal. Do not invalidate the current decision at proposal time. Mark an old record `Superseded` only after a new decision has been accepted and explicitly replaces it.

## Installation

Copy the complete directory into the Codex skills directory:

```text
%CODEX_HOME%\skills\gang-comment
```

When `CODEX_HOME` is not set, this is usually:

```text
C:\Users\<username>\.codex\skills\gang-comment
```

After installation, invoke it with `$gang-comment` in a new session, or let Codex select it for a matching task.

## Boundaries

GangComment does not require labels on every line and does not run as a background synchronizer. It adds context only when there is a real design rationale, runtime problem, constraint, or durable decision. Routine formatting, mechanical renames, and obvious local fixes should not create extra documentation.
