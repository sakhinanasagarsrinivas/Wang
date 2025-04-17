% \vspace{-1mm}
% \subsection{Experimental Settings}% combines memory efficiency with task-specific adaptability
% We fine-tuned the Llama 3.2 1B model with QLoRA (Quantized Low-Rank Adaptation) for task-specific applications. The pre-trained weights were frozen and quantized to 4-bit precision, reducing memory usage while supporting long input sequences. Additional low-rank matrices in full precision (e.g., FP16) were introduced to key layers such as query, key-value, output, and gate projections, with a rank of 16 and a scaling factor of 16. During forward computation, the 4-bit weights were combined with LoRA weights, and only the LoRA weights were updated during backward passes, ensuring efficient fine-tuning on resource-constrained hardware without compromising performance. Key training parameters included a batch size of 2 per GPU (the workload for each GPU in a distributed training setup), with gradient accumulation over 4 steps to simulate an effective batch size of 8. This approach allowed gradients to be calculated and accumulated over multiple forward and backward passes before updating the LoRA weights, optimizing memory usage and maintaining training stability. The training process began with 10 warm-up steps, during which the learning rate (\(lr\)) was gradually increased from zero to its target value (\(lr = 1 \times 10^{-3}\)), stabilizing updates and preventing abrupt optimization changes. Training concluded after 60 optimization steps, where accumulated gradients were used to iteratively update the model's trainable parameters, ensuring convergence and effective learning rather than being constrained by a fixed number of epochs. The AdamW optimizer used block-wise quantization to store parameters such as gradients, momentum, and variance in 8-bit precision, significantly reducing memory usage while maintaining performance comparable to 32-bit optimizers. A weight decay rate of 0.01 was applied to prevent overfitting, and the learning rate was dynamically adjusted using a linear scheduler for stable convergence. Mixed-precision training techniques, specifically BF16 (16-bit brain floating point), were optimized for NVIDIA A100 GPUs to accelerate computation and ensure training stability. Random seeds ensured reproducibility, and all training outputs were logged for analysis.

% \section{Experimental Settings}

% The fine-tuning process for the Meta LLaMA 3.2-1B model was carried out in three sequential stages, progressively refining the model’s ability to generate responses by leveraging different datasets. The first phase involved fine-tuning on a QA dataset, where the model learned fundamental question-answering abilities. This was followed by Direct Preference Optimization (DPO) fine-tuning, which improved response alignment by distinguishing between preferred and rejected responses. Finally, the model was further enhanced through Retrieval-Augmented Instruction Tuning (RAIT), which integrated retrieved contextual information to support multi-hop reasoning and knowledge grounding.

% For all three stages, training was conducted using Low-Rank Adaptation (LoRA), which enabled efficient parameter tuning while keeping the majority of model weights frozen. LoRA was configured with a rank of 16, a scaling factor of 16, and no dropout, optimizing both performance and memory efficiency. The model was trained with a batch size of 2 per device, using 4 gradient accumulation steps to achieve an effective batch size of 8. The AdamW optimizer with 8-bit quantization was employed, using a learning rate of \(2 \times 10^{-4}\) with a linear decay schedule and a weight decay of 0.01. Mixed precision training was applied with FP16 or BF16, depending on hardware compatibility, and all experiments were executed on NVIDIA Tesla T4 GPUs. To handle long-context inputs effectively, all fine-tuning steps were conducted with a maximum sequence length of 4096 tokens.

% Initially, QA and RAIT fine-tuning were performed for 5 epochs, while DPO was trained for 2 epochs. However, the model did not reach convergence in these initial runs. To improve performance, additional fine-tuning was conducted, extending QA and RAIT training to 15 epochs and DPO to 5 epochs. This additional training allowed the model to better capture instruction-following patterns, refine preference-based response ranking, and integrate contextual information from retrieved knowledge.

