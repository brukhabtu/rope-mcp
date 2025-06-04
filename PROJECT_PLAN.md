# Rope MCP Server - Project Plan & Scope

## Project Overview

**Goal**: Create a Model Context Protocol (MCP) server with companion CLI that exposes Python Rope refactoring library capabilities to Claude Code and command-line users, enabling AI-assisted and manual code analysis and refactoring operations.

**Vision**: Allow Claude Code to perform safe, professional-grade Python refactoring with the same precision as experienced developers using IDEs, while providing standalone CLI tools for testing and direct developer use.

**Target Integration**: 
- Claude Code (primary MCP integration)
- Command-line interface (testing, standalone use, scripting)

## Project Scope

### MVP Scope (Dual Interface: MCP + CLI)

**Core Refactoring Operations**
- Safe symbol renaming across entire codebase
- Method extraction from code selections
- Simple file backup and restore mechanisms

**Code Analysis Capabilities**
- Symbol definition location finding
- Reference/usage finding across project
- Object information and metadata retrieval

**Interface Components**
- **MCP Server**: FastMCP-based server for Claude Code integration
- **CLI Interface**: Command-line tools mapping 1:1 with MCP tools
- **Shared Core**: Common business logic used by both interfaces

**Safety & Reliability Features**
- File-level backup before modifications
- Rope's built-in safety checks and validation
- Project boundary enforcement
- Simple rollback via file restore
- Dry-run mode for destructive operations (CLI)

**Key Benefits of Dual Interface**:
- **Testing**: CLI enables independent testing without MCP overhead
- **Development**: Direct debugging and development workflow
- **Flexibility**: Developers can choose AI-assisted or direct CLI usage
- **Consistency**: Shared core ensures identical behavior across interfaces

### Explicitly Out of Scope (MVP)

**Advanced Refactoring**
- Move operations between classes/modules
- Signature changes and complex restructuring
- Import management and organization
- Code generation and templating

**Multi-Tool Integration**
- VS Code / Copilot support
- LSP server integration
- Real-time editor synchronization

**Performance Optimization**
- Large codebase handling (>50k LOC)
- Concurrent operation support
- Advanced caching strategies

**Complex Safety Mechanisms**
- Multi-file rollback systems
- Version control integration
- Sophisticated undo/redo chains

## Technical Architecture

### Core Design Decisions

**Dual Interface Strategy**: Shared core logic with MCP server and CLI interfaces
**Project Management**: Persistent Rope projects for performance and simplicity
**Concurrency**: Single-threaded operations (defer multi-user support)  
**Safety Strategy**: File-level backups + Rope's built-in validation
**Tool Philosophy**: Fewer, more powerful combined tools vs. many granular operations
**Target Integration**: Claude Code (MCP) + standalone CLI for testing/development

### Architecture Overview

```
rope-mcp/
├── core/                   # Shared business logic
│   ├── tools/             # Tool implementations (used by both interfaces)
│   └── rope_manager.py    # Rope project management
├── server/                # MCP server interface
│   ├── main.py           # FastMCP server
│   └── handlers.py       # MCP tool handlers (thin wrappers)
├── cli/                   # CLI interface
│   ├── main.py           # Click-based CLI entry point
│   └── commands/         # CLI command implementations
└── shared/               # Common utilities
    ├── validation.py     # Input validation
    ├── errors.py         # Error handling
    └── backup_manager.py # File backup system
```

### Core Components

**1. Shared Core Logic**
- Tool implementations used by both MCP and CLI
- Persistent Rope project management
- File backup/restore system
- Change validation pipeline
- Error handling and logging

**2. MCP Server Interface**
- FastMCP-based JSON-RPC server
- Thin handlers wrapping core tools
- Claude Code integration optimized
- Robust error reporting

**3. CLI Interface**
- Click-based command-line tools
- 1:1 mapping with MCP tools
- Human-readable and JSON output modes
- Dry-run capabilities for safety

**4. MVP Tool Set (Available in Both Interfaces)**

**Combined Analysis Tool**
- `rope_analyze_symbol` / `rope-mcp analyze-symbol` - Definition + references + object info

**Core Refactoring Tools**
- `rope_rename_symbol` / `rope-mcp rename-symbol` - Safe symbol renaming
- `rope_extract_method` / `rope-mcp extract-method` - Extract code to new method

