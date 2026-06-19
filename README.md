## Layered Beta P&L Attribution for Long–Short Equity Portfolios

Implemented in the attached Jupyter notebook, which includes ample description of steps taken.

### Chapter 1. The Layered Beta Attribution Methodology

#### Methodology Outline

The objective of the methodology is to explain where the cumulative performance of an equity portfolio came from over a chosen window of dates, in terms that map directly onto investment decisions: how much was earned simply by carrying market exposure, how much by tilting toward particular industries or countries, how much by moving those exposures around through time, and how much by picking individual securities. The decomposition is built in layers, each layer measured incrementally to the ones above it, so that the components are additive and sum (together with a residual) to the total portfolio P&L.

The methodology proceeds in three estimation steps followed by a decomposition step.

**Step 1 — market betas.** For every security in the portfolio, a regression of the security's historical returns on the returns of the S&P 500 index (SPX) determines the security's market beta. The regression is run on a trailing window so that the beta of each security is allowed to evolve through time.

**Step 2 — bucketing.** All securities are grouped into sub-portfolios ("buckets"). For US-centric portfolios the buckets represent industries; for international portfolios they represent countries. For each bucket an appropriate, investable stock market index is chosen to represent it — for example a sector SPDR or iShares industry ETF for a US industry, or a single-country MSCI ETF for a country.

**Step 3 — incremental bucket betas.** For every security, a second regression determines the security's incremental beta to its bucket's index. Crucially, this bucket beta is estimated on the security's *incremental* returns after the impact of SPX has been removed — i.e., on the residuals of the Step 1 regression — against the bucket index's own returns orthogonalized to SPX. Because the market component has been stripped from both sides, the SPX beta contribution and the bucket beta contribution are additive rather than double-counting the market.

#### The Five Attribution Components, in Order

Over the attribution window, let *B̄* denote the average portfolio SPX beta and *Ḡ*<sub>*b*</sub> the average incremental beta to bucket *b*. The cumulative performance of the portfolio during the specified range of dates is decomposed into the following components, in this order. Each period's contributions are computed as below and cumulated across the window.

| **#** | **Component** | **Definition (per period t)** | **Question it answers** |
| --- | --- | --- | --- |
| 1 | SPX beta | Performance of SPX times the constant SPX beta, set equal to the portfolio's average SPX beta over the period: C1 = B̄ · r<sub>M,t</sub>. | What did plain market exposure earn? |
| 2 | Industry / geography betas | Performance of a basket of the same industries/geographies relative to SPX, held at constant betas equal to their period averages: C2 = Σ<sub>b</sub> Ḡ<sub>b</sub> · x<sub>b,t</sub>. | What did the standing sector or country tilts earn over the market? |
| 3 | Market timing | Incremental contribution, relative to components 1 and 2, of the changes in the SPX and bucket betas during the period: C3 = (B<sub>t</sub> − B̄) · r<sub>M,t</sub> + Σ<sub>b</sub> (G<sub>b,t</sub> − Ḡ<sub>b</sub>) · x<sub>b,t</sub>. | Did moving exposures up and down add or destroy value? |
| 4 | Individual stocks | Residual contribution of the securities held, with the variable-beta SPX and bucket contributions removed: C4 = r<sub>P,t</sub>(mapped) − C1 − C2 − C3. | Did security selection add value beyond systematic exposures? |
| 5 | Residual | Performance of the portion of the portfolio for which a bucket cannot be determined or historical prices cannot be retrieved to estimate betas, plus any non-stock investments. | How much of the P&L sits outside the equity attribution framework? |

*Table 1. The five additive components of the layered beta decomposition, in the order in which they are computed.*

Two features of the ordering deserve emphasis. First, the constant-beta components (1 and 2) are computed with period-average betas, so they represent the strategic, average posture of the book; component 3 then captures everything attributable to deviations of the actual betas from those averages, i.e., timing of both the market and the buckets. Second, component 4 is defined as a residual after the three systematic layers, which guarantees additivity: components 1 through 4 sum exactly to the return of the mapped (bucketed and priced) portion of the portfolio, and component 5 closes the gap to total portfolio P&L.

#### Illustrative Example

To illustrate the approach we selected three widely held vehicles with distinct profiles: ARKK (ARK Innovation ETF), an actively managed, high-beta US thematic ETF for which industry buckets are appropriate; JLGMX (JPMorgan Trust II: JPMorgan Large Cap Growth Fund), a diversified US large-cap growth mutual fund, also bucketed by industry; and VWIGX (Vanguard International Growth), an international equity fund bucketed by country. Table 2 summarizes the configuration; the bucket indexes are liquid US-listed ETFs.

