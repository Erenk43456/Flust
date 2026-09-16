# Flust

> A Python based modular AI agent framework designed for autonomous task execution, AI-assisted software development, persistent memory, and self-improvement.

**Flust** is a Python-based AI agent framework built around modular agents, orchestrators, tools, memory systems, repository intelligence, and model abstractions.

The project is designed around a simple principle:

```text
Understand → Plan → Execute → Validate → Learn → Improve
```

The long-term objective is to evolve Flust from an AI-assisted development framework into a system capable of **continuously analyzing, validating, and improving its own software environment through a controlled self-development loop**.

---

# Vision

Flust is being developed toward a **self-developing AI system**.

The long-term architecture is intended to allow Flust to:

* understand a software repository
* maintain persistent project knowledge
* identify problems and improvement opportunities
* create structured development plans
* modify code through controlled tools
* validate changes through automated tests
* analyze execution results
* update its project knowledge
* identify the next improvement cycle

The target architecture is therefore not simply:

```text
User → AI → Code
```

but:

```text
                ┌──────────────────────┐
                │       Flust          │
                │                      │
                │  Understand          │
                │  Plan                │
                │  Execute             │
                │  Validate            │
                │  Learn               │
                │  Improve             │
                └──────────┬───────────┘
                           │
                           ▼
                    Next Development
                         Cycle
```

The self-development loop is a **long-term architectural goal**. The current implementation already contains several of its required building blocks, but autonomous self-improvement is not yet considered a fully implemented subsystem.

---

# Architecture

Flust uses a layered architecture based on dependency containers, orchestrators, agents, tools, memory, repository intelligence, and LLM abstractions.

The current application composition is centered around `MainContainer`.

```text
                         MainContainer
                              │
        ┌─────────────┬───────┼────────┬──────────────┐
        │             │       │        │              │
        ▼             ▼       ▼        ▼              ▼
   CoreContainer  ModelContainer  MemoryContainer  ToolContainer
        │             │       │        │              │
        │             │       │        ▼              │
        │             │       │   Memory System       │
        │             │       │                       │
        │             │       └────────┬──────────────┘
        │             │                │
        │             ▼                ▼
        │        LLM Providers      Agents
        │                              │
        │                              ▼
        │                       System Containers
        │                         │           │
        │                         ▼           ▼
        │                       Chat      Development
        │                         │           │
        └─────────────────────────┴─────┬─────┘
                                        ▼
                                MainOrchestrator
```

The architecture separates **dependency composition** from **runtime execution**.

```text
Composition
    ↓
Containers
    ↓
Orchestrators
    ↓
Agents
    ↓
Tools / Memory / LLM
```

---

# Runtime Request Flow

A normal request enters through the application layer and is routed by the main orchestrator.

```text
User
 │
 ▼
PySide6 Application
 │
 ▼
AI Worker
 │
 ▼
MainOrchestrator
 │
 ▼
DecisionAgent
 │
 ├───────────────┬────────────────┐
 ▼               ▼                ▼
Chat          Memory         Development
 │               │                │
 ▼               ▼                ▼
Chat           Memory       Development
Orchestrator   Orchestrator  Orchestrator
```

The `MainOrchestrator` is responsible for selecting the appropriate subsystem and forwarding the request together with the execution trace.

---

# Development Architecture

Development tasks are handled by a dedicated development subsystem.

```text
                       DevelopmentContainer
                                │
        ┌───────────────────────┼────────────────────────┐
        │                       │                        │
        ▼                       ▼                        ▼
 DevelopmentContext       ProjectMemorySync       WorkspaceWatcher
        │                       │                        │
        │                       ▼                        │
        │                Project Memory                  │
        │                                                │
        └───────────────────────┬────────────────────────┘
                                │
                                ▼
                    DevelopmentOrchestrator
                                │
             ┌──────────────────┼──────────────────┐
             │                  │                  │
             ▼                  ▼                  ▼
          Planner            ToolAgent       Repository Analyzer
             │                  │
             ▼                  │
      Structured Plan           │
                                ▼
                         Tool Registry
                                │
              ┌─────────────────┼─────────────────┐
              │                 │                 │
              ▼                 ▼                 ▼
          File Tool       Code Tools        Analysis Tools
                                │
                                ▼
                           Execution
                                │
                                ▼
                             Result
```

The current development orchestrator supports three development actions:

```text
analyze
improve
code
```

Normal code tasks follow the development context and planning pipeline. Repository analysis can be used directly, while project-memory information can be synchronized when the required development context is unavailable.

---

# Development Execution Pipeline

The primary development execution path is:

