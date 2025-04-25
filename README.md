%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

\subsection{Width and Depth Pruning}
Transformer-based language models are computationally expensive. Pruning \cite{kim2024shortened, sun2025curse, wu2024llm, zhu2024survey, sun2023simple, sandri20252ssp, tang2025darwinlm, gao2024bypass, lu2024alphapruning} removes less critical components, reducing model size and inference costs while preserving accuracy. This enables efficient deployment of smaller, faster, and cheaper models in resource-constrained environments.
We consider a small-scale transformer-based language model represented as a parameterized function as follows,

\vspace{1mm}
\resizebox{0.985\linewidth}{!}{
\begin{minipage}{\linewidth}
\begin{equation}
\mathcal{F}_\theta: \mathbb{R}^{T \times d_{\text{model}}} \rightarrow \mathbb{R}^{T \times V} \nonumber
\end{equation}
\end{minipage}
}

\vspace{1mm}
where $T$ is the sequence length, $d_{\text{model}}$ is the hidden dimensionality of the model, and $V$ is the vocabulary size. The model consists of $L$ stacked transformer blocks $\{\mathcal{T}_\ell\}_{\ell=1}^L$, each comprising a Grouped Query Attention (GQA) module, a feedforward network (FFN), and residual connections, all wrapped in pre-layer normalization. GQA separates the number of query and key-value heads. Let $H_q$ and $H_{kv}$ denote the number of query and key-value heads, respectively, with $H_q > H_{kv}$. Let $g = H_q / H_{kv}$ be the number of query heads that share each key-value head, and $d_h = d_{\text{model}} / H_q$ the dimensionality per query head. Given input $\mathbf{X} \in \mathbb{R}^{T \times d_{\text{model}}}$, the linear projections are defined as follows,

\vspace{1mm}
\resizebox{0.985\linewidth}{!}{
\begin{minipage}{\linewidth}
\begin{equation}
\begin{split}
\mathbf{Q} = \mathbf{X} W^Q \in \mathbb{R}^{T \times H_q \times d_h}, \quad \mathbf{K} = \mathbf{X} W^K \in \mathbb{R}^{T \times H_{kv} \times d_h}, \\
\mathbf{V} = \mathbf{X} W^V \in \mathbb{R}^{T \times H_{kv} \times d_h} \nonumber
\end{split}
\end{equation}
\end{minipage}
}

\vspace{1mm}
For each query head $i \in \{1, \dots, H_q\}$, its associated key-value head is $k(i) = \lfloor i / g \rfloor$, and the attention output is as follows,

\vspace{1mm}
\resizebox{0.985\linewidth}{!}{
\begin{minipage}{\linewidth}
\begin{equation}
\mathbf{O}_i = \text{softmax} \left( \frac{\mathbf{Q}_i \mathbf{K}_{k(i)}^\top}{\sqrt{d_h}} \right) \mathbf{V}_{k(i)} \in \mathbb{R}^{T \times d_h} \nonumber
\end{equation}
\end{minipage}
}

\vspace{1mm}
The final GQA output is obtained via as follows,

\vspace{1mm}
\resizebox{0.985\linewidth}{!}{
\begin{minipage}{\linewidth}
\begin{equation}
\text{GQA}(\mathbf{X}) = \text{Concat}(\mathbf{O}_1, \dots, \mathbf{O}_{H_q}) W^O, \quad W^O \in \mathbb{R}^{d_{\text{model}} \times d_{\text{model}}} \nonumber
\end{equation}
\end{minipage}
}

\vspace{1mm}
\textbf{Width pruning} reduces the intermediate FFN dimensionality $d_{\text{ff}}$ by eliminating unimportant neurons. Decoder language models implement FFNs using Gated Linear Units (GLUs), expressed as 

\vspace{1mm}
\resizebox{0.985\linewidth}{!}{
\begin{minipage}{\linewidth}
\begin{equation}
\text{FFN}(\mathbf{h}) = W_2 \left( \phi(W_1^{(a)} \cdot \mathbf{h}) \odot (W_1^{(b)} \cdot \mathbf{h}) \right) \nonumber
\end{equation}
\end{minipage}
}

\vspace{1mm}
where $W_1^{(a)}, W_1^{(b)} \in \mathbb{R}^{d_{\text{ff}} \times d_{\text{model}}}$, $W_2 \in \mathbb{R}^{d_{\text{model}} \times d_{\text{ff}}}$, and $\odot$ denotes elementwise multiplication. Let $z_j$ denote the $j$-th neuron output from the GLU, and its importance is estimated by 

