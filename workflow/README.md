# n8n workflow

Import these workflows into the same n8n project:

- `n8n-lagos-hr-workflow.json` — application, AI assessment, approval, outcome emails and audit writes.
- `error-notifications.json` — shared production-error email handler.
- `candidate-audit-retention-90-days.json` — daily audit cleanup.

## Required configuration

1. Assign your own OpenAI credential to **OpenAI Chat Model**.
2. Assign your own Gmail OAuth credential to:
   - **Recruiter Approval**
   - **Send Interview Invite**
   - **Send Status Update**
3. Create `candidate_decisions` with the columns in `candidate-decisions-table-schema.json`.
4. Select that table in **Store Invite Audit**, **Store Decline Audit**, and **Delete Audit Rows Older Than 90 Days**.
5. Replace `REPLACE_WITH_RECRUITER_EMAIL` in the approval and error-notification Gmail nodes.
6. Confirm the Webhook node uses `POST`.
7. Publish the error workflow first.
8. In the main workflow settings, select it as the **Error workflow**.
9. Publish the retention and main workflows, then copy the main workflow's Production URL into the frontend.

The validation template permits unauthenticated submissions while
`REPLACE_WITH_SHARED_WEBHOOK_SECRET` remains unchanged. For production, place the form behind a server-side proxy, replace that placeholder in the Code node, and have the proxy send the same value in the `x-n8n-lagos-webhook-secret` header. Never put the secret in browser JavaScript.

The exported JSON intentionally contains no credential bindings and is inactive on import.

## Main stages

- **Webhook** — accepts the website submission.
- **Validate Application** — validates fields, normalizes values and supplies role criteria.
- **Assess Candidate** — creates an evidence-based structured assessment.
- **Validate Assessment** — constrains the AI result to expected fields and recommendation values.
- **Recruiter Approval** — sends the review email and waits for a human decision.
- **Approved?** — routes the human decision.
- **Applicant email nodes** — send formatted invite or status emails.
- **Audit Code nodes** — assemble a minimal decision record after the applicant email succeeds.
- **Audit Data Table nodes** — insert the record into `candidate_decisions`.
- **Presenter Guide — Node Map** — canvas documentation for the live walkthrough.

The AI evidence score summarizes alignment between candidate-provided evidence and the supplied job criteria. The workshop guide uses 70–100 as ADVANCE, 50–69 as REVIEW and 0–49 as DO_NOT_ADVANCE. It remains advisory; the recruiter makes the final decision.

The error workflow runs for published production failures, not manual editor failures. The retention workflow deletes audit rows older than 90 days using `responded_at`.
