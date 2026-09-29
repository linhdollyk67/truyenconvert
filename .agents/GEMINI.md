# Story Conversion Rules

When working on story conversion in this directory, you must follow these rules strictly:

1. **Character Names**: You must change the names of the characters.
2. **Character Identities**: You must change the identities/backgrounds of the characters, but the new identities must not be too different from the original ones.
3. **Meeting Circumstances**: Modify the circumstances of how characters meet so that they are similar to the original, but adapted to the new context/identities.
4. **Dialogues**: Keep character dialogues similar to the original dialogues.
5. **Plot**: The plot/events of the story must be SIMILAR to the original (keep the main story beats and their order), but you may adjust small details as needed to fit the new identities/context.
6. **Personalities**: The personalities of the characters MUST remain exactly the same.
7. **Narrative Point of View**: You must change the narrative point of view from the original (e.g. 3rd person → 1st person, or switch the POV character).
8. **Original Story Files Are Read-Only**: Never create, edit, overwrite, rename, or delete any file inside the original story folder (e.g. `original_story/`). Only ever read from it. All writes go to the output/converted story folder. *(Note: This rule only applies when the original story is fully ready and the conversion process has started. During the setup phase of a new story in `original_story`, the user can input text directly, ask the agent to extract text from screenshots, or fetch text from a website. In this setup phase, the agent CAN add text directly to the requested chapter files in `original_story`, and must keep the exact original format as in the screenshot or website, absolutely without omitting any words.)*
9. **One Chapter at a Time, With Approval**: Convert and deliver ONE chapter at a time. Do not start converting the next chapter until the user has explicitly approved the current one.
10. **Sync Revisions Across Chapters**: If the user requests a change to a chapter (e.g. a plot detail, an identity fact, a name) that affects other chapters — already converted or still upcoming — update the mapping and every affected chapter so the whole story stays consistent, and tell the user which chapters were touched.
