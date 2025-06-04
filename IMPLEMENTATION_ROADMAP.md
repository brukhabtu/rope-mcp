# Rope MCP Implementation Roadmap

## Project Structure

```
rope-mcp/
├── src/
│   ├── rope_mcp/
│   │   ├── __init__.py
│   │   ├── core/                # Shared business logic
│   │   │   ├── __init__.py
│   │   │   ├── rope_manager.py  # Rope project management
│   │   │   └── tools/           # Core tool implementations
│   │   │       ├── __init__.py
│   │   │       ├── analyze.py   # rope_analyze_symbol logic
│   │   │       ├── rename.py    # rope_rename_symbol logic
│   │   │       ├── extract.py   # rope_extract_method logic
│   │   │       ├── project.py   # rope_init_project logic
│   │   │       └── backup.py    # rope_backup_restore logic
│   │   ├── server/              # MCP server interface
│   │   │   ├── __init__.py
│   │   │   ├── main.py          # FastMCP server entry point
│   │   │   └── handlers.py      # MCP tool handlers (thin wrappers)
│   │   ├── cli/                 # CLI interface
│   │   │   ├── __init__.py
│   │   │   ├── main.py          # Click-based CLI entry point
│   │   │   ├── commands/        # CLI command implementations
│   │   │   │   ├── __init__.py
│   │   │   │   ├── init_project.py
│   │   │   │   ├── analyze.py
│   │   │   │   ├── rename.py
│   │   │   │   ├── extract.py
│   │   │   │   └── backup.py
│   │   │   └── utils.py         # CLI utilities (output formatting, etc.)
│   │   ├── shared/              # Common utilities
│   │   │   ├── __init__.py
│   │   │   ├── validation.py    # Input validation
│   │   │   ├── errors.py        # Error handling
│   │   │   └── backup_manager.py # File backup system
│   │   └── types.py             # Type definitions
├── tests/
│   ├── test_core/               # Core logic tests
│   ├── test_cli/                # CLI interface tests  
│   ├── test_server/             # MCP server tests
│   ├── test_integration/        # End-to-end tests
│   └── fixtures/                # Test data
├── examples/
│   ├── sample_project/          # Sample Python project for testing
│   ├── cli_examples.md          # CLI usage examples
│   └── mcp_examples.md          # MCP/Claude Code examples
├── docs/
│   ├── cli_reference.md         # CLI command reference
│   ├── mcp_reference.md         # MCP tool reference
│   └── integration_guide.md     # Claude Code integration
├── pyproject.toml
├── README.md
└── .gitignore
```

## Implementation Steps

### Phase 1: Foundation & Shared Core

**Step 1.1: Project Bootstrap**
- [ ] Create project structure with dual interface design
- [ ] Setup `pyproject.toml` with dependencies (rope, fastmcp, click)
- [ ] Configure development tools (black, mypy, pytest)
- [ ] Create basic shared types and error handling

**Step 1.2: Core Infrastructure**
- [ ] Implement `RopeManager` class for project lifecycle
- [ ] Create shared error handling framework
- [ ] Setup logging and debugging infrastructure
- [ ] Basic input validation utilities
- [ ] File backup system implementation

**Step 1.3: Core Tool Logic - Analysis & Project Management**
- [ ] Implement core `analyze_symbol` function
- [ ] Implement core `init_project` function
- [ ] Test core logic on sample Python projects
- [ ] Handle edge cases (empty directories, permission issues)

### Phase 2: MCP Server Interface

**Step 2.1: MCP Server Setup**
- [ ] Create FastMCP server skeleton
- [ ] Implement MCP tool registration and discovery
- [ ] Create thin handler wrappers around core tools
- [ ] Setup JSON response formatting

**Step 2.2: Basic MCP Tools**
- [ ] Implement `rope_init_project` MCP handler
- [ ] Implement `rope_analyze_symbol` MCP handler
- [ ] Integration testing with Claude Code
- [ ] Error handling and MCP protocol compliance

### Phase 3: CLI Interface

**Step 3.1: CLI Framework Setup**
- [ ] Create Click-based CLI application structure
- [ ] Implement global options (--project-path, --json, --quiet, etc.)
- [ ] Create output formatting utilities (human-readable vs JSON)
- [ ] Setup CLI error handling

