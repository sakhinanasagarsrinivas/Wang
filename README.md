\section{Submission of papers to NeurIPS 2025}


\section{Methodology}
We extend the DRAGIN framework to the multimodal setting by proposing MM-DRAGIN (Multimodal DRAGIN: Dynamic Textual Retrieval-Augmented Generation for Vision-Language Models), a retrieval-augmented generation mechanism tailored for **Vision-Language Models (VLMs). MM-DRAGIN enables a VLM to generate text conditioned on both a static image and a textual prompt. Crucially, during generation, the model dynamically determines *when* external textual knowledge should be retrieved and what query should be issued, guided by internal measures of predictive uncertainty and multimodal attention. In contrast to existing retrieval-augmented approaches, MM-DRAGIN introduces several key innovations. First, it maintains a fixed visual input, without performing any form of image retrieval. Second, it dynamically triggers retrieval based on the model's internal assessment of multimodal information needs. Third, it restricts retrieval to a static textual corpus, ensuring consistency and interpretability. Finally, it leverages the VLM's internal attention maps to construct retrieval queries that are contextually grounded and image-aware, yet text-only. Formally, we consider a VLM that receives three inputs: (i) an image $\mathcal{I} \in \mathbb{R}^{H \times W \times C}$, (ii) a text prompt $\mathbf{T}_{\text{prompt}} = (t_1, \dots, t_m)$, and (iii) a partially generated text sequence up to time step $t-1$, denoted $\mathbf{T}_{\text{gen}}^{<t} = (g_1, \dots, g_{t-1})$. The model's current multimodal generation context at step $t$ is represented as:

\begin{minipage}{\linewidth}
\begin{equation}
\mathcal{C}_t = \{ \mathbf{I}_{\text{emb}}, \mathbf{T}_{\text{prompt}}, \mathbf{T}_{\text{gen}}^{<t} \},
\label{eq:context}
\end{equation}
\end{minipage}

where $\mathbf{I}_{\text{emb}} = (\mathbf{v}_1, \dots, \mathbf{v}_K) \in \mathbb{R}^{K \times d}$ denotes the image embeddings obtained from a pretrained vision encoder (such as ViT), and $d$ is the shared hidden dimensionality used across both visual and textual tokens. This unified context serves as the basis for dynamic retrieval decisions and attention-based query formulation throughout the generation process.

\subsection{MM-RIND: Multimodal Real-Time Information Needs Detection}
To determine whether external textual retrieval should be triggered during the generation process, MM-DRAGIN introduces a module called MM-RIND (Multimodal Real-Time Information Needs Detection). This mechanism operates at each generation step $t$ and computes a retrieval necessity score that estimates the model's current need for additional information. The score is based on three key factors: the model's uncertainty about its next token prediction, the downstream influence of the last generated token, and its semantic informativeness. Formally, the score is defined as:

\begin{minipage}{\linewidth}
\begin{equation}
S_{\text{MM-RIND}}(g_{t-1}) = H_t \cdot a_{\text{max}}(g_{t-1}) \cdot s(g_{t-1}),
\label{eq:mm-rind}
\end{equation}
\end{minipage}

where $S_{\text{MM-RIND}}(g_{t-1}) \in \mathbb{R}_{\geq 0}$ denotes the **information need score** associated with the most recently generated token $g_{t-1}$. Retrieval is triggered if this score exceeds a tunable threshold $\theta \in \mathbb{R}_{> 0}$. Each multiplicative term in Equation (\ref{eq:mm-rind}) serves a distinct purpose: $H_t$ quantifies **predictive uncertainty** at the next token, $a_{\text{max}}(g_{t-1})$ captures the **influence** of the last token on future generations, and $s(g_{t-1})$ acts as a **semantic gating function** that filters out irrelevant tokens such as stopwords. The first component, $H_t$, measures the entropy of the VLM's next-token probability distribution conditioned on the current multimodal context $\mathcal{C}_t$. It is defined as:

\begin{minipage}{\linewidth}
\begin{equation}
H_t = - \sum_{v \in \mathcal{V}_{\text{text}}} p(v \mid \mathcal{C}_t) \log p(v \mid \mathcal{C}_t),
\label{eq:entropy}
\end{equation}
\end{minipage}