| **Fund** | **Vehicle** | **Bucket scheme** | **Buckets (index ETF)** | **Avg. SPX beta** |
| --- | --- | --- | --- | --- |
| ARKK | Active ETF | Industry | Software & Cloud (XLK), Biotechnology & Genomics (XBI), Semiconductors (SOXX), Internet & E-commerce (ARKW), Fintech & Digital Assets (FINX), Autonomous & Robotics (ROBO), Gaming & Entertainment (NERD) | 1.40 |
| JLGMX | Mutual fund | Industry | Technology (XLK), Healthcare (XLV), Consumer Discretionary (XLY), Financials (XLF), Industrials (XLI), Energy (XLE), Consumer Staples (XLP) | 1.03 |
| VWIGX | Mutual fund | Country / region | Japan (EWJ), China/Hong Kong (MCHI), Europe (VGK), Emerging Markets (VWO), United States (SPY), Developed Asia Pacific (VEA) | 0.55 |

*Table 2. Configuration of the three illustrative funds.*

Figures 1–3 show the resulting stacked area charts of the cumulative P&L decomposition over January 2019 – December 2024. Positive cumulative contributions stack above the zero line and negative ones below it; the black line is total cumulative P&L, the sum of the five bands.

![Figure 1. ARKK - cumulative P&L decomposition, 2019-2026](arkk.png)

Figure 1. ARKK - cumulative P&L decomposition, 2019-2026

The ARKK panel shows the methodology doing exactly what it is designed to do. The dominant blue band is the constant average SPX beta of 1.4 compounding with the market. The green timing band turns visibly negative, reflecting beta that drifted highest into the drawdown, and the purple selection band, strongly positive through the 2020 run-up, reverses sign afterwards. The point of the chart is that a single line of fund NAV would conflate all four stories.

![Figure 2. JLGMX - cumulative P&L decomposition, 2019-2026](jlgmx.png)

Figure 2. JLGMX - cumulative P&L decomposition, 2019-2026

For the diversified JPMorgan Large Cap Growth fund profile, the decomposition is dominated by the market component with a beta near one, a steady positive industry band driven by standing technology and communication-services tilts, and a steady stock selection band — the signature of a large, benchmark-aware fund whose active risk is modest relative to its market exposure.

![Figure 3. VWIGX - cumulative P&L decomposition, 2019-2026](<img width="1000" height="540" alt="VWIGX" src="https://github.com/user-attachments/assets/f35616d9-3932-4d0a-90a8-abb6a7753e8d" />
<img width="1000" height="540" alt="JLGMX" src="https://github.com/user-attachments/assets/b5d85b26-f7a2-4675-b118-a18a79bff2f6" />
<img width="1000" height="540" alt="ARKK" src="https://github.com/user-attachments/assets/5bd8bf90-80b3-465a-8255-c8c57d5173a4" />
vwigx.png)

Figure 3. VWIGX - cumulative P&L decomposition, 2019-2026

---

### Chapter 2. Literature Review and Potential Future Analysis

#### Literature Review

Our layered beta methodology sits at the intersection of three strands of the performance-evaluation literature: returns-based decompositions that separate market exposure, timing and selectivity; holdings-based attribution in the Brinson tradition; and factor-model attribution as practiced with commercial risk models. Here, we review each strand briefly and note the alternative methodologies they suggest.

**Returns-based decompositions: selectivity and timing.** The earliest formal decomposition is Fama (1972), which splits a portfolio's excess return into "selectivity" — return beyond what the portfolio's beta would warrant under the CAPM — and "timing," the part earned by varying systematic risk. Our components 1, 3 and 4 are a direct, multi-layer descendant of this split. The market-timing literature refined the idea: Treynor and Mazuy (1966) detect timing as convexity in the fund-versus-market regression by adding a squared market term, while Henriksson and Merton (1981) model the timer as holding an option-like exposure that switches with the sign of the market. Both deliver a regression-based alternative to our component 3: rather than measuring realized covariance between beta changes and index returns from holdings, they infer timing ability from the shape of the return relationship alone. The advantage is that only fund returns are needed; the disadvantage, documented at length since, is low power and bias when betas change for non-timing reasons (flows, drift in holdings, derivatives).

Sharpe (1992) introduced returns-based style analysis: constrained regression of fund returns on a palette of asset-class indexes, interpreting the fitted weights as an effective style mix and the residual as selection. This is the natural returns-only competitor to our Steps 2–3. It requires no holdings, but its rolling-window weights confound timing with estimation lag, exactly the problem our holdings-based bucket betas avoid.

