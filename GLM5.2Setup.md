# GLM 5.2

## Complete Setup Guide

The open-source AI coding agent that rivals Claude Code

### What is GLM 5.2?

GLM 5.2 is the latest flagship AI model from Zhipu AI (now branded as Z.ai), a Chinese AI lab. Released on June 13, 2026, it is the most powerful open-source coding model available right now.

**Key specs:**

*   753 billion parameters (~40B active per token via Mixture-of-Experts architecture)
*   1 million token context window (5x larger than GPT-5.5 and most competitors)
*   131,072 output tokens per response
*   MIT License (fully open-source, no regional restrictions)
*   Two reasoning modes: High (fast, routine tasks) and Max (deep reasoning for complex coding)
*   Works with 8+ coding tools: OpenCode, Claude Code, Cline, Roo Code, OpenClaw, Kilo Code, Goose, and Crush

### How good is it really?

On FrontierSWE (long-horizon coding benchmark), GLM 5.2 trails Claude Opus 4.8 by just 1% and beats GPT-5.5. On Terminal-Bench 2.1, it scores 81.0 vs Claude Opus 4.8's 85.0. It is the strongest open-source coding model available as of June 2026.

### What You Need

*   A computer with a terminal (macOS, Linux, or Windows with WSL)
*   Node.js installed (for `npm install` method) or just `curl`
*   A Z.ai account (for the API key — GLM Coding Plan starts at ~$18/month, which is 10x cheaper than Claude Code at $200/month)

### Method 1: GLM 5.2 + OpenCode (Recommended)

OpenCode is the fastest-growing open-source AI coding agent with 160K+ GitHub stars. It runs in your terminal and supports 75+ AI model providers. The tool itself is completely free.

#### Step 1: Install OpenCode

Open your terminal and run one of these commands:

**Option A — Quick install (recommended):**

```bash
curl -fsSL https://opencode.ai/install | bash
```

**Option B — Via npm:**

```bash
npm i -g opencode-ai@latest
```

**Option C — macOS via Homebrew:**

```bash
brew install anomalyco/tap/opencode
```

**Option D — Windows:**

```bash
scoop install opencode
```

Verify the installation by running:

```bash
opencode --version
```

#### Step 2: Get Your Z.ai API Key

