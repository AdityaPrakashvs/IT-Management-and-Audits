# BI Dashboard Builder

A professional Business Intelligence dashboard builder that transforms raw CSV
and Excel data into interactive, visually compelling dashboards with KPI
tracking, dynamic filtering, and configurable chart templates.

---

## What Is Business Intelligence?

Business Intelligence (BI) refers to the strategies, technologies, and
practices used to collect, integrate, analyze, and present business data. The
goal is to support better decision-making by turning raw data into actionable
insights through:

- **Data aggregation** - Consolidating data from multiple sources
- **KPI monitoring** - Tracking key metrics against targets
- **Trend analysis** - Identifying patterns over time
- **Visual analytics** - Presenting data through charts, graphs, and dashboards
- **Self-service reporting** - Empowering non-technical users to explore data

This project provides a lightweight, self-hosted BI tool that makes it easy to
upload a dataset and instantly generate a professional dashboard without
complex ETL pipelines or expensive SaaS subscriptions.

---

## Features

### Data Processing
- Automatic CSV, XLSX, and JSON file loading with encoding detection
- Intelligent column type detection (numeric, categorical, date, text)
- Built-in data cleaning and normalization
- Flexible aggregation engine (sum, mean, median, count, and more)
- Time series resampling and pivot table generation

### KPI Calculation Engine
- 20+ aggregation functions (sum, mean, median, percentiles, std, cv, etc.)
- Period-over-period comparison with trend indicators
- Growth rate, moving average, and year-over-year analysis
- Top/bottom performer identification
- Budget variance analysis with group breakdowns
- Percentile ranking and statistical profiling

### Interactive Charts
- 13 chart types powered by Plotly:
  - Line, Bar, Pie, Donut, Scatter, Area
  - Stacked Bar, Histogram, Box Plot
  - Heatmap, Treemap, Waterfall, Funnel
- Professional color schemes (10 built-in palettes)
- Responsive sizing with fullscreen expansion
- Hover tooltips and zoom/pan controls

### Dashboard UI
- Responsive grid layout (desktop, tablet, mobile)
- Dynamic filter bar with dropdowns and date range pickers
- Sortable, searchable, paginated data preview table
- Real-time AJAX refresh without page reload
- Dashboard configuration export as JSON
- Print-optimized styling

### Template System
- JSON-based dashboard configuration templates
- Pre-built sales and finance dashboard templates
- Auto-generation mode that detects optimal charts from data shape
- Easy to create and share custom templates

---

## Project Structure

```
bi-dashboard-builder/
    app/
        __init__.py              # Package initialization
        main.py                  # Flask application and routes
        data_processor.py        # Data loading, cleaning, aggregation
        chart_generator.py       # Plotly chart generation engine
        kpi_calculator.py        # KPI computation and formatting
        templates/
            base.html            # Base HTML layout with sidebar
            dashboard.html       # Dashboard page with KPIs and charts
            upload.html          # Upload and configuration page
        static/
            css/dashboard.css    # Professional responsive styling
            js/dashboard.js      # Client-side interactivity
    templates/
        sales_dashboard.json     # Sales dashboard template
        finance_dashboard.json   # Finance dashboard template
    data/
        sample_sales.csv         # 1000 sample sales records
        sample_financial.csv     # 36 months of financial data
    tests/
        __init__.py
        test_processor.py        # DataProcessor unit tests
        test_kpi.py              # KPICalculator unit tests
    docs/
        TEMPLATE_GUIDE.md        # Custom template documentation
        KPI_REFERENCE.md         # KPI calculations reference
        DEPLOYMENT.md            # Deployment guide
        screenshots/
            SCREENSHOTS.md       # Screenshot documentation
    requirements.txt             # Python dependencies
    .gitignore                   # Git ignore rules
    LICENSE                      # MIT License
    README.md                    # This file
```

---

## Installation

### Prerequisites

- Python 3.10 or higher
- pip (Python package manager)

### Setup

