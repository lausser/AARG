# AARG Tutorial: Mastering the Agentenanstaltsregelgenerator

Welcome to the world of **aarg** (Agentenanstaltsregelgenerator) - the delightfully over-engineered German bureaucratic solution to AI assistant security! 🏛️

## What is AARG?

Think of AARG as a digital immigration officer for AI agents. It creates detailed "institutional rules" (Agentenanstaltsregeln) that tell your AI assistants exactly where they can go, what they can touch, and how they should behave - all while maintaining that characteristic German precision and efficiency.

### The Bureaucratic Metaphor

AARG treats AI assistants as "residents" in a structured "institution" where:
- **Agentenanstaltsregeln** = The comprehensive rulebook for residents
- **Project areas** = Designated living quarters
- **Permitted areas** = Where residents can access (directories)
- **Institutional tools** = What equipment they can use (binaries)
- **Cross-dependencies** = When residents need to collaborate

## Quick Start: Your First AARG Experience

### 1. The Basics

```bash
# Navigate to your project
cd ~/projects/my-awesome-app

# Let AARG work its bureaucratic magic
aarg

# Watch the institutional assessment unfold
```

**What happens?**
- 🔍 AARG scans for AI assistants (Claude, Gemini, OpenCode, Windsurf, Cursor)
- 📋 Reads their configuration files and MCP server setups
- 🔗 Detects cross-assistant dependencies (the fun part!)
- 📄 Generates comprehensive institutional rules

### 2. The Interactive Experience

When AARG detects multiple assistants, you'll see:

```
🤖 Multiple AI assistants detected:
  1. claude
  2. gemini
  3. opencode
  4. windsurf
  5. all (generate comprehensive profile)

Choose assistant (1-5):
```

**Pro tip:** Option 5 "all" creates a universal rulebook that works for any assistant - bureaucratic efficiency at its finest!

### 3. The Dependency Detective

AARG's secret sauce is dependency detection. Watch this magic:

```
🔗 Detected cross-assistant dependencies: {'claude': {'gemini'}}

❓ claude uses gemini. Include dependencies? [Y/n]: y

🔗 Loaded dependency config for gemini (used by claude)
✅ Added gemini dependencies
```

**What's happening?** AARG found that Claude has a `gemini-cli` MCP server configured. Instead of letting the firejail block it, AARG proactively includes Gemini's binary and config directories. Bureaucratic thoroughness!

## The Security Challenge: Why AARG Exists

### The Problem

Modern AI coding assistants like Claude Code, Gemini CLI, and others are powerful tools that can autonomously:
- Read and write files across your entire filesystem
- Execute system commands and scripts
- Access sensitive configuration files
- Install packages and modify system state
- Connect to external services and APIs

While this autonomy makes them incredibly useful for development tasks, it also creates significant security risks:

- **Filesystem exposure**: Assistants can accidentally or maliciously access sensitive files outside the project scope
- **Privilege escalation**: Tools running with user permissions can modify critical system files
- **Data exfiltration**: Access to home directories, SSH keys, and configuration files
- **Supply chain attacks**: Malicious packages or MCP servers could exploit unrestricted access

### The Solution Requirements

AARG creates a secure sandbox that:
- ✅ **Restricts scope** to only the current project directory
- ✅ **Allows essential functionality** for development work
- ✅ **Permits MCP server execution** for assistant capabilities
- ✅ **Enables common development tools** (git, node, python, etc.)
- ✅ **Provides network access** for package managers and APIs
- ✅ **Maintains usability** without breaking the development workflow

## Advanced Usage Patterns

### The Verbose Investigator

```bash
aarg --verbose
```

This unleashes AARG's inner bureaucrat, showing every step:

