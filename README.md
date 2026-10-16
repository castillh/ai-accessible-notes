# Turning handwritten notes into accessible documents with AI

A practical workflow for instructors at any level, from K-12 to graduate courses, on Windows, Mac, or Linux.

Horacio Castillo, Department of Physics and Astronomy, Ohio University (castillh@ohio.edu)
Version 0, October 2026. Facts about tools and rules were checked on 2026-10-15; they change often, so check the linked sources before relying on a detail.

---

## 1. What this guide is for

You have handwritten material: class notes, worked examples, whiteboard photos, tablet/stylus pages.
You need a version that a student using a screen reader, magnification, or other assistive technology can use.
This guide shows how to get there with an AI chat tool plus software you probably already have.

The result is a document with:

- real headings (so a screen reader can navigate by section),
- real equations (not pictures of equations),
- alt text on every figure (a description a screen reader reads aloud),
- real lists and tables,
- correct reading order and document language.

## 2. Why now

- The US Department of Justice rule under ADA Title II requires web content and course documents of public entities to meet **WCAG 2.1 Level AA**.
- Public universities and public K-12 school districts are both covered.
- Compliance dates (as amended on 2026-04-20): **April 26, 2027** for entities serving 50,000 people or more; **April 26, 2028** for smaller entities.
- A school district's size is counted by the population of the city, county, or district it serves (see the ADA.gov page in the references).
- Your own institution may have a stricter internal timeline. Check with your accessibility office.

## 3. The one idea that matters most: rewrite, do not transcribe

The first approach most people try is to make the AI reproduce the handwritten page as faithfully as possible: cut the page into regions, transcribe each one, crop each drawing, and have several AI "reviewers" check the result against the image.

I tried that. It was slow, it used up my AI usage limits repeatedly before finishing one page, and it still made errors.

What works is different: **treat the handwritten page as a content outline.**
Ask the AI to write a clean, new document that presents the same content, in the same order, with the same notation.
Diagrams are redrawn or cropped and described, not traced.

With this approach, one lecture's worth of notes became a finished, validated document in a single working session, plus my own review.

## 4. Choose your path

| Path | You need | Output | Math quality | Best for |
|---|---|---|---|---|
| **A. Chat AI + Word** | A browser and Microsoft Word (Windows, Mac, or Word for the web) | .docx, or a tagged PDF exported from Word | Native Word equations; good for most courses | Most instructors, K-12 and introductory courses |
| **B. Chat AI + Overleaf** | A browser and a free Overleaf account | Tagged PDF from LaTeX | Best available math accessibility | Equation-heavy upper-level courses; anyone who already writes LaTeX |
| **C. AI agent on your own computer** | A local LaTeX installation, an AI coding agent (for example Claude Code), a validator | Tagged PDF, scripted and validated | Best available | Converting many documents; people comfortable with a terminal |

All three paths share the same steps (Section 5). Only the tools change.

**Is a PDF required?** Often it is not. Many universities prefer an LMS page (Canvas, Blackboard, Google Classroom) or a Word file over a PDF, because those are easier to make accessible. If a PDF is required, Paths B and C give the best result for math.

## 5. The steps (every path)

1. **Prepare the source.** Scan or photograph each page flat, in good light, at a readable resolution. A tablet/stylus export to PDF is ideal. Remove anything that identifies a student.
2. **Use an approved AI account.** Sign in with your institutional account when your school provides one (Section 9). Do not upload student work or student data unless your school's policy allows it for that tool (Section 9).
3. **Generate the draft.** Upload the page(s) to the AI tool and paste the prompt for your path (in the `prompts/` folder). The prompts tell the AI to rewrite rather than transcribe, to use real structure, and to list everything it could not read instead of guessing silently.
4. **Answer the AI's questions.** The AI lists symbols and words it could not read confidently. Answer them in the chat and ask for a corrected file.
5. **Check with an automated tool** (Section 7). Fix everything it flags.
6. **Read it yourself, as the expert.** This step cannot be skipped (Section 8). Check every equation, sign, subscript, and number against your notes. Read every alt text against its figure.
7. **Publish** in the format your course uses.

## 6. Step-by-step by path

### Path A: chat AI + Word

1. Open a chat AI that can read images and produce a downloadable Word file. As of October 2026 this includes Claude (all plans, including free), Google Gemini (all users), and Microsoft Copilot (Microsoft 365 Copilot Chat). ChatGPT accepts uploads on the free plan; check whether your plan can produce a .docx download. Choose the most capable model your plan offers: in our test, a faster setting produced equations as flat text and no diagrams, while the strongest setting produced real equations and cropped diagrams (it took about 7 minutes for one dense page).
2. Upload the page(s) and paste `prompts/tierA_word_prompt.txt`, with the course and topic filled in.
3. Download the .docx and open it in Word. If you use Word for the web and the equations appear only as the placeholder "[Equation]", switch browsers: in our tests, Word for the web in Firefox on Linux showed every equation that way, even in documents created by desktop Word, while Chromium showed them correctly. (Not yet tested with Firefox on Windows or Mac.)
4. Run **Review > Check Accessibility** (available in Word for Windows, Mac, and the web). Fix every error and warning.
5. Check that every equation is a real Word equation (click on it: an equation editor should appear).
6. If you need a PDF:
   - **Windows:** File > Save As (or Save a Copy) > PDF > Options > keep **"Document structure tags for accessibility"** checked.
   - **Mac:** File > Save As > File Format: PDF > choose **"Best for electronic distribution and accessibility"**.
   - **Word for the web:** File > Download as PDF (not "Download as PDF with comments").
   - **Never use "Print to PDF".** It throws away all the structure.
