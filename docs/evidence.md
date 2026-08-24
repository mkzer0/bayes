# Where the numbers come from

`data/library.json` holds the selectable starting beliefs and signal strengths. This document
records what each number rests on, and — more importantly — where the evidence runs out.

## The headline finding

**Priors can be sourced from published research. Likelihoods essentially cannot.**

There is a substantial literature on how often software bets pay off: experiment win rates,
IT programme overruns, business survival. Those give defensible starting beliefs.

There is no published table of `P(signal | success)` and `P(signal | failure)` for product
discovery signals. Nobody has run the study: it would require a large sample of bets, a
consistent record of which signals appeared before the decision, and an agreed definition of
success measured years later. What exists instead is a well-argued *rank ordering* of signals
by how costly they are to fake.

So the library separates the two, and labels every entry:

| Provenance | Meaning |
| --- | --- |
| `published` | Traces to a named study with a stated sample size. |
| `anchored` | Judgement, calibrated so the implied likelihood ratio lands in a defensible band on a published interpretation scale. |
| `local` | A placeholder for a number only your organisation can supply. |

Presenting `anchored` numbers as if they were measured would be exactly the false precision
this tool exists to argue against. The rank ordering is the substantive claim; the specific
decimals are starting points to be replaced.

## Priors

### A change to a mature, well-optimised product — 0.15

Optimizely reports **12% of 127,000 experiments** across ~1,100 companies won on the primary
metric. Kohavi & Thomke put Google, Bing and Microsoft at **10–20%** positive. CXL/Convert
found **20% of 28,304 experiments** reached significance.

These are per-experiment win rates on a chosen metric, not project success rates — a lower bar
in one sense (a metric, not a business outcome) and a higher one in another (statistically
significant, not merely "seemed fine").

### A change to a younger or less-optimised product — 0.33

Kohavi's summary of twelve years of Microsoft A/B tests: about **one third positive and
significant, one third flat, one third negative and significant**. The gap between this and
Bing's much lower rate is itself the lesson — the better-tuned the product, the harder a win
is to find. Teams new to experimentation typically do worse than the one-third rule, not
better, because their weakest ideas were never filtered out.

### A large IT programme delivering promised value — 0.20

McKinsey with the BT Centre for Major Programme Management at Oxford, **5,400+ projects over
$15M**: on average **45% over budget, 7% over schedule, and 56% less value than predicted**;
roughly half massively blow their budgets. The 0.20 is an interpretation of "delivered close
to what was promised" — the study reports a value shortfall, not a binary success rate.

### An IT project avoiding catastrophic overrun — 0.83

Flyvbjerg & Budzier, **1,471 IT projects**: average cost overrun 27%, but **one in six was a
"black swan"** with a 200% average cost overrun and a ~70% schedule overrun. This is the base
rate for avoiding ruin, not for succeeding — the right prior when the question is "could this
sink us", the wrong one when it is "will this pay off".

### A new venture still trading after five years — 0.50

US Bureau of Labor Statistics survival series: about **50% fail within five years**, ~20% in
year one, ~65% by year ten; the information sector is worse at ~53% by year five. Survival is
a low bar and a poor proxy for the funded bet paying off.

### Your own portfolio's hit rate — unset, deliberately

Take the last 20 bets your organisation funded that are far enough along to judge, and count
how many delivered the benefit named in the business case. That fraction beats every number
above, because it carries your selection process, your market and your delivery capability.

Judge them against what was claimed at funding time, not a story rewritten afterwards.

### A note on the Standish CHAOS figures

The widely-quoted "only 16% of projects succeed" line comes from Standish's CHAOS reports.
**It is deliberately excluded from this library.** Eveleens & Verhoef, applying Standish's own
definitions to 5,457 forecasts across 1,211 real projects, found the definitions rest solely
on estimation accuracy, are one-sided, reward bad estimation practice, and average numbers
carrying unknown bias. The sample frame is unpublished and the raw data proprietary.

A tool about honest evidence should not open with a statistic that fails its own test.

## Signal strengths

Every signal is `anchored`. The method:

1. **Rank by commitment currency.** Rob Fitzpatrick's argument in *The Mom Test* — compliments
   are free, money is not — is the most defensible ordering available. A signal is strong
   in proportion to what it cost the other party to send.
2. **Pick a defensible band**, using Jaeschke et al. (1994), *Users' Guides to the Medical
   Literature*, JAMA — the standard scale for reading a likelihood ratio:

   | LR | Shift in belief |
   | --- | --- |
   | > 10 | Large |
   | 5–10 | Moderate |
   | 2–5 | Small |
   | 1–2 | Negligible |