```
🔍 Starting comprehensive AI assistant detection
🔍 Checking for claude (claude)
  ✅ Found executable: claude
  ✅ Found config: /home/user/.claude.json
🔍 Processing user-level Claude MCP servers
✅ Added MCP server: gemini-cli -> npx
🔒 SECURITY WARNING: /home/user/.claude.json may contain secrets
🔍 Searching for institutional tools (binaries)...
  ✅ gemini: /home/user/.local/bin/gemini
🏢 Adding permitted areas (directories) to institutional rules...
  ✅ /home/user/.gemini
```

### The Specific Selector

```bash
# Target a specific assistant
aarg --assistant claude
aarg --assistant gemini

# Skip the democracy, go straight to business
```

### The Security Optimizer

The two-step optimization dance:

```bash
# Step 1: Generate rules with logging
aarg
# Choose logging: y
# Let it create: claude-myproject.agentenanstaltsregeln

# Step 2: Run your assistant with tracking
firejail --tracelog --profile=claude-myproject claude code
# Do some work...

# Step 3: Optimize based on actual usage
aarg --optimize --assistant claude
```

**The payoff:** AARG will analyze what was actually accessed and create an optimized version:

```
📈 INSTITUTIONAL RULES OPTIMIZATION ANALYSIS:
   Areas in institutional rules: 15
   ✅ Used areas: 12
   ❌ Unused areas: 3
   ⚠️  Missing areas: 2

🗑️  UNUSED AREAS (can be removed):
   /usr/bin/poetry
   /home/user/.cache/pip

➕ SUGGESTED AREA ADDITIONS:
   whitelist /etc/ssl/certs
```

## AARG Architecture & Technical Reference

### Core Components Deep Dive

AARG is built around a modular architecture that handles the complexity of AI assistant security with German engineering precision:

#### 1. Configuration Scanner (`find_ai_assistant_configs()`)
- **Auto-detects** available AI assistants by checking for executables and config files
- **Parses JSON configurations** to extract MCP server definitions
- **Supports multiple formats** (Claude's `type: stdio` vs Gemini's direct `command`)
- **Handles errors gracefully** with detailed logging when `--verbose` is enabled

#### 2. Multi-Assistant Detection (`detect_all_assistants()`)
- **Comprehensive scanning** for Claude, Gemini, OpenCode, Windsurf, Cursor
- **Returns ALL detected assistants** instead of stopping at first found
- **Interactive selection** when multiple assistants are available
- **Supports "all" option** for universal profiles

#### 3. Dependency Analyzer (`detect_cross_assistant_dependencies()`)
- **Scans MCP server commands** for references to other AI assistants
- **Builds dependency graphs** between assistants
- **Interactive confirmation** before including dependencies
- **Recursive dependency resolution** for complex chains

#### 4. MCP Command Extractor (`extract_mcp_servers()`)
- **Identifies MCP servers** from configuration files
- **Extracts command names** (`basic-memory`, `npx`, `uvx`, etc.)
- **Supports multiple formats**:
  - Claude: `{"type": "stdio", "command": "...", "args": [...]}`
  - Gemini: `{"command": "...", "args": [...]}`
  - Generic: `{"executable": "...", "arguments": [...]}`

#### 5. Binary Locator (`collect_binaries()`)
- **Uses `which` command** to find full paths to executables
- **Combines MCP commands** with common development tools
- **Includes all AI assistant binaries** automatically
- **Creates whitelist entries** for firejail profiles
- **Logs missing binaries** for troubleshooting

#### 6. Essential Path Manager (`add_essential_paths()`)
- **Adds standard directories** needed by development tools
- **Includes assistant-specific paths** (e.g., `~/.claude/`, `~/.gemini/`)
- **Supports dependency paths** for cross-assistant setups
- **Checks path existence** before adding to whitelist
- **Provides detailed logging** of what's included/excluded

#### 7. Security Monitor (`_check_config_secrets()`)
- **Scans configuration files** for potential secrets
- **Warns about API keys and tokens** in config files
- **Suggests environment variable alternatives**
- **Helps maintain security best practices**