\vspace{1mm}
\resizebox{0.985\linewidth}{!}{
\begin{minipage}{\linewidth}
\begin{equation}
I_j = \mathbb{E}_{\mathbf{x} \sim \mathcal{D}} \left[ \left| \frac{\partial \mathcal{L}}{\partial z_j} \cdot z_j \right| \right] \nonumber
\end{equation}
\end{minipage}
}

\vspace{1mm}
where $\mathcal{L}$ is the task loss. Neurons with the lowest $I_j$ values are pruned, reducing the width to $\tilde{d}_{\text{ff}} < d_{\text{ff}}$ by removing rows in $W_1^{(a)}$, $W_1^{(b)}$ and the corresponding columns in $W_2$. \textbf{Depth pruning} removes entire transformer blocks based on their contribution to the task. For layer $\ell \in \{1, \dots, L\}$, its importance is computed as

\vspace{1mm}
\resizebox{0.985\linewidth}{!}{
\begin{minipage}{\linewidth}
\begin{equation}
I^{(\ell)} = \mathbb{E}_{\mathbf{x} \sim \mathcal{D}} \left[ \left| \left\langle \frac{\partial \mathcal{L}}{\partial \mathbf{h}^{(\ell)}}, \mathbf{h}^{(\ell)} \right\rangle \right| \right] \nonumber
\end{equation}
\end{minipage}
}

\vspace{1mm}
where $\mathbf{h}^{(\ell)}$ is the residual output of block $\ell$. Layers with small $I^{(\ell)}$ values are removed, and the retained set is denoted $\mathcal{S} \subset \{1, \dots, L\}$ with $|\mathcal{S}| = \tilde{L} \ll L$. For \textbf{joint pruning}, we introduce binary gates: $\gamma^{(\ell)} \in \{0,1\}$ for layer retention, and $g_j^{(\ell)} \in \{0,1\}$ for neuron retention in layer $\ell$. The forward computation becomes as follows,

\vspace{1mm}
\resizebox{0.985\linewidth}{!}{
\begin{minipage}{\linewidth}
\begin{equation}
\mathbf{h}^{(\ell)} = \mathbf{h}^{(\ell-1)} + \gamma^{(\ell)} \cdot \text{FFN}_\ell^{(g)}\left(\text{GQA}_\ell(\text{LN}(\mathbf{h}^{(\ell-1)}))\right) \nonumber
\end{equation}
\end{minipage}
}

\vspace{1mm}
with the gated FFN defined as follows,

\vspace{1mm}
\resizebox{0.985\linewidth}{!}{
\begin{minipage}{\linewidth}
\begin{equation}
\hspace{-5mm} \text{FFN}_\ell^{(g)}(\mathbf{h}) = \sum_{j=1}^{d_{\text{ff}}} g_j^{(\ell)} \cdot \left[ W_2[:,j] \cdot \left( \phi(W_1^{(a)}[j,:] \cdot \mathbf{h}) \cdot (W_1^{(b)}[j,:] \cdot \mathbf{h}) \right) \right] \nonumber
\end{equation}
\end{minipage}
}

The constrained optimization objective combines empirical risk and sparsity penalties, where the goal is to minimize task loss while promoting structured sparsity (i.e., layer and neuron pruning) as follows,

\vspace{-1mm}
\resizebox{0.985\linewidth}{!}{
\begin{minipage}{\linewidth}
\begin{equation}
\hspace{-5mm} \min_{\theta, \gamma, g} \mathbb{E}_{(\mathbf{x}, y) \sim \mathcal{D}} \left[ \mathcal{L}(\mathcal{F}_{\theta, \gamma, g}(\mathbf{x}), y) \right] + \lambda_1 \sum_{\ell=1}^L (1 - \gamma^{(\ell)}) + \lambda_2 \sum_{\ell=1}^L \sum_{j=1}^{d_{\text{ff}}} (1 - g_j^{(\ell)}) \nonumber
\end{equation}
\end{minipage}
}

