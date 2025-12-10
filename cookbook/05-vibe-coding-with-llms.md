# 05 – Vibe Coding with LLMs  
**Build real stuff without feeling “good enough” at code**

Imagine we’re sitting in Starbucks and you say:

> “Okay, I kinda get how LLMs think… but how do I actually **build things** with this if I’m not a pro engineer yet?”

That’s what *vibe coding* is for.

Vibe coding = using an LLM as a **super-fast coding buddy**, while you stay in control of what gets built.

---

## 1. The mindset: you are the senior, the model is the junior

Most people flip this:

- They treat the model like a **magic senior engineer**  
- And treat themselves like the clueless one

That’s how you get:

- giant “build my whole app” prompts  
- broken code  
- lots of confusion

Better mental model:

> The LLM is a **very fast junior dev**.  
> You are the one with taste and final say.

The junior:

- writes first drafts super fast  
- knows lots of syntax and libraries  
- will confidently make things up if you’re vague

Your job:

- describe the feature  
- keep the scope small  
- run the code  
- feed back errors  
- decide when it’s “good enough”

---

## 2. The basic vibe-coding loop

Here’s the loop you’ll run over and over:

1. **Describe what you want in plain English**
2. **Ask for a tiny plan**
3. **Have it build the smallest useful version**
4. **Run it**
5. **Paste errors or weird behavior back in**
6. **Repeat until it works, then polish**

That’s it. No magic. Just a tight feedback loop.

---

## 3. Give the model a little context

Before you ask for code, give it a “stage” to stand on:

- **Language & stack**  
  - “Python 3.11, no external libraries if possible”  
  - “Node 18 + Express”  
  - “React + TypeScript, Vite”

- **Where this code lives**  
  - “This is a new standalone script.”  
  - “This is a React component.”  
  - “This is part of my FastAPI backend.”

- **What success looks like**  
  - “Success = script prints a summary to the console.”  
  - “Success = button click sends a request and shows a message.”

Example opener:

```text
You are my coding partner.

Stack:
- Language: Python 3.11
- Environment: simple script, run with `python main.py`

Success:
- Read a CSV of transactions
- Print total amount per category to the console
```

Now the model knows what world it’s in.

---

## 4. Pattern: “smallest useful version”

This one saves you from overengineering.

Instead of:

> “Build a full analytics dashboard…”

Try:

```text
I want to build this feature: {{plain_english_description}}.

First, propose the **smallest useful version** we could build in a single file.

Tell me:
- what it does
- what it explicitly does NOT do yet
- the filename you’d use
```

Then, after it proposes something reasonable:

```text
Great. Now write that file.
```

You’ve just kept the scope tiny on purpose.

---

## 5. Use errors as fuel, not shame

Your script will break. That’s normal.

When it does:

1. Copy the **error message**  
2. Copy the **relevant code snippet**  
3. Paste both into the chat

Example prompt:

```text
I ran `python main.py` and got this error:

{{error_message}}

Here is the part of the code that seems related:

```python
{{code_snippet}}
```

What is the most likely cause, and what is the **smallest change** I should make to fix it?
Show me the fixed snippet only.
```

You’re telling it:

- “Don’t rewrite everything”  
- “Just fix the specific problem”

That’s exactly how you’d talk to a junior dev.

---
```
## 6. Make it explain the code back to you

Don’t just accept code you don’t understand.

Ask:

```text
Explain this code to me like I’m new to {{language}}.
```
then paste your code (you can add language too, but most LLM's can recognize the language)
```
{{language}}
{{code_snippet}}
```

Walk through it step by step:
- what each part does
- how the data flows
- any edge cases I should worry about

This turns “mysterious blob of code” into something you can reason about.

Bonus: if the explanation sounds wrong or fuzzy, that’s a red flag about the code too.

---

## 7. Refactor without breaking it

Once it works, you can ask for a clean-up pass:

```text
Refactor this code to make it easier to read and maintain,
but do **not** change what it does.

```{{language}}
{{code_snippet}}
```

Keep:
- the same function names
- the same inputs and outputs

After you show the new version, explain briefly what you improved and why.

Now your tiny feature is both working **and** nicer to live with.

---

## 8. Tiny end-to-end example

Let’s do a super small example.

> Goal: Python script that reads `transactions.csv` and prints total amount per category.

**Step 1 – Describe + plan**

_FYI: you can also upload `transactions.csv` to most LLM's and that can also help them write the code better_

```text
You are my coding partner.

Stack:
- Python 3.11
- Standalone script

Goal:
- Read a file called `transactions.csv`
- It has columns: date, category, amount
- Print the total amount per category

First, propose the smallest useful version of this script in one file.
```

You get a short plan back.

**Step 2 – Generate**


```text
Great. Now write `transactions_summary.py` following that plan,
using only the built-in `csv` module.
```

Save the file, run:

```bash
python transactions_summary.py
```

**Step 3 – Debug**

If you get an error, feed it back:

```text
I ran the script and got this error:

{{error_message}}

Here’s the code around that line:

```python
{{snippet}}
```

What’s the smallest edit I should make to fix this?
```

Patch → run again → repeat.
```
**Step 4 – Polish**

Once it works:

```text
Now polish this script:
- improve variable names
- handle the case where the file doesn’t exist
- add a short comment at the top explaining what it does

Show me the full final file.
```

Congrats, you just vibe-coded a real, useful script.

---

## 9. Vibe-coding checklist

When you sit down to build with an LLM, ask yourself:

- [ ] Did I tell it which language / stack I’m using?
- [ ] Did I describe the feature in plain English?
- [ ] Did I ask for the **smallest useful version**, not “the whole app”?
- [ ] Am I building one file or function at a time?
- [ ] Did I actually run the code?
- [ ] When it broke, did I paste the **error + snippet**, and ask for the smallest fix?
- [ ] Did I ask it to explain the code back to me?
- [ ] Did I refactor once it worked?

If you follow that, you’re not “cheating” at coding.

You’re doing what good engineers do:

> Keep the scope small, get feedback fast, and use tools (including LLMs) to move quicker without giving up control.
