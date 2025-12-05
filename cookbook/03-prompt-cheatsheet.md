# Prompt Cheatsheet: Make the Model Actually Think

Quick, copy-pasteable patterns that push an LLM out of “generic advice mode” and into **systematic analysis**.

For deeper explanations, see:

- [LLM mental models](./01-llm-mental-models.md)  
- [Basic System Prompt](./02-basic-system-prompt.md)  

---

## 🧭 Decision & Planning Prompts

**Act like you’re solving this for yourself**  
> *“Act like you’re solving this for yourself. Here’s my situation: … What would you do and why?”*  
Use when you want **real recommendations**, not “it depends.”

**What’s the pattern here?**  
> *“Here are my notes / decisions: … What’s the pattern here? What do I keep optimizing for?”*  
Use to extract **themes and structure** from messy info.

**Zoom out — what’s the bigger picture?**  
> *“Zoom out — what’s the bigger picture of what I’m trying to do here, and what are 2–3 other ways to get there?”*  
Use to check if you’re solving the **right problem**, not just the visible one.

**What would [expert] say about this?**  
> *“What would a [role: therapist / Staff engineer / investor / etc.] say about this plan: … ?”*  
Use to get **domain-lensed feedback** instead of generic advice.

**Now make it actionable**  
> *“Now make it actionable: turn this into a concrete plan with specific steps for the next 7 days.”*  
Use whenever the answer feels right but **too abstract**.

---

## ⚠️ Risk & Critical Thinking Prompts

**How would this backfire?**  
> *“Here’s my plan: … How could this backfire? List specific ways this could go wrong for me or others.”*  
Use to force **worst-case thinking** and find failure modes.

**Steelman my opponent’s argument**  
> *“I believe: … Steelman my opponent’s argument — what are the strongest points someone smart would make against me?”*  
Use to uncover **strong counterarguments** and avoid strawmen.

---

## 🧠 Self-Awareness & Meta Prompts

**What am I optimizing for without realizing it?**  
> *“Here are my last few decisions: … What am I optimizing for without realizing it?”*  
Use to surface **hidden priorities** and goal mismatches.

**What assumptions am I making?**  
> *“Given this plan: … what assumptions am I making that could be wrong?”*  
Use to expose **implicit assumptions** before they bite you.

---

## 🧩 Stackable “Combo” Prompts

You can chain patterns together for deeper analysis.

### Decision combo (expert + risk + meta + action)

```text
Act like you’re solving this for yourself.
What would a {{expert_role}} say about my plan to {{goal}}?
How would this backfire, and what am I optimizing for without realizing it?
Now make it actionable with clear steps for the next 7 days.
```

### Clarity combo (pattern + zoom out + assumptions)

```text
Here are my notes: {{dump}}.
What’s the pattern here?
Zoom out — what’s the bigger picture of what I’m trying to do?
What assumptions am I making that could be wrong?
```

### Argument combo (steelman + action)

```text
I believe: {{position}}.
Steelman my opponent’s argument.
Now make it actionable: based on that, what should I change in my plan, if anything?
```

---

## ✅ How to Use This Cheatsheet

- Treat each block as **LEGO for prompts** — copy, tweak, and stack them.  
- When answers feel generic, add **one of these patterns** as the first line.  
- When answers feel right but vague, append: **“Now make it actionable.”**
