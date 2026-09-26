---
name: consistency-checker
description: >-
  Use this skill after converting all (or a batch of) chapters of a story, to
  scan the converted output for consistency issues across chapters: leftover
  original names/terms, contradicting character identities, and inconsistent
  narrative POV.
---

# Consistency Checker Skill

## Context
Long stories are converted chapter by chapter, often across multiple sessions.
It's easy for a leftover original name to slip through, or for a character's
identity to be described differently in chapter 20 than in chapter 2. This
skill re-reads everything already converted and checks it against the
mapping, catching drift that per-chapter conversion misses.

## Workflow

1. **Load ground truth**: read `mapping.md` for the story (from `mapping-builder`).
   If it doesn't exist, stop and tell the user to run `mapping-builder` first —
   without it there's no authoritative reference to check against.

2. **Read every converted chapter** in the output folder
   (`convert_story/<ten-truyen>/`) with `view_file`.

3. **Check each chapter for**:
   - **Leftover original terms**: any occurrence of an original character name,
     location, or term from `mapping.md`'s "Original" column that should have
     been replaced but wasn't.
   - **Identity contradictions**: does this chapter describe a character's
     identity/background/occupation in a way that conflicts with `mapping.md`
     or with how an earlier chapter described them?
   - **POV consistency**: does the narrative person/viewpoint match what
     `mapping.md` specifies for the whole story? Flag any chapter that drifts
     back toward the original POV or switches inconsistently (unless the
     mapping explicitly calls for POV to shift at specific points).

4. **Produce `consistency_report.md`** in the story's output folder
   (`convert_story/<ten-truyen>/consistency_report.md`), listing
   each issue found with: chapter file, a short quote as evidence, and what's
   wrong (leftover name / contradiction / POV drift).

5. **Ask the user** whether to auto-fix the flagged chapters now or just leave
   the report for manual review — don't silently rewrite chapters without
   confirmation, since "fixing" a false positive could introduce new errors.
