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

## Step-by-Step Setup

### Step 1: Add MCP config

Copy `.mcp.json` to your home directory (`~/.mcp.json`) or merge it with your existing config:

```json
{
  "chrome-devtools": {
    "command": "npx",
    "args": ["-y", "chrome-devtools-mcp@latest", "--browser-url=http://127.0.0.1:9222"]
  }
}
```

> The `--browser-url` flag tells the MCP server to connect to your running Chrome instance on port 9222 instead of launching a new browser.

### Step 2: Launch Chrome with remote debugging

Close all existing Chrome windows first, then run:

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

> **Important:** Keep this Chrome window open the entire time. This is the browser Claude will control.

### Step 3: Open 10015.io

In the Chrome window that just launched, navigate to https://10015.io and open any tool you want to use (e.g., **Text to Handwriting**).

### Step 4: Restart Claude Code

Close and reopen Claude Code so it picks up the new MCP server. After restart, `chrome-devtools` should appear as an available MCP server.

### Step 5: Ask Claude to interact with the page

Example prompts you can try:

| Task | Prompt |
|---|---|
| Screenshot | *"Take a screenshot of the current Chrome tab"* |
| Text to Handwriting | *"Navigate to https://10015.io/tools/text-to-handwriting-converter, type 'Hello World' in the text area, and take a screenshot of the result"* |
| CSS Gradient | *"Go to https://10015.io/tools/css-gradient-generator and create a blue-to-purple gradient, then give me the CSS code"* |
| Lorem Ipsum | *"Navigate to https://10015.io/tools/lorem-ipsum-generator, generate 3 paragraphs, and paste the output here"* |
| List tabs | *"List all open tabs in Chrome"* |
| Any tool | *"Navigate to https://10015.io and find the [tool name]"* |

## Alternative: Auto-Connect (Chrome 144+)

If you have Chrome 144+, you can skip the `--remote-debugging-port` step and use `--autoConnect`:

```json
{
  "chrome-devtools": {
    "command": "npx",
    "args": ["-y", "chrome-devtools-mcp@latest", "--autoConnect"]
  }
}
```

Then just open Chrome normally — no special flags needed.

## How It Works

```
Claude Code  -->  Chrome DevTools MCP Server  -->  Chrome Browser  -->  10015.io
   (AI)              (MCP protocol)               (DevTools CDP)       (Web UI)
```

Claude sends commands through the MCP server, which uses the Chrome DevTools Protocol (CDP) to control the browser — navigating pages, filling forms, clicking buttons, and reading results.

## Troubleshooting

| Problem | Solution |
|---|---|
| "Cannot connect to browser" | Make sure Chrome is running with `--remote-debugging-port=9222`. Close ALL other Chrome windows before launching. |
| MCP server not showing in Claude Code | Restart Claude Code. Verify `~/.mcp.json` is valid JSON. |
| Wrong tab is being controlled | Ask Claude to *"list all open tabs"* then *"switch to tab [number]"* |
| Chrome won't launch with debug port | Another Chrome instance may be running. Close all Chrome windows and try again. |
| Tools on 10015.io look different | The site may have updated its layout. Adjust your prompts or ask Claude to *"take a screenshot and describe what you see"* |

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
