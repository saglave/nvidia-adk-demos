# Multi-Agent NAT (NVIDIA Agent Toolkit) Project

This project demonstrates two different architectural approaches for implementing multi-agent systems using NAT (NVIDIA Agent Toolkit) with Google ADK (Agent Development Kit). Both implementations provide weather and time functionality but differ fundamentally in how they handle agent composition and wrapping.

## 🏗️ Architectural Approaches

### Individual Wrap Approach (`nat_adk_individual_wrap/`)

**Philosophy**: Each agent is individually wrapped as a separate NAT function and managed independently.

**Architecture**:
- **Separate NAT Functions**: The sub-agent is defined as its own NAT function (`adk_sub_agent`) in a separate file (`sub_agent.py`)
- **Independent Wrappers**: Each agent (root and sub) gets its own ADK Runner, session service, and artifact service
- **NAT-Level Integration**: The root agent references the sub-agent through NAT's function registry using `await builder.get_function("adk_sub_agent")`
- **Distributed Management**: Each agent manages its own lifecycle, sessions, and state independently

**Key Characteristics**:
```python
# In agent.py - Root agent gets sub-agent via NAT
sub_agent = await builder.get_function("adk_sub_agent")
sub_agent_config = sub_agent.config
model = await builder.get_llm(sub_agent_config.llm, wrapper_type=LLMFrameworkEnum.ADK)

# In sub_agent.py - Separate NAT function registration
@register_function(config_type=ADKFunctionConfig, framework_wrappers=[LLMFrameworkEnum.ADK])
async def adk_agent(config: ADKFunctionConfig, builder: Builder):
    # Independent agent implementation
```

**Benefits**:
- ✅ **Modularity**: Each agent is a standalone, reusable NAT component
- ✅ **Scalability**: Easy to distribute agents across different services/processes
- ✅ **Independent Lifecycle**: Agents can be started, stopped, and updated independently
- ✅ **NAT Integration**: Full integration with NAT's function registry and lifecycle management

### Single Wrap Approach (`nat_adk_single_wrap/`)

**Philosophy**: The entire multi-agent system is wrapped as a single NAT function with internal agent composition.

**Architecture**:
- **Unified NAT Function**: Only the root agent is exposed as a NAT function
- **Internal Composition**: Sub-agents are created as ADK Agent objects directly within the root agent's implementation
- **Shared Infrastructure**: All agents share the same Runner, session service, and artifact service
- **ADK-Level Integration**: Agent composition happens purely within the ADK framework using the `sub_agents` parameter

**Key Characteristics**:
```python
# In agent.py - Sub-agent created internally as ADK Agent object
sub_agent = Agent(
    name="adk_sub_agent",
    model=await builder.get_llm(sub_agent_config.llm, wrapper_type=LLMFrameworkEnum.ADK),
    description="A sub-agent for demonstration purposes.",
    instruction=sub_agent_config.prompt,
    tools=tools,
    before_agent_callback=read_session_state_callback,
)

# Root agent directly includes sub-agent
agent = Agent(
    name=config.name,
    model=model,
    description=config.description,
    instruction=config.prompt,
    tools=tools,
    sub_agents=[sub_agent],  # Direct ADK composition
    before_agent_callback=update_session_state_callback
)
```

**Benefits**:
- ✅ **Simplicity**: Single NAT function to manage and deploy
- ✅ **Performance**: Shared infrastructure reduces overhead
- ✅ **Atomic Operations**: All agents are deployed and managed as one unit
- ✅ **Tight Coupling**: Efficient communication between agents in the same process

## 📊 Comparison Matrix

| Aspect | Individual Wrap | Single Wrap |
|--------|----------------|-------------|
| **NAT Functions** | Multiple (1 per agent) | Single (root only) |
| **Agent Lifecycle** | Independent | Unified |
| **Infrastructure** | Per-agent (separate Runners) | Shared (single Runner) |
| **Modularity** | High (separate deployments) | Low (monolithic) |
| **Resource Usage** | Higher (multiple services) | Lower (shared services) |
| **Scalability** | Horizontal (distribute agents) | Vertical (single process) |
| **Complexity** | Higher (coordination needed) | Lower (self-contained) |
| **Use Case** | Microservices architecture | Monolithic application |

## 🚀 Usage

### Individual Wrap Implementation
```bash
nat run --config_file src/nat_adk_individual_wrap/configs/config.yml --input "What is the time in New York?"
```

### Single Wrap Implementation
```bash
nat run --config_file src/nat_adk_single_wrap/configs/config.yml --input "What is the time in New York?"
```

## 🛠️ Setup

1. **Environment Setup**
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # On Windows: .venv\Scripts\activate
   ```

2. **Install Dependencies**
   ```bash
   pip install nvidia-nat  # Or your preferred NAT installation method
   ```

3. **Configuration**
   - Copy `.env.template` files to `.env` in each implementation directory:
     ```bash
     cp src/nat_adk_individual_wrap/.env.template src/nat_adk_individual_wrap/.env
     cp src/nat_adk_single_wrap/.env.template src/nat_adk_single_wrap/.env
     ```
   - Update the `.env` files with your OpenAI API credentials and endpoints

## 🔧 Features

Both implementations include:
- **Weather Update Tool**: Get current weather for specified cities
- **City Time Tool**: Get current time for specified cities
- **Sub-Agent**: Specialized agent for handling time-related queries
- **OpenAI LLM Integration**: Configurable LLM backend
- **Phoenix Tracing**: Built-in observability and tracing
- **Session Management**: Conversation state management with caching
- **Callback System**: Pre/post-agent execution hooks

## 🎯 When to Use Which Approach

### Choose Individual Wrap When:
- Building microservices-based architecture
- Need independent agent deployment and scaling
- Agents have different resource requirements
- Multiple teams developing different agents
- Fault isolation is critical
- Planning to distribute agents across multiple services

### Choose Single Wrap When:
- Building a cohesive application with tightly coupled agents
- Optimizing for performance and resource usage
- Simpler deployment and management is preferred
- All agents share similar infrastructure needs
- Quick prototyping and development

## 📁 Project Structure

```
src/
├── nat_adk_individual_wrap/          # Individual wrapping approach
│   ├── agent.py                      # Root agent (NAT function)
│   ├── sub_agent.py                  # Sub-agent (separate NAT function)
│   ├── nat_time_tool.py              # Time functionality
│   ├── weather_update_tool.py        # Weather functionality
│   ├── configs/config.yml            # Configuration
│   └── .env.template                 # Environment template
└── nat_adk_single_wrap/              # Single wrapping approach
    ├── agent.py                      # Combined agent (single NAT function)
    ├── nat_time_tool.py              # Time functionality
    ├── weather_update_tool.py        # Weather functionality
    ├── configs/config.yml            # Configuration
    └── .env.template                 # Environment template
```

## 📜 License

Licensed under the Apache License, Version 2.0. See the configuration files for full license text.