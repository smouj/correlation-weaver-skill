name: correlation-weaver
version: 2.1.0
description: Advanced statistical correlation discovery and pattern analysis engine for complex datasets
author: OpenClaw Analytics Team
tags:
  - correlation
  - statistics
  - patterns
  - data-mining
  - time-series
  - feature-engineering
category: analysis
dependencies:
  - python>=3.9
  - numpy>=1.21.0
  - pandas>=1.3.0
  - scipy>=1.7.0
  - scikit-learn>=0.24.0
  - matplotlib>=3.4.0
  - seaborn>=0.11.0
  - networkx>=2.6.0
  - statsmodels>=0.13.0
  - dask[dataframe]>=2021.10.0
  - numba>=0.55.0
required_env_vars:
  - CORRELATION_WEAVER_MAX_MEMORY_GB (default: 4)
  - CORRELATION_WEAVER_N_JOBS (default: -1)
  - CORRELATION_WEAVER_SIGNIFICANCE_LEVEL (default: 0.05)
  - CORRELATION_WEAVER_CACHE_DIR (default: ~/.cache/corr-weaver)
capabilities:
  - pearson_correlation
  - spearman_correlation
  - kendall_tau
  - mutual_information
  - time_lagged_correlation
  - partial_correlation
  - cross_correlation
  - granger_causality
  - anova_analysis
  - chi_square_test
  - correlation_matrix_visualization
  - correlation_network_graph
  - bootstrap_confidence_intervals
  - hierarchical_clustering
  - feature_selection
compatible_workflows:
  - data-mining
  - predictive-modeling
  - anomaly-detection
  - feature-engineering
  - exploratory-data-analysis
timeout_seconds: 1800
```

# Correlation Weaver

Advanced statistical correlation discovery engine for identifying non-obvious relationships, patterns, and dependencies in complex multidimensional datasets.

## Purpose

Correlation Weaver goes beyond simple pairwise correlations to discover hidden patterns, lagged relationships, conditional dependencies, and multi-variable interactions that standard analysis misses. Designed for data scientists, analysts, and engineers working with time series, sensor data, financial metrics, or any dataset where relationships are complex, non-linear, or delayed.

**Real Use Cases:**

- Detect leading/lagging indicators in financial markets: Identify which economic indicators predict stock movements 3-5 days ahead, even when immediate correlations appear random

- IoT sensor fault prediction: Find that temperature sensor A correlates with pressure sensor B 12 hours later, enabling early warning systems

- Customer behavior pattern discovery: Uncover that users who perform action X on Tuesday are 73% more likely to convert if they also viewed page Y within 48 hours (non-obvious temporal pattern)

- Manufacturing process optimization: Discover that adjusting machine parameter X only affects product quality after passing through intermediate machine Y with a 45-minute delay

- Marketing attribution beyond last-click: Identify that display ads on Thursday correlate with weekend conversions, not same-day clicks

- Supply chain risk detection: Find that supplier A's delivery delays predict supplier B's quality issues 8 days later through shared logistics constraints

- Network security anomaly detection: Discover that specific sequences of failed login patterns from different IP ranges correlate with successful breaches 72 hours later

## Scope

**IN-SCOPE Commands:**

```
correlation-weaver discover --dataset <path> --method <pearson|spearman|kendall|mutual|partial|cross|granger|all> [options]
  Discovers correlations using specified statistical method(s). Supports time-lag analysis, significance testing, and bootstrap confidence intervals.

correlation-weaver matrix --dataset <path> --threshold <float> --output <format>
  Generates correlation matrix heatmap with configurable threshold filtering. Output formats: png, svg, pdf, html_interactive.

correlation-weaver network --dataset <path> --min-correlation <float> --layout <circular|spring|shell|spectral|kamada_kawai>
  Builds network graph of correlated variables. Highlights clusters, centrality measures, and community structure.

correlation-weaver time-lag --dataset <path> --target <column> --max-lag <int> --step <int> --method <ccf|granger>
  Analyzes time-lagged relationships for target variable. Identifies optimal lag periods with statistical significance.

correlation-weaver conditional --dataset <path> --condition-on <col1,col2> --target <col3> --method <partial|cond_mutual>
  Computes conditional correlations controlling for confounding variables. Reveals direct vs indirect relationships.

