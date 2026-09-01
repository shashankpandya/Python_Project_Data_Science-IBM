# Course 5: Python Project for Data Science - Complete Study Guide
IBM Data Science Professional Certificate - Course 5 of 12
Learner: Pandya Shashank | Status: Completed - 100%

---

## Course Overview

Python Project for Data Science is a capstone mini-project that reinforces practical Python skills through real-world financial data analysis. The project involves extracting stock price data via yfinance, scraping quarterly revenue data with BeautifulSoup, and building an interactive Plotly dual-panel dashboard for Tesla and GameStop.

Key Libraries: yfinance, BeautifulSoup (bs4), requests, Pandas, Plotly

---

## Project 1: Tesla Stock and Revenue Analysis

### Step 1 - Extract Stock Data with yfinance

`python
import yfinance as yf
import pandas as pd

# Create Ticker object
tesla = yf.Ticker("TSLA")

# Download full price history (OHLCV - Open, High, Low, Close, Volume)
# period options: 1d, 5d, 1mo, 3mo, 6mo, 1y, 2y, 5y, 10y, ytd, max
tesla_data = tesla.history(period="max")

# Reset index to make Date a regular column (not the index)
tesla_data.reset_index(inplace=True)
tesla_data["Date"] = pd.to_datetime(tesla_data["Date"])

print(tesla_data.head())    # First 5 rows
print(tesla_data.shape)     # (rows, cols)
print(tesla_data.dtypes)    # Column data types
`

Key yfinance Ticker Attributes:
| Attribute | Returns |
|:---|:---|
| ticker.info | Dict of company metadata (name, sector, market cap, PE ratio) |
| ticker.history(period="1y") | OHLCV DataFrame for specified period |
| ticker.dividends | Series of historical dividend payments |
| ticker.splits | Series of historical stock splits |
| ticker.financials | Quarterly income statement DataFrame |
| ticker.balance_sheet | Balance sheet DataFrame |
| ticker.cashflow | Cash flow statement DataFrame |

### Step 2 - Scrape Revenue Data with BeautifulSoup

`python
import requests
from bs4 import BeautifulSoup
import pandas as pd

url = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/revenue.htm"
response = requests.get(url)
html_data = response.text   # Raw HTML string

# Parse HTML tree
soup = BeautifulSoup(html_data, "html.parser")

# Find all table elements
tables = soup.find_all("table")

# Extract specific table into DataFrame using pd.read_html
all_tables = pd.read_html(str(tables[1]))  # Index 1 = second table
tesla_revenue = all_tables[0]

# Rename columns
tesla_revenue.columns = ["Date", "Revenue"]

# Clean: remove commas and dollar signs
tesla_revenue["Revenue"] = (
    tesla_revenue["Revenue"]
    .str.replace(",", "")
    .str.replace("$", "")
)

# Remove empty/null rows
tesla_revenue.dropna(subset=["Revenue"], inplace=True)
tesla_revenue = tesla_revenue[tesla_revenue["Revenue"] != ""]

# Convert to numeric
tesla_revenue["Revenue"] = pd.to_numeric(tesla_revenue["Revenue"])

print(tesla_revenue.tail())
`

---

## Project 2: GameStop (GME) Stock and Revenue Analysis

### Extract GME Stock Data
`python
import yfinance as yf

gme = yf.Ticker("GME")
gme_data = gme.history(period="max")
gme_data.reset_index(inplace=True)
gme_data["Date"] = pd.to_datetime(gme_data["Date"])
print(gme_data.head())
`

### Scrape GME Revenue Data
`python
url_gme = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/stock.html"
response_gme = requests.get(url_gme)
soup_gme = BeautifulSoup(response_gme.text, "html.parser")
tables_gme = soup_gme.find_all("table")

gme_revenue = pd.read_html(str(tables_gme[1]))[0]
gme_revenue.columns = ["Date", "Revenue"]
gme_revenue["Revenue"] = gme_revenue["Revenue"].str.replace(",", "").str.replace("$", "")
gme_revenue.dropna(subset=["Revenue"], inplace=True)
gme_revenue = gme_revenue[gme_revenue["Revenue"] != ""]
gme_revenue["Revenue"] = pd.to_numeric(gme_revenue["Revenue"])
`

---

## Dashboard Function: make_graph()

`python
import plotly.graph_objects as go
from plotly.subplots import make_subplots
import pandas as pd

def make_graph(stock_data, revenue_data, stock):
    """
    Creates a two-panel interactive Plotly dashboard:
    - Top panel: Historical closing stock price (line chart)
    - Bottom panel: Historical quarterly revenue (bar chart)
    
    Args:
        stock_data   (DataFrame): columns 'Date' and 'Close'
        revenue_data (DataFrame): columns 'Date' and 'Revenue'
        stock        (str): Ticker name used in chart titles
    """
    
    # Filter to last 10 years for visual clarity
    recent_stock   = stock_data[stock_data["Date"] <= "2021-06-14"]
    recent_revenue = revenue_data[revenue_data["Date"] <= "2021-04-30"]
    
    # Create 2x1 subplot layout
    fig = make_subplots(
        rows=2,
        cols=1,
        shared_xaxes=True,      # Both panels share the x-axis
        subplot_titles=(
            f"Historical Share Price: {stock}",
            f"Historical Revenue: {stock}"
        ),
        vertical_spacing=0.1    # 10% gap between panels
    )
    
    # TOP PANEL: Line chart of closing price
    fig.add_trace(
        go.Scatter(
            x=pd.to_datetime(recent_stock["Date"]),
            y=recent_stock["Close"].astype("float"),
            name="Share Price",
            line=dict(color="royalblue", width=1.5)
        ),
        row=1, col=1
    )
    
    # BOTTOM PANEL: Bar chart of quarterly revenue
    fig.add_trace(
        go.Bar(
            x=pd.to_datetime(recent_revenue["Date"]),
            y=recent_revenue["Revenue"].astype("float"),
            name="Revenue (USD Million)",
            marker_color="mediumseagreen"
        ),
        row=2, col=1
    )
    
    # Axis labels and layout
    fig.update_yaxes(title_text="Price (USD)", row=1, col=1)
    fig.update_yaxes(title_text="Revenue (USD Million)", row=2, col=1)
    fig.update_layout(
        height=900,
        title_text=f"{stock} Stock Analysis Dashboard",
        title_font_size=18,
        template="plotly_white",
        showlegend=True
    )
    
    fig.show()

# Generate dashboards for both stocks
make_graph(tesla_data, tesla_revenue, "Tesla")
make_graph(gme_data,   gme_revenue,   "GameStop")
`

