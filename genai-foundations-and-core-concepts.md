
# GenAI Foundations and Core Concepts
## The conceptual base required to understand the rest of the Google Cloud GenAI stack

## Abstract

This note reconstructs the conceptual foundation on which the rest of the learning material depends. The central argument is simple: most confusion in generative AI does not come from advanced mathematics but from category errors. Readers often mix artificial intelligence with machine learning, machine learning with deep learning, and deep learning with generative AI. They also confuse training, inference, data types, and language processing because the same systems are described at different levels of abstraction [1].

The study material repeatedly returns to the same foundational distinctions for that reason. Artificial intelligence is the broadest category. Machine learning refers to systems that improve at tasks from data rather than explicit step-by-step programming. Deep learning refers to neural-network-based approaches. Generative AI is the subset focused on producing new content such as text, images, audio, or video. Large language models are then a specialised subset of foundational models, strongly associated in the transcript with transformer architectures and token prediction [1].

These distinctions matter operationally. Once the categories are stable, the rest of the product ecosystem becomes far easier to understand. NotebookLM, Gemini, Vertex AI Studio, RAG, grounding, and prompt engineering all become applications or tools built around clearer model types rather than disconnected brand names. This note therefore treats the foundations not as introductory filler but as the load-bearing layer of the entire repository.

## 1. Introduction

Generative AI discussions often become muddled because the same word “AI” is used to describe radically different things. A spreadsheet formula can be marketed as AI. A recommendation engine can be called AI. A speech recogniser can be called AI. A conversational assistant can also be called AI. At a business level that may be tolerable, but for serious analysis the categories must be separated [1].

The transcript does exactly that by repeatedly defining the hierarchy from AI to ML to deep learning to generative AI. It also revisits natural language processing, supervised and unsupervised learning, inference, feature engineering, foundational models, and large language models from several angles, precisely because many learners only understand these ideas after seeing them framed multiple times [1].

That repetition is economically rational. In most enterprise settings, many strategic errors are not caused by sophisticated technical failures but by misunderstandings about what a system is actually doing. If one team thinks a model is “reasoning” in a human way while another understands it as a probabilistic pattern-completion engine, the organisation will make different design, risk, and governance choices. Foundations therefore matter not just pedagogically but institutionally.

## 2. Data and stylised facts

The transcript defines artificial intelligence as the broad category of machines performing tasks that mimic or simulate aspects of human intelligence, including problem solving, decision making, language understanding, speech recognition, or image recognition [1]. Machine learning is then described more narrowly as systems that improve at a task from data rather than from explicit fixed programming, while deep learning refers to artificial neural networks inspired by the human brain and used for more complex pattern recognition [1].

Generative AI is then presented as the subset focused on creating new outputs such as text, images, audio, video, and even molecular or scientific artefacts in some broader applications [1]. Large language models are described as foundational models that implement transformer architectures and generate text token by token, predicting the next likely sequence under learned language patterns [1].

A second stylised fact is that the material treats natural language processing as a bridge field between computer science and linguistics, useful for interpreting, transforming, and analysing text or speech. The transcript lists tasks such as sentiment analysis, translation, question answering, information extraction, speech synthesis, and command interpretation as examples of what NLP makes possible [1]. The exact list is less important than the structural insight: language processing is not identical to GenAI. It predates it and also supports it.

A third stylised fact concerns learning modes. Supervised learning is explained as learning from labelled data, while unsupervised learning is explained as learning patterns from unlabelled data, often discovering clusters or structures without predefined answers [1]. This matters later when the transcript discusses labelled versus unlabelled data and the role of training data, especially in contexts like summarisation or classification.

A fourth stylised fact concerns inference. Once a model is trained and deployed, using it to produce an output from new input is called inference [1]. This may sound basic, but the distinction between training and inference becomes essential once the material discusses cost, latency, hardware, and product design. Much of the cloud AI business model is built not only around training models but also around monetising inference at scale.

Finally, the transcript gives operational meaning to the token concept. Tokens are not identical to words, but they are the units processed by many language models. Longer sequences increase both memory usage and computation, because each extra token increases the amount of context the model must handle and the amount of work it must do [1]. This is a core economic fact of AI infrastructure, not just a technical curiosity.

## 3. Framework

A useful way to frame the foundations is to define the stack in terms of functions.

Let:
- $A$ denote the broad set of artificial intelligence systems;
- $M \subset A$ denote machine learning systems;
- $D \subset M$ denote deep learning systems;
- $G \subset D$ denote generative AI systems;
- $L \subset G$ denote large language models.

Then, at a high level:

$$
L \subset G \subset D \subset M \subset A
$$

This nesting matters because many conversations collapse all five levels into one. A better approach is to ask what function each layer performs.

Artificial intelligence is the broad ambition: mimic or simulate intelligent behaviour. Machine learning is a method class: learn from data. Deep learning is a model family: layered neural systems. Generative AI is a task orientation: produce novel outputs. Large language models are a specialised architecture-and-modality combination: transformer-based models focused on language-like sequential generation [1].

We can also define the supervised/unsupervised distinction formally. Let a training dataset be:

$$
\mathcal{D} = \{(x_i, y_i)\}_{i=1}^N
$$

in supervised learning, where each input $x_i$ has a corresponding label or target $y_i$. In unsupervised learning we instead observe:

$$
\mathcal{D} = \{x_i\}_{i=1}^N
$$

