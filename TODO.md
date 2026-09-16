# Flust TODO

Flust development roadmap and remaining engineering work.

The checklist reflects the current architecture and separates implemented foundations from active stabilization work and longer-term development goals.

---

# Phase 1 — Core Stabilization

## 1.1 Application Foundation

* [x] Main application container
* [x] Main orchestrator
* [x] Agent container
* [x] Memory container
* [x] Tool container
* [x] Development container
* [x] Shared dependency wiring
* [ ] Finalize application startup and shutdown lifecycle
* [ ] Complete desktop application runtime validation

## 1.2 Configuration

* [x] Configuration loading
* [x] Configuration persistence
* [x] Working-directory-independent configuration paths
* [ ] Finalize user configuration behavior
* [ ] Document supported configuration options
* [ ] Validate packaged application configuration behavior

## 1.3 Error Handling

* [x] Controlled tool errors
* [x] LLM error handling
* [x] Planner fallback behavior
* [x] Development pipeline failure propagation
* [x] File-operation validation
* [ ] Standardize user-facing error reporting
* [ ] Audit remaining unhandled application-level exceptions

---

# Phase 2 — Agent and Orchestration

## 2.1 Agent Architecture

* [x] Base Agent abstraction
* [x] Decision Agent
* [x] Memory Agent
* [x] Planner Agent
* [x] Code Agent
* [x] Tool Agent
* [x] Agent contracts
* [x] Agent memory integration
* [ ] Audit unnecessary agent responsibilities
* [ ] Simplify overlapping agent behavior where appropriate

## 2.2 Decision and Routing

* [x] Decision-based request classification
* [x] Chat routing
* [x] Memory routing
* [x] Development routing
* [x] LLM-assisted classification
* [x] Fallback routing
* [ ] Expand classification coverage
* [ ] Reduce routing ambiguity between development actions

## 2.3 Planning

* [x] Structured planner output
* [x] Tool/action validation
* [x] Single-step normalization
* [x] Invalid-plan fallback
* [x] Planner contract
* [x] Calculator fallback
* [x] Repository-analysis fallback
* [ ] Expand planner validation for complex multi-step tasks
* [ ] Improve planner reliability for architecture-aware development tasks

## 2.4 Tool Execution

* [x] Tool Registry
* [x] Tool metadata
* [x] Tool registration
* [x] Tool execution
* [x] Tool failure propagation
* [x] Multi-step ToolAgent execution
* [x] Planner → ToolAgent integration
* [ ] Continue reducing tool-specific normalization complexity
* [ ] Define stable conventions for future tools

---

# Phase 3 — Repository Intelligence

## 3.1 Repository Analysis

* [x] Repository Analyzer
* [x] Python file discovery
* [x] Python symbol extraction
* [x] Import extraction
* [x] Module-role detection
* [x] Tool discovery
* [x] Repository overview generation
* [x] Wiring checks
* [x] Issue collection
* [x] Analysis status evaluation
* [ ] Expand mixed-language repository analysis
* [ ] Improve repository-level architecture detection

## 3.2 Repository Indexing

* [x] Repository indexing
* [x] Mixed-language metadata collection
* [x] Ignored-directory handling
* [x] JSON-serializable repository metadata
* [ ] Expand language-specific analyzers
* [ ] Improve incremental indexing performance

## 3.3 Repository Context

* [x] Target-file resolution
* [x] Symbol filtering
* [x] Dependency relationships
* [x] Reverse relationships
* [x] Relationship scoring
* [x] Deterministic context ordering
* [x] Context size limits
* [x] Truncation reporting
* [x] Architecture-aware context
* [ ] Improve relevance scoring for large repositories
* [ ] Expand architecture relationship modeling

---

# Phase 4 — Project Memory

## 4.1 Project Memory

* [x] Repository memory storage
* [x] Repository overview
* [x] File information
* [x] Architecture information
* [x] Repository search
* [x] Development context
* [x] Domain-separated repository stores
* [x] Snapshot persistence
* [x] Snapshot identity
* [ ] Continue hardening persistence failure recovery
* [ ] Define long-term repository-memory schema evolution

## 4.2 Incremental Synchronization

