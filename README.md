# OpenCode Enhanced Configuration

> A comprehensive configuration for OpenCode with custom skills, agents, commands, and superpowers plugin integration.

This repository provides an enhanced OpenCode configuration with specialized skills for development workflows, custom agents for different task complexities, and commands for common operations. It integrates the "superpowers" plugin for advanced capabilities.

## Features

- **12+ Specialized Skills**: From React development to Stitch design automation
- **Multi-tier Agent System**: High/medium/low complexity agents with appropriate model assignments
- **Custom Commands**: Streamlined workflows for common tasks
- **Superpowers Integration**: Enhanced capabilities through the superpowers plugin
- **MCP Server Support**: Stitch and Notion integrations (configurable)

## Prerequisites

- [OpenCode](https://opencode.ai/) installed and configured
- Environment variables (optional):
  - `STITCH_API_KEY`: For Stitch MCP server integration
  - `OPENCODE_DOMAIN`: For CORS configuration

## Installation & Setup

### Using This Configuration

This configuration is designed for the OpenCode configuration directory (`~/.config/opencode`). To use it:

1. **Backup your existing configuration** (if any):
   ```bash
   cp -r ~/.config/opencode ~/.config/opencode.backup
   ```

2. **Clone or copy this configuration**:
   ```bash
   # If cloning as a new configuration
   git clone <repository-url> ~/.config/opencode-new
   mv ~/.config/opencode-new/* ~/.config/opencode/
   ```

3. **Configure environment variables** (optional):
   ```bash
   # Add to your shell profile (~/.bashrc, ~/.zshrc, etc.)
   export STITCH_API_KEY="your-stitch-api-key"
   export OPENCODE_DOMAIN="your-opencode-domain"
   ```

4. **Verify configuration**:
   ```bash
   opencode --version
   ```

### Configuration Structure

The main configuration file is `opencode.json` which defines:
- Server settings (CORS, domains)
- Plugin integrations (superpowers)
- Permission system
- Agent definitions with model assignments
- MCP server configurations