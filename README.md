# 多 agent 工作流（Multi-Agent Workflow）

[English](README.en.md)

給「用 Claude Code、Codex、Gemini 等多個 AI agent 一起做事」的人：一套可以直接讀、也可以讓 agent 照著做的工作流規範。內容來自實際長期使用，整理成與特定工具無關的版本。

## 裡面有什麼

| 路徑 | 內容 | 成熟度 |
| --- | --- | --- |
| `skills/multi-agent-project/` | 寫程式、修 bug：三層分工、先講好完成標準、開工前規格關、派工範圍、交件與收件、異質審查、測試安全。附 `references/`（細節）與 `templates/`（規格、審查規則、審查題目範本） | 實戰使用 |
| `skills/multi-agent-learning/` | 用多個 AI 讀書：整理讀本、邊讀邊問、兩段式回收 | **實驗中**，較少實戰驗證 |
| `skills-en/` | 上面兩個 skill 的英文版（入口檔同樣叫 `SKILL.md`，name 加 `-en`） | 同上 |

## 核心想法（一分鐘版）

- 動手前先講好「怎樣算做完」，中型以上的工作先寫一頁規格、冷審過才開工（硬閘）。
- 同一件工作只派一個人；派工寫明可碰的檔案與時限；看產出（commit、改檔）不看狀態字。
- 同一個方向連續失敗兩次，先寫下共同假設的前提並驗證，再修第三次。
- 寫程式的和審查的不同家；審查只給材料與 diff，不給作者的結論。
- 同一類錯犯兩次，往上升級成強制檢查，不只多寫一行規則。

## 安裝

**Claude Code**：把要用的 skill 資料夾複製到專案的 `.claude/skills/`（或個人的 `~/.claude/skills/`）。繁中用 `skills/` 底下的資料夾，英文用 `skills-en/` 底下的；不要兩種語言同時裝同一個 skill。

**Codex 與其他 agent**：把 `AGENTS.md` 放進專案根目錄，它會指向 `skills/` 裡的規範；也可以直接把 `SKILL.md` 與 `references/` 貼給 agent 當指示。

## 沒有這些工具也能用

文件裡提到的「任務看板」「重工作排隊鎖」「盯下層的監看」只是角色名稱；每處都附一句沒有工具時的做法（例如用 `TASKS.md`、一次只跑一件重工作、定時看一次進度）。

## 授權

MIT，見 `LICENSE`。
