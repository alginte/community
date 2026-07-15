# Security Policy

Alginte connects to and manages Kafka clusters, so we take security seriously and
appreciate responsible disclosure.

## Reporting a vulnerability

**Please do _not_ open a public issue for security vulnerabilities.**

Report privately, one of two ways:

1. **GitHub private vulnerability reporting** — use the **"Report a vulnerability"**
   button on this repo's [Security tab](https://github.com/alginte/community/security)
   (preferred; keeps the report private until a fix ships).
2. **Email** — `security@alginte.com` with steps to reproduce and impact.

Please include: affected version (from the in-app **About** page), a clear
reproduction, and the potential impact.

## What to expect

- We aim to acknowledge reports within a few business days.
- We'll keep you updated on the fix and coordinate a disclosure timeline.
- Credit is given to reporters who wish to be named, once a fix is available.

## Scope note

Alginte is designed to run on a **trusted-network / localhost boundary** and is
unauthenticated by default (an optional single-account / OIDC login can be
enabled). Reports that assume the app is exposed on an untrusted network without
that boundary or login are expected behavior, not vulnerabilities — but if you
find a way to cross the intended boundary, we want to hear it.