```text
User Request
     │
     ▼
DevelopmentOrchestrator
     │
     ▼
DevelopmentContext
     │
     ├── Project Memory
     │
     └── Repository Context
     │
     ▼
PlannerAgent
     │
     ▼
Structured Execution Plan
     │
     ▼
ToolAgent
     │
     ▼
Tool Registry
     │
     ├── File Operations
     ├── Code Operations
     ├── Repository Analysis
     └── Other Registered Tools
     │
     ▼
Execution Results
     │
     ▼
Development Response
```

The LLM is therefore separated from direct infrastructure access.

```text
LLM
 ↓
Plan
 ↓
Validated Application Flow
 ↓
Controlled Tool
 ↓
Result
```

---

# Self-Development Architecture

The long-term self-development system extends the current development pipeline with validation, feedback, and improvement cycles.

```text
                    Self-Development Loop
                           │
                           ▼
                  Repository Observation
                           │
                           ▼
                    Repository Analysis
                           │
                           ▼
                     Problem Detection
                           │
                           ▼
                   Improvement Planning
                           │
                           ▼
                    Controlled Changes
                           │
                           ▼
                       Validation
                           │
                  ┌────────┴────────┐
                  │                 │
                PASS              FAIL
                  │                 │
                  ▼                 ▼
             Knowledge         Failure Analysis
              Update                │
                  │                 │
                  └────────┬────────┘
                           ▼
                    Improvement Memory
                           │
                           ▼
                    Next Development
                         Cycle
```

This architecture is deliberately separated from unrestricted autonomous modification.

Future self-development must remain bounded by:

* workspace isolation
* explicit tool capabilities
* structured plans
* validation
* test execution
* execution tracing
* persistent project knowledge
* failure propagation

---

# Agents

Agents provide specialized reasoning or execution responsibilities.

The current architecture includes components such as:

| Agent           | Responsibility                             |
| --------------- | ------------------------------------------ |
| `DecisionAgent` | Request and subsystem classification       |
| `PlannerAgent`  | Natural language request → structured plan |
| `ToolAgent`     | Controlled execution of planned tool steps |
| `CodeAgent`     | Code generation and modification           |
| `MemoryAgent`   | Memory-oriented operations                 |

Agents receive their dependencies from the application containers rather than constructing the complete application graph themselves.

---

# Orchestrators

Orchestrators control workflows between agents, tools, memory, and application systems.

| Orchestrator              | Responsibility         |
| ------------------------- | ---------------------- |
| `MainOrchestrator`        | Global request routing |
| `ChatOrchestrator`        | Chat workflow          |
| `MemoryOrchestrator`      | Memory workflow        |
| `DevelopmentOrchestrator` | Development workflow   |

This separation keeps system-level workflow control independent from individual tool implementations.

---

# Tool System

Flust exposes concrete capabilities through a centralized tool registry.

The tool layer provides controlled operations such as:

* file operations
* code writing
* code analysis
* code repair
* repository analysis
* validation
* formatting
* memory operations
* calculation

The general execution model is:

```text
Agent
  │
  ▼
ToolAgent
  │
  ▼
Tool Registry
  │
  ▼
Registered Tool
  │
  ▼
Execution
  │
  ▼
Structured Result
```

Tools provide the controlled boundary between AI-generated plans and real application operations.

---

# Workspace Isolation

Development operations are restricted to the configured workspace.

```text
                  Flust
                    │
                    ▼
             Workspace Boundary
                    │
          ┌─────────┴─────────┐
          │                   │
       Allowed              Blocked
       Paths                 Paths
          │                   │
          ▼                   ▼
     Project Files      Outside Workspace
```

Workspace isolation is a core safety requirement for AI-assisted development.

The framework is designed so that an AI-generated operation does not automatically receive unrestricted access to the host filesystem.

---

# Repository Intelligence

Flust contains repository-oriented analysis infrastructure.

The repository intelligence layer is responsible for building development context from the project itself.

Conceptually:

```text
Repository
    │
    ▼
Repository Analyzer
    │
    ├── Repository Structure
    ├── Source Information
    ├── Project Knowledge
    └── Development Context
            │
            ▼
      Planner / Agents
```

The development system can use stored project information and synchronize it when workspace changes occur.

---

# Project Memory

Project memory provides persistent knowledge about the development environment.

```text
                Project Memory
                     │
        ┌────────────┼────────────┐
        │            │            │
        ▼            ▼            ▼
 Architecture     Modules      Development
 Knowledge        / Files        Context
```

The current development container initializes `ProjectMemorySync` and attaches a `WorkspaceWatcher`.

```text
Workspace Change
       │
       ▼
WorkspaceWatcher
       │
       ▼
ProjectMemorySync
       │
       ▼
Repository Analyzer
       │
       ▼
Project Memory
```

This allows project knowledge to evolve with repository changes.