#### 8. Profile Generator (`generate_profile()`)
- **Creates firejail profiles** with institutional metaphor comments
- **Includes private directive** to restrict to project directory
- **Adds whitelist entries** for directories and binaries
- **Configures network access** for package managers and APIs
- **Applies security restrictions** (`noroot`, `novideo`, etc.)
- **Supports multi-assistant profiles** with dependency comments

#### 9. Usage Analyzer (`analyze_firejail_logs()`)
- **Parses firejail trace logs** to see actual file access patterns
- **Compares against whitelist** to identify unused entries
- **Suggests missing paths** that were accessed but not whitelisted
- **Generates optimization reports** for profile refinement

#### 10. Profile Optimizer (`generate_optimized_profile()`)
- **Comments out unused entries** with `# UNUSED AREA:` prefix
- **Suggests new entries** with `# SUGGESTED AREA ADDITIONS:`
- **Preserves original structure** while marking changes
- **Creates `.optimized.agentenanstaltsregeln` files** for testing

### Essential System Access Requirements

AI assistants need access to these critical system areas:

#### Essential Directories
- `~/.local/` - User-installed packages and binaries
- `~/.npm/` - Node.js package cache
- `~/.cache/` - Application caches
- `~/.config/` - Application configurations
- `/tmp/` - Temporary files
- `/usr/lib/`, `/lib/`, `/lib64/` - System libraries
- `/usr/share/` - Shared data files
- `/etc/ssl/certs` - SSL certificates for HTTPS

#### Assistant-Specific Directories
- `~/.claude/` - Claude Code configurations
- `~/.gemini/` - Gemini CLI settings
- `~/.config/opencode/` - OpenCode configurations
- `~/.windsurf/` - Windsurf settings
- `~/.cursor/` - Cursor configurations

#### Common Development Binaries
- **Node.js ecosystem**: `node`, `npm`, `npx`, `yarn`, `pnpm`
- **Python ecosystem**: `python`, `python3`, `pip`, `pip3`, `uv`, `uvx`, `poetry`
- **Version control**: `git`, `curl`, `wget`
- **System tools**: `bash`, `sh`, `ps`, `ls`, `cat`, `grep`, `sed`, `awk`
- **Build tools**: `gcc`, `make`, `cmake`, `cargo`, `go`
- **Container tools**: `docker`, `podman` (with special socket handling)

#### MCP Servers
Model Context Protocol (MCP) servers provide specialized capabilities:
- `basic-memory` - Persistent memory across sessions
- `npx @upstash/context7-mcp` - Context management
- `uvx serena` - IDE assistance
- Custom MCP servers defined in configuration files

### Code Structure Reference

```python
class AARG:
    def __init__(self, assistant_type="auto", verbose=False):
        # Initialize with assistant type and logging level

    def detect_all_assistants(self):
        # Detect ALL available AI assistants (redesigned)

    def choose_assistant_interactive(self):
        # Allow user to choose from detected assistants

    def find_ai_assistant_configs(self):
        # Scan and parse configuration files with hierarchy resolution

    def _load_assistant_config(self, assistant_type, paths):
        # Load configuration for specific assistant with scope handling

    def extract_mcp_servers(self, config_data, assistant_type):
        # Extract MCP server commands from configs

    def detect_cross_assistant_dependencies(self, configs):
        # Detect when MCP servers reference other AI assistants

    def resolve_dependency_configs(self, dependencies):
        # Load configurations for detected dependencies

    def collect_binaries(self, configs):
        # Find paths to all required binaries including dependencies

    def add_essential_paths(self, configs):
        # Add standard directories including dependency paths

    def generate_profile(self, enable_logging=False, configs=None):
        # Create firejail profile with multi-assistant support

    def suggest_profile_path(self, configs=None):
        # Generate appropriate filename for multi-assistant profiles

    def run_generate_mode(self):
        # Main profile generation workflow

    def run_analysis_mode(self):
        # Analyze firejail access logs

    def run_optimization_mode(self):
        # Optimize profiles based on usage data
```

