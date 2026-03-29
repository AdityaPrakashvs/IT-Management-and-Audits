# IT Audit Toolkit

A comprehensive, professional-grade command-line toolkit for conducting Information Technology audits across enterprise environments. Built for IT auditors, compliance officers, and information security professionals who need a structured, repeatable approach to evaluating IT controls.

---

## Table of Contents

- [Overview](#overview)
- [What is IT Auditing?](#what-is-it-auditing)
- [Features](#features)
- [Architecture](#architecture)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Sample Walkthrough](#sample-walkthrough)
- [CLI Reference](#cli-reference)
- [Control Frameworks](#control-frameworks)
- [Project Structure](#project-structure)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

The IT Audit Toolkit provides a structured methodology for planning, executing, and reporting on IT audits. It covers six critical audit domains with pre-built checklists containing realistic controls mapped to industry frameworks including COBIT 2019, ISO 27001:2022, NIST CSF, and PCI-DSS v4.0.

Whether you are auditing a financial services platform, healthcare system, e-commerce application, or enterprise ERP environment, this toolkit provides the foundation for thorough, defensible audit work.

## What is IT Auditing?

IT auditing is the systematic examination of an organization's information technology infrastructure, policies, and operations. The goal is to determine whether IT controls adequately protect assets, maintain data integrity, and operate effectively in support of the organization's objectives.

### Key Objectives of IT Auditing

1. **Safeguarding Assets** - Verify that IT systems and data are protected from unauthorized access, modification, or destruction
2. **Data Integrity** - Ensure that information is complete, accurate, and processed correctly
3. **Operational Effectiveness** - Confirm that IT operations support business objectives efficiently
4. **Regulatory Compliance** - Validate adherence to applicable laws, regulations, and industry standards
5. **Risk Management** - Identify, assess, and recommend mitigation for IT-related risks

### The Audit Lifecycle

```
Planning  -->  Fieldwork  -->  Analysis  -->  Reporting  -->  Follow-Up
   |              |              |              |              |
   v              v              v              v              v
 Scoping      Testing        Risk          Draft          Track
 Objectives   Controls      Scoring       Report        Remediation
 Resources    Evidence      Findings      Review        Re-testing
```

## Features

### Audit Management
- Create and manage audit engagements with full metadata tracking
- Define audit scope across six IT control domains
- Track engagement status from planning through follow-up

### Pre-Built Checklists (69 Controls)
- **Access Control** - 15 controls covering identity management, authentication, authorization, and access monitoring
- **Change Management** - 12 controls for change request, approval, testing, and deployment processes
- **Incident Response** - 10 controls spanning detection, containment, eradication, recovery, and lessons learned
- **Data Backup & Recovery** - 10 controls for backup operations, integrity verification, and disaster recovery
- **Network Security** - 12 controls for perimeter defense, segmentation, monitoring, and wireless security
- **Compliance** - 10 controls addressing PCI-DSS, SOX, and cross-framework regulatory requirements

### Risk Assessment
- Quantitative risk scoring using likelihood x impact matrix (5x5 grid)
- Automated compliance percentage calculation per audit area
- Risk aggregation across the full engagement
- Color-coded severity classification (Critical / High / Medium / Low / Informational)

### Professional Reporting
- Executive summary with key metrics and risk posture overview
- Detailed findings with evidence references and remediation recommendations
- Visual risk heat map with color-coded severity indicators
- Prioritized remediation roadmap with effort and timeline estimates
- HTML report output suitable for stakeholder distribution

### Command-Line Interface
- Built on Click for intuitive command structure
- Rich terminal output with formatted tables and progress indicators
- JSON-based data persistence for audit state management
- Pipeline-friendly for integration with CI/CD and automation workflows

## Architecture

```
it-audit-toolkit/
|
|-- src/
|   |-- __init__.py          # Package initialization
|   |-- models.py            # Core data models (dataclasses)
|   |-- audit_engine.py      # Audit execution engine
|   |-- risk_calculator.py   # Risk scoring and compliance metrics
|   |-- reporter.py          # Report generation (Jinja2)
|   |-- cli.py               # Command-line interface (Click)
|   |-- checklists/          # Pre-built control checklists
|       |-- access_control.json
|       |-- change_management.json
|       |-- incident_response.json
|       |-- data_backup.json
|       |-- network_security.json
|       |-- compliance.json
|
|-- templates/
|   |-- audit_report.html    # HTML report template
|
|-- data/
|   |-- sample_audit.json    # Sample completed audit
|
|-- tests/
|   |-- __init__.py
|   |-- test_risk.py         # Risk calculator tests
|   |-- test_reporter.py     # Reporter tests
|
|-- docs/
|   |-- AUDIT_METHODOLOGY.md
|   |-- CONTROL_FRAMEWORKS.md
|   |-- RISK_ASSESSMENT.md
|   |-- SAMPLE_WALKTHROUGH.md
|   |-- screenshots/
|       |-- SCREENSHOTS.md
|
|-- requirements.txt
|-- .gitignore
|-- LICENSE
|-- README.md
```

## Installation

### Prerequisites

- Python 3.9 or higher
- pip package manager

### Setup

```bash
# Clone the repository
git clone <repository-url>
cd it-audit-toolkit

# Create a virtual environment (recommended)
python -m venv venv

# Activate the virtual environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

### Verify Installation

```bash
python -m src.cli --help
```

You should see the available commands: `new-audit`, `run-checklist`, `add-finding`, `calculate-risk`, and `generate-report`.

## Quick Start

```bash
# 1. Create a new audit engagement
python -m src.cli new-audit \
  --name "Q1 2026 Infrastructure Audit" \
  --client "Acme Financial Services" \
  --lead-auditor "J. Smith" \
  --output data/my_audit.json

# 2. Run the access control checklist
python -m src.cli run-checklist \
  --audit data/my_audit.json \
  --checklist access_control

# 3. Add a finding discovered during fieldwork
python -m src.cli add-finding \
  --audit data/my_audit.json \
  --title "Privileged Accounts Lack MFA" \
  --area "Access Control" \
  --severity high \
  --description "Domain administrator accounts do not enforce multi-factor authentication"

# 4. Calculate risk scores
python -m src.cli calculate-risk \
  --audit data/my_audit.json

# 5. Generate the audit report
python -m src.cli generate-report \
  --audit data/my_audit.json \
  --output reports/q1_audit_report.html
```

## Sample Walkthrough

This walkthrough demonstrates a complete audit of a fictional digital lending platform called "QuickLend Pro."

### Scenario

QuickLend Pro is a mid-sized fintech company processing approximately 50,000 loan applications per month. They handle sensitive financial data including SSNs, bank account numbers, and credit reports. The platform runs on a hybrid cloud infrastructure with on-premises database servers and cloud-hosted application tier.

### Step 1: Initiate the Engagement

```bash
python -m src.cli new-audit \
  --name "QuickLend Pro Annual IT Audit" \
  --client "QuickLend Pro Inc." \
  --lead-auditor "Maria Chen, CISA" \
  --output data/quicklend_audit.json
```

### Step 2: Execute Checklists

Run each checklist domain, answering control questions as prompted:

```bash
# Access controls - critical for financial data protection
python -m src.cli run-checklist --audit data/quicklend_audit.json --checklist access_control

# Change management - important for platform stability
python -m src.cli run-checklist --audit data/quicklend_audit.json --checklist change_management

# Data backup - essential for disaster recovery readiness
python -m src.cli run-checklist --audit data/quicklend_audit.json --checklist data_backup
```

### Step 3: Document Findings

```bash
python -m src.cli add-finding \
  --audit data/quicklend_audit.json \
  --title "Service Accounts Using Shared Credentials" \
  --area "Access Control" \
  --severity critical \
  --description "Three production service accounts share the same password, which has not been rotated in 14 months. These accounts have direct database access to loan application records containing PII."

python -m src.cli add-finding \
  --audit data/quicklend_audit.json \
  --title "No Formal Change Advisory Board" \
  --area "Change Management" \
  --severity medium \
  --description "Changes to production are approved by individual team leads without a formal CAB review process. Emergency changes bypass all approval workflows."
```

### Step 4: Assess Risk and Generate Report

```bash
python -m src.cli calculate-risk --audit data/quicklend_audit.json
python -m src.cli generate-report --audit data/quicklend_audit.json --output reports/quicklend_report.html
```

The generated HTML report includes an executive summary, detailed findings with remediation recommendations, a risk heat map, and a prioritized remediation roadmap.

## CLI Reference

| Command | Description |
|---------|-------------|
| `new-audit` | Create a new audit engagement with metadata |
| `run-checklist` | Execute a control checklist interactively |
| `add-finding` | Record an audit finding with severity rating |
| `calculate-risk` | Compute risk scores and compliance percentages |
| `generate-report` | Generate a professional HTML audit report |

### Global Options

| Option | Description |
|--------|-------------|
| `--help` | Display help information for any command |
| `--verbose` | Enable detailed logging output |

## Control Frameworks

This toolkit's controls are informed by the following industry frameworks:

| Framework | Version | Coverage |
|-----------|---------|----------|
| COBIT | 2019 | IT governance and management objectives |
| ISO 27001 | 2022 | Information security management controls |
| NIST CSF | 2.0 | Cybersecurity framework functions |
| PCI-DSS | 4.0 | Payment card data security standards |
| SOX | Section 404 | IT general controls for financial reporting |
| ITIL | 4 | IT service management practices |

For detailed framework mapping, see [docs/CONTROL_FRAMEWORKS.md](docs/CONTROL_FRAMEWORKS.md).

## Contributing

Contributions are welcome. To contribute:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-checklist`)
3. Add your changes with appropriate tests
4. Ensure all tests pass (`python -m pytest tests/`)
5. Submit a pull request with a clear description of the changes

### Adding Custom Checklists

Place new checklist JSON files in `src/checklists/` following the existing schema:

```json
{
  "domain": "Your Audit Domain",
  "version": "1.0",
  "framework_references": ["ISO 27001:2022", "COBIT 2019"],
  "controls": [
    {
      "id": "DOMAIN-001",
      "title": "Control Title",
      "description": "Detailed control description",
      "test_procedure": "Steps to test this control",
      "expected_evidence": "What evidence to collect",
      "framework_mapping": {"ISO 27001": "A.x.x", "COBIT": "APO.xx"},
      "risk_weight": 3
    }
  ]
}
```

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
