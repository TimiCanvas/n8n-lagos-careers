# n8n workflow

Import `n8n-lagos-hr-workflow.json` into n8n.

## Required configuration

1. Assign your own OpenAI credential to **OpenAI Chat Model**.
2. Assign your own Gmail OAuth credential to:
   - **Recruiter Approval**
   - **Send Interview Invite**
   - **Send Status Update**
3. Replace `REPLACE_WITH_RECRUITER_EMAIL`.
4. Confirm the Webhook node uses `POST`.
5. Publish the workflow and copy its Production URL into the frontend.

The exported JSON intentionally contains no credential bindings and is inactive on import.

## Main stages

- **Webhook** — accepts the website submission.
- **Validate Application** — validates fields, normalizes values and supplies role criteria.
- **Assess Candidate** — creates an evidence-based structured assessment.
- **Validate Assessment** — constrains the AI result to expected fields and recommendation values.
- **Recruiter Approval** — sends the review email and waits for a human decision.
- **Approved?** — routes the human decision.
- **Applicant email nodes** — send formatted invite or status emails.
- **Audit nodes** — create an execution-visible decision record.

For production use, add authentication/rate limiting to the webhook, durable audit storage, retention rules, error notifications and an appropriate privacy notice.
