
# Prompting, Model Behaviour, and Limitations
## How prompting strategies, sampling settings, and model constraints shape output quality

## Abstract

Prompting is often presented superficially as “just asking good questions,” but the transcript-based material reviewed here presents it more seriously. Prompting is a control interface for probabilistic systems whose outputs depend on context, framing, parameter settings, and model constraints. The note therefore treats prompting not as a soft skill but as a practical mechanism for guiding model behaviour under uncertainty [1].

The material repeatedly highlights a small number of techniques as high-value for the exam and for real-world understanding: zero-shot, few-shot, prompt chaining, and chain-of-thought prompting. It also emphasises the importance of core settings such as temperature, token limits, seed, and top-$p$, together with the limits of the model itself: hallucinations, bias, knowledge cutoffs, edge cases, and data dependency [1]. These are not isolated details. Together they describe how and why model outputs vary.

The central analytical claim of this note is that model output quality is jointly determined by three layers: the prompt, the model, and the surrounding context or retrieval system. Better prompting can improve a weak answer, but it cannot completely neutralise a poor model, missing external knowledge, or unsuitable context length. Good prompting is therefore best understood as a way of improving expected output quality subject to hard constraints.

## 1. Introduction

Generative AI systems create a strange cognitive trap. Because natural-language interfaces look easy, users often assume that quality failure must be random or mysterious. In reality, many failures are structured. The prompt may be underspecified. The model may lack needed context. The sampling parameters may be too loose. The task may be too large for one pass. Or the model may simply not know the answer because of training limits [1].

The transcript approaches this problem sensibly. It treats prompting as a family of methods rather than a single trick. A well-formed prompt can constrain the role of the model, the tone, the task, the expected structure, the amount of context, and the reasoning pattern [1]. Similarly, a poorly chosen setting such as excessive randomness or low output budget can degrade answers even when the model is capable.

This note therefore studies prompting and output quality together. It is not enough to know what zero-shot or chain-of-thought means in isolation. One also needs to understand how those methods interact with parameter settings and with known model limitations such as hallucination or knowledge cutoff.

## 2. Data and stylised facts

The transcript identifies several prompting techniques as directly testable and operationally important. Zero-shot prompting refers to asking the model to perform a task without providing examples, relying on capabilities already learned during training [1]. Few-shot prompting refers to including examples in the prompt so the model can infer the pattern of the desired output [1]. Prompt chaining refers to splitting a complex task into sequential smaller prompts, where the output of one step becomes the input of the next [1]. Chain-of-thought prompting refers to explicitly asking the model to reason step by step in order to improve accuracy, particularly on tasks requiring intermediate reasoning [1].

The transcript also identifies key model parameters. Temperature controls randomness: lower values push the model toward more deterministic and conservative output, while higher values increase diversity and creativity [1]. Output token limits cap the amount of generated output, affecting not only length but also cost, latency, and in some cases answer completeness [1]. Seed is described as a way to promote consistency by initialising the model’s random generator in a repeatable way [1]. Top-$p$ controls the probability mass from which candidate tokens are sampled, with lower values making token selection more conservative and higher values allowing broader sampling [1].

On the limitation side, the transcript highlights hallucinations, bias, fairness issues, knowledge cutoffs, data dependency, and edge cases as recurring weaknesses of foundational models [1]. These are not minor caveats. They are central to understanding why the same model can look brilliant in one context and unreliable in another. Hallucinations are especially important because generative systems often produce plausible language even when underlying factual grounding is absent.

A further stylised fact is that some mitigation methods recur elsewhere in the ecosystem. Grounding, RAG, Google Search augmentation, and enterprise search integration are all introduced partly to reduce hallucination and improve factual relevance [1]. This creates an important analytical link: prompting matters, but it is not the only answer to quality problems.

## 3. Framework

We can model answer quality as:

$$
Q = F(P, M, C, \Theta)
$$

where:
- $Q$ is output quality,
- $P$ is prompt design,
- $M$ is model capability,
- $C$ is context quality and relevance,
- $\Theta$ represents sampling and generation settings such as temperature, top-$p$, and output token limits.

This framing is useful because it avoids a common mistake: attributing every output failure to the prompt. In reality, a prompt can only guide the model within its capability and context constraints. If the model lacks knowledge, if the context is outdated, or if the output budget is too small, better phrasing alone may not solve the problem.

For zero-shot prompting, the task is:

$$
\hat{y} = f_\theta(x)
$$

where the model receives only the task input $x$. In few-shot prompting, we instead supply examples:

$$
\hat{y} = f_\theta(x, e_1, e_2, \dots, e_k)
$$

where each $e_j$ is an example pair or pattern. Prompt chaining can then be represented as a sequence:

$$
z_1 = f_\theta(x), \qquad z_2 = f_\theta(z_1), \qquad \dots, \qquad \hat{y} = f_\theta(z_{n-1})
$$

This sequence is useful when a single-step prompt would overload the model or produce unstable output. Chain-of-thought can be represented conceptually as asking the model to generate intermediate reasoning $r$ before final answer $\hat{y}$:

$$
(r, \hat{y}) = f_\theta(x)
$$

The answer is not guaranteed to be correct merely because reasoning text appears, but the transcript treats step-by-step reasoning as especially useful for some classes of tasks [1].

For token settings, a simplified view is that higher temperature and higher top-$p$ widen the set of plausible next-token candidates, while lower values narrow it. Output token limits cap the total generation budget. If input consumes too much of the context window, the remaining output capacity shrinks [1].

