---
name: handoff
description: 将当前对话压缩为交接文档，供另一个 agent 接续工作。 (Compact the current conversation into a handoff document for another agent to pick up.)
argument-hint: "下一个会话将用于什么？ (What will the next session be used for?)"
---

编写一份交接文档，总结当前对话，以便一个新的 agent 可以继续工作。将其保存到 `mktemp -t handoff-XXXXXX.md` 生成的路径（写入前先读取文件）。

> Write a handoff document summarising the current conversation so a fresh agent can continue the work. Save it to a path produced by `mktemp -t handoff-XXXXXX.md` (read the file before you write to it).

建议下一个会话需要使用的技能（如果有的话）。

> Suggest the skills to be used, if any, by the next session.

不要重复已记录在其他工件中的内容（PRD、计划、ADR、issue、提交、diff）。请通过路径或 URL 引用它们。

> Do not duplicate content already captured in other artifacts (PRDs, plans, ADRs, issues, commits, diffs). Reference them by path or URL instead.

如果用户传递了参数，将它们视为对下一个会话重点的描述，并相应地调整文档。

> If the user passed arguments, treat them as a description of what the next session will focus on and tailor the doc accordingly.