**Step 3.2: Basic CLI Commands**
- [ ] Implement `rope-mcp init-project` command
- [ ] Implement `rope-mcp analyze-symbol` command  
- [ ] Add dry-run capabilities where applicable
- [ ] CLI testing framework setup

**Step 3.3: CLI Testing & Validation**
- [ ] Unit tests for CLI commands using Click's test runner
- [ ] Integration tests comparing CLI and MCP tool outputs
- [ ] Validate identical functionality between interfaces

### Phase 4: Core Refactoring Operations

**Step 4.1: Rename Implementation**
- [ ] Implement core `rename_symbol` function
- [ ] Integration with Rope's rename refactoring
- [ ] Conflict detection and validation
- [ ] Cross-file rename testing

**Step 4.2: MCP & CLI Rename Integration**
- [ ] Implement `rope_rename_symbol` MCP handler
- [ ] Implement `rope-mcp rename-symbol` CLI command
- [ ] Add backup integration for both interfaces
- [ ] Test rename operations end-to-end

**Step 4.3: Extract Method Implementation**
- [ ] Implement core `extract_method` function
- [ ] Code selection validation
- [ ] Method signature generation
- [ ] Variable scope analysis for parameters/returns

**Step 4.4: MCP & CLI Extract Integration**
- [ ] Implement `rope_extract_method` MCP handler
- [ ] Implement `rope-mcp extract-method` CLI command
- [ ] Integration with backup system
- [ ] End-to-end testing for both interfaces

### Phase 5: Backup & Safety Systems

**Step 5.1: Backup System Implementation**
- [ ] Implement core backup/restore functionality
- [ ] Backup metadata tracking and cleanup
- [ ] File restoration with validation
- [ ] Backup storage management

**Step 5.2: Backup Tool Interfaces**
- [ ] Implement `rope_backup_restore` MCP handler
- [ ] Implement `rope-mcp backup` CLI commands (list, create, restore, cleanup)
- [ ] Integration with refactoring tools
- [ ] Comprehensive backup testing

### Phase 6: Testing & Documentation

**Step 6.1: Comprehensive Testing**
- [ ] Unit tests for all core tool implementations
- [ ] Integration tests with real Python projects
- [ ] Edge case testing (large files, complex inheritance, etc.)
- [ ] Performance testing and optimization

**Step 6.2: Interface Parity Testing**
- [ ] Automated tests ensuring MCP and CLI tool equivalence
- [ ] Error handling consistency validation
- [ ] Output format verification

**Step 6.3: Documentation & Examples**
- [ ] CLI reference documentation
- [ ] MCP tool reference documentation
- [ ] Claude Code integration guide
- [ ] Usage examples and best practices
- [ ] Troubleshooting guide

## Key Implementation Details

### Rope Project Management

```python
class RopeManager:
    def __init__(self):
        self.projects = {}  # path -> rope.Project
        
    def get_project(self, project_path: str) -> rope.Project:
        """Get or create persistent Rope project"""
        
    def refresh_project(self, project_path: str):
        """Force refresh of project cache"""
        
    def validate_file_in_project(self, project_path: str, file_path: str):
        """Ensure file belongs to project"""
```

### Error Handling Strategy

```python
class RopeError(Exception):
    def __init__(self, code: str, message: str, details: dict = None):
        self.code = code
        self.message = message
        self.details = details or {}
        
def handle_rope_exceptions(func):
    """Decorator to convert Rope exceptions to our error format"""
```

### Backup System Design

```python
class BackupManager:
    def create_backup(self, file_path: str) -> str:
        """Create timestamped backup, return backup_id"""
        
    def restore_backup(self, backup_id: str) -> bool:
        """Restore file from backup"""
        
    def list_backups(self, file_path: str = None) -> List[BackupInfo]:
        """List available backups"""
```

## Development Environment Setup

**Required Dependencies**:
```toml
[tool.poetry.dependencies]
python = "^3.8"
rope = "^1.0.0"
fastmcp = "^0.4.0"
click = "^8.0.0"

[tool.poetry.group.dev.dependencies]
pytest = "^7.0.0"
black = "^23.0.0"
mypy = "^1.0.0"
click-testing = "^0.1.0"  # For CLI testing
```

