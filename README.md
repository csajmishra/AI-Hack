# AI-Hack: Build AI Solutions on Azure

A four-day virtual workshop and individual hackathon for developers and architects with beginner C# skills.

**Status: event-planning documentation.** This repository contains the event design, not a runnable application or a ready-to-deploy starter kit. Starter projects, sample datasets, checkpoint branches, infrastructure templates, and helper scripts described in the plan are specifications for facilitators to implement and rehearse before delivery. Code snippets are illustrative, not compiled or SDK-version-validated examples.

## Event at a glance

| Item | Plan |
|---|---|
| Audience | 16 individual participants; beginner C#; no prior agent experience |
| Delivery | Virtual; four sessions of four hours |
| Tools | VS Code, GitHub Copilot, C#, Microsoft Agent Framework |
| Azure access | One subscription per participant |
| Project | One cumulative solution with a choice of customer support, HR helpdesk, or order processing |
| Budget | Planning estimate: USD 17-25 per participant for five days; verify regional rates and quota before delivery |

## Learning journey

| Day | Theme | Event plan |
|---|---|---|
| 1 | End-to-end agents + Azure AI Agent Service | [Build the first agent](event-plan/day-1.md) |
| 2 | Advanced RAG | [Ground answers in company knowledge](event-plan/day-2.md) |
| 3 | Multi-agent orchestration | [Build a specialist agent team](event-plan/day-3.md) |
| 4 | Azure App Platform and Integration Services | [Deploy, integrate, and showcase](event-plan/day-4.md) |

Start with the [complete event overview](event-plan/README.md) and [pre-event setup blueprint](event-plan/setup.md). Each daily plan includes learning objectives, a timed agenda, two business use cases, a Mermaid architecture, milestones, a starter-code outline, a rubric, prerequisites, and common pitfalls.

Day 1 combines agent fundamentals with the managed Agent Service runtime. Days 2 and 3 add retrieval and orchestration before Day 4 adds deployment and integration. Day 4 deliberately uses 30 minutes of learning, 105 minutes of hacking, and 105 minutes for all 16 showcases.

## Delivery readiness

Before advertising this as a hands-on-ready workshop, facilitators must:

1. Build the proposed C# starter kit and checkpoint branches; publish its real URL in the setup guide.
2. Pin a supported .NET SDK and compatible NuGet versions after a clean-subscription rehearsal. Prefer an actively supported LTS SDK; the .NET 9 references in the planning draft require updating for a future event.
3. Confirm model availability, regional quotas, RBAC, service support, pricing, and any preview-feature constraints.
4. Test all three scenario tracks, sample datasets, evaluation fixtures, provisioning, deployment, and teardown.
5. Enforce authentication, per-user authorization, explicit approval before writes, bounded agent execution, idempotent event processing, and redacted telemetry. Use synthetic data only.

Estimated business benefits in the use-case tables are illustrative opportunities, not measured results or contractual commitments. Azure budgets are alerts, not automatic spending caps. Optional tools, preview services, connectors, and retained resources can increase costs.

## Sharing

This repository is public and suitable for sharing as a workshop proposal and facilitation blueprint. It does not include customer data, credentials, or production deployment assets. No reuse license is granted by this publication; contact the repository owner to agree reuse terms.
