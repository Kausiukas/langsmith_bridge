# CODE INVENTORY & MAPPING

This document lists all major code components in the application, with a focus on identifying LangSmith Bridge-oriented code for migration to a separate repository.

---

## 1. Top-Level Scripts & Entry Points
- **background_agents_dashboard.py**: Streamlit dashboard for monitoring all agents, including LangSmith Bridge Agent. Imports and initializes agents.
- **launch_background_agents.py**: Script for launching background agents, including LangSmith Bridge Agent (see `_create_phase1_agents`).
- **example_integration.py**: Example of integrating background agents (including LangSmith Bridge) into another system.

---

## 2. Core Agent & Monitoring Modules
- **background_agents/monitoring/langsmith_bridge.py**: 
  - Implements `LangSmithBridgeAgent` (core of LangSmith Bridge functionality).
  - Handles API connection, trace collection, performance metrics, error detection, and insight generation.
  - Integrates with tracing and shared state.
- **background_agents/monitoring/__init__.py**: Exports `LangSmithBridgeAgent` for use in other modules.
- **background_agents/monitoring/langgraph_executor.py**: Profile Import Executor Agent (not LangSmith-specific, but interacts via shared state and tracing).
- **background_agents/monitoring/self_healing_agent.py**: Self-Healing Autopatch Agent (not LangSmith-specific).

---

## 3. Coordination & Shared State
- **background_agents/coordination/agent_coordinator.py**: AgentCoordinator for managing and registering agents (including LangSmith Bridge).
- **background_agents/coordination/shared_state.py**: Shared state and metrics storage for all agents.
- **background_agents/coordination/base_agent.py**: Base class for all agents.

---

## 4. Tests & Utilities
- **test_background_agents.py**: Tests creation and registration of LangSmithBridgeAgent.
- **test_langsmith.py**: (If present) Additional tests for LangSmith integration.
- **utils/**: May contain utility code used by agents (not LangSmith-specific unless referenced).

---

## 5. Configuration & Documentation
- **requirements.txt**: Lists dependencies (aiohttp, langchain, etc.).
- **langsmith_bridge.md**: Documentation for LangSmith Bridge Agent.
- **langsmith_bridge_architecture.md**: Architecture diagrams and schemas for LangSmith Bridge and related agents.
- **2025-06-15_update_workplan.md**: Workplan including migration and code mapping tasks.

---

## 6. Cross-References & Imports
- `LangSmithBridgeAgent` is imported and used in:
  - `background_agents_dashboard.py`
  - `launch_background_agents.py`
  - `example_integration.py`
  - `test_background_agents.py`
  - `background_agents/monitoring/__init__.py`
  - `background_agents/__init__.py`
- It is registered with the `AgentCoordinator` and interacts with `SharedState`.

---

## 7. Migration Notes (LangSmith Bridge)
- **To Move:**
  - `background_agents/monitoring/langsmith_bridge.py` (core agent)
  - Related tests: `test_background_agents.py` (LangSmithBridgeAgent sections)
  - Documentation: `langsmith_bridge.md`, `langsmith_bridge_architecture.md`
  - Any config or environment setup specific to LangSmith Bridge
- **Dependencies:**
  - `aiohttp`, `langchain`, `os`, `asyncio`, `datetime`, `json`, `logging`, `traceback`
  - Internal: `BaseAgent`, `AgentState`, `SharedState`, `PerformanceMetric`
- **To Review:**
  - Any shared code in `coordination/` or `monitoring/` that is tightly coupled and may need to be refactored for modularity.
  - Tracing integration (ensure all tracing logic is portable or abstracted).

---

## 8. Next Steps
- Review this inventory with stakeholders.
- Tag all LangSmith Bridge-specific code for migration.
- Draft migration scripts and update documentation as needed. 