ADK Features for Functional Multi-Agent Support

MVP Requirements

1. Sub-Agent Invocation and Context Management
- Proper context passing to sub-agents - Session state, conversation history
- Error propagation from child to parent agents
- Flow termination capability by sub-agents (Part of orchestration logic)
2. Agent Composition Patterns
- Support for SequentialAgent, ParallelAgent, LoopAgent - Doesn't require separate handling as long as ADK orchestration logic is retained
- Agents-as-tools should be treated as such
3. Multi-Agent Orchestration
- Retain ADK's orchestration logic - NAT should not alter this
- Sub-agent can terminate flows - ADK orchestrator handles this
- Handle context transfer as specified - ADK allows to decide whether or not to pass session state, conversation history, etc. to sub-agents. This should be respected by NAT
4. Agent Lifecycle Management
- Creation, initialization, execution, termination of each agent
- Exit conditions and graceful termination for agent loops
- Exception handling
5. Observability
- Full trace propagation - Agent hierarchy should be visible in traces
- `transfer_to_agent` shouldn't show up as a separate tool call
6. Testing & Validation
- Integration testing for multi-agent flows - Agent trajectory, for example, should be evaluatable

Can Wait

1. Shared State & Context
- Prompt injection from session state - If the prompt is specified in NAT config, it should know to inject session state into the prompt
2. Testing & Validation
- Isolation testing for individual agents