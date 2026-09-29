# 为什么要维护这份清单

本仓库是 [czm15053/write-notes-like-deepseek](https://github.com/czm15053/write-notes-like-deepseek) 的 fork。下面五个文件有意与本体不同，只为这边的用法服务，不打算回推本体；其余文件应当与本体一致（行尾差异除外）。

这份清单唯一会失真的地方是：同步本体时顺手把某个文件整体替换成本体版本，差异就无声地消失了。所以同步完先跑一次下面的比对。

## 现在与本体不同的五个文件

| 文件 | 为什么不一样 |
| --- | --- |
| `README.md` | npm script 示例写作 `archive-agent-note`（与 `package.json` 一致）；本体写 `archive-note`，照抄跑不起来 |
| `references/note-format.md` | 补上那行逐字注释豁免的适用范围、以及「注释是小节替代品、两者不得并存」，并写明中文状态能过门禁但项目统一写英文 |
| `references/verification.md` | 补 `import-dsh-notes` 条目（导入后的自动补封印是静默失败的），改正「不接受占位注释豁免」的旧描述 |
| `scripts/verify-agent-note-format.ts` | 那行逐字注释按行判定、跳过代码块，不用全文 `includes`——否则一篇展示该注释的笔记会被误判成豁免 |
| `scripts/import-dsh-notes.ts` | 补封印改用 `__dirname` 定位脚本、`AGENT_NOTE_ROOT` 钉住本次导入的目标树；根 README 只取中文版，免得被英文版覆盖成自指的切换行 |

## 同步本体的更新

```bash
git remote add upstream https://github.com/czm15053/write-notes-like-deepseek.git   # 只需一次
git fetch upstream && git merge upstream/main
git diff --name-only upstream/main...HEAD
```

最后一条应当只列出上面这五个文件，加上本文件（`.upstream/DECISIONS.md`）。多出来的文件要么是有意的新分歧——补进这张表，要么是同步时被改回去的旧分歧。

## 取舍

- **合并交给 git，不自己写同步脚本**：`git merge` 带完整历史，冲突按 git 的常规方式处理。这里曾有一版按基线做三方合并的脚本（`.upstream/sync.mjs` 加一份分歧账本 JSON），它能挡住「某个文件悄悄偏离本体」，但那点信息 `git diff --name-only upstream/main...HEAD` 已经给了；多一份 JSON 和两百行合并器，只是多一处要维护的东西。
- **不回推本体**：把修正提成 PR 给本体是最彻底的解法，收下之后就不必再维护分叉。没选它是因为这几处只是这边的用法偏好（与 `package.json` 对齐的 script 名、中文单语宿主的读法），替本体承担兼容责任换不来这里的收益。
- **不把修正散在工作区里**：散着改，`git blame` 追不到理由，未提交的修改还会污染每次合并的「本地」一侧。所以它们落在 `codex/local-divergences` 的提交里，这份清单只补一句「为什么」。
