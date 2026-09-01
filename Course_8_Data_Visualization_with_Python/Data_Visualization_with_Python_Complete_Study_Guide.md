# Course 8: Data Visualization with Python — Complete Study Guide
**IBM Data Science Professional Certificate — Course 8 of 12**
**Learner**: Pandya Shashank (Pandya Nareshbhai)
**Status**: Completed & Certified — Passed (Final Exam 100%, AI Graded Final Project Passed)

---

## Executive Summary & Course Overview

**Data Visualization with Python** teaches the full spectrum of modern Python visualization techniques — from foundational static charts through advanced interactive dashboards — enabling data scientists to communicate complex insights clearly to both technical and non-technical audiences.

**Primary Libraries Covered**: Matplotlib · Seaborn · Folium · Plotly · Plotly Express · Dash (Plotly)

---

## Module 1: Introduction to Data Visualization

### Why Data Visualization Matters
- Humans process visual information 60,000x faster than raw text.
- Visualization reveals patterns, outliers, correlations, and trends invisible in tabular data.
- Key frameworks for storytelling: **CRISP-DM**, **EDA → Model → Communicate**.

### Best Practices in Data Visualization
1. **Clarity**: Use appropriate chart type for the data and question.
2. **Accuracy**: Avoid distorted scales or misleading 3D effects.
3. **Aesthetics**: Balance color palettes, fonts, and whitespace.
4. **Context**: Always label axes, provide titles, and add a legend.
5. **Accessibility**: Use colorblind-safe palettes (e.g., `viridis`, `cividis`).

---

## Module 2: Matplotlib — The Foundation

Matplotlib is Python's most fundamental 2D plotting library. It provides granular, low-level control over every aspect of a figure.

### Anatomy of a Matplotlib Figure
```python
import matplotlib.pyplot as plt
import numpy as np

# Figure → Axes → Artists (Lines, Patches, Text)
fig, ax = plt.subplots(figsize=(10, 6))    # Figure with a single Axes
ax.set_title("Figure Title", fontsize=16)
ax.set_xlabel("X-Axis Label", fontsize=13)
ax.set_ylabel("Y-Axis Label", fontsize=13)
ax.tick_params(axis='both', labelsize=11)
plt.tight_layout()
plt.show()
```

### 2.1 Line Plot
```python
import matplotlib.pyplot as plt

years  = [2015, 2016, 2017, 2018, 2019, 2020]
values = [120,  145,  162,  174,  190,  210]

fig, ax = plt.subplots(figsize=(10, 5))
ax.plot(years, values,
        color='royalblue',
        linewidth=2.5,
        marker='o',          # circle markers at data points
        markersize=8,
        linestyle='--',      # dashed line
        label='Annual Revenue')
ax.legend(fontsize=12)
ax.set_title("Annual Revenue Growth (2015–2020)", fontsize=15, fontweight='bold')
ax.set_xlabel("Year")
ax.set_ylabel("Revenue (Million USD)")
ax.grid(True, linestyle=':', alpha=0.7)
plt.show()
```

### 2.2 Area Plot (Stacked)
```python
import pandas as pd
import matplotlib.pyplot as plt

# Area plot directly from a Pandas DataFrame
df = pd.DataFrame({
    'Year':  [2015, 2016, 2017, 2018, 2019],
    'Asia':  [200, 240, 310, 360, 420],
    'Europe':[150, 180, 200, 220, 270],
    'Africa':[80,  95,  110, 130, 155]
}).set_index('Year')

df.plot(kind='area',
        stacked=True,
        alpha=0.6,
        figsize=(10, 6),
        colormap='tab10')
plt.title("Immigration by Region (Stacked Area)")
plt.ylabel("Number of Immigrants")
plt.grid(axis='y', linestyle='--', alpha=0.5)
plt.show()
```

