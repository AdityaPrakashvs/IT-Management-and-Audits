# Incident Response Playbook Engine

A comprehensive incident response simulation and playbook management framework built on the **NIST SP 800-61 Rev. 2** methodology. This tool enables security teams to practice incident response decision-making through interactive simulations, manage structured playbooks for common incident types, and generate professional incident reports.

---

## Overview

The IR Playbook Engine provides:

- **Interactive simulations** for five common incident types with scored decision trees
- **Structured playbooks** following the NIST IR lifecycle (Preparation, Detection & Analysis, Containment/Eradication/Recovery, Post-Incident Activity)
- **Severity calculator** combining CVSS-inspired technical scoring with business impact assessment
- **Evidence tracker** with full chain-of-custody documentation
- **Timeline generator** producing realistic incident timelines with industry-standard metrics (MTTD, MTTC, MTTR)
- **Report generator** creating professional HTML, JSON, and text incident reports using Jinja2 templates
- **CLI interface** for streamlined access to all features

---

## NIST SP 800-61 Methodology

This project implements the four-phase incident response lifecycle defined in NIST Special Publication 800-61 Revision 2:

1. **Preparation** - Policies, procedures, tools, training, and communication planning
2. **Detection & Analysis** - Alert validation, event correlation, scope determination, severity classification
3. **Containment, Eradication & Recovery** - Stopping damage, removing threats, restoring operations
4. **Post-Incident Activity** - Lessons learned, reporting, evidence retention, control improvements

Each simulation scenario walks the analyst through all four phases with realistic decision points and scored feedback.

---

## Simulation Features

### Scenario Library

| Scenario | Type | Severity | Description |
|----------|------|----------|-------------|
| **Data Breach** | `data_breach` | Critical | Customer database exfiltration via SQL injection and compromised service account |
| **Ransomware** | `ransomware` | Critical | LockStorm ransomware deployment across corporate network via phishing and lateral movement |
| **Phishing** | `phishing` | High | Spear-phishing campaign leading to credential compromise and $340K wire fraud attempt |
| **DDoS** | `ddos` | High | Multi-vector DDoS attack (38 Gbps) targeting e-commerce platform during peak event |
| **Insider Threat** | `insider_threat` | High | Departing DBA exfiltrating trade secrets and planting logic bombs on production databases |

### Scoring System

Each decision point offers 4 choices scored from 0-10 based on IR best practices:
- **Expert (90-100%):** Demonstrates comprehensive IR knowledge
- **Proficient (70-89%):** Solid skills with areas for refinement
- **Developing (50-69%):** Foundational understanding; review recommended
- **Novice (<50%):** Significant gaps; structured training recommended

### Feedback
Every choice receives immediate feedback explaining:
- Why the chosen action was effective or problematic
- What the consequences of that action are in the scenario
- What the optimal approach would have been

---

## Playbook Descriptions

### Data Breach Playbook (`playbooks/data_breach_playbook.md`)
22-step comprehensive response procedure for data exfiltration incidents. Covers alert validation, scope assessment, data classification, regulatory notification timelines (GDPR, HIPAA, PCI-DSS, CCPA), evidence collection checklist, and communication templates.

### Ransomware Playbook (`playbooks/ransomware_playbook.md`)
22-step response for ransomware attacks including rapid containment procedures, backup assessment framework, credential remediation (KRBTGT rotation), ransom payment decision framework, and recovery prioritization tiers.

### Phishing Playbook (`playbooks/phishing_playbook.md`)
20-step response for phishing campaigns and business email compromise. Includes email header analysis guide, blast radius assessment, account containment procedures, financial fraud prevention, and O365/M365 containment commands.

### DDoS Playbook (`playbooks/ddos_playbook.md`)
20-step response for volumetric, protocol, and application-layer DDoS attacks. Covers upstream ISP coordination, attack classification matrix, layered mitigation techniques, and monitoring threshold reference.

### Insider Threat Playbook (`playbooks/insider_threat_playbook.md`)
21-step response for malicious and negligent insider incidents. Addresses HR/Legal coordination requirements, simultaneous access revocation procedures, sabotage detection, evidence handling for litigation, and departing employee checklists.

---

## Project Structure

