# Do Macroeconomic and Geopolitical Risk Factors Improve FX Volatility Forecasts? A Mixed-Frequency Transformer Approach

**Qimu Luo**, Ruixi Xu, Zihan Liang, Zhuoqi Hou, Ren Zhao

Presented at the **International Business and Economy Conference (IBEC)**, 4 January 2026.

📄 [Presentation slides](IBEC_presentation.pdf) · 📝 [Paper on SSRN](http://dx.doi.org/10.2139/ssrn.5418894)

## Research question

> Do macroeconomic fundamentals and geopolitical risk (GPR) indicators improve USD/CNH volatility forecasts beyond what technical indicators already provide?

| Hypothesis | Channel |
|---|---|
| **H1** | Adding macro and GPR information improves forecasts relative to a technical-only model |
| **H1a** — Expectation anchoring | Macro fundamentals help at medium to long horizons |
| **H1b** — Shock amplification | GPR indicators help at short horizons by capturing uncertainty shocks and volatility spikes |
| **H1c** — Dynamic modulation | Combining macro and GPR information gives complementary gains across volatility regimes |

## Data and target

- **Market:** daily USD/CNH OHLC data, August 2015 – December 2024.
- **Technical indicators:** SMA(20), EMA(50), RSI(14), ATR(14), daily log return, and 10-day rolling mean and standard deviation of returns.
- **Macro fundamentals** (classic PPP / interest-rate-parity / balance-of-payments channels): CPI, PPI, M2, interest rates, FX reserves, employment, gold, oil, equity indices, EPU. These come at daily and monthly frequencies.
- **Geopolitical risk:** Caldara–Iacoviello GPR (Acts, Threats, country-level), daily and monthly.
- **Target:** Parkinson range-based variance $\sigma^2_t = \frac{1}{4\ln 2}\left(\ln\frac{H_t}{L_t}\right)^2$, smoothed over a 10-day rolling window, then averaged over the next $h$ days for each horizon $h \in \{1, 5, 10\}$.

## Model: Mixed-Frequency Transformer (MFT)

1. **Feature selection.** Separate streams for technical, macro and GPR features.
2. **Encoding and alignment.** One encoder per stream. Monthly series are forward-filled onto the daily grid, and time/position encodings are added.
3. **Cross-attention fusion.** The technical stream provides the queries; the macro and GPR streams provide the keys and values. Each fusion layer adds a residual connection and layer normalisation.
4. **Transformer stack.** Multi-head self-attention encoder layers.
5. **Multi-horizon heads.** 1-, 5- and 10-day volatility forecasts, trained with a joint multi-horizon MSE loss.

Four variants are compared: **Transformer baseline** (technical only), **Macro-augmented MFT**, **GPR-augmented MFT** and **Joint Macro–GPR MFT**. They are evaluated with $R^2$, RMSE, MAE, MAPE, directional accuracy (DA) and spike-hit rate, on the full sample, in calm and stress regimes, and in an event study.

## Key findings

- **Full sample.** The technical-only baseline has the best point accuracy (pooled $R^2$ = 68.8%). The Joint Macro–GPR MFT has the highest directional accuracy (62.9%) and spike-hit rate (66.7% pooled; 89.3% at $h = 1$).
- **Stress regime.** The Macro-augmented MFT has the lowest RMSE at all horizons. The GPR-augmented MFT improves spike detection at short and medium horizons.
- **Calm regime.** Technical signals are enough for point forecasts. The joint model still detects most spikes.
- **Case study: 2024 U.S. presidential election (5–22 Nov).** The GPR-augmented MFT fits the volatility spike best ($R^2$ = 61.2%). The joint model reaches the highest DA (76.9%). Integrated-Gradients attributions show the drivers shifting over time: GPR Acts/Threats at the announcement, then CPI, oil and money supply, then technical trend indicators.
- **Hypotheses.** H1 supported (gains mainly on risk-sensitive metrics rather than point error), H1a partially supported, H1b supported.

## Related

This paper grew out of the SURF 2025 project *FX Risk Sentinel: AI and Econometrics for Predictive Risk Analysis*. The poster and the FX risk-signal platform code are in [FX-Risk-Signal-Detection](https://github.com/QimuLuo-hub/FX-Risk-Signal-Detection).

## Citation

```bibtex
@inproceedings{luo2026fxvol,
  title     = {Do Macroeconomic and Geopolitical Risk Factors Improve FX Volatility Forecasts? A Mixed-Frequency Transformer Approach},
  author    = {Luo, Qimu and Xu, Ruixi and Liang, Zihan and Hou, Zhuoqi and Zhao, Ren},
  booktitle = {International Business and Economy Conference (IBEC)},
  year      = {2026},
  doi       = {10.2139/ssrn.5418894}
}
```
