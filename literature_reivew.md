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
