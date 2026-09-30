---
name: make-knowledge-cards
description: Turn an article or a local Markdown/TXT file into 5-8 distilled knowledge cards, each containing a title, one core knowledge point, a plain-language explanation, and an example or self-test question. Use when the user pastes an article or points to a .md/.txt file and asks for knowledge cards, study cards, review cards, flashcards, key-takeaway cards, or a distilled learning summary. Extracts only knowledge grounded in the source, keeps one knowledge point per card, removes duplicated content, and never pads or fabricates. Does not fetch web pages, parse PDF, export to Anki, or provide a GUI.
---

# Make Knowledge Cards

## Overview

Distill a text source into 5-8 knowledge cards a learner can review. Every card carries exactly one knowledge point and must be fully traceable to the source.

## Scope

Supported input:

- Article text pasted directly in the conversation
- Local Markdown (`.md`) or plain text (`.txt`) file

Out of scope — state the limitation briefly and offer the closest supported option (e.g., paste the text instead); never attempt these:

- Web page / URL scraping (do not fetch URLs)
- PDF or other binary document parsing
- Anki / CSV flashcard export
- Any GUI

## Workflow

### 1. Get the source

- If the user pastes text, use it as-is.
- If the user names a file, read it. Accept only `.md` / `.txt`; for other formats, explain the limitation and ask for pasted text or a converted file.

### 2. Extract candidate knowledge points

Scan the source for what a learner should retain:

- definitions, concepts, principles
- mechanisms — how and why things work
- methods, rules, or steps that generalize
- cause-effect relations and conclusions the source supports
- important facts, numbers, and constraints stated by the source

Skip transitions, filler, hype, pure opinions, and anecdotes that carry no transferable knowledge.

### 3. Select, dedupe, and size

- Rank candidates by (1) centrality to the topic and (2) usefulness to a learner; keep the highest-value points.
- Merge duplicates: two candidates are duplicates when one's content is fully covered by the other, when they would reuse the same example, or when they merely restate each other at different levels of detail (e.g., an idea and its step-by-step expansion). Merge into one card, or re-scope so each card owns distinct material; keep the clearest explanation and the strongest grounded example.
- Aim for 5-8 cards. Never pad:
  - If the source yields fewer than 5 distinct points after dedupe, produce only that many cards and add one closing line noting that the source does not contain enough distinct knowledge for 5 cards.
  - If it yields more than 8, keep the 8 most important and note in one line which minor points were left out.
- If the user explicitly requests a count outside 5-8, follow the user; all other rules still apply.
- Order cards from foundational to advanced so the set reads as a learning path.

### 4. Write the cards

Write cards in the same language as the source unless the user requests another language. Use exactly one of 例子 or 自测 in the fourth field.

```markdown
## Card 1 · <card title>

- **核心知识：** <the single knowledge point, one sentence>
- **简明解释：** <2-3 sentences of plain-language explanation>
- **例子：** <concrete example grounded in the source>
```

For non-Chinese output use the labels `Core point`, `Explanation`, `Example`, `Self-test`.

Field rules:

- **Title**: name the point, not the section; keep it under ~15 words.
- **核心知识 / Core point**: exactly one knowledge point. If a sentence holds two points, split them into two cards.
- **简明解释 / Explanation**: clarify the point in simpler words using only claims the source makes. Do not add causes, benefits, or reasons the source never states — even plausible-sounding ones.
- **例子 / Example**: use a real case, instance, or data point from the source. Never invent cases, numbers, or names. Do not reuse the wording of the explanation, and do not pass off a restatement of the point itself as an example — if no distinct example exists, use 自测 instead.
- **自测 / Self-test**: when no grounded example exists, write one question answerable from the source, followed by a short answer stated in the source:

```markdown
- **自测：** <question whose answer is unambiguous>（答案：<short answer from the source>）
```

### 5. Quality check before delivering

Verify every card against this checklist and fix violations first:

1. **Traceability** — every statement maps to the source; delete anything the source does not say, including unsourced reasons or benefits inside explanations.
2. **One point per card.**
3. **No duplication** — titles, explanations, and examples are all distinct across cards; no card's material is contained in another card.
4. **Count** — 5-8 cards, or fewer with the sufficiency note when the source is thin.
5. **Completeness** — all four fields present, with exactly one of 例子/自测.

### 6. Deliver

- Output all cards as one Markdown block, preceded by one header line with the source name and card count.
- If the user asks for a file, save the Markdown to the path the user gives.
