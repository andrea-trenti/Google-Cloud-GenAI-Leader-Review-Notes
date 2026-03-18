
# Data, Models, and Infrastructure
## How data structures, training choices, and compute architecture shape AI systems

## Abstract

This note explains the structural relationship between data, models, and infrastructure in the learning material reviewed for this repository. The central point is that model behaviour cannot be understood in isolation from the form of data it consumes and the hardware or compute environment on which it runs. Many practical enterprise misunderstandings arise because people think in terms of “the model” alone, when in reality performance, cost, latency, and operational feasibility depend on the full pipeline: structured versus unstructured data, labelled versus unlabelled datasets, model type, tuning strategy, and the compute substrate used for training and inference [1].

The transcript repeatedly revisits several concepts that seem basic but are operationally decisive. These include structured, semi-structured, and unstructured data; labelled and unlabelled data; fine-tuning and its variants; and the hardware distinctions among CPUs, GPUs, iGPUs, and Google TPUs. The material also links tokens and context windows back to compute and memory constraints, which is essential for understanding why large generative models are expensive and why output length matters [1].

The resulting analytical picture is one of trade-offs. Better data quality improves learning but is expensive to obtain. Full fine-tuning gives greater model adaptation but can be slow and costly. High-performance accelerators improve throughput but require capital and cloud resources. This note presents those trade-offs in a coherent framework so the rest of the product stack can be interpreted more seriously.

## 1. Introduction

There is a persistent temptation in AI discussions to treat model quality as if it emerges from the model alone. That is rarely true. A language model trained on poor data, fed through weak retrieval, and constrained by tight context budgets will not behave like the same model deployed with rich context, better tuning, and more suitable infrastructure. The transcript addresses this problem by repeatedly connecting model behaviour to data preparation and compute realities [1].

This is especially important in enterprise settings. Different use cases demand different data structures. A search application may rely on semi-structured and unstructured corpora. A finance dashboard may depend on structured tables. A summarisation system may need labelled examples for tuning. A customer-assist tool may require curated conversation datasets. At the same time, the economics of these systems depend on whether the organisation trains a model, fine-tunes a model, or simply uses hosted inference [1].

This note therefore asks three linked questions. First, what kinds of data matter? Second, how do models use that data? Third, what infrastructure supports the resulting computation? The answers clarify much of what otherwise looks like branding.

## 2. Data and stylised facts

The transcript distinguishes among unstructured, semi-structured, and structured data. Unstructured data is described as loose data without a clean predefined relational schema, such as arbitrary text files, documents, audio, images, or mixed file collections [1]. Semi-structured data retains some machine-readable organisation without becoming fully relational, with examples such as JSON, XML, Avro, or Parquet-like formats mentioned in the explanation [1]. Structured data, by contrast, is organised into more explicit schema-driven forms such as rows and columns, making browsing and querying easier [1].

A second key distinction is labelled versus unlabelled data. Labelled data is data that has been annotated with target values or classes, typically by humans, so the model can learn mappings from inputs to known answers [1]. Unlabelled data lacks those explicit targets and is used in settings where the system must infer patterns or structure for itself [1]. The transcript links this directly to supervised versus unsupervised learning.

The material also introduces “ground truth” as properly labelled data used as an objective standard for training and evaluating a model [1]. This concept matters because many model-quality problems are not due to architecture but to poor or inconsistent labels.

On the model side, the transcript introduces foundational models as broad, pre-trained, general-purpose systems trained on large datasets and then adapted or used across tasks [1]. Large language models are treated as a subset of foundational models, especially associated with transformer architectures [1]. Fine-tuning is then presented as retraining a pre-trained model’s parameters or weights on a smaller, more focused dataset [1].

Several tuning variants are mentioned. Full fine-tuning updates all or most model weights and is expensive [1]. Parameter-efficient fine-tuning updates only a smaller subset of parameters, and LoRA-style approaches are introduced as examples of more efficient adaptation [1]. The material also mentions last-layer tuning and pruning as ways to reduce tuning or model complexity [1].

On the infrastructure side, the transcript discusses CPUs, GPUs, iGPUs, and TPUs. GPUs are described as specialised for highly parallel tasks and therefore useful for machine learning and scientific computation [1]. iGPUs integrate graphics capability into broader chips and can still be useful for AI workloads [1]. TPUs are described as Google-developed tensor processing units optimised for machine learning, especially in the Google ecosystem [1]. CUDA is explained as an NVIDIA compute framework that helps make GPUs highly useful for general-purpose machine-learning tasks [1].

Finally, the transcript ties token count to compute and memory. Longer sequences require more memory and more operations, which directly affects cost and latency [1]. This connects model design back to infrastructure economics.

## 3. Framework

The core framework can be written as a data-to-model-to-output pipeline:

$$
\text{Raw Data} \rightarrow \text{Prepared Data} \rightarrow \text{Model Training or Adaptation} \rightarrow \text{Inference}
$$

Let:
- $x$ denote raw input data,
- $\phi(x)$ denote processed or engineered features,
- $f_\theta$ denote a model with parameters $\theta$,
- $\hat{y}$ denote model output.

Then the basic structure is:

$$
\hat{y} = f_\theta(\phi(x))
$$

where $\phi(x)$ may involve cleaning, tokenisation, labelling, embedding, schema conversion, or feature engineering. The key insight is that poor $\phi(x)$ can degrade $\hat{y}$ even if $f_\theta$ is strong.

For fine-tuning, let $\theta_0$ be the original pre-trained parameters and let $\theta^\ast$ be the adapted parameters after tuning on task-specific data. Then:

$$
\theta^\ast = \arg\min_\theta \mathcal{L}(\theta; \mathcal{D}_{task})
$$

