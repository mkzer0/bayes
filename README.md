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

Three steps, mirrored in the page layout:

1. **Prior `P(H)`** — the outside view. A base rate drawn from a reference class of
   comparable projects, not from how you feel about this one. Default `0.20`.
2. **Signals** — each row is one binary test with an observed outcome and two rates:
   - **Sensitivity `P(E+|H)`** — if the bet is going to work, how often do we see this?
   - **Specificity `P(E−|¬H)`** — if the bet is going to fail, how often do we *not* see it?
3. **Posterior `P(H|E)`** — each test's posterior becomes the next test's prior, so the
   belief walks across the chart one update at a time.

```text
P(H|E) = (P(H)·P(E|H)) / (P(H)·P(E|H) + (1−P(H))·P(E|¬H))
```

Negative observations are handled symmetrically: the page derives the false-positive rate
`1 − specificity` and the false-negative rate `1 − sensitivity`, so an `E−` correctly pushes
belief *down*.

### Reading the likelihood ratio

The steps table shows the likelihood ratio for each observation. It is the whole lesson in
one number:

| LR | Meaning |
| --- | --- |
| `> 1` | Evidence for the hypothesis — the belief rises |
| `≈ 1` | The signal is uninformative — the belief barely moves |
| `< 1` | Evidence against — the belief falls |

A metric that goes up whether or not the bet is working has an LR near 1. That is the
formal definition of a vanity metric, and it is why the page asks for two rates instead of
one confidence number.

## Signal vocabulary

Signals are picked from a shared list grouped as **Demand**, **Adoption**, **Delivery**, and
**Quality** — a design partner paying for a pilot, users switching from a workaround,
shipping a thin slice to production inside the timebox, support load staying manageable.

The shared list is deliberate. It keeps a room arguing about the same signals in the same
words rather than each person inventing a bespoke metric. A **Custom…** option is there for
signals the list does not cover.

## Presets

Three buttons seed the page:

- **Reset defaults** — a paid design partner plus a shipped timebox slice.
- **Weak signals** — repeat usage and teammate invites at sensitivity ≈ specificity, so the
  posterior hardly moves. Use this one to make the vanity-metric point concrete.
- **Strong signals** — the same structure with genuinely diagnostic rates.

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
