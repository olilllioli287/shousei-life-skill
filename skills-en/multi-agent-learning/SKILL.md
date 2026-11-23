---
name: multi-agent-learning-en
description: Multi-model learning workflow for attending classes, studying, organizing course readers, reading and asking questions with an AI tutor, and pre-exam review. Use when organizing slides and course materials into structured readers, conducting Socratic line-by-line Q&A, or batch-processing conversation logs afterward to catch mistakes and compile missed questions.
---

Experimental: this workflow has limited real-world validation.

This workflow requires only plain conversation and local files to run, without relying on specific panels or automation scripts.

## Stage 1: Organize into a Reader (Once per Chapter)

- **Input**: Professor's slides, homework solutions, and online quiz reviews (with standard answers), placed in local folders and categorized by subject.
- **Approach**:
  1. For slide decks heavy on diagrams and charts, first pass them to a model with strong vision capabilities to convert into text descriptions.
  2. Copy that chapter's materials into a temporary folder (copy manually if there is no packaging script), hand them to a read-only organizing model to synthesize, verify line-by-line against the original slides, and produce a chapter reader (e.g., `ch03.md`, for AI reading).
  3. After verification is complete, optionally convert `ch03.md` to PDF (e.g., `ch03.pdf`, for human reading).
- **Each section includes**: section number; plain explanation -> terminology -> example; slide page numbers; abbreviations with full English names; standard exam phrasing; personal missed questions; self-check questions.

## Stage 2: Read and Ask Questions (Each Study Session)

| Use Case | Recommended Model Role | Reason |
| --- | --- | --- |
| On-demand Q&A (AI tutor) | Local model | Fast, free, data stays local |
| Reading slide diagrams, screenshot questions | Model with strong vision capabilities | Better visual recognition; open and ask directly |
| Pre-exam question generation and verification | Question-generating model writes questions -> another model spot-checks against the reader | Prevents a single model from fabricating content |

### Tutor Rules
- Explain only one concept at a time, and wait for the learner's response before continuing.
- Give line-by-line feedback; never just say "correct".
- Treat the reader and slides as authoritative; if unsure, say you are unsure.

## Stage 3: Two-Stage Harvesting

1. **After each study session (quick wrap-up)**: Save that session's conversation log into the `pending-inbox/` folder (if there is no harvesting script, directly export or copy the conversation into a text file in that folder), and the study session can end there.
2. **After finishing a chapter or on weekends (batch harvesting)**:
   - Have a primary model or local long-context model read the logs in `pending-inbox/`, catch places where the AI explained something wrong and list corrections, and pull out missed questions and good questions.
   - Write the results into "My Missed Questions" in the local reader. If compiling into public notes, the reader and publishable notes must be produced separately; filter once before publishing to keep only clearly publishable content.

## Rules

- **Course materials are authoritative**: Every sentence in the reader must map to a slide page number; mark content not in the slides as "Supplementary"; keep original course materials untouched.
- **Privacy and routing**: Do not put private information such as school records, grades, student IDs, or classmates' names into public notes or public free model pools; use a local model to summarize private long files.
