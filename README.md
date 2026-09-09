# Automated Public Comparables Valuation Matrix

## Overview

The Automated Public Comparables Valuation Matrix is a Python-based equity valuation tool designed to automate the public comparable companies analysis commonly used in investment banking, equity research, and corporate finance.

The project allows the user to enter an Indian listed company as the target. The system then identifies the company's sector and industry, searches an Indian listed-company universe for relevant comparable companies, filters the peer set based on market capitalization, retrieves market and financial data, calculates valuation multiples, and applies peer valuation ranges to the target company.

The final output includes an investment banking-style trading comparables table, peer valuation statistics, implied target share prices, a football field valuation chart, and an Excel workbook containing the analysis.

The project is designed to demonstrate the practical application of relative valuation concepts through Python-based financial data automation.

---

## Project Objective

Public comparables analysis is a core component of investment banking and equity research. A traditional comps analysis requires an analyst to manually identify comparable companies, collect market capitalization and financial statement data, calculate Enterprise Value, spread LTM financials, calculate trading multiples, and update the analysis whenever market prices or financial results change.

This project aims to automate that workflow.

The primary objective is to create a reusable valuation engine where the analyst only needs to specify the target company. The system then performs the majority of the repetitive data collection and valuation calculations automatically.

---

## Key Features

### 1. Dynamic Target Company Selection

The notebook begins by asking the user to enter a target company or NSE ticker.

Examples:

```text
TCS
INFY
RELIANCE
HDFCBANK
MARUTI
```

The input is converted into the corresponding Yahoo Finance NSE ticker format and validated before the analysis proceeds.

---

### 2. Automatic Sector and Industry Identification

Once the target company is identified, the system retrieves its company metadata from Yahoo Finance.

The following information is extracted:

* Company name
* NSE ticker
* Sector
* Industry
* Market capitalization
* Current share price
* 52-week high
* 52-week low
* Shares outstanding
* Country
* Company website

The sector and industry classification are subsequently used to identify potential comparable companies.

---

### 3. Automated Peer Discovery

The project maintains an Indian listed-company search universe covering multiple industries, including:

* Information Technology
* Banking
* Financial Services
* Oil and Gas
* Automobiles
* Pharmaceuticals
* FMCG
* Industrials
* Metals
* Cement
* Consumer and Retail
* Telecom
* Power and Utilities
* Real Estate

The system compares the target company's industry with the industry classification of companies in the search universe.

The target company itself is excluded from the peer set.

Potential peers are then filtered based on market capitalization to avoid comparing companies with substantially different scales.

The largest qualifying companies are selected as the final comparable-company set.

---

## Valuation Methodology

The project uses relative valuation based on commonly used public-company trading multiples.

### Enterprise Value

Enterprise Value represents the value attributable to all capital providers.

The project uses:

```text
EV = Market Capitalization + Debt + Minority Interest - Cash
```

For the current implementation, minority interest is treated as zero unless separately incorporated into the data pipeline.

---

### EBITDA

The project calculates EBITDA using:

```text
EBITDA = EBIT + Depreciation
```

The financial statement extraction identifies operating profit, depreciation, and net profit from the company's Screener.in financial tables.

---

### EV / EBITDA

```text
EV / EBITDA = Enterprise Value / LTM EBITDA
```

EV/EBITDA is an enterprise-value-based trading multiple and is particularly useful for comparing companies with different capital structures.

---

### P/E

```text
P/E = Market Capitalization / LTM Net Income
```

P/E is an equity-value multiple that relates the market value of shareholders' equity to the company's earnings.

Companies with non-positive earnings are excluded from meaningful P/E calculations.

---

### P/B

```text
P/B = Market Capitalization / Book Value
```

P/B compares a company's market capitalization with its accounting book value.

---

## Peer Valuation Statistics

After calculating the trading multiples for the comparable companies, the project calculates:

* Minimum
* 25th percentile
* Median
* Mean
* 75th percentile
* Maximum

For example:

```text
                  Minimum   25th %ile   Median   Mean   75th %ile   Maximum
EV / EBITDA          ...
P / E                ...
P / B                ...
```

The 25th percentile, median, and 75th percentile are subsequently used to construct valuation ranges for the target company.

---

