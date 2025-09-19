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

AARG generates custom firejail profiles that focus your AI assistant's full power exactly where you need it - in your project folder. It:
- Auto-detects your AI assistants (Claude, Gemini, etc.)
- Parses their configurations to find MCP servers and tool dependencies
- Creates comprehensive "institution rules" (firejail profiles)
- Ensures the agent works with full capabilities inside your project, and only there
- Prevents accidental access to unrelated system areas during normal operations

With AARG, your AI agent becomes a focused productivity powerhouse, channeling all its capabilities into your current project while preventing any accidental interactions with unrelated parts of your system. It's like having a dedicated workspace where your AI assistant can operate at full strength, professionally contained to the task at hand.

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
