---
name: product-discovery
description: Use when the user wants to validate ideas before building, run product experiments, reduce risk on a new feature, or decide whether a problem is worth solving. Also use when the user mentions 'product discovery,' 'validate before building,' 'opportunity solution tree,' 'prototype testing,' 'hypothesis testing,' 'dual-track,' or 'de-risking a feature.'
---

# Product Discovery

Discovery is the work done before building to ensure the right problem is solved and the right solution is chosen. The cost of discovery is days. The cost of building the wrong thing is months.

## The Four Discovery Risks

Every product bet carries four types of risk. Discovery must reduce each one before build begins.

| Risk | Question to answer |
|---|---|
| **Value risk** | Will customers find this valuable enough to use / pay for? |
| **Usability risk** | Can customers figure out how to use it? |
| **Feasibility risk** | Can we build this in a reasonable timeframe? |
| **Viability risk** | Does this work for the business? (legal, revenue, ops) |

Most teams only test value and feasibility. Usability and viability failures surface at launch.

## Opportunity Solution Tree (Teresa Torres)

Structures the relationship between outcomes, opportunities, and solutions:

```
Desired Outcome (the product goal)
├── Opportunity 1 (customer problem/need)
│   ├── Solution A
│   └── Solution B
├── Opportunity 2
│   ├── Solution C
│   └── Solution D
└── Opportunity 3
```

Rules:
- Start with the outcome, not the solution
- Opportunities are customer problems — not features
- Each solution must be testable before building
- Never evaluate a solution without first naming the opportunity it addresses

## Discovery Techniques by Risk Type

**Value risk:**
- Customer interviews ("would you use this" is not enough — test with a prototype)
- Concierge test (do the job manually for 5 customers before automating it)
- Fake door test (landing page with signup before building)

**Usability risk:**
- Prototype walkthroughs (no guidance — watch where they get stuck)
- Hallway testing (5 people, one afternoon, reveals most usability problems)
- First-click test (where do users click first? Does it match intent?)

**Feasibility risk:**
- Spike / technical prototype (not a UI — a proof of concept for the hard engineering problem)
- Engineering estimate with 2 approaches compared

**Viability risk:**
- Legal / compliance review before full build
- Finance modeling (unit economics at target scale)
- Ops review (support load, infrastructure cost)

## The Experiment Hierarchy (Weakest to Strongest Evidence)

1. Expert opinion / desk research
2. Customer interview (what they say)
3. Survey (what they say at scale)
4. Prototype test (how they behave with a mock)
5. Concierge test (how they behave with the real service)
6. A/B test on live product (how they behave at scale)

For high-risk bets, run experiments at level 4+. For low-risk improvements, level 2-3 is enough.

## Discovery Cadence

**Weekly:** 1-2 customer interviews or usability sessions
**Sprint:** Minimum 1 experiment that reduces the top risk on the next big bet
**Quarterly:** Review opportunity solution tree — are we working on the right opportunities?

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "We don't have time for discovery" | You don't have time to rebuild a feature you didn't validate. Discovery is the time saver. |
| "We already know the solution" | You know a solution. Discovery finds the best one. |
| "Our customers are asking for this feature" | Customer requests are opportunities, not solutions. Test the job before committing to the feature. |
| "A prototype test isn't realistic enough" | Prototypes reveal usability problems. You don't need production fidelity to find them. |

## Verification

- [ ] Opportunity solution tree exists for the current product goal
- [ ] All four risk types assessed before committing to build
- [ ] At least one experiment run per major new bet before sprint starts
- [ ] Discovery insights traceable to specific decisions in PLAN.md
- [ ] "We already know" claims backed by level 3+ evidence, not assumption
