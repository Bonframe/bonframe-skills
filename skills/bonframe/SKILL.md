---
name: bonframe
description: Use authorized Bonframe MCP tools to find video projects, create editing or translation, dubbing and lip-sync jobs, track persistent tasks, and review or export results. Use for video production and task management in Bonframe, including distributed execution on local devices.
---

# Bonframe

Operate the user's video projects through Bonframe MCP. Reuse the product's workflows and task management rather than creating a separate scheduler or bypassing MCP authorization with scripts. Describe projects, devices, progress and results to users; queue and database implementation details are unnecessary.

## Connection and capabilities

- Discover the Bonframe MCP tools available in the current session and call `get_capabilities`. Host prefixes may vary; use the current tool schemas, capability responses and execution results as the authority.
- If tools are missing, explain that a Skill does not establish an MCP connection. The user must open Bonframe and authorize the connection from its MCP device page; the pairing entry point is `bonframe://mcp`. Reload the Agent if needed. Skip installation and pairing when already connected; do not modify existing configuration. This Skill does not automatically register compute devices.
- Distinguish SDK transport tests, CLI Agent tests and tools loaded in the current chat. Claim only the layer actually verified.

## Select material and create a task

1. Select a project with `list_projects`, then read its summary with `get_project`. Projects are imported video material in the connected Bonframe instance, not workflows, rough-cut versions or Memory. Clarify ambiguous sources or duplicate names rather than guessing an ID.
2. Use `search_library` or `ask_memory` for cross-project retrieval and `search_transcript` within a project. Read-only discovery requests must not create production tasks or retrieve entire transcripts.
3. Prefer `run_video_workflow` for complete video production. Supply a complete `instruction`, explicit `projectIds`, and the requested `targetLanguages` and `lipSync`. To continue an approved cut, use the supported `selection.mode=current_cut` contract and its real `sourceVersionId` rather than cutting again.
4. Use `create_batch_job` for explicit standalone batches, with `jobType` (`recut`, `voice_translate` or `redact`) and `jobGoal`. Translation, dubbing and lip-sync must follow the product's existing pipeline. Reuse existing dubbing only when the current tool explicitly supports that artifact; do not silently translate or synthesize it again.
5. Before editing, reuse a matching `directorTheme` object returned by `get_project`. If none matches, write a direct editing instruction. Call `suggest_abstract_prompts` only when the user requests new angles.
6. Keep a stable `idempotencyKey` for the same submission intent. Record the returned task and project IDs, the connected Bonframe instance and production parameters. After a timeout, establish whether the job already exists before resubmitting. Use a new key for an explicitly new task.

Do not ask again to create a task the user has already requested. Default to a review workflow. Handle `autoApply`, confirmation cards and export according to the current schema and existing explicit authorization; production authorization alone does not authorize approval and export.

## Distributed execution

For translation, dubbing, lip-sync, distributed tasks or failure recovery, read [references/task-lifecycle.md](references/task-lifecycle.md) as needed for preflight checks, reuse and result retrieval.

Production compute runs on the user's local devices. The current product scope is macOS and Windows. Bonframe manages paired devices, GPU slots, leases and result delivery. Do not configure system databases, GPU passthrough or production cloud compute.

For explicitly requested device distribution, use `lipExecution=lan` if the tool supports it; use `auto` when product selection is acceptable. Do not invent unsupported fields. Report unavailable devices or actual single-device execution honestly; never claim distributed completion after silently falling back locally.

Worker artifacts return to the connected Bonframe instance. Say they returned to the current machine only when that instance runs here. Task lists are not guaranteed to synchronize across Bonframe instances.

The initiating Bonframe runtime prepares localization data for compatible workers. Identical source video, original audio and speaker timelines can reuse it; changed inputs require revalidation. Both ends must support the same localization contract, and the initiator needs its localization runtime. Distributed execution and hash-verified result delivery have been exercised on macOS and Windows. Use the actual device receipts for each task; successful computation does not imply human quality approval.

## Progress and controls

- Query authoritative state with `get_batch_job(batchId)` and respect `nextPollAfterMs`. Preserve the returned Markdown status template and its facts. Stop polling at a terminal state or when user review is required. Ending a chat does not cancel background work. Do not create recurring automation unless requested.
- Pausing stops further dispatch; the current stage may still finish. Cancellation requests stop computation while preserving generated artifacts. Use the available `cancel_batch_job` tool and its required confirmation fields when the user requests cancellation.
- The current baseline MCP does not expose pause, resume or retry tools. Use the App's MCP task details page for those actions. If official tools become available, follow their schemas; otherwise explain the page entry point rather than inventing tools or calling internal HTTP endpoints to bypass permissions.
- Read the specific task error before recovery. Retry only authorized failed items through supported tools after fixing recoverable input or connection issues; preserve successful languages. Report authentication, licensing, human review or external prerequisites and stop unchanged retries.

## Review and export

Generation is not human approval or export. Provide accessible video/audio previews, language and task IDs. If unable to watch or listen, leave quality review to the user rather than claiming lip-sync or audio quality passed.

The App entry point is MCP → Tasks → task details → Preview / Review & edit → Export. The list does not guarantee a standalone one-click download button. Read [references/review-export.md](references/review-export.md) when remote review links or approval/export operations are needed.

Preserve export authorization, allowed destination directories and no-overwrite rules. Paths belong to the connected Bonframe host; remote filesystem paths are not downloadable local files. Show only accessible media or real returned links, and do not promise an unavailable download capability.

## Examples

“Use Bonframe to list imported video projects and check for Sales Advice. Do not create a task yet.” → Query projects and return matches.

“Translate Sales Advice into Japanese, preserve the speaker's voice, require lip-sync, use registered devices, and let me review the result.” → Resolve the project and language, check capabilities, create a persistent task and follow it until review. Do not automatically approve or export.

“Pause that task.” → Use an actual pause tool if available; otherwise give the App task page entry point. Do not substitute cancellation.