7. In our test, the PDF from Word for the web was tagged, every equation had a spoken text version, and the automated PDF/UA-1 check (veraPDF) found only 2 small failures out of 106 rules (a table-header attribute and a missing PDF/UA identification label). Recent Word versions tag equations in the exported PDF with alt text. Math accessibility in Word-exported PDFs is still weaker than in the .docx itself, so give students the .docx (or an LMS page) when you can.

**Google Docs:** since early 2025 its PDF export includes tags for headings, lists, links, and image alt text. How it handles equations in exported PDFs is not documented. Use Word for math-heavy material until this is clearer.

### Path B: chat AI + Overleaf (LaTeX)

1. Create a free account at overleaf.com and start a blank project.
2. In the project **Menu**, set **Compiler = LuaLaTeX** and **TeX Live version = 2025 or 2026**. Older versions cannot tag math.
3. Upload `templates/accessible-notes-template.tex` and compile it once. You should get a one-page sample document. This template was validated as PDF/UA-1 with veraPDF (Section 7).
4. In the chat AI, upload your page(s) **and** the template, and paste `prompts/tierB_latex_prompt.txt`.
5. Paste the AI's LaTeX into Overleaf and compile. If the AI asks you to crop a diagram, save that crop as an image and upload it to the Overleaf project.
6. Download the PDF and validate it (Section 7).

Things to know:

- Keep the `\DocumentMetadata` block exactly as in the template. It is what turns on tagging.
- Use the `article` class. The `beamer` (slides) and `revtex` classes do not support tagging yet.
- Every `\includegraphics` and TikZ figure needs an `alt={...}` description.
- Overleaf's free plan has a short compile-time limit. Long documents with tagging may exceed it. Split the document, or compile with `tagging=draft` while editing and switch back to `tagging=on` for the final version.
- Overleaf's own accessibility documentation suggests the newer PDF/UA-2 standard with MathML math tagging. The template uses PDF/UA-1 because that is what the most common checker (PAC, Section 7) tests. Either is defensible; check what your institution expects.

### Path C: AI agent on your own computer

This is my own working setup, on Linux: TeX Live 2026 (installed in my home directory), Claude Code as the AI agent, and veraPDF as the validator. Each of these tools is also available for Windows and Mac, but I have only tested the workflow on Linux.

The agent does steps 3 to 5 of Section 5 by itself: it reads the source, writes the LaTeX, redraws every figure in TikZ with alt text, compiles, runs the validator, renders the pages to images and inspects them, and fixes problems before handing the document to me. I then do the expert read (step 6), usually in two to five short rounds of corrections.

What I learned about running it efficiently:

- One document per fresh session. Long sessions full of images use up usage limits fast.
- Write your standing rules (notation, style, "no mention of the source", "number every equation") in a file the agent reads every time, so you do not repeat corrections.
- Ask for a plan (structure, notation, scope) before the agent drafts anything.

## 7. Checking accessibility with software

| Tool | Platforms | Cost | Checks |
|---|---|---|---|
| Word Accessibility Checker | Windows, Mac, web | Included with Word | Word documents |
| PAC (PDF Accessibility Checker) | Windows only | Free | PDFs, against PDF/UA-1 |
| veraPDF | Windows, Mac, Linux | Free | PDFs, against PDF/UA-1 or PDF/UA-2 |
| Adobe Acrobat Pro | Windows, Mac | Paid | PDFs |

A clean report from a checker is necessary but not sufficient.
Checkers confirm that the structure exists.
They cannot tell whether an alt text is correct, whether an equation was transcribed correctly, or whether the reading order makes sense.

## 8. What the AI gets wrong, and what only you can check

These came up repeatedly in my own conversions:

- **Misread handwriting.** A handwritten v, a, or F; a + or −; a subscript 1 or l. The AI often picks one reading confidently.
- **Confident agreement is not proof.** I had three separate AI reviewers independently agree on a reading of one handwritten symbol. All three were wrong. AI systems built the same way tend to make the same mistakes, so asking more AI reviewers does not fix this. (Using a different kind of tool, for example a dedicated math-OCR program such as Mathpix, as a second opinion does help.)
- **Plausible but invented details in alt text.** An alt text described a pair of rotated axes that was not in the drawing. It sounded right, so nobody caught it until I compared it with the drawing myself.
- **Scope creep.** The AI adds material you did not teach, or "improves" your notation. The prompts ask it not to; check anyway.
- **References to the conversion.** Phrases such as "as shown in the handwritten notes" do not belong in the student document.

