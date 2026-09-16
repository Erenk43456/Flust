# Flust — Tests & Benchmarks

Flust uses a layered testing architecture covering unit behavior, architectural contracts, multi-component integration, benchmark tasks, and long-term validation of development workflows.

The test system is designed to verify not only individual components, but also the boundaries between agents, planners, tools, memory, repository intelligence, and controlled development operations.

---

## Testing Stack

Flust currently uses:

* **pytest** — primary test framework
* **Python 3.12+** — supported runtime
* **Unit tests** — isolated component behavior
* **Contract tests** — architecture and interface guarantees
* **Integration tests** — multi-component workflows
* **Benchmark tests** — controlled development-task evaluation
* **Fakes and fixtures** — deterministic test dependencies
* **Workspace isolation** — protection against unintended filesystem access

The project configures pytest through `pytest.ini`:

```ini
[pytest]
pythonpath = .
testpaths = tests

addopts =
    -ra
    --strict-markers
    -m "not benchmark"

markers =
    unit: fast isolated unit tests
    contract: architecture and interface contract tests
    integration: multi-component integration tests
    e2e: end-to-end application workflow tests
    benchmark: evaluation and benchmark tests
    slow: slow-running tests
    network: tests requiring network access
    llm: tests requiring a real LLM
```

The default test command therefore excludes benchmark tests:

```powershell
pytest
```

Benchmark tests can be executed explicitly when benchmark evaluation is required.

---

# Test Architecture

Flust organizes tests by responsibility:

```text
tests/
├── unit/
├── contracts/
├── integration/
├── e2e/
├── evaluation/
├── benchmarks/
└── conftest.py
```

### Unit Tests

Unit tests verify isolated behavior of:

* Agents
* Containers
* Orchestrators
* LLM infrastructure
* Memory
* Tools
* Repository intelligence
* Development context
* Workspace watching
* Persistence
* Workers
* Logging
* Configuration

### Contract Tests

Contract tests verify that interchangeable components continue to satisfy their required interfaces.

Current contract areas:

* Agent contract
* LLM provider contract
* Planner contract
* Tool contract

### Integration Tests

Integration tests verify interactions between multiple components.

Current integration areas include:

* Agent ↔ Memory
* Development pipeline
* Planner ↔ ToolAgent
* ToolAgent ↔ registered tools
* Code-agent delegation

### End-to-End Tests

`tests/e2e/` is reserved for complete application workflows.

The current repository contains the E2E test location, but the supplied test inventory contains no E2E `test_*` functions.

### Evaluation Tests

`tests/evaluation/` is reserved for evaluation-oriented infrastructure.

The supplied test inventory contains no evaluation `test_*` functions.

---

# Unit Tests

The unit suite provides the majority of Flust's behavioral coverage.

## Agent Container

### `tests/unit/test_agent_container.py`

Tests construction and dependency wiring of agent components.

* `test_agent_container_creates_agents` — verifies agent creation.
* `test_agent_container_creates_all_agents` — verifies that all expected agents are created.

---

## API LLM

### `tests/unit/test_api_llm.py`

Tests API-backed LLM behavior, cancellation, retries, model checks, and connection handling.

* `test_api_llm_generate_success` — verifies successful generation.
* `test_api_llm_generate_retries_timeout` — verifies retry behavior after timeout.
* `test_api_llm_generate_cancellation_before_request` — verifies cancellation before a request starts.
* `test_api_llm_generate_cancellation_after_retryable_error` — verifies cancellation after a retryable failure.
* `test_api_llm_has_model_checks_configured_model` — verifies configured model detection.
* `test_api_llm_check_connection_uses_post` — verifies connection checks use POST.
* `test_api_llm_check_connection_returns_false_on_request_error` — verifies request errors produce a failed connection result.

---

## Atomic Writer

### `tests/unit/test_atomic_writer.py`

Tests safe and atomic workspace file writes.

* `test_atomic_writer_writes_content` — verifies content is written.
* `test_atomic_writer_preserves_file_on_failure` — verifies the original file survives a failed write.
* `test_atomic_writer_blocks_path_outside_workspace` — verifies workspace boundary enforcement.
* `test_atomic_writer_creates_target_content_exactly` — verifies exact output content.

---

## Base Agent

### `tests/unit/test_base_agent.py`

Tests core agent initialization, memory access, and execution.

* `test_agent_initializes` — verifies basic agent initialization.
* `test_agent_remember` — verifies memory storage through the agent.
* `test_agent_recall` — verifies memory retrieval.
* `test_agent_without_memory` — verifies operation without a memory dependency.
* `test_agent_run` — verifies the base run behavior.

---

## Calculator

### `tests/unit/test_calculator.py`

Tests calculator operations, validation, conversion, and metadata.

* `test_calculator_add` — verifies addition.
* `test_calculator_subtract` — verifies subtraction.
* `test_calculator_multiply` — verifies multiplication.
* `test_calculator_divide` — verifies division.
* `test_calculator_divide_by_zero` — verifies division-by-zero handling.
* `test_calculator_requires_two_numbers` — verifies operand-count validation.
* `test_calculator_requires_numbers` — verifies numeric operand validation.
* `test_calculator_unsupported_operation` — verifies unsupported-operation handling.
* `test_calculator_converts_numbers_to_float` — verifies numeric conversion.
* `test_calculator_handles_invalid_number` — verifies invalid-number handling.
* `test_calculator_add_method` — verifies the direct add method.
* `test_calculator_subtract_method` — verifies the direct subtract method.
* `test_calculator_multiply_method` — verifies the direct multiply method.
* `test_calculator_divide_method` — verifies the direct divide method.
* `test_calculator_metadata` — verifies calculator metadata.

---

## Chat

### `tests/unit/test_chat_manager.py`

Tests chat persistence and thread safety.

* `test_chat_manager_load_failure_preserves_existing_chats` — verifies existing chats remain available after load failure.
* `test_chat_manager_create_chat_is_thread_safe` — verifies thread-safe chat creation.

### `tests/unit/test_chat_orchestrator.py`

Tests chat orchestration.

* `test_chat_orchestrator_runs_chat` — verifies chat execution.
* `test_chat_orchestrator_sets_conversation` — verifies conversation assignment.

---

# Code Development Tests

## Code Agent

### `tests/unit/test_code_agent.py`

Tests task extraction, development context construction, repository analysis, LLM failures, JSON repair, and code-writer integration.

* `test_code_agent_execute_uses_input_as_task` — verifies task extraction from input.
* `test_code_agent_execute_uses_message_when_input_missing` — verifies message fallback.
* `test_code_agent_execute_uses_task_when_input_and_message_missing` — verifies task fallback.
* `test_code_agent_execute_accepts_plain_string` — verifies string input handling.
* `test_code_agent_run_successfully_calls_code_writer` — verifies code-writer execution.
* `test_code_agent_uses_development_context_builder` — verifies context-builder usage.
* `test_code_agent_builds_development_context_when_not_provided` — verifies automatic context construction.
* `test_code_agent_uses_explicit_development_context` — verifies explicit context usage.
* `test_code_agent_context_builder_failure_uses_legacy_context` — verifies legacy fallback after context-builder failure.
* `test_code_agent_repository_analyzer_runs_when_fallback_enabled` — verifies repository-analysis fallback.
* `test_code_agent_skips_repository_analyzer_when_analysis_exists` — verifies reuse of existing analysis.
* `test_code_agent_handles_missing_repository_analyzer` — verifies missing analyzer handling.
* `test_code_agent_returns_failure_when_llm_raises` — verifies LLM exception handling.
* `test_code_agent_returns_failure_when_llm_returns_dict` — verifies invalid dictionary responses.
* `test_code_agent_returns_failure_when_llm_returns_invalid_type` — verifies invalid response types.
* `test_code_agent_returns_failure_when_json_repair_fails` — verifies JSON repair failure handling.
* `test_code_agent_uses_repaired_json_plan` — verifies repaired plans are used.
* `test_code_agent_returns_failure_when_code_writer_is_missing` — verifies missing writer handling.
* `test_code_agent_returns_failure_when_code_writer_raises` — verifies writer exception handling.
* `test_code_agent_returns_writer_failure_result` — verifies propagation of writer failure results.
* `test_code_agent_returns_files_written_from_code_writer` — verifies written-file reporting.
* `test_code_agent_stores_repository_analysis_in_development_context` — verifies repository analysis persistence in context.
* `test_code_agent_repository_context_handles_missing_architecture` — verifies missing architecture handling.