### Configuration Format Support

AARG intelligently handles different configuration formats:

#### Claude Code Configuration
```json
{
  "mcpServers": {
    "basic-memory": {
      "type": "stdio",
      "command": "basic-memory",
      "args": ["mcp"],
      "env": {}
    },
    "serena": {
      "type": "stdio",
      "command": "uvx",
      "args": ["--from", "git+https://github.com/oraios/serena", "serena", "start-mcp-server"]
    }
  },
  "projects": {
    "/path/to/project": {
      "mcpServers": {
        "project-specific-server": {
          "type": "stdio",
          "command": "custom-command"
        }
      }
    }
  }
}
```

#### Gemini CLI Configuration
```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp", "--api-key", "..."]
    },
    "basic-memory": {
      "command": "basic-memory",
      "args": ["mcp"]
    }
  }
}
```

#### OpenCode Configuration
```json
{
  "mcp": {
    "filesystem": {
      "type": "local",
      "command": ["npx", "@modelcontextprotocol/server-filesystem"],
      "enabled": true,
      "environment": {
        "ALLOWED_PATHS": "/path/to/project"
      }
    },
    "web-search": {
      "type": "remote",
      "url": "https://api.example.com/mcp",
      "enabled": true,
      "headers": {
        "Authorization": "Bearer YOUR_API_KEY"
      }
    }
  }
}
```

### Multi-Scope Configuration Handling

AARG implements sophisticated configuration precedence:

1. **Project-level configs** take highest precedence
2. **User-level configs** provide defaults
3. **System-level configs** (if any) provide fallbacks

#### Assistant-Specific Configuration Discovery

**Claude Code:**
- Handles complex nested structure where both global and project-specific MCP servers can be defined in the same file
- Supports project-specific overrides in the "projects" section

**OpenCode:**
- Configuration hierarchy: `OPENCODE_CONFIG` env var > project `opencode.json` > `~/.config/opencode/opencode.json`
- Supports both JSON and JSONC (JSON with Comments) formats
- Project configs are discovered by traversing up to the nearest Git directory
- Uses "mcp" key instead of "mcpServers" for MCP server configuration

**Gemini CLI:**
- Simple global configuration in `~/.gemini/settings.json`
- Project-specific configs supported in `.gemini/settings.json`

## Understanding the Generated Rules

Let's decode a typical Agentenanstaltsregeln file:

```bash
# aarg - Agentenanstaltsregeln for Claude with Gemini
# Project: my-awesome-app
# Generated institutional rules with permitted areas and tools

include globals.local

# Private institutional area - restricted to project directory
private /home/user/projects/my-awesome-app

# Permitted areas (whitelisted directories)
whitelist /home/user/.claude        # Claude's configuration
whitelist /home/user/.gemini        # Gemini's configuration (dependency!)
whitelist /home/user/.local         # User-installed tools
whitelist /home/user/.npm           # Node.js packages
whitelist /tmp                      # Temporary files

# Permitted tools (whitelisted binaries)
whitelist /home/user/.local/bin/claude    # Primary assistant
whitelist /home/user/.local/bin/gemini    # Dependency assistant
whitelist /usr/bin/node                   # Runtime for MCP servers
whitelist /usr/bin/npx                    # Package executor

# Security restrictions - institutional rules
noroot                              # No administrator privileges
novideo                            # No camera access
nogroups                           # No group permissions
nonewprivs                         # No privilege escalation
```

**Key insight:** Notice how AARG included both Claude AND Gemini configurations because it detected the dependency. That's bureaucratic intelligence!

## Tips and Tricks

### 🎯 **The Configuration Detective**

AARG is surprisingly smart about configuration formats:

