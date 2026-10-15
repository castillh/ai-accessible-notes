# Example: one page of Newton's laws, Path A

These files show one real run of Path A (chat AI + Word) on one dense page of class notes, exactly as produced. **The output has not been edited.** It is here to show what the AI does well and what you still have to check yourself.

## Files

- `newtons-laws_handwritten.png`: the input, one page of stylus notes (Newton's laws, friction, drag, and a two-block incline-and-pulley example).
- `prompt_as_run.txt`: the prompt used for this run. The current prompt in `../prompts/tierA_word_prompt.txt` adds one line ("Do not add topics I did not include") because of what this run did.
- `newtons-laws_AI-output.docx`: the Word file the AI produced.
- `newtons-laws_AI-output.pdf`: the PDF exported from that file with Word for the web (File > Download as PDF).

## How it was made

- Tool: Claude on the web, Opus 5.5 model, high effort setting, on 2026-10-15. It took about 7 minutes.
- The AI wrote the content in Markdown with LaTeX math, converted it to Word with pandoc, cropped the 10 diagrams from the page image with Python, and checked its own PDF rendering before handing over the file.
- An earlier run with a faster model setting produced equations as flat text and no diagrams, while stating that the equations would render properly.

## Checks

- Word Accessibility Checker (Word for the web): no issues.
- The .docx contains 64 structured Word equations, 10 cropped diagrams with alt text, real Title/Heading 1/Heading 2 styles, real lists, and a table with a header row.
- The PDF is tagged. Each equation is tagged as a formula with a spoken text version.
- veraPDF, PDF/UA-1 profile: 104 of 106 rules passed. The 2 failures: a table header cell without a Scope attribute, and no PDF/UA identification in the file's metadata.

## What a human still has to fix

- The AI added two things that are not in the notes: a one-sentence statement of the third law, and a table of each block's coordinate axes.
- Two headings carry labels from the handwritten page that do not belong in a student document: "August 26, 2026" and "(b1), (c1), (d1) Block 1".
- The AI listed 7 marks it could not read with confidence (for example, whether a drag-force relation is "∝" or "="). They have to be answered by the author, and the diagrams' alt text has to be read against the drawings.