---

## Code Analyzer Tool

### `tests/unit/test_code_analyzer_tool.py`

Tests code extraction, workspace file access, truncation, LLM response handling, JSON parsing, and analysis planning.

* `test_code_analyzer_execute_analyzes_plain_string` — analyzes plain string input.
* `test_code_analyzer_execute_uses_code_from_plan` — prioritizes planned code.
* `test_code_analyzer_execute_uses_context_when_code_missing` — uses context as fallback.
* `test_code_analyzer_execute_uses_content_when_code_and_context_missing` — uses content as final fallback.
* `test_code_analyzer_returns_file_not_found` — handles missing files.
* `test_code_analyzer_reads_file_from_workspace` — reads workspace files.
* `test_code_analyzer_empty_code_returns_failure` — rejects empty code.
* `test_code_analyzer_whitespace_only_code_is_not_empty` — preserves whitespace-only input as non-empty.
* `test_code_analyzer_truncates_large_code` — truncates oversized code.
* `test_code_analyzer_handles_empty_llm_response` — handles empty LLM responses.
* `test_code_analyzer_handles_llm_error_string` — handles string LLM errors.
* `test_code_analyzer_handles_llm_error_dict` — handles dictionary LLM errors.
* `test_code_analyzer_handles_llm_exception` — handles LLM exceptions.
* `test_code_analyzer_clean_json_parses_plain_json` — parses plain JSON.
* `test_code_analyzer_clean_json_removes_json_markdown` — removes JSON markdown fences.
* `test_code_analyzer_clean_json_removes_plain_code_fence` — removes generic code fences.
* `test_code_analyzer_clean_json_extracts_json_from_extra_text` — extracts JSON embedded in surrounding text.
* `test_code_analyzer_clean_json_returns_parse_error_for_invalid_json` — reports invalid JSON.
* `test_code_analyzer_clean_json_returns_empty_object_for_empty_text` — handles empty text.
* `test_code_analyzer_clean_json_accepts_dict` — accepts dictionary input.
* `test_code_analyzer_clean_json_handles_json_with_nested_objects` — handles nested JSON objects.
* `test_code_analyzer_analyze_code_returns_parsed_markdown_json` — parses analysis output.
* `test_code_analyzer_analyze_code_preserves_unparseable_response` — preserves unparseable output.
* `test_code_analyzer_plan_prefers_code_over_context_and_content` — verifies input precedence.
* `test_code_analyzer_plan_prefers_context_over_content` — verifies context precedence.
* `test_code_analyzer_plan_uses_content_as_last_fallback` — verifies content fallback.
* `test_code_analyzer_empty_plan_returns_empty_code_error` — handles empty plans.
* `test_code_analyzer_filename_without_workspace_falls_back_to_empty_code` — handles filename without workspace.
* `test_code_analyzer_accepts_numeric_plan` — handles numeric plans.
* `test_code_analyzer_accepts_list_plan` — handles list plans.
* `test_code_analyzer_exact_max_code_length_is_not_truncated` — preserves code at the exact limit.
* `test_code_analyzer_over_max_code_length_is_truncated` — truncates code beyond the limit.
* `test_code_analyzer_rejects_file_outside_workspace` — blocks workspace escape.
* `test_analyze_code_reports_truncation` — reports truncation.
* `test_code_analyzer_clean_json_uses_first_valid_json_object` — selects the first valid JSON object.

---

## Code Repair Tool

### `tests/unit/test_code_repair_tool.py`

Tests LLM-based code repair, validation, fallback behavior, and safe file modification.

* `test_code_repair_repairs_valid_python` — verifies repair of valid Python input.
* `test_code_repair_rejects_non_string_code` — rejects non-string code.
* `test_code_repair_rejects_empty_code` — rejects empty code.
* `test_code_repair_handles_llm_exception` — handles LLM exceptions.
* `test_code_repair_handles_llm_dict_response` — handles dictionary LLM responses.
* `test_code_repair_handles_invalid_llm_response_type` — handles invalid response types.
* `test_code_repair_handles_empty_llm_response` — handles empty responses.
* `test_code_repair_rejects_invalid_repaired_python` — rejects syntactically invalid repaired code.
* `test_code_repair_clean_code_removes_markdown_fence` — removes markdown fences.
* `test_code_repair_clean_code_adds_trailing_newline` — normalizes trailing newline.
* `test_code_repair_clean_code_handles_empty_value` — handles empty cleaned values.
* `test_code_repair_validate_python_accepts_valid_code` — accepts valid Python.
* `test_code_repair_validate_python_reports_syntax_error` — reports syntax errors.
* `test_code_repair_execute_repairs_file_successfully` — repairs a workspace file.
* `test_code_repair_execute_returns_file_error_when_file_missing` — handles missing files.
* `test_code_repair_execute_does_not_write_when_repair_fails` — prevents writes after failed repair.
* `test_code_repair_execute_uses_supplied_code_before_file` — prioritizes supplied code.
* `test_code_repair_execute_uses_input_as_supplied_code` — extracts supplied code from input.
* `test_code_repair_execute_returns_repair_result_for_plain_string` — handles plain string execution.
* `test_code_repair_execute_uses_context_as_code_fallback` — uses context as fallback.
* `test_code_repair_execute_supplied_code_adds_filename_to_success` — preserves filename metadata.
* `test_code_repair_execute_without_workspace_does_not_read_file` — avoids filesystem access without workspace.
* `test_code_repair_retries_when_first_generated_code_is_invalid` — retries invalid generated code.

---

## Code Writer

### `tests/unit/test_code_writer_atomic_write.py`

Tests the relationship between the code writer and atomic file-writing infrastructure.

* `test_code_writer_uses_atomic_writer` — verifies atomic-writer usage.
* `test_code_writer_does_not_directly_write_file` — verifies direct writes are avoided.

### `tests/unit/test_code_writer_tool.py`

Tests safe code generation, file validation, repair, rollback, semantic preservation, and workspace isolation.