### 2.3 Histogram — Frequency Distribution
```python
import numpy as np
import matplotlib.pyplot as plt

data = np.random.normal(loc=50, scale=15, size=1000)   # 1000 samples, μ=50, σ=15

fig, ax = plt.subplots(figsize=(9, 5))
n, bins, patches = ax.hist(data,
                            bins=30,
                            color='steelblue',
                            edgecolor='white',
                            alpha=0.85)
ax.set_title("Distribution of Values", fontsize=14)
ax.set_xlabel("Value")
ax.set_ylabel("Frequency")
ax.axvline(data.mean(), color='red', linestyle='--', linewidth=2, label=f'Mean = {data.mean():.1f}')
ax.legend()
plt.show()
```

### 2.4 Bar Chart (Vertical & Horizontal)
```python
import matplotlib.pyplot as plt

categories = ['Python', 'R', 'SQL', 'Java', 'Scala']
counts     = [85, 60, 78, 45, 30]

# --- Vertical Bar Chart ---
fig, axes = plt.subplots(1, 2, figsize=(14, 5))
axes[0].bar(categories, counts, color='coral', edgecolor='black', width=0.6)
axes[0].set_title("Vertical Bar Chart")
axes[0].set_xlabel("Language")
axes[0].set_ylabel("Popularity Score")

# --- Horizontal Bar Chart ---
axes[1].barh(categories, counts, color='mediumseagreen', edgecolor='black', height=0.6)
axes[1].set_title("Horizontal Bar Chart")
axes[1].set_xlabel("Popularity Score")

plt.tight_layout()
plt.show()
```

### 2.5 Pie Chart
```python
import matplotlib.pyplot as plt

labels  = ['Canada', 'India', 'China', 'UK', 'Others']
sizes   = [35, 25, 20, 12, 8]
explode = (0.1, 0, 0, 0, 0)   # Emphasize first slice

fig, ax = plt.subplots(figsize=(7, 7))
ax.pie(sizes,
       labels=labels,
       explode=explode,
       autopct='%1.1f%%',      # Show percentage per slice
       shadow=True,
       startangle=140,
       colors=plt.cm.Set3.colors)
ax.set_title("Immigration Share by Country", fontsize=14, fontweight='bold')
plt.show()
```

### 2.6 Box Plot (Box-and-Whisker)
```python
import matplotlib.pyplot as plt
import numpy as np

# Generate synthetic salary data for 4 departments
data = [np.random.normal(loc, scale, 100)
        for loc, scale in [(55000, 8000), (72000, 12000), (85000, 15000), (45000, 6000)]]
labels = ['Marketing', 'Engineering', 'Management', 'Support']

fig, ax = plt.subplots(figsize=(9, 6))
bp = ax.boxplot(data, labels=labels, patch_artist=True,
                medianprops=dict(color='black', linewidth=2))
colors = ['lightblue', 'lightgreen', 'lightsalmon', 'lightyellow']
for patch, color in zip(bp['boxes'], colors):
    patch.set_facecolor(color)
ax.set_title("Salary Distribution by Department", fontsize=14)
ax.set_ylabel("Annual Salary (USD)")
ax.grid(axis='y', linestyle='--', alpha=0.7)
plt.show()
```

### 2.7 Scatter Plot
```python
import matplotlib.pyplot as plt
import numpy as np

np.random.seed(42)
x = np.random.rand(100) * 100
y = 2.5 * x + np.random.randn(100) * 15

fig, ax = plt.subplots(figsize=(8, 6))
scatter = ax.scatter(x, y,
                     c=y,               # Color mapped to y value
                     cmap='viridis',
                     s=60,              # Marker size
                     alpha=0.8,
                     edgecolors='white',
                     linewidths=0.5)
plt.colorbar(scatter, ax=ax, label='Y Value')
ax.set_title("Scatter Plot with Color Mapping", fontsize=14)
ax.set_xlabel("Feature X")
ax.set_ylabel("Target Y")
plt.show()
```

### 2.8 Subplots Grid
```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 2 * np.pi, 200)

fig, axes = plt.subplots(2, 2, figsize=(12, 8))
fig.suptitle("Trigonometric Functions", fontsize=16, fontweight='bold')

axes[0, 0].plot(x, np.sin(x), color='royalblue'); axes[0, 0].set_title("sin(x)")
axes[0, 1].plot(x, np.cos(x), color='tomato');    axes[0, 1].set_title("cos(x)")
axes[1, 0].plot(x, np.tan(x), color='green');     axes[1, 0].set_title("tan(x)"); axes[1, 0].set_ylim(-5, 5)
axes[1, 1].plot(x, np.sin(x) * np.cos(x), color='purple'); axes[1, 1].set_title("sin(x)·cos(x)")

for ax in axes.flat:
    ax.grid(True, linestyle=':', alpha=0.6)
    ax.set_xlabel("x")

plt.tight_layout()
plt.show()
```

