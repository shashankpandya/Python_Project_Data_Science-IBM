# Course Study & Project Guide: Python Project for Data Science (Course 5)

**Learner**: Pandya Shashank / Pandya Nareshbhai  
**Course Status**: **PASSED**  
**Final Grade**: **95%**  

---

## 📋 Course Overview & Objectives

*Python Project for Data Science* is the capstone hands-on project course in the IBM Data Science Professional Certification. It tests real-world application of Python fundamentals, web scraping, financial data extraction, and interactive dashboard creation using financial market data for **Tesla (TSLA)** and **GameStop (GME)**.

---

## 🏆 Final Assessment Results & Grades Breakdown

| Assessment Item | Weight | Status | Grade Achieved |
|---|---|---|---|
| **1. Extracting Stock Data Using a Python Library** | 20% | **Success: Passed** | **90%** |
| **2. Extracting Stock Data Using Web Scraping** | 20% | **Success: Passed** | **100%** |
| **3. Final Assignment: Analyzing Historical Data** | 60% | **Success: Passed** | **95%** |
| **OVERALL COURSE GRADE** | **100%** | **PASSED** | **95%** |

---

## 📚 Module Breakdown & Key Concepts

### Module 1: Extracting Stock Data (`yfinance` & Web Scraping)
- **`yfinance` Library**:
  - `yf.Ticker("TSLA")`: Instantiates a ticker object for downloading market data.
  - `.history(period="max")`: Downloads historical stock prices (Open, High, Low, Close, Volume) into a Pandas DataFrame.
  - `.info`: Returns a Python dictionary containing static company profile metadata (sector, business summary, market cap).
- **Web Scraping (`requests` & `BeautifulSoup`)**:
  - `requests.get(url).text`: Fetches raw HTML page content.
  - `BeautifulSoup(html_data, "html.parser")`: Parses HTML structure into a navigable tree.
  - `find_all('tbody')[1].find_all('tr')`: Traverses HTML table rows (`<tr>`) and cells (`<td>`) to extract quarterly revenue.
  - String Cleansing: `.replace("$", "").replace(",", "")` to parse raw text into clean numerical float values.

### Module 2: Dashboard Visualization (`Plotly`)
- **Plotly Subplots (`make_subplots`)**:
  - Creates dual-panel interactive charts (Row 1: Share Price vs Date, Row 2: Quarterly Revenue vs Date).
  - `go.Scatter()`: Plots time-series stock price line graphs alongside revenue trends.

---

## 💻 Full Final Project Source Code (`Analyzing_Historical_Stock_Data_Dashboard.ipynb`)

```python
import yfinance as yf
import pandas as pd
import requests
from bs4 import BeautifulSoup
import plotly.graph_objects as go
from plotly.subplots import make_subplots

# ----------------------------------------------------
# Question 1: Use yfinance to Extract Tesla Stock Data
# ----------------------------------------------------
tesla = yf.Ticker("TSLA")
tesla_data = tesla.history(period="max")
tesla_data.reset_index(inplace=True)
print(tesla_data.head())

# ----------------------------------------------------
# Question 2: Webscraping to Extract Tesla Revenue Data
# ----------------------------------------------------
url = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/revenue.htm"
html_data = requests.get(url).text
soup = BeautifulSoup(html_data, "html.parser")
tesla_revenue = pd.DataFrame(columns=["Date", "Revenue"])
for row in soup.find_all("tbody")[1].find_all("tr"):
    col = row.find_all("td")
    date = col[0].text
    revenue = col[1].text.replace("$", "").replace(",", "")
    tesla_revenue = pd.concat([tesla_revenue, pd.DataFrame([{"Date": date, "Revenue": revenue}])], ignore_index=True)
tesla_revenue.dropna(inplace=True)
tesla_revenue = tesla_revenue[tesla_revenue['Revenue'] != ""]
print(tesla_revenue.tail())

# ----------------------------------------------------
# Question 3: Use yfinance to Extract GameStop Stock Data
# ----------------------------------------------------
gme = yf.Ticker("GME")
gme_data = gme.history(period="max")
gme_data.reset_index(inplace=True)
print(gme_data.head())

# ----------------------------------------------------
# Question 4: Webscraping to Extract GME Revenue Data
# ----------------------------------------------------
url_gme = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/stock.html"
html_data_gme = requests.get(url_gme).text
soup_gme = BeautifulSoup(html_data_gme, "html.parser")
gme_revenue = pd.DataFrame(columns=["Date", "Revenue"])
for row in soup_gme.find_all("tbody")[1].find_all("tr"):
    col = row.find_all("td")
    date = col[0].text
    revenue = col[1].text.replace("$", "").replace(",", "")
    gme_revenue = pd.concat([gme_revenue, pd.DataFrame([{"Date": date, "Revenue": revenue}])], ignore_index=True)
gme_revenue.dropna(inplace=True)
gme_revenue = gme_revenue[gme_revenue['Revenue'] != ""]
print(gme_revenue.tail())

# ----------------------------------------------------
# Question 5 & 6: Plot Stock & Revenue Dashboards
# ----------------------------------------------------
def make_graph(stock_data, revenue_data, stock):
    fig = make_subplots(rows=2, cols=1, shared_xaxes=True, subplot_titles=("Historical Share Price", "Historical Revenue"), vertical_spacing = .3)
    stock_data_single = stock_data[stock_data.Date <= '2021-06-14']
    revenue_data_single = revenue_data[revenue_data.Date <= '2021-04-30']
    fig.add_trace(go.Scatter(x=pd.to_datetime(stock_data_single.Date, infer_datetime_format=True), y=stock_data_single.Close.astype("float"), name="Share Price"), row=1, col=1)
    fig.add_trace(go.Scatter(x=pd.to_datetime(revenue_data_single.Date, infer_datetime_format=True), y=revenue_data_single.Revenue.astype("float"), name="Revenue"), row=2, col=1)
    fig.update_xaxes(title_text="Date", row=1, col=1)
    fig.update_xaxes(title_text="Date", row=2, col=1)
    fig.update_yaxes(title_text="Price ($US)", row=1, col=1)
    fig.update_yaxes(title_text="Revenue ($US Millions)", row=2, col=1)
    fig.update_layout(showlegend=False, height=900, title=stock, xaxis_rangeslider_visible=True)
    return fig

make_graph(tesla_data, tesla_revenue, 'Tesla')
make_graph(gme_data, gme_revenue, 'GameStop')
```

---

## 📁 Workspace Locations & Remote GitHub Link

- **Local Workspace**: [`C:\Users\Shashank\.vscode\Code\DataScience\Course_5_Python_Project_for_Data_Science`](file:///C:/Users/Shashank/.vscode/Code/DataScience/Course_5_Python_Project_for_Data_Science)
- **GitHub Repository**: [https://github.com/shashankpandya/Python_Project_for_Data_Science-IBM-](https://github.com/shashankpandya/Python_Project_for_Data_Science-IBM-)
- **Jupyter Notebook**: [Analyzing_Historical_Stock_Data_Dashboard.ipynb](https://github.com/shashankpandya/Python_Project_for_Data_Science-IBM-/blob/main/Analyzing_Historical_Stock_Data_Dashboard.ipynb)
