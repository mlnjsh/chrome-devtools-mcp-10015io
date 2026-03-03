# Chrome DevTools MCP + 10015.io Developer Toolkit Integration

Use [10015.io Developer Toolkit](https://10015.io) (text-to-handwriting, CSS generators, encoders, color tools, and 40+ utilities) directly from [Claude Code](https://claude.ai/claude-code) via the Chrome DevTools MCP server.

## Why This Exists

10015.io has no public API, so there is no native MCP integration. This setup uses the **Chrome DevTools MCP server** to let Claude Code control a live Chrome browser — automating 10015.io's web UI as if you were clicking through it yourself.

## What You Can Do

- Convert text to handwriting using 10015.io's tool, driven entirely by Claude
- Generate CSS (gradients, shadows, triangles) through browser automation
- Use any of 10015.io's 40+ tools hands-free from your terminal
- Take screenshots, read page content, and debug via DevTools

## Prerequisites

- [Node.js](https://nodejs.org/) v20.19+
- [Chrome](https://www.google.com/chrome/) (stable)
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI or VS Code extension

## Setup

### 1. Add MCP config

Copy `.mcp.json` to your home directory (`~/.mcp.json`) or merge it with your existing config:

```json
{
  "chrome-devtools": {
    "command": "npx",
    "args": ["-y", "chrome-devtools-mcp@latest"]
  }
}
```

### 2. Launch Chrome with remote debugging

**Windows:**
```bash
"C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=9222 --user-data-dir="%TEMP%\chrome-profile-stable"
```

**macOS:**
```bash
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9222 --user-data-dir=/tmp/chrome-profile-stable
```

**Linux:**
```bash
google-chrome --remote-debugging-port=9222 --user-data-dir=/tmp/chrome-profile-stable
```

### 3. Navigate to 10015.io

Open https://10015.io in the launched Chrome window and go to the tool you want to use (e.g., Text to Handwriting).

### 4. Restart Claude Code

Close and reopen Claude Code so it picks up the new MCP server.

### 5. Ask Claude

Example prompts:
- *"Take a screenshot of my current Chrome tab"*
- *"Go to 10015.io text-to-handwriting and convert this text: Hello World"*
- *"Generate a CSS gradient using 10015.io"*

## Alternative: Auto-Connect (Chrome 144+)

If you have Chrome 144+, you can use `--autoConnect` instead of remote debugging:

```json
{
  "chrome-devtools": {
    "command": "npx",
    "args": ["-y", "chrome-devtools-mcp@latest", "--autoConnect"]
  }
}
```

This avoids the manual `--remote-debugging-port` step.

## How It Works

```
Claude Code  -->  Chrome DevTools MCP Server  -->  Chrome Browser  -->  10015.io
   (AI)              (MCP protocol)               (DevTools CDP)       (Web UI)
```

Claude sends commands through the MCP server, which uses the Chrome DevTools Protocol (CDP) to control the browser — navigating pages, filling forms, clicking buttons, and reading results.

## Limitations

- This is browser automation, not a native API — it interacts with the UI
- Requires Chrome to be running with remote debugging enabled
- Page layout changes on 10015.io may require prompt adjustments
- Some tools that require file uploads may need manual intervention

## Credits

- [Chrome DevTools MCP](https://github.com/nicholasosterman/chrome-devtools-mcp) by Google
- [10015.io](https://10015.io) — All Online Tools in One Box
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) by Anthropic

## License

MIT