1.  Go to [z.ai](https://z.ai) and create an account
2.  Navigate to the GLM Coding Plan and subscribe (Lite tier starts at ~$18/month)
3.  Go to Plan Overview and create an API Key
4.  Copy the API key immediately (you cannot view it again after creation)

**Important**

Keep your API key private. Never hard-code it in your source code or share it publicly. Store it in environment variables.

#### Step 3: Connect GLM 5.2 to OpenCode

Set these environment variables in your terminal. Add them to your shell config file (`.bashrc`, `.zshrc`, or equivalent) so they persist:

```bash
export OPENAI_BASE_URL="https://api.z.ai/api/coding/paas/v4"
export OPENAI_API_KEY="your-zai-api-key-here"
export OPENAI_MODEL="glm-5.2"
```

For the 1 million token context window, use this model name instead:

```bash
export OPENAI_MODEL="glm-5.2[1m]"
```

After saving your shell config, restart your terminal or run:

```bash
source ~/.zshrc  # or source ~/.bashrc
```

#### Step 4: Start Building

Navigate to your project folder and launch OpenCode:

```bash
cd your-project
opencode
```

OpenCode will open a terminal interface. You can now start coding with GLM 5.2. Try asking it something:

```
Build a React login form with email validation
```

**Pro Tip: Plan vs Build mode**

Press `Tab` to switch between Plan mode (read-only analysis) and Build mode (makes actual changes). Start complex tasks in Plan mode to outline your approach, then switch to Build for implementation.

### Method 2: GLM 5.2 in Claude Code

If you already use Claude Code, you can swap the backend to GLM 5.2 without changing your workflow.

#### Step 1: Install Claude Code (if not already installed)

```bash
npm install -g @anthropic-ai/claude-code
```

#### Step 2: Edit Claude Code Settings

Open the settings file at `~/.claude/settings.json` and add or modify the `env` fields:

```json
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "your-zai-api-key",
    "ANTHROPIC_BASE_URL": "https://api.z.ai/api/anthropic",
    "API_TIMEOUT_MS": "3000000",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "glm-5.2[1m]",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "glm-5.2[1m]",
    "CLAUDE_CODE_AUTO_COMPACT_WINDOW": "1000000"
  }
}
```

#### Step 3: Restart and Verify

Close and reopen your terminal, then run:

```bash
cd your-project
claude
```

Use the `/status` command to confirm the model is set to `glm-5.2`. Use `/effort` to switch between High and Max reasoning.

**Note on ANTHROPIC_AUTH_TOKEN**

Z.ai's official docs use `ANTHROPIC_AUTH_TOKEN` (not `ANTHROPIC_API_KEY`). Using the wrong variable name is a common setup error. Also make sure to use the Anthropic-compatible endpoint (`api.z.ai/api/anthropic`), not the OpenAI-compatible one.

### Method 3: Automatic Setup (Coding Tool Helper)

Z.ai provides an automated helper that configures everything for you in one command:

```bash
npx @z_ai/coding-helper
```

Follow the on-screen prompts. It will automatically detect your installed coding tools (OpenCode, Claude Code, Cline, etc.), configure the API connection, and set up MCP servers.

### Understanding Reasoning Effort

GLM 5.2 has two thinking modes that control how deeply it reasons before responding:

*   **High** — Faster responses, good for routine coding tasks like writing functions, fixing bugs, or generating boilerplate code.
*   **Max** — Deeper reasoning, recommended by Z.ai for complex refactoring, architecture decisions, and multi-step agentic work. Uses more tokens but produces more reliable output.

In Claude Code, switch between them using the `/effort` command. In OpenCode, Max is typically the default for coding tasks.

### Why GLM 5.2 Matters

*   **10x cheaper than Claude Code** — GLM Coding Plan starts at ~$18/month vs Claude Code's $200/month subscription.
*   **5x larger context window** — 1 million tokens means you can load an entire mid-sized codebase without chunking or losing context.
*   **Open source (MIT License)** — You can inspect, modify, and self-host the model weights. No vendor lock-in.
*   **Drop-in replacement** — Works in tools you already use (Claude Code, OpenCode, Cline, etc.) with just an API key swap.
*   **Near-frontier performance** — Within 1% of Claude Opus 4.8 on long-horizon coding tasks. Beats GPT-5.5 on multiple benchmarks.

### Common Issues & Fixes

*   **"Model not found" error with `[1m]` suffix**
    Update your coding tool (OpenCode or Claude Code) to the latest version. Older versions may not recognize the `[1m]` context window suffix.
*   **Requests timing out on long tasks**
    GLM 5.2 can take 30–90 seconds for first-token latency on 1M-context calls. If using Claude Code, make sure `API_TIMEOUT_MS` is set to `3000000` (50 minutes) in your `settings.json`.
*   **Wrong model being used**
    After configuring, verify by asking "What model are you?" in the chat. If it says `glm-4.7` or `claude-sonnet` instead of `glm-5.2`, double-check your environment variables and restart the terminal.
*   **OpenCode not recognizing environment variables**
    Make sure you added the `export` lines to your shell config file (`.zshrc` or `.bashrc`) AND ran `source` on it. Alternatively, use `/connect` inside OpenCode to set the provider directly.

### Useful Links

*   Z.ai Official Docs: [docs.z.ai/devpack/quick-start](https://docs.z.ai/devpack/quick-start)
*   GLM Coding Plan: [z.ai/subscribe](https://z.ai/subscribe)
*   OpenCode GitHub: [github.com/anomalyco/opencode](https://github.com/anomalyco/opencode)
*   OpenCode Docs: [opencode.ai/docs](https://opencode.ai/docs)
*   GLM 5.2 Weights (HuggingFace): [huggingface.co/zai-org](https://huggingface.co/zai-org)
*   OpenCode Desktop App: [opencode.ai/download](https://opencode.ai/download)