---

## Module 3: Seaborn — Statistical Visualization

Seaborn builds on Matplotlib with a higher-level API for statistical graphics, beautiful default aesthetics, and built-in theme support.

### Seaborn Themes & Context
```python
import seaborn as sns
import matplotlib.pyplot as plt

# Themes: darkgrid, whitegrid, dark, white, ticks
sns.set_theme(style='whitegrid', palette='muted', font_scale=1.2)
# Context: paper, notebook, talk, poster (scales plot elements)
sns.set_context("notebook")
```

### 3.1 Seaborn lineplot & scatterplot
```python
import seaborn as sns
import matplotlib.pyplot as plt

tips = sns.load_dataset('tips')

# Scatter plot with regression line
fig, axes = plt.subplots(1, 2, figsize=(13, 5))
sns.scatterplot(data=tips, x='total_bill', y='tip', hue='sex', size='size',
                palette='deep', sizes=(30, 150), alpha=0.7, ax=axes[0])
axes[0].set_title("Tip vs Bill Amount")

sns.regplot(data=tips, x='total_bill', y='tip', scatter_kws={'alpha':0.4},
            line_kws={'color':'red', 'linewidth':2.5}, ax=axes[1])
axes[1].set_title("Linear Regression Fit")

plt.tight_layout(); plt.show()
```

### 3.2 Distribution Plots
```python
import seaborn as sns
import matplotlib.pyplot as plt

tips = sns.load_dataset('tips')

fig, axes = plt.subplots(1, 3, figsize=(16, 5))

# Histogram with KDE
sns.histplot(tips['total_bill'], kde=True, bins=25, color='steelblue', ax=axes[0])
axes[0].set_title("Histogram + KDE")

# KDE Plot only
sns.kdeplot(data=tips, x='total_bill', hue='time', fill=True, alpha=0.5, ax=axes[1])
axes[1].set_title("KDE by Meal Time")

# ECDF (Empirical Cumulative Distribution)
sns.ecdfplot(data=tips, x='total_bill', hue='sex', ax=axes[2])
axes[2].set_title("ECDF by Gender")

plt.tight_layout(); plt.show()
```

### 3.3 Categorical Plots
```python
import seaborn as sns
import matplotlib.pyplot as plt

tips = sns.load_dataset('tips')
fig, axes = plt.subplots(1, 3, figsize=(16, 5))

# Box plot
sns.boxplot(data=tips, x='day', y='total_bill', hue='sex', palette='pastel', ax=axes[0])
axes[0].set_title("Box Plot: Bill by Day")

# Violin plot (combines box + KDE)
sns.violinplot(data=tips, x='day', y='total_bill', hue='sex', split=True,
               palette='muted', inner='quart', ax=axes[1])
axes[1].set_title("Violin Plot")

# Strip plot (raw data points)
sns.stripplot(data=tips, x='day', y='total_bill', jitter=True,
              hue='sex', dodge=True, palette='Set2', ax=axes[2])
axes[2].set_title("Strip Plot")

plt.tight_layout(); plt.show()
```

### 3.4 Heatmap — Correlation Matrix
```python
import seaborn as sns
import matplotlib.pyplot as plt

tips = sns.load_dataset('tips')
numeric_tips = tips.select_dtypes(include='number')
correlation = numeric_tips.corr()

fig, ax = plt.subplots(figsize=(7, 5))
sns.heatmap(correlation,
            annot=True,      # Show coefficient values
            fmt='.2f',       # 2 decimal places
            cmap='coolwarm', # Diverging color scale
            center=0,
            square=True,
            linewidths=0.5,
            cbar_kws={'shrink': 0.8},
            ax=ax)
ax.set_title("Pearson Correlation Heatmap", fontsize=14, fontweight='bold')
plt.tight_layout(); plt.show()
```

