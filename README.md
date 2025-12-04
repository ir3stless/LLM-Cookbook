# LLM Cookbook

> Copy-pasteable patterns, prompts, and mental models for using LLMs in the real world.

This repo is a **reading-first** cookbook.

No big framework, no “install 10 things and run this project” setup.  
Just clear, opinionated notes on how to actually use large language models as they exist **right now**.

I’m building this for myself first, and keeping it public so other people can drop in, skim a few recipes, and walk away smarter.

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
> There are no running Python examples yet, so you don’t need API keys or credits to get value.

---

## What’s inside (for now)

- `cookbook/00-index.md`  
  High-level map of the topics in this repo.

- `cookbook/01-llm-mental-models.md`  
  How to think about LLMs so you don’t fight them.

- `cookbook/02-basic-system-prompt.md`  
  A simple, solid system prompt pattern you can adapt to almost any app.

- `cookbook/_template_recipe.md`  
  The format I use for new “recipes” so this stays consistent.

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
- [ ] A “prompting mistakes I see everywhere” page
- [ ] A simple RAG mental model (no code, just diagrams)
- [ ] One or two domain-specific recipes (e.g. legal assistant, support bot)

Later (when I want code):

- [ ] Add a minimal `examples/` folder with pseudocode
- [ ] Then add real examples that call APIs

---

## Why this exists

Most LLM content is either:

- Super hype / marketing, or  
- 50-page research PDFs nobody actually reads.

This repo is the middle: practical, opinionated, and easy to skim.

If you find something useful or want to suggest a recipe idea, feel free to open an issue or PR.
