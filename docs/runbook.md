# Deployment Runbook

This runbook is the single source of truth for deploying changes from `main` to production. It is intentionally short: every section answers a question that comes up during a real deployment.

## Overview

Deployments ship from the `main` branch. Never deploy from a feature branch or from a fork. If `main` is red, stop and escalate rather than deploying around a failure.

## Pre-deployment checks

Before starting a deployment, confirm all of the following:

- The pull request that introduced the change is fully merged into `main`.
- Required status checks are green on the last commit of `main`.
- No open dependency alerts are blocking the release.
- A second teammate is reachable during the deployment window.

## Deployment steps

1. Announce the deployment in the team channel, including the commit SHA being shipped.
2. Tag the release with the version being deployed.
3. Apply the change and watch the health-check endpoints for ten minutes.
4. Confirm the new revision is visible in the logs.
5. Post a go/no-go summary in the team channel.

## Rollback

If a deployment misbehaves, roll back first and investigate second.

1. Re-deploy the previous known-good release tag.
2. Confirm the health-check endpoints return to normal within five minutes.
3. Record the rollback in the incident log, including the SHA that was pulled and the SHA that was restored.
4. Do not attempt a forward fix unless the on-call leads it explicitly and a rollback is impossible.

A rollback is a normal outcome, not a failure. Never hesitate to use this section.

## Escalation

If you are stuck, escalate. Do not sit on a broken deployment.

- **Primary on-call:** deployment rotation owner for this week.
- **Secondary on-call:** next person on the rotation.
- **Team lead:** reach out if both on-call contacts are unavailable or the decision is above the rotation's authority.
- Keep the escalation thread in the deployment channel so the next owner has context.

When escalating, include the commit SHA, what you observed, and what you have already tried.

## Post-deployment

- Leave the health-check endpoints under watch for one hour.
- Update the runbook if the deployment surfaced anything this document missed.
- Hand over open issues to the next rotation owner in writing.
