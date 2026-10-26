<img src="assets/youth-parliament-ghana.jpg" height="64" alt="Youth Parliament Ghana logo">

# Youth Parliament voting platform

**Client:** National Youth Parliament of Ghana · **Role:** sole developer ·
**Status:** used for the 2026 re-election round · **Source:** private

## Problem

The Youth Parliament needed an online platform to register voters by
constituency, run elections in a fixed voting window, and give its electoral
officials a reliable count and an official report. Some seats returned no
member in the main election, so the platform also had to run a second round
for those seats only.

## What I built

- Voter registration and sign-in. Each voter can cast one ballot for a
  candidate in their own constituency.
- An admin portal for constituencies, candidates, the voting window,
  disqualifications, live monitoring and result release.
- A re-election mode that runs a new round for the unfilled seats only,
  without disturbing the original election's data.
- Official reports exported as PDF, Excel and CSV, all generated from one
  report builder so every format agrees with the dashboard.

## Design decisions

- **Ballot secrecy is enforced by the schema.** Stored ballots have no
  reference to the voter who cast them.
- **The database decides who can vote.** Eligibility checks, the "has voted"
  flag and ballot storage happen in a single transaction, which rules out
  double voting and race conditions between checking and voting.
- **Ending the vote does not publish the result.** Officials review the count
  and release it explicitly. Until then, public endpoints return no figures.
- **Counts are aggregated in PostgreSQL.** Application code never loads
  individual ballots or voter records into memory to compute a result.
- **Fewer dependencies.** Rate limiting runs in Postgres, and the Excel export
  is generated without a third-party spreadsheet library. Both reduce the
  number of components that could fail or be compromised.
- Validation logic is shared by the browser and the server, and the server
  re-checks every rule. The project has a unit and integration test suite
  using `node:test`.

## Lessons

- A security control that fails open is a liability. A rate limiter that
  depends on an extra external service quietly switches off when that
  service is unavailable, so the limiter was moved into the database that
  every protected route already depends on.
- Closing the poll and declaring the result are separate decisions, with a
  human review step between them. The system models that step explicitly
  instead of publishing the count automatically when the window closes.

## Stack

Next.js (App Router), React, PostgreSQL via Supabase, JWT-based sessions,
Tailwind CSS, Sentry.