* `test_code_writer_metadata` — verifies tool metadata.
* `test_code_writer_execute_rejects_non_dict` — rejects invalid plan types.
* `test_code_writer_execute_rejects_invalid_files_list` — validates file lists.
* `test_code_writer_execute_returns_failure_for_empty_files` — rejects empty file plans.
* `test_code_writer_execute_ignores_invalid_file_entries` — ignores malformed entries.
* `test_code_writer_execute_stores_development_context` — stores development context.
* `test_code_writer_modify_file_requires_workspace` — requires workspace for modification.
* `test_code_writer_rejects_path_outside_workspace` — blocks workspace escape.
* `test_code_writer_returns_file_not_found` — handles missing files.
* `test_code_writer_rejects_directory_target` — rejects directory targets.
* `test_code_writer_returns_llm_exception` — handles LLM exceptions.
* `test_code_writer_rejects_invalid_llm_response_type` — rejects invalid LLM response types.
* `test_code_writer_rejects_non_string_llm_response` — rejects non-string generated code.
* `test_code_writer_rejects_empty_generated_code` — rejects empty generated code.
* `test_code_writer_successfully_updates_file` — verifies successful file modification.
* `test_code_writer_execute_successfully_updates_multiple_files` — verifies multi-file changes.
* `test_code_writer_repairs_invalid_python` — verifies syntax repair.
* `test_code_writer_returns_failure_when_syntax_repair_fails` — handles failed repair.
* `test_code_writer_syntax_repair_requires_registry` — verifies repair-tool dependency.
* `test_code_writer_rejects_removed_class` — detects class removal.
* `test_code_writer_repairs_removed_class` — repairs an unintended class removal.
* `test_code_writer_detects_removed_method` — detects method removal.
* `test_code_writer_detects_inheritance_change` — detects inheritance changes.
* `test_code_writer_detects_removed_top_level_function` — detects removed functions.
* `test_code_writer_repair_tool_exception_is_handled` — handles repair-tool exceptions.
* `test_code_writer_repair_tool_missing_returns_failure` — handles missing repair tools.
* `test_code_writer_repair_returns_invalid_code` — rejects invalid repair output.
* `test_code_writer_execute_reports_partial_failure` — reports partial multi-file failure.
* `test_code_writer_returns_written_files` — reports successful writes.
* `test_code_writer_excludes_failed_files_from_files_written` — excludes failed files from success reporting.
* `test_code_writer_detects_removed_import` — detects removed imports.
* `test_code_writer_rejects_file_with_empty_changes` — rejects empty changes.
* `test_code_writer_rejects_unchanged_generated_code` — rejects unchanged output.
* `test_code_writer_rejects_semantically_unchanged_generated_code` — detects semantic no-op changes.
* `test_code_writer_rejects_public_method_signature_change` — protects public method signatures.
* `test_execute_rejects_missing_workspace` — rejects execution without workspace.
* `test_code_writer_verification_accepts_safe_expression` — accepts safe verification expressions.
* `test_code_writer_verification_rejects_unsafe_expression` — rejects unsafe expressions.
* `test_code_writer_verification_rejects_attribute_access` — blocks attribute access.
* `test_code_writer_verification_supports_not_in` — supports `not in` expressions.
* `test_code_writer_execute_rejects_file_outside_workspace` — blocks external file targets.
* `test_execute_rolls_back_previous_file_changes_on_failure` — rolls back previous changes after failure.
* `test_execute_preserves_original_snapshot_for_duplicate_paths` — preserves the original state for duplicate paths.

---

# Configuration and Contracts

## Configuration

### `tests/unit/test_config_manager.py`

* `test_config_manager_path_is_independent_of_working_directory` — verifies configuration path independence.
* `test_config_manager_loads_and_saves_user_config` — verifies configuration persistence.

---

## Internal Contracts

### `tests/unit/test_contracts.py`

* `test_decision_contract_defaults` — verifies decision contract defaults.
* `test_contract_agent_to_decision_contract_from_dict` — converts dictionary input into a decision contract.
* `test_contract_agent_to_decision_contract_invalid_system_fallback` — verifies invalid-system fallback.
* `test_planner_step_defaults` — verifies planner-step defaults.
* `test_contract_agent_to_planner_contract_from_dict` — converts dictionary input into a planner contract.
* `test_contract_agent_to_planner_contract_single_step` — handles a single planner step.
* `test_contract_agent_to_tool_step_contract` — converts tool-step data.
* `test_contract_agent_to_tool_result_from_str` — converts string tool results.
* `test_contract_agent_to_tool_result_from_dict` — converts dictionary tool results.
* `test_contract_agent_to_tool_result_from_exception` — converts exceptions into tool results.
* `test_contract_agent_to_memory_contract` — verifies memory contract conversion.

---

## Conversation

### `tests/unit/test_conversation.py`

* `test_conversation_save_propagates_persistence_error` — verifies persistence errors are propagated.

---

# Decision and Development Orchestration

## Decision Agent

### `tests/unit/test_decision_agent.py`

Tests request classification and routing.

* `test_decision_memory_get` — routes memory retrieval decisions.
* `test_decision_memory_save` — routes memory-save decisions.
* `test_decision_calculation` — identifies calculation requests.
* `test_decision_calculation_turkish` — handles Turkish calculation requests.
* `test_decision_python_file` — identifies Python-file tasks.
* `test_decision_bug_fix` — identifies bug-fix tasks.
* `test_decision_file_operation` — identifies file operations.
* `test_decision_uses_llm_for_chat` — uses the LLM for chat classification.
* `test_decision_parses_json_response` — parses structured LLM decisions.
* `test_decision_invalid_system_falls_back_to_development` — falls back for invalid systems.
* `test_decision_llm_error_falls_back_to_chat` — falls back to chat after LLM failure.
* `test_decision_repository_analysis` — identifies repository-analysis requests.
* `test_decision_file_inspection` — identifies file-inspection requests.
* `test_decision_bug_fix_routes_to_code` — routes bug fixes to code execution.
* `test_decision_refactor_routes_to_improve` — routes refactoring to improvement.
* `test_process_extracts_first_json_object_from_extra_response_content` — extracts the first JSON object from extra response content.

---

## Development Container

### `tests/unit/test_development_container.py`

* `test_development_container_creates_components` — verifies development subsystem construction.
* `test_development_container_uses_shared_dependencies` — verifies shared dependency wiring.
* `test_workspace_changes_trigger_project_memory_sync` — verifies watcher-to-memory synchronization.
* `test_development_container_close_stops_watcher` — verifies watcher shutdown.
* `test_development_container_context_manager_closes_watcher` — verifies context-manager cleanup.

---

## Development Context

### `tests/unit/test_development_context.py`

Tests targeted repository context, relationships, strategy selection, memory fallback, and prompt serialization.

* `test_extract_target_files_normalizes_and_deduplicates` — normalizes and deduplicates target files.
* `test_development_context_returns_targeted_context` — returns targeted context.
* `test_development_context_forwards_targets_symbols_and_max_files` — forwards context constraints.
* `test_build_uses_targeted_context_when_targets_are_explicit` — uses targeted context for explicit targets.
* `test_build_without_explicit_targets_keeps_existing_context_flow` — preserves the default context flow.
* `test_build_returns_targeted_context_error_when_resolver_fails` — handles resolver failure.
* `test_development_context_minimal_memory_constructor_remains_compatible` — preserves constructor compatibility.
* `test_development_context_handles_resolver_failure_safely` — safely handles resolver errors.
* `test_development_context_targeted_context_does_not_use_filesystem_or_analyzer` — verifies targeted context avoids direct filesystem/analyzer access.
* `test_build_collects_targets_related_files_relationships_and_strategy` — builds complete development context.
* `test_find_related_files_scores_same_package_and_dependency_relationships` — scores related files.
* `test_architecture_relationship_adds_architecture_score` — includes architecture relationships in scoring.
* `test_determine_strategy_selects_expected_strategy_types` — selects expected strategies.
* `test_multiple_targets_use_multi_target_fix_strategy` — selects multi-target strategy.
* `test_repository_analysis_fallback_is_enabled_when_memory_is_empty` — enables analysis fallback.
* `test_memory_errors_fall_back_to_empty_context` — falls back after memory errors.
* `test_to_prompt_returns_valid_json` — verifies prompt serialization.
* `test_same_layer_or_directory_alone_does_not_make_file_related` — avoids false relationships.
* `test_direct_dependency_reference_is_meaningful_relationship` — identifies direct dependencies.
* `test_target_memory_reference_makes_related_file_meaningful` — uses memory references as relationships.
* `test_architecture_layer_is_supporting_evidence_not_meaningful_relationship` — treats architecture layer as supporting evidence.
* `test_feature_with_architecture_related_file_uses_architecture_aware_strategy` — selects architecture-aware strategy.

---

## Development Orchestrator

### `tests/unit/test_development_orchestrator.py`

