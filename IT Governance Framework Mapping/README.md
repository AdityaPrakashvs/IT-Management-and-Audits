# IT Governance Framework Mapper

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Python 3.8+](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://python.org)
[![Framework: COBIT 2019](https://img.shields.io/badge/COBIT-2019-green.svg)](#)
[![Framework: ITIL v4](https://img.shields.io/badge/ITIL-v4-green.svg)](#)

An open-source CLI tool that helps organizations map their IT processes to **COBIT 2019** and **ITIL v4** governance frameworks. It performs automated gap analysis, calculates compliance coverage, and generates actionable reports — making governance assessments faster and more consistent.

---

## Problem Statement

Organizations often struggle to align their IT operations with industry governance frameworks. Manual mapping of processes to framework controls is time-consuming, inconsistent, and error-prone. This tool automates the mapping process, identifies coverage gaps, and provides a clear compliance scorecard.

---

## Features

- **Framework Data:** Complete COBIT 2019 (40 objectives across 5 domains) and ITIL v4 (34 practices across 3 categories) reference data
- **Smart Mapping:** Keyword-based matching engine that maps organizational processes to framework controls with confidence scoring
- **Gap Analysis:** Identifies covered vs. uncovered framework areas with coverage percentages
- **Compliance Scorecard:** Visual compliance summary with domain-level breakdown
- **Report Generation:** Terminal tables (via Rich) and standalone HTML reports
- **Sample Data:** Pre-built mapping for a digital payments company

---

## Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/governance-framework-mapper.git
cd governance-framework-mapper

# Create a virtual environment
python -m venv venv
source venv/bin/activate    # Linux/macOS
venv\Scripts\activate       # Windows

# Install dependencies
pip install -r requirements.txt
```

---

## Usage

### Map Organization Processes to Frameworks

```bash
# Map processes from a JSON file against COBIT 2019
python src/cli.py map --input data/sample_org_processes.json --framework cobit

# Map against ITIL v4
python src/cli.py map --input data/sample_org_processes.json --framework itil

# Map against both frameworks
python src/cli.py map --input data/sample_org_processes.json --framework all
```

### Run Gap Analysis

```bash
# Analyze coverage gaps
python src/cli.py analyze --input data/sample_org_processes.json --framework cobit
```

### Generate Reports

```bash
# Terminal report
python src/cli.py report --input data/sample_org_processes.json --format terminal

# HTML report
python src/cli.py report --input data/sample_org_processes.json --format html --output reports/compliance_report.html
```

### Sample Output

```
┌─────────────────────────────────────────────────────────────────┐
│                  COBIT 2019 Compliance Scorecard                │
├──────────────────────┬──────────┬──────────┬───────────────────┤
│ Domain               │ Covered  │ Total    │ Coverage          │
├──────────────────────┼──────────┼──────────┼───────────────────┤
│ EDM - Governance     │ 3        │ 5        │ ██████░░░░ 60.0%  │
│ APO - Management     │ 9        │ 14       │ ██████░░░░ 64.3%  │
│ BAI - Build/Acquire  │ 7        │ 10       │ ███████░░░ 70.0%  │
│ DSS - Deliver        │ 5        │ 6        │ ████████░░ 83.3%  │
│ MEA - Monitor        │ 2        │ 5        │ ████░░░░░░ 40.0%  │
├──────────────────────┼──────────┼──────────┼───────────────────┤
│ TOTAL                │ 26       │ 40       │ ██████░░░░ 65.0%  │
└──────────────────────┴──────────┴──────────┴───────────────────┘
```

---

## Project Structure

```
governance-framework-mapper/
├── README.md
├── LICENSE
├── requirements.txt
├── setup.py
├── .gitignore
├── src/
│   ├── frameworks/
│   │   ├── cobit.json          # COBIT 2019 objectives
│   │   └── itil.json           # ITIL v4 practices
│   ├── mapper.py               # Mapping engine
│   ├── analyzer.py             # Gap analysis
│   ├── reporter.py             # Report generation
│   └── cli.py                  # CLI interface
├── data/
│   └── sample_org_processes.json
├── templates/
│   └── report.html             # HTML report template
├── tests/
│   ├── test_mapper.py
│   └── test_analyzer.py
└── docs/
    ├── COBIT_REFERENCE.md
    ├── ITIL_REFERENCE.md
    ├── USAGE_GUIDE.md
    └── screenshots/
        └── SCREENSHOTS.md
```

---

## Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Commit changes: `git commit -m "Add your feature"`
4. Push: `git push origin feature/your-feature`
5. Open a Pull Request

---

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
