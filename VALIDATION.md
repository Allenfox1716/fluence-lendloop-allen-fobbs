# Observed validation — November 18, 2026 (Pacific)

This file distinguishes locally observed behavior from provider-backed behavior that has not been tested. No employer application, external notification or cloud deployment was performed by these checks.

## Passed locally

Environment: Node **24.22.1**, npm **11.19.1**. Pinned runtime dependencies: Next.js **16.4.6**, React **19.3.1**, Clerk Next.js SDK **7.10.4**, Convex **1.51.0**. `package-lock.json` records the resolved dependency graph.

| Command/check | Observed result |
| --- | --- |
| `npm run typecheck` | Passed, strict TypeScript, no emitted errors. |
| `npm test` | **23 tests passed** in two files, using actual Convex functions/schema/router plus raw-body/domain checks. |
| `npm run test:n8n` | **17 tests passed**, actual workflow Code bodies, export graphs and a signed local HTTP/idempotency fixture. |
| `npm run build` | Passed optimized Next.js production compilation, type checking and static generation. |
| `npm audit --omit=dev --audit-level=high --json` | Reported **0 known production dependency vulnerabilities** at the time queried. This is not a security certification. |
| Production server loopback smoke | Started `next start` on `127.0.0.1:4318`; GET `/` returned **200**, rendered LendLoop and its truthful unconfigured-catalog state, and included `nosniff`/`DENY` headers. Server was stopped after the check. |
| Cross-runtime signature check | Actual Convex `signBody` accepted by n8n raw-body verifier, including UTF-8; n8n signed acknowledgment accepted by actual Convex verifier; raw-byte tampering rejected. |
| Focused independent source review | Confirmed server RBAC and matched HTTP/HMAC contracts. Found two recovery races; both were fixed and regression-tested before packaging. |

The two recovery regressions verify that a delayed failure callback cannot reset a newer sender’s lease/state, and that no duplicate scheduled task can initiate attempt six. Another regression retains durable acceptance when the response fails after an acknowledgment and proves a replay does not send externally again.

`convex-test` uses controlled identities and fake timers. It verifies the actual authorization functions and persistence contracts, but it is not a hosted Convex deployment. The n8n wire fixture is not an imported/running n8n engine.

## Not live-validated

- Clerk dashboard/application setup, real sign-in and JWT issuance/rotation.
- Deployment of the schema/functions/cron to Convex, real reactive updates and hosted transaction/concurrency limits.
- Actual n8n engine import, activation, Code-node runtime/helper compatibility and scheduled executions. **Docker was not installed on this host.**
- Hosted Convex → reachable n8n → signed durable Convex callback.
- Actual process/container restart or crash recovery across those providers. The recovery logic is covered locally; the live scenario remains to be observed.
- Browser visual/accessibility review of authenticated screens. The HTTP smoke only rendered the unconfigured public shell.
- Email/SMS delivery. This exercise intentionally uses a logged mock and does not contain a real notification adapter.

## Controlled live acceptance sequence

Use dedicated development accounts and harmless fictional tools, with explicit fixture labels. No employer or real neighbor data is needed.

1. Configure Clerk development JWT integration and Convex development environment. Set the admin allowlist before the admin user’s first profile creation. Run the actual app with its public keys; keep server secrets out of browser code and logs.
2. As signed-out, inspect the public catalog’s actual network result: only names/categories. Anonymous direct mutations must fail.
3. User A lists a fixture drill. User B requests dates. User C must be unable to edit/delete A’s listing or decide A/B’s request, including by direct RPC.
4. A approves B’s request. Try an overlapping approval and an exact duplicate request; both must fail. B or A marks it returned. Inspect persisted history and availability.
5. Admin views all loans, marks A trusted and retires a listing; ordinary users must fail those privileged calls. An unrelated admin still cannot mark another person’s loan returned.
6. Start n8n from the pinned Compose file, import and link all three exports as documented, run controlled webhook verification, and only then activate request/reminder workflows. Use an authenticated HTTPS endpoint reachable from Convex.
7. Create one request and capture its outbox ID, workflow execution ID, verified raw signature, one `event:<id>` activity and acknowledged state. Replay the exact callback; one activity record must remain.
8. Use an explicitly isolated fixture loan with an elapsed due date. Run the reminder workflow manually, observe one `reminder:<loanId>:<UTC-day>` record, then rerun: no second reminder for that day. Returned/non-overdue loans must not be returned by the endpoint.
9. Interrupt a worker after lease acquisition; observe recovery after lease expiry plus the five-minute cron. Interrupt after the durable acknowledgment, and confirm no new logical notification on restart. Simulate callback failure/HTTP timeout without deleting the existing accepted record.
10. Confirm exhausted retries and workflow errors are visible and sanitized. Record the actual provider evidence before marking this sequence passed. Do not infer success from HTTP health or test counts.