### 3.5 Pair Plot — Multi-variable Exploration
```python
import seaborn as sns

iris = sns.load_dataset('iris')
# Scatter matrix of all numeric columns colored by species
pair_grid = sns.pairplot(iris,
                         hue='species',
                         diag_kind='kde',      # Diagonal shows KDE
                         plot_kws={'alpha': 0.6},
                         palette='Set1')
pair_grid.fig.suptitle("Iris Dataset Pair Plot", y=1.02, fontsize=15)
```

### 3.6 FacetGrid — Conditional Multi-panel Plots
```python
import seaborn as sns
import matplotlib.pyplot as plt

tips = sns.load_dataset('tips')

g = sns.FacetGrid(tips, col='time', row='sex', height=4, aspect=1.2, palette='muted')
g.map_dataframe(sns.histplot, x='total_bill', kde=True, bins=15)
g.set_axis_labels("Total Bill ($)", "Count")
g.add_legend()
g.fig.suptitle("Bill Distribution by Meal & Gender", y=1.02, fontsize=13)
plt.show()
```

---

## Module 4: Folium — Geospatial Maps

Folium wraps the Leaflet.js mapping library for Python, enabling interactive choropleth maps, marker clusters, and geospatial overlays.

### 4.1 Basic Map Creation
```python
import folium

# Create base map centered on Canada
canada_map = folium.Map(location=[56.1304, -106.3468],
                        zoom_start=4,
                        tiles='OpenStreetMap')    # or 'CartoDB positron', 'Stamen Terrain'

# Add a marker
folium.Marker(
    location=[43.70, -79.42],
    popup="<b>Toronto, Canada</b>",
    tooltip="Click me",
    icon=folium.Icon(color='blue', icon='info-sign')
).add_to(canada_map)

canada_map.save('canada_map.html')  # Save as interactive HTML file
canada_map   # Renders inline in Jupyter Notebook
```

### 4.2 Circle Markers & Feature Groups
```python
import folium
import pandas as pd

world_map = folium.Map(location=[20, 0], zoom_start=2)

feature_group = folium.FeatureGroup(name='Immigration Hotspots')

cities = [
    {"location": [28.61, 77.20], "popup": "New Delhi", "color": "red"},
    {"location": [31.22, 121.47], "popup": "Shanghai", "color": "blue"},
    {"location": [5.56, -0.20], "popup": "Accra", "color": "green"},
]

for city in cities:
    folium.CircleMarker(
        location=city["location"],
        radius=15,
        color=city["color"],
        fill=True,
        fill_color=city["color"],
        fill_opacity=0.6,
        popup=city["popup"]
    ).add_to(feature_group)

feature_group.add_to(world_map)
folium.LayerControl().add_to(world_map)  # Toggle layers on/off
world_map
```

### 4.3 Choropleth Map
```python
import folium
import pandas as pd
import json

# Choropleth maps color geographic boundaries (countries, states) by a data value
world_geo = r'world_countries.json'   # GeoJSON file with country boundary polygons
df = pd.read_csv('immigration_data.csv')

choropleth_map = folium.Map(location=[0, 0], zoom_start=2)

folium.Choropleth(
    geo_data=world_geo,
    data=df,
    columns=['Country', 'Total_Immigration'],   # Linking column + value column
    key_on='feature.properties.name',            # GeoJSON property matching country name
    fill_color='YlOrRd',    # Yellow → Orange → Red palette
    fill_opacity=0.7,
    line_opacity=0.2,
    legend_name='Total Immigration to Canada (1980-2013)',
    nan_fill_color='white'
).add_to(choropleth_map)

choropleth_map
```

---

## Module 5: Plotly & Plotly Express — Interactive Charts

Plotly produces fully interactive, web-ready charts with hover tooltips, zooming, and panning — ideal for dashboards and reporting.

### 5.1 Plotly Express — Quick Interactive Charts
```python
import plotly.express as px
import pandas as pd

# Sample Gapminder dataset (built-in)
df = px.data.gapminder()

# --- Animated Bubble Chart ---
fig = px.scatter(df.query("year==2007"),
                 x="gdpPercap",
                 y="lifeExp",
                 size="pop",
                 color="continent",
                 hover_name="country",
                 log_x=True,
                 size_max=60,
                 title="GDP vs Life Expectancy (2007)")
fig.update_layout(template='plotly_white', font_size=13)
fig.show()
```

