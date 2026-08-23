# Bayesian Product Discovery — an evidence updater for product bets

Static, interactive page for changing your mind on purpose. Start with an outside-view
prior on a hypothesis, then update it test by test as real signals come back.

The point is not the arithmetic. It is the question each input forces you to answer:
**how often would I see this signal even if the bet were going to fail?**

## Run locally

```bash
python3 -m http.server 8080
```

Open `http://127.0.0.1:8080/`.

## What the demo shows

Four steps, mirrored in the page layout:

1. **Starting belief** — the outside view. A base rate drawn from a reference class of
   comparable projects, not from how you feel about this one. Default `0.20`.
2. **Decision threshold** — the bar you will commit at, set *before* any evidence is on the
   screen. Pre-registering it is what stops the model becoming a rationalisation engine; it
   is drawn on the chart as a dashed line and resolved into a Commit / Hold verdict.
3. **Signals** — each row is one test with an observed outcome and two rates:
   - **If it works, we'd see it** — of the bets like this that succeed, how often does this
     signal show up? Higher is better.
   - **If it fails, we'd see it anyway** — of the bets that fail, how often does it show up
     regardless? Lower is better. This is the vanity check.
4. **Result** — each test's answer becomes the next test's starting belief, so the belief
   walks across the chart one update at a time.

Both rates are asked as "how often would we see this", so there is no polarity flip between
the two fields. Internally the second is the false positive rate `P(E+|¬H)`, i.e.
`1 − specificity`; negative observations are handled symmetrically from `1 − sensitivity`, so
a signal that failed to show up correctly pushes belief *down*.

```text
P(H|E) = (P(H)·P(E|H)) / (P(H)·P(E|H) + (1−P(H))·P(E|¬H))
```

### Reading the weight of evidence

The table shows a likelihood ratio for each observation, labelled **weight of evidence**. It
is the whole lesson in one number:

| Weight | Meaning |
| --- | --- |
| `> 1` | Evidence for the bet — the belief rises |
| `≈ 1` | The signal is uninformative — the belief barely moves |
| `< 1` | Evidence against — the belief falls |

A metric that shows up whether or not the bet is working has a weight near 1. That is the
working definition of a vanity metric, and it is why the page asks for two rates instead of
one confidence number.

### The independence warning

Chaining updates assumes each test is *independent* evidence given the hypothesis. Signals
from the same family usually are not — one keen customer can trip three Demand signals at
once, and the chain would read that as three separate confirmations.

When two or more signals from the same group are in play, the page raises a notice above the
chart. It is the one way this model can quietly manufacture confidence, so the warning is
deliberately hard to miss.

## Signal vocabulary

Signals are picked from a shared list grouped as **Demand**, **Adoption**, **Delivery**, and
**Quality** — a design partner paying for a pilot, users switching from a workaround,
shipping a thin slice to production inside the timebox, support load staying manageable.

The shared list is deliberate. It keeps a room arguing about the same signals in the same
words rather than each person inventing a bespoke metric. A **Custom…** option is there for
signals the list does not cover.

## Presets

Three buttons seed the page:

- **Reset defaults** — a paid design partner plus a shipped timebox slice; ends around `0.88`.
- **Weak signals** — repeat usage and teammate invites where both rates are nearly equal, so
  the belief crawls from `0.20` to `0.23`. Use this one to make the vanity-metric point
  concrete. It also trips the independence warning, since both are Adoption signals.
- **Strong signals** — the same structure with genuinely diagnostic rates.

Displayed probabilities are rounded to two decimals. The inputs are judgement calls, so more
digits would only dress up a guess; values under `0.01` and over `0.99` are labelled rather
than rounded to a misleading `0` or `1`.

## Repository structure

```text
.
├── index.html          # markup, styles, and the full update engine (vanilla JS + Plotly)
├── README.md
├── data/
│   └── scenarios.json  # reference copy of the in-page presets
└── scripts/
```

`index.html` is self-contained; the only external dependency is Plotly.js from a CDN.
`data/scenarios.json` documents the presets in a portable form — the page does not fetch it,
the values live in `index.html`. Keep the two in step if you change a preset.

## GitHub Pages (free hosting)

1. Push this repo to GitHub.
2. Go to **Settings → Pages → Build and deployment**.
3. Set **Source** to **Deploy from a branch**.
4. Set branch to **main** and folder to **/ (root)**.
5. The site URL will be `https://<user>.github.io/<repo>/`.

GitHub Pages only serves static files; the chart runs in the browser with Plotly.js.

## Licence

Open source — add a `LICENSE` file of your choice, for example MIT.
