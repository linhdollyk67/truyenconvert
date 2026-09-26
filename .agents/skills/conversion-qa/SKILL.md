---
name: conversion-qa
description: >-
  Use this skill to review one or more already-converted chapters against
  their original counterparts, scoring compliance against the 7 conversion
  rules in GEMINI.md, and flagging specific violations chapter by chapter.
  Unlike consistency-checker (which scans the whole converted story for
  cross-chapter drift), this skill compares each converted chapter directly
  against its original source.
---

# Conversion QA Skill

## Context
`story-converter` produces the rewritten chapters, but nothing automatically
verifies each one actually followed all 7 rules in `GEMINI.md`. This skill
does a pairwise, rule-by-rule review of original vs. converted chapter, so
problems are caught chapter by chapter instead of only surfacing much later.

## Workflow

1. **Identify chapter pairs**: for each chapter to review, locate the original
   chapter file (under `original_story/<ten-truyen>/`) and its converted
   counterpart (under `convert_story/<ten-truyen>/`).

2. **For each pair, check against the 7 rules** from `GEMINI.md`:
   - Tên nhân vật: đã đổi tên, đúng theo `mapping.md`?
   - Thân phận: đã đổi, và không quá khác bản gốc?
   - Hoàn cảnh gặp gỡ: đã thích nghi theo danh tính mới nhưng vẫn tương tự bản gốc?
   - Lời thoại: có tương tự tinh thần/nội dung lời thoại gốc, không bị dịch máy móc hay bịa mới hoàn toàn?
   - Ngôi kể: đã đổi đúng theo `mapping.md`?
   - Tình tiết: có giữ đúng các mốc sự kiện chính theo thứ tự gốc (cho phép điều chỉnh chi tiết nhỏ), không thiếu/thừa sự kiện quan trọng?
   - Tính cách nhân vật: có giữ nguyên y hệt bản gốc, không bị lệch?

3. **Mark each rule** ✅ / ⚠️ / ❌ per chapter, with a one-line reason and a short
   quote as evidence when flagging ⚠️/❌.

4. **Produce `qa_report.md`** in the story's output folder
   (`convert_story/<ten-truyen>/qa_report.md`): a per-chapter table
   (rows = chapters, columns = 7 rules) plus a short summary of which chapters
   need rework.

5. **If the user wants fixes applied**: for chapters marked ❌, propose the
   specific correction and re-run `story-converter`'s rewrite step for just
   that chapter — don't bulk re-convert the whole story on the basis of a few
   flagged chapters.
