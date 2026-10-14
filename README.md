# Bonframe Skills

Use your AI agent to find video moments, create rough cuts, translate and dub videos, and coordinate lip-sync work across your paired devices.

[Explore Bonframe](https://bonframe.com/en/mcp) · [Download the desktop app](https://bonframe.com/en/download) · [MCP setup guide](https://bonframe.com/en/docs/agent-control)

## Start here

1. Install Bonframe on a supported Mac or Windows workstation and import your footage.
2. Open **Bonframe → MCP**, choose **Connect Codex** or **Connect Claude**, and complete the authorization shown by the App.
3. Install the optional **bonframe** Skill below. It guides your agent; it does not establish the MCP connection or register compute devices.
4. Ask your agent to check Bonframe tools and list your video projects before creating a task.

## Install the Skill

Download this repository with **Code → Download ZIP** and extract it. Copy the complete `skills/bonframe` directory, including its `references` and `agents` directories, to the location used by your agent:

- **Codex:** `~/.codex/skills/bonframe`
- **Claude Code, project scope:** `<your-project>/.claude/skills/bonframe`
- **Claude Code, personal scope:** `~/.claude/skills/bonframe`

Restart or reload the agent session so it discovers the Skill. Keep existing customization backed up before replacing an installed version. This repository contains instruction files, not an executable MCP server, model weights or the desktop application.

## Try these requests

### Verify the connection

> Use Bonframe MCP to list my imported video projects. Do not create a task yet.

The agent reads capabilities and projects on the connected Bonframe workstation. A project is imported video material, not a workflow or a Memory entry.

### Create a rough cut

> Find the moments about long-term investing in Sales Advice and create a 60-second rough-cut preview. Let me review it before export.

### Search Memory

> Search Bonframe Memory for pricing insights across my projects. Cite the source projects and timestamps. Do not edit anything yet.

### Make language versions

> Use the current approved cut of Sales Advice. Translate it into Spanish and Japanese, preserve the speakers' voices, require SD lip sync, and use my paired devices. Let me review both results before export.

Use project titles that actually exist in your library. Ask the agent to resolve ambiguous names before submitting production work.

## What happens after you ask

Your agent selects the project and submits a persistent task through the authorized MCP connection. Bonframe performs the requested processing on your devices, exposes task progress and keeps the results associated with the source project.

For multilingual lip-sync work, the initiating workstation prepares translation, dubbing and compatible speaker localization. Bonframe distributes supported lip-sync tasks to available paired workers and returns verified artifacts to the initiating workstation. Mac and Windows devices must be paired, online and model-ready. One language task is not necessarily split across multiple machines.

Nearby discovery works on a supported local network. Remote or routed networks require reachable connections and authorization; installing this Skill does not make arbitrary networks discoverable.

## Review and export

Ask for changes in the same agent conversation, or open **Bonframe → MCP → Tasks → task details** to preview and review the result. A remote workstation path is not a downloadable file on the agent's computer. Use supported result links or the App's preview/export controls.

MCP task access is free. Export follows your Bonframe plan and explicit authorization. Generation, human approval and export are separate steps; this Skill does not bypass the watermark or export boundary.

## Skills in this repository

- [bonframe](skills/bonframe/SKILL.md): project discovery, rough cuts, Memory retrieval, translation, dubbing, lip sync, persistent tasks and result handoff.

## Contributing

Report reproducible Skill issues in this repository. Include the client and Bonframe version, the request and the observed behavior. Remove credentials, private media, signed URLs and personal information from reports. Changes should follow the tools and schemas exposed by Bonframe rather than invent new tools.

The Skill instruction files are MIT licensed. Bonframe software, models, media and branding retain their respective licenses and rights.
