---
name: concept-learning
description: This skill should be used when the user wants to deeply learn a technical concept (e.g., "Agent", "大模型的上下文", "Skill", "RAG", "Transformer") and produce a structured study guide. It generates a concept-map-first learning material that includes: a visual concept map, definition, layered explanation (intuition / mechanism / boundaries), classic analogies, common misconceptions, and a self-check section. Output is saved as Markdown next to the project notes folder, with an optional companion HTML preview. Use this skill whenever the user says "学一下 X"、"给我讲讲 X"、"学习 X 这个概念" or asks for a study note on a named concept.
agent_created: true
---

# Concept Learning — 概念学习资料生成 Skill

## Overview

Turn a bare concept name (e.g., "Agent", "大模型的上下文", "Skill") into a complete, beginner-friendly study material with consistent structure. The material always opens with a concept map (mental model), then drills down into definition, layered explanation, analogies, common misconceptions, and a self-check. Output is written as Markdown into `notes/concepts/` and rendered as a side-by-side HTML preview.

## When to Use

- User says: "学习 X"、"讲讲 X"、"X 是什么"、"给我整理 X 的学习资料"
- User wants a study guide for a single named concept
- User wants beginner-friendly but technically accurate material

Do NOT use this skill for:
- Comparing two or more concepts (use a comparison skill)
- Implementing code that uses a concept (use a code-helper skill)
- Looking up a fact or definition in one line (just answer directly)

## Output Structure (mandatory, in this order)

Every concept study material must contain the following sections. Section names in Chinese; content can mix Chinese and English terms.

1. **概念地图（Concept Map）** — A short bulleted list or a one-line ASCII tree showing how the concept relates to its surrounding context. Goal: 30 seconds to grasp where this concept sits in the bigger picture.
2. **一句话定义（Definition）** — One sentence, plain language, no jargon.
3. **分层理解（Layered Explanation）** — Three sub-blocks:
   - 直观类比（Intuition）— A non-technical analogy from everyday life.
   - 机制原理（Mechanism）— How it actually works, in 3–5 short paragraphs.
   - 边界与误区（Boundaries）— What it is NOT, and the most common misconceptions.
4. **正反例（Examples）** — At least one good example and one bad/misleading example.
5. **自检问题（Self-Check）** — 3 questions the learner should be able to answer after reading.

## File Layout

All output goes to `notes/concepts/`:

```
notes/concepts/
├── <concept-slug>.md     # main study material
└── <concept-slug>.html   # optional HTML preview (single file, dark-on-light, opens in browser)
```

Slug rule: lowercase, ASCII, hyphens only. Example: `Agent` → `agent`, `大模型的上下文` → `llm-context`, `Skill` → `skill`.

## Workflow

Follow these steps in order. Do not skip.

### Step 1 — Confirm the concept and the language

If the user request names a single concept, proceed. If it's ambiguous (e.g., "讲讲 AI"), ask one short clarifying question: "你想学哪个具体概念？比如 Agent、上下文、Skill？"

Default output language: 简体中文. If the user wrote in English, mirror English.

### Step 2 — Write the Markdown study material

Create `notes/concepts/<slug>.md` with the 5 mandatory sections above. Length target: 800–1500 Chinese characters. Tone: patient teacher talking to a curious beginner. Use concrete examples, avoid abstract philosophy.

### Step 3 — Build the HTML preview

Create `notes/concepts/<slug>.html` as a single-file HTML. Requirements:
- Self-contained (no external CDN, no external CSS, no JS).
- Light theme (white-ish background, dark text) — matches the user's IDE theme.
- Layout: a left sidebar that mirrors the Markdown TOC, a right main column with the rendered content.
- Use inline CSS in a single `<style>` block.
- Filename: same slug as the Markdown.

### Step 4 — Show the result

After writing both files, present them with `present_files` so the user can preview the HTML and download the Markdown. Do not just say "done" — actually call `present_files` with both absolute paths.

### Step 5 — Quick self-review

Before finishing, check the Markdown against this checklist:
- [ ] 5 sections present, in the right order
- [ ] At least one everyday-life analogy
- [ ] At least one misconception explicitly called out
- [ ] 3 self-check questions, with answer hints in a `<details>` block
- [ ] No unverified factual claims (e.g., invented paper titles, fake API names)

If any box is unchecked, fix the Markdown before calling `present_files`.

## Style Guide

- Use second-person address ("你") — feels like a 1-on-1 tutoring session.
- Bold **key terms** on first mention.
- Inline code with backticks for tool/parameter names.
- Code blocks (```) only when the example is actual runnable code.
- Prefer short paragraphs (3–5 lines) over long walls of text.
- The 直观类比 must use a real-life situation (cooking, traveling, office work), not another technical concept.

## What This Skill Does NOT Do

- It does not produce comparison tables across multiple concepts.
- It does not write or modify code.
- It does not fetch web pages or call external APIs.
- It does not generate images or videos.

If the user's request needs any of the above, mention it and suggest the right tool instead.