```
incident-response-playbook/
|-- src/
|   |-- __init__.py              # Package initialization
|   |-- models.py                # Core data models (Incident, Alert, Evidence, etc.)
|   |-- simulator.py             # Interactive simulation engine
|   |-- timeline.py              # Incident timeline generation
|   |-- severity_calculator.py   # CVSS-based severity scoring
|   |-- evidence_tracker.py      # Chain-of-custody evidence management
|   |-- reporter.py              # Jinja2-based report generation
|   |-- cli.py                   # Click CLI interface
|   |-- scenarios/
|       |-- __init__.py           # Scenario registry
|       |-- data_breach.py        # Data breach scenario
|       |-- ransomware.py         # Ransomware scenario
|       |-- phishing.py           # Phishing scenario
|       |-- ddos.py               # DDoS scenario
|       |-- insider_threat.py     # Insider threat scenario
|-- playbooks/
|   |-- data_breach_playbook.md
|   |-- ransomware_playbook.md
|   |-- phishing_playbook.md
|   |-- ddos_playbook.md
|   |-- insider_threat_playbook.md
|-- templates/
|   |-- incident_report.html     # HTML report template
|   |-- evidence_log.md          # Evidence collection template
|-- tests/
|   |-- __init__.py
|   |-- test_simulator.py        # Simulation engine tests
|   |-- test_severity.py         # Severity calculator tests
|-- docs/
|   |-- IR_METHODOLOGY.md        # NIST IR lifecycle documentation
|   |-- SEVERITY_FRAMEWORK.md    # Severity rating methodology
|   |-- EVIDENCE_HANDLING.md     # Digital forensics evidence guide
|   |-- SIMULATION_GUIDE.md      # How to run simulations
|   |-- screenshots/
|       |-- SCREENSHOTS.md
|-- requirements.txt
|-- .gitignore
|-- LICENSE
|-- README.md
```

---

## Setup

### Prerequisites
- Python 3.8 or higher
- pip package manager

### Installation

```bash
# Clone the repository
git clone <repository-url>
cd incident-response-playbook

# Create virtual environment (recommended)
python -m venv venv
source venv/bin/activate    # Linux/macOS
venv\Scripts\activate       # Windows

# Install dependencies
pip install -r requirements.txt
```

### Verify Installation
```bash
# Run tests
pytest tests/ -v

# List available scenarios
python -m src.cli list-scenarios
```

---

## Usage

### List Available Scenarios
```bash
python -m src.cli list-scenarios
python -m src.cli list-scenarios --format json
```

### Run Interactive Simulation
```bash
python -m src.cli simulate --scenario data_breach
python -m src.cli simulate --scenario ransomware --output results.json
python -m src.cli simulate --scenario phishing
python -m src.cli simulate --scenario ddos
python -m src.cli simulate --scenario insider_threat
```

### Generate Incident Report
```bash
python -m src.cli generate-report --scenario data_breach --format html
python -m src.cli generate-report --scenario ransomware --format json --output report.json
python -m src.cli generate-report --scenario phishing --format text
```

### Calculate Severity
```bash
python -m src.cli severity \
    --data-class confidential \
    --records 250000 \
    --system-tier tier_1_critical \
    --frameworks GDPR,HIPAA,PCI-DSS \
    --notification
```

### View Reference Materials
```bash
python -m src.cli evidence-guide       # Evidence collection priority order
python -m src.cli severity-matrix      # Severity classification matrix
python -m src.cli timeline             # Generate sample incident timeline
```

---

## Running Tests

```bash
# Run all tests
pytest tests/ -v

# Run specific test module
pytest tests/test_simulator.py -v
pytest tests/test_severity.py -v

# Run with coverage
pytest tests/ -v --cov=src
```

---

## Documentation

| Document | Description |
|----------|-------------|
| [IR Methodology](docs/IR_METHODOLOGY.md) | NIST SP 800-61 lifecycle explained with roles and metrics |
| [Severity Framework](docs/SEVERITY_FRAMEWORK.md) | Composite severity calculation methodology |
| [Evidence Handling](docs/EVIDENCE_HANDLING.md) | Digital forensics evidence collection and preservation |
| [Simulation Guide](docs/SIMULATION_GUIDE.md) | How to run simulations and interpret results |

---

## Key Design Decisions

- **NIST SP 800-61 alignment:** All scenarios follow the four-phase lifecycle
- **Realistic scenarios:** Attack narratives based on documented real-world incidents and common threat patterns
- **Scored decision trees:** Each choice is scored against IR best practices, not arbitrary preferences
- **Blameless feedback:** Feedback explains the reasoning, not just the score
- **Comprehensive playbooks:** 20+ steps per playbook covering the full response lifecycle
- **Evidence integrity:** Chain-of-custody tracking with hash verification follows forensic standards
- **Regulatory awareness:** Playbooks include jurisdiction-specific notification requirements

---

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