* [x] Workspace change detection
* [x] Changed-file normalization
* [x] Incremental Python-file analysis
* [x] Non-Python file handling
* [x] Deleted-file cleanup
* [x] Relationship recomputation
* [x] Full-analysis fallback
* [x] Persistence rollback
* [ ] Improve large-repository incremental synchronization
* [ ] Measure synchronization performance

## 4.3 Workspace Watcher

* [x] Python file discovery
* [x] Non-Python file discovery
* [x] New-file detection
* [x] Modified-file detection
* [x] Deleted-file detection
* [x] Multiple-change detection
* [x] Callback forwarding
* [x] Ignored-directory handling
* [ ] Harden watcher lifecycle behavior
* [ ] Validate long-running watcher stability

---

# Phase 5 — Safe Code Modification

## 5.1 Workspace Isolation

* [x] Workspace-based file resolution
* [x] Relative-path validation
* [x] Absolute-path validation
* [x] Workspace escape prevention
* [x] Missing-workspace rejection
* [x] Directory-target rejection
* [ ] Audit every development tool for consistent workspace enforcement

## 5.2 File Operations

* [x] File creation
* [x] File reading
* [x] File writing
* [x] Backup handling
* [x] Atomic writing
* [x] Parent-directory creation
* [x] Incomplete-content protection
* [ ] Standardize file-operation result contracts

## 5.3 Code Writer

* [x] LLM-based code generation
* [x] Multi-file modification
* [x] Syntax validation
* [x] Syntax repair
* [x] Removed-class detection
* [x] Removed-method detection
* [x] Removed-function detection
* [x] Removed-import detection
* [x] Inheritance-change detection
* [x] Public-method signature protection
* [x] No-op change detection
* [x] Partial-failure reporting
* [x] Multi-file rollback
* [x] Original snapshot preservation
* [ ] Expand semantic-preservation checks
* [ ] Improve validation of complex refactoring operations

## 5.4 Code Repair and Validation

* [x] Code Repair Tool
* [x] Python syntax validation
* [x] Invalid-response handling
* [x] Repair retry behavior
* [x] Validation Tool
* [x] Read-only validation
* [ ] Expand post-change validation
* [ ] Integrate broader regression validation into development execution

---

# Phase 6 — LLM Infrastructure

## 6.1 LLM Abstraction

* [x] LLM abstraction
* [x] Provider abstraction
* [x] Local provider support
* [x] Model discovery
* [x] Current-model reporting
* [x] Model availability checks
* [x] Connection checks
* [x] Generation
* [x] Cancellation support
* [ ] Improve provider error normalization
* [ ] Expand provider compatibility

## 6.2 LLM Routing

* [x] LLM Router
* [x] Planner model detection
* [x] Chat model detection
* [x] Missing-provider handling
* [x] Unknown-model handling
* [ ] Expand model-selection policy
* [ ] Improve routing behavior for specialized development tasks

---

# Phase 7 — Testing and Quality

## 7.1 Test Infrastructure

* [x] pytest configuration
* [x] Unit test structure
* [x] Contract test structure
* [x] Integration test structure
* [x] Benchmark infrastructure
* [x] Test fakes
* [x] Test markers
* [x] Workspace test isolation
* [ ] Expand E2E coverage
* [ ] Expand evaluation infrastructure

## 7.2 Regression Coverage

* [x] Agent tests
* [x] Planner tests
* [x] Tool tests
* [x] Memory tests
* [x] LLM tests
* [x] Repository intelligence tests
* [x] Development-context tests
* [x] Project-memory tests
* [x] Workspace-watcher tests
* [x] Code-writer safety tests
* [x] Integration pipeline tests
* [ ] Maintain regression coverage for every new development capability

## 7.3 Benchmarks

* [x] B01 — New File
* [x] B02 — Modify Existing File
* [x] B03 — Preserve Unrelated Files
* [x] B04 — Multi-File Feature
* [x] B05 — Existing Code Preservation
* [x] Benchmark result persistence
* [ ] Expand benchmark task coverage
* [ ] Add more complex repository-level development tasks
* [ ] Track benchmark regressions across major architecture changes

---

# Phase 8 — Desktop Application and Distribution