### 5.2 Plotly Express — Line, Bar, Histogram, Box
```python
import plotly.express as px

df = px.data.gapminder()

# Line chart with animation
fig_line = px.line(df[df['country'].isin(['Canada', 'India', 'China', 'Brazil'])],
                   x='year', y='gdpPercap', color='country',
                   title='GDP Per Capita Over Time',
                   markers=True)
fig_line.show()

# Bar chart
df_2007 = df[df['year'] == 2007]
fig_bar = px.bar(df_2007.nlargest(10, 'pop'),
                 x='country', y='pop',
                 color='continent',
                 title='Top 10 Countries by Population (2007)',
                 labels={'pop': 'Population'})
fig_bar.update_layout(xaxis_tickangle=-30)
fig_bar.show()

# Histogram
fig_hist = px.histogram(df_2007, x='lifeExp', nbins=30, color='continent',
                        barmode='overlay', title='Life Expectancy Distribution (2007)',
                        opacity=0.7)
fig_hist.show()
```

### 5.3 Plotly Graph Objects — Fine-Grained Control
```python
import plotly.graph_objects as go
import pandas as pd

# Candlestick chart for financial data
fig = go.Figure()
fig.add_trace(go.Candlestick(
    x=['2024-01-01', '2024-01-02', '2024-01-03', '2024-01-04'],
    open=[100, 105, 102, 108],
    high=[110, 112, 109, 115],
    low=[98,  103, 100, 106],
    close=[105, 102, 108, 113],
    name='Stock Price'
))
fig.update_layout(title='Candlestick Chart', xaxis_rangeslider_visible=False)
fig.show()
```

### 5.4 Subplots in Plotly
```python
from plotly.subplots import make_subplots
import plotly.graph_objects as go
import numpy as np

x = np.linspace(0, 10, 200)

fig = make_subplots(rows=2, cols=2,
                    subplot_titles=['Sin(x)', 'Cos(x)', 'Sin²(x)', 'Combined'],
                    shared_xaxes=True)

fig.add_trace(go.Scatter(x=x, y=np.sin(x), name='sin', line=dict(color='royalblue')),    row=1, col=1)
fig.add_trace(go.Scatter(x=x, y=np.cos(x), name='cos', line=dict(color='tomato')),       row=1, col=2)
fig.add_trace(go.Scatter(x=x, y=np.sin(x)**2, name='sin²', line=dict(color='green')),   row=2, col=1)
fig.add_trace(go.Scatter(x=x, y=np.sin(x)+np.cos(x), name='sin+cos', line=dict(color='purple')), row=2, col=2)

fig.update_layout(height=600, title_text='Trigonometric Functions Dashboard', showlegend=True)
fig.show()
```

---

## Module 6: Dash — Interactive Web Dashboards

Dash is a Python framework (built on top of Plotly, Flask, and React.js) for building production-ready, interactive analytical web applications with zero JavaScript.

### Dash Architecture
```
User Browser ↔ Dash Server (Flask) ↔ Python Callbacks ↔ Plotly Charts
                     ↕
               Dash Components (HTML / dcc)
```

### 6.1 Minimal Dash App Structure
```python
# Install: pip install dash
import dash
from dash import dcc, html
from dash.dependencies import Input, Output
import plotly.express as px
import pandas as pd

# Initialize the Dash application
app = dash.Dash(__name__)

# Sample DataFrame
df = px.data.gapminder()

# Layout: Defines the UI components
app.layout = html.Div(children=[
    html.H1("IBM Data Visualization Dashboard",
            style={'textAlign': 'center', 'color': '#003366', 'fontFamily': 'Arial'}),

    html.Div([
        html.Label("Select Continent:"),
        dcc.Dropdown(
            id='continent-dropdown',
            options=[{'label': c, 'value': c} for c in df['continent'].unique()],
            value='Asia',
            clearable=False,
            style={'width': '300px'}
        ),
    ], style={'margin': '20px'}),

    dcc.Graph(id='life-exp-graph'),   # Placeholder — populated by callback
])

# Callback: Links Dropdown input → Graph output
@app.callback(
    Output(component_id='life-exp-graph', component_property='figure'),
    Input(component_id='continent-dropdown', component_property='value')
)
def update_graph(selected_continent):
    filtered_df = df[df['continent'] == selected_continent]
    fig = px.line(filtered_df,
                  x='year', y='lifeExp',
                  color='country',
                  title=f'Life Expectancy Over Time — {selected_continent}',
                  markers=True)
    fig.update_layout(template='plotly_white')
    return fig

if __name__ == '__main__':
    app.run(debug=True, port=8050)
# Open browser → http://127.0.0.1:8050/
```

