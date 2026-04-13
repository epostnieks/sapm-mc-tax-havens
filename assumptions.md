# Monte Carlo Assumptions — Tax Havens / Sovereignty Arbitrage

All values in $B USD (annualized). Every parameter traces to paper §4–§5
or the citations in `data_sources.md`. Run `python mc_simulation.py` to
reproduce bit-identical results.

---

## Simulation Parameters

| Parameter | Value | Justification |
|-----------|-------|---------------|
| Seed | 42 | Fixed for reproducibility |
| N draws | 100,000 | 4-decimal CI stability |
| Cross-channel correlation ρ | 0.3 | Shared macro drivers (GDP, population, regulation) |
| Private payoff Π | $492.0B/yr | Annual industry revenue — see `data_sources.md` |
| β_W median (result) | 6.27 | Confirmed by N=100,000 draws |
| ΔW median (result) | $3,084.7B/yr | Sum of channel medians (correlated) |

**Π = revenue, not profit.** SAPM Iron Law: βW = ΔW/Π where Π is annual
revenue. Using profit would inflate βW by 5–20× for low-margin industries.

---

## Channel Parameters

| Channel | Dist | Low | Mid | High | Description |
|---------|------|-----|-----|------|-------------|
| `C1_fiscal_drain` | normal | $400B | $492B | $600B | Direct tax revenue lost to profit shifting |
| `C2_public_services` | lognormal | $450B | $640B | $850B | Public services degradation from lost revenue |
| `C3_inequality` | lognormal | $350B | $520B | $750B | Inequality amplification, wealth concentration |
| `C4_financial_stability` | lognormal | $300B | $480B | $700B | Financial system opacity, crisis transmission |
| `C5_governance` | lognormal | $250B | $420B | $600B | Sovereignty erosion, regulatory race to bottom |
| `C6_developing_countries` | lognormal | $280B | $500B | $750B | Disproportionate impact on Global South fiscal capacity |


All ranges represent [P5, P95] of the channel-specific distribution as
calibrated from literature in paper §4.

---

## Impossibility Floor

The floor β_W ≥ 2.5 is the minimum ratio achievable while the industry operates.
This bounds the simulation from below: the theorem holds regardless of parameter values,
because even best-case scenarios exceed the floor.

In 100,000 draws: P(β_W < 2.5) = 0.0000%

## Sensitivity (VSL × Double-Counting Grid)

Central VSL (1.0×): no DC adj β_W = 6.2 | 20% DC adj = 4.96 | 40% DC adj = 3.72

See `mc_results.json` → `sensitivity_matrix` for full 5×5 grid.

## Distribution Robustness

The simulation uses a lognormal/normal mix calibrated to channel-specific
uncertainty structure. Results are robust: the central β_W changes by less
than ±0.3 under all-lognormal or all-normal configurations.

---

## Plausibility Checks (SAPM IL#28)

- **Annual flow**: All W_i are $/yr flows ✓
- **GDP bound**: ΔW = $3,085B = 2.9% of world GDP ($106T) ✓
- **β_W range**: 6.27 is within the [0.5, 100] plausible range ✓
- **P(β_W < 1)**: 0.0000% ✓
