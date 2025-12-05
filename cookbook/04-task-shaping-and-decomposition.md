# 04 – Task Shaping & Decomposition  
**Stop Asking Models to “Do Everything at Once”**

This page is about **turning messy, oversized asks into tasks LLMs can actually nail**.

From earlier chapters:

- LLMs are **P(next token | context)** machines.  
- They imitate **patterns** and respond to the **job description** you give them.  
- If the job is “do everything, perfectly, in one shot”… they usually flail.

> 💡 **Task shaping** = taking a vague, huge, or underspecified request and turning it into  
> a **clear, bounded, step-by-step job** the model can reliably perform.

You’ll use this constantly when vibe coding, planning, writing, or designing systems around LLMs.

---

## 🧩 1. Symptoms of an under-shaped task

You’re probably under-shaping when you see:

- **Generic soup** – “Here are 12 random tips…” instead of what you actually need.  
- **Overwhelm** – 3 pages of text when you wanted something tiny and concrete.  
- **Incoherence** – it mixes strategy, implementation, and copy into one blob.  
- **Missed constraints** – it ignores your tech stack, audience, or format.  
- **You feel lost** – you don’t know what to do next after reading the answer.

Underlying issue:

> You asked it to solve **too many problems at once** with too much freedom.

Task shaping fixes that by being specific about **scope** and **output**.

---

## 🎯 2. Principle: one clear job per prompt

Strong default:

> **Each prompt should give the model one main job, plus a clear output format.**

**Weak prompt**

> “Help me build my SaaS, write the copy, design the architecture, and give me a marketing plan.”

**Better shaped**

1. “Help me define the *core user* and *one-line promise* for this SaaS.”  
2. “Given that, outline the main pages and core features.”  
3. “Write hero copy and 3 CTA variants for the landing page.”  
4. “Propose a minimal architecture for v0 using \[stack\].”

Same overall goal, but now each step is:

- smaller  
- evaluable (“is this good?”)  
- easy to iterate on and debug

---

## 🔄 3. Core pattern: Clarify → Outline → Fill → Polish

A simple pipeline you can reuse for almost everything:

1. **Clarify** – make the problem sharp.  
2. **Outline** – get the structure.  
3. **Fill** – generate content/code per section.  
4. **Polish** – refine, format, or make actionable.

You can run this loop for landing pages, study plans, code features, product ideas, etc.

---

### 🗣️ 3.1 Clarify – “Ask me questions first”

Ask the model to **ask you questions** or restate the goal before doing anything.

```text
You are helping me with {{goal}}.

First, restate my goal in your own words.
Then ask up to 5 clarifying questions that would help you give a precise, useful answer.
Wait for my answers before continuing.
```

This stops the “generic advice” spiral and forces **shared understanding**.

---

### 🧱 3.2 Outline – “Structure first, no details yet”

Once the goal is clear, ask **just for structure**, not full content.

**Example – landing page**

```text
Given this product: {{description}},
outline the sections of a high-converting landing page.

Just give:
- section names
- 1-line purpose for each
- suggested order
```

**Example – code feature**

```text
Given this feature: {{feature_description}},
outline the steps to implement it in {{tech_stack}}.

Return a numbered list of steps and which file(s) each step likely touches.
```

You now have a **map**. Every later prompt can target one part at a time.

---

### ✍️ 3.3 Fill – “Do one piece at a time”

Now fill in **one part at a time** instead of everything.

**Example – copy**

```text
Using this outline: {{outline}},
write only the hero section copy:
- headline
- subheadline
- 3 short bullet benefits
- 1 CTA button label

Keep it under 80 words total.
```

**Example – code**

```text
Using this plan: {{implementation_plan}},
implement just Step 1 as code.

Do not touch later steps yet.
Explain briefly what you’re doing in comments.
```

Narrowing to one chunk makes it:

- easy to review and edit  
- easier to localize errors  
- trivial to swap or redo specific pieces

---

### ✨ 3.4 Polish – “Refine once the rough shape exists”

When the rough draft is there, **refine**:

- tighten language  
- add or adjust constraints  
- make it actionable  
- tweak voice/tone/stack

**Example – copy polish**