**Entry Points**:
```toml
[tool.poetry.scripts]
rope-mcp = "rope_mcp.cli.main:cli"              # CLI interface
rope-mcp-server = "rope_mcp.server.main:main"   # MCP server
```

**Development Commands**:
```bash
# Setup development environment
poetry install

# Run all tests
poetry run pytest

# Run only CLI tests
poetry run pytest tests/test_cli/

# Run only MCP server tests  
poetry run pytest tests/test_server/

# Run core logic tests
poetry run pytest tests/test_core/

# Format code
poetry run black src/

# Type checking
poetry run mypy src/

# Test CLI directly
poetry run rope-mcp --help
poetry run rope-mcp init-project

# Run MCP server
poetry run rope-mcp-server
```

## Testing Strategy

**Core Logic Testing**:
- Unit tests for shared core tool implementations
- Tests independent of interface (CLI/MCP)
- Mocked Rope operations for fast testing
- Edge case and error condition testing

```python
# Example core logic test
def test_analyze_symbol_core():
    result = analyze_symbol_core(
        project_path="/path/to/project",
        file_path="test_file.py", 
        line=10,
        column=4
    )
    assert result['success'] == True
    assert 'symbol_name' in result
```

**CLI Testing**:
- Click testing utilities for command execution
- Human-readable and JSON output validation
- Dry-run mode testing
- Error handling and exit codes

```python
# Example CLI test
from click.testing import CliRunner

def test_analyze_symbol_cli():
    runner = CliRunner()
    result = runner.invoke(cli, [
        'analyze-symbol',
        '--file', 'test_file.py',
        '--line', '10', 
        '--column', '4',
        '--json'
    ])
    
    assert result.exit_code == 0
    data = json.loads(result.output)
    assert data['success'] == True
```

**MCP Server Testing**:
- FastMCP testing framework
- JSON-RPC protocol compliance
- MCP tool registration and discovery
- Error response formatting

**Integration Testing**:
- Real Rope operations on sample Python projects
- End-to-end tool invocation (both CLI and MCP)
- File system operations and backup/restore
- Cross-platform compatibility

**Interface Parity Testing**:
- Automated tests ensuring CLI and MCP produce identical results
- Same input parameters → same output format
- Consistent error handling across interfaces

**Test Fixtures**:
- Sample Python projects with various patterns
- Edge cases (large files, complex inheritance, dynamic features)
- Error conditions (invalid syntax, permission issues, malformed input)

**Performance Testing**:
- Response time benchmarks for different project sizes
- Memory usage validation
- Concurrent operation testing (future)

## Deployment & Distribution

**Packaging**:
- Single Python package with dual entry points:
  - `rope-mcp` - CLI interface
  - `rope-mcp-server` - MCP server
- Clear installation instructions for both CLI and Claude Code users
- Dependencies properly specified and locked

**Installation Options**:
```bash
# From PyPI (when released)
pip install rope-mcp

# From source
git clone https://github.com/brukhabtu/rope-mcp.git
cd rope-mcp
pip install -e .

# Verify installation
rope-mcp --help              # CLI interface
rope-mcp-server --help       # MCP server
```

**Documentation Structure**:
- **README.md**: Quick start for both CLI and MCP usage
- **docs/cli_reference.md**: Complete CLI command reference
- **docs/mcp_reference.md**: MCP tool specifications
- **docs/integration_guide.md**: Claude Code setup and usage
- **examples/**: Working examples for both interfaces
- **TROUBLESHOOTING.md**: Common issues and solutions

**Release Strategy**:
- Semantic versioning (major.minor.patch)
- GitHub releases with detailed changelogs
- PyPI package publication with both CLI and server entry points
- Integration testing with Claude Code before releases
- Documentation updates with each release

**Claude Code Integration**:
- Clear MCP server configuration instructions
- Example Claude Code workflows
- Troubleshooting guide for common setup issues

**CLI Distribution**:
- Standalone CLI usage documentation
- Integration examples for development workflows
- Scripting and automation guides
- Performance benchmarks and recommendations
