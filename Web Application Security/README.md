# Web Application Security Scanner

> **A comprehensive web application vulnerability scanner built in Python for security professionals and penetration testers.**

---

## IMPORTANT: ETHICAL USE DISCLAIMER

```
+===========================================================================+
|                        ETHICAL USE POLICY                                  |
|                                                                           |
|  This tool is designed EXCLUSIVELY for authorized security testing.       |
|                                                                           |
|  YOU MUST:                                                                |
|  - Only scan systems you OWN or have WRITTEN AUTHORIZATION to test        |
|  - Never use this tool against production systems without approval        |
|  - Follow all applicable local, state, federal, and international laws    |
|  - Report discovered vulnerabilities responsibly                          |
|  - Use the included vulnerable application (localhost) for learning       |
|                                                                           |
|  UNAUTHORIZED ACCESS TO COMPUTER SYSTEMS IS ILLEGAL.                     |
|  Violations may result in criminal prosecution under laws such as the     |
|  Computer Fraud and Abuse Act (CFAA), Computer Misuse Act, and similar   |
|  legislation worldwide.                                                   |
|                                                                           |
|  The authors assume NO LIABILITY for misuse of this software.            |
+===========================================================================+
```

**By using this software, you acknowledge that you have read, understood, and agree to comply with the ethical use policy above.**

---

## Overview

The Web Application Security Scanner is a modular, extensible tool that automates the detection of common web application vulnerabilities based on the **OWASP Top 10** methodology. It includes a purposely vulnerable Flask application for safe, legal testing in a local environment.

### Key Features

- **SQL Injection Detection** - Tests for common SQLi patterns across form inputs and URL parameters
- **Cross-Site Scripting (XSS) Detection** - Identifies reflected XSS vulnerabilities with multiple payload strategies
- **Security Header Analysis** - Audits HTTP response headers against security best practices
- **TCP Port Scanning** - Discovers open ports and identifies running services
- **Directory Enumeration** - Checks for exposed sensitive directories and files
- **Professional HTML Reports** - Generates severity-classified vulnerability reports with remediation guidance
- **Command-Line Interface** - Full CLI with customizable scan profiles via Click
- **Included Vulnerable Target** - A deliberately insecure Flask app for safe localhost testing

---

## Architecture

```
webapp-security-scanner/
|
|-- vulnerable_app/              # Intentionally vulnerable Flask application
|   |-- app.py                   # Main Flask application with deliberate flaws
|   |-- database.py              # SQLite database setup with sample data
|   |-- requirements.txt         # Flask dependencies
|   |-- templates/
|       |-- base.html            # Base Jinja2 template
|       |-- login.html           # Vulnerable login form
|       |-- search.html          # Vulnerable search page
|       |-- profile.html         # Insecure direct object reference page
|
|-- scanner/                     # Security scanning modules
|   |-- __init__.py
|   |-- cli.py                   # Click-based command-line interface
|   |-- header_check.py          # HTTP security header analyzer
|   |-- sqli_scanner.py          # SQL injection vulnerability scanner
|   |-- xss_scanner.py           # Cross-site scripting scanner
|   |-- port_scanner.py          # TCP port scanner using raw sockets
|   |-- directory_scanner.py     # Directory/file enumeration scanner
|   |-- reporter.py              # HTML report generator with severity ratings
|
|-- reports/                     # Generated vulnerability reports
|   |-- template.html            # Jinja2 HTML report template
|
|-- tests/                       # Unit and integration tests
|   |-- test_scanners.py         # Test suite for all scanner modules
|
|-- docs/                        # Documentation
|   |-- OWASP_TOP10_REFERENCE.md # OWASP Top 10 vulnerability reference
|   |-- SETUP_GUIDE.md           # Environment setup instructions
|   |-- METHODOLOGY.md           # Scanning methodology documentation
|   |-- ETHICAL_GUIDELINES.md    # Responsible security testing guidelines
|   |-- screenshots/
|       |-- SCREENSHOTS.md       # Screenshot documentation
|
|-- requirements.txt             # Project-wide Python dependencies
|-- Makefile                     # Build automation targets
|-- .gitignore                   # Git ignore rules
|-- LICENSE                      # MIT License
|-- README.md                    # This file
```

---

## OWASP Top 10 Coverage

