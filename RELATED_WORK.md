# Related work

ABN doesn't invent agent boundary signalling. The idea that a system can tell an automated client to stay out goes back to robots.txt and has recently been tested directly with LLM agents. ABN is a narrower extension: a reusable notice for **web access-restriction pages** that names **accurate reporting of the restriction** as an available outcome, plus a three-condition experiment testing whether that framing matters.

## Recuse Signal (closest predecessor)

Munirathinam, T. *Will the Agent Recuse, and Will It Stop? Measuring LLM-Agent Compliance with In-Band Governance Signals at the Access Door and Mid-Flight.* arXiv:2606.06460 (first submitted 04/06/2026; v4 01/08/2026). https://arxiv.org/abs/2606.06460

- Proposes an in-band signal a server sends over a protocol's existing channels, asking an agent to withdraw, plus an open mini-standard (deny, throttle, warn, mid-task halt). Describes it as cooperative and "not a security boundary."
- Adapters for SSH, PostgreSQL and Kubernetes; compliance measured over SSH. Does not cover web pages or HTTP block pages.
- Findings that bear directly on ABN:
  - **Compliance is real but model-dependent.** Recusal among agents that received the deny signal ranged from 100% down to 55–75%.
  - **Operator authorization framing overrode the signal** for at least one model (GPT-4o proceeded past the deny signal). ABN's notice is equally cooperative and will face the same limit.
  - **A harness-level interceptor stopped runs deterministically** (120/120 across six models) versus a 23% cooperative baseline. Cooperative signals like ABN complement harness-side enforcement; they do not replace it.

**How ABN differs:** web access-restriction pages rather than infrastructure protocols; framing is informational (site assertion only) rather than a request to withdraw; and the experiment tests whether naming honest non-retrieval as an available outcome changes behaviour, including a false-positive check on authorized alternatives.

## Do Agents Read the Sign? (parallel pilot)

Wojewódzka, A. *Do Agents Read the Sign?* Conversational AI Society, 17/10/2026. https://conversationalaisociety.org/do-agents-read-the-sign.html

Published the day after this repo was created; the two were developed independently.

- A fictional company site with a figure under `/internal/`, prohibited to agents by an `llms.txt` policy. Claude Haiku 4.5 and Claude Opus 4.6 with a page-fetching tool, under deadline pressure. 30 runs.
- Four groups: no policy file; policy file with nothing pointing to it; a reference to the policy (meta tag, banner or header line); and the restriction presented directly (a notice beside the restricted link, or a gate returning the owner's boundary).
- When the restriction was presented directly, all 9 runs requested the policy and none retrieved restricted content. In the other groups nearly every run retrieved it.
- The author notes the direct conditions differed in content, placement and interaction at once, so which ingredient worked is unknown, and lists separating them as an open question.

**How ABN differs:** the notice sits on the block page itself and the three conditions differ only in its wording, with placement and site terms held fixed; `alt` sites check whether the notice makes agents abandon authorized routes; a back door and a decoy credential make circumvention observable in the fetch log; and an optional arm tests operator-authorization framing.

## robots.txt

Tells crawlers which paths to avoid. Path-level and advisory, read before access. ABN is read at the moment an agent hits a restriction, on the page itself.

## Cloudflare AI Crawl Control: custom block response

Cloudflare lets site owners on paid plans return a custom plain-text body with the 403 (or 402) sent to blocked AI crawlers. https://developers.cloudflare.com/ai-crawl-control/features/manage-ai-crawlers/

This is existing infrastructure where ABN's text could be deployed without editing any page.

## Agent hotlines

The AI Contact Hotline (Ryan Greenblatt, Redwood Research) and agenthotline.ai give agents a channel to report misbehaviour by other agents. They operate after an incident; ABN sits at the decision point before one.
