# How LLMs Think (Mental Models for the Inside)

This page is about what’s going on **inside** an LLM – as close as we can get to how it “thinks” without turning this into a research paper.

Not product, not logging, not pricing. Just:

- What the model is doing mathematically  
- How context shapes its behavior  
- Why examples and wording matter so much  
- Why the same prompt can give different answers  

---

## If you’re totally new, read this first

You can get pretty far with three mental models:

- **Autocomplete on steroids**  
  An LLM is a giant conditional autocomplete that does:  
  **“Given this text so far, what token probably comes next?”**

- **Limited working memory**  
  It only “sees” the text in its **context window** (up to its max token limit). Anything outside that is invisible.

- **Guided randomness**  
  Each next token is a weighted dice roll. Settings like **temperature** decide how wild that roll is.

The rest of this page just fills in those three ideas.

---

## 1. Big picture: what happens to your text?

Very high level, the pipeline looks like this:

```text
Text → tokens → vectors → transformer layers → probabilities → sampled tokens
```

- **Tokens** – your text is split into small pieces (subwords, punctuation, etc.).  
- **Vectors** – each token becomes a high-dimensional vector (an embedding).  
- **Transformer layers** – stacks of attention + feed-forward layers mix information across tokens.  
- **Probabilities** – for the next token, the model outputs a probability for every possible token in its vocabulary.  
- **Sampling** – it then *samples* one token, appends it to the text, and repeats the process.

Everything in this page is just different ways of understanding that loop.

---

## 2. The core idea: P(next token | context)

At the lowest level, an LLM does one thing:

> It repeatedly picks the next token based on the previous tokens.

In math-ish terms, it approximates:

> **P(next_token | all_tokens_so_far)**

Where:

- **tokens** = pieces of text produced by the tokenizer (not always whole words)  
- **context** = everything you send: system prompt, user message, chat history, retrieved docs, etc.

Important consequences:

- The model doesn’t have beliefs or goals.  
  It outputs the next token that fits the learned distribution given the context.
- “Reasoning” is what you get when next-token prediction runs over text that *looks like* reasoning: step-by-step arguments, code, proofs, plans.
- Tiny changes in context → different probability distributions → different outputs.

**Mental model:**  
> “The model is an insanely good conditional autocomplete: `P(next token | context)`.”

---

## 3. Context window = working memory

The model can’t see everything you’ve ever said. It only “sees” the tokens in the **current context window** (up to its max token limit).

Think of this as its **working memory**:

- Anything **inside** the window can influence the next token.  
- Anything **outside** the window might as well not exist.  
- If you cram the window with junk, important bits are harder for the model to “notice.”

Inside the transformer, tokens interact via **attention**: each token can “look at” others in the window and decide how much to care about them. You don’t need the math; from the outside, it’s enough to think:

- **Recency bias** – tokens near the end usually matter more.  
- **Repetition effect** – repeating something makes it more salient.  
- **Order matters** – if you contradict yourself, later instructions often win.

**Mental model:**  
> “The model thinks inside a limited window of text. My job is to control what’s in that window and in what order.”

### Tiny example: order + recency

```text
System: You are a terse assistant. Answer in one short sentence.
User: Explain transformers in detail to a beginner.
```

vs.

```text
System: You are a terse assistant. Answer in one short sentence.
User: Explain transformers in detail to a beginner.
User (later): Actually, keep it under 20 words.
```

The second version is more likely to stay short, because the “20 words” instruction appears **later** and closer to where the model predicts tokens.

---

## 4. Pattern matching & in-context learning

During training, the model saw a massive number of patterns:

- Q&A pairs  
- Dialogues  
- Code and comments  
- Articles, docs, forum posts, etc.

At inference time:

> When you show it a pattern, it tries to **continue that pattern**.

This explains a lot:

- **Few-shot examples** – you show a few input → output pairs; it continues with another pair in the same style.  
- **Formatting** – if you show a table, it tends to answer as a table. If you show JSON, it tends to answer in JSON.  
- **Style** – if you write casually, it’s more likely to respond casually.

This behavior is often called **in-context learning**:

- The model’s **weights are fixed** at inference time.  
- You feed it examples **inside the prompt**.  
- It uses those examples as patterns to imitate under `P(next token | context)`.