3. **Read off rates that land in that band**, rather than picking two numbers and discovering
   the strength afterwards.

The resulting order, strongest first:

| Signal | LR | Band |
| --- | --- | --- |
| Design partner pays for a pilot | 11.7 | Strong |
| Users switch from their workaround | 6.3 | Moderate |
| User invites a teammate | 3.8 | Weak |
| Users complete the workflow | 3.6 | Weak |
| User shares real data / repeat usage | 3.0 | Weak |
| Ship a thin slice in the timebox | 2.7 | Weak |
| Design partner signs an LOI | 1.9 | Negligible |
| Support load stays manageable | 1.4 | Negligible |
| Cycle time improves | 1.3 | Negligible |

Three placements are deliberate teaching choices:

- **The paid pilot and the LOI sit six times apart.** Same customer, same enthusiasm, same
  meeting. One costs money and one costs a signature. This pair is the fastest way to show a
  room what diagnosticity means.
- **Cycle time is parked below the vanity line.** It is a real operational metric and a poor
  test of whether a bet pays off; teams reliably get faster at building things nobody wants.
- **Shipping in the timebox is weak, not strong.** It is capability evidence, not demand
  evidence — useful for confirming delivery risk is not the binding constraint, and not much
  else.

### Signals whose published support is weaker than it looks

- **Retention.** A flattening retention curve is the most-cited behavioural marker of fit, and
  the benchmarks in circulation (Lenny Rachitsky and Casey Winters: ~25%+ day-30 for consumer,
  ~35%+ for B2B) are practitioner aggregates, not studies. The library rates one week of repeat
  usage as a weak proxy; the real signal needs a curve flattening at week 4–8.
- **The Sean Ellis 40% test.** Introduced in 2009 from pattern-matching across roughly 100
  startups. It is a threshold on a survey metric, not a base rate, and it inherits
  self-selection bias — it surveys people who stayed, and under-samples the churned users whose
  behaviour actually defines fit. Not included as a prior.
- **Fake-door and smoke tests.** Clicks measure curiosity, not commitment; novelty inflates
  early results. Any fake-door signal belongs near the bottom of the ordering above unless it
  is paired with a price and a real conversation.

## Replacing these with your own numbers

The tool gets meaningfully better once a team stops using the defaults:

1. Keep a log: for each funded bet, which signals were present at decision time, and what
   happened.
2. After ~20 bets, count directly. Of the ones that worked, what fraction showed signal X?
   That is your sensitivity. Of the ones that failed, what fraction *still* showed signal X?
   That is your false positive rate.
3. Replace the entry and mark it `provenance: "measured"`.

Step 2 is usually where an organisation discovers its favourite metric has a likelihood ratio
of about 1.

## Sources

- Kohavi & Thomke, *The Surprising Power of Online Experiments*, HBR 2017 — https://hbr.org/2017/09/the-surprising-power-of-online-experiments
- Kohavi, *Unexpected Results in Online Controlled Experiments*, KDD Explorations — https://kdd.org/exploration_files/v12-02-8-UR-Kohavi.pdf
- A/B test win-rate benchmarks (Optimizely, VWO, CXL/Convert) — https://www.conversionteam.com/ab-test-win-rate/
- Flyvbjerg & Budzier, *Why Your IT Project May Be Riskier Than You Think* — https://arxiv.org/abs/1304.0265
- Bloch, Blumberg & Laartz (McKinsey / Oxford), *Delivering large-scale IT projects on time, on budget, and on value* — https://www.mckinsey.com/capabilities/tech-and-ai/our-insights/delivering-large-scale-it-projects-on-time-on-budget-and-on-value
- US BLS business survival data, summarised — https://www.lendingtree.com/business/small/failure-rate/
- Eveleens & Verhoef, *The Rise and Fall of the Chaos Report Figures* — https://www.cs.vu.nl/~x/the_rise_and_fall_of_the_chaos_report_figures.pdf
- Jaeschke et al. (1994), likelihood ratio interpretation scale, discussed in — https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8086426/
- Flyvbjerg, reference class forecasting — https://www.sciencedirect.com/science/article/pii/S2666721523000248
- Retention benchmarks, Lenny Rachitsky — https://lennyrachitsky.wiki/articles/retention
- Sean Ellis PMF score, origin and limitations — https://learningloop.io/glossary/sean-ellis-score
- Rob Fitzpatrick, *The Mom Test* (2013) — commitment currency