## 4. Scenarios and analysis

### Scenario A — Zero-shot classification

Suppose the user asks the model to classify a sentence as positive, negative, or neutral without examples. The transcript gives a simple sentiment-style illustration of this kind of task [1]. If the model has already internalised the general pattern during training, zero-shot is sufficient. This works well for common and simple tasks.

The advantage is speed and simplicity. The downside is reduced control. If the user wants a very specific interpretation style, schema, or threshold, examples may become necessary.

### Scenario B — Few-shot formatting

Now consider a task in which the user wants outputs in a very specific structure, such as a sequence of labelled translations or a custom JSON format. If the user provides two or three examples of the exact desired structure, the model can often align more reliably. This is the practical logic behind few-shot prompting [1].

An illustrative invented example would be customer-service tagging. A zero-shot prompt may produce acceptable tags, but a few-shot prompt showing exactly how “billing dispute,” “delivery delay,” and “refund request” should be written can substantially improve consistency.

### Scenario C — Prompt chaining for a long workflow

The transcript gives a useful applied intuition here: when a task is too large or too complex, splitting it into smaller tasks works better than asking for the full result in a single prompt [1]. An example described in the material involves translating subtitle lines, extracting grammar points, explaining them, and then formatting the result. A single huge prompt performed poorly; chained steps performed better [1].

This points to an important mechanism. Large tasks create error compounding because the model must simultaneously track multiple objectives, constraints, and structures. Prompt chaining lowers local complexity.

### Scenario D — Chain-of-thought for reasoning

Consider a simple reasoning task: identify two prime numbers summing to ten. The transcript contrasts a direct question with a version explicitly asking for step-by-step reasoning [1]. For some models and tasks, chain-of-thought improves reliability because intermediate steps help the model stabilise the reasoning path.

However, there is a caveat. Visible reasoning does not guarantee true correctness. It merely increases the chance that the model will arrive at a better answer on certain tasks. Users should therefore treat chain-of-thought as a performance aid, not as proof.

### Scenario E — Parameter misalignment

Imagine a user asking for a factual policy summary but setting temperature very high. The model may become more creative than appropriate. Conversely, imagine a brainstorming task with very low temperature and extremely tight output limits. The output may become flat, generic, and incomplete. This scenario shows that prompts and settings must match task type.

A practical rule implied in the transcript is that fact-heavy or structured tasks generally benefit from lower randomness, while ideation or creative tasks can tolerate more diversity [1].

### Scenario F — Hallucination under missing knowledge

Suppose the user asks for a very recent real-world update that is not in the model’s training distribution. Even with a good prompt, the model may produce plausible but false information. This is exactly the type of case where the transcript later recommends grounding or retrieval support [1]. The lesson is structural: prompting cannot fully substitute for access to relevant external information.

## 5. Risks and caveats

The first major risk is overconfidence in prompt engineering. Prompting matters, but it is not magic. If the model is weak, misaligned, under-informed, or deprived of context, prompt refinement may yield only modest gains.

The second risk is mistaking fluency for reliability. A beautifully worded answer can still hallucinate, misclassify, or overstate confidence. This is one of the reasons the transcript repeatedly stresses model limitations and grounding [1].

The third risk is parameter confusion. Temperature and top-$p$ are often treated as interchangeable “creativity sliders,” but the transcript suggests a more disciplined approach: usually adjust one while leaving the other closer to its default, rather than moving both aggressively at once [1]. That is a practical heuristic rather than a theorem, but it reflects good operational caution.

The fourth risk is task overload. Users often try to make one prompt do too much. Prompt chaining exists precisely because some tasks are better decomposed. When a user sees unstable, partial, or structurally inconsistent output, the answer may not be a better adjective in the prompt but a better task decomposition.

The most robust conclusions in this chapter are the definitions of the prompting methods and the importance of hallucinations, bias, and knowledge limits. More fragile conclusions concern when exactly a given chain-of-thought or parameter setting will help, because that varies by model and task.

## 6. Comparison and implications

Compared with classical software interfaces, prompts are under-specified control mechanisms. They rely on language, examples, and context rather than explicit deterministic rules. Compared with ordinary search queries, prompts must often contain more structure because they are not merely retrieving information but shaping generation.

For founders and operators, the implication is that prompt design should be treated as part of product logic, not as an afterthought. For investors, it means many superficial “AI products” are less differentiated than they appear if they rely only on generic prompts over generic models. For policymakers and enterprise decision-makers, it means output quality governance must consider not only models but also prompt patterns, retrieval design, and user interfaces.

A useful comparison is between prompting and retrieval. Prompting changes how the model uses what it already has; retrieval changes what information is available to it. The two are complements, not substitutes.

## 7. Conclusion

Prompting is the practical interface between user intent and model behaviour, but it operates within hard limits imposed by model capability, context access, and sampling choices [1]. Zero-shot, few-shot, prompt chaining, and chain-of-thought are therefore best understood as structured control strategies rather than tricks or hacks.

The key insight is that output quality is multi-causal. Better prompts help, but the best outcomes usually come from the joint design of prompt structure, parameter settings, and external grounding or retrieval when needed. The most important counter-intuitive lesson is that many failures blamed on “bad AI” are actually failures of task decomposition, context provision, or expectation management.

## References

[1] Public online course transcript and transcript-based study notes on the Google Cloud GenAI Leader learning path, 2025, learning transcript / study summary.
