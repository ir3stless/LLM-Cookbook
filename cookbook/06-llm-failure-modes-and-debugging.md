# 06 – When LLMs Go Wrong  
**Failure modes & debugging without losing your mind**

At some point, every model will:

- make stuff up  
- ignore your instructions  
- dump a wall of text  
- refuse for no good reason  
- or just give you a weird answer

This page is your **“oh no, why did it do that?”** guide.

Think of it as a debugging flow:

> When the model is being weird,  
> walk through these failure modes one by one.

You don’t need deep math. Just a few mental switches.

---

## 1. Mental model refresher (why this happens at all)

From earlier chapters:

- An LLM is basically:  
  **“Pick the next token based on all previous tokens (context).”**
- It only “sees” what’s in the **current context window**
- It’s insanely good at **imitating patterns** it has seen before

So most failures are really:

1. Context problems – what you fed it  
2. Task problems – what you asked for  
3. Capability problems – what it’s literally able to do

We’ll go through each with examples + prompts you can copy.

---

## 2. Hallucinations: confident but wrong

### What it looks like

- Fake case law / statutes / citations  
- APIs or functions that don’t exist  
- Made-up URLs, dates, or numbers

### Why it happens

The model is trained to produce **plausible** text, not **guaranteed-true** text.

If your prompt sounds like:

> “There must be a specific answer, give it to me.”

…it will often guess, even when it doesn’t really “know”.

### How to debug it

**a) Change the job description**

Bad:

```text
Give me the exact statute and case law that proves X.
```

Better:

```text
Help me brainstorm possible statutes or case law that might relate to X.

If you’re not sure about a specific citation, say:
"I’m not sure about the exact citation here."

Do not make up citations or numbers.
```

**b) Separate facts from guesses**

```text
Answer in three sections:

1) Facts you are confident about (high probability)
2) Things you are inferring or guessing
3) Things you cannot know from this context
```

**c) Use the model as a research assistant, not a source of truth**

- Let it **suggest** places to look  
- You still **verify** in official sources (docs, laws, docs, etc.)

---

## 3. Ignoring instructions or formats

### What it looks like

- You ask for JSON → you get a paragraph + half-broken JSON  
- You say “3 bullets” → it gives you 9  
- You say “answer only yes/no” → it explains itself anyway

### Why it happens

Your prompt may be:

- doing too many things at once  
- burying the format request in a long story  
- conflicting (“be creative, but strict JSON only”)

### How to debug it

**a) Make the format first-class**

```text
Task: classify the message.

Output:
Return valid JSON only. No extra text.

Schema:
{
  "label": "spam" | "not_spam",
  "reason": "string"
}

Message:
{{message}}
```

**b) Provide a micro-example**

```text
Follow this format exactly.

Example:

Input: "Win a free iPhone now!"
Output:
{
  "label": "spam",
  "reason": "classic giveaway spam"
}

Now do the same for this message:
{{message}}
```

**c) Remove fluff**

Move from:

> “Hey, could you maybe help me with this thing? I’m trying to…”

To something like:

```text
Classify this email as "spam" or "not_spam".

Output: just the label on its own line.
Email:
{{email}}
```

You’re being clear, not rude.

---

## 4. Generic, fluffy, or “Pinterest board” answers

### What it looks like

- “Here are 10 generic tips…”  
- You feel like it could have been written for literally anyone

### Why it happens

Your prompt + context is too vague, so it falls back to the **“average internet advice”** pattern.

### How to debug it

**a) Make your situation real**

Instead of:

```text
Give me advice on starting a business.
```

Try:

```text
Here’s my real situation:
- Age: 18
- Location: Florida
- Skills: {{skills}}
- Time available: {{hours}} hours/week
- I care most about: {{goals}}

Given that, what are 3 realistic next steps for the next 30 days?
```

**b) Add constraints**

```text
Only suggest actions that:
- cost $0
- I can start this week
- take under 5 hours total

Avoid generic advice like "stay motivated" or "believe in yourself".
Be concrete.
```

**c) Force a “coach” voice instead of “blogger” voice**

```text
Act like my coach, not a blogger.

Given this situation: {{context}},
what are 3 specific moves I should make in the next 7 days?

Each move should be:
- 1 sentence
- something I can literally do on a calendar
```

---

## 5. Overly long or rambling answers

### What it looks like

- 5 screens of text when you wanted 5 lines  
- The one thing you needed is buried somewhere in the middle

### Why it happens

You didn’t tell it:

- how long to be  
- what structure to use  
- what to prioritize

So it “overhelps”.

### How to debug it

**a) Set a word limit**

```text
Keep the entire answer under 150 words.
```