without explicit labels. The model’s task is then to discover structure rather than reproduce known targets. In plain language, supervised systems learn from “questions with answers,” while unsupervised systems learn from “questions without answer keys.”

For inference, let a trained model be $f_\theta$, where $\theta$ represents its learned parameters. Then inference is simply:

$$
\hat{y} = f_\theta(x)
$$

where $x$ is new input and $\hat{y}$ is the model output or prediction. The simplicity of the formula hides a major practical fact: training may happen rarely, but inference can happen millions or billions of times. That is why deployment economics matter so much.

## 4. Scenarios and analysis

### Scenario A — The language model as text autocomplete

Suppose a learner treats an LLM as “just autocomplete.” That description is too reductive, but it captures part of the mechanism. The transcript explains that the model predicts the next sequence of tokens and feeds outputs back into itself to continue generation [1]. In this sense, the model resembles a highly advanced probabilistic continuation engine.

An illustrative example is a sentence-completion prompt such as: “The capital of France is…”. The model is likely to produce “Paris” because it has learned that pattern from training distributions. This simple view explains a lot of ordinary LLM behaviour, including fluency and many forms of hallucination. It also explains why chain-of-thought and better prompting can improve outputs without changing the model’s underlying weights.

### Scenario B — The distinction between training and inference

Consider an image classifier trained to recognise bananas. During training, the system sees many labelled examples. After training, a new image is passed to the model. The act of classifying that new image is inference [1]. The transcript uses similar intuition when explaining a raw banana image being processed by a deployed model that returns a banana classification with some level of confidence [1].

This scenario matters because many business users blur the distinction and assume “using AI” means training a model. In practice, many systems rely far more on inference than on training. That is particularly true in enterprise applications that consume hosted APIs.

### Scenario C — NLP as enabling layer, not identical to GenAI

Take a translation system or sentiment classifier. These are language-processing systems, but they are not necessarily generative systems in the broad contemporary sense. NLP includes analysis, extraction, tagging, chunking, and language understanding tasks that may or may not produce novel open-ended outputs [1]. GenAI may use NLP foundations, but the two categories should not be collapsed.

The practical implication is that many product features described as “AI” in enterprise settings are still better understood as classical NLP or structured classification layers. Not every language-aware feature is an LLM workflow.

### Scenario D — Why token limits matter

Suppose a learner pastes a very long conversation into a model and asks for a detailed structured answer. The transcript explains that longer token sequences raise memory requirements and compute demands [1]. This means the model may become slower, more expensive, or unable to generate as much new output if the input already consumes a large share of the context window.

An illustrative case is a customer-support transcript summarisation task. If the input is already extremely long, the output budget shrinks. This is one reason why summarising previous context before continuing a conversation can improve feasibility and cost.

## 5. Risks and caveats

The first risk is anthropomorphism. Large language models can appear to reason, plan, or “understand” in human-like ways, but the transcript is careful to describe them in terms of learned patterns, sequence prediction, and transformer-based processing [1]. Over-humanising them can lead to poor governance and inflated expectations.

The second risk is category collapse. If a learner says “AI” for everything, they will struggle to distinguish between model classes, use cases, and tools. This is not just a semantic problem. It affects procurement, integration, capability assessment, and even staffing decisions.

The third risk is mistaking pedagogy for ontology. Simplified hierarchies are extremely useful for study, but real systems can blur category boundaries. A product can combine classical NLP, deep learning, retrieval systems, ranking logic, and generative outputs. The hierarchy remains useful, but one should not assume every real system sits neatly in one box.

The most robust conclusions in this chapter are the category distinctions and the centrality of inference, tokens, and language-processing foundations. The more fragile conclusions are those about how much a specific user should interpret an LLM as “reasoning,” since that depends partly on philosophical framing and partly on model capability.

## 6. Comparison and implications

Compared with traditional software, GenAI systems are less about explicit rule-writing and more about distribution-learning and context-sensitive generation. Compared with older machine-learning workflows, large language models often offer broader generality but also greater ambiguity in behaviour. Compared with rule-based NLP, modern generative systems provide flexibility and fluency but may reduce determinism.

For founders and operators, the implication is that one should identify whether the real business need is classification, extraction, generation, or search augmentation before choosing a tool. For investors, it means separating companies that rely on genuine model leverage from those that merely wrap generic interfaces around commodity capabilities. For policymakers, it means governance must begin from correct categorisation: not all “AI” systems pose the same risks or require the same rules.

The transcript’s layered explanation is useful because it reduces false novelty. Many AI capabilities that look revolutionary at the product layer still rest on older distinctions: labelled versus unlabelled data, training versus inference, representation versus prediction, and analysis versus generation.

## 7. Conclusion

The foundations matter because everything else in the stack depends on them. Once artificial intelligence, machine learning, deep learning, generative AI, NLP, foundational models, LLMs, and inference are correctly distinguished, the rest of the ecosystem becomes far easier to analyse [1].

The key practical lesson is that conceptual precision is not academic decoration. It is the basis for better prompts, better tool selection, better product design, and better strategic judgement. A learner who stabilises these foundations early will study the rest of the material more efficiently and with fewer errors.

The most important counter-intuitive insight is that introductory distinctions are often the highest-leverage part of the learning process. They look basic, but they carry most of the explanatory load.

## References

[1] Public online course transcript and transcript-based study notes on the Google Cloud GenAI Leader learning path, 2025, learning transcript / study summary.