Here, \( \mathcal{D} \) denotes the training data distribution, and \( (\mathbf{x}, y) \) represents an input-output pair sampled from \( \mathcal{D} \). The function \( \mathcal{F}_{\theta, \gamma, g} \) corresponds to the pruned model, in which specific layers (controlled by \( \gamma^{(\ell)} \)) and neurons (controlled by \( g_j^{(\ell)} \)) are selectively removed. The loss function \( \mathcal{L}(\cdot, \cdot) \), such as cross-entropy, quantifies the task-specific prediction error. The depth sparsity penalty term, \( \lambda_1 \sum_{\ell=1}^L (1 - \gamma^{(\ell)}) \), counts the number of pruned transformer layers, where \( \lambda_1 \) is a regularization hyperparameter that governs the penalty for pruning entire layers. Similarly, the width sparsity penalty term, \( \lambda_2 \sum_{\ell=1}^L \sum_{j=1}^{d_{\text{ff}}} (1 - g_j^{(\ell)}) \), captures the total number of pruned neurons across all FFNs, with \( \lambda_2 \) acting as the corresponding regularization weight that controls the extent of neuron-level pruning. To enable end-to-end differentiability, we relax the binary gates using the Concrete distribution. Each gate \( g_j^{(\ell)} \in [0, 1] \) is sampled as:

\vspace{-1mm}  
\resizebox{0.985\linewidth}{!}{  
\begin{minipage}{\linewidth}  
\begin{equation}  
\hspace{-5mm} g_j^{(\ell)} = \text{Sigmoid} \left( \frac{1}{\tau} \left( \log \alpha_j^{(\ell)} + \log u - \log(1 - u) \right) \right), \quad u \sim \mathcal{U}(0, 1) \nonumber  
\end{equation}  
\end{minipage}  
}  

Here, \( \alpha_j^{(\ell)} \) is a learnable logit parameter, and \( \tau > 0 \) is the temperature that controls the smoothness of the relaxation. A similar sampling strategy is applied to the layer-level gates \( \gamma^{(\ell)} \). In short, the joint pruning objective function jointly optimizes task performance and model sparsity by minimizing loss while penalizing the number of active layers and neurons. It uses regularization weights \( \lambda_1 \) and \( \lambda_2 \) to control the trade-off between accuracy and compression. The optimization learns both the pruned model structure and the corresponding parameters \( \theta \). Since pretraining is prohibitively expensive, we adopt QLoRA (Quantized Low-Rank Adaptation) to efficiently fine-tune the pretrained model in tandem with pruning. We propose an efficient pruning framework that combines QLoRA with structured sparsity, where the pretrained weights \( W_i^{\text{frozen}} \) remain quantized (4-bit NF4) and frozen, while trainable low-rank adapters \( W_i = W_i^{\text{frozen}} + A_iB_i^\top \), with \( A_i \in \mathbb{R}^{d \times r} \), \( B_i \in \mathbb{R}^{r \times d} \),  and \( r \ll d \), are injected into each layer. Pruning is guided by neuron importance \( I_j = \mathbb{E}[|\frac{\partial\mathcal{L}}{\partial z_j} \cdot z_j|] \) and layer importance \( I^{(\ell)} = \mathbb{E}[|\langle \frac{\partial\mathcal{L}}{\partial \mathbf{h}^{(\ell)}}, \mathbf{h}^{(\ell)} \rangle|] \). Differentiable pruning is enabled during training via continuous gates \( g_j^{(\ell)} = \text{sigmoid}((\log \alpha_j^{(\ell)} + \mathcal{L}_{\text{logit}})/\tau) \). After training, components with binarized gates \( \gamma^{(\ell)}, g_j^{(\ell)} = 0 \) are removed, while active paths are retained in the form \( W_i^{\text{frozen}} + A_iB_i^\top \), preserving 4-bit storage for the base weights. This joint optimization enables compression through (1) 4-bit quantization, (2) structured sparsity, and (3) parameter-efficient adaptation—while maintaining downstream task accuracy. Please note that pruning is applied to the active model—frozen 4-bit weights plus LoRA adapters—during fine-tuning, removing unimportant layers and neurons based on their joint contribution. Pruned components, including their adapters, are discarded, while retained layers and neurons preserve both frozen weights and learned low-rank updates.

%%%%%%%%%%%%%%%%%%%%%%%%%  Pruning Results  %%%%%%%%%%%%%%%%%%%%%%%%%%%%