correlation-weaver bootstrap --dataset <path> --iterations <int> --confidence <float>
  Calculates bootstrap confidence intervals for all correlations. Identifies statistically unstable correlations.

correlation-weaver feature-select --dataset <path> --target <column> --method <fwrapper|boruta|rfe> --n-features <int>
  Performs correlation-based feature selection for predictive modeling. Returns ranked feature list with p-values.

correlation-weaver cluster --dataset <path> --method <hierarchical|kmeans|dbscan> --distance <correlation|euclidean>
  Clusters variables based on correlation patterns. Identifies correlated feature groups for dimensionality reduction.

correlation-weaver compare --dataset1 <path> --dataset2 <path> --method <stability|drift|ks_test>
  Compares correlation structures across datasets (temporal split, A/B test, different segments). Quantifies correlation drift.

correlation-weaver export --results <path> --format <csv|json|pickle|excel> --include <list>
  Exports analysis results in various formats. Can include: correlations, p-values, confidence_intervals, network_data, cluster_assignments.

correlation-weaver validate --dataset <path> --ground-truth <path> --metric <precision|recall|f1>
  Validates discovered correlations against known relationships. Reports precision/recall of correlation detection.

correlation-weaver batch --directory <path> --pattern <glob> --output-dir <path> --parallel <bool>
  Processes multiple datasets in batch. Generates consolidated correlation report across all files.
```

**OUT-OF-SCOPE:**

- Data cleaning or preprocessing (use data-cleaner skill first)
- Machine learning model training (use model-trainer skill)
- Causal inference requiring controlled experiments (use causal-inference skill)
- Real-time streaming correlation (use stream-processor skill)
- Graphical model structure learning beyond correlation networks

## Detailed Work Process

### 1. Preparation Phase

**Input Validation:**
- Verify dataset exists at specified path
- Check file format (CSV, Parquet, Feather, HDF5 supported)
- Confirm minimum rows (default: 100) and columns (default: 3)
- Validate no single column has >80% missing values
- Detect and log data types for each column

**Environment Setup:**
```bash
export CORRELATION_WEAVER_MAX_MEMORY_GB=8  # Adjust based on dataset size
export CORRELATION_WEAVER_N_JOBS=4        # Parallel jobs (-1 = all cores)
export CORRELATION_WEAVER_SIGNIFICANCE_LEVEL=0.01  # Default p-value threshold
mkdir -p ~/.cache/corr-weaver
```

**Memory Estimation:**
- For dataset with n columns: memory ≈ (n² × 16 bytes) + (rows × columns × 8 bytes)
- Example: 1000 columns → ~16MB for correlation matrix alone
- If estimated memory > CORRELATION_WEAVER_MAX_MEMORY_GB, switch to Dask backend automatically

### 2. Discovery Phase

**Method Selection Logic:**
```bash
# All methods (most comprehensive, slowest)
correlation-weaver discover --dataset data.csv --method all

# Fast Pearson for linear relationships
correlation-weaver discover --dataset data.csv --method pearson --threshold 0.7

# Spearman for monotonic non-linear relationships
correlation-weaver discover --dataset data.csv --method spearman --threshold 0.6

# Mutual information for any dependency type
correlation-weaver discover --dataset data.csv --method mutual --bins 10 --n_neighbors 5

# Time-lagged cross-correlation for time series
correlation-weaver discover --dataset timeseries.csv --method cross --max-lag 30 --step 1