* `test_development_analyze` — executes analysis action.
* `test_development_improve` — executes improvement action.
* `test_development_executes_code_task` — executes code tasks.
* `test_development_planner_failure` — handles planner failure.
* `test_development_empty_plan` — handles empty plans.
* `test_development_unknown_tool` — handles unknown tools.
* `test_development_tool_execution` — executes planned tools.
* `test_development_delegates_plan_execution_to_tool_agent` — delegates plan execution to ToolAgent.
* `test_development_string_tool_error_marks_overall_failure` — treats string tool errors as failures.
* `test_development_success_string_does_not_mark_overall_failure` — preserves successful string results.
* `test_development_rejects_unknown_action` — rejects unknown actions.

---

# Test Doubles

## Fakes

### `tests/unit/test_fakes.py`

Tests deterministic fake implementations used throughout the test suite.

* `test_fake_llm_generate` — verifies fake LLM generation.
* `test_fake_llm_tracks_prompts` — verifies prompt tracking.
* `test_fake_memory_stores_data` — verifies fake memory storage.
* `test_fake_memory_clear` — verifies fake memory clearing.
* `test_fake_tool_tracks_execution` — verifies fake tool execution tracking.
* `test_fake_llm_error_injection` — verifies injected LLM errors.
* `test_fake_llm_sequential_responses` — verifies sequential fake responses.
* `test_fake_registry_get_tracks_calls` — verifies registry call tracking.
* `test_fake_project_memory_read_side` — verifies project-memory reads.
* `test_fake_project_memory_write_side` — verifies project-memory writes.
* `test_fake_project_memory_error_injection` — verifies project-memory error injection.
* `test_fake_repository_analyzer_returns_analysis` — verifies fake repository analysis.
* `test_fake_code_agent_run_tracks_calls` — verifies code-agent call tracking.
* `test_fake_development_context_build` — verifies fake development-context building.

---

# File and Formatting Tools

## File Tool

### `tests/unit/test_file_tool.py`

Tests workspace file operations and filesystem safety.

* `test_file_tool_metadata` — verifies tool metadata.
* `test_file_tool_uses_workspace_as_base_path` — verifies workspace base-path usage.
* `test_file_tool_execute_rejects_non_dict` — rejects invalid input.
* `test_file_tool_execute_defaults_to_read` — verifies read as the default action.
* `test_file_tool_rejects_unknown_action` — rejects unsupported actions.
* `test_file_tool_create_file` — creates files.
* `test_file_tool_create_file_with_nested_directory` — creates files in nested directories.
* `test_file_tool_create_file_missing_filename` — handles missing filenames.
* `test_file_tool_read_file` — reads files.
* `test_file_tool_read_missing_file` — handles missing files.
* `test_file_tool_write_existing_file_creates_backup` — creates backups for existing files.
* `test_file_tool_blocks_incomplete_generated_content` — blocks incomplete generated content.
* `test_file_tool_rejects_path_outside_workspace` — blocks external paths.
* `test_file_tool_rejects_absolute_path_outside_workspace` — blocks external absolute paths.
* `test_file_tool_get_path_returns_none_for_missing_filename` — handles missing filename resolution.
* `test_file_tool_get_path_returns_resolved_workspace_path` — resolves workspace paths.
* `test_file_tool_write_missing_filename` — handles missing write targets.
* `test_file_tool_write_blocks_none_content` — rejects `None` content.
* `test_file_tool_write_allows_empty_string_content` — allows empty string content.
* `test_file_tool_create_defaults_to_empty_content` — verifies empty-content defaults.
* `test_file_tool_create_rejects_existing_file` — prevents accidental overwrite during creation.
* `test_file_tool_write_blocks_existing_content_placeholder` — blocks placeholder content.
* `test_file_tool_atomic_write_creates_parent_directory` — verifies parent directory creation.
* `test_file_tool_read_rejects_workspace_escape` — prevents read escapes.
* `test_file_tool_write_rejects_workspace_escape` — prevents write escapes.
* `test_file_tool_create_rejects_workspace_escape` — prevents create escapes.
* `test_file_tool_rejects_missing_workspace` — requires a workspace.

---

## Formatter Tool

### `tests/unit/test_formatter_tool.py`

Tests Python formatting and workspace-safe file formatting.

* `test_formatter_format_code_formats_valid_python` — formats valid Python.
* `test_formatter_format_code_accepts_code_mapping` — accepts code mappings.
* `test_formatter_format_code_accepts_input_mapping` — accepts input mappings.
* `test_formatter_format_code_accepts_context_mapping` — accepts context mappings.
* `test_formatter_format_code_rejects_empty_code` — rejects empty code.
* `test_formatter_format_code_rejects_invalid_python` — rejects invalid Python.
* `test_formatter_format_code_rejects_unsupported_input_type` — rejects unsupported inputs.
* `test_formatter_format_file_formats_python_file_without_writing` — formats a file without modifying it.
* `test_formatter_format_file_writes_formatted_code` — writes formatted code.
* `test_formatter_format_file_returns_not_found_for_missing_file` — handles missing files.
* `test_formatter_format_file_rejects_non_python_file` — rejects non-Python files.
* `test_formatter_execute_formats_plain_code` — formats plain code.
* `test_formatter_execute_returns_error_message_for_invalid_code` — reports formatting errors.
* `test_formatter_execute_formats_file_inside_workspace` — formats workspace files.
* `test_formatter_execute_denies_path_outside_workspace` — blocks external paths.
* `test_formatter_execute_without_filename_uses_code` — uses code when no filename is provided.

---

# Persistence and Memory

## JSON Store

### `tests/unit/test_json_store.py`

* `test_load_returns_default_when_file_does_not_exist` — verifies default loading.
* `test_save_and_load_round_trip` — verifies persistence round-trip.
* `test_save_creates_parent_directories` — creates missing parent directories.
* `test_load_raises_controlled_error_for_invalid_json` — handles invalid JSON.
* `test_save_is_valid_json` — verifies valid JSON output.

---

## LLM

### `tests/unit/test_llm.py`

* `test_llm_initializes_from_config` — initializes LLM configuration.
* `test_llm_normalizes_endpoint` — normalizes endpoints.
* `test_llm_accepts_generate_endpoint` — accepts generation endpoints.
* `test_llm_current_model` — exposes the current model.
* `test_llm_has_model` — verifies model availability.
* `test_llm_missing_model` — handles missing models.
* `test_llm_connection_success` — handles successful connections.
* `test_llm_connection_failure` — handles failed connections.
* `test_llm_connection_exception` — handles connection exceptions.
* `test_llm_get_models` — retrieves models.
* `test_llm_get_models_failure` — handles model-list failures.
* `test_llm_generate_success` — verifies generation.
* `test_llm_generate_empty_response` — handles empty responses.
* `test_llm_generate_connection_failure` — handles generation connection failures.
* `test_llm_generate_does_not_run_separate_connection_check` — verifies generation does not perform a separate connection check.
* `test_llm_generate_respects_cancel_event` — verifies cancellation support.

---

## LLM Planner

### `tests/unit/test_llm_planner.py`

Tests structured LLM planning and plan validation.