% The entire fine-tuning pipeline was implemented in Python using PyTorch and Unsloth, leveraging gradient checkpointing to reduce memory consumption. Datasets were processed with the Hugging Face \texttt{datasets} library, and model training was managed using \texttt{SFTTrainer} for supervised fine-tuning and \texttt{DPOTrainer} for preference optimization. Performance was evaluated across all stages using standard NLP metrics, including BLEU, ROUGE, METEOR, BERTScore, and Similarity metric. 



\documentclass[48pt]{beamer}

\usetheme{Madrid}

\title{Reinforcement Learning Fine-Tuning of Large Language Models for Domain-specific Customization to Auto-generate PFDs and P\&IDs for novel Chemical Production Process}
\author{Sagar Srinivas}
\date{\today}

% Global font settings — safe and reliable
\setbeamerfont{frametitle}{size=\footnotesize}
\setbeamerfont{itemize/enumerate body}{size=\footnotesize}
\setbeamerfont{itemize/enumerate subbody}{size=\footnotesize}
\setbeamerfont{normal text}{size=\footnotesize}

\begin{document}

\frame{\titlepage}

\begin{frame}
\frametitle{Limitations of Supervised Fine-Tuning (SFT)}
\begin{itemize}
    \item SFT adapts LLMs by learning from labeled input-output pairs using next-token log-likelihood loss.
    \item However, SFT cannot enforce nuanced domain-specific goals such as:
    \begin{itemize}
        \item Long-term coherence and factual alignment. \\
        \textit{(e.g., In legal text, generating a consistent argument over multiple paragraphs.)}
        \item Adherence to domain-specific policies or constraints. \\
        \textit{(e.g., Medical models must avoid suggesting unverified treatments.)}
        \item Preference for concise or verbose reasoning depending on context. \\
        \textit{(e.g., Software Release Notes vs. Developer Documentation.  Same event or fact (a software update) is described differently depending on the reader- RL   can help by aligning outputs to contextual preferences — whether to be concise or elaborate)}
    \end{itemize}
    \item Outputs may be syntactically correct but semantically suboptimal for real-world domain needs. \\
    \textit{(e.g., A chemistry answer might look fluent but describe the wrong reaction conditions.)}
\end{itemize}
\end{frame}

\begin{frame}
\frametitle{Reinforcement Learning: A Policy-level Fine-Tuning Paradigm}
\begin{itemize}
    \item RL fine-tunes LLMs by treating them as policies \(\pi_\theta(y|x)\) that generate full sequences.
    \item Instead of optimizing log-likelihood, we optimize expected utility:
    \[
    \mathbb{E}_{y \sim \pi_\theta(\cdot|x)}[R(x, y)]
    \]
    \item \(R(x, y)\) is a reward function capturing desired properties such as:
    \begin{itemize}
        \item Task completion correctness (e.g., chemical process QA) \\
        \textit{(e.g., Correctly describing the heat exchanger role in ammonia synthesis.)}
        \item Preference alignment (via human feedback or a reward model) \\
        \textit{(e.g., Preferring answers that are easier to understand for high-school students.)}
        \item Domain-specific compliance (e.g., safety, legal constraints) \\
        \textit{(e.g., Never recommending toxic solvents in pharmaceutical synthesis.)}
        \item Response style adaptation (e.g., varying tone, formality, or verbosity) \\
        \textit{(e.g., Explaining a drug mechanism casually for patient education vs. formally in a research paper.)}
    \end{itemize}
\end{itemize}
\end{frame}