---

## Core Library Deep Dive

### yfinance Extended Patterns
`python
import yfinance as yf

# Company metadata
msft = yf.Ticker("MSFT")
print(msft.info["longName"])      # Microsoft Corporation
print(msft.info["marketCap"])     # Market cap in USD
print(msft.info["trailingPE"])    # Trailing Price/Earnings ratio
print(msft.info["sector"])        # Technology

# Download with explicit date range instead of period
hist = msft.history(start="2020-01-01", end="2023-12-31")

# Download multiple tickers simultaneously
multi = yf.download(["AAPL", "GOOGL", "MSFT"], period="1y", group_by="ticker")
aapl_close = multi["AAPL"]["Close"]

# Calculate daily percentage returns
hist["Daily_Return"] = hist["Close"].pct_change() * 100

# 30-day rolling average
hist["MA30"] = hist["Close"].rolling(window=30).mean()
`

### BeautifulSoup Key Patterns
`python
from bs4 import BeautifulSoup
import requests

html = requests.get("https://example.com").text
soup = BeautifulSoup(html, "html.parser")

# Searching the DOM
soup.find("h1")                       # First matching h1 tag
soup.find_all("table")                # All table tags as list
soup.find("div", class_="data")       # Div with specific CSS class
soup.find("a", id="main-link")        # Element by id attribute

# Extracting content
tag = soup.find("p")
tag.text                              # Plain inner text
tag.get_text(strip=True)             # Stripped plain text
tag["href"]                           # Attribute value (for anchor/link tags)
tag["src"]                            # Attribute value (for image/script tags)

# Navigating the tree
tag.parent                            # Parent element
list(tag.children)                    # Direct child elements
tag.find_next_sibling("td")           # Next sibling with matching tag

# Quick way to get ALL tables as DataFrames
import pandas as pd
all_dfs = pd.read_html(html)          # Returns list of DataFrames for each table
first_table = all_dfs[0]
`

### requests Library Key Patterns
`python
import requests

# GET request with query parameters
response = requests.get(
    "https://api.example.com/data",
    params={"symbol": "TSLA", "limit": 100},
    headers={"Authorization": "Bearer TOKEN"},
    timeout=15
)

# Check response
print(response.status_code)          # 200 OK, 404 Not Found, 500 Server Error
print(response.headers["Content-Type"])
data = response.json()               # Parse JSON body to Python dict/list

# POST request
resp = requests.post(
    "https://api.example.com/submit",
    json={"key": "value"},            # Sends Content-Type: application/json
    timeout=10
)

# Error handling (raises exception for 4xx/5xx responses)
response.raise_for_status()

# Download binary file (e.g., PDF, image)
with open("output.pdf", "wb") as f:
    f.write(requests.get(url).content)
`

---

## Assessment and Grade Summary

| Assessment | Weight | Score | Status |
|:---|:---:|:---:|:---:|
| Stock/Revenue Dashboard Project (Peer Reviewed) | 100% | 80/80 | Passed |
| **Overall Grade** | **100%** | **100%** | **PASSED** |

---

## Key Concepts Cheat Sheet

| Concept | Description | Code Pattern |
|:---|:---|:---|
| yf.Ticker(symbol) | Create Yahoo Finance ticker object | tesla = yf.Ticker("TSLA") |
| .history(period="max") | Get full OHLCV price history | data = ticker.history(period="1y") |
| BeautifulSoup parsing | Parse raw HTML to navigable tree | soup = BeautifulSoup(html, "html.parser") |
| soup.find_all("table") | Get all HTML table elements | tables = soup.find_all("table") |
| pd.read_html() | Extract tables from HTML to DataFrames | dfs = pd.read_html(html_str) |
| make_subplots(rows=2) | Create multi-panel Plotly figure | fig = make_subplots(rows=2, cols=1) |
| fig.add_trace(row=, col=) | Add chart to specific subplot panel | fig.add_trace(go.Scatter(...), row=1, col=1) |
| str.replace(",", "") | Strip commas from numeric strings | df["Revenue"].str.replace(",", "") |
| pd.to_numeric() | Convert string column to numeric dtype | pd.to_numeric(series, errors="coerce") |
| pct_change() | Calculate percentage change row-over-row | df["Close"].pct_change() * 100 |

---
IBM Data Science Professional Certificate - Pandya Shashank