## 8.1 GUI

* [x] PySide6 application
* [x] Main window
* [x] Main container integration
* [ ] Complete full GUI workflow validation
* [ ] Harden long-running application behavior
* [ ] Improve user-facing development-operation reporting

## 8.2 Packaging

* [ ] Finalize executable packaging
* [ ] Validate packaged application startup
* [ ] Validate configuration behavior in packaged builds
* [ ] Validate workspace selection in packaged builds
* [ ] Document release procedure
* [ ] Establish repeatable release validation

---

# Phase 9 — Controlled Self-Development

This phase represents the long-term direction of Flust. It should only advance as the underlying repository intelligence, planning, execution, validation, and memory systems become sufficiently reliable.

## 9.1 Observe

* [x] Repository scanning
* [x] Repository analysis
* [x] Workspace change detection
* [x] Project-memory synchronization
* [ ] Build a unified repository observation model

## 9.2 Analyze

* [x] Code analysis
* [x] Repository analysis
* [x] Architecture-aware context
* [x] Issue detection
* [ ] Improve architectural issue detection
* [ ] Improve cross-file reasoning

## 9.3 Plan

* [x] Structured development plans
* [x] Tool-aware planning
* [x] Planner validation
* [ ] Improve plans for complex changes
* [ ] Add stronger change-scope constraints

## 9.4 Controlled Change

* [x] Workspace isolation
* [x] Atomic writes
* [x] Code generation
* [x] Code repair
* [x] Multi-file changes
* [x] Rollback
* [ ] Strengthen semantic preservation
* [ ] Add stricter change authorization boundaries

## 9.5 Validate

* [x] Syntax validation
* [x] Code analysis
* [x] Development tests
* [x] Regression tests
* [x] Benchmark evaluation
* [ ] Automatically select appropriate validation for a generated change
* [ ] Require successful validation before accepting autonomous changes

## 9.6 Learn

* [x] Project-memory synchronization
* [x] Repository snapshot persistence
* [x] Incremental knowledge updates
* [ ] Store structured development outcomes
* [ ] Record successful and failed change strategies
* [ ] Build historical development context

---

# Phase 10 — Flust + Mentacore Integration

Phase 10 represents the long-term architectural convergence of Flust and Mentacore.

Flust will eventually operate as the **AI Development System** on top of a Python-based AI Kernel, which itself runs on a native Python Runtime provided by Mentacore.

Target architecture:

```text
┌─────────────────────────────────────┐
│               Flust                 │
│       AI Development System         │
├─────────────────────────────────────┤
│             AI Kernel               │
│          Python-based               │
├─────────────────────────────────────┤
│          Python Runtime             │
├─────────────────────────────────────┤
│         Mentacore Kernel            │
│               Rust                  │
└─────────────────────────────────────┘
```

## 10.1 Mentacore Foundation

* [ ] Complete Mentacore bootloader
* [ ] Complete Mentacore kernel foundation
* [ ] Establish stable kernel interfaces
* [ ] Establish process and thread infrastructure
* [ ] Establish memory management
* [ ] Establish userspace execution
* [ ] Establish syscall infrastructure
* [ ] Establish stable kernel/userspace boundary

## 10.2 Python Runtime

* [ ] Design native Python runtime integration
* [ ] Define Python runtime ↔ Mentacore kernel interface
* [ ] Implement required runtime services
* [ ] Provide memory management integration
* [ ] Provide filesystem integration
* [ ] Provide process/thread integration
* [ ] Provide timing and synchronization primitives
* [ ] Validate Python execution directly on Mentacore

## 10.3 AI Kernel

* [ ] Define Python-based AI Kernel architecture
* [ ] Implement AI Kernel core runtime
* [ ] Integrate model execution infrastructure
* [ ] Integrate memory and knowledge services
* [ ] Integrate tool execution
* [ ] Integrate development-system interfaces
* [ ] Define AI Kernel ↔ Python Runtime boundary
* [ ] Validate AI Kernel execution on Mentacore

## 10.4 Flust Runtime Integration