```bash
# Clone the repository
git clone <repository-url>
cd bi-dashboard-builder

# Create and activate a virtual environment
python -m venv venv

# Windows
venv\Scripts\activate
# macOS / Linux
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

### Running the Application

```bash
# Start the development server
python -m app.main
```

Open your browser to **http://localhost:5000**.

### Running Tests

```bash
pytest tests/ -v
```

---

## Quick Start Guide

1. **Open the application** at `http://localhost:5000`.
2. **Upload a CSV file** using drag-and-drop or the file browser,
   or select a **sample dataset** (sales or financial data).
3. **Choose a template** (Sales Dashboard, Finance Dashboard) or
   click **Auto-Generate Dashboard** to let the engine detect optimal charts.
4. **Explore the dashboard** - interact with charts, apply filters,
   sort the data table, and expand charts to fullscreen.
5. **Export the configuration** using the Export button to save as JSON.

---

## Template Customization

Dashboard templates are JSON files stored in the `templates/` directory.
Each template defines KPI cards, chart types, filters, and layout options.

### Creating a Custom Template

1. Create a new `.json` file in `templates/`.
2. Define the `title`, `kpis`, and `charts` arrays.
3. Map column names to fields in your data.
4. Restart or refresh the application.

### Example Template

```json
{
    "title": "My Custom Dashboard",
    "description": "Tailored analytics for my dataset",
    "kpis": [
        {"label": "Total Sales", "column": "revenue", "function": "sum", "format": "currency"},
        {"label": "Order Count", "column": "order_id", "function": "count", "format": "integer"}
    ],
    "charts": [
        {"type": "line", "title": "Revenue Trend", "x": "date", "y": "revenue", "agg": "sum"},
        {"type": "bar", "title": "Sales by Region", "x": "region", "y": "revenue", "agg": "sum"}
    ]
}
```

For complete documentation, see [docs/TEMPLATE_GUIDE.md](docs/TEMPLATE_GUIDE.md).

---

## API Endpoints

| Endpoint                        | Method | Description                              |
|---------------------------------|--------|------------------------------------------|
| `/`                             | GET    | Upload page                              |
| `/upload`                       | POST   | Handle file upload                       |
| `/configure/<session_id>`       | GET    | Dashboard configuration page             |
| `/dashboard/<session_id>`       | GET/POST | Render dashboard                       |
| `/api/data/<session_id>`        | GET    | Raw data as JSON (with pagination)       |
| `/api/kpis/<session_id>`        | GET    | KPI calculations as JSON                 |
| `/api/chart/<session_id>/<idx>` | GET    | Single chart Plotly JSON                 |
| `/api/refresh/<session_id>`     | POST   | Refresh dashboard with filters           |
| `/api/export/<session_id>`      | GET    | Export dashboard configuration           |

---

## Technology Stack

| Component       | Technology                    |
|-----------------|-------------------------------|
| Backend         | Flask (Python)                |
| Data Processing | pandas, NumPy                 |
| Charting        | Plotly                        |
| Templating      | Jinja2                        |
| Frontend        | Vanilla JavaScript, CSS Grid  |
| Typography      | Inter (Google Fonts)          |
| Icons           | Inline SVG                    |

---

## Configuration

### Environment Variables

| Variable      | Default   | Description                     |
|---------------|-----------|---------------------------------|
| `SECRET_KEY`  | (dev key) | Flask session secret key        |
| `PORT`        | `5000`    | Server port                     |
| `FLASK_DEBUG` | `1`       | Debug mode (`0` for production) |

### File Upload Limits

The maximum upload size is 50 MB by default. Modify `MAX_CONTENT_LENGTH`
in `app/main.py` to change this limit.

---

## Deployment

For production deployment options including Docker, Gunicorn, Nginx reverse
proxy, and cloud platforms (AWS, GCP, Azure), see
[docs/DEPLOYMENT.md](docs/DEPLOYMENT.md).

---

## Documentation

- [Template Customization Guide](docs/TEMPLATE_GUIDE.md)
- [KPI Calculations Reference](docs/KPI_REFERENCE.md)
- [Deployment Guide](docs/DEPLOYMENT.md)
- [Screenshots](docs/screenshots/SCREENSHOTS.md)

---

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
