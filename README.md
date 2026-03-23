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
   git clone https://github.com/katphlab/opencode ~/.config/opencode-new
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

## Skills Catalog

OpenCode skills are specialized workflows that activate automatically based on task context. This configuration includes:

| Skill | Description | When to Use | Examples |
|-------|-------------|-------------|----------|
| **design-md** | Analyze Stitch projects and synthesize semantic design systems into DESIGN.md files | When working with Stitch designs that need design system documentation | `@design-md analyze project.stitch` |
| **shadcn** | Manage shadcn components and projects — adding, searching, fixing, debugging, styling, and composing UI | When working with shadcn/ui, component registries, or projects with components.json | `@shadcn add button`, `@shadcn init` |
| **agent-browser** | Browser automation CLI for AI agents | When needing to interact with websites, fill forms, click buttons, take screenshots, or scrape data | `@agent-browser open https://example.com`, `@agent-browser fill form` |
| **stitch-design** | Unified entry point for Stitch design work | When doing UI/UX design work with Stitch, needs prompt enhancement or design system synthesis | `@stitch-design create dashboard` |
| **skill-creator** | Create or update reusable OpenCode skills | When creating new skills or editing existing skills for any project | `@skill-creator create new-skill` |
| **react-components** | Convert Stitch designs into modular Vite and React components | When converting Stitch designs to React components with AST-based validation | `@react-components convert design.stitch` |
| **agent-creator** | Create or update OpenCode agents | When creating new agents or editing existing agents for any project | `@agent-creator create new-agent` |
| **stitch-loop** | Iteratively build websites using Stitch with autonomous baton-passing | When building websites iteratively with Stitch using loop patterns | `@stitch-loop build portfolio-site` |
| **enhance-prompt** | Transform vague UI ideas into polished, Stitch-optimized prompts | When needing to enhance UI prompts with specificity and design system context | `@enhance-prompt "create login page"` |
| **command-creator** | Create or update reusable OpenCode commands | When creating new commands or editing existing commands | `@command-creator create new-command` |
| **react-ts-frontend** | Modern React stack: React 19, TypeScript, Tailwind CSS, Vite, TanStack Query | When building React apps, components, state management, or UI | `@react-ts-frontend create component`, `@react-ts-frontend setup project` |

### Skill Activation

Skills activate automatically when their triggers match the task context. You can also explicitly invoke skills using `@skill-name` syntax.