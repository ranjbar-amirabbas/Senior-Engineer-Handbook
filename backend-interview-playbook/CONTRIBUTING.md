# Contributing

## Philosophy

This repository is opinionated. The content should help engineers reason clearly about backend systems, not collect fragmented notes.

Every contribution should improve one of these outcomes:

- teach a concept with practical clarity
- improve interview usefulness
- add a realistic example or trade-off discussion
- make navigation and discovery easier

## Style Guide

- Prefer short paragraphs and explicit headings.
- Favor plain language over academic phrasing.
- Explain trade-offs, not just definitions.
- Write from a production engineering perspective.
- Avoid filler, hype, and generic best-practice lists with no context.

## Naming Conventions

- Use lowercase folder names.
- Use kebab-case file names.
- Use Markdown files only.
- Name files after the topic, not the format.

Examples:

- `async-await.md`
- `system-design.md`
- `deployment-strategies.md`

## Markdown Standards

- Use one H1 per file.
- Use H2 sections for the required document structure.
- Keep sections easy to scan.
- Use fenced code blocks for code, SQL, JSON, or command examples.
- Use relative links when linking to other repository files.

## Required Content Patterns

### Learning Documents

Files in `fundamentals/`, `framework/`, and `architecture/` should follow this structure:

1. Overview
2. Core Concepts
3. Why It Matters in Real Systems
4. Practical Example
5. Common Pitfalls
6. Interview Takeaways
7. Related Topics

### Interview Documents

Interview files should:

- group questions by subtopic
- answer from basic to advanced where possible
- use this exact pattern for each question:
  - Question
  - Short Answer
  - Deep Explanation
  - Senior-Level Notes
  - Common Mistakes

## How To Add A New Topic

1. Choose the correct section.
2. Decide whether the file belongs in `fundamentals/`, `interview/`, or `examples/`.
3. Check for overlapping coverage before creating a new file.
4. Use the template from `templates/category-template.md` or `templates/question-template.md`.
5. Add links from the section `README.md` if the file is a meaningful entry point.

## How To Add New Interview Questions

1. Put concept explanations in fundamentals, not in the interview file.
2. Add questions only if they introduce a new angle, trade-off, or failure mode.
3. Keep the short answer concise.
4. Use the deep explanation to show reasoning, not repetition.
5. Add common mistakes that reflect actual interview weak spots.

## Duplication Avoidance Rules

- Do not repeat the same explanation across multiple fundamentals files.
- Keep conceptual teaching in fundamentals.
- Keep question-answer practice in interview.
- Keep implementation walkthroughs in examples.
- If two files overlap, choose one as the canonical explanation and link to it.

## Pull Request Expectations

- Ensure links are valid.
- Keep file naming and headings consistent.
- Prefer improving an existing file over creating a thin new file.
- If you add a new section, update the relevant `README.md` and the root [README.md](README.md).
