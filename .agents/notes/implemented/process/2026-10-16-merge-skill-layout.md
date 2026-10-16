# Agent Note: 合入 Skill 目录迁移并保留本地修复

Status: implemented

## Problem

主分支将可安装资源迁入 `skills/write-notes-like-deepseek/`，本地分支对旧位置的校验、导入和说明有独立修复。直接覆盖任何一侧都会丢失修复或保留失效路径。

## Decision

使用 Git merge 合入主分支，沿用新的目录结构。校验器保留逐行识别历史豁免注释并跳过代码块的行为；导入器以自身目录定位迁移后的封印脚本，并保留 `AGENT_NOTE_ROOT` 和中文 README 选择。README 保留 `archive-agent-note` 命名，同时使用新的安装路径。同步 `.upstream/DECISIONS.md` 中受迁移影响的路径。

历史检索：本地没有 `.agents/notes/`；`.upstream/DECISIONS.md` 持有分歧原因，与本次部分重叠，原地更新路径和事实，不改变既有决定。

## Alternatives considered

- 完全采用主分支冲突版本：能保持与主分支一致、减少维护分歧，但会丢掉已经提交的校验和导入修复，不符合本分支维护目的。
- 完全采用本地冲突版本：保留修复最直接，但导入器还会指向旧脚本位置，README 也不符合安装后的目录结构。

## Verification

- 四处合并冲突全部解决，校验器的逻辑修复随文件重命名保留。
- `npm run verify-notes` 通过。
- 原安装测试在 Windows / Node.js 22.19.0 下直接执行时报 `spawnSync npx ENOENT`；临时副本仅将 npx 启动替换为 Node 执行 npm 自带的 `npx-cli.js`。链接和复制两种安装均通过 Codex / Claude 路径、宿主校验、看板和归档检查，仓库原测试不变。
- 六个格式回归场景通过：代码块示例可与备选小节共存，代码块和行内示例不能豁免，历史逐行注释可在截止日期前豁免，截止日期当天不能豁免，注释与备选小节不能共存。
- 从其他工作目录向带空格的目标路径导入通过：中文 README 和标准化链接正确；实际 manifest 哈希与归档文件一致，未在外部 cwd 或继承的错误 `AGENT_NOTE_ROOT` 下写封印。

## Consequences

- 收益：主分支的安装目录边界与本地五处独立修复同时保留；维护清单与说明中的路径符合迁移后的实际位置。
- 代价：仍需维护这五处与本体的分歧，后续迁移需要同步导入器路径。导入器继续静默吞掉补封印失败，导入后仍需检查封印。上游安装测试的 Windows 启动限制保留，后续若要在 Windows CI 直接运行，应单独修复其进程启动方式。