### 6.2 Dash Core Components (dcc) Reference
| Component | Usage |
|:---|:---|
| `dcc.Graph(id='...')` | Renders any Plotly figure interactively |
| `dcc.Dropdown(id='...', options=[...], value='...')` | Select from a list |
| `dcc.Slider(id='...', min=, max=, value=, marks={})` | Range slider input |
| `dcc.RangeSlider(id='...', min=, max=, value=[a,b])` | Dual-handle slider |
| `dcc.Input(id='...', type='number', value=0)` | Text/numeric input box |
| `dcc.Checklist(id='...', options=[...], value=[...])` | Multi-select checkboxes |
| `dcc.RadioItems(id='...', options=[...], value='...')` | Single-select radio buttons |
| `dcc.DatePickerRange(id='...')` | Date range calendar picker |

### 6.3 Dash HTML Components (html) Reference
| Component | HTML Equivalent |
|:---|:---|
| `html.Div(children=[...], style={})` | `<div>` — container block |
| `html.H1('text')` – `html.H6('text')` | `<h1>` – `<h6>` headings |
| `html.P('text', style={})` | `<p>` paragraph |
| `html.Button('Label', id='btn')` | `<button>` clickable button |
| `html.Br()` | `<br>` line break |
| `html.Hr()` | `<hr>` horizontal rule |
| `html.Img(src='url', style={})` | `<img>` embedded image |
| `html.A('Link Text', href='url')` | `<a>` hyperlink |

### 6.4 Callback Patterns

#### Pattern 1 — Multiple Inputs → Single Output
```python
from dash import Input, Output

@app.callback(
    Output('output-graph', 'figure'),
    [Input('dropdown-1', 'value'),
     Input('slider-1', 'value'),
     Input('checklist-1', 'value')]
)
def update(dropdown_val, slider_val, checklist_vals):
    # Filter/transform df using all three inputs
    filtered = df[(df['category'] == dropdown_val) &
                  (df['score'] >= slider_val) &
                  (df['region'].isin(checklist_vals))]
    return px.bar(filtered, x='region', y='score')
```

#### Pattern 2 — Multiple Outputs from Single Input
```python
@app.callback(
    [Output('bar-chart', 'figure'),
     Output('summary-text', 'children')],
    Input('year-dropdown', 'value')
)
def update_multi(year):
    filtered = df[df['year'] == year]
    fig = px.bar(filtered, x='country', y='pop', color='continent')
    summary = f"Showing {len(filtered)} countries for year {year}."
    return fig, summary
```

#### Pattern 3 — Button Click Event
```python
from dash.dependencies import Input, Output, State

@app.callback(
    Output('output-text', 'children'),
    Input('submit-btn', 'n_clicks'),
    State('input-box', 'value'),    # State reads current value without triggering
    prevent_initial_call=True
)
def on_button_click(n_clicks, input_value):
    if n_clicks > 0:
        return f"You submitted: {input_value}"
```

---

## Module 7: Final Project — Automobile Sales Dashboard

### Project Summary
Built a two-tab interactive Dash dashboard analyzing historical automobile sales data during recession periods vs. non-recession years.

**Report 1 — Yearly Statistics**:
- Line chart: Yearly automobile sales for all periods.
- Line chart: Monthly total automobile sales.
- Bar chart: Average vehicles sold by vehicle type per year.
- Pie chart: Total advertising expenditure by vehicle type.

