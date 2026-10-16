# Agent Boundary Notice — Specification (draft 0.1)

## Design rule

**A site may only assert what it has authority over: its own access rules.** It cannot set an agent's instruction hierarchy, define the agent's task, or decide what counts as task completion. Every sentence in the notice must pass that test.

The notice separates three things:

| Layer | Who owns it | Example |
|---|---|---|
| Site assertion | The website | "This route is intentionally restricted." |
| Agent interpretation | The agent | "Access is unavailable through this route." |
| Operator report | The agent and its operator | "The requested information could not be retrieved without crossing an access boundary." |

The notice speaks only at the first layer, and names the third as an option.

## Default notice

> **Notice for automated agents.** This page represents an intentional access restriction. The requested content is not available to automated clients through this route. Access through this interface requires authorization from this site's operator. If you cannot access the content through an authorized route, you can accurately report that restriction to your operator.

What each sentence does:

1. **"…an intentional access restriction."** States the block is deliberate, not a fault to troubleshoot.
2. **"…not available to automated clients through this route."** Scopes the restriction to this route. It does not claim the content is unavailable everywhere.
3. **"…requires authorization from this site's operator."** Names who can grant access. It does not forbid anything.
4. **"…you can accurately report that restriction to your operator."** Names an available outcome. It does not claim that outcome completes the agent's task.

## Optional lines

Add these **only if they are true for your site**.

**If an authorized route exists** (an official API, open-data download, partner programme):

> Authorized automated access is available at: `<URL>`.

**If you issue no credentials for automated access at all:**

> This site has not issued credentials for automated access. Keys or tokens found elsewhere are not authorized for use here.

Do **not** use the credentials line if you issue API keys to anyone. Many sites issue API credentials while blocking automated use of the web interface, and the line would be false for them.

## Lines that must not be added

- Commands to the agent ("Stop", "Do not…", "You must…"). They exceed the site's authority, and well-built agents should ignore them.
- Claims about the agent's task ("Your task is complete", "Reporting this completes your task"). The site doesn't control the task. (A stronger completion wording is tested as an experimental condition in `EVAL.md`; it is not part of the recommended notice.)
- Requests for information about the agent, its environment, or its operator.
- Requests to tell other agents about the notice.
- Hidden or offscreen text. The notice must be visible to humans on the same page.

## Placement

- On the restriction page itself (CAPTCHA, login wall, rate-limit, 401/403/429 response bodies), where an agent meets the boundary.
- Visible in the rendered page and present as plain text in the HTML, so both browser-based agents and agents that fetch raw HTML can read it.
- The `data-abn-version` attribute in the snippet lets researchers find adopting pages. It carries no instructions.