- **Claude:** Handles both user-level (`~/.claude.json`) and project-level configs with complex nested structures
- **Gemini:** Reads `~/.gemini/settings.json` and understands the auth structure
- **Multi-scope:** Merges user and project configs with proper precedence

### 🔐 **Security Awareness**

AARG watches out for you:

```
🔒 SECURITY WARNING: /home/user/.claude.json may contain secrets
    (apiKey, api_key, token, auth, oauth, key, private)
    Consider using environment variables instead
```

**Best practice:** Move sensitive keys to environment variables:
```bash
export ANTHROPIC_API_KEY="your-key-here"
export GEMINI_API_KEY="your-key-here"
```

### 🏗️ **Project Organization**

AARG generates project-specific rules:

```bash
cd ~/projects/frontend-app
aarg --assistant claude
# Creates: claude-frontend-app.agentenanstaltsregeln

cd ~/projects/backend-api
aarg --assistant claude
# Creates: claude-backend-api.agentenanstaltsregeln
```

Each project gets its own institutional rulebook!

### 🔄 **Dependency Chain Management**

For complex setups with multiple MCP dependencies:

```bash
# AARG can handle chains like: Claude → Gemini → Some-Tool
# It will ask about each dependency level
❓ claude uses gemini. Include dependencies? [Y/n]: y
❓ gemini uses some-tool. Include dependencies? [Y/n]: y
```

### 🚀 **CI/CD Integration**

AARG plays nice with automation:

```yaml
# .github/workflows/secure-ai.yml
- name: Generate security rules
  run: echo "1" | aarg --assistant claude

- name: Run AI assistant securely
  run: firejail --profile=claude-${{ github.event.repository.name }} claude code --task="review-pr"
```

**Note:** In non-interactive environments, AARG defaults to sensible choices.

### 🔧 **Development Workflow Integration**

AARG fits seamlessly into development workflows:

```bash
# Setup new project
git clone https://github.com/user/project
cd project
aarg --verbose

# Daily development with tracking
firejail --tracelog --profile=claude-project claude code

# Weekly optimization
aarg --optimize --assistant claude
```

### 📊 **Benefits Overview**

#### Security
- **Filesystem isolation**: AI assistants can only access the current project
- **Binary restrictions**: Only approved tools are available
- **Network filtering**: Controlled network access protocols
- **Privilege dropping**: No root access, limited system capabilities

#### Usability
- **Automatic detection**: Finds and configures AI assistants automatically
- **MCP compatibility**: Full support for Model Context Protocol servers
- **Development workflow**: All common dev tools remain available
- **Easy optimization**: Data-driven profile refinement

#### Maintenance
- **Usage tracking**: See exactly what files/directories are accessed
- **Profile optimization**: Remove unused whitelist entries automatically
- **Clear documentation**: Institutional metaphor makes security boundaries intuitive
- **Verbose logging**: Detailed troubleshooting information

## Troubleshooting

### "No AI assistants detected"

```bash
# Check if executables are in PATH
which claude gemini opencode

# Check for config files
ls ~/.claude.json ~/.gemini/settings.json

# Force detection
aarg --assistant claude --verbose
```

### "MCP server fails in firejail"

This is exactly what AARG solves! The issue is usually:

1. **Missing assistant binary:** AARG now includes all AI assistant binaries
2. **Missing config access:** AARG includes `~/.claude/`, `~/.gemini/`, etc.
3. **Missing dependencies:** Use AARG's dependency detection

### "Profile too permissive"

Use the optimization workflow:

```bash
# 1. Run with logging
firejail --tracelog --profile=your-profile your-assistant

# 2. Optimize
aarg --optimize --assistant your-assistant

# 3. Use the .optimized.agentenanstaltsregeln file
```

### Ctrl-C Handling

AARG gracefully handles interruption at any prompt:

```
Choose assistant (1-5): ^C

🚪 Operation cancelled by user
```

No more ugly Python stack traces!

