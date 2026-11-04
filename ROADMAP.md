# Development Roadmap — Freelancer Marketplace Billing

## Project Overview

Freelancer Marketplace Billing is a TypeScript-based, multi-tenant billing system for freelancer marketplaces. The existing proof of concept models organizations, entities, teams, and users, with pay-as-you-go charges, prepaid credits, seat-based subscriptions, and marketplace-event processing.

The next phase aims to evolve this proof of concept into a reliable, extensible open-source reference for marketplace billing workflows. The focus is on correctness, tenant isolation, developer experience, and maintainable integrations—not on handling real customer financial records. Development and demonstrations will use synthetic data.

**Status note:** The existing README and project notes differ on the completeness of some capabilities (including credit expiration and mid-cycle proration). Phase 1 will verify actual behavior against the code and tests, and publish an accurate feature-status matrix. Items below are *planned work*, not claims of completed functionality.

## Planned Work — Estimated 400 Hours

| Phase | Focus | Estimated hours |
| --- | --- | ---: |
| 1 | Baseline audit, documentation reconciliation, and test foundations | 40 |
| 2 | Tenant boundaries, hierarchical access control, and authorization tests | 60 |
| 3 | Marketplace event reliability, idempotency, and concurrency | 65 |
| 4 | Credit lifecycle and subscription billing correctness | 70 |
| 5 | Configurable billing policies and developer-facing APIs | 45 |
| 6 | End-to-end test environments and performance validation | 50 |
| 7 | Observability, operational workflows, and resilience | 35 |
| 8 | Documentation, reference integrations, and release stabilization | 35 |
| **Total** | | **400** |

Hours are planning estimates for independent development, not a commitment by any third party to fund or approve them.

### Phase 1 — Baseline Audit & Test Foundations (40h)
- Reproduce local setup with PostgreSQL and seed data.
- Run the existing test suite and record verified results.
- Reconcile `README.md`, `CLAUDE.md`, and `ADAPTATION_PLAN.md` against the implementation.
- Publish an implemented/partial/planned feature matrix.
- Review database migrations, Git ignore rules, environment examples, and dependency versions.
- Establish baseline integration tests for billing and tenant-sensitive operations.

**Exit criteria:** documented local setup; repeatable tests; an accurate status matrix; identified technical debt recorded as issues.

### Phase 2 — Tenant Boundaries & Authorization (60h)
- Review organization → entity → team → user scope throughout the API and data access layer.
- Enforce tenant scoping and cross-entity membership rules consistently.
- Refine role and permission checks for billing actions.
- Add negative-path tests for cross-tenant reads and writes.
- Document authorization decisions and permission propagation.

**Exit criteria:** automated cross-tenant isolation tests and documented permission behavior for core operations.

### Phase 3 — Event Processing Reliability (65h)
- Establish explicit event schemas and deduplication identifiers.
- Make marketplace-event billing effects idempotent under retries.
- Define transaction boundaries for balance mutations and event processing.
- Handle concurrent events, delayed delivery, and failure recovery.
- Add replay and reconciliation tooling for synthetic events.

**Exit criteria:** repeatable concurrent-processing tests showing no double application of the same event.

### Phase 4 — Credits & Subscriptions (70h)
- Verify current proration and credit-expiration behavior before extending it.
- Implement and test explicit credit expiration policies where missing.
- Refine seat-count changes, upgrades/downgrades, and mid-cycle proration.
- Define rounding, precision, timezone, and boundary-date rules.
- Add property-based and regression tests for monetary invariants.

**Exit criteria:** documented calculation rules and deterministic automated tests for key billing edge cases.

### Phase 5 — Configurable Policies & APIs (45h)
- Separate billing policies from transport and persistence concerns.
- Introduce configurable event pricing and entity-level billing settings.
- Improve request validation, error contracts, and API documentation.
- Add webhook event contracts using synthetic examples and local test receivers.
- Provide an example TypeScript integration client.

**Exit criteria:** documented APIs and examples for configuring and exercising each supported billing model.

### Phase 6 — E2E & Performance Validation (50h)
- Introduce Playwright-based end-to-end tests for core user journeys.
- Provision repeatable database and application environments in CI.
- Add load tests for event ingestion and concurrent credit consumption.
- Identify slow queries and improve indexing where supported by measurements.
- Track flaky tests and make failures reproducible.

**Exit criteria:** reproducible E2E and performance test commands with baseline results.

### Phase 7 — Observability & Resilience (35h)
- Standardize structured logs, correlation IDs, and actionable error messages.
- Add metrics for event processing, failed jobs, and billing operations.
- Improve retry policies and operational recovery for failed workflows.
- Document operational diagnostics and common failure modes.

**Exit criteria:** observability examples and a documented procedure to investigate synthetic failure scenarios.

### Phase 8 — Developer Experience & Release (35h)
- Consolidate architecture, setup, deployment, and API documentation.
- Create realistic but synthetic demo scenarios for each billing model.
- Document contribution workflow and supported use cases.
- Resolve release-blocking issues and prepare a versioned open-source release.

**Exit criteria:** a documented, reproducible release candidate with tests and examples.

## Longer-Term Product Directions

These are potential directions, **not** additional commitments within the 400-hour plan.

1. **Marketplace Billing Platform:** deepen multi-entity invoicing policies, team-level entitlements, and marketplace-specific billing workflows.
2. **Usage & Credit Ledger:** offer reusable metering, credit allocation, expiry, and reconciliation primitives independent of the marketplace UI.
3. **Billing Policy Simulator:** evaluate subscription, credit, and pay-as-you-go configurations using synthetic workloads before changing production billing policies.

## Working Principles

- Preserve the original Git history and clearly label the original take-home proof of concept.
- Use synthetic data only; never commit personal records, payment credentials, API keys, or customer financial data.
- Prefer correctness, isolation, traceability, and deterministic tests over feature count.
- Keep planned work distinct from completed, tested functionality.
- Add an accepted open-source `LICENSE` file only after confirming that the relevant intellectual-property rights permit it.
