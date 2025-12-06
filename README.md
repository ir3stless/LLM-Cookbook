# LLM Cookbook

> 📚 Copy-pasteable patterns, prompts, and mental models for using LLMs in the real world.

This repo is a **reading-first** cookbook.

No big framework, no “install 10 things and run this project” setup.  
Just clear, opinionated notes on how to actually use large language models as they exist **right now**.

I’m building this for myself first, and keeping it public so other people can drop in, skim a few recipes, and walk away smarter.

---

## 🧭 How to use this repo

Think of this like a mini book / notebook:

1. Start with the **index**  
   → [`cookbook/00-index.md`](./cookbook/00-index.md)

2. Read whatever seems interesting:
   - LLM mental models  
   - System prompt examples  
   - Prompt “recipes” you can copy into your own projects  
   - Vibe-coding patterns for building with LLMs

3. When you want provider/tool-specific stuff, check **integrations**:
   - [`Integrations/OpenAI/`](./Integrations/OpenAI/)
   - [`Integrations/Lovable/`](./Integrations/Lovable/)

4. Come back later  
   I’ll be adding new recipes and updating old ones as the LLM ecosystem changes.

> **Note:** This is intentionally **docs-only** right now.

---

## 📂 What’s inside (for now)

### 🧠 Core cookbook

- [`cookbook/00-index.md`](./cookbook/00-index.md)  
  High-level map of the topics in this repo.

- [`cookbook/01-llm-mental-models.md`](./cookbook/01-llm-mental-models.md)  
  How to think about LLMs so you don’t fight them.

- [`cookbook/02-basic-system-prompt.md`](./cookbook/02-basic-system-prompt.md)  
  A simple, solid system prompt pattern you can adapt to almost any app.

- [`cookbook/03-prompt-cheatsheet.md`](./cookbook/03-prompt-cheatsheet.md)  
  Quick, copy-pasteable patterns that push an LLM out of “generic advice mode” and into systematic analysis.

- [`cookbook/04-task-shaping-and-decomposition.md`](./cookbook/04-task-shaping-and-decomposition.md)  
  Stop asking models to “do everything at once” and break work into clear, solvable steps.

### 🔌 Integrations

Vendor/tool-specific notes live under `integrations/`:

- [`integrations/OpenAI/`](./integrations/OpenAI/)  
  OpenAI-specific notes and guides (APIs, models, ChatGPT workflows).

- [`integrations/Lovable/`](./integrations/Lovable/)  
  Lovable.dev-specific notes and workflows.

---

## 🗺️ Roadmap (small steps)

Short-term:

- [x] Basic structure + README  
- [x] Index + mental model notes  
- [x] Basic system prompt recipe  
- [x] Prompt cheatsheet  
- [x] Task shaping & decomposition  
- [x] Vibe coding with LLMs  
- [ ] A “prompting mistakes I see everywhere” page  
- [ ] A simple RAG mental model (no code, just diagrams)  
- [ ] A basic OpenAI integration overview  
- [ ] A basic Lovable + LLMs overview  

---

## 🤔 Why this exists

Most LLM content is either:

- Super hype / marketing, or  
- 50-page research PDFs nobody actually reads.

This repo is the middle: practical, opinionated, and easy to skim.

If you find something useful or want to suggest a recipe idea, feel free to open an issue or PR.