* `test_clean_json_returns_json_object` — parses JSON objects.
* `test_clean_json_removes_markdown` — removes markdown formatting.
* `test_clean_json_extracts_json_from_text` — extracts JSON from text.
* `test_clean_json_invalid_input_returns_empty_object` — handles invalid input.
* `test_format_tools_returns_tool_information` — formats tool metadata.
* `test_format_tools_without_tools` — handles empty tool sets.
* `test_create_llm_plan_returns_valid_plan` — creates valid plans.
* `test_create_llm_plan_normalizes_single_step` — normalizes single-step plans.
* `test_create_llm_plan_invalid_json_returns_none` — rejects invalid JSON.
* `test_create_llm_plan_empty_steps_returns_none` — rejects empty plans.
* `test_create_llm_plan_llm_error_returns_none` — handles LLM errors.
* `test_create_llm_plan_rejects_step_without_tool` — rejects missing tool fields.
* `test_create_llm_plan_rejects_step_without_action` — rejects missing actions.
* `test_create_llm_plan_rejects_non_object_step` — rejects invalid step types.
* `test_create_llm_plan_rejects_empty_tool` — rejects empty tool names.
* `test_create_llm_plan_rejects_empty_action` — rejects empty actions.
* `test_create_llm_plan_rejects_unknown_tool_when_tools_are_available` — rejects unavailable tools.

---

## LLM Provider

### `tests/unit/test_llm_provider.py`

* `test_provider_initializes_local` — verifies local provider initialization.
* `test_provider_generate` — verifies provider generation.
* `test_provider_get_models` — retrieves provider models.
* `test_provider_has_model` — verifies model availability.
* `test_provider_current_model` — exposes the current model.
* `test_provider_connection` — verifies provider connectivity.

---

## LLM Router

### `tests/unit/test_llm_router.py`

* `test_llm_router_has_model_returns_true_when_planner_has_model` — detects planner models.
* `test_llm_router_has_model_returns_true_when_chat_has_model` — detects chat models.
* `test_llm_router_has_model_returns_false_for_unknown_model` — rejects unknown models.
* `test_llm_router_has_model_returns_false_for_empty_model_name` — rejects empty model names.
* `test_llm_router_has_model_handles_missing_planner_llm` — handles missing planner LLM.
* `test_llm_router_has_model_handles_missing_chat_llm` — handles missing chat LLM.
* `test_llm_router_has_model_returns_false_when_both_llms_are_missing` — handles missing LLMs.

---

## Logger

### `tests/unit/test_logger.py`

* `test_app_logger_does_not_duplicate_handlers` — prevents duplicate logger handlers.

---

# Main Application Wiring

## Main Container

### `tests/unit/test_main_container.py`

* `test_main_container_builds` — verifies container construction.
* `test_main_container_wires_orchestrators` — verifies orchestrator wiring.
* `test_main_container_wires_systems` — verifies system wiring.
* `test_main_container_agents_use_main_container` — verifies agent dependency ownership.

## Main Orchestrator

### `tests/unit/test_main_orchestrator.py`

* `test_main_orchestrator_routes_to_chat` — routes chat requests.
* `test_main_orchestrator_routes_to_memory` — routes memory requests.
* `test_main_orchestrator_routes_to_development` — routes development requests.
* `test_main_orchestrator_defaults_to_chat` — verifies the chat default.
* `test_main_orchestrator_unknown_system` — handles unknown systems.
* `test_main_orchestrator_calls_decision_agent` — verifies decision-agent invocation.

---

# Memory Tests

## Memory

### `tests/unit/test_memory.py`

* `test_memory_starts_empty` — verifies initial empty memory.
* `test_memory_save_and_get` — verifies save/retrieve.
* `test_memory_get_full` — verifies complete memory retrieval.
* `test_memory_update` — verifies updates.
* `test_memory_delete` — verifies deletion.
* `test_memory_clear` — verifies clearing.
* `test_memory_persists_to_disk` — verifies disk persistence.
* `test_memory_loads_existing_data` — verifies loading existing memory.
* `test_memory_save_propagates_persistence_error` — propagates persistence errors.

## Memory Agent

### `tests/unit/test_memory_agent.py`

* `test_memory_agent_extract_name_returns_name` — extracts a name.
* `test_memory_agent_extract_name_ignores_name_question` — avoids treating name questions as names.
* `test_memory_agent_extract_name_returns_only_first_name_word` — extracts the first name word.
* `test_memory_agent_save_handles_memory_exception` — handles memory exceptions.

## Memory Container

### `tests/unit/test_memory_container.py`

* `test_memory_container_creates_components` — verifies memory subsystem construction.
* `test_memory_container_attaches_agents` — verifies agent attachment.

## Memory Orchestrator

### `tests/unit/test_memory_orchestrator.py`

* `test_memory_orchestrator_save` — executes memory save.
* `test_memory_orchestrator_get` — executes memory retrieval.
* `test_memory_orchestrator_missing_decision` — handles missing decisions.
* `test_memory_orchestrator_unknown_action` — handles unsupported actions.

## Memory Tool

### `tests/unit/test_memory_tool.py`

* `test_memory_tool_has_expected_metadata` — verifies metadata.
* `test_memory_tool_rejects_non_dict_plan` — rejects invalid plans.
* `test_memory_tool_rejects_unknown_action` — rejects unknown actions.
* `test_memory_tool_save_uses_default_category` — verifies default categories.
* `test_memory_tool_save_uses_custom_category` — verifies custom categories.
* `test_memory_tool_save_rejects_missing_key_or_value` — validates save fields.
* `test_memory_tool_get_returns_existing_memory` — retrieves existing memory.
* `test_memory_tool_get_returns_failure_when_memory_not_found` — handles missing memory.
* `test_memory_tool_get_rejects_missing_key` — validates retrieval keys.
* `test_memory_tool_save_handles_memory_exception` — handles save exceptions.
* `test_memory_tool_get_handles_memory_exception` — handles retrieval exceptions.

---

# Planner

## Planner Agent

### `tests/unit/test_planner_agent.py`

* `test_planner_empty_task_returns_empty_plan` — handles empty tasks.
* `test_planner_none_task_returns_empty_plan` — handles `None` tasks.
* `test_planner_saves_last_task` — stores the last task.
* `test_planner_accepts_valid_llm_plan` — accepts valid LLM plans.
* `test_planner_rejects_unknown_tool` — rejects unknown tools.
* `test_planner_invalid_steps_use_fallback` — falls back for invalid steps.
* `test_planner_invalid_plan_uses_fallback` — falls back for invalid plans.
* `test_planner_exception_uses_fallback` — falls back after planner exceptions.
* `test_planner_analysis_fallback_uses_repository_analyzer` — uses repository analysis fallback.
* `test_planner_preserves_user_message` — preserves the original request.
* `test_planner_calculation_fallback_uses_calculator` — uses calculator fallback.
* `test_validate_plan_rejects_unsupported_tool_action` — rejects unsupported tool actions.

---

# Project Memory and Repository Intelligence

## Project Memory

### `tests/unit/test_project_memory.py`

* `test_project_memory_tool_has_expected_metadata` — verifies project-memory metadata.
* `test_project_memory_tool_rejects_non_dict_plan` — rejects invalid plans.
* `test_project_memory_tool_defaults_to_overview` — verifies overview default.
* `test_project_memory_tool_file_action` — retrieves file information.
* `test_project_memory_tool_files_action` — retrieves file collections.
* `test_project_memory_tool_architecture_action` — retrieves architecture information.
* `test_project_memory_tool_search_action` — performs project-memory searches.
* `test_project_memory_tool_search_defaults_to_empty_query` — handles empty searches.
* `test_project_memory_tool_context_action` — retrieves project context.
* `test_project_memory_tool_context_defaults_limit_to_five` — verifies context result limits.
* `test_project_memory_tool_overview_explicit_action` — handles explicit overview requests.
* `test_project_memory_tool_unknown_action` — rejects unknown actions.
* `test_project_memory_sync_repository_analysis` — verifies repository-analysis synchronization.

---

## Project Memory Sync

### `tests/unit/test_project_memory_sync.py`

