# Basic System Prompt: Stop Getting Generic Answers

This page is about **how to talk to an LLM so it stops giving you generic advice**.

From the previous chapter: an LLM is a giant conditional autocomplete:  
it continues patterns in the **context window**.

If you just say “Help me with X,” the strongest pattern in its training data is:
> “Here’s some safe, generic, vaguely helpful advice.”

The prompts on this page do one thing:

> They give the model a **more specific job** so it switches from generic advice mode to **systematic analysis mode**.

You can use them as:

- system prompts (if you control the system message), or  
- first-line instructions in your user prompt (for ChatGPT-style chats).

---

## 1. “Act like you’re solving this for yourself”

**Pattern:**

> “Act like you’re solving this for yourself.”

**What it does**

This tells the model to **optimize for its own outcome**, not just “be helpful.”  
In practice, that pushes it toward:

- more thorough reasoning  
- more concrete trade-offs  
- fewer vague “it depends” answers

It’s like saying: *“If you were actually in my shoes, what would you do?”*

**Example**

> *Act like you’re solving this for yourself. You’re a junior engineer deciding between two job offers. Here are the details… Which would you pick and why?*

Use this when:

- you’re deciding between options  
- you want **specific recommendations**, not a menu of possibilities

---

## 2. “What’s the pattern here?”

**Pattern:**

> “What’s the pattern here?”

**What it does**

This prompt tells the model to **summarize and connect dots**, not just repeat details.  
It’s great for:

- spotting trends in your decisions  
- extracting themes from messy notes  
- noticing structure in “random” data points

**Example**

> *Here are notes from my last 10 job decisions and side projects: [paste]. What’s the pattern here? What do I keep optimizing for?*

Use this when:

- you have **a pile of info** and want structure  
- you suspect there’s a theme but can’t see it

---

## 3. “How would this backfire?”

**Pattern:**

> “How would this backfire?”

**What it does**

Most default answers are **cheerleader mode**: pros, enthusiasm, encouragement.

This flips it to **critic mode**:

- downside scenarios  
- edge cases  
- failure modes and risks

You go from “sounds good” to “here’s how this can explode if I’m not careful.”

**Example**

> *I’m planning to launch this referral program: [outline]. How could this backfire? List specific ways this could go wrong for me or my users.*

Use this when:

- you’re about to ship something important  
- you want to **stress-test** a plan before committing

---

## 4. “Zoom out — what’s the bigger picture?”

**Pattern:**

> “Zoom out — what’s the bigger picture?”

**What it does**

LLMs often fixate on the **local question** you gave them.

This prompt asks for a **higher-level frame**:

- what goal this actually serves  
- what problem sits *above* the one you named  
- alternative ways to get the same outcome

It’s how “I want to learn Python” becomes “I want to solve problems efficiently” with multiple routes.

**Example**

> *I said I want to learn Python and picked this roadmap: [plan]. Zoom out — what’s the bigger picture of what I’m trying to do, and what are 2–3 other ways to get there?*

Use this when:

- you’re not sure if you’re asking the right question  
- you want to sanity-check whether you’re solving the **right problem**

---

## 5. “What would [expert] say about this?”

**Pattern:**

> “What would [expert] say about this?”

Fill in any relevant expert:

- “What would a therapist say about this relationship?”  
- “What would a senior ML engineer say about this architecture?”  
- “What would a skeptical investor say about this deck?”

**What it does**

You’re telling the model to **activate a specific slice of its training data**:

- expert tone  
- expert concerns  
- expert-level caveats and priorities

Instead of generic life advice, you get something closer to a **specialist’s lens**.

**Example**

> *Act like you’re solving this for yourself. What would a Staff-level backend engineer say about my plan for this API design: [details]?*

Use this when:

- you want **domain-specific feedback**  
- you don’t have easy access to a real expert yet

---

## 6. “Now make it actionable”

**Pattern:**

> “Now make it actionable.”

Often used as a **follow-up line**.

**What it does**

LLMs love abstractions: “be more confident,” “improve communication,” “learn the fundamentals.”

This phrase forces a conversion to:

- concrete steps  
- timelines  
- checklists / scripts / templates

Think: “Tell me exactly what to do Monday morning.”

**Example**

> *That advice was helpful. Now make it actionable: give me a 7-day plan with specific tasks I can check off in under 30 minutes each day.*

Use this when:

- the answer feels right, but too vague  
- you’re stuck at “I agree” instead of “I know what to do next”

---

## 7. “Steelman my opponent’s argument”

**Pattern:**

> “Steelman my opponent’s argument.”

**What it does**

Instead of a **strawman** (weak version of the other side), steelmanning asks:

> “What’s the strongest argument *against* my position?”

This forces the model to:

- treat the opposing side with respect  
- find the best reasons you might be wrong  
- show trade-offs clearly

You either update your view or walk away with stronger arguments.

**Example**

> *I believe [your position]. Steelman my opponent’s argument: what are the strongest points someone smart would make against me?*

Use this when:

- you’re making a big decision and want to avoid blind spots  
- you’re preparing for a debate, negotiation, or investor Q&A

---

## 8. “What am I optimizing for without realizing it?”

**Pattern:**

> “What am I optimizing for without realizing it?”

**What it does**

This prompt asks the model to analyze your **implied objective**:

- “Given your choices, what do they reveal about what you really care about?”  
- “You say X, but your actions suggest Y or Z.”

This can surface:

- hidden priorities (status, comfort, freedom, learning)  
- misalignment between stated goals and actual behavior

**Example**

> *Here are my last 8 big decisions in work and life: [list]. What am I optimizing for without realizing it?*

Use this when:

- you feel stuck or conflicted  
- your outcomes don’t match your stated goals

---

## 9. Why these prompts work (tying back to mental models)

From the “How LLMs Think” chapter:

- Models are **P(next token | context)** machines.  
- They imitate **patterns in the prompt**.  
- They don’t “care” about you, but they respond to the **job description** you give them.

These prompts work because they **change the job description**:

- “Act like you’re solving this for yourself” → optimize as if the outcome affects *it*.  
- “How would this backfire?” → switch to worst-case / risk analysis patterns.  
- “Zoom out” → switch from local problem to high-level framing patterns.  
- “What would [expert] say?” → activate expert-like text patterns.  
- “Now make it actionable” → convert abstract patterns to step-by-step ones.  
- “Steelman…” → activate strongest-counterargument patterns.  
- “What am I optimizing for…?” → activate meta-analysis patterns.

You’re still using the same underlying model. You’re just pointing it at **different regions of its behavior**.

---

## 10. A simple “stack combo” you can reuse

Here’s a combined prompt you can paste into a system message or the top of your user message when you’re making a serious decision:

> *Act like you’re solving this for yourself.  
> What would a [relevant expert] say about my plan to [goal]?  
> How would this backfire, and what am I optimizing for without realizing it?  
> Now make it actionable.*

You can parameterize it:

```text
Act like you’re solving this for yourself.
What would a {{expert_role}} say about my plan to {{goal}}?
How would this backfire, and what am I optimizing for without realizing it?
Now make it actionable with clear next steps for the next 7 days.