```text
Here is the draft hero section: {{draft}}.

Polish it:
- keep the same meaning
- make it punchier and clearer for beginners
- keep it under 60 words
- remove buzzwords
```

**Example – code polish**

```text
Here is the implementation of Step 1: {{code}}.

Polish it:
- improve naming
- add docstrings
- suggest 2 simple tests I could write
```

Polish turns “it works” into “this is nice to use and maintain.”

---

## 🧰 4. Task-shaping patterns you can copy

Drop these straight into prompts as needed.

---

### 4.1 “Ask me questions first”

Use when your own spec is fuzzy.

```text
You’re helping me with {{goal}}.

Before you answer, ask me up to 5 clarifying questions that will help you give a precise, useful answer.
After I respond, propose a plan.
```

Why it works: the model switches from **answering immediately** to **information gathering first**.

---

### 4.2 “Outline first, no details yet”

Use when the problem feels big and amorphous.

```text
I want to {{goal}}.

First, outline the high-level steps to get there.
Do not add details or content yet.
Return a numbered list of 5–10 steps max.
```

Then you can follow with:

```text
Now expand just Step 1 into a detailed checklist.
```

---

### 4.3 “Smallest useful version”

Use when you’re vibe coding and don’t want over-engineering.

```text
Given this feature: {{feature_description}},
describe the smallest useful version we could build in an evening.

List:
- what it does
- what it explicitly does NOT do
- what files or components we’d need
```

Then:

```text
Now generate code for that smallest version only.
```

You’ve shaped the task into **MVP**, not “perfect product.”

---

### 4.4 “Constrain the output”

Use when answers are too long or too vague.

```text
Answer in this structure only:
1) one-sentence summary,
2) 3 bullet points of pros/cons,
3) 3 concrete next steps.

Keep the entire response under 200 words.
```

You’re shaping both **task** and **format** so you can scan and act.

---

## ☕ 5. Coffee-shop example: shaping “Help me learn Python”

Bad prompt:

> “Help me learn Python.”

Better shaped as a mini-flow:

1. **Clarify**

   ```text
   I want to learn Python.

   First, ask me up to 5 questions about:
   - my current skill level
   - how much time I have weekly
   - what I want to use Python for
   ```

2. **Outline**

   ```text
   Based on my answers, outline a 6-week learning plan:
   - weekly theme
   - key concepts
   - 1–2 suggested resources per week
   ```

3. **Fill (week 1 only)**

   ```text
   Now flesh out week 1 only:
   - specific daily tasks (max 30–45 minutes each)
   - 2–3 small exercises
   ```

4. **Polish**

   ```text
   Make this week 1 plan more concrete:
   - add example exercise prompts
   - clarify what “done” looks like for each day
   ```

You’ve turned a vague life goal into a **shaped, actionable plan** you can actually follow.

---

## 🧠 6. How a ML engineer would describe what you’re doing

From a machine learning engineer’s perspective, task shaping is basically:

- **Keeping the model on-distribution**  
  You’re asking for realistic chunks of text it saw in training (plans, outlines, code snippets) instead of one impossible “do everything” blob.

- **Decomposing a hard problem into tractable subproblems**  
  Just like ML pipelines do: detect intent → retrieve docs → generate answer → post-process.

- **Reducing ambiguity and degrees of freedom**  
  Smaller, well-scoped steps with clear formats give the model **less room to hallucinate**.

- **Improving debuggability**  
  If Step 3 is bad, you fix Step 3. You don’t throw away the whole interaction.

In plain English:  
> You’re treating the model like a super-fast junior collaborator and giving it work the way a good senior engineer would – one clear step at a time.

---

## ✅ 7. Quick task-shaping checklist

When your prompt is giving you meh output, ask:

- **Is this trying to do more than one job?**  
  → If yes, split it into separate prompts.

- **Have we clarified the goal?**  
  → If not, add a “ask me questions first” step.

- **Do we have an outline before details?**  
  → If not, ask for structure only.

- **Am I asking for the smallest useful version?**  
  → If not, explicitly constrain scope/MVP.

- **Did I specify the output format and length?**  
  → If not, tell it exactly how to respond.

If you apply just these steps, you’ll go from:

> “Why is it giving me vibes?”

to:

> “This thing is actually collaborating with me.”

in almost every domain you use LLMs for.
