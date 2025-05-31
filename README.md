# AIV - AI Valve: Pipes for AI

AIV is a shell-based command-line utility designed for seamless integration with text editors and terminal workflows. It allows you to interact with AI models through Unix pipes while maintaining conversation context and providing intelligent code location detection.

## Features

- **Editor-optimized design** for seamless integration with Helix and other editors
- **Terminal-friendly interface** for command-line workflows and automation
- **Smart context detection** that automatically identifies source files and line locations
- **Conversation history** with thread continuation support
- **Pipe-friendly interface** that works with any Unix tool
- **Flexible input handling** supporting files, globs, and stdin
- **Real-time conversation management** with persistent state

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/o8vm/aiv.git
   ```

2. Make the script executable:
   ```bash
   chmod +x aiv
   ```

3. Create a configuration directory and file:
   ```bash
   mkdir -p ~/.config/aiv
   ```

4. Configure your API settings in `~/.config/aiv/config`:
   ```
   API_KEY="your_anthropic_api_key"
   MODEL="claude-sonnet-4-20250514"
   MAX_TOKENS=4096
   SYS_PROMPT="You are an expert programmer and a shell master and an expert support engineer. You value code efficiency and clarity above all things. What you write will be piped in and out of cli programs so you do not explain anything unless explicitly asked to. Never write ``` around your answer, provide only the result of the task you are given. Preserve input formatting."
   ```

## Usage

```
aiv [options] [prompt]
```

### Options

- `-c [pattern|-]`: Add context from files (glob pattern) or stdin (-)
- `-r`: Repeat the input before output (useful for editor insertion)
- `-e`: Continue previous conversation thread
- `-m MODEL`: Use specified model
- `-s [prompt]`: Override system prompt
- `-h, -v`: Display help/version information

## Examples

### Basic Queries
```bash
# Simple question
aiv "What is the difference between TCP and UDP?"

# Get help with shell commands
aiv "How do I find files modified in the last 24 hours?"

# Code explanation
aiv "Explain what this regex does: '^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'"
```

### Working with Files
```bash
# Analyze a single file
aiv -c main.py "What does this script do?"

# Review multiple files
aiv -c "src/*.js" "Find potential security issues in these files"

# Compare implementations
aiv -c old_version.py -c new_version.py "What are the key differences?"
```

### Pipeline Integration
```bash
# Analyze log files
tail -f /var/log/nginx/error.log | aiv "Summarize these errors"

# Process command output
ps aux | head -20 | aiv "Explain what these processes are doing"

# Git integration
git diff HEAD~1 | aiv "Review this commit for potential issues"

# System analysis
df -h | aiv "Analyze disk usage and suggest optimizations"
```

### Conversation Workflows
```bash
# Start a technical discussion
aiv "I need to design a caching system for a web API"

# Continue the conversation
aiv -e "What about using Redis vs in-memory caching?"

# Add specific context
aiv -c current_api.py -e "How would this integrate with my existing API?"

# Get implementation details
aiv -e "Show me Python code for the Redis implementation"
```

### Multi-context Analysis
```bash
# Analyze function within file context
cat main.rs | grep -A 20 'fn hoge()' | aiv -c main.rs "Explain this function"

# Combine different data sources
cat error.log | aiv -c "src/*.py" -c config.yaml "Why am I getting these errors?"

# Add context and generate
cat hoge.rs | aiv -c - -c "src/*.rs"
aiv -e "explain this context"
```

### Debugging Session
```bash
# Start debugging session
echo "This function isn't working as expected" | aiv -c buggy_code.py -c -

# Add test output
python test.py 2>&1 | aiv -c - -e

# Continue troubleshooting
aiv -e "This is test output. What specific changes should I make?"
```

## Editor Integration (Helix)

AIV is optimized for Helix editor workflows:

### 1. Add Selected Text to Context (`Alt+|`)
Select code in Helix and use pipe-ignore to add context without output:
```bash
aiv -c - [-e]
```

### 2. Generate Code After Selection (`|`)
After adding context, generate new content that gets inserted after your selection:
```bash
aiv -r -e "generate unit tests for this function"
```

### 3. Replace Selection with AI Response (`|`)
Replace selected text with AI-generated content:
```bash
aiv -e "refactor this code for better performance"
```

### 4. Shell Command Integration (`!`)
Use shell commands to generate content at cursor position:
```bash
aiv [-e] [-c "./*"] "create a README for this project"
```

## Mechanism

### Smart Location Detection

When you pipe code through AIV, it automatically:
- Identifies the source file using content matching
- Finds the exact line range in the file
- Adds location context like `[src/main.rs:45:67]`

This helps the AI provide more precise, location-aware assistance.

### File Management

- **Conversation history**: `~/.config/aiv/conversation`
- **Configuration**: `~/.config/aiv/config`

The conversation file maintains context between sessions, allowing you to continue discussions across terminal and editor sessions.

## License

MIT License

---

AIV transforms both your terminal and editor into an AI-powered development environment, making it easy to get contextual assistance without disrupting your workflow.