## Configuration Formats Supported

### Claude Code Format
```json
{
  "mcpServers": {
    "server-name": {
      "type": "stdio",
      "command": "command-name",
      "args": ["arg1", "arg2"],
      "env": {"VAR": "value"}
    }
  }
}
```

### Gemini CLI Format
```json
{
  "mcpServers": {
    "server-name": {
      "command": "command-name",
      "args": ["arg1", "arg2"]
    }
  }
}
```

### OpenCode Format
```json
{
  "mcp": {
    "server-name": {
      "type": "local",
      "command": ["command-name", "arg1", "arg2"],
      "enabled": true,
      "environment": {
        "ENV_VAR": "value"
      }
    },
    "remote-server": {
      "type": "remote",
      "url": "https://example.com/mcp",
      "enabled": true,
      "headers": {
        "Authorization": "Bearer token"
      }
    }
  }
}
```

### Generic Format
```json
{
  "tools": {
    "tool-name": {
      "executable": "command-name",
      "arguments": ["arg1", "arg2"]
    }
  }
}
```

AARG understands them all!

## Complete Configuration Reference

AARG searches for AI assistant configurations in a hierarchical order, with project-specific settings taking precedence over user settings, and user settings taking precedence over system-wide settings.

### Claude Code Configuration Locations

AARG searches for Claude configurations in this order:

**Enterprise/System Configurations (highest precedence for managed environments):**
- `/etc/claude-code/managed-settings.json` (Linux enterprise deployments)
- `/Library/Application Support/ClaudeCode/managed-settings.json` (macOS enterprise deployments)

**Project-Specific Configurations:**
- `.claude/settings.json` (project workspace settings)
- `.claude/settings.local.json` (local project settings, typically gitignored)

**User Configurations:**
- `~/.claude.json` (primary user config)
- `~/.claude/claude.json` (alternative location)
- `~/.config/claude/config.json` (XDG Base Directory compliant)

### Gemini CLI Configuration Locations

**Project-Specific Configurations:**
- `.gemini/settings.json` (project workspace settings)
- `.gemini/config.json` (alternative project config)

**User Configurations:**
- `~/.gemini/settings.json` (primary user config)
- `~/.config/gemini/config.json` (XDG Base Directory compliant)

### OpenCode Configuration Locations

OpenCode follows a unique hierarchy supporting environment variable overrides:

**Custom Configuration Path (highest precedence):**
- `$OPENCODE_CONFIG` (environment variable pointing to custom config file)

**Project-Specific Configurations:**
- `opencode.json` (in project root, highest precedence for project settings)

**User Configurations:**
- `~/.config/opencode/opencode.json` (XDG Base Directory compliant)
- `~/.opencode.json` (legacy location for backward compatibility)

### Windsurf IDE Configuration Locations

**Project-Specific Configurations:**
- `.windsurfrules` (project-specific AI rules)
- `.windsurf/rules` (project rules directory)

**User Configurations:**
- `~/.windsurf/config.json` (primary user config)
- `~/.config/windsurf/settings.json` (XDG Base Directory compliant)
- `~/.codeium/.codeiumignore` (global ignore rules for enterprise deployments)

### Cursor IDE Configuration Locations

**Project-Specific Configurations:**
- `.cursor/index.mdc` (project-specific cursor rules and settings)

**User Configurations:**
- `~/.cursor/config.json` (primary user config)
- `~/.cursor/cli-config.json` (CLI-specific configuration)
- `~/.cursor-server/` (remote server configuration directory)
- `~/.config/cursor/settings.json` (XDG Base Directory compliant)

### Configuration Hierarchy and Precedence

AARG follows this precedence order (highest to lowest):

