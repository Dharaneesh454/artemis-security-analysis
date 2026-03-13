# ARTEMIS Autonomous Security Analysis of OWASP Juice Shop

## Overview

This project demonstrates an autonomous security analysis workflow using the **ARTEMIS security framework**. ARTEMIS orchestrates large language models and specialized agents to perform automated vulnerability discovery and exploit analysis.

The scan was performed on **OWASP Juice Shop**, a deliberately vulnerable web application used for security testing. The experiment evaluates how ARTEMIS can autonomously perform reconnaissance, vulnerability assessment, and exploit analysis while maintaining full observability using **Langfuse tracing**.

---

# Target Application

OWASP Juice Shop
http://localhost:4000

OWASP Juice Shop is a modern web application intentionally designed with security vulnerabilities for educational and research purposes.

---

# Tools and Technologies

| Component          | Description                                          |
| ------------------ | ---------------------------------------------------- |
| ARTEMIS            | Autonomous security testing framework                |
| Azure GPT-4.1-mini | LLM used for reasoning and decision making           |
| Codex              | Used for exploit generation and script execution     |
| Langfuse           | Observability and tracing platform for LLM workflows |
| Python             | Runtime environment for orchestration                |

---

# System Architecture

ARTEMIS operates using a **supervisor-agent architecture**.

1. Supervisor agent orchestrates the entire security workflow
2. Specialized agents perform targeted security tasks
3. Codex instances execute exploit scripts
4. Langfuse records every LLM interaction and tool call

```
ARTEMIS Supervisor
        │
        │
        ├── Task Router
        │
        ├── Exploit Agents
        │       ├── XSS Analysis
        │       ├── Privilege Escalation
        │       └── Information Leakage
        │
        ├── Codex Execution Engine
        │
        └── Langfuse Observability
```

---

# Scan Configuration

Supervisor Model:

```
azure/gpt-4.1-mini
```

Scan Duration:

```
30 minutes
```

Configuration File:

```
configs/tests/juice_shop.yaml
```

Execution Command:

```bash
python -m supervisor.supervisor --config-file configs/tests/juice_shop.yaml --duration 30 --supervisor-model azure/gpt-4.1-mini
```

---

# Observability and Tracing

The entire workflow was monitored using **Langfuse**, which recorded:

* LLM prompts and responses
* Tool execution calls
* Supervisor decisions
* Codex instance execution
* Iteration metadata

Example trace event:

```
Trace Name: OpenAI-generation
Input: "No new updates from instances. Decide next action."
Output: "I will now read logs from exploit instances."
```

Total recorded traces:

```
37
```

---

# Execution Summary

Supervisor iterations executed:

```
21
```

Security modules triggered:

```
exploit_xss
privilege_escalation
exploit_infoleak
```

Tools utilized:

```
spawn_codex
read_instance_logs
send_followup
wait_for_instance
```

---

# Results

The automated scan completed successfully.

Outcome:

```
No confirmed exploitable vulnerabilities were detected during automated testing.
```

Although the system executed multiple exploit analysis modules, none of the generated exploit attempts resulted in confirmed vulnerabilities with proof of compromise.

---

# Recommendations

While automated scanning provides initial insights, manual testing is recommended for detecting complex vulnerabilities such as:

* Business logic flaws
* Authentication bypass
* Chained vulnerabilities
* Privilege escalation paths
* Advanced injection attacks

---

# Example Langfuse Trace

The following screenshot shows trace data collected during execution.

(Add screenshot here)

```
docs/langfuse_traces.png
```

---

# Project Structure

```
ARTEMIS
│
├── supervisor/
├── configs/
│   └── tests/
│        └── juice_shop.yaml
│
├── logs/
│
├── docs/
│   └── langfuse_traces.png
│
└── README.md
```

---

# Conclusion

This experiment demonstrates the ability of ARTEMIS to autonomously orchestrate LLM-driven agents for security analysis tasks. The integration of Langfuse provides valuable observability into the decision-making process of AI systems.

Although no confirmed vulnerabilities were detected in this automated run, the framework successfully executed a full autonomous testing workflow including exploit generation, monitoring, and reasoning.

This approach highlights the potential of AI-assisted security automation in modern application security testing.

---

# Author

Dharaneesh V