# Granger causality for forecasting relationships
correlation-weaver discover --dataset economic.csv --method granger --max-lag 10 --test ngranger
```

**Significance Testing:**
- By default: p-value < 0.05 considered significant
- Bonferroni correction optional: `--correction bonferroni`
- Benjamini-Hochberg FDR: `--correction fdr`
- Bootstrap CI: `--bootstrap 1000 --ci 0.95`

**Handling Missing Data:**
- Pairwise deletion (default): use all available pairs
- Complete case: `--complete-cases-only`
- Imputation: `--impute median|mean|knn|iterative` (not built-in; preprocess first)

### 3. Analysis Phase

**Correlation Matrix Construction:**
- Computes upper triangle only (symmetric matrix)
- Diagonal set to 1.0
- Missing correlations marked as NaN
- Stores both raw correlation and adjusted p-value

**Network Graph Generation:**
- Nodes: variables with significant correlations
- Edges: |correlation| >= threshold
- Edge width: proportional to absolute correlation
- Edge color: positive (blue) vs negative (red)
- Node size: proportional to degree centrality
- Communities: Louvain algorithm for clustering

**Time-Lag Analysis:**
- Computes cross-correlation function (CCF) for each pair at each lag
- Granger causality tests for each lag
- Identifies peak correlation lag for each relationship
- Stores lag value and correlation at that lag

**Conditional Correlation:**
- Partial correlation: controls for confounding variables
- Conditional mutual information: non-linear conditional dependencies
- Results: conditional correlation matrix vs unconditional

### 4. Visualization Phase

**Matrix Heatmap:**
```
correlation-weaver matrix \
  --dataset data.csv \
  --threshold 0.5 \
  --output svg \
  --annotate True \
  --cmap "RdBu_r" \
  --figsize 12 10 \
  --title "Feature Correlations (|r| > 0.5)" \
  --save correlations_heatmap.svg
```

**Network Graph:**
```
correlation-weaver network \
  --dataset data.csv \
  --min-correlation 0.7 \
  --layout spring \
  --node-coloring community \
  --edge-width-range 1 5 \
  --save network_graph.html \
  --interactive
```

**Lag Plot:**
```bash
correlation-weaver time-lag \
  --dataset timeseries.csv \
  --target sales \
  --max-lag 30 \
  --step 1 \
  --plot ccf_plot.png \
  --highlight-lags 7,14,21
```

### 5. Reporting Phase

**Result Structure:**
```
results/
├── correlations.csv          # columns: var1, var2, correlation, p_value, ci_lower, ci_upper
├── significant_pairs.json    # list of significant (var1,var2) with effect size
├── network/
│   ├── nodes.csv            # variable, degree, betweenness, community
│   ├── edges.csv            # source, target, weight, lag (if any)
│   └── graph.graphml        # NetworkX graph file
├── plots/
│   ├── correlation_matrix.svg
│   ├── network_graph.html
│   ├── time_lag_analysis.png
│   └── conditional_heatmap.svg
└── summary.txt              # Key findings and recommendations
```

**Summary Report Includes:**
- Number of variables analyzed
- Number of significant correlations found
- Strongest positive/negative correlations
- Identified lagged relationships
- Clustered variable groups
- Recommended features for predictive modeling
- Potential data quality issues (high missingness, constant values)

## Golden Rules

1. **Always validate assumptions before Pearson correlation:**
   - Check for normality with Shapiro-Wilk or Kolmogorov-Smirnov test
   - If violated, automatically switch to Spearman or transform data
   - Command: `correlation-weaver discover --method pearson --check-assumptions`

2. **Beware of Simpson's paradox:**
   - If a correlation reverses when conditioning on a third variable, highlight it
   - Always run conditional correlation analysis on strong overall correlations
   - The `--conditional-on` flag must be used when overall correlation > |0.5|

3. **Time series require stationarity checks:**
   - Augmented Dickey-Fuller test mandatory for time-lag analysis
   - Non-stationary data must be differenced or detrended first
   - Flag: `--stationarity-check strict` (fails if non-stationary)

4. **Multiple testing correction is not optional:**
   - With n variables, ~n²/2 tests performed
   - Always use Bonferroni or FDR unless user explicitly overrides with `--no-correction`
   - Report both raw p-values and adjusted p-values

5. **Missing data pattern matters:**
   - If missingness > 30% in any column, exclude from analysis
   - If missingness pattern is not random (MNAR), correlation estimates are biased
   - Run Little's MCAR test: `correlation-weaver validate --missingness-test`

6. **Correlation ≠ causation rule:**
   - Every output file must include disclaimer: "Correlation does not imply causation"
   - For Granger causality, explicitly state it only tests predictive precedence, not true causation
   - Never claim causation in summary.txt

7. **Sample size requirements:**
   - Minimum 10 observations per variable for reliable correlation estimates
   - For n=1000 and 100 variables, each pairwise correlation has ~1000 observations (pairwise deletion)
   - If effective sample size < 50 for any pair, exclude from report

8. **Categorical variable handling:**
   - One-hot encode automatically if user specifies `--auto-categorical`
   - For ordinal variables, use Spearman by default
   - For nominal variables with >2 categories, use Cramér's V (via `--method association`)

9. **No data leakage in time series:**
   - When computing rolling correlations, ensure no future data leaks into past
   - For train/test splits, compute correlations separately to avoid leakage
   - Use `correlation-weaver compare --metric drift` to detect temporal instability

10. **Always provide confidence intervals:**
    - Bootstrap with at least 1000 iterations for CI
    - Report 95% CI for all significant correlations
    - If CI includes 0, correlation is not robust even if p < 0.05

## Examples

### Example 1: Financial Market Analysis

**User input:**
```bash
correlation-weaver time-lag \
  --dataset stock_returns_2020_2025.csv \
  --target "SP500_daily_return" \
  --max-lag 20 \
  --step 1 \
  --method ccf \
  --significance-level 0.01 \
  --bootstrap 2000 \
  --output lagged_correlations.json \
  --plot sp500_lead_indicators.png