**Holdings-based attribution: the Brinson tradition.** Brinson and Fachler (1985) and Brinson, Hood and Beebower (1986) decompose benchmark-relative performance, segment by segment, into allocation, selection and interaction effects using portfolio and benchmark weights and segment returns. This remains the dominant practitioner framework, codified in textbooks such as Bacon (2008). Relative to Chapter 1, the Brinson approach differs in two ways. First, it is weight-based rather than beta-based: a segment's exposure is its capital weight, not its regression beta, so a portfolio of low-beta utilities and one of leveraged tech carry the same "exposure" to their sectors if their weights match. For a long–short, variable-leverage book this is a serious shortcoming and the principal reason our methodology is built on betas. Second, Brinson attribution is inherently benchmark-relative, whereas the layered beta decomposition explains total P&L, which suits absolute-return mandates. Extensions of the Brinson framework relevant to our international variant include Ankrim and Hensel (1994) and Karnosky and Singer (1994), which separate currency contribution and the interest-rate differential embedded in currency forwards — a dimension Chapter 1 currently folds into the country buckets.

Within the holdings-based strand, Grinblatt and Titman (1993) propose benchmark-free performance measurement using the covariance between weight changes and subsequent returns — conceptually the closest academic relative of our component 3, since both measure whether exposure shifts were rewarded. Daniel, Grinblatt, Titman and Wermers (1997, "DGTW") match each holding to a characteristic benchmark portfolio (size, book-to-market, momentum quintiles) and decompose fund performance into characteristic selectivity, characteristic timing, and average style; Wermers (2000) applies the framework to the mutual-fund universe at scale. DGTW is a compelling alternative to our bucket layer: instead of one industry or country index per security, each security is benchmarked against peers with identical characteristics, which sharpens the selection signal but requires a full characteristics database and quarterly holdings for the entire market.

**Factor-model attribution.** The third strand replaces buckets with estimated factor structures. Rosenberg (1974) and Rosenberg and McKibben (1973) initiated the "extra-market covariance" models that became Barra: security returns are explained cross-sectionally by industry memberships and style characteristics, yielding daily factor returns and security-level exposures. Menchero, Orr and Wang (2011) document the modern US equity version (USE4). Academic counterparts include the Fama and French (1993) three-factor model and Carhart (1997) four-factor model, in which time-series regressions on factor-mimicking portfolio returns yield exposures and alpha. Grinold and Kahn (2000) provide the canonical treatment of attribution and risk decomposition in this framework. Factor-based attribution is examined further in Chapter 3 as a candidate replacement for Steps 2–3.

#### References

- Ankrim, E. M., and C. R. Hensel (1994). "Multicurrency Performance Attribution." Financial Analysts Journal 50(2), 29–35.

- Bacon, C. R. (2008). Practical Portfolio Performance Measurement and Attribution, 2nd ed. Chichester: Wiley.

- Brinson, G. P., and N. Fachler (1985). "Measuring Non-US Equity Portfolio Performance." Journal of Portfolio Management 11(3), 73–76.

- Brinson, G. P., L. R. Hood, and G. L. Beebower (1986). "Determinants of Portfolio Performance." Financial Analysts Journal 42(4), 39–44.

- Cariño, D. R. (1999). "Combining Attribution Effects Over Time." Journal of Performance Measurement 3(4), 5–14.

- Carhart, M. M. (1997). "On Persistence in Mutual Fund Performance." Journal of Finance 52(1), 57–82.

- Daniel, K., M. Grinblatt, S. Titman, and R. Wermers (1997). "Measuring Mutual Fund Performance with Characteristic-Based Benchmarks." Journal of Finance 52(3), 1035–1058.

- Fama, E. F. (1972). "Components of Investment Performance." Journal of Finance 27(3), 551–567.

- Fama, E. F., and K. R. French (1993). "Common Risk Factors in the Returns on Stocks and Bonds." Journal of Financial Economics 33(1), 3–56.

- Grinblatt, M., and S. Titman (1993). "Performance Measurement without Benchmarks: An Examination of Mutual Fund Returns." Journal of Business 66(1), 47–68.

- Grinold, R. C., and R. N. Kahn (2000). Active Portfolio Management, 2nd ed. New York: McGraw-Hill.

- Henriksson, R. D., and R. C. Merton (1981). "On Market Timing and Investment Performance. II. Statistical Procedures for Evaluating Forecasting Skills." Journal of Business 54(4), 513–533.