**b) Lock in a simple structure**

```text
Answer in this exact structure:
1) one-sentence summary
2) 3 bullet points
3) 1 concrete next step

Nothing else.
```

**c) Ask for a TL;DR after the fact**

If you already got a huge answer:

```text
Now summarize your answer in 5 bullet points,
each under 120 characters.
```

You can train the model into being concise.

---

## 6. Repetition or “stuck in a loop”

### What it looks like

- Repeating the same phrase  
- Listing very similar items over and over  
- “Continue” prompts that keep drifting

### Why it happens

- No clear stopping condition  
- Long generations with low-quality patterns  
- “Continue” prompts on a messy chunk of text

### How to debug it

**a) Set a hard stop**

```text
List up to 5 ideas and then stop.
Do not go past 5.
```

**b) Rewrite instead of continue**

Instead of:

```text
Continue.
```

Try:

```text
Rewrite the answer above to:
- remove repetition
- keep only the 5 best points
- make sure each point is clearly different
```

If you’re using the API directly, you can also:

- lower `max_tokens`
- adjust temperature/top_p
- or regenerate from a cleaner prompt

---

## 7. Unnecessary refusals (“I can’t help with that”)

### What it looks like

- You ask something harmless  
- It responds like you asked for legal/medical/financial advice  
- It refuses even when you’re just asking for structure or examples

### Why it happens

Your prompt *sounds* like a sensitive domain (law, medicine, finance, safety), so the safety layer plays it safe.

### How to debug it (without being sketchy)

**a) Clarify intent**

```text
I'm not asking for legal advice, just structure.

Explain the general sections of a motion to dismiss.
Keep it educational, not jurisdiction-specific.
```

**b) Reframe as “teach me” instead of “tell me what to do”**

```text
Teach this concept at a high level for learning purposes,
not as personal advice:

{{topic}}
```

**c) Stay inside policy**

Use the model to:

- explain patterns  
- draft templates  
- suggest questions to ask a professional

Don’t try to turn it into a doctor or lawyer.

---

## 8. Capability mismatch: asking for impossible things

Sometimes the model fails because you’re asking for something it **cannot** do, no matter how good your prompt is.

### Examples

- “What did this website say yesterday?” (no browsing or past logs)  
- “Run this code and give me the real output on my machine.” (it can’t see your environment)  
- “Give me today’s exact stock price.” (no real-time data unless tools are connected)  
- “Summarize this PDF” (but you never pasted the PDF text or used a file tool)

### How to debug it

**a) Ask: did I actually give it the data?**

If not, you might need to:

- paste the relevant text  
- upload the file (if the tool supports it)  
- call an external API yourself  
- store and retrieve context using your own code + vector search

**b) Ask the model what it can and can’t see**

```text
Before you answer, tell me:
- what information you actually have access to in this chat
- what you do NOT know or cannot see
```

You’ll often realize: “Oh, right, it has no way to know that.”

**c) Use it to design the system, not be the system**

Let it:

- design a scraper  
- design a RAG workflow  
- help you structure results

But remember: **you** or your code actually do the fetching and executing.

---

## 9. A simple debugging checklist

When an answer feels wrong, walk through this list:

1. **Is the task too big or vague?**  
   - Can I split it into smaller steps?  
   - Can I ask for an outline first?

2. **Is the context clear and complete?**  
   - Did I give it the actual code/text/data?  
   - Or am I expecting it to guess?

3. **Did I show it the pattern I want?**  
   - Example input/output?  
   - Desired structure (JSON, bullets, table)?

4. **Did I set boundaries on format and length?**  
   - Word limit?  
   - Required sections?

5. **Am I asking it for something it cannot know or do?**  
   - Real-time info?  
   - External files it’s never seen?

6. **If it hallucinated, did I:**
   - Ask it to separate facts vs guesses?  
   - Explicitly tell it not to invent citations or numbers?

7. **If the code is broken, did I:**
   - Actually run it?  
   - Paste the exact error + relevant snippet?  
   - Ask for the smallest fix?

---

## print("a quick recap")

- **Hallucinations** → out-of-distribution + no ground truth  
- **Ignoring format** → under-specified objective / conflicting instructions  
- **Generic answers** → weak or low-information context  
- **Rambling** → no output length/structure constraints  
- **Repetition** → decoding issues + poor stopping strategy  
- **Over-refusals** → conservative safety heuristics  
- **Capability mismatch** → confusing model memory with tools/data

But you don’t *need* those words to debug.  
The practical prompts on this page are enough to get you from:

> “Why is it like this?”

to:

> “Okay, I know which knob to turn.”
