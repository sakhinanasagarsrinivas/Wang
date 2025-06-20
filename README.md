

### 🤖 Agentic RAG for Time Series Analysis

* Proposes a Retrieval-Augmented Generation (RAG) framework tailored to time series tasks, embedding agentic decision-making throughout the inference pipeline.
* Unlike static RAG systems, the agent autonomously plans retrieval, selects appropriate models, and applies them to tasks such as forecasting, anomaly detection, and imputation.
* Supports dynamic model coordination, selecting the most suitable time series model based on task requirements.
* Model selection is policy-driven, with strategies learned to maximize task-specific utility rather than relying on fixed rules.
* Introduces a domain-specific utility function that scores each model’s output using relevant metrics (e.g., RMSE, MAE, F1-score), guiding the agent toward optimal model selection for each task.



### 🧠 System 2-style Reasoning Loop

* Inspired by the dual-process theory of cognition:

  * System 1: Fast, heuristic-based decision-making.
  * System 2: Slow, reflective, and analytical reasoning.
* The agent adopts a System 2-style reasoning loop, enabling deliberate, adaptive decision-making.
* Employs iterative feedback cycles to refine both retrieval and reasoning over time.
* At each step, the agent:

  * Evaluates whether the retrieved information is sufficient.
  * Decides which model to invoke next.
  * Determines whether further refinement or re-querying is needed.
* This approach mimics human analytical behavior, supporting multi-step, compositional reasoning in complex time series scenarios.



### 🧠 Core Contributions

#### 1. Hierarchical, Agentic RAG Architecture

* A master agent interprets user queries and delegates sub-tasks to specialized sub-agents for forecasting, anomaly detection, imputation, and classification.
* Each sub-agent runs a small, instruction-tuned language model (SLM) (e.g., Gemma, LLaMA), fine-tuned for its specific time series task.

#### 2. Differentiable Prompt Pools as Internal Knowledge

* Each sub-agent maintains a structured prompt pool containing key–value pairs:

  * Keys: Encoded temporal patterns (e.g., seasonality, regime shift).
  * Values: Distilled historical knowledge or reasoning templates.
* Prompts act as soft inductive biases, helping the model understand temporal structure.
* Prompt retrieval is based on latent-space similarity (e.g., cosine similarity), selecting the top‑K most relevant prompts.
* This supports distribution shift adaptation and captures long-range dependencies.

#### 3. Prompt-Oriented Input Augmentation

* Retrieved prompts are fused with input time series windows, enriching the model’s context with historically relevant signals.
* Prompts may encode:

  * Seasonality, cyclic trends, anomalies, regime shifts, and overall dynamics.

#### 4. Agentic Planning & ReAct-Inspired Pipeline

* The master agent employs a ReAct-style loop (Reason → Act → Reflect) to:

  * Detect knowledge gaps,
  * Retrieve relevant prompts,
  * Invoke the appropriate model,
  * Reflect on intermediate outputs,
  * Iterate if necessary.
* Enables model chaining across sub-agents — e.g., imputation → forecasting → anomaly detection on residuals.
* Facilitates compositional generalization, allowing complex multi-step workflows.


### 📊 5. Scalability & Modularity

* Features a plug-and-play architecture where new sub-agents or models can be integrated without retraining the master agent.
* The prompt pool and model selection policies can be independently updated, enabling continual improvement and learning.
* The system supports both horizontal scalability (adding new time series tasks) and vertical scalability (increasing model sophistication).



### 🧩 6. Comparative View: Static RAG vs Agentic RAG

* Retrieval Strategy
  Static RAG: One-shot retrieval.
  Agentic RAG: Iterative, feedback-driven retrieval.

* Model Selection
  Static RAG: Uses fixed, pre-defined models.
  Agentic RAG: Dynamically selects models via learned policies.

* Reasoning Loop
  Static RAG: No reasoning loop.
  Agentic RAG: Incorporates ReAct-style reasoning (Reason → Act → Reflect).

* Adaptability to Distribution Shifts
  Static RAG: Brittle under change.
  Agentic RAG: Adapts through prompt retrieval and model chaining.



### 🔄 7. Example Workflow

Query: “Predict next 7 days of power usage.”

Agentic Reasoning Steps:

1. Detects missing values in the input → invokes the imputation sub-agent.
2. Retrieves prompts relevant to weekly seasonality and demand trends.
3. Invokes the forecasting sub-agent, conditioned on historical patterns and retrieved context.
4. Applies anomaly detection on the forecast residuals to flag deviations.
5. Returns the final forecast along with a trace of reasoning steps for transparency.

---