## Target Company Valuation

The target company's financial metrics are multiplied by the corresponding peer valuation multiples.

### EV / EBITDA Method

First, an implied Enterprise Value is calculated:

```text
Implied EV = Target LTM EBITDA × Selected EV / EBITDA Multiple
```

The implied equity value is then calculated:

```text
Implied Equity Value = Implied EV - Debt + Cash
```

Finally:

```text
Implied Share Price =
Implied Equity Value / Shares Outstanding
```

---

### P/E Method

```text
Implied Market Capitalization =
Target LTM Net Income × Selected P/E Multiple
```

Then:

```text
Implied Share Price =
Implied Market Capitalization / Shares Outstanding
```

---

### P/B Method

```text
Implied Market Capitalization =
Target Book Value × Selected P/B Multiple
```

Then:

```text
Implied Share Price =
Implied Market Capitalization / Shares Outstanding
```

---

## Valuation Range

Three valuation points are calculated for each methodology:

```text
Low Case     = 25th Percentile Peer Multiple
Base Case    = Median Peer Multiple
High Case    = 75th Percentile Peer Multiple
```

This provides a valuation range rather than relying on a single point estimate.

---

## Football Field Valuation

The project generates a classic investment banking-style football field chart.

The chart compares:

* 52-week trading range
* EV / EBITDA implied valuation range
* P/E implied valuation range
* P/B implied valuation range

The target company's current share price is displayed as a vertical reference line.

This allows the analyst to visually compare the company's current market price with the valuation ranges implied by comparable-company trading multiples.

---

## Data Sources

### Yahoo Finance

Yahoo Finance is used primarily for market and company-level information, including:

* Share price
* Market capitalization
* Shares outstanding
* 52-week high
* 52-week low
* Sector
* Industry
* Company metadata

The Python `yfinance` library is used to retrieve the data.

### Screener.in

Screener.in is used for Indian company financial statement data.

The project extracts financial statement tables to obtain items such as:

* Operating Profit
* Depreciation
* Net Profit
* Borrowings
* Cash / Cash Equivalents
* Equity

The financial data is subsequently processed using Python and pandas.

---

## Technology Stack

### Programming Language

* Python

### Data and Financial Analysis

* pandas
* NumPy
* yfinance

### Web Data Extraction

* requests
* BeautifulSoup
* lxml

### Visualization

* Matplotlib

### Output

* openpyxl
* Excel

### Environment

* Google Colab
* Jupyter Notebook

---

## Output

The project produces several analytical outputs.

### Trading Comps Table

The final comps table contains:

* Company
* Ticker
* Role
* Current Share Price
* Market Capitalization
* Enterprise Value
* EBITDA
* Net Income
* Book Value
* Debt
* Cash
* EV / EBITDA
* P/E
* P/B
* 52-week low
* 52-week high
* Data quality status

The target company is explicitly identified separately from its comparable companies.

---

### Peer Statistics

A separate output summarizes the distribution of each valuation multiple across the peer group.

```text
                 Minimum   25th %ile   Median   Mean   75th %ile   Maximum

EV / EBITDA          X.Xx       X.Xx      X.Xx    X.Xx      X.Xx       X.Xx
P/E                  X.Xx       X.Xx      X.Xx    X.Xx      X.Xx       X.Xx
P/B                  X.Xx       X.Xx      X.Xx    X.Xx      X.Xx       X.Xx
```

---

### Target Valuation

The target valuation output provides:

```text
Method          Low          Median          High

EV / EBITDA     ₹XXX         ₹XXX            ₹XXX
P/E             ₹XXX         ₹XXX            ₹XXX
P/B             ₹XXX         ₹XXX            ₹XXX
```

---

### Excel Workbook

The project automatically exports the analysis to an Excel workbook containing:

```text
Trading Comps
Peer Statistics
Target Valuation
Peer Group
Target Information
```

This makes the output easier to review, modify, and use as a starting point for further financial analysis.

---

## Data Quality Controls

The project includes basic data-quality checks to identify missing inputs.

Potential issues include:

* Missing share price
* Missing market capitalization
* Missing Enterprise Value
* Missing EBITDA
* Missing net income
* Missing book value

