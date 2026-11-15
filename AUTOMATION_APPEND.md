【托管到 GitHub Pages 导览页】
目标仓库：lins04/pharmanews（已挂为本 Automation 的 Repository）。固定公开地址：https://lins04.github.io/pharmanews/

这段以仓库里的本文件为准。Automation 提示词里如果还有旧的第 4 步（`git push` 直接到 `main`），以这里的顺序为准，不要再那样做。直接把提交推进 `main` 之后，`open_git_pr` 会失败：分支还没在远端，或者和 `main` 是同一提交，GitHub 无法开 PR。报错原文是 `Could not create or find PR from branch ...`。

每次跑完生成日报 HTML 后，必须按下面做（保留原有抓取/摘要逻辑；微信推送仍用已有【交付到微信】段，链接改用 Pages URL）：

1. 确定日期戳（Asia/Shanghai）：`DATE=$(TZ=Asia/Shanghai date +%Y-%m-%d)`；若同日已存在同名文件，改用 `DATE=$(TZ=Asia/Shanghai date +%Y-%m-%d-%H%M)`。
2. 把本轮完整 HTML 写到仓库文件：`updates/${DATE}.html`（相对仓库根）。
3. 更新根目录 `index.html`：在 `<!-- LINKS:START -->` 与 `<!-- LINKS:END -->` 之间，**最上方**插入一条新链接（去掉原先的 empty 占位 li 若还在）：
   `<li><a href="updates/${DATE}.html">${DATE} PharmaNews</a><span class="meta">更新于 ${DATE}（上海时区）</span></li>`
   保留历史条目，不要清空整页；保持现有样式与导览说明文字。
4. 只在当前分支提交，先不要推 `main`：
   `git add index.html updates/${DATE}.html && git commit -m "pharmanews ${DATE}"`
5. 把当前分支推到远端（不要用 `HEAD:main`）：
   `git push -u origin HEAD`
   记下分支名：`git rev-parse --abbrev-ref HEAD`
6. 立刻调用 `open_git_pr`。`branch` 填第 5 步的分支名，`base_branch` 填 `main`，`draft` 设为 `false`。这一步必须发生在推送当前分支之后、合并之前。
7. PR 打开后，在同一次运行里合并到 `main`，Pages 才会更新：
   `gh pr merge --merge <PR编号或URL>`
   然后用 `gh api repos/lins04/pharmanews/pages/builds` 确认最新构建的 `error` 为 null。状态先是 `building` 时，等它变成 `built` 再写总结。
8. 本轮对外可访问链接一律用：`https://lins04.github.io/pharmanews/updates/${DATE}.html`（导览页：`https://lins04.github.io/pharmanews/`）。微信摘要里的链接也用这个，不要用 ephemeral artifact 链接。只有合并进 `main` 且 Pages 构建成功之后，才能说已上线。
9. 推送、开 PR、合并或 Pages 构建有任何失败，在 run 总结里写明错误原文；不要假装已上线。

【收录重要政策发布】

这段覆盖记忆里 `method.md` 的收录栏。下次运行先把本段写进 `method.md`，以后按这里执行。集采常规中选、仿制药和原料药名单仍然默认不收。

重要政策发布要单列，和药监批准、关键临床读出放在同一优先栏。包括：

- 国家医保药品目录、商业健康保险创新药品目录的印发和正式调整
- 国家医保局、国家药监局、国家卫生健康委发布的、改变支付、准入或临床使用的国家级文件

每次运行打开国家医保局首页 https://www.nhsa.gov.cn/ 。当天没有新的目录或上述国家级文件，记「无新稿」，不降分，也不因此停用该站。

摘要写清发布日期、执行日期、新增和调出数量、一类新药数量、目录总数，并给出官方链接。商保创新药目录另写新增几种、目录内总数，以及基本医保基金不予支付。地方跟进通知和解读软文不单列；只有在含官方文件没有的数字时才用来补数字，并标明是转述。

2026-11-14 发布的 2026 年医保药品目录和商保创新药目录，2026-11-15 那页没有写。下一次日报补上，不要当成已发。官方通知：https://www.nhsa.gov.cn/art/2026/11/14/art_53_23387.html
