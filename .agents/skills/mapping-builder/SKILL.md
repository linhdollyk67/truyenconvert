---
name: mapping-builder
description: >-
  Use this skill after story-analyzer (or when starting a conversion with no
  analysis yet) to create a persistent mapping.md file recording the character
  name/identity/location/POV conversion mapping. This replaces keeping the
  mapping only in scratchpad memory, so it survives across sessions and can be
  reviewed/edited by the user before bulk conversion begins.
---

# Mapping Builder Skill

## Context
Names and identities decided early get used in every single chapter. If the
mapping only lives in the agent's scratchpad, it is lost between sessions and
easy to drift on for long stories. This skill turns the mapping into a real
file that `story-converter`, `consistency-checker`, and `conversion-qa` all
read as the single source of truth.

## Workflow

1. **Get the source material**:
   - Read `analysis.md` produced by `story-analyzer` for this story, if it exists.
   - If it doesn't exist, do a quick lightweight read of the story first to get
     the character list, identities, and narrative POV.

2. **Propose the mapping**, for each character:
   - New name.
   - New identity/background — per `GEMINI.md` rule 2, it must be altered but
     **not too different** from the original (e.g. "surgeon" → "ER doctor" is
     fine; "surgeon" → "street vendor" is not).
   - Note the personality traits carried over unchanged (for reference during
     conversion, not for the user to edit — these must stay identical).

3. **Propose other mappings** as needed:
   - Key locations, organizations, titles, or invented terms that need a
     Vietnamese-friendly or setting-appropriate replacement.
   - The new narrative POV (must differ from the original, per `GEMINI.md` rule 7).

4. **Confirm with the user**:
   - Present the mapping as a table and ask for confirmation or edits before
     finalizing. Getting this wrong is expensive to fix after many chapters are
     already converted, so don't skip this check.

5. **Write `mapping.md`** in the story's output folder
   (`convert_story/<ten-truyen>/mapping.md`), e.g.:

   ```markdown
   # Mapping — <story name>

   ## Narrative POV
   - Original: 3rd person, following <character>
   - New: <chosen POV>

   ## Characters
   | Original name | Role | Original identity | New name | New identity | Personality (unchanged) |
   |---|---|---|---|---|---|
   | ... | ... | ... | ... | ... | ... |

   ## Locations / Organizations / Terms
   | Original | New |
   |---|---|
   | ... | ... |
   ```

6. **Hand off**: tell the user `mapping.md` is ready and `story-converter` can
   now use it as the authoritative mapping for every chapter (instead of
   re-deciding names ad hoc per chapter).
