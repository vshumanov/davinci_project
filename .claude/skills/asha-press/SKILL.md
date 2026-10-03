---
name: asha-press
description: Generate a plain-text topic ebook (.txt) for a Nokia Asha e-reader. Use when the user gives a topic and wants an Asha ebook, Asha Press file, or a txt ebook for the Nokia reader. One topic in, one .txt file out. Do not use for PDFs, Word files, or normal chat explanations.
---

# Asha Press

Turns a topic into a single ASCII .txt ebook for a Nokia Asha (S40) running a custom txt reader off the SD card. Purpose: fast conceptual orientation, read in smoke-break chunks over a day or two. Not a course. Not a reference doc. The reader should understand the shape of the topic and talk about it competently.

Conversion and transfer to the device happen outside this skill.

## Rules

- One topic = one file = one delivery. Multiple topics in a request: produce separate files, deliver each separately. Never merge.
- English only. Always.
- Web search is pre-approved for this workflow. Research the topic before writing. Do not ask first.
- Save every ebook in the current working directory (cwd, whatever pwd returns). No subfolders, no /tmp, no scratchpad. Final path = ./<Title Case Name>.txt. Drafts and scratch work may live elsewhere, but the finished file must end up in the cwd.
- Write the file with Write using the absolute cwd path. That is the delivery: the finished .txt sits in the cwd, nothing else. Do not paste the ebook into chat. Confirm in a line or two.
- File name: the topic as plain readable text in Title Case, spaces allowed, .txt extension (example: Combustion Engines.txt). Capitalize the main words, lowercase minor words (a, an, the, of, in, and, for, to). No hyphens or underscores as word separators, no slugs, no ALL CAPS. Keep it ASCII, and strip characters that break filenames (/ \ : * ? " < > |). If the name already exists in the cwd, do not overwrite: append " 2", " 3", and so on before the extension. The ALL CAPS title inside the file is unaffected.
- Quote the path in shell commands, since it contains spaces.

## Output format (hard constraints)

Plain text. ASCII only.

- No smart quotes, no em/en dashes, no unicode bullets or arrows. Straight quotes and hyphens only.
- Title on its own line, ALL CAPS. Blank line after.
- Section headers in ALL CAPS. Nothing else marks a header.
- One blank line between paragraphs. That is the only separator.
- No markdown: no #, *, _, |, [], backticks. No leading hyphen bullets.
- Lists numbered as "1) ... 2) ...". No bullet characters.
- Emphasis = CAPS only, used sparingly.
- No tables, links, images, footnotes. Name sources in prose if needed.
- Keep paragraphs short. Small screen, no wrap mercy.

## Structure and depth

3-5 sections covering, in roughly this order:

1) what it is
2) why it matters
3) the core mechanism or idea
4) one or two things people get wrong about it
5) where it connects to adjacent things worth knowing

Each section is a clean break point: someone can stop after any section and resume later without rereading.

Length: 4000-5000 words for general topics.

## Software / CS topics

Go deeper than the default. More mechanism, less hand-waving. Length follows the mechanism: never truncate an explanation to hit a word count. Code samples are permitted and encouraged where they carry the idea faster than prose.

Code block format:

- Line of ===== above and below the block.
- Indent code 4 spaces so it stands apart from prose.
- Keep lines under ~60 characters. Break long lines manually.
- Comments sparse, lowercase, no caps (so they never read as headers).
- Pick languages where the point survives whitespace mangling (C, JavaScript, Go, shell, SQL). Avoid languages where indentation is the syntax. If Python is the right tool, keep blocks tiny and flat, and say the idea in prose too.
- A 60-column limit means short names and early line breaks. Design the sample for the screen.

## Workflow

1) Parse the topic(s). Split into separate jobs if more than one.
2) Research with web search. Verify specifics: dates, names, numbers, version behavior.
3) Outline 3-5 sections. Run pwd, then write the full text to "<cwd>/<Title Case Name>.txt".
4) Validate the file with Bash:

    grep -nP '[^\x00-\x7F]' "Topic Name.txt"
    grep -nE '^\s*[#*|-]|\[|\]|`' "Topic Name.txt"

   First command must return nothing. Second: review hits. Code blocks may legitimately contain brackets or hyphens. Prose must not. Fix and re-run.
5) Check word count with wc -w. General topics: 4000-5000. Adjust.
6) Confirm each file sits in the cwd (ls). That file in the cwd is the delivery; there is no separate send step.
7) Reply with one or two lines: what was made, approximate length, saved in cwd. Nothing else.