\begin{frame}
\frametitle{RL Fine-Tuning Workflow for Domain Adaptation}
\begin{enumerate}
    \item \textbf{Policy Initialization:} Use a pretrained instruction-tuned model \(\pi_{\theta_0}\) as the initial policy. \\
    \textit{(Start with a model already trained to follow instructions reasonably well.)}
    \item \textbf{Prompt Sampling:} Sample a batch of prompts \(x_i\) from domain-specific datasets. \\
    \textit{(e.g., Questions like “How is nitric acid produced?” from chemical engineering.)}
    \item \textbf{Response Generation:} Generate outputs \(y_i \sim \pi_\theta(y|x_i)\). \\
    \textit{(The model attempts to answer these domain questions.)}
    \item \textbf{Reward Assignment:} Evaluate responses using:
    \begin{itemize}
        \item Human-labeled preferences \textit{(e.g., Annotators prefer Answer A over B.)}
        \item Automated reward models \textit{(e.g., Another LLM checks if the answer is factually correct.)}
        \item Domain-specific constraints or verification tools \textit{(e.g., Using a simulator to verify reaction steps.)}
    \end{itemize}
    \item \textbf{Policy Optimization:} Update \(\pi_\theta\) using PPO, DPO, or GRPO to maximize reward. \\
    \textit{(Make the model learn to prefer high-quality answers that pass the reward test.)}
\end{enumerate}
\end{frame}

\begin{frame}
\frametitle{Advantages of RL-based Fine-Tuning in Domain-specific LLMs}
\begin{itemize}
    \item Directly aligns model behavior with domain-defined quality measures. \\
    \textit{(e.g., Optimizing to write safe and regulation-compliant chemical procedures.)}
    \item Enables preference-driven learning where gold outputs are ambiguous or subjective. \\
    \textit{(e.g., Multiple ways to describe the same medical procedure.)}
    \item Reduces hallucinations in sensitive domains like medicine or law. \\
    \textit{(e.g., Avoids fabricating case citations or nonexistent compounds.)}
    \item Allows continuous improvement by integrating live feedback from deployed systems. \\
    \textit{(e.g., Learning from user upvotes in a legal advice platform.)}
    \item Supports low-resource settings via reward models instead of labeled data. \\
    \textit{(e.g., No need for manual answers if we have a strong evaluator model.)}
\end{itemize}
\vspace{2mm}
\textbf{Use Cases:} Chemical Process QA, Scientific Text Generation, Legal Drafting, Medical Report Summarization
\end{frame}



\begin{frame}
\frametitle{LLM-Powered Framework for Automated PFD \& P\&ID Generation}
\begin{itemize}
    \item \textbf{Goal:} Automatically synthesize industrial-grade PFDs and P\&IDs given a chemical name (e.g., \texttt{"Nitric Acid"}).
    \item \textbf{Input:} Natural language chemical identifier (e.g., \texttt{"Ammonia"}, \texttt{"Hydrogen Peroxide"}).
    \item \textbf{Framework:} A \textbf{meta-agent} orchestrates the task using instruction-tuned \textbf{LLMs} and tool-augmented sub-agents.
    \begin{itemize}
        \item Subtasks include:
        \begin{itemize}
            \item Retrieving synthesis pathways via web search and Wikipedia APIs.
            \item Identifying major equipment and process steps (e.g., reactors, etc).
            \item Inferring instrumentation and control logic for P\&ID synthesis.
        \end{itemize}
        \item Sub-agents interface with external tools (e.g., DWSIM, Visual Paradigm) and chemical databases.
        \item Outputs are rendered in structured textual formats or exported to simulation environments.
    \end{itemize}
    \item \textbf{Output:}
    \begin{itemize}
        \item \textbf{PFD:} High-level flow of materials and energy across unit operations.
        \item \textbf{P\&ID:} Detailed instrumentation—sensors, valves, controllers (e.g., LIC-101, TIC-203).
    \end{itemize}
    \item \textbf{Applications:} Process design automation, DWSIM model bootstrapping, literature-to-process graph generation.
\end{itemize}
\end{frame}


