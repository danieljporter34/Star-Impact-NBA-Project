# NBA Star Impact Analysis: Do Stars Actually Drive Winning?

## Overview

The modern NBA is often framed as a "star-driven league" — but is that actually true? This project moves beyond anecdotal claims by developing an objective, data-driven definition of an NBA "star" and testing whether star players significantly impact team success.

Using data from the 2025-26 NBA season, I built a multi-stage filtering process to identify stars based on playing time, efficiency, and scoring. I then compared teams with 0, 1, and 2+ stars across three key performance metrics: win percentage, offensive rating, and defensive rating.

**The core finding:** Stars do impact winning — but only offensively, and only when a team has multiple stars. A single star alone does not produce a statistically significant advantage. Defensively, star power shows no measurable benefit.

---

## Research Question

**Do star players significantly impact winning in the NBA?**

More specifically:
- Do teams with more stars have higher win percentages?
- Do stars improve offensive rating (points per 100 possessions)?
- Do stars improve defensive rating (points allowed per 100 possessions)?

---

## Data Source

Data was merged from two sources:
- **Basketball Reference** (via BRScraper) – Traditional stats, advanced metrics (PER, BPM, VORP), team standings
- **NBA API** – Advanced team stats including offensive and defensive ratings

**Season:** 2025-26 NBA season
**Teams:** All 30 NBA teams

---

## Methodology

### Defining a "Star" (Multi-Stage Filtering)

Rather than using arbitrary All-Star selections (limited to 24 players and influenced by popularity), I developed an objective, data-driven definition:

| Filter | Threshold | Rationale |
|--------|-----------|-----------|
| **Filter 1: Minutes per game** | ≥ 27.1 MPG (75th percentile) | Stars play meaningful roles |
| **Filter 2: Player Efficiency Rating (PER)** | ≥ 75th percentile of filtered group | Stars are efficient producers |
| **Filter 3: Points per game** | ≥ 25th percentile of filtered group | Stars have offensive impact |

**Result:** 28 stars identified across the league

### Team Grouping

Teams were grouped into three categories based on star count:
- **0 stars** (n = 11 teams)
- **1 star** (n = 13 teams)
- **2+ stars** (n = 6 teams) — only one team had 3 stars

### Statistical Tests Used

| Test | Purpose |
|------|---------|
| **ANOVA** | Compare means across three groups (parametric) |
| **Kruskal-Wallis** | Compare distributions across three groups (non-parametric, robust to outliers) |
| **Tukey HSD** | Post-hoc pairwise comparisons to identify which specific groups differ |

---

## Key Findings

### Summary Table

| Metric | 0 Stars (n=11) | 1 Star (n=13) | 2+ Stars (n=6) | ANOVA p | Significant Pairwise |
|--------|----------------|----------------|----------------|---------|----------------------|
| Win Percentage | 0.396 | 0.526 | 0.634 | **0.011** | 0 vs. 2+ |
| Offensive Rating | 112.79 | 115.05 | 118.05 | **0.004** | 0 vs. 2+ |
| Defensive Rating | 116.31 | 113.82 | 113.97 | 0.181 | None |

> *Note: Lower defensive rating is better (fewer points allowed per 100 possessions).*

---

### Finding 1: Win Percentage

Teams with **2+ stars** had the highest average win percentage (0.634), followed by 1-star teams (0.526), then 0-star teams (0.396).

- ANOVA (p = 0.011) and Kruskal-Wallis (p = 0.009) rejected the null hypothesis
- **Tukey HSD revealed that only the difference between 0 stars and 2+ stars was significant (p = 0.01)**
- The difference between 0 and 1 star approached but did not reach significance (p = 0.098)

**Interpretation:** Having a single star is not enough to guarantee a statistically significant advantage. The leap from zero stars to two or more stars is what matters.

---

### Finding 2: Offensive Rating

Teams with **2+ stars** averaged 118.05 Off_Rating, compared to 115.05 (1 star) and 112.79 (0 stars).

- ANOVA (p = 0.004) and Kruskal-Wallis (p = 0.011) showed significant differences
- Only 0 vs. 2+ stars was significant in Tukey HSD (p = 0.003)
- 0 vs. 1 star (p = 0.138) and 1 vs. 2+ stars (p = 0.093) were not significant

**Interpretation:** Stars drive offensive efficiency, but again, the effect is threshold-driven. Multiple stars produce a clear offensive advantage; a single star does not.

---

### Finding 3: Defensive Rating

**No statistically significant relationship** was found between star count and defensive rating.

- ANOVA (p = 0.181) and Kruskal-Wallis (p = 0.243) both failed to reject the null
- No pairwise comparisons were significant
- While 0-star teams allowed more points (116.31) than 1-star (113.82) and 2+ star (113.97) teams, the differences were not significant

**Interpretation:** Good defense appears to be a function of system, coaching, and role players — not star aggregation. Stars, for all their offensive value, do not meaningfully improve a team's defensive rating.

---

## Visualizations

### Figure A1: Win Percentage by Star Group

Teams with 2+ stars show the highest mean (0.634) and lowest variability (SD = 0.043). Teams with 0 stars show the lowest mean (0.396) and widest spread (SD = 0.136).

### Figure B1: Offensive Rating by Star Group

A clear rightward shift occurs as star count increases. The 2+ star group is tightly clustered around 118.05, while 0-star teams cluster around 112.79.

### Figure C1: Defensive Rating by Star Group

Substantial overlap across all three groups. No clear pattern emerges, consistent with the non-significant statistical results.

### Figure D1: Flowchart

Complete analytics logic from raw player data through filtering, grouping, merging, and statistical testing.

*(See Appendix in original report for full figures.)*

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Python (pandas, numpy) | Data cleaning and manipulation |
| seaborn, matplotlib | Visualization |
| BRScraper | Basketball Reference data |
| nba_api | NBA advanced stats |
| SciPy | ANOVA, Kruskal-Wallis |
| Statsmodels | Tukey HSD post-hoc tests |

---

## Repository Contents

| File | Description |
|------|-------------|
| `NBA_StarAnalysisReport.pdf` | Full project report with methodology, results, and appendices |
| `NBAStarAnalysis.ipynb` | Jupyter notebook with data cleaning, filtering, and statistical analysis |
| `data/` | Player and team statistics for 2025-26 season |
| `requirements.txt` | Python dependencies |
| `README.md` | This file |

---

## How to Run

1. Clone this repository:
   ```bash
   git clone https://github.com/danieljporter34/nba-star-impact-analysis.git
