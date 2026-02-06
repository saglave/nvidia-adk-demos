# Multi-Agent NAT (NVIDIA Agent Toolkit) Project

This project demonstrates different architectural approaches for implementing multi-agent systems using NAT (NVIDIA Agent Toolkit) with Google ADK (Agent Development Kit).

## Architectural Approaches

### Approach 1: Agent-as-a-Tool (`nat_adk_individual_wrap/`)

Each agent is individually wrapped as a separate NAT function and managed independently. Sub-agents are treated as tools that can be called by other agents.

### Approach 2: Wrap LLM calls and Tool calls (`nat_adk_single_wrap/`)

The entire multi-agent system is wrapped as a single NAT function with internal agent composition. LLM calls and tool calls are wrapped together within a unified system.

### Approach 3: Native Agent Support in NAT
NAT natively supports agents and their interactions without needing to wrap them as tools. This will require adding an `agent` component to NAT and framework-specific implementation for each agent type.


## Usage

### Approach 1 (Individual Wrap)
```bash
nat run --config_file src/nat_adk_individual_wrap/configs/config.yml --input "What is the time in New York?"
```

### Approach 2 (Single Wrap)
```bash
nat run --config_file src/nat_adk_single_wrap/configs/config.yml --input "What is the time in New York?"
```

### Approach 3 (Native Agent Support)
Find the sample config file in the src/3_native_agent_component/configs/ directory.