where $\mathcal{L}$ is the loss function and $\mathcal{D}_{task}$ is the task-specific dataset. In full fine-tuning, many or all parameters are updated. In parameter-efficient tuning, only a subset or low-rank adaptation is updated, reducing compute burden.

We can also represent compute burden loosely as depending on three variables:

$$
\text{Compute Load} = h(N, d, T)
$$

where:
- $N$ is the number of model parameters or active computation pathways,
- $d$ is data or context dimensionality,
- $T$ is token or sequence length.

This makes the transcript’s token point intuitive. Larger contexts increase $T$, which raises compute demand and memory usage [1].

On the data side, a stylised classification is:

$$
\mathcal{D} = \mathcal{D}_{structured} \cup \mathcal{D}_{semi} \cup \mathcal{D}_{unstructured}
$$

Each subset requires different storage, retrieval, and preparation logic. A generative system interacting with enterprise data often combines all three.

## 4. Scenarios and analysis

### Scenario A — Structured dashboard versus document assistant

Consider two applications. The first is a dashboard answering questions from a relational business database. The second is a document assistant summarising a folder of contracts and PDFs. The first leans heavily on structured data. The second leans heavily on unstructured data. A team that uses the same retrieval and processing logic for both will likely perform poorly.

The dashboard use case benefits from schema clarity and deterministic query logic. The document-assistant use case requires text extraction, chunking, indexing, and possibly embeddings. This comparison shows why “data type” is not a cosmetic detail. It determines the architecture.

### Scenario B — Labelled customer-support data

Suppose a firm wants to train or adapt a support model to detect refund requests and delivery problems. If its conversation logs are unlabelled, the firm may still cluster common issues or retrieve relevant passages, but supervised classification quality will remain limited. Once the conversations are labelled with categories such as “refund,” “lost shipment,” or “billing issue,” model performance can improve because the system now has target examples [1].

This is a direct illustration of why labelled data is expensive but valuable. Human annotation is costly, but it converts raw logs into trainable supervision.

### Scenario C — Full fine-tuning versus efficient tuning

Imagine a team adapting a base model to a narrow domain such as internal policy documents. Full fine-tuning may provide strong domain alignment but can be slow and expensive. Efficient tuning or LoRA-like strategies update fewer parameters, reducing cost and time [1]. The transcript mentions very long training times in other contexts, such as 8–24 hours for some enterprise training workflows, reinforcing the point that adaptation choice has major operational consequences [1].

An illustrative invented example would be a mid-sized firm choosing between a week-long expensive adaptation pipeline and a lighter retrieval-plus-efficient-tuning approach. The right answer depends on budget, latency needs, domain specificity, and maintenance burden.

### Scenario D — Hardware mismatch

Suppose a team assumes any consumer laptop can run any open-weight model locally because the model is “open.” The transcript’s hardware discussion shows why this is naive. Token length, model size, memory requirements, and accelerator availability determine feasibility [1]. An iGPU may be useful for some local AI tasks, but a demanding model may still require dedicated GPU resources or hosted inference [1].

This scenario matters because product choices such as Gemini versus Gemma are partly infrastructure choices. Hosted models externalise compute burdens; local models internalise them.

### Scenario E — Token growth and cost

Take a long enterprise chat assistant. Each extra turn may increase context size. If the conversation is never summarised or pruned, token count grows, memory use rises, and compute cost increases [1]. This provides a simple operational lesson: context management is part of infrastructure economics.

## 5. Risks and caveats

The first risk is underestimating data work. Many organisations talk as if the main challenge is “choosing the model,” but the transcript strongly suggests that data preparation, structure, labels, and retrieval logic are at least as important [1].

The second risk is assuming fine-tuning is always the answer. In many settings, retrieval or better prompting may achieve enough improvement without the cost of adaptation. Fine-tuning should be treated as one tool among several, not as the default sign of seriousness.

The third risk is infrastructure simplification. GPU, iGPU, TPU, and hosted inference options differ in cost, speed, flexibility, and ecosystem compatibility. A wrong infrastructure assumption can turn a feasible prototype into an impractical deployment.

The fourth risk is false precision. The transcript introduces concepts such as token cost and compute load qualitatively. That is useful, but one should not infer exact scaling laws or cost curves from it alone. The robust conclusion is directional: more context and more adaptation usually raise cost and complexity.

## 6. Comparison and implications

Compared with traditional analytics systems, AI systems rely much more heavily on unstructured and semi-structured data. Compared with rule-based systems, they depend more strongly on representation quality and compute. Compared with simple API consumption, local or fine-tuned systems demand more infrastructure control but may offer more flexibility or privacy.

For founders and operators, the implication is to design backwards from the data form and the latency budget. For investors, it means asking whether a company’s advantage lies in data quality, model access, tuning, retrieval, or infrastructure efficiency. For policymakers, it means understanding that compute, data access, and model openness are all part of the industrial structure of AI.

A useful benchmark is this: when a company claims it is “building with AI,” the first analytical questions should be what kind of data it uses, how that data is prepared, whether the model is trained, fine-tuned, or merely consumed through inference, and what compute assumptions support the workflow.

## 7. Conclusion

Data, models, and infrastructure form an integrated system. Structured and unstructured data require different preparation paths. Labelled data enables supervised learning. Fine-tuning changes how models adapt to domains. Hardware and context limits determine what is economically and operationally feasible [1].

The key lesson is that AI quality is not just a model property. It is a pipeline property. The most important counter-intuitive insight is that many apparent “model” problems are really data-shaping or infrastructure problems in disguise.

## References

[1] Public online course transcript and transcript-based study notes on the Google Cloud GenAI Leader learning path, 2025, learning transcript / study summary.