\subsubsection{\textbf{The Impact of Width and Depth Pruning on Model Performance: A Qualitative Analysis}}
Figure \ref{fig:pruning_impact_test} illustrates the effects of width and depth pruning on model performance on using five qualitative metrics scored from 0 to 4 using pretrained Nemotron-4-340B-Reward model. Both pruning methods generally lead to a decrease in performance scores as the pruning percentage increases. Width pruning (Figure \ref{fig:pruning_impact_test}a) particularly impacts correctness, complexity, and helpfulness, showing noticeable drops especially at the 20\% level, while coherence remains relatively high. Depth pruning (Figure \ref{fig:pruning_impact_test}b) shows a more pronounced negative effect on coherence and complexity, particularly at higher pruning ratios (20\% and 50\%). Correctness appears somewhat robust to low levels of depth pruning (1-5\%) but declines significantly thereafter. Across both methods, verbosity remains the least affected metric at low to moderate pruning levels. These results highlight the trade-off between model compression and performance, indicating that while pruning can reduce model size/complexity, it comes at the cost of quality, with different pruning strategies impacting specific aspects like coherence or correctness more significantly. Figures \ref{fig:width_prune_chemeval}(c) and \ref{fig:depth_prune_chemeval}(d) illustrate the impact of width and depth pruning, respectively, on model performance during ChemEval benchmark tasks, which involve PFD/PID generation for unseen chemicals. Both figures demonstrate that increasing the pruning percentage generally degrades the quality of the model's responses across all five metrics. Specifically, width pruning (Figure \ref{fig:width_prune_chemeval}(c)) shows that higher pruning levels (particularly 20\%) lead to significant reductions in correctness and complexity scores. Depth pruning (Figure \ref{fig:depth_prune_chemeval}(d)) also reduces overall quality, with coherence and correctness being notably impacted as pruning percentages increase, showing a steady decline especially at 20\% and 50\% levels. Across both width and depth pruning applied to these ChemEval tasks, verbosity remained the least affected metric. These results indicate that structural compression via either width or depth pruning hinders the model's ability to effectively generate accurate and coherent PFD/PID descriptions for novel chemical processes. Figures \ref{fig:prun}e and \ref{fig:prun}f present the relationship between computational time and the percentage of width and depth pruning applied, respectively. Figure \ref{fig:prun}e shows that increasing the width pruning percentage correlates with a decrease in computational time: the baseline (0\%) took approximately 1350.2 minutes, 5\% pruning took 1269.1 minutes, and 20\% pruning took 1066.2 minutes. Similarly, Figure \ref{fig:prun}f indicates that increasing depth pruning percentage also leads to reduced computational time. The baseline time was 1350.2 minutes, decreasing incrementally through 1\% (1342.8 min), 5\% (1296.2 min), and 20\% (1120.7 min), with the most significant reduction observed at 50\% depth pruning (796.6 minutes). Both figures illustrate a consistent inverse relationship between the applied pruning percentage and the measured computational time.

\vspace{-2mm}
\subsection{KV Caching - Paged Attention}  
We implement a critical optimization technique for improving the memory efficiency and computational throughput of fined-tuned, pre-trained (SLMs) during autoregressive decoding (i.e., generating text token-by-token). In autoregressive transformer decoding, each generated token \( x_i \) (at step \( i \)) computes a query vector \( q_i \in \mathbb{R}^d \) and attends to all previous tokens \( x_1, \dots, x_{i-1} \) via their key vectors \( k_j \in \mathbb{R}^d \) and value vectors \( v_j \in \mathbb{R}^d \). The attention mechanism computes a weighted sum over the values based on the query-key interactions:  

\vspace{1mm}  
\resizebox{0.985\linewidth}{!}{  
\begin{minipage}{\linewidth}  
\begin{equation}
\text{Attention}(q_i, K, V) = \sum_{j=1}^{i-1} \text{softmax}\left( \frac{q_i^\top k_j}{\sqrt{d}} \right) v_j \nonumber
\end{equation}  
\end{minipage}  
}  