where $\mathcal{V}_{\text{text}}$ is the model's output vocabulary, and $p(v \mid \mathcal{C}_t)$ is the probability assigned to token $v$ given the current context $\mathcal{C}_t = \{ \mathbf{I}_{\text{emb}}, \mathbf{T}_{\text{prompt}}, \mathbf{T}_{\text{gen}}^{<t} \}$ as defined in Equation (\ref{eq:context}). Intuitively, a higher entropy value indicates greater uncertainty in selecting the next token, which may imply insufficient knowledge grounded in the current context—thus motivating external retrieval. The second component, $a_{\text{max}}(g_{t-1})$, evaluates the importance of the most recent token $g_{t-1}$ by examining how much attention subsequent tokens are likely to pay to it. Specifically, we consider the attention matrix $A \in \mathbb{R}^{L \times L}$ from the final layer of the Transformer, where $L$ denotes the total length of the multimodal sequence (including image patches, prompt tokens, and generated tokens). Letting $\text{idx}(g_{t-1})$ denote the index of $g_{t-1}$ in this sequence, we define:

\begin{minipage}{\linewidth}
\begin{equation}
a_{\text{max}}(g_{t-1}) = \max_{\substack{k > \text{idx}(g_{t-1}) \\ \text{token}_k \in \text{Text}}} A_{k, \text{idx}(g_{t-1})}.
\label{eq:max-attention}
\end{equation}
\end{minipage}

This expression captures the maximum attention weight assigned to $g_{t-1}$ by any future text token $g_k$ where $k > t - 1$. A higher value of $a_{\text{max}}(g_{t-1}) \in [0, 1]$ suggests that $g_{t-1}$ plays a critical role in guiding future generation and is therefore a strong candidate for triggering retrieval. The final term, $s(g_{t-1})$, introduces a simple semantic filtering mechanism that prevents retrieval triggers on tokens with little informative value. It is defined as a binary indicator:

\begin{minipage}{\linewidth}
\begin{equation}
s(g_{t-1}) =
\begin{cases}
1, & \text{if } g_{t-1} \notin \mathcal{S} \\
0, & \text{if } g_{t-1} \in \mathcal{S},
\end{cases}
\label{eq:semantic-filter}
\end{equation}
\end{minipage}

where $\mathcal{S} \subset \mathcal{V}_{\text{text}}$ is a predefined stopword set (e.g., articles, conjunctions, prepositions). The function $s(\cdot)$ ensures that retrieval is not wasted on syntactically frequent but semantically weak tokens. Together, Equations (\ref{eq:mm-rind}) through (\ref{eq:semantic-filter}) compose a principled formulation for detecting retrieval needs in a multimodal generation setting. By combining uncertainty, influence, and semantic filtering, MM-RIND enables retrieval to be selectively activated at generation steps where the VLM is both knowledge-hungry and structurally dependent on its immediate past, leading to more effective and efficient use of external textual resources.






\subsection{MM-QFS: Multimodal Query Formulation via Self-Attention}

When the retrieval score $S_{\text{MM-RIND}}(g_{t-1})$ exceeds the threshold $\theta$, MM-DRAGIN triggers a retrieval event. The subsequent challenge is to formulate a precise and contextually relevant query that captures the model's current information need. To achieve this, we introduce **MM-QFS** (Multimodal Query Formulation via Self-Attention), a mechanism that constructs a textual query by leveraging the VLM's self-attention maps. Specifically, we analyze how the next token $g_t$ attends to previously seen tokens during its generation. Let $A \in \mathbb{R}^{L \times L}$ denote the final-layer attention matrix, where $L$ is the length of the flattened multimodal sequence. From this matrix, we define the attention vector:

\begin{minipage}{\linewidth}
\begin{equation}
\boldsymbol{\alpha}*t = (\alpha\_t(j))*{j \in \mathcal{M}*{\text{text}}}, \quad \alpha\_t(j) = A*{\text{idx}(g\_t), j},
\label{eq\:qfs-attn}
\end{equation}
\end{minipage}

where $\alpha_t(j) \in [0, 1]$ is the attention weight assigned by token $g_t$ to a prior token $j$, and $\text{idx}(g_t)$ is the index of $g_t$ in the flattened sequence. The index set $\mathcal{M}_{\text{text}} \subset \{1, \ldots, L\}$ refers to all prior **textual** tokens in $\mathbf{T}_{\text{prompt}} \cup \mathbf{T}_{\text{gen}}^{<t}$, explicitly excluding visual tokens from $\mathbf{I}_{\text{emb}}$. This design choice ensures that the formulated query is purely textual, yet remains implicitly conditioned on the image, as the attention weights themselves are modulated by the full multimodal context during training.

