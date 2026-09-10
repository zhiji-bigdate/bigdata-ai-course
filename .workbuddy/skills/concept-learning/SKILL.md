---
name: concept-learning
description: This skill generates a complete, structured study pack for ANY single technical concept (e.g., "Agent", "大模型的上下文", "Skill", "RAG", "Transformer") — not a one-paragraph definition. Input: a single concept name (Chinese or English). Output: a Markdown study pack in `notes/concepts/SLUG.md` plus a single-file HTML preview at `notes/concepts/SLUG.html`, both following a mandatory 8-section structure (learning objectives → driving questions → concept map → structured explanation → application cases → concept disambiguation → self-check → references). Use this skill whenever the user says "学一下 X"、"给我讲讲 X"、"学习 X 这个概念"、"整理 X 的学习材料" or asks for a study note on a named concept. NOT for cross-concept comparisons (use a separate comparison skill) and NOT for code implementation.
agent_created: true
---

# Concept Learning — 概念学习资料生成 Skill

## Overview

Turn a bare concept name (e.g., "Agent", "大模型的上下文", "Skill") into a **complete study pack**, not a one-paragraph definition. Every pack follows a fixed 8-section structure so that the learner gets the full learning loop: know what you'll learn → see the big picture → understand the mechanism → see real uses → know what it isn't → test yourself → know where to go next. Output is written as Markdown into `notes/concepts/SLUG.md` with a matching single-file HTML preview at `notes/concepts/SLUG.html`.

## When to Use

- User says: "学习 X"、"讲讲 X"、"X 是什么"、"给我整理 X 的学习资料"、"学 X 并出学习目标/案例/题"
- User wants a study guide for a single named concept
- User wants beginner-friendly but technically accurate material

Do NOT use this skill for:
- Comparing two or more concepts (use a comparison skill)
- Implementing code that uses a concept (use a code-helper skill)
- Looking up a fact or definition in one line (just answer directly)

## Output Structure (mandatory, in this order, 8 sections)

Every study pack **must** contain the following 8 sections, in this exact order. Section names in Chinese; content can mix Chinese and English terms.

1. **学习目标（Learning Objectives）** — 3–5 bullet points, each starting with a verb ("理解", "能区分", "能举例", "能用自己的话解释", "能识别常见误区"). These are what the learner can DO after reading.
2. **核心问题（Driving Questions）** — 3–5 open questions the learner should be able to answer after reading. Phrased as real questions, with `?` at the end. These pre-frame the material.
3. **概念地图（Concept Map）** — A short bulleted list or ASCII tree showing how this concept relates to its surrounding context. Goal: 30-second mental model.
4. **结构化解释（Structured Explanation）** — The main body. Three sub-blocks:
   - 4.1 一句话定义（Definition）— One sentence, plain language, no jargon.
   - 4.2 直观类比（Intuition）— A non-technical analogy from everyday life (cooking, traveling, office, sports). NOT another technical concept.
   - 4.3 机制原理（Mechanism）— How it actually works, in 3–5 short paragraphs, with concrete technical detail.
   - 4.4 边界与误区（Boundaries）— What it is NOT, and the most common misconceptions (≥ 3).
5. **应用案例（Application Cases）** — 2 cases:
   - 一个**真实场景** where the concept is used (named product / paper / workflow).
   - 一个**课堂/练习** where the learner can try it themselves (e.g., "打开 VS Code，新建一个 .py 文件…").
6. **概念辨析（Concept Disambiguation）** — A short table or 2–3 mini-blocks that contrast this concept with **nearby concepts** the learner might confuse it with. Format: "X vs Y：X 是……，Y 是……；区别在于……".
7. **自测问题（Self-Check）** — 3 questions, each in a `details` block with an answer hint (NOT the full answer — just a hint or one-line summary that the learner can reveal after thinking).
7.5 **选择题练习（Multiple Choice with Feedback）— 可选扩展** — 3–5 multiple-choice questions, each with 4 options (A/B/C/D). For each option, give a concrete feedback (why it's right or wrong). End with a "考点" callout and a "你如果选了 X" tip for the most common wrong answer. The foldable `details` block holds the answer + feedback so the learner tries first, then reveals. This is great for fast self-testing and for concepts with strong "right vs wrong" boundaries (definitions, components, common mistakes).
8. **参考来源（References）** — 3–5 references the learner can read next. Mix of:
   - Official docs (e.g., 官方文档链接)
   - Well-known articles / blog posts
   - Books / papers (with full citation)
   - For each: a one-line "为什么推荐" explanation.
   - If a reference is uncertain, mark it as `[未验证]` rather than fabricating.

## File Layout

All output goes to `notes/concepts/`:

```
notes/concepts/
├── SLUG.md     # main study pack
└── SLUG.html   # single-file HTML preview
```

Slug rule: lowercase, ASCII, hyphens only. Example: `Agent` → `agent`, `大模型的上下文` → `llm-context`, `Skill` → `skill`.

## Workflow

Follow these steps in order. Do not skip.

### Step 1 — Confirm the concept and the language

If the user request names a single concept, proceed. If it's ambiguous (e.g., "讲讲 AI"), ask one short clarifying question: "你想学哪个具体概念？比如 Agent、上下文、Skill？"

Default output language: 简体中文. If the user wrote in English, mirror English.

### Step 2 — Write the Markdown study pack

Create `notes/concepts/<slug>.md` with the **8 mandatory sections** above. Length target: 1500–3000 Chinese characters (this is a study pack, not a snippet). Tone: patient teacher talking to a curious beginner. Use concrete examples, avoid abstract philosophy.

### Step 3 — Build the HTML preview

Create `notes/concepts/<slug>.html` as a single-file HTML. Requirements:
- Self-contained (no external CDN, no external CSS, no JS).
- Light theme (white-ish background, dark text) — matches the user's IDE theme.
- Layout: a left sidebar that mirrors the 8-section TOC, a right main column with the rendered content.
- Use inline CSS in a single `<style>` block.
- Filename: same slug as the Markdown.

### Step 4 — Show the result

After writing both files, present them with `present_files` so the user can preview the HTML and download the Markdown. Do not just say "done" — actually call `present_files` with both absolute paths.

### Step 5 — Quick self-review

Before finishing, check the Markdown against this checklist:
- [ ] All 8 sections present, in the right order
- [ ] Learning objectives start with action verbs
- [ ] At least one everyday-life analogy (NOT another technical concept)
- [ ] At least 3 misconceptions explicitly called out in section 4.4
- [ ] At least 2 application cases (1 real-world, 1 hands-on)
- [ ] At least 2 disambiguation comparisons in section 6
- [ ] 3 self-check questions, each with a hint in `<details>`
- [ ] 3–5 references, each with a "为什么推荐" line; uncertain refs marked `[未验证]`
- [ ] No invented paper titles, fake API names, or fabricated URLs

If any box is unchecked, fix the Markdown before calling `present_files`.

## Style Guide

- Use second-person address ("你") — feels like a 1-on-1 tutoring session.
- Bold **key terms** on first mention.
- Inline code with backticks for tool/parameter names.
- Code blocks (```) only when the example is actual runnable code.
- Prefer short paragraphs (3–5 lines) over long walls of text.
- The 直观类比 must use a real-life situation (cooking, traveling, office work), not another technical concept.

## What This Skill Does NOT Do

- It does not produce comparison tables across multiple concepts (use a comparison skill).
- It does not write or modify code.
- It does not fetch web pages or call external APIs.
- It does not generate images or videos.

If the user's request needs any of the above, mention it and suggest the right tool instead.