Here, \( K = [k_1, \dots, k_{i-1}] \in \mathbb{R}^{(i-1) \times d} \) and \( V = [v_1, \dots, v_{i-1}] \in \mathbb{R}^{(i-1) \times d} \) represent the cached key-value (KV) matrices. The memory contiguity problem arises because this KV cache grows dynamically, requiring storage of \((i-1) \times d\)-dimensional matrices per layer and head at each step \(i\). The linearly growing KV cache in standard autoregressive attention consumes substantial memory, leading to fragmentation and restricting batch sizes. This, combined with its quadratic computational cost that increases per-token latency, significantly hinders overall throughput. Conventionally, the KV cache is stored contiguously, necessitating pre-allocation of a fixed-size buffer (for a maximum sequence length \(L_{\text{max}}\)) for each sequence to avoid costly reallocations. However, since sequences vary in length and grow dynamically, this approach is inefficient. It leads to internal fragmentation, where memory within an allocated block is wasted when the actual sequence length \(L\) is much smaller than \(L_{\text{max}}\). More critically, it causes external fragmentation: when multiple sequences run concurrently, each is allocated a large contiguous block, and as sequences complete at different times, their blocks are freed, leaving gaps of varying sizes between active blocks. Over time, GPU memory becomes a patchwork of allocated blocks and free spaces, and even if the total free memory is sufficient, it may be split into small, non-contiguous chunks. Consequently, a new request for a large contiguous block (required for a new sequence) may fail because no single free chunk is large enough, despite sufficient total free memory. Furthermore, reallocating the cache for sequences exceeding the pre-allocated space incurs significant \(O(L)\) time and memory overhead. These combined inefficiencies result in fragmented GPU memory, reducing achievable batch sizes and overall throughput during model serving.  To address the memory inefficiencies of traditional KV caching, PagedAttention \cite{kwon2023efficient, rehg2024kv, prabhu2024vattention} adapts the concept of virtual memory paging from operating systems.
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

Instead of requiring contiguous memory allocation per sequence - which leads to fragmentation - it implements a more flexible approach to KV cache management. The system divides each sequence's key-value cache into fixed-size KV blocks, where each block contains the keys and values for exactly B tokens.  We can formally define the j-th KV block as:

\vspace{1mm}  
\resizebox{0.985\linewidth}{!}{  
\begin{minipage}{\linewidth}  
\begin{equation}
\hspace{-5mm} K_j = [k_{(j-1)B+1}, ..., k_{jB}] \in \mathbb{R}^{B \times d}, \quad V_j = [v_{(j-1)B+1}, ..., v_{jB}] \in \mathbb{R}^{B \times d} \nonumber
\end{equation}  
\end{minipage}  
}  

\vspace{1mm}
The innovation lies in the block table per sequence that maps logical block indices to physical memory locations. This indirection enables three key features: (1) non-contiguous storage where blocks can reside anywhere in GPU memory, (2) on-demand allocation where physical blocks are allocated only when needed, and (3) efficient memory sharing where multiple sequences can reference the same blocks, particularly useful for shared prompts. Rather than computing attention over all past \( i-1 \) tokens, it reformulates this as a block-wise computation: For token position i, the operation:

\vspace{-1mm}
\resizebox{0.985\linewidth}{!}{
\begin{minipage}{\linewidth}
\begin{equation}
a_{ij}^{(b)} = \frac{\exp(q_i^\top k_{j_b} / \sqrt{d})}{\sum_{t=1}^{\lceil i/B \rceil} \sum_{b=1}^{B} \exp(q_i^\top k_{t_b} / \sqrt{d})}, \quad o_i = \sum_{j=1}^{\lceil i/B \rceil} V_j \cdot a_{ij}^\top \nonumber
\end{equation}
\end{minipage}
}


\vspace{1mm}
Here, \( a_{ij} \) is the attention weight vector over the \( j \)-th KV block (indexed block-wise), and \( V_j \in \mathbb{R}^{B \times d} \). The score matrix is computed block-wise as \( A_{ij} = q_i^\top K_j / \sqrt{d} \in \mathbb{R}^B \), and the softmax normalization is computed across all \( \lceil i/B \rceil \) blocks. The implementation achieves this through block-aware memory access patterns, efficient physical location resolution via block tables, and optimized memory prefetching for consecutive blocks. This design provides significant advantages by eliminating both internal fragmentation (through fixed-size blocks) and external fragmentation (via non-contiguous allocation). It enables memory sharing through copy-on-write semantics for shared prefixes while maintaining identical outputs to standard attention. The result is dramatically improved memory utilization that enables larger batch sizes, longer sequence handling, and better overall throughput - crucial benefits for production serving systems. While PagedAttention addresses memory fragmentation and enables non-contiguous block-level key-value (KV) caching, it does not reduce the per-parameter memory footprint, as each key and value vector is typically stored in high-precision formats such as FP32 or FP16, which remain memory-intensive. To further improve efficiency, we adopt group-wise quantization for KV cache compression, a technique that reduces memory usage per parameter without requiring retraining during autoregressive decoding. In this approach, each cached KV block—comprising the key matrix \( K_j \in \mathbb{R}^{B \times d} \) and value matrix \( V_j \in \mathbb{R}^{B \times d} \)—is partitioned into column-wise groups, with each group quantized independently using its own scaling factor \( \alpha_g \) and optional zero-point \( z_g \). The quantization process for a group \( g \) in \( K_j \) is defined as 