**Mental model:**  
> “The model doesn’t just read my instructions; it imitates the patterns and formats I embed in the context.”

### Tiny example: pattern controls format

```text
User:
Q: 2 + 2 =
A: 4

Q: 10 + 5 =
A:
```

The model is likely to reply:

```text
A: 15
```

because it is imitating the **Q/A pattern** and exact formatting, not just doing “math in the air.”

When behavior is weird, ask:

- Did I give it at least one **clear example** of what I want?  
- Does the **format** of my example match what I expect back?  
- Am I accidentally showing it a **bad pattern** (e.g., a broken example, contradictory instructions)?

---

## 5. Sampling: why the same prompt can differ

Even with the same context, LLMs can produce different outputs.

Internally:

1. The model produces a **logit** (score) for each possible next token.  
2. A **softmax** turns those scores into probabilities.  
3. The next token is **sampled** from that probability distribution.

Two important knobs (names vary by API):

### Temperature

- Scales logits before softmax.  
- Higher temperature → probabilities flatten → more randomness, more creativity.  
- Lower temperature → probabilities sharpen → more deterministic, more repetitive.

### Top-k / top-p

- **Top-k**: sample only from the k most likely tokens.  
- **Top-p**: sample from the smallest set of tokens whose probabilities add up to p.  
- Lower values → safer, more predictable.  
- Higher values → more variety, more risk of nonsense.

So:

- **High temperature + loose instructions** → diverse, sometimes chaotic answers.  
- **Low temperature + strong structure** → more consistent, stable answers.

**Mental model:**  
> “Every next token is a weighted dice roll. Temperature controls how wild the dice are.”

For **creative tasks** (brainstorming, writing), some randomness is good.  
For **strict tasks** (JSON output, tool calls, parsing), you usually want:

- lower temperature  
- clear output format  
- (when available) tool/JSON mode instead of free-form text

---

## 6. Training vs inference: where its “habits” come from

How the model “thinks” is mostly shaped **before** you ever prompt it.

### 1. Pretraining

- Trained on huge text corpora.  
- Objective: predict the next token from previous ones.  
- Gives it:
  - language fluency  
  - world knowledge (up to a certain date)  
  - familiarity with code, docs, writing styles, etc.

### 2. Fine-tuning / alignment

Additional training to shape behavior:

- more helpful  
- better at following instructions  
- more honest about uncertainty  
- safer (less likely to produce harmful content)

Often done with supervised fine-tuning + human feedback (RLHF-style methods).

### 3. Inference (when you use it)

At inference time, we **do not update the weights**. We only:

- choose a model  
- provide context (prompts, history, retrieved docs)  
- adjust sampling settings (temperature, top-p, etc.)

**Mental model:**  
> “Training gave the model its skills and habits.  
> At inference, prompting doesn’t rewrite its brain; it steers how that fixed brain responds to the context.”

So when something feels impossible to prompt:

- Sometimes the model truly **lacks the capability**.  
- Sometimes the capability exists, but your **context and examples aren’t activating** the right pattern.

---

## 7. Quick “how it thinks” debug checklist

When an answer is bad or weird, think like the model and check four things:

### 1. Context window

- Is all the important information actually **in the prompt**?  
- Is anything critical **buried under noise** or contradictions?  
- Did you exceed the context window so that older parts were dropped?

### 2. Pattern & examples

- Did you show at least **one good example** of what you want?  
- Does the **example format** match what you expect back (tables, JSON, bullet points)?  
- Are you accidentally including bad examples or conflicting instructions?

### 3. Sampling

- Is **temperature** too high for this task?  
- Do you need stricter structure (e.g., “output valid JSON with fields `x`, `y`, `z`”)?  
- For critical paths, should you lower randomness or add validation/parsing on top?

### 4. Capability

- Are you asking for:
  - real-time data,  
  - exact obscure facts, or  
  - precision it’s not designed for?  
- Should you be using **external tools or retrieval** instead of expecting the model to “just know”?

If you keep these lenses in your head – **probabilities, context window, pattern imitation, and sampling** – you’ll be closer to how the model actually “thinks” than most people using it.
