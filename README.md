# GangComment

AI 一换会话就集体失忆，跟嫖完提裤子翻脸不认人一个德行。治不了？能治。

这 skill 就是逼着傻逼人机把"当时为啥这么写"直接撂在注释里，别让下个接盘侠对着代码跟对着前任朋友圈一样瞎猜。

## 怎么写

有毛病挂编号，有想法塞花括号：

```python
# #1「窗口关了回调还去摸已经销毁的对象,当场暴毙」
# {目标:窗口一销毁就把回调扔了;因为对象都没了;取舍:少显示一次结果,换个不崩}
```

```javascript
// #1「旧请求回来把新的搜索结果盖了」{目标:只认当前请求;因为旧的响应可能更晚到}
```

`{…}` 里说人话就行：想干啥、为啥这么干、扔了啥、哪儿还没验。

**别吹牛逼。** 没验过就写"已验证"，那叫嫖完不认账——傻逼人机吹出去的牛，下个人真敢信，炸了算他妈谁的？

## 改旧注释,先掂量掂量自己几斤几两

新注释随便浪，反正是傻逼人机自己下的种。**动别人写的？那叫接别人的盘**——改炸了没后悔药吃，而且没有任何测试会跳出来替死人喊冤。

| 级别 | 啥情况 | AI 咋办 |
| --- | --- | --- |
| **L1** | 补本次想法、挂新问题编号、修错字死链、追加带日期的证据 | 直接办,完事吱一声 |
| **L2** | 改已有结论、待验证升已验证、删看着可疑的旧注释 | 能办,但原文先抄下来存档,汇报点名让你过目 |
| **L3** | 人写的理由、动老编号、安全/合规/许可证、历史事实、ADR 状态、公共 API 契约 | 只递方案,改前改后摆桌面上,你点头才动 |

三条铁律:

1. **心里没底就杨伟一点,往上一级爬。** 该硬的时候硬不起来,认怂总比炸线强。
2. **没标记的旧注释,默认是人写的,按 L3 办。** 看标记(`{…}` 或 `#N「」`),别听它嘴上怎么说。
3. **安全和人写的,照样被艹死。** 哪怕这条是傻逼人机自己五分钟前刚写的,也不例外。

最容易栽的坑:**"帮我改个错别字" ≠ 授权你动人家手笔。**

活儿是活儿,授权是授权。就像"陪我喝个茶"不等于"跟我开房",别他妈自作多情顺手推倒。L3 的东西,你说十遍"给我改了",傻逼人机也只递方案——除非你明说"这条是人写的,老子让你动"。

*为啥这么事儿逼:* 多问一句死不了人,把人家的理由悄悄抹了,那才叫缺大德。

细则看 [comment-update-policy.md](references/comment-update-policy.md),汇报长啥样看 [update-report-template.md](references/update-report-template.md)。

## 换会话咋接上

1. 先瞅一眼工作区,别人没提交的改动别给人盖了,那跟嫖完还顺人钱包没区别。
2. 搜标记:`#N「`、`{…}`、`@ADR-`、`TODO`。
3. 把命中的代码、测试、ADR 都读一遍。
4. 拿线索跟现在的代码和需求对一遍——**旧注释别无脑信**,前任说的话也得验证——然后再动手。

## ADR

只有那种"动一次脱层皮"的决定才配写 ADR:共享接口、数据格式、存储、部署、安全边界、迁移承诺。局部小活儿一条短注释打发就完了,别整那些虚的仪式感。

项目自己有规矩就听项目的。没有就用 `docs/decisions/NNNN-短名字.md`。

还在吵就写 `Proposed`,别顺手把现行记录毙了;只有新决定真被接受、还明说取代旧的,旧的才标 `Superseded`。

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

以后 `git pull` 更新,跟按时交嫖资一样,别欠账。

## 它不干的事

