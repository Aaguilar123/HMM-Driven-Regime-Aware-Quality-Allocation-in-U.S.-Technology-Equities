# **HMM-Driven-Regime-Aware-Quality-Allocation-in-U.S.-Technology-Equities BDA Project Github Structure**


The workflow is divided into two main Jupyter notebooks, each corresponding to a key stage of the research process. Each stage in the BDA Project builds directly on the previous one.

## **1. Data Collection and Exploratory Data Analysis (Data_Collection_and_EDA.ipynb)**

The first notebook `Data_Collection_and_EDA.ipynb`, constructs the pillars of the dataset. It includes the ingestion of fundamental data that was later transformed into a structured, point-in-time aligned `Thesis_Fundamentals.csv` dataset for the `THE_ENGINE.ipynb`. It accessed the quarterly Excel files `"stock"_quarterly_all_descending.xlsx` downloaded from FastFS https://fastfs.org/ using free credits exactly limited to 10 csv downloads. On the other hand, market price data was extracted with Yahoo Finance, followed by preprocessing steps such as cleaning, alignment, and time-series formatting, creating `market_data_2007_2026.csv` for further use in modelling.

As part of the Exploratory Data Analysis, statistical testing, distribution analysis, volatility assessment, and examination of factor behavior was conducted serving as economical and statistical foundation for future modelling.

## **2. Model Development, Regime Detection, and Evaluation (THE_ENGINE)**

The second notebook contains the complete implementation of the strategy, including factor construction, regime detection, portfolio construction, and evaluation. It uses the structured market dataset (`market_data_2007_2026.csv`) together with the `Thesis_Fundamentals.csv` and prepares the separated Training, validation, and holdout periods (2007–2017, 2018–2024, and 2025–2026) 

Fundamental data is aligned with market returns using a backward-looking `merge_asof` procedure with a 60-day reporting lag to eliminate look-ahead bias. From the aligned financial statements, two core quality factors are constructed: Return on Equity (ROE) calculated as *Net Income divided by Total Equity*, and Free Cash Flow Margin (FCF Margin) calculated as *(Operating Cash Flow + Capital Expenditures) divided by Revenue*. It’s important to note that Capital Expenditures are reported as negative cash outflows in the raw FastFS cash flow statements, therefore the addition has to be interpreted as subtraction in the traditional Free Cash Flow formulation.   These ratios are computed in a time-consistent manner using only information available at each market date. As an intermediate auditing step, the aligned dataset is saved as `aligned_fundamentals_debug.csv`, serving as a verification snapshot of the alignment and transformation logic.

The modeling framework combines ranking-based factor selection with a two-state Gaussian Hidden Markov Model implemented. Portfolio construction follows a Top 30% selection rule with periodic rebalancing and incorporates transaction cost assumptions to ensure realistic performance evaluation. The notebook concludes with validation analysis, regime diagnostics, statistical significance testing, downside protection evaluation, and full unseen holdout testing to assess robustness and generalization.

---

# **3. Design Philosophy**

The repository is structured in an academic manner and avoids unnecessary complexity and instead separates the workflow into two clearly defined stages: data preparation and model implementation.

All methodological refinements, including factor reduction, ranking methodology, rebalancing frequency, benchmark selection, and regime integration, are explained within the thesis instead of being separated into additional scripts or notebooks for reproducibility. On the other hand, `THE_ENGINE.ipynb` keeps some simple and clear key interpretations.

---

# **4. Supporting Files**

The repository is constructed as follows:

- **README.md** : Provides an overview of the project structure, workflow logic, and step-by-step instructions to reproduce the results.

- **requirements.txt** : Lists all Python dependencies required to execute the notebooks and reproduce the results.

- **data/** : Directory containing all datasets used in the project, including:
  - `market_data_2007_2026.csv`
  - `Thesis_Fundamentals.csv`
  - `aligned_fundamentals_debug.csv`
  - Quarterly fundamental Excel files for all included companies (`AAPL`, `ADBE`, `AMAT`, `AMD`, `CSCO`, `INTC`, `INTU`, `MSFT`, `NVDA`, `QCOM`).

- **Data_Collection_and_EDA.ipynb** : Notebook for data construction, preprocessing, and exploratory analysis.

- **THE_ENGINE.ipynb** : Notebook containing the final model implementation, regime detection, evaluation, and out-of-sample testing.

---

# **5. Dependencies and Reproducibility**

The project was implemented in Python within a Jupyter Notebook environment. The Libraries `pandas` and `numpy` were used for data manipulation, vectorized computations, time-series alignment, rolling calculations, and portfolio construction. Statistical analysis and hypothesis testing were supported using `scipy`, while `matplotlib` and `seaborn` were used for the visualizations and performance diagnostics of these. The two-state Gaussian Hidden Markov Model at the beginning of the framework was implemented using `hmmlearn`, and historical market data was collected using `yfinance`.

All datasets, notebooks, and supporting files required to reproduce the results are included directly in the repository, therefore no additional manual downloads or external data collection steps are necessary.
