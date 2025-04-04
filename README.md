# AIV - AI Valve: Pipes for AI

AIV is a command-line utility that allows you to interact with AI models through Unix pipes. It provides a simple and flexible interface for sending prompts to AI models and receiving responses directly within your terminal.

## Features

- Pipe-friendly design for seamless integration with Unix tools
- Context management with support for files and stdin
- Conversation history support for multi-turn interactions
- Customizable system prompts
- Model selection options

## Installation

1. Clone the repository:
   ```
   git clone https://github.com/o8vm/aiv.git
   ```

2. Make the script executable:
   ```
   chmod +x aiv
   ```

3. Create a configuration file at `~/.config/aiv/config` with your API key:
   ```
   API_KEY="your_anthropic_api_key"
   MODEL="claude-3-7-sonnet-latest"
   MAX_TOKENS=4096
   SYS_PROMPT="You are a helpful AI assistant."
   ```

## Usage

```
aiv [options] [prompt]
```

### Options

- `-c [file_pattern|-]`: Add context from files (glob pattern) or stdin (-)
- `-r`: Repeat the input before output
- `-e`: Continue previous conversation thread
- `-m MODEL`: Use specified model (default: claude-3.7-sonnet-latest)
- `-h`: Display help message
- `-s [prompt]`: Overwrite system prompt
- `-v`: Display version information

### Examples

Basic usage:
```shell
echo "What is Unix?" | aiv
```

Adding context from a file:
```shell
aiv -c mycode.js "Explain this code"
```

Adding context from stdin and files:
```shell
cat mydata.txt | aiv -c - -c file1.txt -c file2.txt "Summarize this data"
```

Continue a conversation:
```shell
aiv -e "Tell me more about that"
```

### Editor Integration

In an editor, you can add the selected text to the conversation context.
The `-e` option maintains the previous conversation thread:
You can use the pipe function of your editor which ignores output:
```shell
aiv -c - -e 
```

After adding context, you can generate new content with additional commands.
You can use the shell command, inserting output.
```shell
aiv -e "generate something"
```

The `-r` option to re-output the input is particularly useful when using in an editor.
This allows you to insert AI output after the selected text in the editor.
```shell
aiv -r ["some prompt"]
```

This workflow lets you seamlessly integrate AI assistance while editing, maintaining conversation context between commands for more coherent assistance.

## License

MIT License