To construct the retrieval query, we identify the top-$n$ text tokens receiving the highest attention from $g_t$. Let $\mathcal{T}_{\text{top}}$ denote the top-$n$ indices in $\mathcal{M}_{\text{text}}$ based on $\alpha_t(j)$:

\begin{minipage}{\linewidth}
\begin{equation}
\mathcal{T}*{\text{top}} = \text{arg top-}n \left( { \alpha\_t(j) \mid j \in \mathcal{M}*{\text{text}} } \right).
\label{eq\:qfs-topn}
\end{equation}
\end{minipage}

These top-$n$ tokens are then ordered according to their original sequence positions and concatenated to form the textual query $Q_t$:

\begin{minipage}{\linewidth}
\begin{equation}
Q\_t = \text{Concat}(t\_{j\_1}, \ldots, t\_{j\_n}), \quad j\_k \in \mathcal{T}\_{\text{top}}.
\label{eq\:qfs-query}
\end{equation}
\end{minipage}

The resulting query $Q_t$ is thus a distilled representation of the most semantically and structurally relevant parts of the prior textual context, as interpreted by the VLM at the moment it begins to generate $g_t$. While image tokens are excluded from the query explicitly, their influence is implicitly captured via the attention mechanism and hidden state conditioning, enabling the formulation to remain image-aware without introducing visual noise into the retrieval pipeline.

\subsection{Text Retrieval and Augmented Generation}

Once the query $Q_t$ is formed, it is submitted to a text-only retriever (e.g., BM25 or a dense dual encoder) to fetch relevant documents from an external corpus $\mathcal{C}_{\text{text}}$. The retriever returns the top-$k$ documents:

\begin{minipage}{\linewidth}
\begin{equation}
\mathcal{D}*t = { D\_t^{(1)}, D\_t^{(2)}, \ldots, D\_t^{(k)} }, \quad D\_t^{(i)} \in \mathcal{C}*{\text{text}}.
\label{eq\:retrieved-docs}
\end{equation}
\end{minipage}

These retrieved documents are then embedded into the input prompt to augment the generation context for token $g_t$. The new generation prompt combines (i) the original image embeddings $\mathbf{I}_{\text{emb}}$, (ii) the prompt and previously generated tokens, and (iii) the retrieved knowledge snippets, resulting in a multimodal-augmented input of the form:

\begin{quote}
\small
\texttt{\[Image Tokens: $\mathbf{I}_{\text{emb}}$]}

\texttt{Original Prompt: $\mathbf{T}_{\text{prompt}}$}

\texttt{Generated Text So Far: $\mathbf{T}_{\text{gen}}^{<t}$}

\texttt{Additional Knowledge:}

\texttt{\[1] $D_t^{(1)}$}

\texttt{\[2] $D_t^{(2)}$}

\texttt{...}

\texttt{Continue generating:}
\end{quote}

The final input to the VLM thus becomes an enriched context:

\begin{minipage}{\linewidth}
\begin{equation}
\mathcal{C}*t^{\text{aug}} = \left{ \mathbf{I}*{\text{emb}}, \mathbf{T}*{\text{prompt}}, \mathbf{T}*{\text{gen}}^{\<t}, \mathcal{D}\_t \right},
\label{eq\:augmented-context}
\end{equation}
\end{minipage}

from which the model resumes generation at step $t$. Notably, the image remains fixed throughout the process, serving as a consistent source of visual grounding, while retrieval augments the textual context only when necessary.

\subsection{Summary of MM-DRAGIN Mechanism}

MM-DRAGIN introduces a dynamic, model-internal mechanism for retrieval-augmented generation in multimodal contexts. The query $Q_t$, constructed from attention-weighted textual elements, reflects the model's current semantic gap and guides the retrieval process accordingly. The resulting documents $\mathcal{D}_t$ are inserted into the prompt without altering the visual context, enabling efficient and adaptive integration of external textual knowledge. This formulation preserves the lightweight, plug-in design philosophy of DRAGIN while extending its applicability to vision-language settings with persistent image grounding and dynamic, context-aware text retrieval.
