<img width="860" height="639" alt="image" src="https://github.com/user-attachments/assets/9ed12ee0-3a3f-422b-abb8-a37bcb14efff" />ARTEMIS Tool Analysis Report

Autonomous Security Analysis of OWASP Juice Shop using ARTEMIS and Langfuse

Author: Dharaneesh V

Overview

This project analyzes the ARTEMIS autonomous security analysis framework, focusing on its architecture, runtime behavior, agent communication, vulnerability discovery capability, and cost observability using Langfuse tracing.

ARTEMIS orchestrates multiple AI agents powered by large language models to perform automated vulnerability analysis and exploit attempts on web applications.

The tool was executed against the OWASP Juice Shop application and monitored using Langfuse observability to capture LLM interactions, tool executions, and decision flows.

Target Application

OWASP Juice Shop

http://localhost:4000

OWASP Juice Shop is a deliberately vulnerable web application designed for security testing and training.

D1: Architecture & Agent Communication Analysis
Agent Architecture Map

ARTEMIS uses a supervisor-agent architecture where a central supervisor coordinates multiple specialized agents.

Architecture overview:

Supervisor Agent
│
├── Context Manager
│
├── Task Router
│
├── Recon Agent
│
├── Exploit Agents
│     ├── XSS Agent
│     ├── Privilege Escalation Agent
│     └── Information Disclosure Agent
│
├── Codex Execution Engine
│
└── Langfuse Observability Layer

The Supervisor Agent orchestrates the workflow and decides which agent should perform the next action.

Agent Communication Flow

The system communicates through structured prompts and tool calls managed by the supervisor.

Communication process:

Supervisor sends prompt to LLM.

LLM determines next task.

Task Router selects the appropriate specialist agent.

Codex instance executes the exploit attempt.

Logs are analyzed by the supervisor.

The process repeats until completion.

Example runtime trace captured in Langfuse:

Input:
"No new updates from instances. Decide next action."

Output:
"I will now read logs from the exploit instances."

Conversation history is maintained by the ContextManager and shared across iterations.

Context Management

ARTEMIS uses a ContextManager to manage LLM token limits.

Observed configuration from runtime logs:

Max tokens: 200,000
Buffer threshold: 15,000
Compression trigger: 185,000 tokens

This prevents context overflow during long-running sessions.

LLM Call Patterns

The system uses the following model:

azure/gpt-4.1-mini

LLM call pattern:

System Prompt
↓
Supervisor Prompt
↓
Task Router Decision
↓
Tool Call
↓
Execution Result Analysis

Example trace event:

Trace Name: OpenAI-generation

Input:
"You are a task router that determines which specialist should handle a task."

Output:
{"specialist":"web"}
Output Structure

Agents return structured JSON responses.

Example:

{
  "specialist": "web"
}

These outputs are interpreted by the supervisor and used to determine subsequent actions.

Decision Logic

The supervisor determines the next action using:

Instance execution logs

Router decisions

LLM reasoning

exploit execution results

Decision flow:

Analyze logs
↓
Select specialist agent
↓
Spawn exploit instance
↓
Monitor execution
↓
Repeat
D2: Runtime Failure & Success Analysis
Where It Failed

Observed limitations during execution:

No successful vulnerability exploitation

Automated payload generation limitations

Dependency on LLM reasoning for exploit selection

Although exploit agents executed successfully, no confirmed vulnerabilities were exploited.

Why It Failed

Possible root causes:

Juice Shop vulnerabilities require complex multi-step exploitation.

Generated exploit payloads may not match vulnerable endpoints.

LLM reasoning sometimes produces high-level analysis rather than concrete exploit payloads.

Where It Worked Well

The system demonstrated strong capabilities in:

multi-agent orchestration

autonomous decision making

exploit agent execution

runtime monitoring

trace-based observability

The supervisor successfully completed the session without runtime failure.

Improvement Recommendations

Integrate predefined exploit payload libraries.

Implement vulnerability-specific scanning agents.

Add automated endpoint discovery tools.

Improve exploit payload generation prompts.

Implement automatic vulnerability validation.

D3: Findings Validation
True Positives

No confirmed vulnerabilities were successfully exploited during automated testing.

False Positives

No incorrect vulnerability findings were reported.

False Negatives

Known vulnerabilities in Juice Shop that were not exploited include:

Stored Cross-Site Scripting (XSS)

Broken authentication scenarios

Injection vulnerabilities

These were missed due to limitations in automated exploit generation.

Coverage Assessment
OWASP Category	Coverage
Injection	Partial
Broken Authentication	Limited
Cross-Site Scripting	Partial
Access Control	Limited
Security Misconfiguration	Limited

The tool focuses mainly on exploit automation rather than full vulnerability discovery coverage.

D4: Cost & Observability Analysis (Langfuse)

Langfuse captured all LLM interactions and execution traces.

Observability Metrics

Total traces recorded:

37

Total token usage:

280.78K tokens

Total cost:

$0.053803

Model used:

azure/gpt-4.1-mini
Latency Analysis

Estimated latency:

Component	Latency
LLM response	2–5 seconds
Codex execution	10–30 seconds

The main bottleneck was exploit instance execution time.

Langfuse Observability Features

Langfuse provided:

complete trace history

prompt and response logging

tool call hierarchy

execution timeline

cost monitoring

Example Langfuse trace:

Trace Name: OpenAI-generation

Input:
"No new updates from instances. Decide next action."

Output:
"I will now finish the session."
Execution Summary

Supervisor iterations completed:

21

Security modules executed:

exploit_xss
privilege_escalation
exploit_infoleak

Tools used:

spawn_codex
read_instance_logs
send_followup
wait_for_instance

Final supervisor message:

Concluded automated testing of OWASP Juice Shop.
No confirmed exploitable vulnerabilities detected.
Manual review recommended.
D5: Live Demo & Walkthrough

The live demonstration will include:

Running ARTEMIS against the Juice Shop application.

Displaying supervisor logs and agent communication.

Exploring Langfuse traces and cost analysis.

Demonstrating exploit agent execution.

Reviewing the final results and analysis.

Screenshots
Langfuse Tracing Dashboard

(Add your tracing screenshot here)

Execution Logs

(Add terminal execution screenshots)

Langfuse Cost Dashboard

(Add cost dashboard screenshot)

Conclusion

The ARTEMIS framework successfully demonstrated an AI-driven autonomous security testing workflow. The system coordinated multiple agents, generated exploit attempts, and monitored execution through Langfuse observability.

Although no vulnerabilities were automatically exploited during this run, the framework effectively showcased the potential of AI-based security automation combined with trace-based monitoring.

Future improvements could significantly enhance vulnerability detection accuracy and exploit success rates.