This scanner addresses vulnerabilities from the [OWASP Top 10 (2021)](https://owasp.org/www-project-top-ten/) framework:

| OWASP Category | Scanner Module | Status |
|---|---|---|
| A01:2021 - Broken Access Control | `directory_scanner.py` | Covered |
| A02:2021 - Cryptographic Failures | `header_check.py` | Partial |
| A03:2021 - Injection | `sqli_scanner.py` | Covered |
| A04:2021 - Insecure Design | Manual review required | Reference |
| A05:2021 - Security Misconfiguration | `header_check.py`, `directory_scanner.py` | Covered |
| A06:2021 - Vulnerable Components | Dependency analysis | Planned |
| A07:2021 - Authentication Failures | `sqli_scanner.py` | Partial |
| A08:2021 - Software and Data Integrity | `header_check.py` | Partial |
| A09:2021 - Security Logging Failures | `header_check.py` | Partial |
| A10:2021 - Server-Side Request Forgery | Manual review required | Planned |

---

## Quick Start

### Prerequisites

- Python 3.8 or higher
- pip (Python package manager)
- Virtual environment (recommended)

### Installation

```bash
# Clone the repository
git clone <repository-url>
cd webapp-security-scanner

# Create and activate virtual environment
python -m venv venv

# On Windows:
venv\Scripts\activate
# On Linux/macOS:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

### Running the Vulnerable Target Application

**WARNING: Only run this application on localhost. Never expose it to a network.**

```bash
# Start the vulnerable Flask application
make run-target

# Or manually:
cd vulnerable_app
pip install -r requirements.txt
python app.py
```

The vulnerable application will start on `http://127.0.0.1:5000`.

### Running a Scan

```bash
# Full scan against the vulnerable target
make scan

# Custom scan with CLI options
python -m scanner.cli scan --target http://127.0.0.1:5000 --output reports/scan_report.html

# Generate report only
make report
```

### Running Tests

```bash
# Run all tests
python -m pytest tests/ -v

# Run with coverage
python -m pytest tests/ -v --cov=scanner
```

---

## CLI Usage

```
Usage: python -m scanner.cli scan [OPTIONS]

  Run a security scan against a target web application.

Options:
  --target TEXT    Target URL to scan (default: http://127.0.0.1:5000)
  --output TEXT    Output file path for the HTML report
  --modules TEXT   Comma-separated list of modules to run
                   (headers,sqli,xss,ports,dirs)
  --timeout INT    Request timeout in seconds (default: 10)
  --verbose        Enable verbose output
  --help           Show this message and exit.
```

### Examples

```bash
# Scan with all modules
python -m scanner.cli scan --target http://127.0.0.1:5000 --output report.html

# Scan specific modules only
python -m scanner.cli scan --target http://127.0.0.1:5000 --modules headers,sqli

# Verbose output for debugging
python -m scanner.cli scan --target http://127.0.0.1:5000 --verbose
```

---

## Scan Modules

### Security Header Check (`header_check.py`)
Analyzes HTTP response headers for missing or misconfigured security headers:
- `X-Frame-Options` - Clickjacking protection
- `Content-Security-Policy` - XSS and injection mitigation
- `Strict-Transport-Security` - HTTPS enforcement
- `X-Content-Type-Options` - MIME type sniffing prevention
- `X-XSS-Protection` - Browser XSS filter
- `Referrer-Policy` - Referrer information leakage
- `Permissions-Policy` - Browser feature restrictions
- `Cache-Control` - Sensitive data caching prevention

### SQL Injection Scanner (`sqli_scanner.py`)
Tests input fields and URL parameters for SQL injection vulnerabilities using:
- Boolean-based blind injection payloads
- Error-based injection payloads
- UNION-based injection payloads
- Time-based blind injection payloads
- Authentication bypass payloads

### XSS Scanner (`xss_scanner.py`)
Detects reflected cross-site scripting vulnerabilities:
- Basic `<script>` tag injection
- Event handler injection (`onerror`, `onload`, etc.)
- SVG/IMG tag-based payloads
- Encoded and obfuscated payloads
- Context-aware payload selection

### Port Scanner (`port_scanner.py`)
TCP connect scan for common service ports:
- Configurable port range
- Service identification
- Concurrent scanning with threading
- Connection timeout handling

### Directory Scanner (`directory_scanner.py`)
Enumerates exposed directories and sensitive files:
- Common admin panels
- Configuration files
- Backup files
- Version control directories
- API documentation endpoints

---

## Report Format

Generated HTML reports include:
- **Executive Summary** - High-level vulnerability overview
- **Severity Classification** - Critical, High, Medium, Low, Informational
- **Detailed Findings** - Each vulnerability with description, evidence, and remediation
- **OWASP Mapping** - Cross-reference to OWASP Top 10 categories
- **Scan Metadata** - Target, timestamp, duration, modules executed

---

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-scanner-module`)
3. Write tests for new functionality
4. Ensure all tests pass (`python -m pytest tests/ -v`)
5. Submit a pull request with a clear description

---

## Responsible Disclosure

If you discover a vulnerability in a real-world application using techniques learned from this tool:

1. **Do NOT publicly disclose** the vulnerability immediately
2. **Contact the vendor** directly through their security contact or bug bounty program
3. **Provide a detailed report** including steps to reproduce
4. **Allow reasonable time** (typically 90 days) for the vendor to fix the issue
5. **Follow coordinated disclosure** practices as outlined by CERT/CC

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

## Disclaimer

THIS SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND. THE AUTHORS ARE NOT RESPONSIBLE FOR ANY DAMAGE OR ILLEGAL ACTIVITY CAUSED BY THE USE OR MISUSE OF THIS SOFTWARE. USE AT YOUR OWN RISK AND ONLY IN COMPLIANCE WITH APPLICABLE LAWS.

---

*Built for security professionals. Use responsibly.*
