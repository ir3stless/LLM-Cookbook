# LLM Cookbook

> Copy-pasteable patterns, prompts, and mental models for using LLMs in the real world.

<p align="center">
  <img alt="Docs only" src="https://img.shields.io/badge/type-docs--only-informational">
  <img alt="Work in progress" src="https://img.shields.io/badge/status-WIP-orange">
</p>

This repo is a **reading-first** cookbook.

No big framework, no “install 10 things and run this project” setup.  
Just clear, opinionated notes on how to actually use large language models as they exist **right now**.

I’m building this for myself first, and keeping it public so other people can drop in, skim a few recipes, and walk away smarter.

---

## Table of contents

- [How to use this repo](#how-to-use-this-repo)
- [What’s inside (for now)](#whats-inside-for-now)
- [Roadmap (small steps)](#roadmap-small-steps)
- [Why this exists](#why-this-exists)

---

## How to use this repo

Think of this like a mini book / notebook:

1. Start with the **index**  
   → [`cookbook/00-index.md`](./cookbook/00-index.md)

2. Read whatever seems interesting:
   - LLM mental models
   - System prompt examples
   - Prompt “recipes” you can copy into your own projects

3. Come back later  
   I’ll be adding new recipes and updating old ones as the LLM ecosystem changes.

> **Note:** Right now this is **docs-only on purpose**.  
> No code, no infra. Just ideas, patterns, and examples you can steal.

---

## What’s inside (for now)

### Cookbook

- [`cookbook/00-index.md`](./cookbook/00-index.md)
  High-level map of the topics in this repo.

- [`cookbook/01-llm-mental-models.md`](./cookbook/01-llm-mental-models.md)
  How to think about LLMs so you don’t fight them.

- [`cookbook/02-basic-system-prompt.md`](./cookbook/02-basic-system-prompt.md)
  A simple, solid system prompt pattern you can adapt to almost any app.
  
- [`cookbook/03-prompt-cheatsheet.md`](./cookbook/03-prompt-cheatsheet.md)
  Quick, copy-pasteable patterns that push an LLM out of “generic advice mode” and into systematic analysis.
  
- [`cookbook/04-task-shaping-and-decomposition.md`](./cookbook/04-task-shaping-and-decomposition.md)
  How to stop asking models to “do everything at once” and instead shape work into smaller, saner steps.

- [`cookbook/05-vibe-coding-with-llms.md`](./cookbook/05-vibe-coding-with-llms.md)
  Build real stuff without feeling “good enough” at code

### Integrations (vendor-specific notes)

These live outside the cookbook so the “core” mental models stay provider-agnostic:

- [`integrations/openai/`](./Integrations/OpenAI/)  
  Notes, patterns, and gotchas for using OpenAI models and APIs.

- [`integrations/lovable/`](./Integrations/Lovable/)  
  Notes and patterns for building / wiring apps with Lovable.

Over time this will grow into sections like:

- Prompting patterns  
- Retrieval & context (RAG)  
- Tools / function calling  
- Evaluation & debugging  
- UX patterns that actually feel good to users  

…but I’m intentionally starting small.

---

## Roadmap (small steps)

Short-term:

- [x] Basic structure + README  
- [x] Index + mental model notes  
- [x] Basic system prompt recipe  
- [x] Prompt cheatsheet page  
- [x] Task shaping & decomposition  
- [ ] A “prompting mistakes I see everywhere” page  
- [ ] A simple RAG mental model (no code, just diagrams)  

---

## Why this exists

Most LLM content is either:

- Super hype / marketing, or  
- 50-page research PDFs nobody actually reads.

This repo is the middle: practical, opinionated, and easy to skim.

If you find something useful or want to suggest a recipe idea, feel free to open an issue or PR.
