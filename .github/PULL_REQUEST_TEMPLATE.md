<!-- PR template -->
**Ticket:** <!-- Hyperlink the issue this PR closes, or state why there isn't one -->

## Why

<!-- What problem does this solve, or what requirement does it fulfill? One sentence is usually
enough if there's a linked issue — it should contain the details. -->

## What

<!-- What changed? Bullet points of the approach taken. -->

### Blast radius

<!-- e.g. role variables / template output / supported platforms / CI config -->

### Deviations from spec

<!-- Bullet points: what, if anything, changed between the issue's description and the final
implementation, and why. Delete this section if there's no spec to deviate from. -->

## Reading guide

<!-- Optional for a small change. For anything touching more than one file, map it for reviewers
before they read the diff. -->

| File | What to look at | Why |
|---|---|---|
| `` | … | … |

## Test plan

**Automated tests added/updated:**
- [ ] …

**Manual verification steps:**
1. …

## Deploy notes

<!-- Delete this section if not needed. -->

**Key/secret changes:** <!-- new secrets to provision, keys to issue, or config to update? -->
**Rollout order:** <!-- any sequencing required (e.g. migrate before deploying)? -->
**Rollback:** <!-- how to revert if this causes a problem in production? -->

## Checklist

- [ ] `ansible-playbook <playbook> --syntax-check` passes
- [ ] Functional test suite (see [AGENTS.md](../AGENTS.md#commands)) run locally, or explained why not
- [ ] Docs updated (`README.md`/`AGENTS.md`) if this changes role variables, supported platforms, or how the role is tested
- [ ] No secrets, tokens, or real customer/financial data included