---

# Conversation Memory

Conversation state is maintained separately from project-oriented knowledge.

```text
Memory
 │
 ├── Conversation Context
 │
 ├── General Memory
 │
 └── Project Memory
```

This separation prevents short-lived conversational context from being tightly coupled to persistent repository knowledge.

---

# LLM Architecture

LLM dependencies are abstracted from the agents that consume them.

The architecture allows different workloads to use different model configurations.

```text
DecisionAgent ──► Decision LLM
PlannerAgent  ──► Planner LLM
CodeAgent     ──► Code LLM
ChatAgent     ──► Chat LLM
```

This allows model specialization without changing the higher-level agent architecture.

The framework can therefore evolve toward configurations such as:

```text
Fast Model       → Routing / Classification
Reasoning Model  → Planning
Coding Model     → Code Generation
General Model    → Conversation
```

Provider-specific implementations remain behind the LLM abstraction layer.

---

# Desktop Application

The desktop interface is built with **PySide6**.

The application separates user interaction from long-running AI operations.

```text
PySide6 GUI
     │
     ▼
AI Worker
     │
     ▼
MainOrchestrator
     │
     ▼
Agents / Systems
     │
     ▼
Tools / Memory / LLM
```

The application entry point is:

```bash
python -m app.gui
```

Background execution prevents long-running AI operations from blocking the graphical interface.

---

# Testing

Flust uses **pytest** as its primary automated testing framework.

The testing architecture is divided into multiple levels:

```text
tests/
│
├── unit/
├── contracts/
├── integration/
├── e2e/
├── benchmarks/
├── evaluation/
├── fixtures/
└── fakes/
```

Pytest is configured with explicit markers for:

* `unit`
* `contract`
* `integration`
* `e2e`
* `benchmark`
* `slow`
* `network`
* `llm`

Benchmark tests are excluded from the default pytest run.

```bash
pytest
```

Detailed test functions, test responsibilities, benchmark methodology, and validation procedures are documented separately.

See:

**`TESTS-BENCHMARKS.md`**

---

# Test Coverage

The repository currently organizes tests by architectural responsibility.

## Unit

### Agents and Orchestration

```text
test_agent_container.py
test_base_agent.py
test_decision_agent.py
test_planner_agent.py
test_code_agent.py
test_main_container.py
test_main_orchestrator.py
test_chat_orchestrator.py
test_development_container.py
test_development_context.py
test_development_orchestrator.py
```

### Tools and Tool Infrastructure

```text
test_calculator.py
test_file_tool.py
test_formatter_tool.py
test_validation_tool.py
test_tool_container.py
test_tool_registry.py
test_code_writer_tool.py
test_code_writer_atomic_write.py
test_code_analyzer_tool.py
test_code_repair_tool.py
test_repository_analyzer_tool.py
```

### Memory

```text
test_memory.py
test_memory_agent.py
test_memory_container.py
test_memory_orchestrator.py
test_memory_tool.py
test_project_memory.py
test_project_memory_sync.py
test_json_store.py
test_conversation.py
```

### Repository Intelligence

```text
test_python_analyzer.py
test_repository_analysis.py
test_repository_analyzer_ownership.py
test_repository_analyzer_project_memory.py
test_repository_context_resolver.py
test_repository_indexer.py
test_repository_knowledge_persistence.py
```

### LLM and Infrastructure

```text
test_api_llm.py
test_llm.py
test_llm_planner.py
test_llm_provider.py
test_llm_router.py
test_config_manager.py
test_contracts.py
test_atomic_writer.py
test_logger.py
test_worker.py
test_workspace_watcher.py
test_fakes.py
```

## Contracts

```text
test_agent_contract.py
test_llm_provider_contract.py
test_planner_contract.py
test_tool_contract.py
```

## Integration

### Agent and Memory

```text
test_agent_memory.py
```

### Development

```text
test_development_pipeline.py
```

### Planning and Tool Execution

```text
test_planner_tool_pipeline.py
```

### Tool Agent

```text
test_tool_agent.py
```

End-to-end test infrastructure is reserved under `tests/e2e/` and can be expanded as full application workflows become part of the release validation process.

Individual test function names and their exact responsibilities belong in `TESTS-BENCHMARKS.md`, keeping this README focused on architecture rather than implementation-level test details.

---

# Benchmarks

Flust contains a dedicated benchmark system for evaluating AI-assisted development behavior.

Current benchmark scenarios include:

```text
B01 — New File Creation
B02 — Existing File Modification
B03 — Unrelated File Preservation
B04 — Multi-File Feature
B05 — Existing Code Preservation
```

The benchmark architecture evaluates the development pipeline across components such as:

