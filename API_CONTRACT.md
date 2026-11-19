# API Contract — OmniLens AI (base: /api/v1)

## 1. POST /api/v1/memories — Save
Request:
```json
{ "text": "I'm building OmniLens AI", "category": "Startup Vision", "retention": "Permanent", "consent": "User Approved" }
```
- 201: `{ "id": "cm...", "createdAt": "2026-11-17T...", "memory": { ... } }`
- 400 if `consent != "User Approved"`: `{ "error": "Consent required: consent must be \"User Approved\"" }`
- 400 if text empty/missing.
Side effect: AuditLog entry, action="create".

## 2. GET /api/v1/memories?q= — Recall
`GET /api/v1/memories?q=OmniLens`
- 200: `{ "memories": [ {id, text, category, retention, consent, createdAt} ], "count": 1 }`
Matches q (case-insensitive) against text and category. No q returns all, newest first.

## 3. DELETE /api/v1/memories/:id — Delete
- 200: `{ "deletionId": "del_xxxxxx", "memoryId": "...", "steps": ["Memory record removed","Search index updated","Retrieval cache cleared","Audit log created","Backup purge scheduled"] }`
- 404 if the memory does not exist.
Side effect: AuditLog entry, action="delete", with deletionId.

## 4. GET /api/v1/deletion/:id/verify — Verify
`:id` is a deletionId.
- 200: `{ "deletionId": "del_xxxxxx", "status": "Passed" }` when the deletion is in the audit log and the memory is gone; otherwise `"status": "Pending"`.

## 5. GET /api/v1/audit — Audit
- 200: `{ "audit": [ {id, action, memoryId, deletionId, createdAt} ], "count": 2 }`, newest first.
