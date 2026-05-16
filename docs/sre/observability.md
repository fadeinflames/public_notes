---
title: Observability
layout: default
parent: SRE
nav_order: 20
---

# Observability

## Core signals

- Latency
- Traffic
- Errors
- Saturation

## Useful page types

- Service overview dashboard
- Golden signal dashboard
- Dependency dashboard
- SLO and error budget page
- Runbook-linked alert page

## Alert quality checklist

- The alert maps to user impact or imminent risk.
- The alert has an owner.
- The alert links to a runbook.
- The page explains how to validate the condition.
- The page explains when to escalate.

{: .tip }
Every alert should be either actionable, ticket-only, or deleted.
