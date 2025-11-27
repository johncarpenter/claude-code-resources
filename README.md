# Claude Code CLI Tips & Tricks

**This guide covers 30+ pro-tips for effectively using Claude Code for agentic coding**

**[Claude Code](https://claude.ai/code)** is Anthropic's agentic coding tool that lives in your terminal and helps you turn ideas into code faster than ever before. It functions as a conversational, agentic command-line tool that can reason about your requests, choose tools (like running shell commands, editing files, searching codebases), and execute multi-step plans to help with your development workflow.

In practical terms, Claude Code acts like a supercharged pair programmer and command-line assistant. It excels at coding tasks, debugging, content generation, codebase exploration, and system automation—all through natural language prompts. Before diving into pro tips, let's quickly recap how to set up Claude Code and get it running.

## Table of Contents

- [Getting Started](#getting-started)
- [Tip 1: Use `CLAUDE.md` for Persistent Context](#tip-1-use-claudemd-for-persistent-context)
- [Tip 2: Create Custom Slash Commands](#tip-2-create-custom-slash-commands)
- [Tip 3: Extend Claude Code with MCP Servers](#tip-3-extend-claude-code-with-mcp-servers)
- [Tip 4: Leverage Memory with `#` Quick Addition](#tip-4-leverage-memory-with--quick-addition)
- [Tip 5: Use Checkpoints and `/rewind` as an Undo Button](#tip-5-use-checkpoints-and-rewind-as-an-undo-button)
- [Tip 6: Create and Use Subagents for Parallel Work](#tip-6-create-and-use-subagents-for-parallel-work)
- [Tip 7: Reference Files with `@` for Explicit Context](#tip-7-reference-files-with--for-explicit-context)
- [Tip 8: Use Agent Skills for Complex Workflows](#tip-8-use-agent-skills-for-complex-workflows)
- [Tip 9: Use Claude Code for System Troubleshooting](#tip-9-use-claude-code-for-system-troubleshooting)
- [Tip 10: Dangerously Skip Permissions Mode](#tip-10-dangerously-skip-permissions-mode)
- [Tip 11: Headless & Print Mode for Automation](#tip-11-headless--print-mode-for-automation)
- [Tip 12: Use Plan Mode for Complex Tasks](#tip-12-use-plan-mode-for-complex-tasks)
- [Tip 13: Use Git Worktrees for Parallel Development](#tip-13-use-git-worktrees-for-parallel-development)
- [Tip 14: Clear Context Often with `/clear` and `/compact`](#tip-14-clear-context-often-with-clear-and-compact)
- [Tip 15: Use Hooks for Automated Actions](#tip-15-use-hooks-for-automated-actions)
- [Tip 16: Shell Commands with `!` (Bang Commands)](#tip-16-shell-commands-with--bang-commands)
- [Tip 17: Leverage All CLI Tools in Your PATH](#tip-17-leverage-all-cli-tools-in-your-path)
- [Tip 18: Use Images and Multimodal Input](#tip-18-use-images-and-multimodal-input)
- [Tip 19: Configure Settings with `/config` and `settings.json`](#tip-19-configure-settings-with-config-and-settingsjson)
- [Tip 20: Track Token Usage with `/cost`](#tip-20-track-token-usage-with-cost)
- [Tip 21: Switch Models with `/model`](#tip-21-switch-models-with-model)
- [Tip 22: Master Keyboard Shortcuts](#tip-22-master-keyboard-shortcuts)
- [Tip 23: Use Vim Mode for Efficient Editing](#tip-23-use-vim-mode-for-efficient-editing)
- [Tip 24: Leverage VS Code Integration](#tip-24-leverage-vs-code-integration)
- [Tip 25: Install the GitHub App for PR Reviews](#tip-25-install-the-github-app-for-pr-reviews)
- [Tip 26: Use Plugins for Extended Functionality](#tip-26-use-plugins-for-extended-functionality)
- [Tip 27: Pipe Input and Chain with Other CLIs](#tip-27-pipe-input-and-chain-with-other-clis)
- [Tip 28: Use Background Tasks for Long-Running Processes](#tip-28-use-background-tasks-for-long-running-processes)
- [Tip 29: Prompt History Search with Ctrl+R](#tip-29-prompt-history-search-with-ctrlr)
- [Tip 30: Use Todos for Task Management](#tip-30-use-todos-for-task-management)

---

## Getting Started

**Installation:** You can install Claude Code via npm. For a global install, use:

```bash
npm install -g @anthropic-ai/claude-code
```

Claude Code is available on all major platforms. Once installed, navigate to your project directory and run:

```bash
cd your-project
claude
```

You'll be prompted to log in on first use. That's it!

**Authentication:** Claude Code supports multiple authentication methods:
1. **Claude Pro/Max subscription** - Sign in with your Anthropic account for included usage
2. **API Key** - Set `ANTHROPIC_API_KEY` environment variable for pay-as-you-go usage

**Basic Usage:** To start an interactive session, just run `claude` with no arguments. You'll get a prompt where you can type requests. For instance:

```bash
$ claude
> Create a React recipe management app using SQLite
```

You can watch as Claude Code creates files, installs dependencies, runs tests, and more to fulfill your request. For a one-shot invocation (non-interactive), use the `-p` flag:

```bash
claude -p "Summarize the main points of README.md"
```

**CLI Interface:** Claude Code provides a rich terminal interface with:
- **Slash commands** (prefixed with `/`) for controlling sessions, tools, and settings
- **Bang commands** (prefixed with `!`) to execute shell commands directly
- **`@` mentions** for referencing files, directories, URLs, and agents
- **`#` prefix** for quick memory additions

By default, Claude Code operates in safe mode where modifications require confirmation. When a tool action is proposed, you'll see a diff or command and be prompted to approve or reject it.

---

## Tip 1: Use `CLAUDE.md` for Persistent Context

**Quick use-case:** Stop repeating yourself in prompts. Provide project-specific context or instructions by creating a `CLAUDE.md` file, so Claude always has important background knowledge without being told every time.

When working on a project, you often have certain overarching details—coding style guidelines, project architecture, or important facts—that you want Claude to keep in mind. Claude Code allows you to encode these in `CLAUDE.md` files. Simply create a Markdown file named `CLAUDE.md` at your project root with whatever notes or instructions you want Claude to persist:

```markdown
# Project Phoenix - AI Assistant

## Coding Standards
- All Python code must follow PEP 8 style
- Use 4 spaces for indentation
- Prefer functional programming paradigms

## Architecture
- Frontend: Next.js with TypeScript
- Backend: Node.js with Express
- Database: PostgreSQL with Prisma

## Testing
- Write tests for all new functions using Jest
- Tests alongside source files with `.test.ts` extension
```


For an example project overview and core guidelines, see [`CLAUDE-GLOBAL.md`](./CLAUDE-GLOBAL.md).




**How it works:** Claude Code uses a hierarchical context loading system with multiple levels:

| Level | Location | Scope |
|-------|----------|-------|
| User Memory | `~/.claude/CLAUDE.md` | All projects (global) |
| Project Memory | `./CLAUDE.md` | Current project (checked in) |
| Local Memory | `./CLAUDE.local.md` | Current project (gitignored) |
| Subdirectory | `./tests/CLAUDE.md` | Specific subdirectory |

More specific files override more general ones. You can inspect what context was loaded using `/memory` to edit all imported memory files.

**Pro Tip:** Use the `/init` slash command to quickly generate a starter `CLAUDE.md`. Running `/init` in a new project creates a template context file with information like the tech stack detected, a summary of the project, and more. You can then edit and expand that file.

For large projects, consider using `@import` syntax in your `CLAUDE.md` to pull in additional context files:

```markdown
@docs/api-guidelines.md
@docs/architecture.md
```

Skills work well with this too but for quick context, it's better to import the files for a quick addition.

With a well-crafted `CLAUDE.md`, you essentially give Claude Code a "memory" of the project's requirements and conventions.

---

## Tip 2: Create Custom Slash Commands

**Quick use-case:** Speed up repetitive tasks by defining your own slash commands. For example, create `/review` to run code reviews, `/test` to generate unit tests, or `/fix-issue` to analyze and fix GitHub issues.

Claude Code supports **custom slash commands** that you can define as Markdown files. Create a `commands/` directory in either:
- `.claude/commands/` for project-specific commands
- `~/.claude/commands/` for personal commands across all projects

**Example: Creating a `/review` command:**

```bash
mkdir -p .claude/commands
cat > .claude/commands/review.md << 'EOF'
Review this code for:
- Security vulnerabilities
- Performance issues  
- Code style violations
- Test coverage gaps

Provide actionable feedback with specific line references.
EOF
```

Now during a session, type `/review` to execute this prompt.

**Parameterized Commands:** Use `$ARGUMENTS` for dynamic input:

```bash
cat > .claude/commands/fix-issue.md << 'EOF'
Please analyze and fix the GitHub issue: $ARGUMENTS

Follow these steps:
1. Use `gh issue view` to get the issue details
2. Understand the problem described
3. Search the codebase for relevant files
4. Implement the necessary changes
5. Ensure code passes linting and type checking
6. Create a descriptive commit message
7. Push and create a PR
EOF
```

Use it: `/fix-issue 123`

**Advanced Frontmatter:** Commands support YAML frontmatter for additional configuration:

```markdown
---
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*)
description: Create a git commit with context
---

## Context
- Current status: !`git status`
- Current diff: !`git diff HEAD`
- Current branch: !`git branch --show-current`

Create a well-structured commit message based on the changes.
```

**Pro Tip:** Custom commands in `.claude/commands/` are automatically shared when team members clone your repository, creating consistent workflows across your entire team. The `/help` command shows all available slash commands, including custom ones.

---

## Tip 3: Extend Claude Code with MCP Servers

**Quick use-case:** Connect Claude Code to external systems—databases, APIs, design tools like Figma, project management like Jira—by configuring MCP (Model Context Protocol) servers. This lets Claude read from and write to systems beyond your local filesystem.

Claude Code supports MCP servers that provide additional tools and integrations. You can add servers using the CLI:

```bash
# Add a Puppeteer server for web automation
claude mcp add puppeteer -s user -- npx -y @modelcontextprotocol/server-puppeteer

# Add GitHub integration
claude mcp add github -- npx -y @modelcontextprotocol/server-github

# Add filesystem access (for specific directories)
claude mcp add filesystem -- npx -y @modelcontextprotocol/server-filesystem /path/to/allowed/dir

# Add Supabase integration
claude mcp add --transport stdio supabase \
  --env SUPABASE_ACCESS_TOKEN=YOUR_TOKEN \
  -- npx -y @supabase/mcp-server-supabase@latest
```

**Import from Claude Desktop:** If you've already configured MCP servers in Claude Desktop:

```bash
claude mcp add-from-claude-desktop
```

**Manage MCP Servers:**

```bash
# List all configured servers
claude mcp list

# View server details and health
/mcp   # In interactive mode

# Enable/disable servers by @-mentioning them
@github  # Toggle GitHub server
```

**Configuration in settings.json:**

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "your-token"
      }
    }
  }
}
```

**Popular MCP Servers:**
- **Puppeteer** - Web automation and testing
- **GitHub** - Repository operations, issues, PRs
- **Filesystem** - Controlled file access
- **Supabase** - Database operations
- **Context7** - Documentation and context management
- **AWS Serverless** - Lambda and serverless operations

**Pro Tip:** MCP servers can provide rich, multi-modal results including images and formatted data. They support OAuth 2.0 for secure API connections. You can create custom MCP servers for internal systems your team uses.

See [`USEFUL_MCP.md`](./USEFUL_MCP.md) for a list of useful MCP servers.

---

## Tip 4: Leverage Memory with `#` Quick Addition

**Quick use-case:** Instantly add important facts or preferences to Claude's memory by starting your message with `#`. This persists information across sessions without manually editing files.

Claude Code provides a quick memory system. Simply start a message with `#` to add it to memory:

```
# Always use TypeScript strict mode in this project
# The API endpoint is https://api.example.com/v2
# User prefers concise responses without excessive explanations
```

**Memory Commands:**

| Command | Action |
|---------|--------|
| `#your note` | Quick add to memory |
| `/memory` | Edit all imported memory files |
| View memory | Context shown with `/context` |

**Use Cases:**
- Store discovered configuration details: `# Database runs on port 5673`
- Record decisions: `# We chose React Query over SWR for data fetching`
- Save credentials/endpoints: `# Staging API key: xyz123`
- Personal preferences: `# Skip verbose explanations, show code directly`

**Pro Tip:** Memory additions are stored in your `CLAUDE.md` files (global or project-level depending on context). Use this for "decision logs"—when you agree on an approach, add it to memory so Claude doesn't contradict it later.

---

## Tip 5: Use Checkpoints and `/rewind` as an Undo Button

**Quick use-case:** If Claude makes changes you're not happy with, instantly roll back to a prior state. Claude Code automatically saves checkpoints before changes, and you can rewind with Escape-Escape or `/rewind`.

Claude Code's **checkpointing** feature acts as a safety net. The system automatically takes snapshots before each modification, allowing you to travel back in time if something goes wrong.

**How to Rewind:**
- **Double-tap Escape** - Quick rewind to last checkpoint
- **`/rewind`** - Open checkpoint selector
- **`/rewind <n>`** - Rewind to specific checkpoint

When you rewind, you can choose to restore:
- **Code only** - Reset files, keep conversation
- **Conversation only** - Keep files, reset chat context
- **Both** - Full restoration to checkpoint state

**Example:**

```
> Refactor the entire auth module to use JWT
[Claude makes extensive changes]
[You realize the approach isn't right]

> /rewind
0: [10:30:15] Before refactoring auth module
1: [10:45:02] Before modifying user service
> /rewind 0
[All changes reverted]
```

**Best Practices:**
- Checkpoints are enabled by default—let Claude attempt bold changes knowing you can rewind
- Use for exploration: "Try this risky refactor, I can always rewind"
- Still use git for actual version control; checkpoints are for quick undo during development

**Pro Tip:** Checkpoints are especially powerful combined with subagents and autonomous work. You can let Claude explore aggressively, knowing you have a safety net.

---

## Tip 6: Create and Use Subagents for Parallel Work

**Quick use-case:** Delegate specialized tasks to subagents that run with their own context windows. Use them for parallel research, specialized code review, or any task that benefits from focused expertise.

Subagents are specialized AI assistants that Claude Code can invoke. Each subagent has:
- A custom system prompt guiding its behavior
- Its own context window (prevents pollution of main conversation)
- Specific tool access permissions
- A designated model (Opus, Sonnet, or Haiku)

**Creating a Subagent:**

```bash
mkdir -p .claude/agents
cat > .claude/agents/security-reviewer.md << 'EOF'
---
name: security-reviewer
description: Expert security code reviewer. Use proactively after code changes.
tools: Read, Grep, Glob, Bash
model: sonnet
---

You are a senior security engineer specializing in code audits.

When invoked:
1. Analyze code for common vulnerabilities (OWASP Top 10)
2. Check for injection risks, auth issues, data exposure
3. Review dependency security
4. Provide severity ratings and remediation steps

Focus on actionable findings. Skip minor style issues.
EOF
```

**Using Subagents:**

```
> @security-reviewer Review the changes in the auth module
```

Claude can also invoke subagents automatically when appropriate based on the task.

**Built-in Plan Subagent:** When you're in plan mode, Claude automatically uses a "Plan" subagent to research your codebase before creating plans. This prevents infinite nesting while gathering necessary context.

**Parallel Exploration:** Subagents can run in parallel for faster exploration:

```
> Explore the codebase using 4 tasks in parallel.
> Each agent should explore different directories.
```

**Practical Examples:**
- **Code Reviewer** - Automated PR reviews
- **Data Analyst** - SQL queries and BigQuery analysis  
- **Test Writer** - Specialized test generation
- **Documentation Writer** - API docs and READMEs
- **Debugger** - Error analysis and root cause identification

**Pro Tip:** Each subagent gets its own context window, making this a way to gain additional context capacity for large codebases. Start with Claude-generated agents using `/agents`, then iterate to customize them.

---

## Tip 7: Reference Files with `@` for Explicit Context

**Quick use-case:** Instead of describing file contents verbally, point Claude directly to them. Using `@` syntax, you attach files, directories, images, or URLs into your prompt, ensuring Claude sees exactly what's in those resources.

```
> Explain this code to me: @src/main.js
> Compare @foo.py and @bar.py and tell me the differences
> Review all files in @./utils/
```

**What You Can Reference:**

| Syntax | What it references |
|--------|-------------------|
| `@file.ts` | Single file |
| `@./directory/` | Entire directory (recursive) |
| `@https://example.com` | URL content |
| `@image.png` | Images (multimodal) |
| `@agent-name` | Invoke a subagent |
| `@mcp-server` | Toggle MCP server |

**Key Benefits:**
- **Precision** - Claude reads the actual file, not your summary
- **Large context** - Claude can handle substantial files
- **Automatic ignoring** - Respects `.gitignore` and `.claudeignore`

**Pro Tip:** Use `@` for explicit context injection rather than relying on memory or summarizing yourself. It's more precise and prevents hallucination. You can include multiple files in one prompt:

```
> Given @package.json, @tsconfig.json, and @src/index.ts, 
> explain the project setup and suggest improvements
```

---

## Tip 8: Use Agent Skills for Complex Workflows

**Quick use-case:** Skills are modular capabilities that Claude automatically invokes based on context. Unlike slash commands (user-invoked), Skills are model-invoked—Claude uses them autonomously when relevant.

Skills extend Claude's functionality with comprehensive workflows including instructions, scripts, and resources. They're stored in:
- `.claude/skills/` - Project skills
- `~/.claude/skills/` - Personal skills

**Creating a Skill:**

```bash
mkdir -p .claude/skills/commit-messages
cat > .claude/skills/commit-messages/SKILL.md << 'EOF'
---
name: generating-commit-messages
description: Generates clear commit messages from git diffs. Use when writing commit messages or reviewing staged changes.
---

# Generating Commit Messages

## Instructions
1. Run `git diff --staged` to see changes
2. Analyze the nature of changes
3. Generate a commit message with:
   - Summary under 50 characters
   - Detailed description if needed
   - Affected components listed

## Best Practices
- Use present tense ("Add feature" not "Added feature")
- Explain what and why, not how
- Reference issue numbers when applicable
EOF
```

**Key Differences from Slash Commands:**

| Feature | Slash Commands | Agent Skills |
|---------|---------------|--------------|
| Invocation | User types `/command` | Claude invokes automatically |
| Complexity | Simple prompts | Multi-file with scripts |
| Structure | Single `.md` file | Directory with `SKILL.md` + resources |
| Use case | Quick prompts | Comprehensive workflows |

**Installing Skills:**
```bash
# Install from marketplace
/plugin install skill-name

# Skills are auto-discovered in .claude/skills/
```

**Pro Tip:** Use the built-in "skill-creator" skill to help create new skills interactively. Claude asks about your workflow, generates the folder structure, and formats the SKILL.md file.

---

## Tip 9: Use Claude Code for System Troubleshooting

**Quick use-case:** Use Claude Code outside of code projects to help with general system tasks. Fix shell configurations, diagnose errors, customize your dev environment, or automate setup processes.

Claude Code isn't just for coding projects—it's an AI helper for your entire development environment:

**Editing Dotfiles:**
```
> My PATH isn't picking up Go binaries. Fix my ~/.bashrc
> Optimize my .zshrc for better performance
> Add alias for common git commands to my shell config
```

**Diagnosing Errors:**
```
> When I run npm install, I get EACCES permission error. How do I fix this?
> This error message appeared: [paste error]. What does it mean?
> My Docker container keeps crashing. Here's the log: @container.log
```

**System Configuration:**
```
> Install Docker on my system
> Configure Git to sign commits with GPG
> Set up SSH key for GitHub
```

**Pro Tip:** For system-level tasks, Claude can run shell commands and analyze outputs. Always review proposed commands before approving, especially for operations with system-wide impact. You can ask Claude to explain a command before running it.

---

## Tip 10: Dangerously Skip Permissions Mode

**Quick use-case:** When you trust the AI's actions and want maximum speed, bypass permission checks so Claude can work uninterrupted. Use this in containers or for low-risk workflows.

```bash
claude --dangerously-skip-permissions
```

This flag bypasses all permission prompts, letting Claude execute tool actions immediately without confirmation. It's useful for:
- Fixing lint errors automatically
- Generating boilerplate code
- Running in CI/CD pipelines
- Automated batch operations

**⚠️ Big Warning:** This mode is risky and can result in:
- Data loss
- System corruption  
- Potential data exfiltration (via prompt injection)

**Safe Usage:**
1. **Use in containers** - Run in Docker without internet access
2. **Use for low-risk tasks** - Lint fixes, formatting, boilerplate
3. **Have backups** - Git commits, checkpoints enabled

**Docker Reference Implementation:**
```bash
# Run in isolated container
docker run -it --rm \
  -v $(pwd):/workspace \
  --network none \
  claude-code-image \
  claude --dangerously-skip-permissions
```

**Pro Tip:** For a middle ground, consider configuring specific commands to auto-approve in settings rather than using full skip-permissions mode.

---

## Tip 11: Headless & Print Mode for Automation

**Quick use-case:** Use Claude Code in scripts or automation by running it in print mode (`-p`). This outputs a response and exits, perfect for CI/CD, scheduled tasks, or piping to other tools.

```bash
# Single question, get answer
claude -p "Summarize the main changes in the last 5 commits"

# Output in JSON format
claude -p "List all TODO comments" --output-format json

# Pipe input
cat error.log | claude -p "What went wrong?"
echo "Count to 10" | claude

# Process files
claude -p "Analyze this CSV and find anomalies" < data.csv
```

**Output Formats:**
- Default: Human-readable text
- `--output-format json`: Structured JSON (for programmatic parsing)
- `--output-format stream-json`: Streaming JSON

**CI/CD Integration:**

```yaml
# GitHub Action example
- name: Generate Changelog
  run: |
    changelog=$(claude -p "Generate changelog from commits since last tag")
    echo "$changelog" >> CHANGELOG.md
```

**Custom System Prompts:**

```bash
# Override system prompt entirely
claude -p "Perform task X" --system-prompt "You are a strict code reviewer..."

# Load system prompt from file
claude -p "Review this" --system-prompt-file ./prompts/reviewer.md

# Append to default system prompt
claude -p "Help me" --append-system-prompt "Always respond in Spanish"
```

**Pro Tip:** Use `--append-system-prompt` to preserve Claude Code's built-in capabilities while adding custom requirements. Use `--system-prompt` only when you need complete control.

---

## Tip 12: Use Plan Mode for Complex Tasks

**Quick use-case:** For complex multi-step tasks, use plan mode to have Claude research and create a detailed plan before executing. This prevents rushing into implementation without proper understanding.

Start plan mode:
```bash
claude --plan
# or interactively
/plan
```

In plan mode:
1. Claude researches your codebase using the Plan subagent
2. Creates a detailed implementation plan
3. Gets your approval before executing
4. Follows the plan systematically

**Example:**
```
> [In plan mode] Help me refactor the authentication module

Claude: Let me research your authentication implementation first...
[Plan subagent explores auth-related files]
[Returns findings]

Claude: Based on my research, here's my proposed plan:
1. Extract auth logic into separate service
2. Implement token refresh mechanism
3. Add rate limiting
4. Update tests
...

Do you want me to proceed with this plan?
```

**Hybrid Strategies:**

```bash
# Use different models for planning vs execution
claude --model opus-plan    # Opus for planning, Sonnet for execution
claude --model sonnet-plan  # Sonnet for planning, Haiku for execution
```

**Pro Tip:** Plan mode is excellent for tasks where you want to review the approach before any code changes. Use it for refactors, new features, or any task where understanding the scope upfront is valuable.

---

## Tip 13: Use Git Worktrees for Parallel Development

**Quick use-case:** Run multiple Claude Code instances on the same repository by using git worktrees. Each instance works on a separate branch in an isolated directory, enabling true parallel development.

```bash
# Create worktrees for different features
git worktree add ../myapp-auth -b feature/auth main
git worktree add ../myapp-api -b feature/api main

# Run Claude in each
cd ../myapp-auth && claude  # Terminal 1
cd ../myapp-api && claude   # Terminal 2
```

**Use Cases:**

**Parallel Feature Development:**
```bash
git worktree add ../myapp-frontend -b feature/ui main
git worktree add ../myapp-backend -b feature/api main
# Two Claude instances build different components simultaneously
```

**Safe Experimentation:**
```bash
git worktree add ../myapp-experiment -b experiment/refactor main
cd ../myapp-experiment
claude "refactor the entire auth system"
# If it fails, just delete the directory - main is untouched
```

**Model Comparison:**
```bash
git worktree add ../ml-sonnet -b experiment/sonnet main
git worktree add ../ml-opus -b experiment/opus main
cd ../ml-sonnet && claude --model sonnet
cd ../ml-opus && claude --model opus
# Compare approaches from different models
```

**Hotfix While Working:**
```bash
git worktree add ../myapp-hotfix -b hotfix/critical main
cd ../myapp-hotfix
claude "fix the critical bug"
# Original feature work continues in main directory
```

**Pro Tip:** Worktrees are the safest way to run multiple Claude instances because each has complete file isolation. No merge conflicts, no stepping on each other's changes.

---

## Tip 14: Clear Context Often with `/clear` and `/compact`

**Quick use-case:** Prevent context window overflow and keep Claude focused by regularly clearing or compacting your conversation. Every time you start something new, clear the chat.

**Commands:**

| Command | Action |
|---------|--------|
| `/clear` | Reset conversation history completely |
| `/compact` | Summarize conversation, free up tokens |
| `/context` | View current context usage |

**When to Clear:**
- Starting a new task or topic
- Context window warning appears
- Claude seems confused about earlier context
- Switching from exploration to implementation

**When to Compact:**
- Long session you want to preserve
- Need more context space but want to keep key points
- Before starting a complex task in existing session

**Context Management Tips:**
1. **Use `/clear` liberally** - Old conversation history eats tokens
2. **Check `/context`** - Monitor how much context is used
3. **Leverage CLAUDE.md** - Persistent context doesn't count against conversation
4. **Use subagents** - They have separate context windows

**Pro Tip:** If Claude runs a "compaction call" automatically (summarizing to free context), you may lose important details. Proactively manage context to avoid automatic compaction at inopportune times.

---

## Tip 15: Use Hooks for Automated Actions

**Quick use-case:** Configure hooks to automatically run commands at specific points—format code before edits are accepted, run tests after changes, lint before commits, or send notifications.

Hooks are configured in your settings files and execute shell commands automatically:

**Configuration:**
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write",
        "hooks": [
          {
            "type": "command",
            "command": "prettier --check $CLAUDE_FILE_PATH"
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write",
        "hooks": [
          {
            "type": "command",
            "command": "eslint --fix $CLAUDE_FILE_PATH"
          }
        ]
      }
    ]
  }
}
```

**Hook Events:**
- `PreToolUse` - Before a tool executes
- `PostToolUse` - After a tool executes

**Matcher Patterns:**
- Simple string: `"Write"` matches the Write tool
- Wildcard: `"*"` matches all tools
- Empty string: Matches everything

**Environment Variables:**
- `$CLAUDE_FILE_PATH` - Path of the file being operated on
- `$CLAUDE_PROJECT_DIR` - Absolute path to project root

**Example Hooks:**
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write",
        "hooks": [
          {"type": "command", "command": "npx prettier --write $CLAUDE_FILE_PATH"},
          {"type": "command", "command": "npx eslint $CLAUDE_FILE_PATH --fix"}
        ]
      }
    ]
  }
}
```

**⚠️ Warning:** Hooks execute arbitrary shell commands automatically. Test hooks in a safe environment before production use. All matching hooks run in parallel with a 60-second default timeout.

**Pro Tip:** Ask Claude to help set up hooks for your project. It can analyze your workflow and create appropriate hook configurations.

---

## Tip 16: Shell Commands with `!` (Bang Commands)

**Quick use-case:** Execute shell commands directly from Claude Code without leaving the session. Use `!` prefix for quick commands or enter shell mode for multiple commands.

**Single Command:**
```
> !git status
> !npm test
> !ls -la src/
```

**Shell Mode (persistent):**
```
> !
shell> pwd
/home/user/project
shell> npm run build
shell> git log --oneline -5
shell> !
[Back to Claude prompt]
```

**Dynamic Command Execution in CLAUDE.md:**
```markdown
## Context
- Current status: !`git status`
- Current diff: !`git diff HEAD`
- Current branch: !`git branch --show-current`
```

**Use Cases:**
- Run tests to verify changes: `!npm test`
- Check git status during development: `!git status`
- Build and verify: `!npm run build`
- Execute one-off scripts: `!python analyze.py`

**Pro Tip:** The output from `!` commands appears in the conversation context. You can immediately ask Claude to analyze the output: run `!npm test`, see failures, then say "Fix these test failures."

---

## Tip 17: Leverage All CLI Tools in Your PATH

**Quick use-case:** Claude Code can use any command-line tool installed on your system. Your entire `$PATH` is the AI's toolkit—ImageMagick, ffmpeg, Docker, AWS CLI, and more.

Claude has access to all shell commands available in your environment:

```
> Convert all PNG images in this folder to WebP format
[Claude uses ImageMagick's convert command]

> Deploy my app to Docker
[Claude runs docker build and docker run]

> Extract audio from video.mp4
[Claude uses ffmpeg]

> Query my DynamoDB table for recent entries
[Claude uses AWS CLI]
```

**What This Means:**
- Any CLI tool can be used as an "AI tool"
- Claude knows common syntax and can read `--help` output
- You can install tools specifically to give Claude more capabilities

**Example: Database Operations**
```
> Run a query against the production database
[Claude uses psql, mysql, or appropriate CLI]

> Export this MongoDB collection to JSON
[Claude uses mongodump or mongoexport]
```

**Pro Tip:** If a built-in capability doesn't exist, Claude can often accomplish the task using available CLI tools. Think of Claude as having access to every tool in your development environment.

---

## Tip 18: Use Images and Multimodal Input

**Quick use-case:** Claude Code understands images. Drag and drop screenshots, paste from clipboard, or reference image files to get help with UI mockups, error dialogs, diagrams, and more.

**Methods to Add Images:**
1. **Paste from clipboard**: `Ctrl+V` (or `Cmd+Ctrl+Shift+4` on macOS to screenshot directly to clipboard)
2. **Drag and drop**: Drag image files into the prompt
3. **Reference with @**: `@screenshot.png`

**Use Cases:**

**UI Development:**
```
> Here's a screenshot of the design mockup: @design.png
> Create a React component that matches this layout
```

**Error Diagnosis:**
```
> [paste screenshot of error dialog]
> What does this error mean and how do I fix it?
```

**Code Review:**
```
> Here's a diagram of the current architecture: @arch.png
> Does our implementation match this design?
```

**OCR and Data Extraction:**
```
> Extract the text from this image: @document.jpg
> Convert this table screenshot to markdown: @table.png
```

**Pro Tip:** This is particularly useful when working with design mocks as reference for UI development. Claude can analyze layouts, colors, spacing, and generate matching code.

---

## Tip 19: Configure Settings with `/config` and `settings.json`

**Quick use-case:** Customize Claude Code's behavior—default model, theme, permissions, tool access, and more—through the interactive settings interface or direct file editing.

**Access Settings:**
```
> /config   # Open interactive configuration
> /settings # Same as /config
```

**Settings Files:**
- `~/.claude/settings.json` - Global (all projects)
- `.claude/settings.json` - Project-specific
- `.claude/settings.local.json` - Local only (gitignored)

**Common Settings:**
```json
{
  "model": "claude-sonnet-4-20250514",
  "theme": "dark",
  "vimMode": true,
  "autoCompact": true,
  "permissions": {
    "autoApprove": ["Read", "Glob", "Grep"],
    "deny": []
  }
}
```

**Useful Commands:**
```bash
# Set default model (project)
claude config set model claude-sonnet-4-20250514

# Set default model (global)
claude config set --global model claude-sonnet-4-20250514

# View all settings
claude config list

# View global settings
claude config list --global
```

**Pro Tip:** Use project-level settings to enforce team conventions (model choice, tool permissions) and global settings for personal preferences (theme, vim mode).

---

## Tip 20: Track Token Usage with `/cost`

**Quick use-case:** Monitor your token consumption and costs during a session. Understand how much context you're using and optimize for efficiency.

```
> /cost
```

Shows:
- Tokens used this session
- Estimated cost (for API users)
- Context window usage percentage

**For Subscription Users:**
- Claude Pro: ~$20/month for medium-high workload
- Claude Max5: 5x token allowance, Opus access ($100/month)
- Claude Max20: 20x token allowance, larger context ($200/month)

**Cost Optimization Tips:**
1. Use `/clear` between unrelated tasks
2. Use Sonnet for simpler tasks, Opus for complex ones
3. Leverage CLAUDE.md instead of repeating context
4. Use subagents (separate context windows)
5. Consider headless mode for batch operations

**Pro Tip:** Configure OpenTelemetry for detailed cost tracking in team environments. Send metrics to DataDog or other backends for monitoring.

---

## Tip 21: Switch Models with `/model`

**Quick use-case:** Switch between Claude models on the fly. Use Opus for complex reasoning, Sonnet for balanced performance, Haiku for speed.

```
> /model
```

Opens interactive selector for:
- **Claude Opus 4/4.5** - Most capable, best for complex tasks
- **Claude Sonnet 4/4.5** - Balanced performance (default)
- **Claude Haiku 4.5** - Fastest, good for simple tasks

**Hybrid Strategies:**
- **OpusPlan** - Use Opus for planning, Sonnet for execution
- **SonnetPlan** - Use Sonnet for planning, Haiku for execution

**Command Line:**
```bash
claude --model opus
claude --model sonnet
claude --model haiku
```

**Environment Variables:**
```bash
export ANTHROPIC_DEFAULT_SONNET_MODEL=claude-sonnet-4-5-20250929
export ANTHROPIC_DEFAULT_OPUS_MODEL=claude-opus-4-5-20251101
```

**Pro Tip:** The default behavior uses Opus until you hit 50% usage, then switches to Sonnet for cost efficiency. Most users should stick with defaults unless they have specific needs.

---

## Tip 22: Master Keyboard Shortcuts

**Quick use-case:** Navigate Claude Code efficiently with keyboard shortcuts. Interrupt operations, toggle modes, manage history, and more without leaving the keyboard.

**Universal Shortcuts:**

| Shortcut | Action |
|----------|--------|
| `Ctrl+C` | Cancel current operation / clear input |
| `Ctrl+C Ctrl+C` | Exit Claude Code entirely |
| `Escape` | Cancel generation / exit menus |
| `Escape Escape` | Quick rewind to last checkpoint |
| `Ctrl+R` | Search prompt history |
| `Ctrl+D` | Exit (at empty prompt) |

**Navigation:**

| Shortcut | Action |
|----------|--------|
| `Up/Down` | Navigate history |
| `Tab` | Autocomplete commands/files |
| `Ctrl+L` | Clear screen |

**Input Modes:**

| Shortcut | Action |
|----------|--------|
| `Ctrl+E` | Open prompt in external editor |
| `Ctrl+Y` | Toggle YOLO mode (auto-approve) |

**Pro Tip:** Use `Esc` to exit shell mode and return to Claude's chat mode. Double-escape is your quick undo button for when Claude's changes need reverting.

---

## Tip 23: Use Vim Mode for Efficient Editing

**Quick use-case:** Enable Vim keybindings in Claude Code for familiar modal editing in your prompts.

Enable vim mode:
```
> /vim
```

Or in settings:
```json
{
  "vimMode": true
}
```

**Vim Mode Features:**
- Normal, insert, and visual modes
- Standard vim navigation (h, j, k, l)
- Common vim commands (dd, yy, p, etc.)
- Works in prompt input

**Pro Tip:** If you're a vim user, this makes editing longer prompts much more efficient. Combined with `Ctrl+E` to open in external editor, you have full editing power.

---

## Tip 24: Leverage VS Code Integration

**Quick use-case:** Get a graphical interface with inline diffs and real-time change tracking by using the VS Code extension instead of (or alongside) the terminal.

**Installation:**
1. Install "Claude Code" from VS Code Extension Marketplace
2. Or run `/ide install` from Claude Code terminal

**Features:**
- Sidebar panel for conversation
- Inline diffs showing proposed changes
- Real-time progress tracking
- Click to accept/reject changes
- Context from open files

**From Terminal:**
```
> /ide install   # Install extension
> /ide enable    # Enable integration
> /ide status    # Check connection
```

**Pro Tip:** The extension provides a richer experience for users who prefer IDEs over terminals. You can use both—terminal for quick operations, VS Code for reviewing complex changes.

---

## Tip 25: Install the GitHub App for PR Reviews

**Quick use-case:** Have Claude automatically review your pull requests, catching bugs and issues that human reviewers often miss.

```
> /install-github-app
```

This installs a GitHub App that automatically reviews PRs in your repository. Claude will:
- Analyze code changes
- Find logic errors and security issues
- Comment on potential bugs
- Suggest improvements

**Customization:**
After installation, Claude creates a `claude-code-review.yml` file with the review prompt. Edit this to customize what Claude focuses on:

```yaml
# claude-code-review.yml
prompt: |
  Focus on:
  - Security vulnerabilities
  - Performance issues
  - Breaking changes
  Skip:
  - Style nitpicks
  - Minor formatting
```

**Pro Tip:** The default review prompt is often too verbose. Customize it to focus on what matters—Claude finds actual logic errors and security issues, not just variable naming nitpicks.

---

## Tip 26: Use Plugins for Extended Functionality

**Quick use-case:** Install plugins to add new agents, commands, skills, hooks, and MCP servers. Plugins are shared through marketplaces and can extend Claude Code dramatically.

**Plugin Commands:**
```
> /plugin install plugin-name    # Install from marketplace
> /plugin list                   # List installed plugins
> /plugin remove plugin-name     # Uninstall
```

**What Plugins Provide:**
- Custom subagents
- Slash commands
- Agent skills
- Hooks
- MCP server configurations

**Popular Plugin Categories:**
- **Development**: Python, JavaScript/TypeScript, Backend APIs
- **Infrastructure**: Kubernetes, AWS, Azure, GCP
- **Security**: SAST scanning, vulnerability detection
- **Quality**: Code review, testing frameworks

**Configuration:**
Plugins are configured via `marketplace.json` and can be shared at repository level using `extraKnownMarketplaces` configuration.

**Pro Tip:** Each installed plugin loads only its specific context into Claude, keeping token usage efficient. Install only what you need for your current project.

---

## Tip 27: Pipe Input and Chain with Other CLIs

**Quick use-case:** Compose Claude Code with other Unix tools. Pipe data in for analysis, pipe output to other commands, or chain multiple operations together.

**Piping Input:**
```bash
# Analyze log file
cat error.log | claude -p "What errors occurred?"

# Process CSV
cat data.csv | claude -p "Who has the highest score?"

# Analyze git diff
git diff | claude -p "Summarize these changes"

# Review dependencies
npm outdated | claude -p "Which updates are safe to apply?"
```

**Piping Output:**
```bash
# Generate and save directly
claude -p "Write a Python script for..." > script.py

# Generate and process
claude -p "List all API endpoints" | grep POST

# Chain operations
claude -p "Generate test data" | jq '.users[]' > users.json
```

**Combining with Standard Tools:**
```bash
# Find TODOs and analyze
grep -r "TODO" ./src | claude -p "Prioritize these TODOs"

# Analyze test coverage
npm test -- --coverage | claude -p "What areas need more tests?"
```

**Pro Tip:** This transforms Claude Code into a component that integrates with your existing shell workflows. Think of it as a smart filter or transformer in your pipeline.

---

## Tip 28: Use Background Tasks for Long-Running Processes

**Quick use-case:** Keep dev servers, watchers, and other long-running processes active in the background while Claude continues working on other tasks.

Claude Code can manage background tasks that would otherwise block progress:

```
> Start the dev server and keep it running while we work on features
[Claude starts server as background task]

> Run the test watcher
[Tests run continuously in background]
```

**Features:**
- Background tasks don't block Claude's main work
- Output is captured and accessible
- Tasks persist until explicitly stopped
- Multiple background tasks can run simultaneously

**Use Cases:**
- Development servers (`npm run dev`)
- File watchers
- Test runners in watch mode
- Build processes
- Database connections

**Pro Tip:** Combined with subagents and checkpoints, background tasks enable complex autonomous workflows where Claude can run tests, see results, make changes, and iterate—all while keeping services running.

---

## Tip 29: Prompt History Search with Ctrl+R

**Quick use-case:** Search through your previous prompts to quickly find and reuse past commands, saving time on repetitive requests.

Press `Ctrl+R` to open the searchable prompt history:

```
> [Ctrl+R]
(reverse-i-search): refactor
> Refactor the auth module to use JWT tokens
```

**Features:**
- Fuzzy search through all previous prompts
- Works across sessions
- Edit and re-execute found prompts
- Navigate with arrow keys

**Pro Tip:** This is especially useful for complex prompts you've refined over time. Instead of retyping, search for key terms and modify as needed.

---

## Tip 30: Use Todos for Task Management

**Quick use-case:** Maintain task lists that persist across conversation sessions. Claude can add, check off, and reference todos throughout your development workflow.

```
> /todos              # View all todos
> /todos add "Fix auth bug"
> /todos check 1      # Mark as complete
```

**Features:**
- Persistent across sessions
- Claude can reference and update todos
- Integration with memory system
- Track progress on multi-step tasks

**Use in Workflows:**
```
> Before we start, add these tasks:
> 1. Implement user authentication
> 2. Add rate limiting
> 3. Write integration tests
> 4. Update documentation

[Claude adds to todos]

> Let's work on task 1
[Claude works on it]

> Mark task 1 as done and show remaining tasks
```

**Pro Tip:** Use todos for complex multi-session projects. When you return to a project after a break, `/todos` shows you exactly where you left off.

---

## Quick Reference: Essential Commands

### Session Management
| Command | Action |
|---------|--------|
| `/clear` | Reset conversation history |
| `/compact` | Compress conversation |
| `/exit` | End session |
| `/help` | Show all commands |

### Context & Memory
| Command | Action |
|---------|--------|
| `/init` | Generate CLAUDE.md |
| `/memory` | Edit memory files |
| `/context` | View context usage |
| `#note` | Quick add to memory |

### Models & Config
| Command | Action |
|---------|--------|
| `/model` | Switch AI model |
| `/config` | Open settings |
| `/cost` | Show token usage |
| `/doctor` | Diagnose issues |

### Development
| Command | Action |
|---------|--------|
| `/rewind` | Restore checkpoint |
| `/plan` | Enter plan mode |
| `/vim` | Toggle vim mode |
| `/todos` | View task list |

### Tools & Integration
| Command | Action |
|---------|--------|
| `/mcp` | Manage MCP servers |
| `/agents` | Manage subagents |
| `/plugin` | Manage plugins |
| `/install-github-app` | Install PR reviewer |

---

## Further Resources

- **Documentation**: [docs.anthropic.com/claude-code](https://docs.anthropic.com/en/docs/claude-code/overview)
- **Best Practices**: [anthropic.com/engineering/claude-code-best-practices](https://www.anthropic.com/engineering/claude-code-best-practices)
- **Agent SDK**: [anthropic.com/engineering/building-agents-with-the-claude-agent-sdk](https://www.anthropic.com/engineering/building-agents-with-the-claude-agent-sdk)
- **GitHub App**: [github.com/apps/claude-code](https://github.com/apps/claude-code)
- **Awesome Claude Code**: [github.com/hesreallyhim/awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code)

---

*This guide is adapted for Claude Code CLI from similar community resources. Claude Code is actively developed—check the official documentation for the latest features and updates.*