* `test_project_memory_sync_accepts_changed_files` — accepts changed-file lists.
* `test_project_memory_sync_runs_repository_analysis` — runs repository analysis.
* `test_project_memory_sync_does_not_analyze_without_changes` — skips analysis without changes.
* `test_project_memory_sync_incrementally_updates_modified_python_file` — incrementally updates modified Python files.
* `test_project_memory_sync_adds_python_file` — adds Python files.
* `test_project_memory_sync_adds_non_python_without_python_analysis` — handles non-Python files without Python analysis.
* `test_project_memory_sync_removes_deleted_file_records_and_stale_relationships` — removes stale records and relationships.
* `test_project_memory_sync_normalizes_deduplicates_and_sorts_changed_paths` — normalizes changed paths.
* `test_project_memory_sync_uses_full_fallback_for_outside_path` — uses full fallback for external paths.
* `test_project_memory_sync_invalid_snapshot_uses_full_fallback` — falls back for invalid snapshots.
* `test_project_memory_sync_incremental_path_does_not_call_full_analyzer` — verifies incremental isolation.
* `test_project_memory_sync_persists_complete_snapshot_and_recomputes_identity` — persists complete snapshots and identity data.
* `test_project_memory_sync_fallback_success_is_ready_capable` — verifies fallback readiness.
* `test_project_memory_sync_persistence_failure_falls_back` — handles persistence failures.
* `test_project_memory_sync_handles_analysis_failure` — handles analysis failures.
* `test_project_memory_sync_initializes_empty_repository_memory` — initializes empty repository memory.
* `test_project_memory_sync_does_not_reinitialize_valid_snapshot` — preserves valid snapshots.
* `test_project_memory_sync_initialization_failure_is_not_ready` — reports failed initialization.
* `test_project_memory_sync_uses_workspace_for_repository_analysis` — uses the workspace for analysis.
* `test_project_memory_sync_stores_repository_analysis` — stores analysis results.
* `test_project_memory_sync_recomputes_relationships_for_changed_file` — recomputes changed-file relationships.

---

## Python Analyzer

### `tests/unit/test_python_analyzer.py`

* `test_python_analyzer_collects_symbols_and_imports` — extracts Python symbols and imports.
* `test_python_analyzer_skips_invalid_python` — handles invalid Python safely.

---

## Repository Analysis

### `tests/unit/test_repository_analysis.py`

* `test_repository_analysis_serializes_general_and_legacy_fields` — verifies analysis serialization.
* `test_repository_analysis_preserves_legacy_positional_field_order` — preserves compatibility with legacy field ordering.

---

## Repository Analyzer Ownership

### `tests/unit/test_repository_analyzer_ownership.py`

* `test_repository_analyzer_execute_does_not_require_project_memory` — verifies analyzer independence from project memory.
* `test_repository_analyzer_does_not_update_project_memory` — verifies ownership separation.

---

## Repository Analyzer / Project Memory

### `tests/unit/test_repository_analyzer_project_memory.py`

* `test_repository_analyzer_can_analyze_without_project_memory` — analyzes independently.
* `test_project_memory_sync_owns_repository_analysis_result` — verifies ProjectMemorySync ownership.
* `test_project_memory_sync_stores_repository_analysis` — verifies analysis storage.
* `test_project_memory_sync_does_not_sync_when_no_files_changed` — skips synchronization when unchanged.

---

## Repository Analyzer Tool

### `tests/unit/test_repository_analyzer_tool.py`

Tests repository discovery, Python parsing, module roles, tools, wiring, issue detection, and analysis-status evaluation.

* `test_repository_analyzer_has_expected_tool_metadata` — verifies tool metadata.
* `test_repository_analyzer_analyze_returns_path_not_found` — handles invalid repository paths.
* `test_repository_analyzer_analyze_rejects_non_repository_root` — rejects invalid repository roots.
* `test_repository_analyzer_iter_python_files_skips_ignored_directories` — skips ignored directories.
* `test_repository_analyzer_iter_python_files_skips_init_files` — skips `__init__` files.
* `test_repository_analyzer_read_uses_utf8_and_replaces_invalid_bytes` — safely reads invalid UTF-8.
* `test_repository_analyzer_top_level_defs_collects_functions_and_classes` — collects top-level definitions.
* `test_repository_analyzer_top_level_defs_returns_empty_for_invalid_python` — handles invalid Python.
* `test_repository_analyzer_analyze_accepts_repository_with_main_py` — analyzes repositories containing `main.py`.
* `test_repository_analyzer_execute_defaults_to_analyze` — verifies the default action.
* `test_repository_analyzer_accepts_non_python_repository_marker` — accepts non-Python repository markers.
* `test_repository_analyzer_execute_rejects_unsupported_action` — rejects unsupported actions.
* `test_repository_analyzer_collect_definitions_reads_python_files` — collects Python definitions.
* `test_repository_analyzer_collect_definitions_skips_invalid_python` — skips invalid Python.
* `test_repository_analyzer_collect_module_roles_uses_known_roles` — identifies known module roles.
* `test_repository_analyzer_collect_module_roles_ignores_missing_known_files` — ignores missing role files.
* `test_repository_analyzer_collect_overview_reports_python_files` — reports Python-file overview.
* `test_repository_analyzer_collect_tools_discovers_tool_files` — discovers tool files.
* `test_repository_analyzer_collect_tools_excludes_tool_infrastructure_files` — excludes infrastructure files.
* `test_repository_analyzer_collect_wiring_checks_returns_check_results` — reports wiring checks.
* `test_repository_analyzer_collect_issues_returns_list` — returns issue collections.
* `test_code_analyzer_analysis_status_passes_clean_analysis` — passes clean analysis.
* `test_code_analyzer_analysis_status_passes_performance_issue` — accepts performance issues as non-failing analysis.
* `test_code_analyzer_analysis_status_fails_syntax_error` — fails syntax errors.
* `test_code_analyzer_analysis_status_fails_logical_error` — fails logical errors.
* `test_code_analyzer_analysis_status_fails_security_issue` — fails security issues.
* `test_code_analyzer_analysis_status_fails_architecture_issue` — fails architecture issues.
* `test_code_analyzer_analysis_status_fails_high_risk_analysis` — fails high-risk analysis.
* `test_code_analyzer_analysis_status_fails_critical_risk_analysis` — fails critical-risk analysis.
* `test_code_analyzer_analysis_status_fails_parse_error` — fails parse errors.
* `test_code_analyzer_analysis_status_fails_invalid_analysis_type` — fails invalid analysis types.

---

## Repository Context Resolver

### `tests/unit/test_repository_context_resolver.py`

* `test_resolver_returns_target_metadata_symbols_and_dependencies` — returns target metadata and dependencies.
* `test_resolver_includes_direct_and_reverse_relationships` — includes both relationship directions.
* `test_resolver_deduplicates_and_applies_max_files_without_dropping_targets` — limits related files without dropping targets.
* `test_resolver_order_is_deterministic_and_reports_missing_targets` — preserves deterministic ordering.
* `test_resolver_handles_empty_memory_and_missing_relationships` — handles incomplete repository memory.
* `test_resolver_only_reads_snapshot_api` — verifies snapshot-only access.
* `test_resolver_exposes_relationship_reason_and_score_metadata` — exposes relationship metadata.
* `test_resolver_filters_requested_symbols_without_affecting_files` — filters symbols independently of files.
* `test_resolver_marks_truncation_when_related_candidates_exceed_limit` — reports truncation.
* `test_resolver_keeps_all_targets_even_when_limit_is_zero` — preserves explicit targets.
* `test_resolver_handles_missing_symbol_and_relationship_safely` — handles missing metadata.
* `test_resolver_accepts_single_target_path` — accepts a single target.
* `test_resolver_does_not_need_filesystem_or_repository_analyzer` — verifies snapshot-only architecture.
* `test_resolver_normalizes_duplicate_related_paths` — normalizes duplicate paths.

---

## Repository Indexer

### `tests/unit/test_repository_indexer.py`