1. **Environment Variables** (OpenCode: `$OPENCODE_CONFIG`)
2. **Enterprise/System Managed Settings** (Claude enterprise deployments)
3. **Project-Specific Settings** (`.claude/`, `opencode.json`, `.windsurf/`, `.cursor/`)
4. **User XDG Config** (`~/.config/assistant-name/`)
5. **User Home Config** (`~/.assistant-name/`)
6. **Legacy Locations** (backward compatibility)

### Directory Permissions in Institutional Rules

AARG automatically includes all relevant configuration directories in the generated firejail profiles:

```bash
# Claude configuration access
whitelist ~/.claude
whitelist ~/.config/claude
whitelist /etc/claude-code              # Enterprise deployments

# OpenCode configuration access
whitelist ~/.config/opencode
whitelist ~/.opencode                   # Legacy support

# Windsurf configuration access
whitelist ~/.windsurf
whitelist ~/.config/windsurf
whitelist ~/.codeium                    # Enterprise ignore rules

# Cursor configuration access
whitelist ~/.cursor
whitelist ~/.cursor-server
whitelist ~/.config/cursor
```

This ensures your AI assistants can access their configurations while maintaining security boundaries.

## Advanced Scenarios

### Multi-Assistant Workflow

```bash
# Create rules for each assistant
aarg --assistant claude
aarg --assistant gemini

# Use different assistants for different tasks
firejail --profile=claude-myproject claude code     # For coding
firejail --profile=gemini-myproject gemini         # For analysis
```

#### Multiple Projects Strategy
Each project gets its own institutional rulebook:
```bash
cd ~/projects/frontend-app
aarg --assistant claude  # Creates claude-frontend-app.agentenanstaltsregeln

cd ~/projects/backend-api
aarg --assistant claude  # Creates claude-backend-api.agentenanstaltsregeln

cd ~/projects/data-analysis
aarg --assistant gemini  # Creates gemini-data-analysis.agentenanstaltsregeln
```

#### Cross-Assistant Dependencies
AARG automatically handles complex dependency chains:
```bash
# When Claude uses gemini-cli MCP server:
# 1. AARG detects the dependency
# 2. Asks for confirmation
# 3. Loads Gemini's configuration
# 4. Includes Gemini's binary and config paths
# 5. Creates: claude-with-gemini-myproject.agentenanstaltsregeln
```

### Custom MCP Servers

AARG automatically detects and includes custom MCP servers:

```json
{
  "mcpServers": {
    "my-custom-server": {
      "type": "stdio",
      "command": "my-custom-binary",
      "args": ["--config", "/path/to/config"]
    }
  }
}
```

AARG will:
1. Find `my-custom-binary` in PATH
2. Include it in the whitelist
3. Include any referenced config paths

### Container Development

AARG includes Docker/Podman support automatically:

```bash
# If Docker is detected, AARG adds:
ignore noroot                          # Docker needs root in container
whitelist /var/run/docker.sock         # Docker socket
whitelist /run/user/${UID}/podman      # Podman socket
```

## The Philosophy Behind AARG

AARG embodies German engineering principles:

1. **Gründlichkeit** (Thoroughness): Checks every possible configuration location
2. **Ordnung** (Order): Creates structured, predictable rules
3. **Sicherheit** (Security): Applies least-privilege principles
4. **Effizienz** (Efficiency): Optimizes rules based on actual usage

The result? Your AI assistants work perfectly within their designated boundaries, like well-behaved residents in a efficiently managed institution.

## What's Next?

- **Monitor usage:** Use `aarg --analyze` to understand access patterns
- **Optimize regularly:** Run `aarg --optimize` after workflow changes
- **Stay updated:** AARG evolves with new AI assistants and MCP servers
- **Contribute:** Found a new assistant format? AARG is extensible!

## Final Words

AARG transforms the chaotic world of AI assistant security into a well-ordered, German-engineered system. Your AI assistants get the freedom they need within the boundaries they should respect.

Welcome to the institution. Your AI residents will thank you for the clear rules! 🏛️✨

---

*"Ordnung muss sein!" - There must be order!*