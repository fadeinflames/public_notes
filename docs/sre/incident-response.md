---
title: Incident Response
layout: default
parent: SRE
nav_order: 10
---

# Incident Response

## First 10 minutes

- Confirm user impact.
- Identify affected services, regions, tenants, and time window.
- Assign incident commander and communications owner.
- Open the incident channel or bridge.
- Pin the current hypothesis, next action, and owner.

## Triage checklist

- What changed recently?
- Is the failure global or scoped?
- Are dependencies healthy?
- Are errors, latency, saturation, or traffic patterns abnormal?
- Is rollback safer than forward fix?

## Communication

- State known impact plainly.
- Separate facts from hypotheses.
- Give regular updates even when there is no fix yet.
- Record decisions and timestamps for the postmortem.

{: .warning }
Avoid silent parallel debugging during active incidents. Make the next action and owner visible.
