# AGI: Agentic Intelligence and Latent Lookahead for Robust Manipulation

Project page for the ICRA 2027 submission.

A closed-loop manipulation policy: a frozen vision-language agent proposes programs in a
**nine-primitive robot DSL**, a **Code-to-Token Bridge** conditions a JEPA-style latent dynamics
model, and a **learned verifier** ranks imagined trajectories before the arm moves. On LIBERO-PRO
Spatial the complete loop reaches 0.50 Position and 0.49 Task success, where OpenVLA and pi-0 score
0.00. Across Meta-World difficulty splits it averages 94.0%. On a physical AgileX Piper 6-DoF arm,
AGI achieves 83.8% (67/80) overall success across single-object and sequential two-object placement tasks.

## Anonymity

The paper is currently submitted as "Anonymous Authors", so this page is **author-blind on purpose**:
no names, no affiliations, no `citation_author` meta tags, no personal or repository links. Every
place that needs updating after de-anonymization is marked with a `TODO` comment in `index.html`:

1. Replace "Anonymous Authors" in the hero with the author block.
2. Add `citation_author` and `citation_pdf_url` meta tags.
3. Check the canonical and Open Graph URLs.
4. Add arXiv, PDF and code buttons to the hero.
5. Replace the placeholder BibTeX entry.

## Local preview

```bash
python3 -m http.server 8000
# open http://localhost:8000
```

## Structure

```
index.html              # the complete project page
rollouts.html           # LIBERO-X rollout browser (generated, see below)
rollouts/               # one sprite per rollout, lazy-loaded by rollouts.html
static/css/index.css    # styles
static/js/index.js      # nav, sortable and heat tables, decision-rule switch, lightbox, BibTeX copy
static/images/          # publication figures from the paper
static/videos/          # real-robot MP4 demonstration videos (H.264) and posters
```

`rollouts.html` and `rollouts/` are generated, never hand-edited:

```bash
cd ../AGI
python tools/make_liberox_frames_figure.py --web ../AgenticIntelligence
```

## Where the numbers come from

Every figure and number on the main page is taken from the paper source (`root.tex`):

| Page section | Paper source |
|---|---|
| Teaser | `Figs/teaser.pdf` (`fig:teaser`) |
| Real robot | Section VII, `Figs/real_robot.jpg` (`fig:real-robot`), 4 video demos |
| LIBERO-PRO | `Figs/fig_libero_pro_heatmap.png` (`fig:libero-pro-heatmap`), Section VI-A |
| Meta-World + LIBERO-X | Table `tab:cross-benchmark`, `Figs/fig_robustness.png` |
| Decision-rule switch, selector figure | `Figs/fig_selector.png` (`fig:selector-summary`), Section VI-B |
| Component ablations | Table `tab:ablation_interface` |
| Planning cost | Table `tab:realtime` |
| Auditability | `Figs/interpretability_vs_blackbox.png` (`fig:interpretability`) |
| Meta-World / LIBERO-X rollouts | `Figs/MetaWorld_four_difficulty_successful_frames.jpg`, `Figs/LIBEROX_four_verb_successful_frames.jpg` |

Machine-readable copies of the tables live in `../AGI/results/paper/*.json`. If the paper
tables change, update the tables and the `rules` JSON block in `index.html` to match.

The two rollout figures are rebuilt from the evaluation logs, not hand-assembled:

```bash
cd ../AGI
python tools/make_metaworld_frames_figure.py     # --list / --score / --tasks to swap a row
python tools/make_liberox_frames_figure.py       # --list / --tasks to swap a row
cp "ICRA'27_Agentic Intelligence/Figs/MetaWorld_four_difficulty_successful_frames.jpg" \
   ../AgenticIntelligence/static/images/metaworld_frames.jpg
cp "ICRA'27_Agentic Intelligence/Figs/LIBEROX_four_verb_successful_frames.jpg" \
   ../AgenticIntelligence/static/images/liberox_frames.jpg
```

Both draw only on episodes the success checker accepted. Note that they come from runs on our own
hardware: the Meta-World panel above is a cross-paper comparison with a different protocol, and the
LIBERO-X sweep behind those frames uses a different checkpoint and task set from the severity table.
The captions say so.