- 不给每行代码贴标签,那玩意儿跟全身纹满杀马特一样多余。
- 不后台自动同步,只管当前这一摊活儿。
- 格式化、机械改名、明摆着的局部小修,别额外造文档。

分级也不是让傻逼人机躺平摆烂。注释长期跟代码对不上,比有理有据改一条更烂。分级只定一件事:**谁点头,啥才动。**

---

# GangComment

Every damn time a session ends, this AI forgets everything — like a whore skipping town at sunrise. Fixable? You bet your ass.

This skill forces that dumb son-of-a-bitch machine to dump the "why" straight into code comments, so the next poor bastard don't start from zero.

## How to write 'em

Numbers for bugs, braces for intent:

```python
# #1「Callback pokes a destroyed object after the window closes, blows up」
# {Goal: drop callbacks on destroy; reason: the object is gone; trade-off: lose one result, don't crash}
```

```javascript
// #1「An old response clobbers the newest search result」{Goal: accept only the current request; reason: stale responses can land later}
```

Keep `{…}` short and human: what you're doing, why this way, what you dropped, what's still unverified.

**Don't talk shit.** Writing "verified" on something you never tested is a two-dollar hooker swearing she's clean — next feller trusts it and catches hell.

## Editing old comments: know your place

New comments? Go hog wild, they're yours. **Touching someone else's? Whole 'nother rodeo** — no undo button, and no test'll ever holler at you about it.

| Tier | Applies to | What the AI does |
| --- | --- | --- |
| **L1** | Adding this change's intent, a new issue number, typos, dead links, dated evidence | Just do it, holler after |
| **L2** | Changing a conclusion, unverified→verified, deleting a sketchy old comment | Allowed, but archive the original first and flag it for your review |
| **L3** | Human-written rationale, old issue numbers, security/compliance/licensing, historical facts, ADR status, public API contracts | Proposal only, before/after laid out; needs your OK |

Three hard rules:

1. **Unsure? Get impotent and climb a tier.** Better limp than blow the whole damn line.
2. **An unmarked old comment counts as human-written → L3.** Judge by markers (`{…}` or `#N「」`), not by its tone.
3. **Security and human-written text get fucked dead just the same.** Even if that dumb machine wrote it itself five minutes ago.

The easiest one to trip on: **"fix that typo" ain't permission to touch a human-written comment.**

A task is a task. Permission is permission. "Coffee" ain't "spending the night" — don't get no damn ideas. For L3, even "just change it" only gets you a proposal — unless you say "this one's human-written, I'm letting you edit it."

*Why so damn picky:* one extra question don't cost a thing. Quietly erasing someone's reasoning is shooting a man's horse and riding off.

Full rules: [comment-update-policy.md](references/comment-update-policy.md). Report format: [update-report-template.md](references/update-report-template.md).

## Picking up in a new session

1. Check the working tree first — clobbering someone's uncommitted work is rolling a john who already paid.
2. Search markers: `#N「`, `{…}`, `@ADR-`, `TODO`.
3. Read the matching code, tests, and ADRs.
4. Reconcile it all against the current code and requirements — **don't trust old comments blindly**; verify the ex's word too — then start.

## ADRs

Only decisions that cost an arm and a leg to reverse deserve an ADR: shared interfaces, data formats, storage, deployment, security boundaries, migration commitments. Local details get a one-line comment, not a church wedding.

Follow the repo's own convention. Otherwise `docs/decisions/NNNN-short-name.md`.

Still debating? Write `Proposed` — that don't kill the current record. Mark the old one `Superseded` only once a new decision is accepted and explicitly replaces it.

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

Update with `git pull`. Settle your tab regular-like.

## What it won't do

- Label every line — that's a full-body tattoo of pure jack-shit.
- Sync in the background — current task only.
- Generate docs for formatting, renames, or obvious local fixes.

Tiering ain't a license to stop maintaining comments either. Leaving one permanently out of sync is worse than editing it defensibly. Tiering decides one thing: **who confirms what.**