* `test_repository_indexer_collects_mixed_language_metadata` — indexes mixed-language repository metadata.
* `test_repository_indexer_skips_ai_memory_and_is_json_serializable` — skips AI memory and verifies JSON serialization.

---

## Repository Knowledge Persistence

### `tests/unit/test_repository_knowledge_persistence.py`

* `test_project_memory_initializes_domain_separated_repository_stores` — initializes separated repository stores.
* `test_project_memory_syncs_general_repository_domains` — synchronizes repository domains.
* `test_project_memory_does_not_mark_snapshot_ready_after_store_failure` — prevents false readiness.
* `test_project_memory_sync_rolls_back_domain_stores_on_commit_failure` — rolls back failed commits.

---

# Test Infrastructure

### `tests/unit/test_test_insfacture.py`

* `test_test_infrastructure_is_available` — verifies test infrastructure availability.

---

# Tool System

## Tool Container

### `tests/unit/test_tool_container.py`

* `test_tool_container_creates_registry` — creates the tool registry.
* `test_tool_container_registers_tools` — registers tools.
* `test_tool_container_exposes_tools` — exposes registered tools.

## Tool Registry

### `tests/unit/test_tool_registry.py`

* `test_registry_starts_empty` — verifies an empty initial registry.
* `test_register_adds_tool` — registers a tool.
* `test_get_returns_none_for_unknown_tool` — handles unknown tools.
* `test_exists_returns_false_for_unknown_tool` — checks unknown-tool existence.
* `test_register_creates_default_metadata_from_tool` — creates default metadata.
* `test_register_accepts_custom_metadata` — accepts custom metadata.
* `test_get_metadata_returns_empty_dict_for_unknown_tool` — handles missing metadata.
* `test_unregister_existing_tool` — removes existing tools.
* `test_unregister_unknown_tool_returns_false` — handles unknown removal.
* `test_can_execute_returns_true_for_executable_tool` — identifies executable tools.
* `test_can_execute_returns_false_for_unknown_tool` — rejects unknown execution.
* `test_can_execute_returns_false_for_tool_without_execute` — rejects non-executable tools.
* `test_execute_returns_tool_not_found_error` — reports missing tools.
* `test_execute_returns_error_when_tool_has_no_execute` — reports non-executable tools.
* `test_execute_calls_tool_and_wraps_result` — executes and wraps results.
* `test_execute_passes_exact_data_to_tool` — preserves exact execution data.
* `test_execute_handles_tool_exception` — handles tool exceptions.
* `test_get_tool_descriptions_returns_registered_tools` — lists registered tools.
* `test_get_tool_descriptions_uses_metadata_defaults` — applies metadata defaults.
* `test_inspect_tool_returns_tool_information` — exposes tool information.
* `test_inspect_tool_returns_none_for_unknown_tool` — handles unknown inspection.
* `test_execute_preserves_tool_failure_status` — preserves failure status.
* `test_register_stores_tool_metadata` — stores metadata.

---

## Validation Tool

### `tests/unit/test_validation_tool.py`

* `test_validation_tool_metadata` — verifies validation-tool metadata.
* `test_validation_tool_rejects_non_dict` — rejects invalid plans.
* `test_validation_tool_rejects_missing_files` — requires file targets.
* `test_validation_tool_rejects_invalid_files_list` — validates file lists.
* `test_validation_tool_validates_python_file` — validates Python files.
* `test_validation_tool_detects_python_syntax_error` — detects syntax errors.
* `test_validation_tool_validates_multiple_files` — validates multiple files.
* `test_validation_tool_returns_file_not_found` — handles missing files.
* `test_validation_tool_rejects_path_outside_workspace` — blocks external paths.
* `test_validation_tool_rejects_directory` — rejects directory targets.
* `test_validation_tool_does_not_modify_file` — verifies read-only validation.
* `test_validation_tool_can_be_registered_and_executed` — verifies registry integration.

---

# Worker and Workspace Tests

## Worker

### `tests/unit/test_worker.py`

* `test_ai_worker_creates_cancel_event` — verifies worker cancellation state.
* `test_ai_worker_stop_sets_cancel_event` — verifies stop behavior.
* `test_ai_worker_propagates_cancel_event_to_model_container` — propagates cancellation.

## Workspace Watcher

### `tests/unit/test_workspace_watcher.py`

* `test_scan_collects_python_files` — discovers Python files.
* `test_scan_ignores_skipped_directories` — ignores excluded directories.
* `test_detect_changes_finds_new_file` — detects new files.
* `test_detect_changes_finds_modified_file` — detects modified files.
* `test_detect_changes_finds_deleted_file` — detects deleted files.
* `test_detect_changes_returns_empty_when_unchanged` — returns no changes when unchanged.
* `test_scan_collects_non_python_files` — discovers non-Python files.
* `test_detect_changes_reports_multiple_changes` — reports multiple changes.
* `test_watcher_forwards_changes_to_callback` — forwards detected changes.

---

# Contract Tests

Contract tests verify that core interchangeable components conform to their expected interfaces.

## Agent Contract

### `tests/contracts/test_agent_contract.py`

* `test_agent_satisfies_agent_contract` — verifies full agent contract compliance.
* `test_agent_has_name` — verifies the required name interface.
* `test_agent_has_run_method` — verifies the run interface.
* `test_agent_run_returns_result` — verifies result-producing execution.
* `test_agent_can_use_memory` — verifies optional memory support.
* `test_agent_can_recall_memory` — verifies memory retrieval.
* `test_agent_without_memory_is_valid` — verifies memory is not mandatory.

## LLM Provider Contract

### `tests/contracts/test_llm_provider_contract.py`

* `test_model_provider_satisfies_llm_contract` — verifies provider contract compliance.
* `test_model_provider_has_generate` — verifies generation interface.
* `test_model_provider_generate_returns_string` — verifies string generation results.
* `test_model_provider_tracks_generate_calls` — verifies generation-call tracking.
* `test_model_provider_has_connection_check` — verifies connection-check interface.
* `test_model_provider_exposes_current_model` — verifies current-model exposure.
* `test_model_provider_exposes_models` — verifies model-list exposure.
* `test_model_provider_can_check_model` — verifies model availability checks.

## Planner Contract

### `tests/contracts/test_planner_contract.py`

* `test_dummy_planner_satisfies_planner_contract` — verifies dummy planner compliance.
* `test_planner_agent_satisfies_planner_contract` — verifies PlannerAgent compliance.

## Tool Contract

### `tests/contracts/test_tool_contract.py`

* `test_tool_satisfies_tool_contract` — verifies tool contract compliance.
* `test_tool_has_name` — verifies tool naming.
* `test_tool_is_executable` — verifies executable interface.
* `test_tool_execute_returns_result` — verifies execution results.
* `test_tool_tracks_execution` — verifies execution tracking.
* `test_tool_tracks_arguments` — verifies argument tracking.
* `test_tool_supports_empty_result` — verifies empty-result handling.

---

# Integration Tests

## Agent and Memory

### `tests/integration/test_agent_memory.py`

* `test_agent_persists_memory` — verifies agent-to-memory persistence.
* `test_agent_recall_returns_memory` — verifies persisted-memory recall.
* `test_multiple_memory_entries_survive` — verifies multiple entries survive persistence.

## Development Pipeline

### `tests/integration/test_development_pipeline.py`

* `test_development_pipeline_executes_code_task` — verifies complete code-task execution.
* `test_development_pipeline_executes_repository_analysis` — verifies repository-analysis execution.
* `test_development_pipeline_executes_multiple_steps_in_order` — verifies ordered multi-step execution.
* `test_development_pipeline_passes_architecture_aware_context_to_code_agent` — verifies architecture-aware context propagation.
* `test_development_pipeline_runs_repository_analysis_fallback_when_memory_is_unavailable` — verifies repository-analysis fallback.