\vspace{1mm}
\resizebox{0.985\linewidth}{!}{
\begin{minipage}{\linewidth}
\begin{equation}
\tilde{K}_j^{(g)} = \text{round}\left( \frac{K_j^{(g)} - z_g}{\alpha_g} \right), \quad K_j^{(g)} \approx \alpha_g \cdot \tilde{K}_j^{(g)} + z_g \nonumber
\end{equation}
\end{minipage}
}

\vspace{1mm}
where \( \tilde{K}_j^{(g)} \in \mathbb{Z}^{B \times d_g} \) represents the quantized values, and the original values are approximated as \( K_j^{(g)} \approx \alpha_g \cdot \tilde{K}_j^{(g)} + z_g \). The same procedure applies to the value matrix \( V_j \). To maintain model fidelity, we incorporate second-order Hessian information during quantization, which reflects the curvature of the loss landscape, allowing aggressive quantization of low-curvature weights while preserving high-curvature ones more precisely. This Hessian-aware approach enables accurate low-bit (e.g., 4-bit) quantization with minimal performance degradation. The fixed block structure of PagedAttention ensures efficient dequantization during inference, as each block’s group-wise metadata (scales and zero-points) is stored and accessed predictably. Together, PagedAttention and group-wise quantization provide complementary benefits: PagedAttention eliminates memory fragmentation through non-contiguous block management, while quantization drastically reduces the per-parameter memory footprint. This combination enables larger batch sizes, support for longer sequences, and improved inference throughput, making it essential for deploying large language models efficiently on memory-constrained hardware.

\subsubsection{\textbf{Inference Efficiency Gains with Paged Attention}}
\label{sec:paged_attention_results} 
To complement model-level optimizations such as pruning, we evaluated inference-time efficiency gains using Paged Attention \cite{kwon2023efficient}, a technique that manages the Key-Value (KV) cache in non-contiguous, fixed-size blocks. This design mitigates internal and external memory fragmentation inherent in standard contiguous KV caching, and significantly enhances inference performance. Importantly, Paged Attention is an inference-only optimization that does not affect the model's output quality. Therefore, this evaluation focuses solely on system-level performance metrics—distinct from quality metrics such as BLEU, ROUGE, or reward scores assessed elsewhere. The efficiency metrics measured include inference throughput (tokens per second), maximum achievable batch size, peak GPU memory utilization (in GB), and average per-sequence generation latency (in seconds). We benchmarked our best-performing fine-tuned model, LLaMA-3.2 1B with fine-tuning and Graph RAG (Variant A), on an NVIDIA H100 GPU using a 500-example subset of the held-out dataset. The results, shown in Figure~\ref{fig:paged_attention_comparison}, demonstrate significant efficiency improvements.
Paged Attention enabled an approximately 2.0× increase in the maximum batch size (e.g., batch size 16 versus 8) and improved inference throughput by nearly 1.8× (e.g., ~100 vs. 55 tokens/sec) compared to the baseline. While the LLaMA-3.2 1B model itself requires only ~2.3 GB of VRAM in FP16 precision, achieving the larger batch size with Paged Attention increased peak GPU memory usage slightly (~4.8 GB vs. ~4.5 GB) due to greater sequence parallelism. Nonetheless, memory utilization was substantially more efficient owing to reduced fragmentation. The average generation latency per 2048-token sequence was approximately 39.8 seconds, with only a marginal increase—around 5–10\%—attributable to the overhead of block management. These findings highlight the practical benefits of Paged Attention for serving fine-tuned SLMs, especially in RAG-based applications like PFD and PID generation, which involve long and variable-length contexts. This technique complements model-centric strategies such as fine-tuning and pruning, contributing to a more scalable and efficient framework for real-world engineering deployments.

\begin{figure*}[ht!]
\centering
\includegraphics[width=0.9\linewidth]{Images/paged_attention_comparison.pdf} % Replace with your actual figure file
\vspace{-3mm}
\caption{Comparative inference performance metrics (e.g., throughput, max batch size, peak memory for max batch) for the fine-tuned Llama-3.2 1B model with and without Paged Attention, highlighting the efficiency gains achieved.} % Updated caption slightly
\label{fig:paged_attention_comparison}
\vspace{-3mm}
\end{figure*}
