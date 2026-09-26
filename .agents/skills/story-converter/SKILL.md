---
name: story-converter
description: >-
  Use this skill when the user asks to convert, chuyển thể, or chuyển đổi a
  story (truyện) — whether naming a specific story or just saying "convert"/
  "convert truyện"/"chuyển thể truyện" in this project. This skill guides the
  agent to read each story and chapter, apply the conversion rules, and output
  them to `convert_story/`.
---

# Story Converter Skill

This skill provides the step-by-step workflow for converting stories based on specific rules.

## Context
The user will provide an input folder containing multiple stories. Each story has its own subfolder or file containing different chapters.
Your task is to create a new output folder and process each chapter of each story, applying specific creative rewriting rules.

## Trigger Prompts

Recognize any of these (or an obvious paraphrase, in Vietnamese or English) as
a request to start this workflow:
- "Convert truyện `<tên>`" / "Convert giúp tôi truyện `<tên>`"
- "Chuyển thể truyện `<tên>`" / "Chuyển đổi truyện `<tên>`"
- "Bắt đầu convert `<tên>`"
- "Làm bản convert cho `<tên>`"
- "Tiếp tục convert truyện `<tên>`" / "Convert tiếp chương tiếp theo của `<tên>`"
  → this is a **resume**, not a restart: check `convert_story/<tên>/` for
  already-approved chapters and `mapping.md`, and continue from the next
  unconverted chapter instead of starting over.

**Identifying which story**: match `<tên>` against the folder names in
`original_story/` (via `list_dir`). If it matches exactly, proceed. If it's a
close/partial match, confirm with the user which folder they mean before
creating anything. If no story name is given at all, list the folders in
`original_story/` and ask which one to convert.

## Workflow

1. **Identify Folders**: 
   - Default `Input Folder` = `original_story/` and default `Output Folder` =
     `convert_story/` (both at the project root). Only ask the user for
     different paths if these defaults don't apply to the request.
   - Use the `list_dir` tool to scan the input folder and identify all the stories and chapters.
   - **The Input Folder (`original_story/`) is read-only** (per `GEMINI.md`
     rule 8): only ever `view_file`/`list_dir` it. Never write, edit, rename,
     or delete anything inside it — all output goes to the Output Folder
     (`convert_story/`).
   - **Create the story's output folder immediately**, before analysis: for
     each story being converted, create `convert_story/<original folder name>/`
     using the **exact same folder name** as in `original_story/` — identical
     spelling, casing, spacing, and numbering, not translated or altered in
     any way. This is what `story-analyzer`, `mapping-builder`, and every later
     step write into.

2. **Analyze the original story**:
   - Use the `story-analyzer` skill to read the whole story first and produce
     `analysis.md` (characters, identities, relationships, narrative POV,
     meeting circumstances, plot beats, key dialogue). Skip this only for very
     short stories you can fully hold in context in one read.

3. **Establish the Conversion Mapping**:
   - Use the `mapping-builder` skill to turn `analysis.md` into a confirmed,
     persistent `mapping.md` in the output folder (new names, new identities,
     new narrative POV, and any location/organization/term replacements).
   - Do not keep the mapping only in scratchpad — every later step reads
     `mapping.md` as the source of truth.

4. **Convert Chapters One at a Time, With Approval Gates** (per `GEMINI.md` rule 9):
   - Using the output folder already created in step 1 (same name as the
     original), check which chapters already have an approved converted file.
     If some exist (e.g. this is a resumed session), start from the next
     unconverted chapter instead of redoing earlier ones.
   - Process chapters in order. For each chapter:
     a. Read the chapter content using `view_file`.
     b. Rewrite the content following `mapping.md` and the general workspace
        rules (`GEMINI.md`):
        - Change character names and identities according to `mapping.md`.
        - Adapt meeting circumstances to fit the new identities but remain similar to the original.
        - Keep dialogues similar.
        - Change the narrative point of view according to `mapping.md`.
        - **Keep the plot similar (same main beats/order) and character personalities exactly the same.**
     c. Write the converted content to the corresponding new chapter file in the `Output Folder` using `write_to_file`.
     d. **Stop and present the chapter to the user.** Do not read, convert, or
        write the next chapter yet — wait for explicit approval.
     e. **If the user approves as-is**: move on to the next chapter.
     f. **If the user requests changes**:
        - Revise the chapter and rewrite it to the same output file.
        - Check whether the requested change touches a fact `mapping.md` or
          any other chapter already relies on (a name, an identity detail, a
          plot event, something foreshadowed or referenced elsewhere).
        - If it does, update `mapping.md` and every affected chapter — both
          already-converted ones and, when relevant, how you plan to write
          upcoming ones — so the story stays consistent. Tell the user
          exactly which files were touched by the sync.
        - Present the revised chapter again and wait for approval before
          continuing.

5. **Verify**:
   - Ensure all chapters of all stories in the input folder have been processed and approved.
   - Run the `consistency-checker` skill across the full converted story to
     catch cross-chapter drift (leftover original names, contradicting
     identities, inconsistent POV) — this also catches anything a mid-story
     sync in step 4f might have missed.
   - Run the `conversion-qa` skill on a sample (or all) chapters to score
     compliance against the 7 rules in `GEMINI.md` chapter by chapter.
   - Provide the user with a summary of the conversion, highlighting the new
     character mappings and any issues found by the checks above.
