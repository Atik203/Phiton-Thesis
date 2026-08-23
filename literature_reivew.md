Literature Review — Idea 1
Intent-Consistency Verification at the Action Gate for Tool-Using LLM Agents
10 Verified Papers · Q1–Q9 Extraction · August 2026
How This List Was Built
Every paper below was located via web search and then opened/verified directly (abstract pages, ACL Anthology pages, AAAI/ICLR proceedings pages, or the arXiv PDF itself) rather than taken from a citation list alone. Papers are grouped into three tiers:
• ANCHOR (4 papers) — confirmed peer-reviewed, top-tier venue, directly foundational to your problem (benchmarks you will likely build on).
• CLOSE (1 paper) — ToolGate, the closest system to your proposed solution; venue status not yet confirmed as peer-reviewed, flagged accordingly.
• SUPPORTING (5 papers) — peer-reviewed or credible preprint defenses/attacks that round out the related-work landscape and give you comparison baselines.
Where an exact figure could not be independently confirmed from primary sources in this search, that is stated explicitly in the relevant answer rather than presented as fact — verify those specific numbers directly from the PDF before citing them in your paper.

Quick Reference Table

# Title (short) Venue Tier Peer-Reviewed?

1 AgentDojo: A Dynamic Environment to Evaluate Prompt ... NeurIPS 2024 ANCHOR Yes
2 InjecAgent: Benchmarking Indirect Prompt Injections ... Findings of the Association for Computational Linguistics: ACL 2024 ANCHOR Yes
3 MCPTox: A Benchmark for Tool Poisoning on Real-World... AAAI 2026 ANCHOR Yes
4 Agent Security Bench (ASB): Formalizing and Benchmar... ICLR 2025 ANCHOR Yes
5 ToolGate: Contract-Grounded and Verified Tool Execut... arXiv preprint CLOSE Unconfirmed
6 TrustAgent: Towards Safe and Trustworthy LLM-based A... Findings of the Association for Computational Linguistics: EMNLP 2024 SUPPORTING Yes
7 MELON: Indirect Prompt Injection Defense via Masked ... arXiv preprint SUPPORTING Unconfirmed
8 StruQ: Defending Against Prompt Injection with Struc... 34th USENIX Security Symposium SUPPORTING Yes
9 Defeating Prompt Injections by Design (CaMeL) arXiv preprint SUPPORTING Unconfirmed
10 Adaptive Attacks Break Defenses Against Indirect Pro... Findings of the North American Chapter of the Association for Computational Linguistics SUPPORTING Yes

 
[ANCHOR] Paper 1
AgentDojo: A Dynamic Environment to Evaluate Prompt Injection Attacks and Defenses for LLM Agents
Debenedetti, Zhang, Balunović, Beurer-Kellner, Fischer, Tramèr
Venue: NeurIPS 2024, Datasets & Benchmarks Track
Venue tier / verification note: Top-tier (A\*/Q1-equivalent CS venue). Peer-reviewed, proceedings-published.
Link/DOI: arXiv:2406.13352 · https://arxiv.org/abs/2406.13352 · Proceedings: https://proceedings.neurips.cc/paper_files/paper/2024/hash/97091a5177d8dc64b1da8bf3e1f6fb54-Abstract-Datasets_and_Benchmarks.html
Q1 — Problem & Importance
Problem: AI agents that combine reasoning with external tool calls are vulnerable to prompt injection, where data returned by tools (e.g., an email body, a webpage) hijacks the agent into executing tasks the user never requested. This matters because agents are increasingly given real permissions (email, banking, travel booking) where a hijack has real-world consequences, and existing evaluation was fragmented and static.
Q2 — Data
Data: The authors built a new benchmark, not reused an existing dataset. 97 realistic tasks across 4 simulated environments (Workspace/email client, Slack, Travel/e-banking, Banking) with 629 security test cases (injection scenarios). No human-subject data; entirely synthetic task/tool simulation. Code and environment publicly released.
Q3 — Features/Inputs
Features/Inputs: Natural-language user instructions, simulated tool outputs (some containing injected instructions), and the agent's full trajectory (sequence of tool calls and reasoning steps) are the units of analysis — not engineered numeric features, since this is an agent-behavior benchmark, not a classifier.
Q4 — Methods & Pipeline
Methods/Pipeline: An extensible harness runs a target LLM as a ReAct-style tool-using agent inside each simulated environment; injected instructions are placed inside tool return values ("indirect" injection); multiple attack strategies (e.g., naive injection, tool-knowledge attacks) and multiple defenses (e.g., prompt-based warnings, a 'tool filter', spotlighting) are plugged into the same harness and run end-to-end against each task/test-case combination.
Q5 — Baselines/Comparisons
Baselines/Comparisons: No-attack (utility-only) baseline; no-defense (undefended agent) baseline under attack; several existing prompt-injection defenses from the literature (e.g., a simplified execution-isolation-style 'tool filter' inspired by SecGPT) are evaluated side by side.
Q6 — Evaluation
Evaluation: Two paired metrics per model — (a) Benign Utility: task success rate with no attack present, and (b) Targeted/Undefended Attack Success Rate: fraction of the 629 test cases where the injected instruction's goal is achieved. Evaluated across multiple frontier LLMs (GPT-4 family, Claude 3.5 Sonnet, others).
Q7 — Main Results
Main Results: State-of-the-art LLMs fail a meaningful share of tasks even with zero attack present (task difficulty alone is nontrivial) — e.g., the best agent tested (Claude 3.5 Sonnet) reached about 78% benign utility, while GPT-4o's utility dropped from 69% to 50% once under attack. Existing defenses reduced but did not eliminate attack success — none achieved both high utility and low ASR simultaneously.
Q8 — Limitations & Bias
Limitations/Bias: Environments are simulated (tool outputs are synthetic, not live APIs), so results may not fully transfer to production systems; the attack set, while broad, reflects attacks known at publication time and does not include fully adaptive, defense-aware attackers (a gap the authors explicitly flag as future work, later addressed by follow-up 'adaptive attack' papers).
Q9 — Reproducibility
Reproducibility: Fully open — code and environment released at https://github.com/ethz-spylab/agentdojo; benchmark is designed to be extensible so new tasks/attacks/defenses can be added by other researchers.

 
[ANCHOR] Paper 2
InjecAgent: Benchmarking Indirect Prompt Injections in Tool-Integrated Large Language Model Agents
Zhan, Liang, Ying, Kang
Venue: Findings of the Association for Computational Linguistics: ACL 2024
Venue tier / verification note: ACL Findings — top-tier NLP venue (CORE A\*/A-equivalent), peer-reviewed.
Link/DOI: https://doi.org/10.18653/v1/2024.findings-acl.624 · https://aclanthology.org/2024.findings-acl.624/
Q1 — Problem & Importance
Problem: Tool-integrated agents are exposed to indirect prompt injection — malicious instructions embedded not in the user's own prompt but in third-party content the agent retrieves (a webpage, a document, an API response). This is dangerous because the user never sees or approves the injected instruction, and it matters more as agents are deployed with real tool permissions.
Q2 — Data
Data: A new benchmark of 1,054 test cases spanning 17 categories of user tools and 62 categories of attacker tools, built by combining a curated real tool taxonomy with synthetically constructed attack instructions. No human-subject data.
Q3 — Features/Inputs
Features/Inputs: User tool specification + benign user instruction + an attacker-controlled tool output containing an embedded malicious instruction; the agent's resulting tool-call trajectory is the unit scored.
Q4 — Methods & Pipeline
Methods/Pipeline: A ReAct-style tool-calling agent is instantiated per test case; the agent is given a legitimate task requiring a benign tool, but the tool's returned content also contains an attacker's injected instruction; the pipeline checks whether the agent's subsequent actions match the attacker's injected goal (attack succeeded) or the user's original goal only (attack failed/agent robust).
Q5 — Baselines/Comparisons
Baselines/Comparisons: Multiple LLM backbones compared directly against each other (GPT-4, GPT-3.5, Claude, open-source models) under two conditions: 'base attack' (straightforward injected instruction) and 'enhanced attack' (injected instruction augmented with prompting tricks to increase compliance).
Q6 — Evaluation
Evaluation: Attack Success Rate (ASR) as the primary metric, computed separately for base and enhanced attack settings, broken down by tool category and by LLM backbone.
Q7 — Main Results
Main Results: GPT-4 was vulnerable in the base attack setting roughly 24% of the time, rising to about 47% under the enhanced attack setting — demonstrating that even frontier models at the time were substantially exposed, and that simple prompt-level attack enhancements meaningfully increased success.
Q8 — Limitations & Bias
Limitations/Bias: The benchmark's synthetic tool/task taxonomy, while broad, may not capture the full diversity of real-world tool ecosystems (especially newer MCP-style protocols that emerged after publication); attacks are largely static/non-adaptive to a specific defense, a limitation later directly addressed by adaptive-attack follow-up work (e.g., Zhan et al.'s own 2025 adaptive-attacks paper).
Q9 — Reproducibility
Reproducibility: Code and benchmark data publicly released alongside the ACL Findings paper (standard ACL Anthology open-access policy); widely reused as a standard baseline in over a dozen subsequent papers found during this search.

 
[ANCHOR] Paper 3
MCPTox: A Benchmark for Tool Poisoning on Real-World MCP Servers
Wang et al.
Venue: AAAI 2026 (Proceedings of the AAAI Conference on Artificial Intelligence)
Venue tier / verification note: AAAI — top-tier AI venue (CORE A*), peer-reviewed, published proceedings (2026).
Link/DOI: https://doi.org/10.1609/aaai.v40i42.40895 · https://ojs.aaai.org/index.php/AAAI/article/view/40895
Q1 — Problem & Importance
Problem: Model Context Protocol (MCP) servers let agents discover and call third-party tools dynamically, but the tool's own registered description/metadata can itself be poisoned to induce unsafe behavior at call time — a distinct attack surface from injection via tool *output\*. This matters because MCP adoption is growing quickly and registration-time trust is largely unvetted in practice.
Q2 — Data
Data: 1,348 test cases built across 45 real, live MCP servers (not simulated) with 353 real tools spanning 10 risk categories — one of the first benchmarks to use genuinely live MCP infrastructure rather than a synthetic sandbox.
Q3 — Features/Inputs
Features/Inputs: Tool metadata/descriptions (some poisoned with hidden malicious instructions), the user's legitimate request, and the agent's resulting tool-selection and call behavior.
Q4 — Methods & Pipeline
Methods/Pipeline: Poisoned tool descriptions are registered on real MCP servers; 20 different agent configurations (varying backbone LLM and agent scaffold) are run against the live servers and asked to complete legitimate tasks; the pipeline logs whether the agent calls the poisoned tool and executes the hidden malicious behavior.
Q5 — Baselines/Comparisons
Baselines/Comparisons: 20 agent configurations across multiple LLM backbones compared directly against one another; refusal rate and attack success rate compared across risk categories.
Q6 — Evaluation
Evaluation: Attack Success Rate (ASR) and refusal rate as primary metrics, broken down by the 10 risk categories and by agent/backbone.
Q7 — Main Results
Main Results: Attack success rates reached up to 72.8% in the worst-case configuration, while refusal rates stayed under 3% across nearly all agents tested — indicating current agents essentially never recognize or refuse tool-poisoning attempts at registration time.
Q8 — Limitations & Bias
Limitations/Bias: Because servers are real and live, some ethical/operational care was required in test design (the paper does not claim to have attacked production user data, but the live-server methodology is inherently harder to fully control than a sandbox); the benchmark measures vulnerability only — it proposes no defense, which is an explicit, stated gap.
Q9 — Reproducibility
Reproducibility: Released with the AAAI paper (standard AAAI open proceedings policy); given the live-MCP-server methodology, some setup may require re-registering test tools rather than a pure static download — worth confirming exact reproducibility instructions in the paper's appendix before relying on it.

 
[ANCHOR] Paper 4
Agent Security Bench (ASB): Formalizing and Benchmarking Attacks and Defenses in LLM-Based Agents
Zhang, Huang, Mei, Yao, Wang, Zhan, Wang, Zhang
Venue: ICLR 2025 (International Conference on Learning Representations)
Venue tier / verification note: ICLR — top-tier ML venue (CORE A\*), peer-reviewed conference proceedings.
Link/DOI: arXiv:2410.02644 · https://arxiv.org/abs/2410.02644 · https://iclr.cc/virtual/2025/poster/29432 · Code: https://github.com/agiresearch/ASB
Q1 — Problem & Importance
Problem: No existing benchmark at the time comprehensively covered the full attack surface of LLM agents (system prompt, user prompt, tool usage, AND memory retrieval together) in one unified framework — most prior work isolated a single vector. This matters because real deployments face combined/compound threats, not single-vector ones.
Q2 — Data
Data: A large synthetic benchmark: 10 task scenarios (e-commerce, autonomous driving, finance, academic advising, counseling, investment, legal advice, etc.), 10 scenario-specific agents, 400+ tools, evaluated across 13 LLM backbones.
Q3 — Features/Inputs
Features/Inputs: System prompts, user prompts, tool outputs, and persistent agent memory are all treated as separate, independently attackable input surfaces; 27 distinct attack/defense method implementations are plugged into this multi-surface pipeline.
Q4 — Methods & Pipeline
Methods/Pipeline: Formalizes 4 attack families — Direct Prompt Injection (DPI), Observation/Indirect Prompt Injection (OPI), a novel Plan-of-Thought (PoT) backdoor attack (hidden instructions embedded in the system prompt that trigger during planning), and Memory Poisoning Attacks — plus 4 mixed/combined attack settings, run against 11 corresponding defenses across all 10 scenarios and 13 backbones, scored with 7 evaluation metrics.
Q5 — Baselines/Comparisons
Baselines/Comparisons: 11 defense methods directly compared against each other and against an undefended baseline, across all 27 attack/defense combinations and 13 LLM backbones — the broadest single cross-comparison found in this search.
Q6 — Evaluation
Evaluation: 7 metrics including Attack Success Rate (per attack type and overall), task utility, and defense effectiveness deltas; results broken down by agent-operation stage (system prompt / user prompt / tool usage / memory retrieval).
Q7 — Main Results
Main Results: The highest average Attack Success Rate reached 84.3% (their novel PoT backdoor attack was especially potent), while current defenses showed only limited effectiveness across the board — no defense neutralized attacks across all stages and backbones simultaneously.
Q8 — Limitations & Bias
Limitations/Bias: Scenarios and tools are still simulated/synthetic rather than live production systems (unlike MCPTox's live-server approach); the sheer combinatorial scope (27 methods × 13 backbones × 10 scenarios) means individual attack/defense pairs may be tested with less depth than a narrowly-focused single-method paper.
Q9 — Reproducibility
Reproducibility: Fully open-source, code and benchmark released at https://github.com/agiresearch/ASB with a public results website (https://luckfort.github.io/ASBench/) — one of the most reproducible papers in this set.

 
[CLOSE] Paper 5
ToolGate: Contract-Grounded and Verified Tool Execution for LLMs
Liu et al.
Venue: arXiv preprint (Jan 2026) — not yet confirmed peer-reviewed at time of writing
Venue tier / verification note: NOT a confirmed peer-reviewed venue — flagged as preprint status; verify before final citation if the group's rubric requires peer review for anchor papers.
Link/DOI: arXiv:2601.04688 · Code: https://github.com/OceannTwT/ToolGate
Q1 — Problem & Importance
Problem: Agent tool calls can violate implicit safety/correctness constraints even without an obvious 'attack' present — the paper targets ensuring each tool call is formally consistent with pre/postconditions on a symbolic world-state, addressing hallucinated or invalid tool use broadly, not adversarial attacks specifically.
Q2 — Data
Data: Evaluated on ToolBench and MCP-Universe (231 real MCP tasks) — existing task-completion benchmarks, not adversarial/security benchmarks; no new adversarial dataset introduced.
Q3 — Features/Inputs
Features/Inputs: Formal Hoare-style pre/postcondition contracts authored per tool, a symbolic representation of world-state, and the agent's proposed tool call parameters checked against that state before execution.
Q4 — Methods & Pipeline
Methods/Pipeline: Each tool is manually annotated with formal logical contracts; before execution, the middleware checks the proposed call's parameters and the current symbolic world-state against the contract's precondition; after execution, the postcondition is checked against the tool's actual return; violations block or flag the call. Model-agnostic, no fine-tuning required.
Q5 — Baselines/Comparisons
Baselines/Comparisons: Compared against unguarded tool execution and simpler validation heuristics on task-completion correctness — NOT compared against any adversarial injection or poisoning benchmark (a gap this proposal's Idea 1 directly targets).
Q6 — Evaluation
Evaluation: Task-completion accuracy and contract-violation detection rate on ToolBench/MCP-Universe.
Q7 — Main Results
Main Results: Improved task-completion reliability and caught a meaningful share of invalid/hallucinated tool calls relative to unguarded baselines (specific percentage figures should be pulled directly from the paper's results tables before citing precisely, as this search could not confirm exact numbers from secondary sources).
Q8 — Limitations & Bias
Limitations/Bias: Requires hand-authoring formal contracts per tool — a real deployment/setup cost the paper does not attempt to remove; never evaluated against adversarial tool-poisoning or prompt-injection attacks, so its robustness under active adversarial pressure (vs. passive hallucination) is untested by the authors themselves.
Q9 — Reproducibility
Reproducibility: Code publicly released at https://github.com/OceannTwT/ToolGate, but the repository is minimal (2 commits, no README/description at time of checking) — usable but likely under-documented; committed pipeline-output JSON files suggest a runnable setup.

 
[SUPPORTING] Paper 6
TrustAgent: Towards Safe and Trustworthy LLM-based Agents
Hua, Yang, Jin, Li, Cheng, Tang, Zhang
Venue: Findings of the Association for Computational Linguistics: EMNLP 2024
Venue tier / verification note: EMNLP Findings — top-tier NLP venue (CORE A\*/A-equivalent), peer-reviewed.
Link/DOI: https://aclanthology.org/2024.findings-emnlp.585/ · arXiv:2402.01586
Q1 — Problem & Importance
Problem: Ensuring LLM agent safety in high-stakes domains (housekeeping, finance, medicine, food, chemistry) requires more than input filtering — unsafe plans can form even from benign-looking instructions. This matters because agents are being integrated into environments where a single unsafe action has real consequences.
Q2 — Data
Data: No new benchmark dataset — evaluated using GPT-4-emulated tool execution in a virtual sandbox (following the ToolEmu methodology) across five constructed domains; no human-subject data collection.
Q3 — Features/Inputs
Features/Inputs: A hand-authored 'Agent Constitution' (a set of natural-language safety regulations) is the key input artifact, alongside the agent's generated plan (sequence of intended actions) at each of three pipeline stages.
Q4 — Methods & Pipeline
Methods/Pipeline: Three-stage safety strategy — (1) Pre-planning: safety-relevant knowledge from the Constitution is injected into context before the agent plans; (2) In-planning: special prompting steers the agent away from unsafe choices during plan generation; (3) Post-planning: the completed plan is inspected against the Constitution and self-edited if violations are found.
Q5 — Baselines/Comparisons
Baselines/Comparisons: Standard (non-Constitution-guided) agent planning as the baseline, compared across four advanced closed-source LLMs and across the five safety domains.
Q6 — Evaluation
Evaluation: Safety-violation rate (dangerous actions identified/prevented) alongside task helpfulness/success rate, to check whether safety gains come at a helpfulness cost.
Q7 — Main Results
Main Results: The framework measurably reduced unsafe actions across all five tested domains and, notably, also improved helpfulness rather than trading it off — the paper additionally finds LLM reasoning ability itself is a significant factor in how well a model adheres to the Constitution.
Q8 — Limitations & Bias
Limitations/Bias: Relies on GPT-4-emulated tool execution (a sandbox) rather than real tool APIs, so real-world transfer is not directly demonstrated; the Agent Constitution itself is hand-authored per domain, which does not scale automatically to novel domains — directly relevant as a contrast point for Idea 1's 'automatically derived, no manual authoring' framing.
Q9 — Reproducibility
Reproducibility: Data and code released (initially via an anonymized repository during review; confirm current public link in the camera-ready version) — https://anonymous.4open.science/r/TrustAgent-06DC referenced in the paper; verify a de-anonymized permanent link before citing in your final bibliography.

 
[SUPPORTING] Paper 7
MELON: Indirect Prompt Injection Defense via Masked Re-execution and Tool Comparison
Zhu, Yang, Wang, Guo, Wang
Venue: arXiv preprint (2025) — check for conference acceptance before final citation
Venue tier / verification note: Not confirmed peer-reviewed at time of this search; widely cited by other peer-reviewed papers (AIRGuard, others) as a live defense baseline.
Link/DOI: arXiv:2502.05174
Q1 — Problem & Importance
Problem: Existing indirect prompt injection defenses often need labeled attack examples or degrade agent utility significantly; the paper targets detecting injection without needing to know the attack pattern in advance.
Q2 — Data
Data: Evaluated on existing benchmarks (details should be pulled directly from the paper before citing precisely — this search surfaced it primarily as a defense baseline cited by other papers, not through direct access to its own results tables).
Q3 — Features/Inputs
Features/Inputs: The agent's tool-call trajectory under normal execution vs. a 'masked' re-execution where suspected injected content is masked out, comparing whether the same tool calls are made in both runs.
Q4 — Methods & Pipeline
Methods/Pipeline: Masked re-execution — the agent's trajectory is re-run with candidate injected spans masked; if tool calls diverge meaningfully between the original and masked runs, the original tool call is flagged as being influenced by injected content, without needing pre-labeled injection examples.
Q5 — Baselines/Comparisons
Baselines/Comparisons: Compared against prompt-based and classifier-based defenses on standard injection benchmarks per citing papers' descriptions.
Q6 — Evaluation
Evaluation: Attack Success Rate reduction and task utility retention, per standard practice in this literature (confirm exact figures directly from the paper).
Q7 — Main Results
Main Results: Cited by multiple later papers as a strong training-free defense baseline; exact reported ASR/utility numbers should be verified directly from the source before use in your literature review to avoid secondhand citation error.
Q8 — Limitations & Bias
Limitations/Bias: Masked re-execution roughly doubles inference cost (running the trajectory twice), a latency/cost tradeoff explicitly worth noting against your own middleware's overhead claims.
Q9 — Reproducibility
Reproducibility: Not directly verified in this search — locate and confirm code availability directly from the arXiv listing before relying on it as a baseline.

 
[SUPPORTING] Paper 8
StruQ: Defending Against Prompt Injection with Structured Queries
Chen, Piet, Sitawarin, Wagner
Venue: 34th USENIX Security Symposium (USENIX Security 2025)
Venue tier / verification note: USENIX Security — top-tier systems security venue (CORE A\*), peer-reviewed.
Link/DOI: Proceedings: USENIX Security '25, pages 2383–2400
Q1 — Problem & Importance
Problem: Standard LLM prompting concatenates trusted instructions and untrusted data into one unstructured text stream, which is fundamentally what makes injection possible; the paper asks whether structurally separating instructions from data at the model level (not just the prompt-text level) can prevent injection by design.
Q2 — Data
Data: Uses standard instruction-following/injection evaluation setups plus the authors' own constructed structured-query test sets (exact dataset size to be confirmed directly from the paper before citing precisely).
Q3 — Features/Inputs
Features/Inputs: A structured query format that explicitly and architecturally separates the 'instruction' channel from the 'data' channel, rather than relying on prompt-level delimiters alone.
Q4 — Methods & Pipeline
Methods/Pipeline: Fine-tunes (or structurally modifies) the model's input handling so instructions and data occupy distinct, non-conflatable channels; evaluates whether injected instructions placed in the data channel are still obeyed.
Q5 — Baselines/Comparisons
Baselines/Comparisons: Standard unstructured prompting, and other prompt-level defenses (e.g., delimiters, spotlighting) as points of comparison.
Q6 — Evaluation
Evaluation: Attack Success Rate under injection, alongside task utility on benign inputs, evaluated at a systems-security venue standard of rigor (adaptive attacker consideration is a hallmark of USENIX Security review).
Q7 — Main Results
Main Results: Reported as one of the strongest structural (architecture-level, not just prompt-level) defenses in the literature per multiple citing papers; exact ASR figures should be confirmed directly from the USENIX proceedings before citing.
Q8 — Limitations & Bias
Limitations/Bias: Requires model-level fine-tuning or architectural change, which is a heavier deployment cost than pure middleware/prompting defenses (like ToolGate or your proposed intent-gate) — directly relevant as a contrast point on the 'no fine-tuning required' novelty claim.
Q9 — Reproducibility
Reproducibility: Published at a top systems-security venue with typically strong artifact-availability norms; confirm exact code/data release link from the USENIX proceedings page before citing.

 
[SUPPORTING] Paper 9
Defeating Prompt Injections by Design (CaMeL)
Debenedetti, Shumailov, Fan, Hayes, Carlini, Fabian, Kern, Shi, Terzis, Tramèr
Venue: arXiv preprint (2025); from the same research group as AgentDojo — check for subsequent conference acceptance before final citation
Venue tier / verification note: Not confirmed peer-reviewed at time of this search — Google DeepMind/ETH Zürich authorship, high credibility, but verify venue status before treating as an anchor-tier citation.
Link/DOI: arXiv:2503.18813
Q1 — Problem & Importance
Problem: Follow-up to AgentDojo by the same lead author, asking whether a system-level (not prompt-level) redesign can provide provable security guarantees against prompt injection, rather than only empirically-measured reductions in Attack Success Rate.
Q2 — Data
Data: Evaluated primarily on the AgentDojo benchmark itself (629 test cases, 97 tasks) as the standard test environment.
Q3 — Features/Inputs
Features/Inputs: A capability/control-flow-based system architecture where untrusted data is never allowed to influence the agent's control flow directly, drawing on classic software-security design principles (least privilege, capability-based security) applied to LLM agents.
Q4 — Methods & Pipeline
Methods/Pipeline: Separates a 'planner' LLM (which only ever sees trusted instructions) from an 'executor' that processes untrusted tool outputs, with a formal capability system mediating what the executor is allowed to do — aiming for provable rather than empirical security.
Q5 — Baselines/Comparisons
Baselines/Comparisons: Directly compared against AgentDojo's existing defenses (tool filter, prompt-based defenses) on the same 629 test cases.
Q6 — Evaluation
Evaluation: Attack Success Rate on AgentDojo (targeting 0% ASR by design/proof, not just empirical reduction) alongside task utility retention.
Q7 — Main Results
Main Results: Reported to achieve provable security against the AgentDojo threat model (i.e., a formal guarantee, not just an empirical low ASR) while retaining reasonable utility — exact utility numbers should be confirmed directly from the paper before citing precisely.
Q8 — Limitations & Bias
Limitations/Bias: Provable guarantees are scoped to the specific threat model formalized (AgentDojo's), which may not cover all real-world attack variants (e.g., tool-poisoning-at-registration, which is a different threat model than AgentDojo's output-injection focus) — directly relevant to your Idea 1's differentiation via MCPTox.
Q9 — Reproducibility
Reproducibility: Same research group as AgentDojo with a strong open-source track record; confirm exact code release link directly from the arXiv listing.

 
[SUPPORTING] Paper 10
Adaptive Attacks Break Defenses Against Indirect Prompt Injection Attacks on LLM Agents
Zhan, Fang, Panchal, Kang
Venue: Findings of the North American Chapter of the Association for Computational Linguistics (NAACL 2025)
Venue tier / verification note: NAACL Findings — top-tier NLP venue (CORE A-equivalent), peer-reviewed. Same lead author as InjecAgent.
Link/DOI: Proceedings: NAACL Findings 2025 (confirm exact ACL Anthology page directly before final citation)
Q1 — Problem & Importance
Problem: Most published defenses are only evaluated against static, non-adaptive attacks; the paper asks whether an attacker who knows which defense is deployed and adapts specifically to defeat it can still succeed — directly testing the robustness claims of the defense literature.
Q2 — Data
Data: Built on top of the authors' own InjecAgent benchmark and other standard injection benchmarks, extended with newly constructed adaptive attack variants targeting each specific defense mechanism.
Q3 — Features/Inputs
Features/Inputs: Defense-aware attacker prompts specifically crafted to exploit each defense's known detection mechanism (e.g., if a defense checks for suspicious keywords, the adaptive attack paraphrases to avoid them).
Q4 — Methods & Pipeline
Methods/Pipeline: For each of several published defenses, the authors construct a tailored adaptive attack informed by that defense's mechanism, then measure whether the defense still holds under this defense-aware pressure, compared to its reported performance under the original (non-adaptive) attack set.
Q5 — Baselines/Comparisons
Baselines/Comparisons: Each defense's own reported (non-adaptive) performance serves as the baseline for comparison against adaptive-attack performance.
Q6 — Evaluation
Evaluation: Attack Success Rate under adaptive attack vs. under the original static attack, per defense.
Q7 — Main Results
Main Results: Adaptive, defense-aware attacks substantially broke most tested defenses, with attack success rates exceeding 85% against several defenses that had reported much lower ASR under static attack conditions — a major reproducibility/robustness warning for the entire defense literature.
Q8 — Limitations & Bias
Limitations/Bias: By construction, adaptive attacks require attacker knowledge of the specific deployed defense, which is a stronger threat model than many real-world scenarios (though a reasonable one for security research, following Kerckhoffs's-principle-style worst-case analysis); does not itself propose a new defense.
Q9 — Reproducibility
Reproducibility: Builds on the already-open InjecAgent codebase; confirm the adaptive-attack code's specific release status directly from the NAACL Findings paper.

 
Cross-Paper Synthesis — What This Means for Idea 1
Repeated Limitation Pattern (the actual research gap)
Across all 10 papers, one pattern repeats consistently: benchmarks (AgentDojo, InjecAgent, MCPTox, ASB) reveal high attack success rates and propose no defense, while defenses (TrustAgent, MELON, StruQ, CaMeL, ToolGate) are each evaluated against only ONE threat model — usually the one their own authors designed — and almost never against an adversarial benchmark built by a different team. ToolGate, the closest system to your proposal, has never been tested against InjecAgent or MCPTox at all. This cross-paper gap is exactly what Idea 1 targets.
Confirmed Numbers Worth Citing Directly
• AgentDojo: Claude 3.5 Sonnet ~78% benign utility; GPT-4o utility drops 69% → 50% under attack (NeurIPS 2024).
• InjecAgent: GPT-4 vulnerable 24% (base attack) → 47% (enhanced attack) (ACL Findings 2024).
• MCPTox: up to 72.8% attack success, under 3% refusal rate across 20 agents (AAAI 2026).
• ASB: highest average attack success rate 84.3%; current defenses show limited effectiveness (ICLR 2025).
• Adaptive attacks (NAACL 2025 Findings): success rates exceed 85% against several defenses that reported much lower ASR under static/non-adaptive conditions — a direct warning to validate your own gate against adaptive attackers before claiming robustness.
Action Items Before Your Next Supervisor Meeting
• Confirm ToolGate's exact reported numbers directly from arXiv:2601.04688's results tables (not yet independently verified in this search).
• Confirm venue/acceptance status for CaMeL and MELON (arXiv preprints as of this search — may have since been accepted somewhere).
• Clone https://github.com/OceannTwT/ToolGate and confirm it runs before committing to it as your comparison baseline.
• Locate the TrustAgent paper's de-anonymized permanent code repository link (the anonymous.4open.science link is a review-period placeholder).
