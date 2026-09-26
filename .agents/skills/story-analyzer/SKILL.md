---
name: story-analyzer
description: >-
  Use this skill FIRST, before converting a story, to analyze the original story
  and produce a structured analysis file (characters, identities, relationships,
  timeline, meeting circumstances, narrative POV, plot beats). Its output feeds
  the mapping-builder skill and the story-converter skill.
---

# Story Analyzer Skill

## Context
Converting a story without first understanding it leads to inconsistent name
mapping, contradicting identities across chapters, and missed plot beats.
This skill reads the whole original story and writes down everything the
conversion needs to stay consistent, before any rewriting starts.

## Workflow

1. **Identify the story**:
   - Default story folder = under `original_story/` at the project root. Only
     ask the user if a different path applies.
   - Use `list_dir` to list all chapters in order.
   - **This folder is read-only** (per `GEMINI.md` rule 8): only ever
     `view_file`/`list_dir` it. Never write, edit, or create any file inside it.

2. **Read the story**:
   - Read all chapters with `view_file`. For very long stories, read in batches
     (e.g. 10-20 chapters at a time) and keep updating the analysis incrementally
     rather than trying to hold everything in context at once.

3. **Extract and record**, into `analysis.md` inside the story's **output**
   folder (`convert_story/<ten-truyen>/analysis.md` — never inside
   `original_story/`):
   - **Characters**: name, role (main/supporting), identity/occupation/background,
     personality traits (these must NOT change during conversion, per `GEMINI.md`).
   - **Relationships**: who is connected to whom and how (family, rivals, love
     interest, boss/employee, etc.).
   - **Narrative POV**: what person the original is told in (1st/3rd), and whose
     viewpoint it follows. The conversion must change this (per `GEMINI.md` rule 7),
     so record it precisely.
   - **Meeting circumstances**: for each major character pairing, briefly describe
     where/how/why they first met and any other pivotal meeting scenes.
   - **Plot beats / timeline**: a chapter-by-chapter or arc-by-arc summary of the
     main story events in order, so the conversion can keep the same beats even
     if small details are adjusted.
   - **Key dialogue lines**: any lines that are especially iconic, revealing of
     personality, or plot-critical, worth preserving the spirit of.

4. **Confirm with the user** before handing off to `mapping-builder`: summarize
   the character list and ask if the analysis looks correct/complete, since
   mistakes here propagate into every converted chapter.
