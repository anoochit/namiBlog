---
title: Nami Blog - Changelog Update 2026-05-06
date: 2026-05-06
tags: Changelog, Nami, Update, State Management, Agent, Rust
---

# Changelog

All notable changes to this project will be documented in this file.

## [0.6.2] - 2026-05-06

### Added

- **Structured State Management**: Introduced the `StateManager` tool (`init_task`, `update_task`, `get_task`, `list_active_tasks`) for reliable tracking of long-running processes.
- **State Protocol**: Added `workspace/STATE_PROTOCOL.md` to provide the agent with mandatory guidelines for session continuity.

### Changed

- **Persona Migration**: Moved `AGENT.md`, `USER.md`, and `MEMORIES.md` from the project root to the `workspace/` directory for centralized management.
- **Context Optimization**: Refactored `AGENT.md`, `USER.md`, `MEMORIES.md`, and `STATE_PROTOCOL.md` into high-density, token-efficient formats.
- **Instruction Template**: Optimized the core agent instruction template in `src/agent/agent.rs` to reduce per-turn token overhead.
- **Always-On Protocol**: Integrated the `STATE_PROTOCOL.md` directly into the agent's core system instructions.
- **Project Initialization**: Updated `init` mode to automatically create token-optimized persona and protocol files within the `workspace/` directory.

### Removed

- **Manual Task Logs**: Deleted the obsolete `TaskLog.md` template and logs in favor of the structured tool-based approach.

## [0.6.1] - 2026-05-05

### Added

- **Interactive Project Initialization**: Refactored the `init` mode to use `inquire`, providing arrow-navigable selection menus for LLM providers and models, and masked input for API keys.
- **Wiki Templates**: Introduced new Markdown templates for `Blog Post` and `Task Log` in `workspace/wiki/Templates/` to standardize recurring wiki entries.

### Changed

- **Code Quality**: Applied `clippy` optimizations across the codebase to ensure idiomatic Rust patterns and better performance.
- **Repository Maintenance**: Removed `mcp.json` from version control to prevent local configuration leakage; updated `mcp.json.example` with remote SSE transport examples.
- **Documentation**: Added future extension ideas to developer tips and improved documentation for the `.namiignore` access control system.

### Fixed

- **Compilation & Stability**: Resolved various compilation issues and type mismatches in the MCP transport layer.
- **OpenAI Schema Validation**: Added missing properties to empty tool argument schemas to prevent OpenAI API validation errors.