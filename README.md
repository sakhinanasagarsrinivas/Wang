\documentclass[a4paper,10pt]{article}
\usepackage[left=1in, right=1in, top=1in, bottom=1in]{geometry}
\usepackage{hyperref}
\usepackage{enumitem}
\usepackage{titlesec}
\usepackage{fancyhdr}
% \usepackage{tikz} % TikZ is loaded but not used, can be commented out or removed if not needed.
\usepackage{xcolor}

\hypersetup{
    colorlinks=true,      % Enable colored links
    linkcolor=blue,       % Color of internal links (you can leave this as is if not needed)
    urlcolor=blue         % Color of external links (this will apply to your arXiv link)
}

% Set page style
\pagestyle{fancy}
\fancyhf{}
\rhead{\thepage}

% Section formatting
\titleformat{\section}[block]{\normalfont \LARGE \bfseries \color{blue}}{\thesection}{1em}{}
\titleformat{\subsection}[block]{\normalfont \large \bfseries}{\thesubsection}{1em}{}
\titleformat{\subsubsection}[runin]{\normalfont \itshape}{\thesubsubsection}{1em}{}

% Set list spacing and style
\setlist[itemize]{left=1em, label=\textbullet, topsep=0pt, partopsep=0pt}

% Header formatting (Contact Information)
\newcommand{\resumeHeader}{% <--- Added % to avoid potential space
  \begin{center}
    {\LARGE \textbf{S Sagar Srinivas}} \\[0.2em]
    \href{mailto:sagar.sakhinana@tcs.com}{sagar.sakhinana@tcs.com} | (123) 456-7890 | \href{https://www.yourwebsite.com}{www.yourwebsite.com} | LinkedIn: \href{https://www.linkedin.com/in/yourname}{yourname}
  \end{center}% <--- Added % to avoid potential space
  \noindent\rule{\textwidth}{0.4pt}% <--- Moved rule inside the command definition
} % <--- This brace now correctly closes the \newcommand definition


\begin{document}

% Header
\resumeHeader

% Summary
\section*{Summary}
A highly motivated AI researcher with extensive experience in machine learning, deep learning, and artificial intelligence technologies. Proven track record of publishing in top-tier AI conferences, contributing to impactful research, and filing multiple patents. Adept at solving complex problems with innovative approaches and passionate about advancing AI technology.

% Education Section
\section*{Education}
\noindent
\textbf{Your University Name}, City, State \hfill YYYY--YYYY \\
\textit{Ph.D. in Artificial Intelligence} \\
Thesis: \textit{Title of Your Dissertation or Research Topic} \\
Advisor: Dr. Advisor's Name

\noindent
\textbf{Your University Name}, City, State \hfill YYYY--YYYY \\
\textit{Master of Science in Computer Science} \\
Thesis: \textit{Title of Your Thesis}

\noindent
\textbf{Your University Name}, City, State \hfill YYYY--YYYY \\
\textit{Bachelor of Science in Computer Science}

% Experience Section
\section*{Experience}
\noindent
\textbf{Your Current Job Title} \hfill YYYY--Present \\
Company Name, City, State
\begin{itemize}
  \item Led AI research initiatives that resulted in X publications in top-tier conferences.
  \item Developed novel machine learning algorithms for [specific project or field].
  \item Filed Y patents related to [specific AI technology].
  \item Collaborated with cross-disciplinary teams to design and deploy AI solutions in [industry/applications].
\end{itemize}
\vspace{5mm}
\noindent
\textbf{Your Previous Job Title} \hfill YYYY--YYYY \\
Company Name, City, State
\begin{itemize}
  \item Contributed to multiple AI projects with a focus on [specific AI technologies].
  \item Developed tools and frameworks used by [teams, organizations, or clients].
  \item Published X papers in conferences like NeurIPS, ICML, CVPR, etc.
\end{itemize}

% Skills Section
\section*{Skills}
\begin{itemize}
  \item \textbf{Programming Languages:} Python, C++, Bash, SQL, LaTeX
  \item \textbf{Frameworks \& Libraries:} PyTorch, TensorFlow, Hugging Face Transformers, Scikit-learn, RDKit, NetworkX, DGL, OpenCV
  \item \textbf{AI Technologies:} Generative AI, Large Language Models (LLMs), Vision-Language Models, Multimodal Learning, Graph Neural Networks (GNNs), Retrieval-Augmented Generation (RAG), Reinforcement Learning (RL), Hypergraph Learning, Instruction Tuning, Synthetic Data Generation, Agentic AI Systems, Knowledge Graphs
  \item \textbf{Tools \& Platforms:} Docker, Kubernetes, Git, Jupyter, Weights \& Biases, Ray, Neo4j, DWSIM, NVIDIA Triton Inference Server
  \item \textbf{DevOps \& Operations:} Large Language Model Operations (LLMOps), MLOps, Model Serving, Experiment Tracking, Triton Inference Server, Jupyter, Neo4j
  \item \textbf{Scientific Computing:} NumPy, SciPy, SymPy, Pandas, Matplotlib, Seaborn
  \item \textbf{Domain Expertise:} Semiconductor Image Analysis, Process Engineering, Time-Series Forecasting, Patent Analytics, Fluid Dynamics
\end{itemize}



% Awards Section
\section*{Awards and Honors}
\begin{itemize}
  \item \href{https://www.linkedin.com/posts/tcs-research_worldpay-tcsresearch-inventingforimpact-activity-7056880293072310272-Yb2Y}{Patent Champion Award}, Tata Research Development and Design Center.
\end{itemize}

% Professional Memberships
\section*{Professional Memberships}
\begin{itemize}
  \item Member, Association for the Advancement of Artificial Intelligence \href{https://aaai.org/about-aaai/}{(AAAI)}
  \item Member, Association for Computing Machinery \href{https://www.acm.org/about-acm/about-the-acm-organization}{(ACM)}
\end{itemize}

% Conferences Section
\section*{Conferences}
\begin{itemize}
  \item \textbf{Srinivas, S. S.}, Sarkar, R. K., \& Runkana, V., \emph{EMCNet: Graph-Nets for Electron Micrographs Classification}, Workshop on Machine Learning for Materials, International Conference on Knowledge Discovery and Data Mining (ACM SIGKDD), Washington, United States, August 14–18th, 2022. [\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2409.03767}{arXiv:2409.03767}]

  \item \textbf{Srinivas, S. S.}, Sarkar, R. K., \& Runkana, V., \emph{Battery GraphNets: Relational Learning for Lithium-ion Batteries (LiBs) Life Estimation}, Workshop on Graph Learning for Industrial Applications: Finance, Crime Detection, Medicine and Social Media, Neural Information Processing System (NeurIPS), New Orleans, Louisiana, Nov 28–Dec 9, 2022. [\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2408.07624}{arXiv:2408.07624}]

  \item \textbf{Srinivas, S. S.}, Sarkar, R. K., \& Runkana, V., (2022, December). \emph{Hypergraph Learning based Recommender System for Anomaly Detection, Control and Optimization}, In 2022 International Conference on Big Data (Big Data) (pp. 1922-1929). IEEE. [\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2408.11359}{arXiv:2408.11359}]

  \item \textbf{Srinivas, S. S.}, Sarkar, R. K., Gangasani, S., \& Runkana, V., \emph{Vision HGN: An Electron-Micrograph is Worth Hypergraph of Hypernodes}, Practical ML for Developing Countries: Learning under limited/low resource scenarios (PML4DC) workshop, International Conference on Learning Representations (ICLR), Kigali, Rwanda, May 1–5, 2023. [\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2408.11351}{arXiv:2408.11351}]

  \item \textbf{Srinivas, S. S.}, \& Aripirala, K. S., \emph{Multi-Knowledge Fusion Network for Time Series Representation Learning}, Workshop: Machine Learning for IoT: Datasets, Perception, and Understanding, International Conference on Learning Representations (ICLR), Kigali, Rwanda, May 1–5, 2023. [\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2408.12423}{arXiv:2408.12423}]  

  \item \textbf{Srinivas, S. S.}, \& Aripirala, K. S., \emph{Multi-Source Knowledge-Based Hybrid Neural Framework for Time Series Representation Learning}, Knowledge-Based Composition Generalization Workshop, International Joint Conferences on Artificial Intelligence (IJCAI-23), Macau, China, Aug 19–25, 2023. [\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2408.12409}{arXiv:2408.12409}]

  \item \textbf{Srinivas, S. S.}, Gupta, S., Aripirala, K., Runkana, V., \emph{Joint Hypergraph Rewiring and Memory-Augmented Forecasting Techniques in Digital Twin Technology}, Workshop on AI for Digital Twins and Cyber-physical applications, International Joint Conferences on Artificial Intelligence (IJCAI-23), Macau, Aug 19-25, 2023. [\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2408.12634}{arXiv:2408.12634}]

  \item \textbf{Srinivas, S. S.}, Gethan, S., and Runkana, V., \emph{Hierarchical Network Fusion for Multi-Modal Electron Micrograph Representation Learning with Foundational Large Language Models}, Robustness of zero/few-shot learning in foundation models (RO-FoMO) Workshop, Neural Information Processing Systems (NeurIPS), New Orleans, USA, December 10-16, 2023. [\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2408.13661}{arXiv:2408.13661}]

  \item \textbf{Srinivas, S. S.}, and Runkana, V., \emph{Cross-Modal Learning for Chemistry Property Prediction: Let Large Language Models Meet Graph Neural Networks}, Robustness of zero/few-shot learning in foundation models (RO-FoMO) Workshop, NeurIPS, New Orleans, USA, December 10-16, 2023. [\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2408.14964}{arXiv:2408.14964}]

  \item \textbf{Srinivas, S. S.}, and Runkana, V., \emph{Crossing New Frontiers using Large Language Models in Text-Based De Novo Molecule Design}, Robustness of zero/few-shot learning in foundation models (RO-FoMO) workshop, Neural Information Processing Systems (NeurIPS), New Orleans, USA, December 10-16, 2023. [\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2408.11866}{arXiv:2408.11866}]

  \item Sarkar, R. K., Majumdar, R., Jadhav, V., \textbf{Srinivas, S. S.}, and Runkana, V., (2023), \emph{Redefining Super-Resolution: Fine-mesh PDE predictions without classical simulations}, Machine Learning for the Physical Sciences Workshop, NeurIPS, New Orleans, USA, December 10-16, 2023. [\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2311.09740}{arXiv:2311.09740}]

  \item \textbf{Srinivas, S. S.}, Sannidhi, C., Ravuru, C., Gangasani, S., and Runkana, V., \emph{Preliminary Investigations of a Multi-Faceted Robust and Synergistic Approach in Semiconductor Electron Micrograph Analysis using Integrative Models with Large Language and Multimodal Models}, DAI-2024 (2nd Workshop on Deployable AI -- AAAI), Vancouver, Canada, February 20-27, 2024. [\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2408.13621}{arXiv:2408.13621}]

  \item \textbf{Srinivas, S. S.}, Sannidhi, C., Ravuru, C., and Runkana, V., \emph{Reprogramming Foundational Large Language Models (LLMs) for Enterprise Adoption for Spatio-Temporal Forecasting Applications: Data Mining Meets Copilot Guidance}, DAI-2024 (2nd Workshop on Deployable AI -- AAAI), Vancouver, Canada, February 20-27, 2024. [\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2408.14387}{arXiv:2408.14387}]

  \item \textbf{Srinivas, S. S.}, and Runkana, V., \emph{Sparks of Artificial General Intelligence (AGI) in Semiconductor Material Science: Early Explorations into the Next Frontier of Generative AI-Assisted Electron Micrograph Analysis}, Workshop on AI to Accelerate Science and Engineering (AI2ASE), Vancouver, Canada, February 20-27, 2024. [\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2409.12244}{arXiv:2409.12244}]

  \item Sarkar, R., Aripirala, K.S., \textbf{Srinivas, S. S.}, \emph{PointSAGE: Mesh-independent supersresolution approach to fluid flow predictions}, Workshop on AI4DifferentialEquations In Science, International Conference on Learning Representations (ICLR), Vienna, Austria, May 7-11, 2024. [\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2404.04615}{arXiv:2404.04615}]

  \item \textbf{Srinivas, S. S.}, Sannidhi, C., Ravuru, C., \emph{Advancing Enterprise Spatio-Temporal Forecasting Applications: Data Mining Meets Instruction Tuning of Language Models For Multi-modal Time Series Analysis in Low-Resource Settings}, Practical ML for Low Resource Settings Workshop on Learning under limited/low resource scenarios, International Conference on Learning Representations (ICLR), Vienna, Austria, May 7-11, 2024. [\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2408.13622}{arXiv:2408.13622}]

  \item \textbf{Srinivas, S. S.}, and Runkana, V., \emph{Multi-Modal Instruction-Tuning Small-Scale Language-and-Vision Assistant for Semiconductor Electron Micrograph Analysis}, Empowering Machine Learning and Large Language Models with Domain-Specific Knowledge, CAAM-AKATE 2024, SSS-24 (Spring Symposium Series – 24), Stanford University, California, USA, March 25–27, 2024. [\textcolor{blue}{\textbf{AAAI-SS}}: \href{https://ojs.aaai.org/index.php/AAAI-SS/article/view/31205}{ojs.aaai.org/AAAI-SS/31205}] [\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2409.07463}{2409.07463}]

  \item \textbf{Srinivas, S. S.}, Aripirala, K.S., Sannidhi, C., and Runkana, V., \emph{Parameter-Efficient Quantized Mixture-of-Experts Meets Vision-Language Instruction Tuning for Semiconductor Electron Micrograph Analysis}, Workshop on Foundation Models in the Wild, International Conference on Machine Learning (ICML), Vienna, Austria, July 21-27th, 2024. [\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2408.15305}{arXiv:2408.15305}]

  \item \textbf{Srinivas, S. S.}, Ravuru, C., Sannidhi, G., \& Runkana, V., \emph{Foundational Model for Electron Micrograph Analysis: Instruction-Tuning Small-Scale Language-and-Vision Assistant for Enterprise Adoption}, Workshop on ML for Life and Material Science: From Theory to Industry Applications, International Conference on Machine Learning (ICML), Vienna, Austria, July 21-27th, 2024. [\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2408.13248}{arXiv:2408.13248}]

  \item Ravuru, C., \textbf{Srinivas, S. S.}, \& Runkana, V., \emph{Agentic Retrieval-Augmented Generation for Time Series Analysis}, Undergraduate Consortium, Association for Computing Machinery Special Interest Group on Knowledge Discovery and Data Mining (ACM KDD), Barcelona, Spain, August 25-29th 2024. [\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2408.14484}{arXiv:2408.14484}]

  \item Sannidhi, G., \textbf{Srinivas, S. S.}, \& Runkana, V., \emph{Retrieval-Augmented Generation Meets Data-Driven Tabula Rasa Approach for Temporal Knowledge Graph Forecasting}, Undergraduate Consortium, Association for Computing Machinery Special Interest Group on Knowledge Discovery and Data Mining (ACM KDD), Barcelona, Spain, August 25-29th 2024. [\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2408.13273}{arXiv:2408.13273}]

  \item \textbf{Srinivas, S. S.}, Sannidhi, G., \& Runkana, V., \emph{Towards Human-level Understanding of Complex Process Engineering Schematics: A Pedagogical, Introspective Multi-Agent Framework for Open-Domain Question Answering}, Machine Learning for Chemistry and Chemical Engineering (ML4CCE), ECML, Vilnius, Lithuania, September 9th-13th, 2024. [\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2409.00082}{arXiv:2409.00082}]

  \item \textbf{Srinivas, S. S.}, Sannidhi, G., \& Runkana, V., \emph{Retrieval-Augmented Instruction Tuning for Automated Process Engineering Calculations: A Tool-Chaining Problem-Solving Framework with Reflection}, ML4CCE, ECML, Vilnius, Lithuania, September 9th-13th, 2024. [\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2408.15866}{arXiv:2408.15866}]

  \item \textbf{Srinivas, S. S.}, Vaikunth, S. V., \& Runkana, V., \emph{Knowledge Graph Modeling-Driven Large Language Model Operating System (LLM OS) for Task Automation in Process Engineering Problems}, AAAI Fall Symposium 2024, Association for the Advancement of Artificial Intelligence, Arlington, USA, November 7-9, 2024. [\textcolor{blue}{\textbf{AAAI-SS}}: \href{https://ojs.aaai.org/index.php/AAAI-SS/article/view/31796}{ojs.aaai.org/AAAI-SS/31796}][\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2408.14494}{arXiv:2408.14494}]

  \item \textbf{Srinivas, S. S.}, Vaikunth, S. V., \& Runkana, V., \emph{Towards Automated Patent Workflows: AI-Orchestrated Multi-Agent Framework for Intellectual Property Management and Analysis}, Workshop on Open-World Agents: Synergizing Reasoning and Decision-Making in Open-World Environments (OWA), Neural Information Processing Systems (NeurIPS), Vancouver, Canada, Dec 9th-15th, 2024. [\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2409.19006}{arXiv:2409.19006}]

  \item \textbf{Srinivas, S. S.}, Shivam, G., \& Runkana, V., \emph{Accelerating Manufacturing Scale-Up From Material Discovery Using Agentic Web Navigation and Retrieval-Augmented AI for Process Engineering Schematics Design}, Workshop on Advancing LLM-Based Multi-Agent Collaboration, 39th Annual AAAI Conference on Artificial Intelligence, Philadelphia, USA, Feb 3rd-9th, 2025. [\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2412.05937}{arXiv:2412.05937}]

  \item \textbf{Srinivas, S. S.}, Shivam, G., \& Runkana, V., \emph{Agentic Multimodal AI for Hypersonalized B2B and B2C Advertising in Competitive Markets: An AI-Driven Competitive Advertising Framework}, Workshop on Foundation Models in the Wild, International Conference on Learning Representations (ICLR) 2025, Singapore, April 24th–28th, 2025. [\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2504.00338}{arXiv:2504.00338}]

  \item \textbf{Srinivas, S. S.}, Shivam, G., \& Runkana, V., \emph{Scaling Test-Time Inference with Policy-Optimized, Dynamic Retrieval-Augmented Generation via KV Caching and Decoding}, International Conference on Machine Learning (ICML), Toronto, Canada, Jul 13th–Jul 19th, 2025. [\textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/2504.01281}{arXiv:2504.01281}]

  \item \textbf{Srinivas, S. S.}, Shivam, G., \& Runkana, V., \emph{International Conference on Machine Learning}, Toronto, Canada, Jul 13th–Aug 19th, 2025.
    \textcolor{blue}{\textbf{arXiv Link :}} \href{https://arxiv.org/abs/}{arXiv:} % <-- Commented out incomplete entry

\end{itemize}



% Publications Section
\section*{Publications}
\begin{itemize}

  \item \textbf{Srinivas, S. S.}, and Kumaran, V., \emph{After transition in a soft-walled microchannel}, \textbf{Journal of Fluid Mechanics}, Vol. 780, pp. 649–686, 2015. [\textcolor{blue}{\textbf{DOI:}} \href{https://doi.org/10.1017/jfm.2015.476}{10.1017/jfm.2015.476}]

  \item \textbf{Srinivas, S. S.}, and Kumaran, V., \emph{Transitions to different kinds of turbulence in a channel with soft walls}, \textbf{Journal of Fluid Mechanics}, Vol. 822, pp. 267–306, 2017. [\textcolor{blue}{\textbf{DOI:}} \href{https://doi.org/10.1017/jfm.2017.270}{10.1017/jfm.2017.270}]

  \item \textbf{Srinivas, S. S.}, and Kumaran, V., \emph{Effect of viscoelasticity on the soft-wall transition and turbulence in a microchannel}, \textbf{Journal of Fluid Mechanics}, Vol. 812, pp. 1076–1118, 2017. [\textcolor{blue}{\textbf{DOI:}} \href{https://doi.org/10.1017/jfm.2016.839}{10.1017/jfm.2016.839}]

  \item \textbf{Basak, A.}, Rathore, P., Nistala, S. H., \textbf{Srinivas, S. S.}, \& Runkana, V., \emph{Universal Adversarial Attack on Deep Learning Based Prognostics}. \textbf{IEEE International Conference on Machine Learning and Applications (ICMLA)}, 2021, pp. 23–29. [\textcolor{blue}{\textbf{arXiv:}} \href{https://arxiv.org/abs/2109.07142}{2109.07142}]

\end{itemize}


% Invited Talks/Key Notes Section
\section*{Contributed Talks/Key Notes}
\begin{itemize}

   \item \textcolor{blue}{\textbf{Contributed Talk}}, \emph{APS March Meeting 2016}, \textcolor{blue}{Baltimore, Maryland} — Presented research on wall-mode instability driven transition to turbulence in a soft microchannel. [March 14–18, 2016] [\textcolor{blue}{\textbf{APS Link:}} \href{http://meetings.aps.org/link/BAPS.2016.MAR.A53.3}{BAPS.2016.MAR.A53.3}]

   \item \textcolor{blue}{\textbf{Contributed Talk}}, \emph{APS Division of Fluid Dynamics, 69th Annual Meeting}, \textcolor{blue}{Portland, Oregon} — Presented research on transitions to different kinds of turbulence in a soft-walled channel. [November 20–22, 2016] [\textcolor{blue}{\textbf{APS Link:}} \href{http://meetings.aps.org/link/BAPS.2016.DFD.L32.10}{BAPS.2016.DFD.L32.10}]

  \item \textcolor{blue}{\textbf{Contributed Talk}}, \emph{International Conference on Machine Learning (ICML)}, \textcolor{blue}{Toronto, Canada} — Presented research on test-time inference acceleration using policy-optimized retrieval-augmented generation. [July 13–19, 2025]

  \item \textcolor{blue}{\textbf{Contributed Talk}}, \emph{International Conference on Learning Representations (ICLR) Foundation Models in the Wild Workshop}, \textcolor{blue}{Singapore} — Presented framework on agentic multimodal AI for competitive B2B/B2C advertising. [April 24–28, 2025]

  \item \textcolor{blue}{\textbf{Contributed Talk}}, \emph{AAAI 2025 Multi-Agent Collaboration Workshop}, \textcolor{blue}{Philadelphia, USA} — Accelerating manufacturing scale-up with agentic RAG for schematic design. [February 3–9, 2025]

  \item \textcolor{blue}{\textbf{Contributed Talk}}, \emph{NeurIPS 2024 OWA Workshop}, \textcolor{blue}{Vancouver, Canada} — Introduced AI-based multi-agent patent workflow automation. [December 9–15, 2024]

  \item \textcolor{blue}{\textbf{Contributed Talk}}, \emph{AAAI Fall Symposium 2024}, \textcolor{blue}{Arlington, USA} — Presented LLM Operating Systems for process engineering automation. [November 7–9, 2024]

  \item \textcolor{blue}{\textbf{Contributed Talk}}, \emph{ML4CCE Workshop, ECML}, \textcolor{blue}{Vilnius, Lithuania} — Presented dual talks on (1) human-level schematic understanding and (2) retrieval-augmented instruction tuning with reflection. [September 9–13, 2024]

  \item \textcolor{blue}{\textbf{Contributed Talk}}, \emph{KDD 2024 Undergraduate Consortium}, \textcolor{blue}{Barcelona, Spain} — Presented agentic RAG frameworks for (1) temporal tabular forecasting and (2) time series analysis. [August 25–29, 2024]

  \item \textcolor{blue}{\textbf{Contributed Talk}}, \emph{ICML ML for Life and Material Sciences Workshop}, \textcolor{blue}{Vienna, Austria} — Shared work on instruction-tuned LLMs for micrograph analysis. [July 21–27, 2024]

  \item \textcolor{blue}{\textbf{Spotlight Presentation}}, \emph{ICML Foundation Models in the Wild Workshop}, \textcolor{blue}{Vienna, Austria} — Presented quantized MoE framework for vision-language instruction tuning. [July 21–27, 2024]

  \item \textcolor{blue}{\textbf{Contributed Talk}}, \emph{ICLR PML4LR Workshop}, \textcolor{blue}{Vienna, Austria} — Instruction tuning for multimodal time series in low-resource settings. [May 7–11, 2024]

  \item \textcolor{blue}{\textbf{Contributed Talk}}, \emph{ICLR AI4DifferentialEquations Workshop}, \textcolor{blue}{Vienna, Austria} — Presented PointSAGE for mesh-independent fluid flow prediction. [May 7–11, 2024]

  \item \textcolor{blue}{\textbf{Contributed Talk}}, \emph{CAAM-AKATE 2024, AAAI Spring Symposium}, \textcolor{blue}{Stanford University, USA} — Keynote on small-scale multimodal LLMs for semiconductor micrograph analysis. [March 25–27, 2024]

  \item \textcolor{blue}{\textbf{Contributed Talk}}, \emph{Workshop on AI to Accelerate Science and Engineering (AI2ASE), AAAI}, \textcolor{blue}{Vancouver, Canada} — Explored AGI in semiconductor micrograph analysis. [February 20–27, 2024]

  \item \textcolor{blue}{\textbf{Contributed Talk}}, \emph{DAI-2024 Workshop, AAAI}, \textcolor{blue}{Vancouver, Canada} — Presented research on time series forecasting and robust multimodal fusion. [February 20–27, 2024]

  \item \textcolor{blue}{\textbf{Contributed Talk}}, \emph{NeurIPS 2023 ML for Physical Sciences Workshop}, \textcolor{blue}{New Orleans, USA} — Presented PDE-free super-resolution framework. [December 10–16, 2023]

  \item \textcolor{blue}{\textbf{Contributed Talk}}, \emph{NeurIPS RO-FoMO Workshop}, \textcolor{blue}{New Orleans, USA} — Presented three papers on molecule design, property prediction, and multimodal fusion. [December 10–16, 2023]

  \item \textcolor{blue}{\textbf{Contributed Talk}}, \emph{IJCAI-23 Knowledge-Based Composition Workshop}, \textcolor{blue}{Macau, China} — Presented hybrid neural models for time series generalization. [August 19–25, 2023]

  \item \textcolor{blue}{\textbf{Contributed Talk}}, \emph{IJCAI-23 AI for Digital Twins Workshop}, \textcolor{blue}{Macau, China} — Talk on hypergraph rewiring for digital twin forecasting. [August 19–25, 2023]

  \item \textcolor{blue}{\textbf{Contributed Talk}}, \emph{ICLR ML for IoT Workshop}, \textcolor{blue}{Kigali, Rwanda} — Shared multi-knowledge fusion techniques for time series. [May 1–5, 2023]

  \item \textcolor{blue}{\textbf{Contributed Talk}}, \emph{ICLR PML4DC Workshop}, \textcolor{blue}{Kigali, Rwanda} — Introduced Vision HGN for low-resource micrograph learning. [May 1–5, 2023]

  \item \textcolor{blue}{\textbf{Contributed Talk}}, \emph{IEEE Big Data 2022}, \textcolor{blue}{Osaka, Japan} — Presented hypergraph-based anomaly detection system. [December 2022]

  \item \textcolor{blue}{\textbf{Contributed Talk}}, \emph{NeurIPS Battery GraphNets Workshop}, \textcolor{blue}{New Orleans, USA} — Talk on relational learning for LiB life estimation. [Nov 28–Dec 9, 2022]

  \item \textcolor{blue}{\textbf{Contributed Talk}}, \emph{ACM SIGKDD ML4Materials Workshop}, \textcolor{blue}{Washington, USA} — Presented EMCNet for electron micrograph classification. [August 14–18, 2022]
\end{itemize}


% Patents Section
\section*{Patents}
\begin{itemize}
  \item \textbf{Sakhinana, S.J., Nistala, S., Buddhiraju, V., and Runkana, V.}, \emph{System and method for molecular property prediction using hypergraph message passing neural network (HMPNN)}, IDF-1390115-020, Application No. EP21201603.4, Jurisdiction: Europe, Date of Grant: 17 Oct 2023. [\textcolor{blue}{\textbf{Google Patent Link:}} \href{https://patents.google.com/patent/EP4120138B1/en?inventor=Sakhinana+Sagar+Srinivas}{\textcolor{blue}{EP4120138B1}}]

  \item Minimol, Pallavi Venugopal, \textbf{Sagar Srinivas Sakhinana}, and Venkataramana Runkana, \emph{Method and system for evolved sarsa reinforcement learning for flow shop scheduling}, Patent Application 18/071,261, filed September 28, 2023. [\textcolor{blue}{\textbf{Google Patent Link:}} \href{https://patents.google.com/patent/US20230306271A1/en?inventor=Sagar+Srinivas+SAKHINANA}{\textcolor{blue}{US20230306271A1}}]

  \item \textbf{Sakhinana Sagar Srinivas}, Venkataramana Runkana, and Rajat Kumar Sarkar, \emph{System and method for generating mixed variable type multivariate temporal synthetic data}, U.S. Patent Application 17/994,580, filed November 2, 2023. [\textcolor{blue}{\textbf{Google Patent Link:}} \href{https://patents.google.com/patent/US20230351202A1/en?inventor=Sagar+Srinivas+SAKHINANA}{\textcolor{blue}{US20230351202A1}}]

  \item \textbf{Sakhinana Sagar Srinivas}, Rajat Kumar Sarkar, and Venkataramana Runkana, \emph{Congeniality-preserving generative adversarial networks for imputing low-dimensional multivariate time-series data}, U.S. Patent Application 17/815,316, filed September 7, 2023. [\textcolor{blue}{\textbf{Google Patent Link:}} \href{https://patents.google.com/patent/US20230281428A1/en?inventor=Sagar+Srinivas+Sakhinana}{\textcolor{blue}{US20230281428A1}}]

  \item \textbf{Sakhinana Sagar Srinivas}, Venkataramana Runkana, and Rajat Kumar Sarkar, \emph{Privacy preserving generative mechanism for industrial time-series data}, U.S. Patent Application 17/804,682, filed May 31, 2022. [\textcolor{blue}{\textbf{Google Patent Link:}} \href{https://patents.google.com/patent/US20230281427A1/en}{\textcolor{blue}{US20230281427A1}}]

  \item \textbf{Jadhav, V. S.}, Deodhar, A., Srinivas, S. S., and Runkana, V., \emph{System and method for identification and forecasting fouling of heat exchangers in a refinery}, U.S. Patent Application No. 17/476,746, filed September 16, 2021. [\textcolor{blue}{\textbf{Google Patent Link:}} \href{https://patents.google.com/patent/US20220083716A1/en}{\textcolor{blue}{US20220083716A1}}]

 \item \textbf{Sakhinana, S. S.}, Buddhiraju, V. S., Nistala, S. H., and Runkana, V., \emph{Edge conditioned dynamic neighborhood aggregation based molecular property prediction}, U.S. Patent Application No. 17/804,262, filed May 26, 2022. [\textcolor{blue}{\textbf{Google Patent Link:}} \href{https://patents.google.com/patent/US20230116680A1/en}{\textcolor{blue}{US20230116680A1}}]

  \item \textbf{Sakhinana, S. S.}, Buddhiraju, V. S., Nistala, S. H., and Runkana, V., \emph{System and method for molecular property prediction using edge conditioned identity mapping convolution neural network}, U.S. Patent Application No. 17/450,666, filed October 12, 2021. [\textcolor{blue}{\textbf{Google Patent Link:}} \href{https://patents.google.com/patent/US20230045690A1/en}{\textcolor{blue}{US20230045690A1}}]

  \item \textbf{Sakhinana, S. S.}, Buddhiraju, V. S., Nistala, S. H., and Runkana, V., \emph{System and method for molecular property prediction using hierarchical layer-wise propagation of graph pooling layer}, U.S. Patent Application No. 17/731,550, filed April 28, 2022. [\textcolor{blue}{\textbf{Google Patent Link:}} \href{https://patents.google.com/patent/US20230134595A1/en}{\textcolor{blue}{US20230134595A1}}]

 \item \textbf{Sakhinana, S. S.}, Buddhiraju, V. S., Nistala, S. H., and Runkana, V., \emph{Molecular property prediction using edge-information fused global graph-read neural network function}, U.S. Patent Application No. 17/804,263, filed May 26, 2022. [\textcolor{blue}{\textbf{Google Patent Link:}} \href{https://patents.google.com/patent/US20230132463A1/en}{\textcolor{blue}{US20230132463A1}}]

 \item \textbf{Kumar, R.}, Venugopal Minimol, P., Sakhinana, S. S., Baikadi, A., Doan, D., Masampally, V. S., and Runkana, V., \emph{Method and system for performance optimization of flue gas desulphurization (FGD) unit}, U.S. Patent Application No. 17/597,133, filed June 27, 2020. [\textcolor{blue}{\textbf{Google Patent Link:}} \href{https://patents.google.com/patent/US20220246248A1/en}{\textcolor{blue}{US20220246248A1}}]

  % \item Rajat Sarkar, K Sai Aprirala, Vishal Jadhav, \textbf{Sagar Srinivas Sakhinana}, Venkat Runkana, \emph{High Resolution Simulation Prediction For Computational Fluid Dynamics}, Indian Patent Application. (To be Filed Soon.) % Duplicate removed.

\end{itemize}

% References Section
\section*{References}
\begin{itemize}
  \item \textbf{Dr. Venkataramana Runkana} \\
  Chief Scientist, \textit{Tata Consultancy Services (TCS)} \\
  Leads the research program for Manufacturing and Engineering domains \\
  Email: \href{mailto:venkataramana.runkana@tcs.com}{venkataramana.runkana@tcs.com} \\
  LinkedIn: \href{https://www.linkedin.com/in/venkataramana-runkana-42a9447}{https://www.linkedin.com/in/venkataramana-runkana-42a9447} \\
  Profile: \href{https://www.tcs.com/insights/authors/venkataramana-runkana}{https://www.tcs.com/insights/authors/venkataramana-runkana}

  \item \textbf{Trinath Gaduparthi} \\
  Senior Scientist, \textit{Tata Consultancy Services (TCS) – Research} \\
  Email: \href{mailto:trinath.gaduparthi@tcs.com}{trinath.gaduparthi@tcs.com} \\
  LinkedIn: \href{https://www.linkedin.com/in/trinath-gaduparthi-4a2b656/}{https://www.linkedin.com/in/trinath-gaduparthi-42ab656}

  \item \textbf{Muralikrishnan R} \\
  Senior Scientist, \textit{Tata Consultancy Services (TCS) Research} \\
  Email: \href{mailto:muralikrishnan.r@tcs.com}{muralikrishnan.r@tcs.com} \\
  LinkedIn: \href{https://www.linkedin.com/in/murali-krishnan-r-0b5b1319}{https://www.linkedin.com/in/murali-krishnan-r-0b5b1319}
\end{itemize}

\end{document}
