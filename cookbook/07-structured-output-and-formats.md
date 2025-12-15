# 07 – Structured Output & Formats  
**Getting results your code (and brain) can actually use**

So far we’ve focused on *what* to ask and *how* models think.

This chapter is about the next step:

> “How do I get output that my code can reliably use,  
> instead of a pretty paragraph I have to manually clean up?”

That’s what **structured output** is about.

---

## 1. Why structure matters

Freeform text is great for reading, terrible for automation.

If you want to:

- save results in a database  
- send them to another tool or API  
- loop over them in code  
- or run them as part of a pipeline

…you want output that has **clear structure**:

- JSON  
- fixed sections (`Summary / Pros / Cons / Next steps`)  
- bullet lists  
- tables

You’re not changing how the model “thinks” (it’s still predicting tokens).  
You’re just giving it a **template to fill** instead of saying “write whatever.”

---

## 2. Mental model: from “essay mode” to “fill-in-the-blanks mode”

Two modes to keep in your head:

- **Essay mode**  
  > “Write a helpful explanation about X.”  
  → Great for reading, messy for code.

- **Fill-in-the-blanks mode**  
  > “Fill in this structure: { field_1: ..., field_2: ..., steps: [...] }”  
  → Great for parsing and automation.

The model is a pattern machine.  
If you show it a structure, it will *try* to keep filling that structure.

Your job is to:

1. Decide what structure you want  
2. Show that structure clearly  
3. Tell it to use **only** that structure

---

## 3. Light-weight structure (no JSON yet)

Let’s start with “human-friendly but consistent” formats.

### a) Sectioned answers

```text
Answer in 3 sections with these headings:

Summary:
Pros:
Cons:

Each section should be 2–4 bullet points.
```

Example output:

```text
Summary:
- ...

Pros:
- ...

Cons:
- ...
```

You can now:

- skim quickly as a human  
- copy sections into other tools  
- pattern-match this structure in later prompts

### b) Steps / checklists

```text
Explain the plan in this structure:

Goal:
- one sentence

Steps:
1. ...
2. ...
3. ...

Risks:
- bullet list
```

This alone makes answers **way** easier to use and reason about.

---

## 4. JSON basics (prompt-only version)

Now let’s move to something your code can parse easily: **JSON**.

### a) Simple JSON classification

```text
Task: classify this message as "spam" or "not_spam".

Output:
Return valid JSON only. No extra text.

Schema:
{
  "label": "spam" | "not_spam",
  "reason": "string"
}

Message:
{{message_text}}
```

Ideal output:

```json
{
  "label": "spam",
  "reason": "classic giveaway spam with too-good-to-be-true offer"
}
```

Key ideas:

- You **define the keys** and allowed values  
- You say “valid JSON only. No extra text.”  
- You avoid mixing human explanations around the JSON

### b) Common JSON failure modes (and fixes)

**1) Extra text around the JSON**

Model returns:

```text
Here is the JSON:

{
  "label": "spam",
  "reason": "..."
}
```

Fix your prompt:

```text
Return ONLY valid JSON.
Do not include any explanation or text before or after the JSON.
```

If it still misbehaves, you can follow up with:

```text
Convert your last answer into valid JSON only, no extra text.
```

**2) Almost-JSON (trailing commas, single quotes)**

If you get something that’s “almost JSON”, you can run a repair step:

```text
Here is some almost-correct JSON:

```json
{{model_output}}
```

Fix it into valid JSON that a standard JSON parser can load.
Return only the corrected JSON.
```

That little “fix my JSON” trick is surprisingly effective.

---

## 5. Thinking in schemas (even without special APIs)

Some APIs support “JSON mode” or “structured outputs” that enforce JSON + schema for you.

Even if you don’t use those yet, it helps to **think in schemas**:

> “What fields do I want? What types should they be?”

Example “task breakdown” schema:

```json
{
  "task": "string",
  "assumptions": ["string"],
  "steps": [
    {
      "id": "string",
      "description": "string",
      "estimate_hours": "number"
    }
  ]
}
```

Prompt:

```text
Break this project into steps using this JSON schema:

{
  "task": "string",
  "assumptions": ["string"],
  "steps": [
    {
      "id": "string",
      "description": "string",
      "estimate_hours": "number"
    }
  ]
}

Return valid JSON only. No extra text.

Project description:
{{your_description}}
```

Even without hard enforcement, giving a schema:

- focuses the model  
- makes outputs more consistent  
- makes it easier to spot mistakes

---

## 6. Other useful formats (beyond JSON)

You don’t always need JSON. Sometimes simple formats are enough.

### a) Key-value lines

```text
Answer in key: value format, one per line:

Goal: ...
Audience: ...
Tone: ...
Length: ...
```

Example:

```text
Goal: explain LLMs to a non-technical founder
Audience: startup founder, no ML background
Tone: casual but smart
Length: ~400 words
```

Very easy to parse by eye or with simple string code.

### b) CSV-ish rows

```text
List up to 5 ideas in CSV format.

Columns:
name, target_user, difficulty_1_to_5

Output only the header + rows.

Example:
name,target_user,difficulty_1_to_5
...
```

Now you can paste straight into a spreadsheet or parse with a CSV reader.

### c) Tables in Markdown

```text
Create a markdown table with 4 columns:

| Option | Who it's for | Main benefit | Biggest drawback |

List 3–5 rows and nothing else.
```

Great for docs, READMEs, and reports.

---

## 7. Chaining: structure as glue between steps

Structured output really shines when you **chain steps**.

Example flow:

1. **First call** – ask for a plan in JSON:

    ```text
    Create a step-by-step plan as JSON:

    {
      "goal": "string",
      "steps": [
        {
          "id": "string",
          "description": "string"
        }
      ]
    }
    ```

2. **Your code** – parses JSON, picks one step:

    ```python
    step = data["steps"][0]["description"]
    ```

3. **Second call** – feed that step back in:

    ```text
    Implement this step in code:

    {{step_description}}

    Use Python 3.11, no external libraries.
    ```

Because the first output was structured, your code can:

- loop over steps  
- track progress  
- store state

This is the bridge from “chat with a model” → “build a system around a model”.

---

## 8. Quick structured-output checklist

When you want cleaner, more usable output, ask yourself:

- [ ] Did I decide on a format? (sections, bullets, JSON, table…)  
- [ ] Did I **show** that format clearly in the prompt?  
- [ ] Did I tell it to return **only** that format (no extra chatter)?  
- [ ] For JSON: did I define the keys and types I care about?  
- [ ] If it produced messy output, did I:
  - [ ] ask it to repair to valid JSON / clean format?
  - [ ] simplify the schema or structure?

If you keep nudging models into **fill-in-the-blanks mode**,  
you’ll spend less time cleaning up their answers—and more time actually building things.
