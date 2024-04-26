# 4.1 Commit 訊息規範 Commit Message Rules

Git commit 訊息可分為 **摘要** 及 **詳細描述**  
Git commit message can be separated as **summary** and **description**.  

此專案 `main` 分支的 commit 訊息應遵守以下格式：  
Commit messages of the `main` branch of the project should follow the following format:

( `{某些東西}` 代表可替換的內容，其餘為固定格式 )  
( `{something}` represents replaceable content, and the rest is a fixed format )

## 摘要 Summary

摘要應力求簡潔，盡量不要超過50個字元。  
Summaries should be as simple as possible. Great commit summaries should contain fewer than 50 characters.

```
{Type}: {Summary}
```

**類型** 和 **摘要** 的第一個字母必定為大寫。  
**Type** and **Summary** must be Capitalized (The first letter must be UPPERCASE).

以下為允許的類型種類：  
The following are acceptable types:

- **feat**: 新增功能
- **fix**: Bug fixes
- **chore**: 不屬於 fixes 或 feature, 也沒有修改 src 或 test 檔案 (例如: 更新 dependencies)
- **refactor**: 重構程式碼 (不屬於 fixes 或 feature)
- **docs** 更新文件 (例如: README 或其他 markdown 檔案)
- **style**: 不影響程式邏輯的變更，通常為程式風格相關的調整。
- **test**: 修改或新增測試
- **perf**: 改善效能
- **ci**: CI (continuous integration) 相關
- **build**: 與 build system 或 external dependencies 有關的變更
- **revert**: reverts 回先前的 commit

## 詳細描述 Description:

```
{Overall description}

- {list 1}
- {list 2}
......

{Issue keywords}
```

Issue keywords: Read [Using keywords in issues and pull requests - GitHub Docs](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/using-keywords-in-issues-and-pull-requests)  
For example: `Fixed #1`

> ### 關於加入詳細描述的方式 About adding commit description
>
> #### 命令列 CLI
> https://stackoverflow.com/questions/16122234/how-to-commit-a-change-with-both-message-and-description-from-the-command-li
> 
> #### 整合開發環境 IDE
> https://stackoverflow.com/questions/74992148/how-to-write-a-commit-description-in-source-control-vscode
> 
> #### GitHub or GitHub Desktop
> 這類工具提供摘要及詳細描述的欄位，直接輸入即可。
> 
