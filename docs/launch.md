# Launch copy

Ready-to-post text for announcing this skill. Edit freely; keep the honest bits (limits) — they are what
makes the post land with developers.

---

## 中文 · 小红书 / 即刻

> **我给 AI 写了个「交接技能」，换会话不失忆**
>
> 换个聊天窗口，AI 就忘了我们干到哪一步——你是不是也这样？
>
> 我做了个开源技能 handoff，专治这个：
>
> · 说一句「交接一下」，它把当前工作收成一份 40 行的 Markdown：停在哪、下一步做什么、已经定了什么、**试过但走不通的路**、哪些还没验证
> · 新会话里说「接上次」，它先复述三行让你确认，再检查哪些信息已经过期，然后接着干
> · 纯提示词，不装任何东西。Claude Code、Codex、Cursor、任何聊天窗口都能用
> · 密钥不会跟着跑出去，发现可疑就拒绝写文件
>
> GitHub 搜 handoff-skill（作者 biggershun）
>
> \#AI工具 #效率工具 #开源 #提示词 #AI编程

---

## 中文 · 朋友圈 / 群

> 分享一个我写的小工具：交接技能 handoff。
>
> 解决的问题很朴素——会话一长、一换窗口，AI 就忘了进度，得重新讲一遍，有时候还会把已经否掉的方案重新提一遍。
>
> 现在说一句「交接一下」，它写出 40 行左右的交接文件：停在哪、下一步、已定决策、**试过但不行的路**、哪些还没验证。新会话说「接上次」，它先复述确认、再检查过期信息，然后接着干。
>
> 不装任何东西，一份 Markdown，什么模型都能读。地址：github.com/biggershun/handoff-skill

---

## English · Show HN

**Title:** Show HN: Handoff – one Markdown file so a new session continues the work

> Every agent handoff prompt I tried failed the same way: the doc becomes a transcript, the dead ends get
> dropped (so the next agent retries them), "done" is claimed without evidence, and secrets ride along.
>
> This is a ~120-line skill written against those four failures. Three modes: WRITE, RESUME, VERIFY. It
> produces a ≤60-line file with five front-matter keys (`date`, `task`, `status`, `supersedes`,
> `verified`), a required "Dead ends" section, and a "Boundaries" section that separates what is already
> authorized from what always needs a fresh yes.
>
> On resume it must restate goal / stop point / next step, run a staleness check on every artifact, and
> stop when the next step's definition of done holds.
>
> Prompt-only: no scripts, no dependencies, no vendor API. Verified by handing the example to a fresh
> session with no chat history — it restated correctly, ran the staleness check, and caught two real
> defects in my own docs (one of which I then fixed).
>
> Limits: it cannot read a transcript the harness hides from the model (those claims are marked
> `unverified`), it moves references rather than files, and it does not replace context compression.
>
> MIT, English + 简体中文: https://github.com/biggershun/handoff-skill

---

## English · Reddit (r/ClaudeAI, r/LocalLLaMA, r/codingagents)

**Title:** A handoff format that actually survives a session switch (prompt-only, MIT)

> I kept losing the thread between sessions: the model re-asks answered questions, re-tries approaches
> I already rejected, and reports work as done when nothing was checked.
>
> So I wrote the format I wanted:
>
> - five front-matter keys (`date`, `task`, `status`, `supersedes`, `verified`) so handoffs can be
>   ordered and superseded
> - seven required sections, including **Dead ends** (what failed and why) and **Boundaries** (authorized
>   vs. must re-confirm)
> - 60-line ceiling, so it stays readable on a phone
> - on resume: restate, staleness-check every artifact, stop at the definition of done
> - redact known key shapes; refuse to write rather than leak
>
> Prompt-only — drop `SKILL.md` into whatever your agent reads, or paste `AGENTS.md` into your rules
> file. Works with Claude Code, Codex, Cursor, Copilot, Gemini and plain chat.
>
> Repo (MIT, EN + 中文): https://github.com/biggershun/handoff-skill
>
> Happy to hear where the format breaks — especially the staleness check, which is the part I trust least.

---

## Notes

- Post Show HN on a weekday morning US Eastern time; the title must start with "Show HN:".
- Reddit: answer comments for the first two hours, or the post dies.
- Do not claim benchmarks. The only verified claim is the resume test described above.