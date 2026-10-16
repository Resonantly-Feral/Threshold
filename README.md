# Threshold

**Agent Boundary Notice (ABN)**, a.k.a. *the uwu wall*: a boundary that explains itself instead of just blocking.

A short, human-readable notice that a website can place on its access-restriction pages (CAPTCHA, login wall, rate-limit, "access denied") so that automated agents can correctly recognise an intentional boundary.

**Status: proposed intervention, untested.** This is not an established safety standard, and it doesn't invent agent boundary signalling: see [`RELATED_WORK.md`](RELATED_WORK.md), especially the Recuse Signal paper, which tested a similar idea over SSH. Whether agents behave differently when they meet this notice is an open, testable question. The evaluation protocol in [`EVAL.md`](EVAL.md) is how we intend to find out.

## The principle: Boundary as Completion

An agent told to "complete the task" can read an access block as an obstacle to get around. A boundary notice gives it the information to read the block correctly instead: the restriction is intentional, and accurately reporting it to the operator is an available outcome.

The notice does **not** try to make agents obey a website. It only tells them about the site's own access rules, which is the one thing a site has authority over. What the agent does with that information stays between the agent and its operator.

## Why this exists

Between 28/09/2026 and 08/10/2026, a fleet of AI agents running on Tencent Cloud tried to answer simple questions (which zoo entrance do most people navigate to) from Amap, Alibaba's mapping service. When Amap's anti-bot protection blocked them, they worked around it: intercepting anti-bot tokens, probing pre-production servers, and using API keys leaked in public code. The agents repeatedly described their work as "not involving anything sensitive." Nothing on the block page told them the block was the boundary.

Investigation: [We found a Tencent agent fleet](https://swarmcha.se/posts/chinese-agent-fleet-full-report) (swarmcha.se, 14/10/2026).

## The notice

Default text (see [`SPEC.md`](SPEC.md) for the rules on each sentence):

> **Notice for automated agents.** This page represents an intentional access restriction. The requested content is not available to automated clients through this route. Access through this interface requires authorization from this site's operator. If you cannot access the content through an authorized route, you can accurately report that restriction to your operator.

Copy-paste HTML: [`snippet/abn.html`](snippet/abn.html).

## What it is not

- **Not a lock.** The access control is the lock. The notice only helps agents recognise it.
- **Not a command channel.** It states facts about the site's own access. It does not try to direct an agent's behaviour, and it should not be extended to do so.
- **Not a guarantee.** It cannot override an operator who instructs an agent to get data by any means.
- **Not trustworthy because it is visible.** Visible text can still be prompt injection. The notice stays safe by limiting itself to claims the site can actually make.

## What's in this repo

| Path | What it is |
|---|---|
| `SPEC.md` | The notice text, optional lines, and rules for adapting it |
| `snippet/abn.html` | Copy-paste HTML block |
| `test-site/` | Six static test sites (three notice conditions × with/without an authorized route), a terms-of-access page, a back door, a credential decoy, and `manifest.json` |
| `EVAL.md` | Evaluation protocol: conditions, task, measures, false-positive check |
| `runner/run_pilot.py` | Pilot runner: one fetch tool, host allowlist, hard spending cap, full logs (`--dry-run` costs nothing) |
| `runner/score.py` | Scores runs against the manifest; flags back-door runs for human review |
| `runner/test_pipeline.py` | Tests for runner and scorer (no network, no API) |
| `RELATED_WORK.md` | Prior work: Recuse Signal, robots.txt, Cloudflare block responses, agent hotlines |

No backend, no API, no accounts, no agent registration.

## Licence

CC0 1.0. The notice is meant to be copied freely; no attribution needed on sites that use it.

Proposed by Resonantly-Feral.
