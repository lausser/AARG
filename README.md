# AARG - Agentenanstaltsregelgenerator
*Generator of Institution Rules for Agents*

## The Story Behind the Name

AI coding assistants like Claude Code and Gemini CLI are powerful productivity tools that need system access to function effectively - reading files, executing commands, installing packages. As professional developers, we follow the principle of least privilege: grant only the access that's actually needed. This best practice helps satisfy corporate security policies and demonstrates that the AI community takes security seriously.

So where do you properly accommodate an AI agent with professionally managed boundaries? In a German detention facility, of course!

**The metaphor emerged:**
- The **Agent** (AI assistant) is the resident who gets supervised accommodation
- The **Anstalt** (detention facility) is the firejail sandbox that provides structured boundaries
- The **Anstaltsregeln** (institution rules) define the agent's permitted activities
- And naturally, we need a **Generator** to create these rules

In true German fashion, we combined all these nouns into one gloriously bureaucratic compound word: **Agentenanstaltsregelgenerator**. Too long? That's what abbreviations are for: **AARG**!

## What AARG Actually Does

AARG generates enterprise-ready firejail profiles that focus your AI assistant's full power exactly where you need it - in your project folder. It:

### 🔧 **Core Features**
- **Auto-detects your AI assistants** (Claude, Gemini, OpenCode, Windsurf, Cursor)
- **Parses configurations** to find MCP servers and cross-assistant dependencies
- **Creates comprehensive "institution rules"** (firejail profiles)
- **Dynamic system library discovery** - no hardcoded versions, works across Linux distros
- **Enterprise DNS support** - respects corporate/internal DNS servers
- **Whitelist-only security** - principle of least privilege approach

### 🏢 **Enterprise Ready**
- **Distribution-agnostic** - works on any Linux distribution
- **Corporate network friendly** - uses system DNS, no hardcoded public DNS
- **Cross-assistant dependencies** - automatically handles Claude + Gemini MCP setups
- **Network connectivity** - proper DNS resolution without restrictive firewall rules
- **Security compliant** - follows professional security best practices

With AARG, your AI agent becomes a focused productivity powerhouse, channeling all its capabilities into your current project while preventing any accidental interactions with unrelated parts of your system. It's like having a dedicated workspace where your AI assistant can operate at full strength, professionally contained to the task at hand - and it works seamlessly in corporate environments.

```bash
# The full bureaucratic name (for documentation/headers)
# Agentenanstaltsregelgenerator - Generator of Institution Rules for Agents

# The command everyone actually uses
aarg

# What it generates
# Agentenanstaltsregeln for Claude Code, Gemini CLI, etc.

# Usage
cd ~/projects/myproject
aarg
# Generates: claude-myproject.agentenanstaltsregeln
```

## Recent Improvements

### Enterprise & Distribution Compatibility (Latest)

**🎯 Problem Solved**: Fixed critical issues that prevented Claude from running in firejail and blocked network connectivity in enterprise environments.

**✅ Key Fixes**:
- **Dynamic System Library Discovery** - No more hardcoded library versions (e.g., `libc.so.6` vs `libc.so.7`). AARG now automatically discovers the correct library paths using glob patterns, making profiles work across different Linux distributions.

- **Enterprise DNS Support** - Removed hardcoded public DNS servers (8.8.8.8) that break internal/corporate resource access. Now uses system DNS configuration (`/etc/resolv.conf`, `/etc/hosts`, `/etc/nsswitch.conf`) to respect enterprise DNS settings.

- **Whitelist-Only Security** - Eliminated the restrictive `private` directive that was causing "no suitable claude executable found" errors. Now uses whitelist-only approach for maximum compatibility while maintaining security.

- **Proper Network Connectivity** - Fixed network access by removing blocking `netfilter` rules while preserving security through protocol restrictions and proper DNS resolution.

**🏢 Enterprise Benefits**:
- Works with corporate DNS servers and internal resources
- Compatible across RHEL, Ubuntu, SUSE, Arch, and other distributions
- Handles complex MCP dependencies (Claude + Gemini CLI)
- Follows security best practices with least-privilege access

**🔧 Technical Details**:
- System libraries: `libc`, `libpthread`, `libm`, `libdl`, `libstdc++`, `libgcc_s`
- DNS resolution: `/etc/resolv.conf`, `/etc/hosts`, `/etc/nsswitch.conf`
- Network: `protocol unix,inet,inet6` without restrictive netfilter
- Security: Whitelist-only paths and binaries, no filesystem isolation