- Karnosky, D. S., and B. D. Singer (1994). Global Asset Management and Performance Attribution. Charlottesville, VA: Research Foundation of the Institute of Chartered Financial Analysts.

- Lo, A. W. (2008). "Where Do Alphas Come From? A Measure of the Value of Active Investment Management." Journal of Investment Management 6(2), 1–29.

- Menchero, J. (2000). "An Optimized Approach to Linking Attribution Effects Over Time." Journal of Performance Measurement 5(1), 36–42.

- Menchero, J., D. J. Orr, and J. Wang (2011). "The Barra US Equity Model (USE4): Methodology Notes." MSCI Research.

- Rosenberg, B. (1974). "Extra-Market Components of Covariance in Security Returns." Journal of Financial and Quantitative Analysis 9(2), 263–274.

- Rosenberg, B., and W. McKibben (1973). "The Prediction of Systematic and Specific Risk in Common Stocks." Journal of Financial and Quantitative Analysis 8(2), 317–333.

- Sharpe, W. F. (1992). "Asset Allocation: Management Style and Performance Measurement." Journal of Portfolio Management 18(2), 7–19.

- Treynor, J. L., and K. K. Mazuy (1966). "Can Mutual Funds Outguess the Market?" Harvard Business Review 44(4), 131–136.

- Vasicek, O. A. (1973). "A Note on Using Cross-Sectional Information in Bayesian Estimation of Security Betas." Journal of Finance 28(5), 1233–1239.

- Wermers, R. (2000). "Mutual Fund Performance: An Empirical Decomposition into Stock-Picking Talent, Style, Transactions Costs, and Expenses." Journal of Finance 55(4), 1655–1695.

#### Potential future analysis: replacing bucket betas with a standard factor model

**What we consider updating**

Steps 2 and 3 of our methodology outlined here — hand-chosen buckets, one representative index each, and two-pass time-series regressions for incremental betas — would be replaced by exposures and factor returns from a cross-sectional risk model of the Barra type (e.g., MSCI Barra USE4/GEMLT, Axioma, or an in-house clone built on the Rosenberg (1974) design). In such a model, each security's exposures to industries, countries, currencies and styles (value, size, momentum, volatility, quality, liquidity, growth, leverage) are computed from current characteristics, and daily factor returns are obtained from cross-sectional regressions of the security universe on those exposures. The portfolio's P&L then decomposes into: market; style factor contributions; industry/country factor contributions; timing of each exposure block; and a specific return that takes the place of component 4.

**Why this is potentially an improvement**

- **Cleaner exposures.** Trailing time-series betas need years of data, lag reality by construction — a security that pivots its business or doubles its leverage carries its old beta for many months — and are noisy for short histories, IPOs and illiquid names. Characteristic-based exposures update as the characteristics do, and the cross-section of thousands of securities each day estimates factor returns far more precisely than 24 monthly observations estimate a single security's beta.

- **No double counting across correlated buckets.** Our base methodology orthogonalizes each bucket to SPX but not to the other buckets. A software company assigned to the software bucket still co-moves with the internet bucket; with single-bucket assignment that common movement leaks into selection. The multivariate cross-sectional regression attributes each security's return jointly to all factors at once, so correlated industries, countries and styles are disentangled rather than fought over.

- **Styles become visible.** A large share of what the current methodology books as "individual stocks" is in fact compensated style exposure — momentum, value, size, low volatility. Carhart (1997) and the DGTW evidence show these effects are first-order for fund performance. Pulling them out of the selection residual makes component 4 a far sharper measure of idiosyncratic skill and aligns the attribution with the risk model already in use for ex-ante risk, so that ex-ante factor risk and ex-post factor P&L can be reconciled line by line.

- **Currency separation for international books.** Country factors in global models are defined in local terms with explicit currency factors, implementing the Ankrim–Hensel/Karnosky–Singer separation automatically instead of leaving currency inside the country buckets.

**Recommended hybrid**

The costs are real: licensing and data-management expense; reduced transparency to portfolio managers, for whom "your incremental beta to XBI" is more tangible than a normalized style exposure; factor returns that are estimation constructs rather than investable indexes, complicating hedging conversations; and model risk — the attribution inherits every specification choice of the vendor. We therefore recommend a hybrid rather than wholesale replacement: keep component 1 as the SPX layer for continuity; map the model's industry/country factors onto the existing bucket labels so the report's vocabulary survives; add a single new "style factors" line between buckets and selection.