```text
Decision
   ↓
Planner
   ↓
Code / Tool Execution
   ↓
Result Validation
```

Benchmark tasks are stored under:

```text
benchmarks/tasks/
```

and generated benchmark results are stored under:

```text
benchmarks/results/
```

Detailed benchmark methodology and result interpretation are documented in:

**`TESTS-BENCHMARKS.md`**

---

# Current Status

Flust is under active development.

The current implementation provides:

* modular dependency containers
* main request routing
* specialized agents
* chat and memory systems
* development orchestration
* LLM-based planning
* centralized tool execution
* repository analysis
* project memory
* workspace watching
* workspace isolation
* PySide6 desktop application
* pytest-based testing
* contract and integration testing
* development benchmarks

The architecture already contains the primary components required for a future self-development loop.

The autonomous improvement controller itself remains part of the development roadmap.

---

# Development Roadmap

The roadmap is intentionally short and focuses on architectural milestones.

```text
Phase 1 — Core Framework
        │
        ▼
Phase 2 — Tool and Workspace Infrastructure
        │
        ▼
Phase 3 — Repository Intelligence
        │
        ▼
Phase 4 — Memory Evolution
        │
        ▼
Phase 5 — Self-Development Loop
        │
        ▼
Phase 6 — Evaluation and Reliability
        │
        ▼
Phase 7 — Distribution and Runtime
```

The detailed task list is maintained in:

**`TODO.md`**

---

# Development Principles

Flust development follows several core principles.

## Safety First

AI-generated operations must remain inside explicit capability and workspace boundaries.

## Correctness Before Autonomy

A more autonomous system is not considered an improvement if it reduces reliability or makes behavior harder to validate.

## Validate Before Trust

Generated plans and generated code should be validated through deterministic application logic and automated tests wherever possible.

## Explicit Failure Propagation

Failures must remain observable.

The system should not convert failed operations into successful-looking results.

## Controlled Execution

LLMs produce decisions and plans; concrete system operations are performed through controlled application components.

## Persistent Knowledge

Repository knowledge should evolve with the project instead of being reconstructed from scratch for every task.

## Modular Architecture

Agents, orchestrators, tools, memory, and model providers should remain independently replaceable where practical.

## Test Every Architectural Change

New behavior should be accompanied by appropriate tests and validated through the relevant regression and benchmark infrastructure.

---

# Technology

| Component               | Technology                                  |
| ----------------------- | ------------------------------------------- |
| Language                | Python                                      |
| GUI                     | PySide6                                     |
| Testing                 | Pytest                                      |
| AI Models               | Pluggable LLM providers                     |
| Memory                  | Persistent JSON-based systems               |
| Repository Intelligence | Custom analysis and indexing infrastructure |
| Packaging               | PyInstaller                                 |
| Architecture            | Modular agents + orchestrators + tools      |

---

# Repository Structure

```text
Flust/
│
├── agents/
│
├── app/
│   ├── core/
│   │   ├── containers/
│   │   ├── orchestrators/
│   │   ├── memory/
│   │   ├── workspace/
│   │   └── ...
│   │
│   ├── gui/
│   └── ...
│
├── tools/
│
├── tests/
│   ├── unit/
│   ├── contracts/
│   ├── integration/
│   ├── e2e/
│   ├── benchmarks/
│   ├── evaluation/
│   ├── fixtures/
│   └── fakes/
│
├── benchmarks/
│   ├── tasks/
│   └── results/
│
├── contracts/
│
├── memory/
│
├── models/
│
├── README.md
├── TODO.md
├── TESTS-BENCHMARKS.md
└── pytest.ini
```

---

# Development Workflow

A typical development cycle is:

```text
Define Requirement
       │
       ▼
Analyze Repository
       │
       ▼
Build Development Context
       │
       ▼
Create Plan
       │
       ▼
Execute Through Tools
       │
       ▼
Run Tests
       │
       ▼
Validate Result
       │
       ▼
Update Project Knowledge
       │
       ▼
Review Change
```

For future autonomous development:

```text
Observe
   ↓
Analyze
   ↓
Plan
   ↓
Modify
   ↓
Test
   ↓
Evaluate
   ↓
Learn
   ↓
Improve
   └──────────────► Observe
```

---

# Long-Term Goal

The long-term goal of Flust is to become a modular AI system capable of operating as a persistent development environment rather than a simple conversational assistant.

The intended evolution is:

```text
AI Assistant
     │
     ▼
AI Development Agent
     │
     ▼
Repository-Aware Agent
     │
     ▼
Self-Evaluating Development System
     │
     ▼
Self-Improving Development System
```

Flust is designed to provide the software architecture required for this progression while keeping execution controlled, observable, testable, and safe.

---

# License

Flust is released under the MIT License.