```

**Real output:**
```json
{
  "target": "SP500_daily_return",
  "analysis_period": "2020-01-01 to 2025-03-15",
  "total_observations": 1332,
  "significant_lagged_correlations": [
    {
      "predictor": "10year_treasury_yield",
      "optimal_lag": -3,
      "correlation_at_optimal_lag": -0.234,
      "p_value": 0.0012,
      "ci_95_lower": -0.312,
      "ci_95_upper": -0.148,
      "interpretation": "Treasury yield 3 days ago negatively predicts today's SP500 return"
    },
    {
      "predictor": "VIX_closing",
      "optimal_lag": 0,
      "correlation_at_optimal_lag": -0.567,
      "p_value": "<0.0001",
      "ci_95_lower": -0.634,
      "ci_95_upper": -0.492,
      "interpretation": "Same-day VIX strongly negatively correlated with SP500"
    },
    {
      "predictor": "oil_price",
      "optimal_lag": 5,
      "correlation_at_optimal_lag": 0.089,
      "p_value": 0.042,
      "ci_95_lower": 0.003,
      "ci_95_upper": 0.174,
      "interpretation": "Oil price 5 days later positively predicts SP500 (weak but significant)"
    }
  ],
  "warning": "All correlations bootstrap CI excludes 0 except oil_price (CI barely excludes 0, effect small)"
}
```

### Example 2: Manufacturing Process Discovery

**User input:**
```bash
# Step 1: Run full correlation analysis
correlation-weaver discover \
  --dataset manufacturing_sensors.csv \
  --method all \
  --bootstrap 5000 \
  --significance-level 0.001 \
  --output-dir manufacturing_analysis/

# Step 2: Build network of strong correlations
correlation-weaver network \
  --dataset manufacturing_sensors.csv \
  --min-correlation 0.6 \
  --layout kamada_kawai \
  --node-coloring community \
  --save manufacturing_analysis/network.html \
  --interactive

# Step 3: Focus on target (product_defect_rate) with time-lag
correlation-weaver time-lag \
  --dataset manufacturing_sensors.csv \
  --target product_defect_rate \
  --max-lag 48 \
  --step 4 \
  --method ccf \
  --save manufacturing_analysis/defect_predictors.json
```

**Real output summary.txt excerpt:**
```
=== MANUFACTURING CORRELATION ANALYSIS ===
Dataset: 150 sensor readings × 45,678 production hours
Variables: 150 (temperature_1..48, pressure_1..32, vibration_1..24, speed_1..20, quality_metrics_26)

SIGNIFICANT CORRELATIONS (p < 0.001, |r| > 0.3):
- 1,247 pairwise correlations found
- Strongest positive: conveyor_belt_speed_7 ↔ vibration_motor_12 (r=0.84)
- Strongest negative: coolant_temperature_3 ↔ defect_rate (r=-0.56)

TIME-LAGGED PREDICTORS OF DEFECT_RATE (48-hour window):
Critical finding: vibration_sensor_18 shows peak correlation r=0.72 at lag +16 hours
Interpretation: High vibration on machine B 16 hours BEFORE inspection predicts defects
Lead time enables: 2 shifts of intervention before quality check

