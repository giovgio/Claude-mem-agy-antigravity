# Claude-Mem Integration for Antigravity (agy)

This repository details how to integrate and use **[claude-mem](https://github.com/thedotmack/claude-mem)**—the persistent memory compression system—with the AI-first developer platform **Google Antigravity CLI (`agy`)**.

Claude-mem captures your session observations, tool outputs, and decisions in the background, then injects relevant summaries as context at the start of your subsequent sessions.

---

## Prerequisites

* **Antigravity CLI (`agy`)** installed and configured.
* **Node.js** 18+ installed on your machine.
* A free API key from **[Google AI Studio](https://aistudio.google.com/apikey)**.

---

## Installation

### Step 1: Repair / Initialize the MCP Configuration

Antigravity CLI loads Model Context Protocol (MCP) servers from a local configuration file. If the file is corrupt or blank, the installation will abort. Initialize it with a valid empty JSON object first:

```bash
mkdir -p ~/.gemini/antigravity
echo "{}" > ~/.gemini/antigravity/mcp_config.json
```

### Step 2: Install Claude-Mem

Run the interactive `claude-mem` installer, specifying `antigravity` as the target IDE and `gemini` as the provider:

```bash
npx claude-mem install --ide antigravity --provider gemini --no-auto-start
```

This installer registers the Claude-mem MCP server configuration in `~/.gemini/antigravity/mcp_config.json` and creates a local context rule in your active project under `.agents/rules/claude-mem-context.md`.

### Step 3: Configure Google AI Studio API Key

To enable observation summary extraction using Gemini's free tier, configure your credentials:

1. Open `~/.claude-mem/settings.json` in your text editor.
2. Structure it as follows, replacing `"YOUR_API_KEY"` with your Google AI Studio API Key:

```json
{
  "CLAUDE_MEM_RUNTIME": "worker",
  "CLAUDE_MEM_PROVIDER": "gemini",
  "CLAUDE_MEM_GEMINI_API_KEY": "YOUR_API_KEY"
}
```

### Step 4: Start the Memory Daemon

Start the background worker process that manages observation indexing and query handling:

```bash
npx claude-mem start
```

---

## Verification

To ensure that everything is running correctly, verify with the following tools:

### Check Worker Daemon Status
```bash
npx claude-mem status
```
*Expected Output:* Shows a healthy status, port `37701`, and the active PID.

### Run CLI Diagnostics
```bash
npx claude-mem doctor
```

### Search Memories in Terminal
```bash
npx claude-mem search "your query"
```

### View Memory Web UI
Open **[http://localhost:37701](http://localhost:37701)** in a web browser to view the visual memory timeline, project graphs, and captured observations.

---

## Usage

Restart your `agy` session to pick up the newly configured MCP server. 

1. **Passive Learning**: Start working normally. As you view files, write code, and run commands, Claude-mem captures observations passively.
2. **Context Injection**: On your second session in a project workspace, the injected context will automatically populate the agent's context, letting the agent "remember" previous tasks and architecture patterns.