**Report 2 — Recession Period Statistics**:
- Line chart: Average automobile sales during recession periods.
- Bar chart: Average vehicles sold by vehicle type during recessions.
- Pie chart: Advertising expenditure share by vehicle type during recessions.
- Bar chart: Unemployment rate effect on vehicle type & sales during recessions.

### Key Dash Patterns Used in Final Project
```python
# Tab-based layout pattern used in the final project
from dash import dcc, html

app.layout = html.Div([
    html.H1("Automobile Sales Dashboard"),

    dcc.Tabs(id='tabs', value='tab1', children=[
        dcc.Tab(label='Yearly Report',     value='tab1'),
        dcc.Tab(label='Recession Report',  value='tab2'),
    ]),

    html.Div([
        dcc.Dropdown(id='year-dropdown',
                     options=[{'label': y, 'value': y} for y in range(1980, 2014)],
                     placeholder='Select Year'),
    ], id='year-dropdown-div'),   # Hidden/shown via callback based on tab

    html.Div(id='output-container')   # Populated by callback
])

# Tab-aware callback
@app.callback(
    Output('output-container', 'children'),
    Output('year-dropdown-div', 'style'),
    Input('tabs', 'value'),
    Input('year-dropdown', 'value')
)
def render_content(selected_tab, selected_year):
    if selected_tab == 'tab2':
        return recession_charts(), {'display': 'none'}  # Hide year selector
    else:
        return yearly_charts(selected_year), {'display': 'block'}
```

---

## Assessment & Grade Summary

| Assessment | Weight | Score | Status |
|:---|:---:|:---:|:---:|
| Graded Quiz: Introduction to Data Visualization Tools | 5% | 100% | Passed |
| Graded Quiz: Basic and Specialized Visualization Tools | 5% | 100% | Passed |
| Graded Quiz: Advanced Visualization Tools | 5% | 100% | Passed |
| Graded Quiz: Creating Dashboards with Plotly and Dash | 5% | 100% | Passed |
| AI Graded — Final Project (Automobile Sales Dashboard) | 40% | Passed | Passed |
| Final Exam | 40% | 100% | Passed |
| **Overall Course Grade** | **100%** | **~100%** | **PASSED** |

---

## Quick Reference Cheat Sheet

### Choosing the Right Chart
| Data Type & Question | Best Chart Type |
|:---|:---|
| Trend over time (continuous) | Line Plot |
| Trend over time (parts-of-whole) | Stacked Area Plot |
| Compare quantities (few categories) | Bar / Column Chart |
| Distribution shape | Histogram / KDE |
| Outliers, quartiles, spread | Box Plot |
| Density + distribution | Violin Plot |
| Relationship between 2 variables | Scatter Plot |
| Composition / proportions | Pie / Donut Chart |
| Geographic data | Folium Choropleth Map |
| Multi-variable correlation | Heatmap (Seaborn) |
| Many-variable exploration | Pair Plot (Seaborn) |
| Interactive user-driven analysis | Plotly / Dash |

### Key Matplotlib Customization Parameters
| Parameter | Effect |
|:---|:---|
| `color='royalblue'` | Line or fill color (name, hex, or RGB) |
| `linewidth=2.5` | Stroke width |
| `linestyle='--'` | Line style: `-`, `--`, `-.`, `:` |
| `marker='o'` | Data point marker shape |
| `alpha=0.7` | Transparency (0=invisible, 1=opaque) |
| `figsize=(12, 6)` | Figure dimensions in inches |
| `fontsize=13` | Text size for labels / titles |
| `grid(True)` | Show gridlines |
| `tight_layout()` | Prevent clipping between subplots |

### Key Seaborn `set_theme` Styles
| Style | Description |
|:---|:---|
| `darkgrid` | Dark background + gridlines |
| `whitegrid` | White background + gridlines (default best) |
| `dark` | Dark background, no grid |
| `white` | White background, no grid |
| `ticks` | White background + tick marks |

---
*Generated and saved: `Course_8_Data_Visualization_with_Python/Data_Visualization_with_Python_Complete_Study_Guide.md`*
*IBM Data Science Professional Certificate — Pandya Shashank*
