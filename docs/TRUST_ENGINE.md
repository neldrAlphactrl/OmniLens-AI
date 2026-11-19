Trust Engine
The Trust Engine is the core of OmniLens AI. It ties together consent, retention, deletion, audit logs, CI gates, dashboards, and runbooks into a single investor-facing architecture.
Principles
User-owned memory: user can view, export, delete all data
Explicit consent: no memory saved without User Approved consent
Retention controls: Permanent, 30d, 90d, 1y — auto-purge on expiry
Verified deletion: deletion is not complete until verified
End-to-end encryption: data encrypted at rest and in transit
Transparent auditing: every action logged
Consent Model
Every memory requires consent = "User Approved" (POST returns 400 otherwise)
Consent can be revoked → triggers deletion flow
Consent history stored in audit log
Retention
Default: Permanent unless user picks a shorter window (30d, 90d, 1y)
On expiry: auto-delete + verify + audit
Deletion Verification
Steps:
Memory record removed
Search index updated
Retrieval cache cleared
Audit log created
Backup purge scheduled (24h) Returns deletion_id: del_xxxxxx GET /api/v1/deletion/:id/verify returns status Passed / Pending
Audit Logs
Every create / delete is logged with timestamp, action, memoryId, deletionId if applicable. Read them at GET /api/v1/audit.
CI Gates
privacy-gate.yml: fails if any endpoint saves without consent, or deletes without verification
security.yml: dependency scan, secret scan
Dashboards / Runbooks
Dashboard: memories count, deletions verified, retention expiries
Runbook: how to handle failed deletion verification, backup purge failure
