# Review and export tool boundaries

These names come from the current Bonframe MCP catalog. Always follow live schemas when using them.

- `get_batch_job`: Read task, item and artifact details. Preserve `batchId` and returned revision values.
- `create_batch_review`: For requested remote review, supply `batchId`, optional `ttlSec` and `confirm=true`. Returns a temporary read-only review URL; it cannot approve, apply or export. A loopback URL is accessible only from its host. Do not create public tunnels without authorization. Links may contain access credentials; do not write them to persistent memory or public logs.
- `create_batch_preview`: For a requested proposal preview, supply real `projectId`, `proposalId` and confirmation fields. This does not imply the tool previews every video artifact.
- `revoke_batch_review` / `revoke_batch_preview`: Revoke a link using its returned `tokenId`.
- `approve_batch_items`: After explicit approval, use specific `proposalIds` from the latest query, the latest `expectedUpdatedAt` and `confirm=true`. Items marked red / review_required that cannot be remotely batch-approved must be reviewed in the App; preserve these checks.
- `apply_batch_job`: Apply approved items with authorization and `confirm=true`. Applying is not exporting. Do not force standalone translation artifacts through a rough-cut approval/apply flow just to export; use their supported entry point.
- `export_batch`: Export supported applied items using `batchId`, confirmation fields and an optional `exportDir` within allowed directories. Preserve licensing and no-overwrite rules. Artifacts that do not meet the tool's conditions use the App's corresponding export stage; never fabricate applied status.

For “generate it and let me review,” deliver previews and wait for review. Existing explicit approval of specific artifacts or export authorization need not be requested again, but tool confirmation cards and revision checks still apply.
