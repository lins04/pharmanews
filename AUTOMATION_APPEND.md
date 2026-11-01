【托管到 GitHub Pages 导览页】
目标仓库：lins04/pharmanews（已挂为本 Automation 的 Repository）。固定公开地址：https://lins04.github.io/pharmanews/

每次跑完生成日报 HTML 后，必须按下面做（保留原有抓取/摘要逻辑；微信推送仍用已有【交付到微信】段，链接改用 Pages URL）：

1. 确定日期戳（Asia/Shanghai）：`DATE=$(TZ=Asia/Shanghai date +%Y-%m-%d)`；若同日已存在同名文件，改用 `DATE=$(TZ=Asia/Shanghai date +%Y-%m-%d-%H%M)`。
2. 把本轮完整 HTML 写到仓库文件：`updates/${DATE}.html`（相对仓库根）。
3. 更新根目录 `index.html`：在 `<!-- LINKS:START -->` 与 `<!-- LINKS:END -->` 之间，**最上方**插入一条新链接（去掉原先的 empty 占位 li 若还在）：
   `<li><a href="updates/${DATE}.html">${DATE} PharmaNews</a><span class="meta">更新于 ${DATE}（上海时区）</span></li>`
   保留历史条目，不要清空整页；保持现有样式与导览说明文字。
4. `git add index.html updates/${DATE}.html && git commit -m "pharmanews ${DATE}" && git push` 到默认分支（main）。
5. 本轮对外可访问链接一律用：`https://lins04.github.io/pharmanews/updates/${DATE}.html`（导览页：`https://lins04.github.io/pharmanews/`）。微信摘要里的链接也用这个，不要用 ephemeral artifact 链接。
6. 若 push 失败，在 run 总结里写明错误；不要假装已上线。
