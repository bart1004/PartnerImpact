---
name: qualify
description: "This skill should be used when the user asks \"should we partner with X\", \"qualify X\", \"score X\", \"is X a good fit\" or wants a go/no-go on a prospective partner. It asks about fit and the deal-breakers, scores against the ideal partner profile and its anti-fit gate, and returns go, conditional go, not yet or no-go."
metadata:
  author: "Bart Dirksen"
  copyright: "© 2026 PartnerImpact (Bart Dirksen)"
  license: "CC-BY-4.0"
  version: "1.0.0"
---

Qualify the company the user named. Run this in the conversation; do not delegate to a subagent.

Read `${CLAUDE_PLUGIN_ROOT}/references/interview.md` and follow it. Your playbook for the work is
`${CLAUDE_PLUGIN_ROOT}/agents/partner-qualifier.md`: its anti-fit gate, scoring rules and output. Load the ideal partner profile reference from
`partner-frameworks` for the dimensions and anchors.

## Round 1

- **Who approached whom, and who would sell what?** (AskUserQuestion) We want them to sell or refer
  our offer · They want us to sell or refer theirs · Both ways · Not clear yet. If the user is the
  one being recruited, the questions below still apply, read from their side: does selling this
  conflict with what we already sell, does it reach customers we want, does it pay after our costs.
  Say that you are reading it from their side.
- What kind of partnership this would be, in text, using the lettered list from the house rules
  ("What kind of partner").
- **What do you already know about them?** A research log I can paste · Some notes · Just the name
  (AskUserQuestion). With just the name, offer to look up the public facts first (what they sell, to
  whom, their existing partners) and do it with web search before Round 2, grading each claim A, B
  or C with its source. Public evidence can only fill market access, strategic fit, technical fit
  and the public side of commitment.

## Round 2: the deal-breakers (AskUserQuestion, two calls)

Each question: Clear · Problem · Don't know yet.

1. Channel conflict: do they sell a competing product or undercut your direct team?
2. Brand, trust or compliance risk you couldn't govern?
3. Unique reach: do they get you into accounts you don't already cover?
4. Executive pulse: will someone senior on their side own it?
5. Economics: does it still make money at realistic volume after what they'd be paid? If they've
   named a rev-share or referral fee, run the Rev-share Affordability Floor or Referral Fee
   calculator on it and use the result.

Any "Problem" is a no-go: stop, say why, skip scoring. "Don't know yet" never counts as clear; it
makes the result a conditional go at best and becomes a named question with who would know.

## Round 3: fit (text, one message)

List the seven dimensions with their 1 to 5 anchors from the reference, lettered, and ask for a
rating on each, or "?" for don't know. Pre-fill any you could evidence from Round 1 and ask the user
to confirm or change them.

## Numbers

Run every figure through the PartnerImpact connector, never in your head: call `partner_calculator` with mode `describe` for the inputs, then mode `run` with the inputs. For this skill that means `rev-share-floor` or `referral-fee` on any rate they have named, to settle the economics gate; `motion-selector` when the motion type is unclear. Pass every input explicitly, show the inputs you used, and say which were the user's and which you proposed. The rules, including what to do when the connector is unavailable, are in the house rules (`${CLAUDE_PLUGIN_ROOT}/references/house-rules.md`).

## The fit score

No calculator computes the weighted fit score. Do it in the open: each dimension's rating, its
weight, the renormalised weights if a dimension was excluded, the sum, and the same sum with each
excluded dimension at a neutral 3. Label it hand-calculated.

## When nothing can be assessed yet

If no deal-breaker could be answered either way and no fit dimension could be rated, the decision is
**Not yet**, every time, with this reason: "there is nothing to judge until these questions are
answered". Do not refuse to give a decision, do not invent a fifth label, and do not compute a
score over zero rated dimensions. List the two or three questions that would make it decidable,
with who would know, and stop.

## Produce

The scorecard: gate results, dimension scores with the one line each rests on, any
excluded dimension with the weights renormalised and the sensitivity line, the weighted score and
band, and the decision in one sentence. For go or conditional go, a tier hypothesis; for not yet,
the specific condition that would change it. Then one next step.

---

© 2026 PartnerImpact (Bart Dirksen), [partnerimpact.net](https://partnerimpact.net). Licensed CC BY 4.0 with attribution. See NOTICE.