So the expert read (step 6) is the step that makes the document correct. The software makes it accessible; you make it right.

## 9. Privacy and accounts

- Prefer your institution's licensed AI tool and sign in with your school account. Several vendors offer education versions that do not train on your data, for example Microsoft 365 Copilot Chat with a school account, Gemini for Education, ChatGPT for Teachers (free for verified US K-12 staff), ChatGPT Edu, and Claude for Education.
- Your own lecture notes are usually fine to upload. Student work, grades, names, or photos of students: usually not, but check your school's policy for possible exceptions. For example, Ohio University allows sensitive data, including student records covered by FERPA, in the protected version of Microsoft Copilot (signed in with the OHIO login); no other AI tool is approved for such data there.
- Ask your IT or accessibility office which tools are approved.

## 10. Files in this repository

- `prompts/tierA_word_prompt.txt`: prompt for Path A (Word output).
- `prompts/tierB_latex_prompt.txt`: prompt for Path B (LaTeX output, for Overleaf).
- `prompts/second_opinion_prompt.txt`: asks an AI to compare your notes against the typed document and list differences. Useful, but not a replacement for your own read (Section 8).
- `templates/accessible-notes-template.tex`: LaTeX starter template; compiles to a tagged PDF/UA-1 document with headings, a numbered equation, a list, a figure with alt text, and a table with a header row.

- `examples/`: one real, unedited Path A run on a page of Newton's laws: the handwritten input, the prompt, the Word file and PDF the AI produced, and what still had to be fixed by hand.

To use a prompt, open the .txt file, select all, copy, and paste it into the chat.

## 11. Open questions

These are not settled as of October 2026:

- How Google Docs tags equations in exported PDFs.
- How well screen readers handle math in Word-exported PDFs compared with LaTeX-tagged PDFs.
- Whether every LMS accessibility checker correctly recognizes tagged LaTeX PDFs (Overleaf reports that some flag them wrongly).

Suggestions and reports of what worked for you are welcome: castillh@ohio.edu.

## License

- The guide, prompts, and examples are licensed under the Creative Commons Attribution 4.0 International License (CC BY 4.0), see `LICENSE-CC-BY-4.0.txt`. You may copy, adapt, and share them, including for your own courses and workshops, as long as you credit the source.
- The LaTeX template in `templates/` is licensed under the MIT License, see `templates/LICENSE-MIT.txt`.

## References (accessed 2026-10-15)

- DOJ extension of compliance dates, Federal Register, 2026-04-20: https://www.federalregister.gov/documents/2026/04/20/2026-07663/extension-of-compliance-dates-for-nondiscrimination-on-the-basis-of-disability-accessibility-of-web
- ADA.gov, Title II web rule fact sheet: https://www.ada.gov/resources/2024-03-08-web-rule/
- Microsoft, Word PDF accessibility: https://learn.microsoft.com/en-us/office/pdf/word/wordpdfaccessibility
- Microsoft, create accessible PDFs: https://support.microsoft.com/en-us/accessibility/office-accessibility/create-accessible-pdfs
- Microsoft, Accessibility Checker: https://support.microsoft.com/en-us/accessibility/office-accessibility/improve-accessibility-with-the-accessibility-checker
- Google Workspace, tagged PDF export (Dec 2024): https://workspaceupdates.googleblog.com/2024/12/release-notes-12-06-2024.html
- Overleaf, creating accessible PDFs: https://docs.overleaf.com/writing-and-editing/creating-accessible-pdfs
- Overleaf, TeX Live 2026: https://www.overleaf.com/blog/tex-live-2026-is-now-available
- Overleaf, plan limits: https://docs.overleaf.com/getting-started/free-and-premium-plans/plan-limits
- LaTeX Tagging Project, package status: https://latex3.github.io/tagging-project/tagging-status/
- PAC: https://pac.pdf-accessibility.org/en/download
- veraPDF: https://github.com/veraPDF/veraPDF-library/releases
- Adobe Acrobat accessibility checker: https://helpx.adobe.com/acrobat/using/create-verify-pdf-accessibility.html
- Claude file creation: https://support.claude.com/en/articles/12111783-create-and-edit-files-with-claude
- Gemini file generation: https://blog.google/innovation-and-ai/products/gemini-app/generate-files-in-gemini/
- Microsoft 365 Copilot Chat data protection: https://support.microsoft.com/en-us/privacy/data-protection-when-using-microsoft-365-copilot-chat-for-work-or-school
- Gemini for Education: https://edu.google.com/ai/gemini-for-education/
- ChatGPT for Teachers: https://help.openai.com/en/articles/12844995-chatgpt-for-teachers
- Claude for Education data: https://support.claude.com/en/articles/11732894-who-owns-and-manages-the-data-of-my-claude-for-education-account
- Mathpix: https://mathpix.com/pricing
- Ohio University OIT, secure use of AI tools: https://www.ohio.edu/oit/security/secure-use-artificial-intelligence-tools
