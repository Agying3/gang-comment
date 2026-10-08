# GangComment

AI 换个会话就失忆,这事儿没治吗?有。

这个 skill 让 AI 把"当时为啥这么写"直接撂在代码注释里,下一个人接手的时候不用从零猜。

## 怎么写

毛病用编号,想法用花括号:

```python
# #1「窗口关了回调还去摸已经销毁的对象,当场炸」
# {目标:窗口一销毁就把回调扔了;因为对象都没了;取舍:少显示一次结果,换个不崩}
```

```javascript
// #1「旧请求回来把新的搜索结果盖了」{目标:只认当前请求;因为旧的响应可能更晚到}
```

`{…}` 里说人话就行:想干啥、为啥这么干、扔了啥、哪儿还没验。

**别吹牛。** 没验过的,别写成"已验证"。

## 改旧注释,得看级别

新注释随便写,反正是你自己的。**动别人写的,那是另一码事**——改错了没法恢复,而且没有任何测试会跳出来喊你。

| 级别 | 啥情况 | AI 咋办 |
| --- | --- | --- |
| **L1** | 补本次想法、加新问题编号、修错别字和死链、追加带日期的证据 | 直接改,完事吱一声 |
| **L2** | 改已有结论、待验证→已验证、删看着可疑的旧注释 | 能改,但先把原文抄下来存档,汇报时点名让你过一眼 |
| **L3** | 人写的理由、动老编号、安全/合规/许可证、历史事实、ADR 状态、公共 API 契约 | 只给方案,改前改后摆你面前,你点头才动 |

三条铁律:

1. **心里没底就怂一点,往上一级靠。**
2. **没标记的旧注释,默认是人写的,按 L3 办。** 看标记(`{…}` 或 `#N「」`),不看内容。
3. **安全和人写的,一票否决。** 哪怕这条是 AI 自己刚写的,照否。

最容易踩的一脚:**"帮我改个错别字" ≠ 授权你动人家写的注释。**

活儿是活儿,授权是授权。L3 的东西,你说"给我改了",AI 也只递方案——除非你明说"这条是人写的,我让你改"。

*为啥这么较真:* 多问一句费不了几个字,把人家的理由悄悄抹了,那才叫事儿大。

细则看 [comment-update-policy.md](references/comment-update-policy.md),汇报长啥样看 [update-report-template.md](references/update-report-template.md)。

## 换会话咋接上

1. 先瞅一眼工作区,别人没提交的改动别给人盖了。
2. 搜标记:`#N「`、`{…}`、`@ADR-`、`TODO`。
3. 把命中的代码、测试、ADR 读了。
4. 拿这些线索跟现在的代码和需求对一遍——**旧注释别盲信**——然后再动手。

## ADR

只有那种"动一次很贵"的决定才值得写 ADR:共享接口、数据格式、存储、部署、安全边界、迁移承诺。局部实现写条短注释就得了,别整虚的。

项目自己有规矩就听项目的。没有就用 `docs/decisions/NNNN-短名字.md`。

还在讨论就写 `Proposed`,别顺手把现行记录废了;只有新决定被接受、还明说取代旧的,旧的才标 `Superseded`。

## 装

整个目录拷进去:

```text
%CODEX_HOME%\skills\gang-comment               # Codex,一般就是 C:\Users\<用户名>\.codex\skills\
C:\Users\<用户名>\.dsh\skills\gang-comment       # DeepSeek Harness
```

或者干脆:

```text
git clone https://github.com/Agying3/gang-comment.git
```

以后 `git pull` 更新。

## 它不干的事

- 不给每行代码贴标签。
- 不后台自动同步,只管当前这个任务。
- 格式化、机械改名、明摆着的局部修复,别额外造文档。

分级也不是让 AI 撒手不管。注释长期跟代码对不上,比有依据地改一条更烂。分级只管一件事:**谁确认什么。**

---

# GangComment

AI forgets everything when the session ends. Sound familiar?

This skill makes the AI dump the "why" straight into code comments, so the next person doesn't start from zero.

## How to write them

Numbers for bugs, braces for intent:

```python
# #1「Callback pokes a destroyed object after the window closes, blows up」
# {Goal: drop callbacks on destroy; reason: the object is gone; trade-off: lose one result, don't crash}
```

```javascript
// #1「An old response clobbers the newest search result」{Goal: accept only the current request; reason: stale responses can land later}
```

Keep `{…}` short and human: what you're doing, why this way, what you dropped, what's still unverified.

**Don't lie.** Anything unverified must not say "verified".

## Editing old comments is tiered

Write new ones freely — they're yours. **Touching someone else's is a different game** — you can't undo it, and no test will ever scream at you about it.

| Tier | Applies to | What the AI does |
| --- | --- | --- |
| **L1** | Adding this change's intent, a new issue number, typos, dead links, dated evidence | Just do it, mention it after |
| **L2** | Changing a conclusion, unverified→verified, deleting a sketchy old comment | Allowed, but copy the original into an archive first and flag it for your review |
| **L3** | Human-written rationale, old issue numbers, security/compliance/licensing, historical facts, ADR status, public API contracts | Proposal only, before/after laid out; needs your OK |

Three hard rules:

1. **Unsure? Chicken out and go one tier up.**
2. **An unmarked old comment counts as human-written → L3.** Judge by markers (`{…}` or `#N「」`), not by what it says.
3. **Security and human-written text veto everything.** Even if the AI wrote that comment itself this session.

The easiest one to trip on: **"fix that typo" is not permission to touch a human-written comment.**

A task is a task. Permission is permission. For L3, even "just change it" only gets you a proposal — unless you say "this one's human-written, I'm letting you edit it."

*Why so picky:* one extra question costs nothing. Quietly erasing someone's reasoning costs a lot.

Full rules: [comment-update-policy.md](references/comment-update-policy.md). Report format: [update-report-template.md](references/update-report-template.md).

## Picking up in a new session

1. Check the working tree first — don't clobber someone's uncommitted work.
2. Search markers: `#N「`, `{…}`, `@ADR-`, `TODO`.
3. Read the matching code, tests, and ADRs.
4. Reconcile it all against the current code and requirements — **don't trust old comments blindly** — then start.

## ADRs

Only write an ADR for decisions that are expensive to reverse: shared interfaces, data formats, storage, deployment, security boundaries, migration commitments. Local details need a one-line comment, not ceremony.

Follow the repo's own convention. Otherwise `docs/decisions/NNNN-short-name.md`.

Still debating? Write `Proposed` — that doesn't kill the current record. Mark the old one `Superseded` only once a new decision is accepted and explicitly replaces it.

## Install

Copy the whole directory in:

```text
%CODEX_HOME%\skills\gang-comment               # Codex, usually C:\Users\<user>\.codex\skills\
C:\Users\<user>\.dsh\skills\gang-comment         # DeepSeek Harness
```

Or just:

```text
git clone https://github.com/Agying3/gang-comment.git
```

Update with `git pull`.

## What it won't do

- Label every line.
- Sync in the background — current task only.
- Generate docs for formatting, renames, or obvious local fixes.

Tiering isn't a license to stop maintaining comments either. Leaving one permanently out of sync is worse than editing it defensibly. Tiering decides one thing: **who confirms what.**
