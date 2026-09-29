---
name: text-extractor
description: Use this skill when setting up new chapters for an original story, either by transcribing text from a screenshot or fetching text from a website URL. It ensures strict preservation of formatting and complete text extraction without any arbitrary edits or omissions.
---

# Text Extractor Skill

This skill provides the workflow for extracting text from images (screenshots) or websites (URLs) and saving it into the `original_story` chapter files. 

## Context
During the setup phase of a new story, the original text needs to be populated into chapter files inside the `original_story/` folder. The user might provide screenshots or URLs containing the raw text. Your task is to extract that text exactly as it is and save it to the specified file.

## Trigger Prompts
Recognize any of these (or obvious paraphrases, in Vietnamese or English) as a request to use this skill:
- "Extract text from this screenshot and save to `<file>`"
- "Add text from website `<url>` to `<file>`"
- "Lấy text từ ảnh này thêm vào `<file>`"
- "Lấy text từ web `<url>` thêm vào `<file>`" / "Access vào `<url>` và add text"

## Rules & Requirements
1. **Absolute Accuracy**: You must extract and transcribe the text EXACTLY as it appears in the source (screenshot or website). 
2. **No Omissions**: Tuyệt đối không được bỏ sót chữ. Do NOT skip, summarize, or omit any words, sentences, or paragraphs under any circumstances.
3. **Preserve Formatting**: Keep the exact original formatting. If the source text has blank lines between paragraphs, you must keep them. If there are numbered sections or specific spacing, keep them. Do not merge paragraphs unnecessarily.
4. **No Edits**: Không tự ý biến đổi. Do NOT fix typos, adjust grammar, or rewrite sentences. You must preserve the text precisely as the original author wrote it.
5. **Rule 8 Exemption**: Writing directly to files inside `original_story/` is permitted when executing this skill, as it falls under the setup phase exception of Rule 8 in `GEMINI.md`.
6. **Auto-create Chapter Files**: Mỗi một URL mới mà user cung cấp, agent phải tự động tạo file md mới theo format `Chapter-<nextNumber>.md` (ví dụ: `Chapter-05.md`) để lưu nội dung.

## Workflow
1. **Analyze Source**:
   - If a URL is provided, use the `read_url_content` tool to fetch the content. Then use `view_file` or `grep_search` to locate the actual story/chapter text within the fetched data, avoiding ads, navigation menus, and unrelated site content.
   - If a screenshot is provided, carefully read and transcribe the text visible in the image.
2. **Format Text**: Prepare the text as clean Markdown, preserving paragraph breaks (usually a blank line between paragraphs) exactly as they appear in the source.
3. **Write to File**: Use `write_to_file` (if the file is empty) or `replace_file_content` (to append) to insert the text into the specified chapter file in the `original_story` folder.
4. **Report**: Confirm to the user that the text has been successfully added, explicitly stating that formatting was preserved and no words were omitted.
