# Understanding the Transformer Architecture

## A Step-by-Step & In-Depth Guide Using One Example

This guide explains how a Transformer processes text, using one sentence throughout:

> **"The trophy wouldn't fit in the suitcase because it was too large."**

The example illustrates how a model uses context to represent relationships between words. Numerical values below are illustrative, not actual values from a particular model.

---

## 1. Tokenization — Break the Sentence into Pieces

A tokenizer breaks text into smaller units called **tokens** (which in real models are typically subwords via Byte-Pair Encoding / BPE or SentencePiece, rather than strict whole words).

For simplicity, imagine the sentence becomes:

```text
["The", "trophy", "wouldn't", "fit", "in",
 "the", "suitcase", "because", "it", "was",
 "too", "large", "."]
```

Each token is mapped to a numerical ID from the model's vocabulary.

| Token | Illustrative Token ID |
| :--- | :---: |
| `The` | `101` |
| `trophy` | `205` |
| `wouldn't` | `309` |
| `fit` | `412` |
| `suitcase` | `520` |
| `it` | `608` |
| `large` | `710` |

*Note: These IDs are examples only. A real tokenizer may split the sentence differently (e.g., `"would"` + `"n't"`) and use different IDs.*

**Why?** Neural networks operate on numbers, so text must first be converted into discrete numerical indices.

---

## 2. Embeddings — Convert Token IDs into Vectors

Each token ID is used to look up a learned numerical vector in an **embedding matrix** ($E \in \mathbb{R}^{V \times d_{\text{model}}}$, where $V$ is vocabulary size and $d_{\text{model}}$ is the hidden dimension).

For example:

```text
"The"       → [0.21, 0.34, 0.15, ...]
"trophy"    → [0.72, 0.18, 0.91, ...]
"suitcase"  → [0.63, 0.45, 0.28, ...]
"it"        → [0.32, 0.81, 0.42, ...]
```

A real embedding contains hundreds or thousands of dimensions (e.g., $d_{\text{model}} = 768$ to $4096+$). These numbers are illustrative.

An embedding is not a dictionary definition. It is a **learned dense numerical representation** in continuous vector space that the model transforms to capture semantic and syntactic patterns in language.

**Why?** Vectors allow the model to measure similarity, combine features via linear algebra, and differentiate nuanced meanings.

---

## 3. Positional Information — Understand Word Order

Unlike recurrent networks (RNNs) that read tokens one by one, a Transformer processes tokens in parallel. Without position information, it would treat a sentence as an unordered "bag of words."

Consider:
1. *The dog chased the cat.*
2. *The cat chased the dog.*

The words are identical, but the meaning flips when their order changes. A Transformer therefore injects positional information into each token representation:

```text
Token Embedding + Position Information
                  |
                  v
          Input Representation
```

**How models encode position in practice:**
- **Absolute Positional Embeddings / Sinusoidal Encoding:** Adds a position vector directly to the token embedding at the input.
- **Rotary Position Embeddings (RoPE):** Rotates the Query and Key vectors inside attention by an angle proportional to their position, encoding *relative* distance directly into the attention dot product (used in modern LLMs like Llama and Gemma).

---

## 4. Self-Attention — Understand Relationships Between Tokens

**Self-attention is the central mechanism of a Transformer.**

In our example, when the model processes the word **"it"**, it uses information from other tokens in the sentence to figure out what "it" refers to (*trophy* vs. *suitcase*). It calculates how relevant every other token is to the representation being built for `"it"`.

Self-attention projects each token vector $x$ into three learned representations using weight matrices ($W_Q, W_K, W_V$):

| Component | Formula | Simple Way to Think About It |
| :--- | :--- | :--- |
| **Query ($Q$)** | $Q = x W_Q$ | What information am I looking for? |
| **Key ($K$)** | $K = x W_K$ | What information can this token match against? |
| **Value ($V$)** | $V = x W_V$ | What information can this token contribute? |

### Step-by-Step Scaled Dot-Product Attention

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{Q K^T}{\sqrt{d_k}} + M\right) V$$

1. **Score relevance ($Q K^T$):** Compare a token's Query against the Keys of other tokens via dot product.
2. **Scale ($\div \sqrt{d_k}$):** Divide by the square root of the key dimension $d_k$ to keep gradients stable and prevent softmax saturation.
3. **Mask ($+ M$, in decoder-only LLMs):** Set future positions to $-\infty$ before softmax (**causal masking**) so a token cannot peek at future tokens during training or generation.
4. **Normalize ($\text{softmax}$):** Convert scores into attention weights that sum to $1.0$ ($100\%$).
5. **Aggregate ($\times V$):** Multiply the weights by the Value vectors and sum them to build a context-aware representation.

### Illustrative Attention Weights

Imagine, purely for explanation, that the model assigns these weights while processing `"it"`:

| Token | Illustrative Attention Weight |
| :--- | :---: |
| `trophy` | `65%` |
| `suitcase` | `20%` |
| `because` | `5%` |
| Other tokens | `10%` |

These numbers are hypothetical. Actual attention weights vary by model, layer, head, input, and token position.

**Important:** Attention weights are mathematical quantities learned and used internally by the model. They should not automatically be interpreted as a direct explanation of human-like reasoning.

---

## 5. Multi-Head Attention — Examine Relationships in Different Ways

Instead of performing one big attention calculation, a Transformer splits the representation into **$h$ parallel attention heads** (e.g., 12, 32, or 64 heads).

Each head has its own smaller $W_Q^{(i)}, W_K^{(i)}, W_V^{(i)}$ projections and performs its own attention calculation independently. Different heads can learn to specialize in different patterns—such as syntax, pronoun coreference, adjacent-word pairs, or long-range dependencies.

