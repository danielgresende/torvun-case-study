# One microVM per agent

An agent is not a browser tab. Each company agent runs in its own microVM with its own browser, desktop, terminal, connectors and routines. The host gives it no route to another agent's VM, no route to another company's data and no inbound route from third parties.

```mermaid
flowchart TB
  Owner["Owner"] -->|"confirms one action"| Approval["Owner approval gate"]
  Owner -->|"Stop"| Orchestrator["Agent orchestrator"]
  Approval -->|"approved intent"| Orchestrator
  Orchestrator -->|"start · stop"| VM1
  Orchestrator -->|"start · stop"| VM2
  subgraph Host["Host and runtime"]
    subgraph VM2["microVM · agent 2"]
      A2["Browser · desktop · terminal<br/>connectors · routines"]
    end
    Note["No route between agent VMs<br/>no route to another company's data"]:::note
    subgraph VM1["microVM · agent 1"]
      A1["Browser · desktop · terminal<br/>connectors · routines"]
    end
  end
  VM1 -->|"outbound only<br/>no inbound · no VM-to-VM"| Web["Third-party sites"]
  classDef note fill:#fff8e1,stroke:#c9a227,stroke-dasharray:5 3,color:#4a3b00
```

The agent can browse, click, type, run a terminal command, use a connector or follow a routine, because the thing it acts inside is disposable and isolated. The owner is not trusting a chat message: the approval gate binds one exact intent, with a target, a payload and an expiry, to one confirmation, and the riskiest steps also need the owner's password. Stop ends the VM. That is the difference between an assistant that talks about work and an agent that can be allowed to do it.
