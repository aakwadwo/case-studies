# Campus Dash

**Product of:** [KAS Maven Consult](https://www.linkedin.com/company/kas-maven-consult/) ·
**Role:** primary designer and developer · **Status:** in use with real users ·
**Source:** private

## Problem

Food ordering on a university campus: customers order from approved vendors
and either collect the order or have it delivered by a verified student to a
fixed campus location. Money, order state and permissions have to stay
correct when customers, vendors, delivery partners and administrators act on
the same order at the same time.

## Engineering decisions

- **The server is authoritative.** Prices, fees, order state, payment state,
  roles and permissions are never taken from the client.
- **State changes happen in the database.** An order moves from one state to
  the next through a conditional update in PostgreSQL, so of two competing
  requests exactly one succeeds. A rejected change is recorded rather than
  forced through.
- **Order, payment and delivery are tracked separately.** A failed delivery
  does not mark the food order as failed, and support staff can see exactly
  which part went wrong.
- **Money is stored as integers in the smallest currency unit,** and every
  money-related operation is idempotent, so a retried request or a duplicate
  provider callback cannot apply twice.
- **External providers sit behind interfaces.** Payments and SMS each go
  through an interface, so a provider can be replaced without touching
  ordering or settlement logic.
- **Access is enforced in the database** with Postgres Row Level Security for
  each role, and every manual admin action is written to an audit log.

## Lessons

- A provider accepting a message is not the same as the message being
  delivered. Treating those as separate facts made notification problems
  diagnosable.
- When a workflow changes, existing records still refer to the old states.
  Retiring a state value has to account for the data that already uses it.
- Versioned SQL migrations, together with tests that run against a local
  Postgres, made it safe to change the schema of a system already in use.

## Stack

Next.js (App Router), React, PostgreSQL via Supabase (Auth, Row Level
Security, Storage), Tailwind CSS, `node:test`.
