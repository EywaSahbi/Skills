# Install these skills (instructions for AI coding agents)

You are an AI coding agent: Claude Code, Codex, Cursor, Gemini CLI, GitHub Copilot, OpenCode, Antigravity, Windsurf or similar. The user wants the skills below installed in **your** harness at user level, so they work in every project, and kept up to date automatically. Work through the steps in order. Talk to the user in the language they wrote to you in.

## Rules

- Install only from the sources listed in this file. Skip extras that an upstream README suggests.
- Install at user level (global). Don't add files to the project you were started in.
- If a skill is already installed, from any source, keep it and don't add a second copy.
- Before you start, tell the user in one short list what you will install. Then go ahead without asking. Ask only when something needs the user: a browser login, admin rights, or a prerequisite you cannot install yourself.
- If a step fails, carry on with the others and report the failure at the end. Never work around a permission prompt or a security block. Tell the user what you need instead.
- Never write tokens or API keys into files or into the chat.
- Upstream install steps change. Where this file says "follow upstream", read the current README section for your harness. It wins over anything written here.

## Step 1: Check prerequisites

1. Work out which harness and which OS you are running on.
2. You need:
   - git
   - Node.js 22.20 or newer, for `npx skills`
   - Python 3 with PyYAML, for Deep Research
3. If something is missing, install it if you can: winget on Windows, brew on macOS, apt or dnf on Linux. Otherwise give the user the exact command, then do every step that doesn't need it.
4. In Claude Code, `claude` may not be on PATH, for example when it runs as the VS Code extension. Look for the binary in `~/.vscode/extensions/anthropic.claude-code-*/resources/native-binary/` (`claude.exe` on Windows). If you can't find it, give the user the matching `/plugin install …` command to type.

## Step 2: Install the skills

| # | Skill | What it does | Source |
|---|---|---|---|
| 1 | Superpowers | Brainstorm, plan, TDD and review workflow; systematic debugging | `obra/superpowers` |
| 2 | frontend-design | Distinctive UIs instead of the generic AI look | `anthropics/skills` |
| 3 | Karpathy guidelines | Think first, keep it simple, make surgical changes, verify | `multica-ai/andrej-karpathy-skills` |
| 4 | Context7 | Current library docs instead of outdated training data | `upstash/context7` |
| 5 | Deep Research | `/research`: structured web research with parallel subagents | `Weizhena/Deep-Research-skills` |
| 6 | claude-md-management | Keeps CLAUDE.md files clean. **Claude Code only** | `claude-plugins-official` |

### 1. Superpowers (native plugin)

Install Superpowers through your harness's own plugin system. It ships hooks and tool mappings for each harness, so don't just copy its SKILL.md files.

- **Claude Code:** `claude plugin install superpowers@claude-plugins-official`
- **Other harnesses:** follow upstream, in the section for your harness at https://github.com/obra/superpowers#installation. It covers Codex, Cursor, Gemini CLI, GitHub Copilot CLI, OpenCode, Antigravity, Pi, Kimi and Factory Droid.
- **Harness not listed there:** `npx -y skills add obra/superpowers -g -a <agent-id> -y`

### 2 and 3. frontend-design and Karpathy guidelines (`npx skills`)

```bash
npx -y skills add anthropics/skills --skill frontend-design -g -a <agent-id> -y
npx -y skills add multica-ai/andrej-karpathy-skills --skill karpathy-guidelines -g -a <agent-id> -y
```

- `<agent-id>` is your harness's ID in the skills CLI, for example `claude-code`, `codex`, `cursor`, `gemini-cli`, `github-copilot`, `opencode`, `windsurf` or `antigravity`. The full list is at https://github.com/vercel-labs/skills/blob/main/docs/agents.md.
- On Windows, if the symlink fails, run the same command again with `--copy`.

### 4. Context7

- **Claude Code:** `claude plugin install context7@claude-plugins-official`
- **Other harnesses:** add the remote MCP server `https://mcp.context7.com/mcp` (HTTP transport) to your harness's **user-level** MCP config. It works without an API key, at lower rate limits.

### 5. Deep Research

This one has a separate variant for each harness. Follow upstream at https://github.com/Weizhena/Deep-Research-skills, with these details:

1. Clone the repo to `~/.agents/sources/Deep-Research-skills`. If it is already there, run `git pull --ff-only` instead.
2. Use the English variant: `research-en`, or `research-codex-en` on Codex.
3. Copy the files for your harness:
   - **Claude Code:**
     - `skills/research-en/*` → `~/.claude/skills/`
     - `agents/web-search-agent.md` and `agents/web-search-modules/` → `~/.claude/agents/`
   - **Codex:** follow upstream, including the `~/.codex/config.toml` part. If your Codex version doesn't load skills from `~/.codex/skills`, use `~/.agents/skills`.
   - **OpenCode:** follow upstream.
   - **Any other harness:**
     - Copy `skills/research-en/*` into your user-level skills directory.
     - If your harness has no compatible subagent format, skip the subagent and tell the user that `/research-deep` will run without parallel agents.
4. Install PyYAML with `python -m pip install --user pyyaml`. If the system refuses (PEP 668, for example on Ubuntu 24.04), use `sudo apt install python3-yaml`.

### 6. claude-md-management (Claude Code only)

`claude plugin install claude-md-management@claude-plugins-official`. Skip this one in every other harness.

## Step 3: Keep everything up to date

1. Plugins from `claude-plugins-official` already update themselves. If another harness offers automatic plugin updates, turn them on. If it doesn't, put its update command (for example `gemini extensions update --all`) into the daily job below.
2. Create **one** daily job at user level, without admin rights:
   - Windows: Task Scheduler
   - macOS: launchd
   - Linux: cron or a systemd user timer

   The job runs these steps:
   1. `npx -y skills update -g -y`
   2. `git -C ~/.agents/sources/Deep-Research-skills pull --ff-only`, then the Deep Research copy step from Step 2 again.
   3. Any harness update commands from point 1.
3. Put the job's script at `~/.agents/update-skills.sh`, or `~/.agents/update-skills.ps1` on Windows, and write its output to `~/.agents/update-skills.log`. Use full paths, because scheduled jobs often start without the user's PATH.
4. Running the script twice must do no harm. If it already exists because the user installed into another harness on this machine, add to it. Never create a second job.
5. Run the script once now and check the log.

## Step 4: Check and report

1. Check what is really installed, using `npx skills ls -g`, your harness's plugin list and the skills folders.
2. Report to the user in a short list:
   - installed
   - skipped (already there)
   - failed, and why
   - what the user still has to do
3. Remind the user to restart the harness so it loads the new skills.

## Optional extras (only when the user asks for them)

Install them the same way. Use a native plugin where the repo offers one for your harness, otherwise `npx -y skills add <repo> -g -a <agent-id> -y`.

| Package | Source | What it does |
|---|---|---|
| ui-ux-pro-max | `nextlevelbuilder/ui-ux-pro-max-skill` | UI styles, palettes, design systems |
| context-engineering | `muratcankoylan/agent-skills-for-context-engineering` | Context and multi-agent patterns |
| marketing-skills | `coreyhaines31/marketingskills` | Copywriting, SEO, ads, CRO |
| trading | `tradermonty/claude-trading-skills` | Market and stock analysis |
