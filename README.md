# Bayesian Product Discovery — two-level inference demo

Static, interactive page: prior project success belief, posterior after five problem interviews, delivery capability for a four-week production slice, and overall belief.

## Run locally

```bash
python3 -m http.server 8080
```

Open `http://127.0.0.1:8080/`.

## GitHub Pages (free hosting)

1. Push this repo to GitHub.
2. Go to **Settings → Pages → Build and deployment**.
3. Set **Source** to **Deploy from a branch**.
4. Set branch to **main** and folder to **/ (root)**.
5. The site URL will be `https://<user>.github.io/<repo>/`.

GitHub Pages only serves static files; the chart runs in the browser with Plotly.js.

## What the demo shows

The demo is for teaching Bayesian product discovery in a simple product-investment conversation:

1. Start with an outside-view prior belief about project success.
2. Update that belief after five users independently cite the same problem as important.
3. Discount or strengthen confidence based on delivery capability: can the team ship a thin slice to production within four weeks?

The final number is:

```text
overall belief = P(success after problem evidence) × P(deliver a production slice in 4 weeks)
```

## Repository structure

```text
.
├── index.html
├── README.md
├── data/
└── scripts/
```

The `data/` and `scripts/` folders are present to match the structure of the LLN demo and leave room for future generated examples.

## Licence

Open source — add a `LICENSE` file of your choice, for example MIT.
