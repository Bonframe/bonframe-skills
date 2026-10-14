# Production task operations

## Preflight

Discover actual MCP tools and read `get_capabilities` to confirm supported languages, lip-sync modes and production entry points. Project IDs, cut version IDs, batch IDs and translation run IDs identify different objects; use only real returned IDs. Record the connected Bonframe host. Running the Agent and App on the same machine does not establish MCP authorization or register compute devices.

Distributed lip-sync requires an available localization runtime on the initiating App, paired online compatible workers and available compute slots. A capability response without live device information does not prove readiness. Use an available official device query or the App's MCP device page. If no query tool is exposed, state the visibility limit; an already authorized production request may be submitted for product admission checks without asking for approval again. Preserve the task and report concrete prerequisites if execution rejects the environment.

Use `lipExecution=lan` when distribution is required and `lipSync=required` when lip-sync is mandatory. Supply only schema-supported quality values (currently `sd`/`hd`). `auto` permits product selection and does not guarantee distribution. Registering two devices does not mean each individual task is split between them; use actual child-task receipts as evidence.

## Submission and pipeline continuity

The complete pipeline is: select or freeze a cut → translate → clone voices and align audio → localize faces and match speakers on the initiator → distribute lip-sync compute → return and verify artifacts → review → authorized export. Let the product workflow manage these stages. A standalone lip-sync task does not prove translation or TTS was completed.

To continue an approved cut, use `run_video_workflow` with `selection.mode=current_cut` and the corresponding `sourceVersionId`. For explicit standalone translation batches, use `create_batch_job` with `voice_translate`, routing and quality fields supported by the actual schema. Record project, version, languages, lip-sync requirement, quality, execution preference and `idempotencyKey` before submission; save `batchId` from the response.

Initiator localization data is bound to source video, original audio, speaker timeline and policy. Identical inputs can reuse cached data. A different dubbing language alone does not require new localization, but source retiming or changed timelines must be revalidated. Workers reuse compatible reports, build a local plan and continue prepare/infer/compose. Do not promise initiator prelocalization without an installed model/localization runtime.

## Tracking, recovery and stopping

Follow `nextPollAfterMs` from `get_batch_job`. Check every language and child task; one finished language does not mean the batch is finished. Record task/run IDs, actual workers, attempts, errors and artifact details only when the response exposes them. Continue tracking queued or running tasks rather than creating replacements.

After submission timeout or disconnection, query the existing `batchId` first. If no ID was received, replay the same key only when idempotent submission is supported and parameters are unchanged. Do not invent a new key to retry an uncertain submission. Changed language, source, version or routing is a new intent; explain the distinction before creating it.

Before recovery, identify successful artifacts and the failed stage. Use a supported failed-item retry entry point and retry only the failed language/stage. Preserve valid translation and TTS when lip-sync fails. If MCP lacks retry, pause or resume tools, use App → MCP → task details; do not bypass authorization through internal HTTP. Do not replace pause with cancellation. Pause may only prevent further dispatch; cancellation preserves generated files and does not guarantee rollback.

Recoverable input or connection issues may be retried within existing authorization after repair. Stop repeating authentication, licensing or external environment failures when the same error recurs without changed prerequisites. Record task ID, error and recovery conditions. Stop polling after completion, cancellation or a request for human review.

## Result retrieval and handoff

Track four separate states: compute completed, artifact returned and verified, human quality approved, and export completed. Claim each only with matching evidence. A worker that finished while upload is pending has not yet delivered a usable artifact. Progress percentage does not replace terminal state and artifact checks.

Results return first to the connected Bonframe instance. Use media/result links returned by official MCP tools or supported Preview, Review & edit, download/export controls in App task details. When there is no download tool or accessible URL, provide the App entry point rather than constructing a link from an internal server path. A remote Bonframe filesystem path is not a file on the Agent's current machine. For requested local retrieval, use an authorized accessible official result interface; otherwise explain the missing prerequisite rather than opening a public tunnel.

For retrieved files, compare hashes when provided by the interface. If local media tools are available, check duration, audio/video streams and complete decoding. File integrity does not prove accurate translation, natural voice quality or lip synchronization. If unable to watch or listen, mark human review pending. Deliver languages, batch ID, accessible previews/files, execution-device evidence and remaining acceptance items.

Follow [review-export.md](review-export.md) for current revision checks, confirmation fields and authorization boundaries. Production authorization does not automatically authorize publication or file overwrites.
