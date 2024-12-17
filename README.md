# Wang

\vspace{1mm}
\section{Related Work}
\vspace{1mm}
Large Language Models (LLMs) have emerged as powerful tools for streamlining scientific discovery by automating complex tasks. To evaluate their effectiveness in autonomous data-driven research, benchmarks such as DiscoveryBench \cite{majumder2024discoverybench} and ScienceAgentBench \cite{chen2024scienceagentbenchrigorousassessmentlanguage} have been developed. DiscoveryBench focuses on multi-step workflows involved in hypothesis-driven research across diverse disciplines, including sociology, biology, and engineering. It defines each task using a dataset, its metadata, and a clearly articulated discovery objective in natural language, challenging LLMs to demonstrate skills in statistical evaluation and the semantic interpretation of scientific concepts. On the other hand, ScienceAgentBench is designed to assess the capabilities of LLMs in automating various workflows across fields like bioinformatics, computational chemistry, and cognitive neuroscience. The tasks, derived from peer-reviewed studies and validated by experts, involve generating executable Python programs from natural language prompts, incorporating dataset details and optional expert knowledge. This benchmark employs a detailed evaluation protocol to measure task completion, computational efficiency, and the relevance and plausibility of results within scientific contexts. Building on these evaluation datasets, various frameworks have been developed to enhance knowledge discovery and hypothesis generation. The LLM-Duo framework \cite{hu2024automating} employs a dual-agent architecture to enhance large-scale knowledge base curation using the Progressive Ontology Prompting (POP) algorithm. The explorer agent, leveraging Retrieval-Augmented Generation (RAG), processes scientific literature to extract information guided by POP-generated prompts based on a predefined ontology. This structured ontology, often defined by domain experts, represents the target knowledge domain. The evaluator agent ensures the explorer’s responses are accurate and complete through iterative feedback and refinement, achieving high-quality knowledge discovery. In parallel, the SciAgents framework \cite{ghafarollahi2024sciagents} automates scientific discovery in material science by employing a multi-agent system integrated with a large ontological knowledge graph and advanced LLMs. This framework systematically generates, critiques, and refines research hypotheses through the collaboration of agents assigned distinct roles, including planners, ontologists, scientists, and critics. By utilizing heuristic pathfinding within the knowledge graph, the framework identifies relationships between concepts and structures hypotheses into detailed, actionable research plans. Iterative refinement ensures the rigor of the process, while external tools like the Semantic Scholar API assess the novelty and feasibility of the generated hypotheses. This scalable approach expedites hypothesis generation and promotes innovative, cross-disciplinary insights in scientific research. DataVoyager \cite{majumderposition} automates the process of scientific discovery by enabling end-to-end, data-driven hypothesis generation and verification. It incorporates a role-based multi-agent architecture, including planners, programmers, data experts, and critics, to manage workflows from data understanding and hypothesis generation to verification and refinement. Operating in autonomous or user-guided modes, the framework facilitates dynamic hypothesis exploration, robust statistical evaluation, and iterative improvement through user feedback. Highlighting the innovative potential of LLMs, a large-scale evaluation of LLMs in research ideation \cite{si2024can} found that while LLM-generated ideas tend to be more novel, they may be less feasible compared to human-generated ideas. This study underscores both the creative promise and the current limitations of LLMs in scientific discovery, particularly the challenge of balancing creativity with practical feasibility. Addressing some of these limitations, the Scideator framework \cite{radensky2024scideator} is an LLM-powered scientific ideation tool that generates novel research ideas by recombining facets (purpose, mechanism, evaluation) extracted from user-provided and analogous research papers. Users input papers, and the system retrieves related works, extracts key facets, and facilitates the interactive selection and recombination of these facets to generate innovative ideas. A novelty checker, leveraging RAG and expert-labeled examples, assesses idea originality and provides suggestions for improvement. Through iterative refinement, the framework enables researchers to explore diverse idea spaces grounded in literature, fostering creativity and originality in scientific ideation. Most recently, the AI Scientist framework \cite{lu2024ai} fully automates scientific discovery using LLMs by generating ideas, conducting experiments, and drafting research papers with automated reviews. It integrates ideation, experimentation, and refinement to mimic human scientific processes, enabling scalable and efficient research generation at low costs. Collectively, these studies underscore the expanding role of LLMs in automating various aspects of scientific discovery—from knowledge extraction to hypothesis generation and ideation—but also highlight several limitations of data-driven discovery, such as hallucinations, the cost of operating at scale, potential misuse, legal ramifications, and inherent biases. While LLMs have advanced scientific discovery, their application to patent ideation and drafting remains largely unexplored. Patent ideation leverages knowledge extraction from patent literature and scientific discovery to generate feasible hypotheses for novel inventions. Patent drafting translates these inventions into legally sound documents. Integrating LLMs into multi-agent frameworks offers significant potential to revolutionize patent processes. This promises to streamline patent creation and accelerate technological innovation.    \section{Dataset Curation}
%The **Data Collection Pipeline** for the *Inventa* framework is designed to systematically gather and curate high-quality datasets that support each phase of patent ideation and drafting using **open-source and free resources**. The process begins with identifying reliable **data sources**, including patent databases like **USPTO**, **EPO**, and **WIPO**, and scientific repositories such as **arXiv**, focusing on papers accepted in **top AI conferences** like **NeurIPS, ICML, ICLR, CVPR, ACL, and AAAI**. Specifically, the pipeline targets a dataset of at least **2,500 patents** across diverse AI and deep learning domains and **2,500 scholarly articles** from **arXiv** that are published in these top-tier conferences.
%
%Next, **open-source automated extraction tools** such as **llama_parse**, **spaCy**, and **HuggingFace Transformers** are employed to parse and extract structured information, including Subject-Action-Object (SAO) triplets, claims, detailed descriptions, and metadata from patents and scientific articles. To ensure the extracted data meets quality standards, a two-step **annotation process** is implemented: large-scale annotations are crowdsourced using open-source platforms like **Label Studio** or **Doccano**, while specialized tasks requiring legal or technical expertise are validated through contributions from open-source communities and volunteer experts. Additionally, **synthetic data generation** using **open-source LLMs** (such as **LLaMA**, **Falcon**, and **Mistral**) augments the dataset, with generated data reviewed by community-driven efforts to ensure relevance and accuracy. The extracted and annotated data is then organized into **knowledge graphs** (e.g., MPKG and SciKG) using **Neo4j Community Edition** or other graph databases like **ArangoDB**. This structured pipeline ensures a seamless flow of information, reducing computational inference time, graph construction, and querying complexity, resulting in comprehensive datasets that support the end-to-end automation of patent ideation and drafting within the *Inventa* framework using fully open-source resources. \section{Introduction}
``\textit{An invention is something that was 'impossible' until then; that's why governments grant patents.}” (Robert A. Heinlein). A patent is an exclusive right granted by the government, allowing an inventor to make, use, and sell their novel, non-obvious, and useful invention. In return, the inventor publicly discloses the invention’s details, promoting technological advancement and innovation. A standard patent document outlines technical aspects and provides legal protection through specific claims and diagrams, facilitating commercialization. Companies pursue patents to protect innovations and establish market presence, with patent applications serving as a key strategy for safeguarding intellectual property in emerging fields and disruptive technologies. Valued at hundreds of billions of dollars, the global patent market involves specialized IP firms and brokers who play crucial roles in trading and licensing, highlighting patents as vital economic assets that drive innovation and growth. In today’s rapidly advancing technological landscape, there is a need and necessity to accelerate the automation of high-caliber patent ideation and drafting. Traditional patent ideation methods require extensive research, and navigating prior art literature is overwhelming and demands a solid understanding of technical domains and patent law. An overemphasis on technical feasibility can overshadow market needs, resulting in inventions that are technically sound but commercially unviable. Additionally, siloed expertise limits interdisciplinary innovation. Conventional patent drafting, reliant on human experts, is resource-intensive, prone to errors, and inefficient given the multidisciplinary demands, leading to delayed filings that affect market competitiveness. This highlights the need for more efficient and reliable approaches to managing patent creation tasks. To address these challenges, AI-powered research and drafting solutions can transform the patent process by simplifying information navigation, identifying relevant prior art, proposing new inventions, and automating draft generation. This allows patent attorneys to focus on reviewing drafts and providing feedback to enhance patent ideation. In recent times, applications of AI-driven multi-agent frameworks \cite{brown2020language, dibia2024autogen, he2024webvoyager} have revolutionized various sectors by enhancing efficiency and automating complex workflows. There is growing interest in using these frameworks to transform scientific discovery by automating key steps in uncovering new knowledge. This involves analyzing and synthesizing vast amounts of literature, identifying unknown patterns or phenomena, accelerating the formulation of new hypotheses, and testing these hypotheses to generate insights and achieve scientific breakthroughs. However, the use of multi-agent frameworks in patent-related tasks is still emerging and has the potential to revolutionize complex, patent-driven workflows by automating various tasks, thereby transforming the landscape of intellectual property management. %With multiple AI-driven agents powered by large language models (LLMs) such as OpenAI GPT-4o\cite{openai2024gpt4o} and Anthropic Sonnet\cite{anthropic2024claude} collaborating efficiently—each agent specializing in specific tasks—they can improve accuracy and reduce reliance on human intervention. Multi-agent frameworks can automate both patent ideation and drafting, critical processes for protecting intellectual property by generating innovative ideas, expediting the drafting process, and helping innovators secure intellectual property more quickly. This comprehensive automation boosts competitiveness and fosters technological advancement. In this work, we propose a multi-agent framework, \textbf{Inventa}, with agent chaining to automate patent ideation and drafting. In this framework, a meta-agent orchestrates multiple expert sub-agents—powered by LLMs and specialized in different tasks—that collaborate sequentially to handle the complex processes of patent ideation and drafting. The proposed multi-agent framework dynamically selects, integrates, and sequences expert agent invocations for novel patent creation, considering their relevance according to documented protocols and their interdependencies. Furthermore, we incorporate error-handling mechanisms with attributable reflection by utilizing critique reviewers, including human experts, a gold-standard language model (Gold-LLM-as-a-Judge), and a reward model (Reward-Model-as-Judge) to provide feedback. These critique reviewers are essential for producing robust patent applications, given the high stakes associated with intellectual property protection. The attributable reflection mechanism involves reviewing the outputs of expert agents using critique reviewers to identify and correct errors traceable back to task-specific expert agents, thereby ensuring high-quality patent documents. Figures \ref{fig:figure1}–\ref{fig:figure2} provide an overview of the proposed AI-powered multi-agent framework for patent ideation and drafting. Figure \ref{fig:figure1} illustrates the entire framework, from idea submission to patent assessment, with the meta-agent orchestrating the workflow. Figure \ref{fig:figure2} details the multi-agent framework's workflow, where specialized agents handle distinct tasks and critique evaluators review outputs for quality and compliance.