**Project Management Tools**
- `rope_init_project` / `rope-mcp init-project` - Initialize/refresh Rope project
- `rope_backup_restore` / `rope-mcp backup` - File backup and restore operations

## Implementation Phases

### Phase 1: Foundation & Core Logic
**Deliverables:**
- Shared core tool implementations
- Rope project management (persistent projects)
- Basic error handling and validation framework
- File backup system implementation
- `rope_init_project` and `rope_analyze_symbol` core logic

**Success Criteria:**
- Core tools work reliably on sample Python projects
- Project initialization and symbol analysis functional
- Backup system creates and manages file backups

### Phase 2: MCP Server Interface
**Deliverables:**
- FastMCP server setup and configuration  
- MCP tool handlers wrapping core logic
- `rope_init_project` and `rope_analyze_symbol` MCP tools
- Integration testing with Claude Code
- Error handling and JSON response formatting

**Success Criteria:**
- MCP server connects to Claude Code successfully
- Basic analysis operations work through MCP interface
- Error responses properly formatted and helpful

### Phase 3: CLI Interface
**Deliverables:**
- Click-based CLI framework
- CLI commands mapping to all MCP tools
- Human-readable and JSON output modes
- Dry-run capabilities for destructive operations
- CLI testing framework

**Success Criteria:**
- All MCP tools available via CLI with identical functionality
- CLI provides excellent developer experience
- Comprehensive testing via CLI interface

### Phase 4: Core Refactoring Operations
**Deliverables:**
- `rope_rename_symbol` implementation with safety checks
- `rope_extract_method` for code selection extraction
- `rope_backup_restore` tool for rollback operations
- Integration in both MCP and CLI interfaces
- Comprehensive refactoring validation

**Success Criteria:**
- Safe renaming across entire Python projects
- Extract method operations on selected code
- Reliable rollback mechanism for failed operations
- No data loss or corruption in any operations

### Phase 5: Polish & Documentation
**Deliverables:**
- Comprehensive test suite for both interfaces
- Documentation and usage examples
- Integration guides for Claude Code
- Performance optimization and edge case handling
- Release preparation and packaging

**Success Criteria:**
- 95%+ test coverage on core functionality
- Clear documentation for all tools and interfaces
- Graceful handling of edge cases
- Ready for production use

## Key Technical Decisions

### Project Management Strategy
- **Approach**: Persistent Rope projects for performance and simplicity
- **Rationale**: Better performance, simpler state management, fits Claude Code workflows

### Safety Philosophy
- **Approach**: Trust Rope's safety checks + file-level backups for rollback
- **Rationale**: Rope is mature and reliable; simple backup strategy is sufficient for MVP

### Tool Granularity
- **Approach**: Fewer, more powerful combined tools vs. many granular operations
- **Rationale**: Simpler API surface, better Claude Code integration, easier maintenance

### Target Integration
- **Approach**: Claude Code only for MVP
- **Rationale**: Focused scope, better integration quality, faster development

## Risk Assessment & Mitigation

### High Risk
**Rope Performance on Medium+ Codebases**
- *Risk*: Rope can be slow on projects with 10k+ files
- *Mitigation*: Document performance characteristics, consider project size warnings

**File Backup Strategy Limitations**
- *Risk*: Simple file backup doesn't handle complex multi-file refactoring
- *Mitigation*: Limit MVP to operations that primarily affect single files

### Medium Risk
**Persistent Project State Corruption**
- *Risk*: Long-running Rope projects might get into inconsistent states
- *Mitigation*: Project refresh/reset capabilities, clear error handling

**Claude Code Integration Complexity**
- *Risk*: MCP protocol limitations or Claude Code compatibility issues
- *Mitigation*: Early testing, simple tool interfaces, good error reporting

### Low Risk
**Rope Library Limitations**
- *Risk*: Rope can't handle some dynamic Python patterns
- *Mitigation*: Document limitations, graceful failure modes

## Success Metrics

**MVP Success**: Claude Code can safely rename symbols and extract methods in typical Python projects without data loss or corruption.

**Quality Indicators**:
- Zero data loss incidents in testing
- Successful operations on 95%+ of common Python patterns
- Clear, actionable error messages for failure cases
- Sub-5-second response times for typical operations
