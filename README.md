# Rope MCP

A Model Context Protocol (MCP) server with companion CLI that exposes Python Rope refactoring library capabilities to Claude Code and command-line users.

## Features

- **Dual Interface**: Both MCP server for Claude Code integration and CLI for direct use
- **Safe Refactoring**: Symbol renaming and method extraction with built-in safety checks
- **Code Analysis**: Symbol definition finding, reference tracking, and object information
- **Backup System**: Automatic file backups with easy restore capabilities
- **Shared Core**: Identical functionality across MCP and CLI interfaces

## Quick Start

### Installation

```bash
# Install from PyPI (when released)
pip install rope-mcp

# Or install from source
git clone https://github.com/brukhabtu/rope-mcp.git
cd rope-mcp
pip install -e .
```

### CLI Usage

```bash
# Initialize project
rope-mcp init-project

# Analyze a symbol
rope-mcp analyze-symbol --file src/utils.py --line 23 --column 4

# Rename a symbol (with preview)
rope-mcp rename-symbol --file src/utils.py --line 23 --column 4 --new-name "better_name" --dry-run
rope-mcp rename-symbol --file src/utils.py --line 23 --column 4 --new-name "better_name"

# Extract method
rope-mcp extract-method --file src/utils.py --start-line 45 --end-line 67 --method-name "validate_input"
```

### Claude Code Integration

Add to your Claude Code MCP configuration:

```json
{
  "mcpServers": {
    "rope": {
      "command": "rope-mcp-server",
      "args": [],
      "env": {}
    }
  }
}
```

## Documentation

- [CLI Reference](docs/cli_reference.md)
- [MCP Tool Reference](docs/mcp_reference.md) 
- [Claude Code Integration Guide](docs/integration_guide.md)
- [Project Plan](PROJECT_PLAN.md)

## Development Status

🚧 **In Development** - See [PROJECT_PLAN.md](PROJECT_PLAN.md) for roadmap and current status.

## Contributing

See the [Implementation Roadmap](IMPLEMENTATION_ROADMAP.md) for development phases and contributing guidelines.

## License

MIT License - see [LICENSE](LICENSE) for details.
