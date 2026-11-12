# PharmaNews

发布日报时以 `AUTOMATION_APPEND.md` 为准。

不要把提交直接 `git push` 到 `main`。先在当前分支提交并 `git push -u origin HEAD`，再调用 `open_git_pr`（`draft: false`，base 为 `main`），然后 `gh pr merge --merge`。Pages 只从 `main` 发布。直接推 `main` 会让 `open_git_pr` 报 `Could not create or find PR from branch`。
