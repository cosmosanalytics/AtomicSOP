# Declarative SOPs: Constraining Action Spaces with Machine-Readable Runbooks

> **"Constrain action spaces with machine-readable runbooks (YAML/JSON schemas) rather than raw open prompts."**

---

## 💡 The Architectural Problem

Traditional agentic architectures feed unstructured natural language Standard Operating Procedures (SOPs) directly into LLM context windows as raw open prompts. 

This approach suffers from critical production failure modes:
* **Stochastic Drift**: Even with low temperature, language models drift over multi-step workflows.
* **Hallucinated Parameters**: When boundaries are described in prose, agents fabricate unverified thresholds.
* **Bypassed Authorization**: Open-ended prompting lacks verifiable gates, allowing agents to execute out-of-order mutations.
* **Silent Failure Modes**: Violations occur without tripping machine-verifiable circuit breakers.

---

## ⚡ The Solution: Declarative SOP Runbooks

**AtomicSOP** decomposes complex, dense SOPs into combinations of the **255 Atomic Policy Laws** from the Policy Making Ontology, then compiles them into **Declarative Runbooks** (YAML and JSON Schema Draft 2020-12):

1. **Explicit Action Space Enumeration**:
   Agents are strictly restricted to an allowed set of tool calls (`allowed_tools`). Any invocation of an unapproved tool or arbitrary mutation is rejected at schema validation time before execution.
2. **Machine-Readable Parameter Contracts**:
   Parameters are strictly typed with hard bounds (`minimum`, `maximum`, `enum`, `required`), preventing hallucinated inputs.
3. **Hard Mathematical Invariant Bounds**:
   Operational policies enforce non-negotiable inequalities ($P_{floor} \ge P^*$, $\sum Duty \le 60$, $T_{excursion} \le 15\text{m}$) directly in the agent's parameter validation layer.
4. **Finite State Machine Lifecycle Guards**:
   State transitions ($A \rightarrow B \rightarrow C \rightarrow D$) are guarded by deterministic conditions, ensuring terminal auditability.

---

## 🛠️ Machine-Readable Runbook Formats

### 1. YAML Runbook (`.yaml`)
```yaml
schema_version: "2.0-declarative-runbook"
runbook_id: "RUNBOOK-PUBLIC-WATER-LEAD"
metadata:
  title: "Public Water Utility Lead and Copper Rule (LCR) Exceedance Protocol"
  governing_ontology: "8 Components / 42 Classes / 255 Atomic Policies"

constrained_action_space:
  allowed_tools:
    - tool: "log_action_level_exceedance"
      policy_id: 4
      guard_invariant: "lead_ppb > 15.0"
      permitted_fsm_state: "STATE_A_INTAKE"
    - tool: "dispatch_mandatory_public_education"
      policy_id: 38
      guard_invariant: "days_elapsed <= 60"
      permitted_fsm_state: "STATE_B_TRIGGER"
    - tool: "schedule_lead_service_line_replacement"
      policy_id: 141
      guard_invariant: "annual_replacement_percentage >= 7.0"
      permitted_fsm_state: "STATE_C_GUARD"

  forbidden_actions:
    - "unregistered_external_http_call"
    - "unsupervised_state_mutation"
    - "suppress_exceedance_telemetry"
```

### 2. JSON Schema Draft 2020-12 (`.json`)
Directly compatible with **OpenAI Function Calling**, **Claude Tool Use**, and **Model Context Protocol (MCP)**:
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "ConstrainedAgentActionSpace",
  "type": "object",
  "required": ["action_id", "parameters", "fsm_state", "invariant_proof"],
  "additionalProperties": false,
  "properties": {
    "action_id": {
      "type": "string",
      "enum": ["log_action_level_exceedance", "dispatch_mandatory_public_education", "schedule_lead_service_line_replacement"]
    },
    "fsm_state": {
      "type": "string",
      "enum": ["STATE_A_INTAKE", "STATE_B_TRIGGER", "STATE_C_GUARD", "STATE_D_TERMINAL"]
    }
  }
}
```

---

## 🌐 Live Web App

Try the interactive compiler and Action Space Boundary Breaker playground live:
👉 **[https://cosmosanalytics.github.io/AtomicSOP/](https://cosmosanalytics.github.io/AtomicSOP/)**
