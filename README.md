# Code2Evidence

**Cognitive Spatial Reasoning in Physical World**

Code2Evidence studies the interface between visual parsing and cognitive
physical reasoning. Instead of asking a vision-language model (VLM) to recover
physical meaning from long object-state JSON, study-specific programs convert
saved parsing into explicit events, routes, counterfactual rollouts or
interaction traces. The frozen VLM receives the original visual input, the
question and this readable evidence.

The repository contains the public project website. It is plain HTML, CSS and
JavaScript, so no build step, backend or GPU is needed to inspect the saved
results.

## Main result

| Input condition | Event semantics | Route search | Counterfactual rollout | Interaction tracing | Average |
| --- | ---: | ---: | ---: | ---: | ---: |
| Video only | 50.27 | 11.26 | 7.81 | 0.31 | 17.41 |
| VLM-generated code | 51.50 | 22.30 | 0.81 | 2.09 | 19.18 |
| Parse Anything code | 52.47 | 20.13 | 6.30 | 0.33 | 19.81 |
| **Code2Evidence** | **72.14** | **88.55** | **84.01** | **25.39** | **67.52** |

Values are CogGym response-alignment R² in percent. Counterfactual rollout
averages the Gerstenberg 2021 and 2022 studies; the final column averages the
four task categories. Human responses are used only for evaluation.

## What the site shows

- **Event semantics (Bass 2022):** bind named objects and requested worlds,
  then expose event occurrence, order and finite-horizon absence.
- **Route search (Wu 2022):** align grid states to action steps and calculate
  feasible routes under the observed door schedule.
- **Counterfactual rollout (Gerstenberg 2021/2022):** intervene on an object,
  follow supported contacts and preserve unsupported outcomes as unknown.
- **Interaction tracing (Beller 2020):** connect measured encounters and
  motion changes into candidate causal paths.
- **Parsing support:** report localization by study, mIoU/RPE definitions and
  runtime separately from downstream reasoning.

Each study panel pairs the original stimulus, an archived parsing
visualization, a four-stage method animation, the evidence given to the VLM,
the recorded controls and the GT/reference coverage. The public page presents
the five-study paper evaluation; retained audit records also preserve earlier
diagnostic experiments and provenance.

## Local preview

From the repository root:

```bash
python3 -m http.server 8797 --bind 127.0.0.1
```

Open [http://127.0.0.1:8797/](http://127.0.0.1:8797/). HTTP is required because
the page loads local JSON records.

## Validation

```bash
node --test scripts/test_research_site.mjs
```

To regenerate the deterministic method animations:

```bash
npm ci
npm run render:methods -- --qa-dir=/tmp/code2evidence-method-qa
npm test
```

The animations are evidence-flow schematics, not source-video replays or new
model outputs. Their hashes, dimensions and source bindings are recorded in
`data/method-animations.json`.

## Repository map

- `index.html`: public paper website and static result tables.
- `assets/research.css`: responsive visual system.
- `assets/research.js`: five-study interaction and data rendering.
- `data/research-20261105.json`: audited metrics, controls and provenance.
- `data/post-parsing-methods.json`: study-specific evidence interfaces.
- `data/study-media.json`: source-bound stimulus and parsing media.
- `data/method-animations.json`: method animation manifest.
- `scripts/test_research_site.mjs`: integrity and publication checks.

GitHub Actions validates the records, assembles the static artifact and
publishes GitHub Pages on every push to `main`.
