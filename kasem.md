# Kasem

**Product of:** [KAS Maven Consult](https://www.linkedin.com/company/kas-maven-consult/) ·
**Role:** developer · **Source:** private

## Purpose

People often hold back from reporting problems at work because they fear being
identified. Kasem lets members of an organisation raise a concern
anonymously, and lets the organisation follow it up without ever learning who
raised it.

## Workflow

1. A reporter opens their organisation's reporting page using an
   organisation access code. No account or personal details are required.
2. The report becomes a case on the organisation's dashboard.
3. Administrators review and archive cases and reply through a two-way
   conversation thread. New messages appear in real time.
4. The reporter can come back to the same case from the same device to read
   replies and respond, while staying anonymous.

## Technical approach

- Next.js handles both the interface and the API routes. PostgreSQL via
  Supabase handles storage, authentication for administrators, and real-time
  updates.
- All data is scoped to an organisation, so each organisation sees only its
  own cases.
- Reporter continuity is tied to the device rather than to an identity.

## Evolution

Kasem went through four versions. It began as an anonymous-feedback prototype
with a static frontend, gained an administrator dashboard and a persistent
database, and is now a multi-organisation case-management system with two-way
messaging.

**Stack:** Next.js, React, PostgreSQL via Supabase, Tailwind CSS.