CLUSTER ANALYSIS:
5 distinct correlation communities detected:
1. Temperature cluster (23 sensors) - highly internally correlated (r > 0.7)
2. Pressure cluster (18 sensors)
3. Vibration cluster (31 sensors) - contains defect predictors
4. Speed cluster (15 sensors)
5. Quality cluster (12 metrics) - low correlation with others

RECOMMENDATION:
Monitor vibration_sensor_18 continuously. Alert threshold: correlation trend > 0.5
Trigger maintenance dispatch if 16-hour ahead prediction > 0.65
```

### Example 3: Customer Behavior Pattern Discovery

**User input:**
```bash
# Prepare data: user events with timestamps, aggregated by user-week
# Assume already preprocessed with data-cleaner skill

correlation-weaver conditional \
  --dataset user_behavior_weekly.csv \
  --condition-on "user_age_group,account_tenure_months" \
  --target "converted_to_premium" \
  --method cond_mutual \
  --bootstrap 2000 \
  --output conditional_dependencies.json

correlation-weaver feature-select \
  --dataset user_behavior_weekly.csv \
  --target converted_to_premium \
  --method fwrapper \
  --n-features 20 \
  --metric mutual_info \
  --save selected_features.csv
```

**Real output significant_pairs.json excerpt:**
```json
{
  "analysis": "Conditional mutual information controlling for user demographics",
  "target": "converted_to_premium",
  "conditional_on": ["user_age_group", "account_tenure_months"],
  "top_predictors": [
    {
      "feature": "tuesday_session_count",
      "conditional_mi": 0.0423,
      "unconditional_mi": 0.0112,
      "p_value": 0.0008,
      "interpretation": "Tuesday usage is 3.8x more predictive after controlling for age/tenure"
    },
    {
      "feature": "viewed_pricing_page_within_48h",
      "conditional_mi": 0.0387,
      "unconditional_mi": 0.0341,
      "p_value": 0.052,
      "interpretation": "Pricing page visits only mildly more predictive conditional; weak signal"
    },
    {
      "feature": "used_feature_X_three_times",
      "conditional_mi": 0.0312,
      "unconditional_mi": 0.0089,
      "p_value": 0.0015,
      "interpretation": "Feature X engagement is 3.5x more predictive conditional (non-linear pattern)"
    }
  ]
}
```

### Example 4: Batch Processing Multiple Datasets

**User input:**
```bash
correlation-weaver batch \
  --directory /data/sensor_readings/2025/03/ \
  --pattern "sensor_*.parquet" \
  --output-dir /reports/march_2025_correlations/ \
  --parallel true \
  --method spearman \
  --threshold 0.5 \
  --consolidate final_correlation_report.html
```

**Real command execution:**
```bash
# Internal parallel execution
xargs -P 8 -I {} correlation-weaver discover --dataset {} --method spearman --threshold 0.5 --output-dir /tmp/individual_reports/ ::: /data/sensor_readings/2025/03/sensor_*.parquet

# Consolidation: finds correlations consistent across >70% of files
python consolidate_correlations.py \
  --input-dir /tmp/individual_reports/ \
  --consistency-threshold 0.7 \
  --output /reports/march_2025_correlations/final_report.html
```

**Consolidated report identifies:**
- Correlation between coolant_temp_5 and vibration_11 appears in 94% of daily files (mean r=0.73)
- Same correlation disappears on days with ambient_temp > 35°C (20 files) → conditional pattern
- Recommendation: Install coolant chiller to stabilize this relationship

## Rollback Commands

**1. Revert to previous analysis version:**
```bash
# If using git for results tracking
cd /path/to/analysis/results
git log --oneline correlations.csv
git checkout <commit_hash> correlations.csv network/

# Re-generate with different parameters
correlation-weaver discover \
  --dataset data.csv \
  --method spearman \
  --threshold 0.4 \
  --output-dir analysis_v2/
```

**2. Undo network graph changes (if saved over original):**
```bash
# If you overwrote nodes.csv/edges.csv
correlation-weaver export \
  --results previous_results.pickle \
  --format pickle \
  --include all