## Planner and Tool Pipeline

### `tests/integration/test_planner_tool_pipeline.py`

* `test_planner_creates_valid_tool_plan` — verifies planner-generated tool plans.
* `test_planner_plan_executes_through_tool_agent` — verifies plan execution through ToolAgent.
* `test_planner_to_tool_pipeline_preserves_input` — verifies request preservation across the pipeline.

## ToolAgent

### `tests/integration/test_tool_agent.py`

* `test_tool_agent_executes_registered_tool` — executes a registered tool.
* `test_tool_agent_returns_error_for_unknown_tool` — handles unknown tools.
* `test_tool_agent_executes_multiple_steps` — executes multiple planned steps.
* `test_tool_agent_delegates_code_step_to_injected_code_agent` — delegates code steps to CodeAgent.
* `test_tool_agent_normalizes_file_write_input` — normalizes file-write input.
* `test_tool_agent_normalizes_calculator_input` — normalizes calculator input.
* `test_tool_agent_execute_steps_returns_list_for_empty_plan` — handles empty plans.
* `test_tool_agent_code_generation_fails_when_llm_is_missing` — handles missing LLM.
* `test_tool_agent_code_generation_does_not_return_existing_content_on_llm_error` — prevents stale-content leakage after LLM errors.
* `test_tool_agent_calculator_ignores_unrelated_number_before_operands` — ignores unrelated numbers during calculator normalization.
* `test_tool_agent_returns_typed_error_for_missing_tool_name` — returns typed errors for missing tools.
* `test_normalize_calculator_preserves_all_operands` — preserves all calculator operands.
* `test_tool_agent_normalize_does_not_mutate_original_plan` — preserves the original plan.
* `test_tool_agent_normalize_calculator_does_not_mutate_original_plan` — preserves the original calculator plan.

---

# Benchmark System

Benchmarks evaluate controlled development tasks separately from the normal regression suite.

Benchmark tests are excluded from the default pytest run through:

```text
-m "not benchmark"
```

The benchmark test entry point is:

```text
tests/benchmarks/test_benchmarks.py
```

Current benchmark tests:

* `test_benchmark` — executes benchmark evaluation.
* `test_benchmark_tasks_exist` — verifies benchmark task definitions exist.

---

# Benchmark Tasks

The current benchmark suite contains the following development-task categories:

| Benchmark                            | Purpose                                                                                |
| ------------------------------------ | -------------------------------------------------------------------------------------- |
| **B01 — New File**                   | Evaluate creation of a new repository file.                                            |
| **B02 — Modify Existing File**       | Evaluate modification of an existing implementation.                                   |
| **B03 — Preserve Unrelated Files**   | Evaluate whether unrelated files remain untouched.                                     |
| **B04 — Multi-File Feature**         | Evaluate coordinated changes across multiple files.                                    |
| **B05 — Existing Code Preservation** | Evaluate preservation of existing functionality while implementing a requested change. |

Benchmark task definitions are stored under:

```text
benchmarks/tasks/
```

Benchmark results are stored under:

```text
benchmarks/results/
```

Current result artifacts include:

```text
B01_new_file/
B02_modify_existing_file/
B03_preserve_unrelated_files/
B04_multi_file_feature/
B05_existing_code_preservation/
```

Benchmark results should be treated as evaluation artifacts tied to a specific repository state rather than permanent claims about all future executions.

---

# Benchmark Evaluation Model

Benchmarks evaluate development behavior rather than only textual output.

Relevant properties include:

* Correct file creation
* Correct file modification
* Preservation of unrelated files
* Multi-file consistency
* Existing-code preservation
* Validation after modification
* Controlled workspace access
* Correct task execution

The benchmark system complements unit and integration tests rather than replacing them.

---

# Standard Regression Run

The standard regression suite is:

```powershell
pytest
```

Because benchmark tests are marked with `benchmark`, the default configuration excludes them.

For a complete validation cycle:

```powershell
pytest
pytest -m benchmark
```

A successful regression cycle should verify:

1. Unit tests pass.
2. Contract tests pass.
3. Integration tests pass.
4. Benchmark infrastructure is available.
5. Benchmark tasks are present.
6. Benchmark evaluation completes successfully when explicitly executed.

---

# Validation Policy

Flust treats validation as part of the development pipeline.

A development operation should conceptually follow:

```text
Request
  ↓
Decision
  ↓
Plan
  ↓
Tool Execution
  ↓
Code Change
  ↓
Validation
  ↓
Result
  ↓
Memory / Project Knowledge Update
```

For code changes, validation may include:

* Python syntax validation
* Code analysis
* Workspace-boundary validation
* Existing-code preservation checks
* Multi-file consistency checks
* Regression tests

The test suite specifically protects these boundaries through dedicated tests in the code writer, validation tool, file tool, repository intelligence, and integration layers.

---

# Self-Development Validation

Self-development is a long-term architectural goal of Flust rather than a claim that the complete autonomous loop is already implemented.

The intended development loop is:

```text
Observe Repository
       ↓
Analyze
       ↓
Detect Problem / Opportunity
       ↓
Plan
       ↓
Controlled Change
       ↓
Validate
       ↓
Learn / Update Memory
       ↓
Next Cycle
```

The current test architecture already validates several foundations required by this direction:

* Repository analysis
* Repository context resolution
* Project-memory synchronization
* Development context construction
* Planning
* Tool execution
* Code generation
* Code repair
* Validation
* Workspace isolation
* Rollback
* Integration pipelines

Future tests should extend these foundations toward complete controlled self-development cycles.

---

# Reproducibility

Tests should remain deterministic wherever possible.

Preferred test dependencies include:

* Fakes
* Temporary workspaces
* Controlled filesystem state
* Injected LLM responses
* Explicit planner outputs
* Explicit tool registrations
* Deterministic repository snapshots

Tests that require external services should be explicitly marked using the appropriate pytest marker.

Available markers include:

```text
unit
contract
integration
e2e
benchmark
slow
network
llm
```

---

# Release Validation

Before treating a significant Flust change as complete:

```text
Implementation
    ↓
Unit Tests
    ↓
Contract Tests
    ↓
Integration Tests
    ↓
Benchmark Validation
    ↓
Documentation Update
```

Changes affecting core architecture should update the relevant tests before being considered complete.

Changes affecting:

* Agents
* Planners
* Orchestrators
* Tools
* Memory
* Repository intelligence
* Development workflows
* Workspace isolation

should include corresponding regression coverage.

---

# Test Inventory Summary

The supplied repository test inventory contains:

* **2 benchmark test functions**
* **25 contract test functions**
* **25 integration test functions**
* **538 unit test functions**

for a total of:

**590 discovered `test_*` functions.**

The inventory is based on the repository test-function enumeration supplied for this document.

---

# Long-Term Testing Architecture

As Flust evolves, testing should expand alongside the development architecture.

Target direction:

```text
                 ┌─────────────────────┐
                 │   User / Task Input │
                 └──────────┬──────────┘
                            ↓
                 ┌─────────────────────┐
                 │ Decision / Planning│
                 └──────────┬──────────┘
                            ↓
                 ┌─────────────────────┐
                 │ Controlled Execution│
                 └──────────┬──────────┘
                            ↓
                 ┌─────────────────────┐
                 │ Validation / Tests  │
                 └──────────┬──────────┘
                            ↓
                 ┌─────────────────────┐
                 │ Repository Memory  │
                 └──────────┬──────────┘
                            ↓
                 ┌─────────────────────┐
                 │ Next Development   │
                 │      Cycle         │
                 └─────────────────────┘
```

The testing architecture should therefore evolve from isolated component verification toward increasingly complete validation of controlled autonomous development.

The core principle remains:

> **Every new development capability should be accompanied by tests that verify its behavior, boundaries, failure modes, and integration points.**