The project also excludes non-meaningful valuation multiples such as negative or zero P/E, P/B, and EV/EBITDA observations.

This is important because a mechanically calculated multiple is not necessarily an economically meaningful valuation metric.

---

## Financial Concepts Demonstrated

This project applies several core investment banking and equity research concepts:

* Relative valuation
* Comparable company analysis
* Enterprise Value
* Equity Value
* LTM financials
* EBITDA
* Market capitalization
* Net debt
* EV/EBITDA
* P/E
* P/B
* Peer-group analysis
* Valuation quartiles
* Median trading multiples
* Implied share price
* Sensitivity through valuation ranges
* Football field valuation

---

## Why This Project Matters

A public comparables analysis is one of the most common valuation exercises performed in investment banking and equity research.

The traditional process requires analysts to repeatedly collect market data, spread financial statements, calculate valuation multiples, update peer statistics, and revise valuation ranges.

This project demonstrates how that repetitive workflow can be converted into a reusable Python-based valuation pipeline.

Instead of manually rebuilding the analysis for every target company, the user can enter a different Indian listed company and allow the system to identify its industry, construct a relevant peer set, calculate trading multiples, and produce a valuation output.

The project therefore combines financial modeling knowledge with practical data automation.

---

## Limitations

The current version is designed as a functional valuation prototype rather than a production-grade financial data platform.

### Peer Universe

The current notebook uses a predefined search universe of Indian listed companies. Peer selection within that universe is dynamic, but the underlying universe itself is not yet sourced from a live NSE security master.

### Financial Statement Parsing

The project relies on web-scraped financial tables. Changes to the source website's HTML structure may require modifications to the extraction logic.

### LTM Calculation

The LTM methodology requires careful validation of quarterly column chronology. The notebook includes inspection steps because the source data structure should be verified before relying on automated LTM calculations.

### Valuation Interpretation

Comparable-company valuation is market-based and should not be interpreted as an intrinsic valuation by itself. Differences in growth, profitability, leverage, business mix, and risk can justify differences in trading multiples.

The output should therefore be treated as a valuation framework and analytical tool rather than an investment recommendation.

---

## Future Development

Potential extensions to the project include:

1. Replacing the predefined company universe with a live NSE security master.
2. Building a robust automated quarterly financial statement parser.
3. Automating true LTM calculations from the latest four reported quarters.
4. Adding consolidated versus standalone financial statement selection.
5. Incorporating minority interest, preferred equity, investments, and other EV adjustments.
6. Adding annual report and exchange filing links.
7. Adding historical trading multiple analysis.
8. Adding valuation sensitivity tables.
9. Building a standardized investment banking-style Excel template.
10. Adding automated PDF valuation reports.
11. Adding sector-specific operating metrics such as EV/Room for hotels or EV/Capacity for selected industries.
12. Adding historical snapshots to track how the target's valuation changes over time.

---

## How to Run

### Option 1: Google Colab

Open the notebook in Google Colab and execute the cells sequentially.

When prompted, enter an NSE-listed company ticker:

```text
TCS
```

The notebook will then execute the comparable-company valuation workflow.

### Option 2: Jupyter Notebook

Install the required packages:

```bash
pip install yfinance beautifulsoup4 lxml requests pandas numpy matplotlib openpyxl
```

Then open the notebook using Jupyter Notebook or JupyterLab.

---

## Example Use Case

Suppose the target company is:

```text
TCS
```

The system identifies:

```text
Sector:
Technology

Industry:
Information Technology Services
```

It then searches the Indian company universe for companies classified within the same industry and applies the market-cap filter.

The resulting peer group is used to calculate:

```text
EV / EBITDA
P/E
P/B
```

The peer median and quartile multiples are then applied to TCS's financial metrics to derive implied share-price ranges.

The final output is presented through a trading comps table, valuation summary, football field chart, and Excel workbook.

---

## Author's Note

This project was built to bridge the gap between financial valuation theory and practical financial-data automation.

The primary focus is not simply calculating valuation multiples, but understanding the workflow behind a public comparables analysis and translating that workflow into a repeatable Python process.

The project can be extended into a broader equity research and investment banking analytics platform by adding automated filings, historical financial data, forecasting capabilities, transaction comparables, DCF valuation, and standardized financial-model outputs.