The outputs of all heads are concatenated and passed through an output projection matrix ($W_O$):

$$\text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, \dots, \text{head}_h) W_O$$

Think of this as examining the same sentence through several learned perspectives simultaneously.

---

## 6. Feed-Forward Network (FFN) — Transform the Information Further

After the attention sublayer, each token's updated representation passes independently through a **Feed-Forward Network (FFN)** (often a 2-layer MLP or gated variant like **SwiGLU** in modern LLMs).

A simple distinction:
- **Attention** mixes information *across* different token positions.
- **The FFN** applies learned nonlinear transformations *at each position individually*, acting as a key-value memory that refines features and stores factual/linguistic associations.

```text
Attention Output
       |
       v
Linear Transformation (Expand dimension, e.g., 4x)
       |
       v
Nonlinear Activation (GELU / SiLU / SwiGLU)
       |
       v
Linear Transformation (Project back to d_model)
       |
       v
Updated Representation
```

---

## 7. Transformer Blocks Repeat (Residual Connections & Normalization)

A Transformer stacks these operations across many identical **Transformer blocks** (layers)—from 12 layers in smaller models to 80+ layers in frontier LLMs.

Inside each block, two stabilizing mechanisms surround the Attention and FFN sublayers:
1. **Residual (Skip) Connections ($x + \text{Sublayer}(x)$):** Allow gradients to flow cleanly during backpropagation and let earlier information pass straight through without being overwritten.
2. **Normalization (LayerNorm or RMSNorm):** Keeps activation magnitudes well-behaved across deep stacks.

```text
Input Representations
         |
         v
+-----------------------------------+
|  Transformer Block 1              |
|   1. Norm → Multi-Head Attention  |
|   2. + Residual Connection        |
|   3. Norm → Feed-Forward Network  |
|   4. + Residual Connection        |
+-----------------------------------+
         |
         v
   Transformer Block 2..N
         |
         v
   Final Representations
```

---

## 8. Generate the Next Token

After the final Transformer block, a generative (decoder-only) language model predicts what token comes next:

1. **Language Model Head:** Projects the final hidden vector of the last token into a vector of raw scores across the entire vocabulary ($V$). These raw scores are called **logits**.
2. **Softmax:** Converts logits into a probability distribution over all possible next tokens (optionally shaped by **temperature**).

For example, given `"The trophy was too..."`, the model might produce:

| Possible Next Token | Illustrative Probability |
| :--- | :---: |
| `large` | `80%` |
| `heavy` | `12%` |
| `small` | `5%` |
| Other | `3%` |

3. **Decoding Strategy:** Selects the next token (e.g., greedy decoding, top-$k$, or top-$p$ / nucleus sampling). Suppose it selects **`"large"`**.
4. **Autoregressive Loop:** The selected token is appended to the input context, and the process repeats token-by-token until an end-of-sequence (`<EOS>`) token or max-length limit is reached. *(In inference, a **KV Cache** stores past Keys and Values so earlier tokens don't have to be recomputed on every step.)*

---

## 9. Complete Transformer Flow

Here is the full simplified flow for a decoder-only generative LLM:

```text
INPUT SENTENCE
      |
      v
TOKENIZATION
Break text into tokens
      |
      v
TOKEN IDs
Map tokens to vocabulary numbers
      |
      v
EMBEDDINGS (+ POSITIONAL INFORMATION)
Look up learned numerical vectors and encode order
      |
      v
TRANSFORMER BLOCKS (x N Layers)
      |
      +--> Norm → Multi-Head Self-Attention (Q, K, V + Causal Mask)
      |       + Residual Connection
      |
      +--> Norm → Feed-Forward Network (FFN)
      |       + Residual Connection
      |
      v
FINAL REPRESENTATIONS
      |
      v
LANGUAGE MODEL HEAD → LOGITS
      |
      v
SOFTMAX / DECODING (Temperature, Top-k / Top-p)
      |
      v
SELECT NEXT TOKEN → APPEND TO CONTEXT → REPEAT
      |
      v
GENERATED TEXT
```

---

## 10. Five Core Ideas to Remember

| Component | What It Does |
| :--- | :--- |
| **Tokenization** | Breaks text into subword/word tokens and maps them to IDs. |
| **Embeddings & Position** | Converts token IDs into vectors and encodes sequence order. |
| **Self-Attention** | Lets each token gather contextual information from relevant earlier tokens. |
| **Feed-Forward Network** | Transforms each token's representation further with learned nonlinearities. |
| **Language Model Head** | Produces vocabulary scores (logits) to predict and sample the next token. |

### A Simple Reading Analogy

Imagine reading a paragraph:
1. **Tokenization:** Break the text into pieces.
2. **Embeddings:** Represent those pieces numerically.
3. **Positional information:** Track their order.
4. **Attention:** Let each word look back at related words to resolve context (like linking `"it"` to `"trophy"`).
5. **Feed-forward network:** Digest and refine that gathered meaning.
6. **Multiple blocks:** Repeat this reading and refining process across deeper layers of abstraction.
7. **Language model head:** Predict the most likely next word.

---

## Final Takeaway

A Transformer converts text into numerical vectors, uses **multi-head self-attention** to combine contextual information across tokens, refines those representations through **feed-forward networks**, and—in a generative LLM—predicts text **autoregressively**, one token at a time.

**The key insight:** A Transformer does far more than process isolated words. Its attention mechanism dynamically routes information between tokens across the sequence, allowing the model to resolve ambiguity and build rich context for every prediction.