\begin{frame}
\frametitle{Group Relative Policy Optimization (GRPO)}
\begin{itemize}
    \item \textbf{What is GRPO?}
    \begin{itemize}
        \item A \textbf{reinforcement learning} (RL) algorithm designed to fine-tune \textbf{large language models} (LLMs).
        \item Optimizes model behavior by ranking multiple generated responses for the same prompt.
    \end{itemize}
    \item \textbf{Key Idea:}
    \begin{itemize}
        \item Instead of using a separate value function, GRPO uses \textbf{group-based reward normalization}.
        \item Within each group (prompt-response set), responses are scored and normalized to estimate their relative quality.
        \item This allows for \textbf{advantage estimation} without a critic model.
    \end{itemize}
    \item \textbf{Benefits:}
    \begin{itemize}
        \item Simpler and more efficient than traditional policy optimization methods like \textbf{PPO (Proximal Policy Optimization)}.
        \item Enables stable RL fine-tuning even with limited feedback data.
    \end{itemize}
    \item \textbf{Applications:}
    \begin{itemize}
        \item Used in training models like \textbf{DeepSeek-R1} for math reasoning, QA, and code generation tasks.
    \end{itemize}
\end{itemize}
\end{frame}


\begin{frame}
\frametitle{Composite Reward GRPO: Our Extension for PFD/P\&ID Generation}
\begin{itemize}
    \item We extend standard GRPO to fine-tune a small-scale language model (SLM) for structured generation of process flow diagrams (PFDs) and piping and instrumentation diagrams (P\&IDs).
    \item The model is treated as a stochastic policy \(\pi_\theta(y \mid x)\), where \(x\) is a process synthesis query (e.g., \texttt{"Describe the PFD for nitric acid"}).
    \item For each prompt \(x\), we sample a group of outputs \(\mathcal{O}(x) = \{o_1, o_2, \dots, o_G\}\) from the old policy \(\pi_{\theta_{\text{old}}}\).
    \item We introduce a task-specific \textbf{composite reward}:
    \[
    r(o_i, r_x) = 0.3\, r^{\text{rouge}} + 0.2\, r^{\text{length}} + 0.5\, r^{\text{LLM-domain}}
    \]
    \item Reward components:
    \begin{itemize}
        \item \(r^{\text{rouge}}(o_i, r_x)\): Surface-level semantic alignment with reference descriptions.
        \item \(r^{\text{length}}(o_i, r_x)\): Penalizes verbosity mismatch (under/over-detailed).
        \item \(r^{\text{LLM-domain}}(o_i, r_x)\): Judgment from a domain-aligned reward model that verifies process structure (e.g., unit operations, flow direction, safety logic).
    \end{itemize}
    \item This domain-aware reward design guides the model to produce both technically accurate and well-structured PFD/P\&ID descriptions.
\end{itemize}
\end{frame}


\begin{frame}
\frametitle{Policy Optimization with Composite GRPO (Ours)}
\begin{itemize}
    \item After computing group-level rewards \(\{r_1, \dots, r_G\}\), we normalize them to obtain relative advantages:
    \[
    \mu_x = \frac{1}{G} \sum_{i=1}^G r_i \quad \quad \sigma_x = \sqrt{\frac{1}{G} \sum_{i=1}^G (r_i - \mu_x)^2}
    \]
    \[
    \widehat{A}_i = \frac{r_i - \mu_x}{\sigma_x}
    \]
    \item Each output \(o_i = (o_{i,1}, \dots, o_{i,T})\) represents a structured description of a PFD or P\&ID.
    \item For each token \(t\), compute the probability ratio between new and old policies:
    \[
    r_{i,t}(\theta) = \frac{\pi_\theta(o_{i,t} \mid x, o_{i,<t})}{\pi_{\theta_{\text{old}}}(o_{i,t} \mid x, o_{i,<t})}
    \]
    \item Our modified GRPO objective:
    \[
    J_{\text{GRPO}}(\theta) = \mathbb{E} \left[ \sum_{i=1}^G \sum_{t=1}^{|o_i|} \min \left( r_{i,t}(\theta)\widehat{A}_i, \operatorname{clip}(r_{i,t}(\theta))\widehat{A}_i \right) \right] - \beta D_{\mathrm{KL}}(\pi_\theta \| \pi_{\text{ref}})
    \]
    \item This policy update promotes domain-aligned structure generation, with no critic model required.
\end{itemize}
\end{frame}


\end{document}