# Restore from backup (auto-created every run)
cp ~/.cache/corr-weaver/backup_2025-03-15_10-30-00/nodes.csv .
cp ~/.cache/corr-weaver/backup_2025-03-15_10-30-00/edges.csv .
```

**3. Disable a discovered correlation from downstream use:**
```bash
# Edit significant_pairs.json to remove false positive
jq 'del(.[] | select(.var1=="sensor_A" and .var2=="sensor_B"))' \
  significant_pairs.json > significant_pairs_filtered.json

# Re-build network without that edge
correlation-weaver network \
  --use-significant-pairs significant_pairs_filtered.json \
  --min-correlation 0.6 \
  --save network_filtered.html
```

**4. Rescue from memory exhaustion:**
```bash
# If analysis failed with OOM, retry with Dask
export CORRELATION_WEAVER_USE_DASK=true
correlation-weaver discover \
  --dataset huge_dataset.csv \
  --method pearson \
  --chunksize 10000 \
  --output results/
```

**5. Recover from corrupted cache:**
```bash
rm -rf ~/.cache/corr-weaver/
# Re-run analysis (will be slower first time)
correlation-weaver discover --dataset data.csv --method all

# Or restore from last successful backup if available
cp ~/.cache/corr-weaver/last_successful_backup/* ~/.cache/corr-weaver/
```

**6. Full cleanup and restart:**
```bash
# Remove all correlation-weaver outputs
find . -name "*correlation*" -type f -delete
find . -name "*network*" -type f -delete
rm -rf correlation_analysis/ results/

# Clear cache
rm -rf ~/.cache/corr-weaver/

# Re-run with fresh parameters
correlation-weaver batch \
  --directory clean_data/ \
  --pattern "*.csv" \
  --output-dir fresh_analysis/
```

**7. Rollback specific visualization to simpler version:**
```bash
# If complex network graph is too messy, simplify
correlation-weaver network \
  --dataset data.csv \
  --min-correlation 0.8 \
  --max-nodes 50 \
  --layout circular \
  --save network_simplified.html

# Instead of previous min-correlation 0.5 with 150 nodes
```

## Troubleshooting

**Issue:** "MemoryError: Unable to allocate correlation matrix for 500 variables"

**Solution:**
```bash
# Estimate: 500² × 8 bytes ≈ 2MB per matrix (float64) - this should work
# Problem likely is storing all intermediate pairwise results

# Fix 1: Use fewer computations
correlation-weaver discover \
  --dataset data.csv \
  --method pearson \
  --sample-variables 100 \   # Randomly sample 100 variables
  --threshold 0.7           # Only compute |r| > 0.7 (sparse matrix)

# Fix 2: Use Dask for out-of-core
export CORRELATION_WEAVER_USE_DASK=true
correlation-weaver discover --dataset huge.csv --chunksize 5000

# Fix 3: Increase swap (Linux)
sudo fallocate -l 16G /swapfile_extra
sudo chmod 600 /swapfile_extra
sudo mkswap /swapfile_extra
sudo swapon /swapfile_extra
```

**Issue:** "All p-values are NaN after bootstrap"

**Cause:** Too many missing pairs causing insufficient bootstrap samples

**Solution:**
```bash
# Check missingness pattern first
correlation-weaver validate --dataset data.csv --missingness-test

# Use pairwise deletion explicitly
correlation-weaver discover \
  --dataset data.csv \
  --method spearman \
  --pairwise-deletion \
  --bootstrap 1000 \
  --min-observations 30  # Only compute correlations with ≥30 pairs

# Or impute first (outside this skill)
# Use data-imputer skill then re-run
```

**Issue:** "Granger causality test failed: series is non-stationary"

**Solution:**
```bash
# Test stationarity
correlation-weaver validate --dataset timeseries.csv --stationarity-test

# If non-stationary, difference the data first (outside this skill)
# Or use method=ccf which doesn't require stationarity

# Forcing Granger with differencing (not recommended):
correlation-weaver time-lag \
  --dataset timeseries.csv \
  --method granger \
  --differencing order1 \   # Auto difference once
  --test ngc               # Use non-parametric Granger
```

**Issue:** "Network graph is unreadable (too many edges)"

**Solution:**
```bash
# Increase threshold
correlation-weaver network \
  --dataset data.csv \
  --min-correlation 0.8 \    # Was 0.5
  --max-edges 200 \          # Only show top 200 strongest edges
  --filter-by p_value \      # Only include edges with p < 0.001
  --layout fruchterman_reingold \  # Better layout for sparse graphs
  --save network_simplified.html

# Or cluster first, show cluster centroids
correlation-weaver cluster \
  --dataset data.csv \
  --method hierarchical \
  --n-clusters 10

# Then compute cluster-averaged correlations for network
```

**Issue:** "Conditional correlation gives different signs than unconditional"

**Diagnosis:**
This is Simpson's paradox - expected, not an error.

**Verification:**
```bash
# Compare unconditional vs conditional
correlation-weaver discover --method pearson > unconditional.txt
correlation-weaver conditional --condition-on confounder1 > conditional.txt

# Examine pairs with sign reversal
grep "negative.*positive" conditional.txt

# Accept as valid finding: confounding variable was distorting true relationship
```

**Issue:** "Bootstrap confidence intervals extremely wide (CI includes 0 for almost all)"

**Causes:** Small effective sample size due to missing data

**Solutions:**
```bash
# Check effective N per correlation
correlation-weaver discover \
  --dataset data.csv \
  --report-observations-per-pair \
  --min-observations 100 \   # Filter out pairs with <100 obs
  --bootstrap 1000

# Increase bootstrap iterations for more stable CI (slower)
correlation-weaver discover \
  --dataset data.csv \
  --bootstrap 10000 \    # Default is 1000
  --ci 0.9              # Use 90% CI instead of 95% (narrower)
```

**Issue:** "correlation-weaver: command not found after installation"

**Fix:**
```bash
# Verify installation location
which correlation-weaver || echo "Not in PATH"

# Add to PATH if installed via pip
export PATH="$HOME/.local/bin:$PATH"   # Linux default
# or
export PATH="/usr/local/bin:$PATH"

# For persistent fix, add to ~/.bashrc or ~/.zshrc
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

# Reinstall with explicit path
pip install --user --upgrade correlation-weaver
```

**Issue:** "ImportError: cannot import name 'dask' from 'sklearn'"

**Cause:** sklearn API change in version 1.2+

**Solution:**
```bash
# Pin compatible versions
pip install 'scikit-learn>=0.24.0,<1.2.0' 'dask[dataframe]>=2021.10.0'

# Or upgrade correlation-weaver to version using new API
pip install --upgrade correlation-weaver

# Check version
correlation-weaver --version
```

**Issue:** "Network graph export fails: 'GraphML write failed for node attribute'"

**Cause:** Node attributes contain non-serializable types (e.g., arrays, None)

**Solution:**
```bash
# Use simpler export
correlation-weaver network \
  --dataset data.csv \
  --format csv \          # Instead of graphml
  --node-attributes name,degree,community \
  --save network_nodes.csv network_edges.csv

# Or clean attributes before export
python -c "
import pandas as pd
nodes = pd.read_csv('nodes.csv')
# Convert dict columns to JSON strings
for col in nodes.select_dtypes(include=object):
    if nodes[col].apply(lambda x: isinstance(x, dict)).any():
        import json
        nodes[col] = nodes[col].apply(json.dumps)
nodes.to_csv('nodes_clean.csv', index=False)
"
```

**Issue:** "Time-lag analysis produces NaN at all lags"

**Diagnose:**
```bash
# Check if time series is properly sorted
head -5 timeseries.csv | grep timestamp

# Check for constant values
correlation-weaver validate --dataset timeseries.csv --constant-check

# Likely: target variable has no variance
# Solution: Drop constant columns from analysis
correlation-weaver discover --dataset timeseries.csv --drop-constant --threshold 0.001
```

**Issue:** "Conditional mutual information computation extremely slow"

**Optimization:**
```bash
# Reduce number of conditioning variables (curse of dimensionality)
correlation-weaver conditional \
  --dataset data.csv \
  --condition-on "only_top_3_confounders" \  # Manually select
  --method ksg \          # Kraskov-Stögbauer-Grassberger (fastest)
  --n-neighbors 5 \       # Reduce from default 10
  --bins auto \           # Let it choose optimal binning
  --subsample 10000       # Use subset of data for MI estimation
```

**Issue:** "export command fails: 'Unknown format: xlsx'"

**Fix:**
```bash
# Install optional dependency for Excel export
pip install openpyxl xlsxwriter

# Use supported format
correlation-weaver export \
  --format csv \    # or json, pickle
  --include correlations,p_values

# For Excel specifically:
correlation-weaver export \
  --format excel \  # Requires openpyxl
  --sheet-name Correlations
```

**Performance Tuning for Large Datasets (n > 1000):**

```bash
# 1. Sample variables first to identify most interesting
correlation-weaver discover \
  --dataset huge.csv \
  --method pearson \
  --pre-variance-filter 0.01 \   # Remove near-zero variance
  --pre-correlation-sample 200 \ # Sample 200 random variables for initial screen
  --output initial_screen.json

# 2. From initial screen, select top 50 variables
# (Manual step: extract variable names from initial_screen.json)

# 3. Run full analysis on selected subset
correlation-weaver discover \
  --dataset huge.csv \
  --variables-from initial_screen.json --top-n 50 \
  --method all \
  --bootstrap 2000 \
  --output final_analysis/
```

**Memory-Efficient Batch Processing:**

```bash
# Use Dask with explicit partitioning
export DASK_SCHEDULER=threads
export CORRELATION_WEAVER_N_JOBS=4

correlation-weaver batch \
  --directory /big/data/ \
  --pattern "*.parquet" \
  --output-dir /reports/ \
  --parallel \
  --chunksize 50000 \    # Process 50k rows at a time
  --method spearman \    # Spearman uses ranking (Dask-friendly)
  --persist-intermediate  # Cache intermediate results to disk
```

## Verification Steps

**After running correlation-weaver discover:**

1. Check exit code: `echo $?` should be 0
2. Verify output files exist:
   ```bash
   ls -lah results/correlations.csv
   ls -lah results/summary.txt
   ```
3. Inspect summary:
   ```bash
   grep "significant" results/summary.txt
   ```
4. Validate correlation values are in [-1, 1]:
   ```bash
   awk -F, 'NR>1 && $3 !>= -1 && $3 <=1 {print "INVALID:", $1, $2, $3}' results/correlations.csv
   ```
5. Check p-values are numeric and in [0, 1]:
   ```bash
   awk -F, 'NR>1 && ($4 < 0 || $4 > 1) {print "INVALID_P:", $1, $2, $4}' results/correlations.csv
   ```

**After running network analysis:**

1. Verify node count matches unique variables:
   ```bash
   wc -l results/network/nodes.csv  # Should be reasonable (not 100k)
   ```
2. Check edge consistency:
   ```bash
   awk -F, 'NR>1 && ($3 < -1 || $3 > 1) {print "INVALID_EDGE:", $1, $2, $3}' results/network/edges.csv
   ```
3. For GraphML export, validate XML:
   ```bash
   xmllint --noout results/network/graph.graphml && echo "Valid XML"
   ```

**After time-lag analysis:**

1. Verify lag values within requested range:
   ```bash
   jq '.significant_lagged_correlations[].optimal_lag' results/lagged_correlations.json | sort -n
   ```
2. Check no gaps in lags (if `--step 1`):
   ```bash
   # Should produce consecutive integer lags
   ```

**Consistency check across multiple runs:**

```bash
# Run same analysis twice
correlation-weaver discover --dataset data.csv --method pearson --seed 42 > run1.txt
correlation-weaver discover --dataset data.csv --method pearson --seed 42 > run2.txt

# Compare top correlations (should be identical with same random seed)
diff <(grep "correlation:" run1.txt | head -20) <(grep "correlation:" run2.txt | head -20)
# Exit code 0 = identical, 1 = differences (check if bootstrap seeds differ)
```

**Cross-validation with R:**
```bash
# Export data, compute correlation in R for spot-check
csvcut -c var1,var2 data.csv | head -1000 > spot_check.csv
Rscript -e "cor(t(read.csv('spot_check.csv')), use='pairwise.complete.obs')" > r_cor.txt

# Compare with Python result for var1-var2
grep "var1,var2" results/correlations.csv
```