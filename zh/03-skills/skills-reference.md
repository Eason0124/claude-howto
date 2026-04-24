# Skills 完整參考

> 依官方 [Skills 文件](https://code.claude.com/docs/en/skills) 整理。以官方為準。

## Frontmatter 完整欄位

**所有欄位皆可選**(`name` 省略時用目錄名)。

| 欄位 | 說明 |
|------|------|
| `name` | 顯示名稱,預設用目錄名。僅允許小寫字母、數字、連字號,最長 64 字元 |
| `description` | 用途說明,Claude 依此自動觸發。與 `when_to_use` 合計 1,536 字元上限 |
| `when_to_use` | 補充觸發條件 / 觸發詞 / 示例請求(計入同一上限) |
| `argument-hint` | 自動完成提示,如 `[issue-number]` |
| `arguments` | 命名位置引數(`$name` 用),接受空格分隔字串或 YAML list |
| `disable-model-invocation` | `true` 時只有使用者能觸發。預設 `false` |
| `user-invocable` | `false` 時從 `/` 選單隱藏,只有 Claude 能觸發。預設 `true` |
| `allowed-tools` | 執行此 skill 時免除許可提示的工具 |
| `model` | 執行時使用的模型,接受 `/model` 相同的值,或 `inherit` |
| `effort` | `low` / `medium` / `high` / `xhigh` / `max` |
| `context` | `fork` 時在隔離 subagent 執行 |
| `agent` | `context: fork` 時使用哪個 subagent 類型 |
| `hooks` | skill 生命週期 hook(見 [hooks 事件表](../06-hooks/events-reference.md)) |
| `paths` | glob 模式限制自動觸發條件(如 `*.ts,*.tsx`) |
| `shell` | `bash`(預設)或 `powershell`,控制 `` !`...` `` 的 shell |

> ⚠️ 舊教材可能說「`name`、`description` 必填」。官方明確:**兩者都非必填**(但 description 強烈建議)。

## 目錄結構

```
<skill-name>/
├── SKILL.md           # 必需
├── template.md        # 可選
├── examples/
│   └── sample.md
└── scripts/
    └── validate.sh
```

路徑:

- `~/.claude/skills/<name>/SKILL.md` — 個人
- `.claude/skills/<name>/SKILL.md` — 專案
- `<plugin>/skills/<name>/SKILL.md` — 外掛(命名空間為 `plugin-name:skill-name`)
- Managed settings 的 `.claude/skills/` — 企業

## 優先順序(高 → 低)

1. Enterprise / Managed
2. Personal(`~/.claude/skills/`)
3. Project(`.claude/skills/`)
4. Plugin(`<plugin>/skills/`)

同名時高優先勝出;Plugin 用命名空間隔離。

若 `.claude/commands/` 與 skill 同名,**skill 優先**。

## 自動發現

- 監看 `~/.claude/skills/`、`.claude/skills/`、`--add-dir` 下的 `.claude/skills/`
- 支援 monorepo 巢狀(如 `packages/frontend/.claude/skills/`)
- **檔案變更**即時生效
- **新建目錄**需重啟才會被監看

## String Substitution 變數

### 引數

| 變數 | 說明 |
|------|------|
| `$ARGUMENTS` | 全部引數字串。若內文未含 `$ARGUMENTS`,會自動附加 `ARGUMENTS: <value>` |
| `$ARGUMENTS[N]` | 索引(0 為起始)。`$ARGUMENTS[0]` 是第一個 |
| `$N` | `$ARGUMENTS[N]` 的縮寫,如 `$0`、`$1` |
| `$<name>` | 若 `arguments` frontmatter 有定義,按位置映射 |

多字詞引數需引號:`/my-skill "hello world" second` 會讓 `$0` = `hello world`、`$1` = `second`。

### 系統變數

| 變數 | 說明 |
|------|------|
| `${CLAUDE_SESSION_ID}` | 當前會話 ID |
| `${CLAUDE_SKILL_DIR}` | 含 `SKILL.md` 的目錄(plugin skills 指向 plugin 內 subdir,不是 plugin 根目錄) |

> ⚠️ 官方**沒有** `${CLAUDE_PROJECT_DIR}` 或 `${CLAUDE_HOME}` 這類 skill 變數(hooks 才有 `CLAUDE_PROJECT_DIR`)。

## 預授權工具語法

```yaml
---
name: commit
allowed-tools: Bash(git add *) Bash(git commit *) Bash(git status *)
---
```

語法細節:

- 空格分隔的 list
- 也可寫成 YAML array
- 括號內支援 glob(`*`)
- **不改變工具可用性**,只是免去許可提示
- 未列的工具仍可用,但需要使用者許可

## 權限規則搭配 Skill

Settings / managed 可用:

```text
Skill(commit)           # 精確
Skill(review-pr *)      # 前綴 + 引數
Skill(deploy *)         # 拒絕特定 skill
```

## Extended Thinking 觸發

在 skill 內容任何地方寫 **"ultrathink"** 即啟用 extended thinking。

官方原句:
> "To enable extended thinking in a skill, include the word 'ultrathink' anywhere in your skill content."

## Shell 執行控制(預處理)

` !\`<command>\` ` 與 ` ```! ` 區塊在**送給 Claude 前就執行**,輸出取代 placeholder:

```markdown
現在的 git 狀態:
!`git status --short`
```

Claude 收到的是執行後的結果。

管理員若 settings 設 `disableSkillShellExecution: true`,命令被替換為 `[shell command execution disabled by policy]`。Bundled / managed skills 不受此限制。

`shell` frontmatter 欄位可改預設 shell:

```yaml
---
shell: powershell
---
```

## 內容生命週期

- `SKILL.md` 首次呼叫時進入對話為**單一訊息**,保留至會話結束
- Claude 不會再重新讀(除非 compaction 後再次觸發)
- Auto-compaction 時最近 5,000 字元保留;多個 skill 共 25,000 字元預算

> 寫 skill 時要當成**持續指引**,不是一次性步驟。

## 支援檔案

- 從 `SKILL.md` 用 markdown link 引用子檔案
- Claude 按需載入
- 建議 `SKILL.md` 保持 500 行內

## Bundled Skills 清單

Claude Code 內建(都是 prompt-based playbook):

| Skill | 用途 |
|-------|------|
| `/simplify` | 簡化程式碼品質 |
| `/batch` | 批次處理 |
| `/debug` | 除錯工作流 |
| `/loop` | 遞迴執行任務 |
| `/claude-api` | Claude API / Anthropic SDK 輔助 |

> 差異:bundled skill 是給 Claude 的 playbook;內建指令(`/help`、`/compact` 等)執行固定邏輯。

## Skills vs 自訂 Slash Commands

官方原句:
> "Custom commands have been merged into skills. A file at `.claude/commands/deploy.md` and a skill at `.claude/skills/deploy/SKILL.md` both create `/deploy` and work the same way."

- 舊 `.claude/commands/` 仍相容
- Skill 推薦(支援更多欄位、更多檔案、自動載入)
- 同名時 skill 勝

## 相關文件

- [Skills 總覽](README.md)
- [Hooks 事件表](../06-hooks/events-reference.md)
- [官方 Skills 文件](https://code.claude.com/docs/en/skills)
