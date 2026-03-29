# Financial Data Visualizer

A professional-grade Python toolkit for analyzing and visualizing financial data. Generate publication-ready charts, interactive dashboards, and statistical reports from transaction records, market data, and revenue/expense datasets.

---

## Features

- **Multi-format Data Ingestion**: Load and validate CSV and JSON financial datasets with comprehensive error handling and type coercion.
- **Statistical Analysis Engine**: Compute moving averages (7/30/90-day), year-over-year growth rates, variance, percentile distributions, and full correlation matrices.
- **Six Visualization Types**:
  - **Line Charts** -- Revenue trends, stock price histories, and moving average overlays
  - **Bar Charts** -- Monthly comparisons, category breakdowns, and top-N analysis
  - **Pie Charts** -- Expense distribution and revenue segmentation
  - **Heatmaps** -- Correlation matrices and transaction volume by hour/day-of-week
  - **Scatter Plots** -- Risk vs. return analysis and statistical outlier detection
  - **Candlestick Charts** -- OHLC market data with volume overlay
- **Interactive HTML Dashboard**: Auto-generated Plotly-based dashboard combining multiple visualizations into a single navigable report.
- **Command-Line Interface**: Streamlined CLI powered by Click for quick analysis, chart generation, and dashboard creation.

---

## Chart Descriptions

| Chart Type | Purpose | Best For |
|---|---|---|
| Revenue Trend Line | Tracks revenue over time with optional moving average bands | Identifying growth patterns and seasonal cycles |
| Stock Price Line | Displays closing prices with 7/30/90-day moving averages | Technical analysis and trend identification |
| Monthly Comparison Bars | Side-by-side monthly totals for selected metrics | Period-over-period performance comparison |
| Category Breakdown Bars | Horizontal bars showing spending or revenue by category | Budget analysis and cost center review |
| Top-N Analysis Bars | Ranked bar chart of highest-value items | Identifying key revenue drivers or expense sinks |
| Expense Distribution Pie | Proportional breakdown of expenses by category | Quick budget allocation overview |
| Revenue Segment Pie | Revenue share across business segments or product lines | Portfolio composition analysis |
| Correlation Heatmap | Color-coded matrix of variable correlations | Identifying relationships between financial metrics |
| Volume Heatmap | Transaction density by hour and day of week | Operational pattern discovery |
| Risk vs. Return Scatter | Plots expected return against volatility | Portfolio optimization and risk assessment |
| Outlier Detection Scatter | Highlights statistical outliers in transaction data | Fraud detection and anomaly investigation |
| OHLC Candlestick | Open-High-Low-Close bars with volume subplot | Market analysis and trading decisions |

---

## Installation

```bash
# Clone the repository
git clone <repository-url>
cd financial-data-visualizer

# Create a virtual environment (recommended)
python -m venv venv
source venv/bin/activate   # Linux/macOS
venv\Scripts\activate      # Windows

# Install dependencies
pip install -r requirements.txt
```

### Dependencies

- Python 3.9+
- pandas >= 2.0.0
- matplotlib >= 3.7.0
- seaborn >= 0.12.0
- plotly >= 5.15.0
- click >= 8.1.0
- numpy >= 1.24.0

---

## Dataset Format

The project ships with three sample datasets in `data/`. You can replace them with your own data as long as the column structure matches.

### transactions.csv

| Column | Type | Description |
|---|---|---|
| date | YYYY-MM-DD | Transaction date |
| amount | float | Transaction amount (positive = income, negative = expense) |
| category | string | Expense/income category |
| merchant | string | Vendor or source name |
| type | string | "credit" or "debit" |

### market_data.csv

| Column | Type | Description |
|---|---|---|
| date | YYYY-MM-DD | Trading date |
| open | float | Opening price |
| high | float | Intraday high |
| low | float | Intraday low |
| close | float | Closing price |
| volume | int | Shares traded |

### revenue_expenses.csv

| Column | Type | Description |
|---|---|---|
| date | YYYY-MM-DD | First day of month |
| revenue | float | Monthly revenue |
| cogs | float | Cost of goods sold |
| operating_expenses | float | Operating expenses |
| marketing | float | Marketing spend |
| salaries | float | Salary expenses |
| rent | float | Rent and facilities |
| utilities | float | Utility costs |
| net_income | float | Bottom-line profit/loss |

---

## Usage Examples

### Command-Line Interface

```bash
# Run a statistical summary on transaction data
python -m src.cli analyze data/transactions.csv

# Generate a specific chart type
python -m src.cli chart data/transactions.csv --type bar --output charts/monthly.png

# Generate line chart with moving averages
python -m src.cli chart data/market_data.csv --type line --output charts/stock.png

# Build an interactive HTML dashboard
python -m src.cli dashboard data/ --output reports/dashboard.html

# Generate candlestick chart from market data
python -m src.cli chart data/market_data.csv --type candlestick --output charts/ohlc.png
```

### Python API

```python
from src.data_loader import DataLoader
from src.statistics import FinancialStatistics
from src.charts.line_charts import LineChartGenerator
from src.dashboard import DashboardBuilder

# Load data
loader = DataLoader()
df = loader.load_csv("data/transactions.csv")

# Compute statistics
stats = FinancialStatistics(df, date_col="date", value_col="amount")
summary = stats.summary()
ma_30 = stats.moving_average(window=30)

# Generate a chart
line_gen = LineChartGenerator()
line_gen.plot_trend(df, x="date", y="amount", title="Transaction Amounts Over Time")

# Build dashboard
builder = DashboardBuilder(data_dir="data/")
builder.build("reports/dashboard.html")
```

### Running Tests

```bash
# Run all tests
python -m pytest tests/ -v

# Run specific test module
python -m pytest tests/test_statistics.py -v
```

---

## Project Structure

```
financial-data-visualizer/
├── README.md
├── LICENSE
├── requirements.txt
├── .gitignore
├── data/
│   ├── transactions.csv
│   ├── market_data.csv
│   └── revenue_expenses.csv
├── src/
│   ├── __init__.py
│   ├── data_loader.py
│   ├── statistics.py
│   ├── dashboard.py
│   ├── cli.py
│   └── charts/
│       ├── __init__.py
│       ├── line_charts.py
│       ├── bar_charts.py
│       ├── pie_charts.py
│       ├── heatmaps.py
│       ├── scatter_plots.py
│       └── candlestick.py
├── tests/
│   ├── __init__.py
│   ├── test_statistics.py
│   └── test_charts.py
└── docs/
    ├── CHART_GALLERY.md
    ├── DATA_FORMAT.md
    ├── DASHBOARD_GUIDE.md
    └── screenshots/
        └── SCREENSHOTS.md
```

---

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-chart-type`)
3. Commit your changes (`git commit -m "Add waterfall chart support"`)
4. Push to the branch (`git push origin feature/new-chart-type`)
5. Open a Pull Request

Please ensure all tests pass before submitting a PR.

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
