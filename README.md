# LendLoop — Fluence take-home source

**Borrow what you need. Lend what you don’t.**

This repository contains the complete LendLoop source as a ZIP archive, plus the exported n8n workflows and the authorship/validation notes. It is a source submission, **not a deployed application**.

## Download the application source

[Download LendLoop-Source-2026-11-18.zip](https://github.com/Allenfox1716/fluence-lendloop-allen-fobbs/raw/refs/heads/main/LendLoop-Source-2026-11-18.zip)

The archive contains all **54 source files**, including Next.js App Router/TypeScript, Clerk integration, Convex functions/schema, tests, Docker Compose, n8n Code-node sources and exports, and full setup/security documentation.

**Extract the ZIP before running commands. The application is not at this repository’s root.**

```sh
unzip LendLoop-Source-2026-11-18.zip
cd fluence-lendloop
npm ci
cp .env.example .env.local
```

Follow the **README inside the extracted `fluence-lendloop` folder** to configure dedicated development Clerk/Convex accounts and n8n. No keys, passwords, live environment files, applicant résumé or real neighbor records are included.

After configuration:

```sh
npm run dev
```

For local checks, from the extracted application folder:

```sh
npm run check
```

## Exported n8n workflows

The same workflow exports are also available individually here:

- [Request notification](request-notification.json)
- [Daily overdue reminders](overdue-reminders.json)
- [Sanitized error workflow](errors.json)

They are **inactive exports**, not a claim that an n8n engine has imported or executed them. The ZIP contains `n8n/README.md`, Docker Compose and the exact import/linking instructions. Notification mode is a deliberately **logged mock**, as allowed by the exercise; it does not send email or SMS.

## What the source implements

- Public catalog restricted to tool names/categories; authenticated listings and category filtering.
- Borrow requests, inclusive date ranges, owner approval/decline, participant returns and borrower cancellation.
- Convex-enforced ownership/admin permissions, trusted-lender badges and protected loan history.
- Transactional overlap/duplicate guards, durable outbox, bounded retries, sender leases and interrupted-delivery recovery.
- Raw-body HMAC authentication, minimal automation payloads, idempotent acknowledgments/daily reminders and per-loan activity.

## Observed validation and remaining limits

Strict TypeScript, **23 Convex/domain/signature tests**, **17 n8n Code/export/wire tests**, the optimized Next.js build and a local production HTTP smoke passed. The production dependency audit reported zero known vulnerabilities when run.

**Live Clerk sign-in, hosted Convex execution, actual n8n-engine import/run, hosted integration, authenticated visual review and real restart/crash recovery have not been validated.** Docker was not installed on the test host. No external notification delivery is claimed.

See [VALIDATION.md](VALIDATION.md) for the exact observed checks and controlled live acceptance sequence. A passing local build/test is not a provider-backed deployment.

## AI assistance and source integrity

This exercise was created with substantial AI assistance, which the posting permits. [AUTHORSHIP.md](AUTHORSHIP.md) records that assistance and makes no claim that this was Allen’s past employment or a previously deployed portfolio project.

- Original source commit: `698ef779317f6f122347a3428c1c21ea29b7f0a6`
- ZIP SHA-256: `6549be67306bfe1b2776e04fb445348da8e612170fea7cefd6e6bf0e96bea634`
- ZIP size: `87,852` bytes

The archive packaging preserves the original complete source and tests. The repository’s packaging README is separate from the full application README inside the ZIP.