* [ ] Define Flust ↔ AI Kernel interface
* [ ] Port required Flust runtime components
* [ ] Integrate Flust agents
* [ ] Integrate planner and orchestration systems
* [ ] Integrate Tool Registry and ToolAgent
* [ ] Integrate project memory
* [ ] Integrate repository intelligence
* [ ] Integrate controlled code modification
* [ ] Validate Flust execution on the native platform

## 10.5 Native AI Development Environment

* [ ] Provide native workspace access
* [ ] Provide native repository observation
* [ ] Provide native project-memory persistence
* [ ] Provide native development tools
* [ ] Provide native validation and testing
* [ ] Provide controlled code modification
* [ ] Provide rollback and recovery mechanisms
* [ ] Remove dependencies that are only required by the external desktop environment

## 10.6 Integrated Development Loop

The final system should connect the existing Flust development loop to the native Mentacore platform:

```text
┌──────────────────────┐
│      Flust           │
│ AI Development       │
│      System          │
└──────────┬───────────┘
           ↓
┌──────────────────────┐
│      AI Kernel       │
│      Python          │
└──────────┬───────────┘
           ↓
┌──────────────────────┐
│   Python Runtime     │
└──────────┬───────────┘
           ↓
┌──────────────────────┐
│  Mentacore Kernel    │
│       Rust           │
└──────────────────────┘
```

The development cycle remains:

```text
Observe
   ↓
Analyze
   ↓
Detect
   ↓
Plan
   ↓
Change
   ↓
Validate
   ↓
Learn
   ↓
Next Cycle
```

The difference is that the complete stack will eventually execute directly on the Mentacore platform.

## 10.7 Final Architecture

The long-term target is a vertically integrated system:

```text
Flust
  │
  │ AI Development System
  ↓
AI Kernel
  │
  │ Python
  ↓
Python Runtime
  │
  │ Native Runtime Services
  ↓
Mentacore Kernel
  │
  │ Rust
  ↓
Hardware
```

Each layer should have a clearly defined responsibility and interface.

The final architecture should avoid treating Flust and Mentacore as unrelated projects. They become layers of the same long-term system:

```text
Mentacore
    └── Kernel / OS Foundation
          └── Python Runtime
                └── AI Kernel
                      └── Flust
                            └── AI Development System
```

## 10.8 Integration Validation

* [ ] Define cross-project integration tests
* [ ] Define Python Runtime regression tests
* [ ] Define AI Kernel regression tests
* [ ] Define Flust-on-Mentacore integration tests
* [ ] Define complete-stack boot validation
* [ ] Define complete-stack development workflow tests
* [ ] Validate failure recovery across layer boundaries
* [ ] Validate workspace and execution isolation across the complete stack

## 10.9 Final Goal

The long-term goal is a single vertically integrated AI-native computing environment:

```text
Hardware
   ↓
Mentacore Kernel
   ↓
Python Runtime
   ↓
Python AI Kernel
   ↓
Flust
   ↓
AI Development System
```

Flust provides the development intelligence.

The AI Kernel provides the AI-native execution layer.

The Python Runtime provides the runtime environment.

Mentacore provides the operating-system and hardware abstraction foundation.

---

# Development Principles

1. **Tests accompany capabilities.**
2. **Architecture must reflect actual implementation.**
3. **Development operations remain workspace-bound.**
4. **Generated changes must be validated before acceptance.**
5. **Existing functionality must be preserved unless intentionally changed.**
6. **Repository knowledge must remain synchronized with repository state.**
7. **Failures must be explicit and recoverable.**
8. **Autonomous development must remain controlled and reversible.**
9. **New abstractions should solve concrete architectural problems.**
10. **Long-term self-development must be built incrementally on verified foundations.**

---

# Priority Order

Current development priority:

```text
1. Core stabilization
        ↓
2. Agent / planner / tool reliability
        ↓
3. Repository intelligence
        ↓
4. Project memory
        ↓
5. Safe code modification
        ↓
6. LLM infrastructure
        ↓
7. Testing / benchmarks
        ↓
8. Desktop / distribution
        ↓
9. Controlled self-development
        ↓
10. Long-term autonomous development architecture
```

A future phase should not be considered complete merely because its implementation exists. Its relevant tests, integration behavior, failure handling, and documentation should also be validated.