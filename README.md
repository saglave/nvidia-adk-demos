# Multi-Agent NAT (NVIDIA Agent Toolkit) Project

This project demonstrates different architectural approaches for implementing multi-agent systems using NAT (NVIDIA Agent Toolkit) with Google ADK (Agent Development Kit).

## Architectural Approaches

### Approach 1: Agent-as-a-Tool (`nat_adk_individual_wrap/`)

Each agent is individually wrapped as a separate NAT function and managed independently. Sub-agents are treated as tools that can be called by other agents.

### Approach 2: Wrap LLM calls and Tool calls (`nat_adk_single_wrap/`)

The entire multi-agent system is wrapped as a single NAT function with internal agent composition. LLM calls and tool calls are wrapped together within a unified system.

### Approach 3: [Template - To be filled]

[Description of third approach to be added]

## Usage

### Approach 1 (Individual Wrap)
```bash
nat run --config_file src/nat_adk_individual_wrap/configs/config.yml --input "What is the time in New York?"
```

### Approach 2 (Single Wrap)
```bash
nat run --config_file src/nat_adk_single_wrap/configs/config.yml --input "What is the time in New York?"
```

## Setup

1. **Environment Setup**
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # On Windows: .venv\Scripts\activate
   ```

2. **Install Dependencies**
   ```bash
   pip install nvidia-nat
   ```

3. **Configuration**
   ```bash
   cp src/nat_adk_individual_wrap/.env.template src/nat_adk_individual_wrap/.env
   cp src/nat_adk_single_wrap/.env.template src/nat_adk_single_wrap/.env
   ```
   Update the `.env` files with your OpenAI API credentials and endpoints.

## License

Licensed under the Apache License, Version 2.0.