# Numerical analysis files

Numerical analysis files accompanying the undergraduate thesis *"Cosmological
Constraints and Dark Energy Reconstruction with Bayesian Evidence: ΛCDM, wCDM,
CPL and the Sign-Switching Λ_sCDM"* (Department of Physics, University of
Ioannina; supervisor: Prof. L. Perivolaropoulos).

The analysis is organized in four annotated Jupyter notebooks, which together
reproduce every quantitative result and figure of the thesis:

| File | Thesis part |
|---|---|
| `CosmologicalConstraintsFinal.ipynb` | Main fits and model comparison (Chapter 5) |
| `IsolationExperiment.ipynb` | Dataset grid over BAO type / SNIa sample / CC (Chapter 6) |
| `AkarsuReproduction.ipynb` | Reproduction of the published Λ_sCDM constraints (Section 6.1) |
| `Reconstruction.ipynb` | Quintessence / phantom reconstruction, V(ϕ) (Chapter 7) |

(File names in this table should match the notebooks in this repository.)
All notebooks share the same cosmology and likelihood definitions; the main
notebook is the reference implementation and is documented in detail below.

## Requirements

```
pip install -U nautilus-sampler getdist numpy scipy matplotlib pandas
```

Runs on a standard laptop or Google Colab. A complete run of all fits takes
several hours (each Nautilus fit uses n_live = 1500; evidences are re-computed
over 5 random seeds).

## Data

- **Pantheon+** and **DES-Dovekie** supernova catalogues and covariances are
  downloaded automatically by the notebook from the official
  PantheonPlusSH0ES and des-science GitHub releases.
- **DESI DR2 BAO** measurements and correlations are hard-coded from the
  official data release (DESI Collaboration 2025).
- **Compressed CMB** (R, l_A, ω_b) follows Zhai & Wang (2019).
- **Cosmic chronometers**: `HzTable_MM_BC03.dat` (Moresco compilation,
  included here) with the covariance model of Moresco et al. (2020).

## Inputs per notebook

- **CosmologicalConstraintsFinal.ipynb** — Pantheon+ and DES-Dovekie catalogues
  + covariances (auto-downloaded from the official releases); DESI DR2 BAO and
  compressed CMB (hard-coded from the publications); `HzTable_MM_BC03.dat` +
  Moresco et al. (2020) covariance (included here).
- **IsolationExperiment.ipynb** — in addition to the above: the SDSS-era 3D BAO
  compilation and the transversal (2D) BAO compilation of Nunes et al.
  (hard-coded from the corresponding papers), the original Pantheon (2018)
  distance moduli, and the Moresco (2022) 33-point chronometer table with
  diagonal errors.
- **AkarsuReproduction.ipynb** — the exact dataset combinations of Akarsu et al.
  (2023): SDSS-era BAO / transversal BAO / Lyα, Pantheon (2018), and the SH0ES
  M_B prior.
- **Reconstruction.ipynb** — no external data: it consumes the posterior chains
  / best-fit parameters produced by the main notebook.

(If any of these inputs is stored as a separate file in this repository rather
than hard-coded, its filename should appear next to the corresponding entry.)

## Main notebook map (`CosmologicalConstraintsFinal.ipynb`)

| Notebook section | Thesis result |
|---|---|
| §1 Constants, cosmology functions, r_d (Aizpuru GA fit) | Eq. (4.22), §4.4.2 |
| §2 Data loading & likelihoods (CMB, BAO, CC, SNIa) | Chapter 3, §4.4 |
| §3 ΛCDM fits (Pantheon+, DES, anchored, partial combos) | Tables 5.1–5.2, Figs. 5.1–5.3 |
| §4 wCDM fits | Table 5.3, Fig. 5.4 |
| §5 CPL fits | Table 5.4, Figs. 5.5–5.6 |
| §6 Λ_sCDM fits, profile likelihood, BAO-compilation grid | Tables 5.5, 6.2–6.3, Figs. 5.7–5.9, 6.1–6.3 |
| H₀ whisker plot | Table 5.7, Fig. 5.10 |
| Bayesian model comparison (multi-seed evidence, AIC/BIC) | Table 5.6 |
| A.1 Stability w.r.t. the transition rapidity ν | §4.5 robustness check |
| A.2 Profile likelihood without DESI Lyα | §6 systematics |
| A.3 SH0ES calibrator cross-check | §4.4.6 |

The evidence cell prints the raw ln Z per seed, so the ±0.01 uncertainties of
Table 5.6 can be verified directly. The H₀ whisker cell contains the final
(thesis Table 5.7) values as a built-in consistency guard: it warns if live
results deviate from the published numbers.

**IsolationExperiment.ipynb** produces Tables 6.1–6.3 and the z† posterior
comparisons across BAO compilations (SDSS-era 3D, DESI DR2, transversal),
including the widened-prior test. **AkarsuReproduction.ipynb** reproduces the
published constraints of Akarsu et al. with the original datasets and both
sign conventions (Δln Z′ of Section 6.1). **Reconstruction.ipynb** produces
the w(z), ϕ(z) and V(ϕ) reconstructions of Chapter 7 (Figures 7.1–7.4).

## Citation

If you use these files, please cite the thesis and the Λ_sCDM literature
referenced therein (Akarsu et al. 2021, 2023).
