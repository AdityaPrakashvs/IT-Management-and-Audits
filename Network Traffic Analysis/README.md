# Network Traffic Analyzer

A comprehensive Python toolkit for parsing, analyzing, and visualizing network traffic captured in PCAP format. Built for security researchers, network administrators, and incident responders who need fast, scriptable insight into packet captures.

> **Legal Notice:** This tool is intended exclusively for analyzing network traffic that you are **authorized** to capture and inspect. Unauthorized interception or analysis of network traffic may violate local, state, federal, or international law. Always obtain explicit written permission before capturing or analyzing network traffic on any network you do not own.

---

## Overview

Network Traffic Analyzer ingests PCAP files and produces structured analysis covering:

- **Protocol distribution** -- breakdown of TCP, UDP, ICMP, DNS, HTTP, and other protocols observed in the capture.
- **Top talkers** -- source and destination IPs ranked by packet count and total bytes transferred.
- **Port frequency analysis** -- which destination ports see the most traffic, useful for service enumeration.
- **Bandwidth estimation** -- bytes transferred per IP address over the capture window.
- **Anomaly detection** -- automatic flagging of port scans, traffic volume spikes, and suspicious DNS query patterns.
- **Visual reports** -- publication-ready charts (pie, bar, timeline) and markdown/HTML reports.

The tool runs entirely offline against static PCAP files. It does **not** perform live capture or inject any traffic.

## Use Cases

| Scenario | How This Tool Helps |
|---|---|
| Incident response triage | Quickly identify top talkers, unusual protocols, and scanning activity in a packet capture from a compromised host. |
| Firewall rule validation | Capture traffic before and after a rule change, then compare protocol and port distributions. |
| Baseline profiling | Analyze a "known good" capture to establish normal traffic patterns, then detect deviations in future captures. |
| CTF / lab exercises | Parse synthetic PCAPs and practice identifying anomalous traffic in a controlled environment. |
| Compliance auditing | Document which protocols and services are active on a network segment. |

## Features

- **Scapy-powered parsing** with a pure-Python fallback for environments where Scapy cannot be installed.
- **Click-based CLI** with dedicated subcommands: `parse`, `analyze`, `detect`, `visualize`, `report`.
- **Rich terminal output** with colored tables and progress indicators.
- **Matplotlib visualizations** saved as PNG files for embedding in reports.
- **Markdown and HTML report generation** with all findings in a single document.
- **Extensible anomaly detection** with configurable thresholds.

## Project Structure

```
network-traffic-analyzer/
├── src/
│   ├── __init__.py
│   ├── parser.py            # PCAP parsing (Scapy + fallback)
│   ├── analyzer.py          # Statistical traffic analysis
│   ├── anomaly_detector.py  # Anomaly detection engine
│   ├── visualizer.py        # Chart generation
│   ├── reporter.py          # Markdown / HTML report builder
│   └── cli.py               # Click CLI entry point
├── samples/
│   ├── generate_sample_pcap.py  # Synthetic PCAP generator
│   └── README.md
├── tests/
│   ├── __init__.py
│   ├── test_parser.py
│   └── test_anomaly.py
├── docs/
│   ├── WIRESHARK_GUIDE.md
│   ├── ANALYSIS_METHODOLOGY.md
│   ├── ANOMALY_PATTERNS.md
│   └── screenshots/
│       └── SCREENSHOTS.md
├── requirements.txt
├── .gitignore
├── LICENSE
└── README.md
```

## Requirements

- Python 3.8 or later
- Operating system: Linux, macOS, or Windows
- Optional: Wireshark / tshark for live packet capture (this tool only reads existing PCAPs)

## Installation

```bash
# Clone the repository
git clone https://github.com/youruser/network-traffic-analyzer.git
cd network-traffic-analyzer

# Create a virtual environment (recommended)
python -m venv venv
source venv/bin/activate   # Linux/macOS
venv\Scripts\activate      # Windows

# Install dependencies
pip install -r requirements.txt
```

### Dependencies

| Package | Purpose |
|---|---|
| scapy | PCAP parsing and packet dissection |
| matplotlib | Chart and graph generation |
| rich | Terminal formatting and tables |
| click | Command-line interface framework |

## Quick Start

### 1. Generate a sample PCAP (optional)

If you do not have a PCAP file handy, generate a synthetic one:

```bash
python samples/generate_sample_pcap.py --output samples/demo.pcap --count 500
```

### 2. Parse a PCAP file

```bash
python -m src.cli parse samples/demo.pcap --output parsed.json
```

This writes a JSON array of packet metadata dictionaries to `parsed.json`.

### 3. Analyze traffic

```bash
python -m src.cli analyze samples/demo.pcap
```

Prints protocol distribution, top source/destination IPs, port frequency, and bandwidth summary to the terminal.

### 4. Detect anomalies

```bash
python -m src.cli detect samples/demo.pcap --port-threshold 15 --spike-factor 3.0
```

Flags port scans, traffic spikes, and suspicious DNS queries found in the capture.

### 5. Generate visualizations

```bash
python -m src.cli visualize samples/demo.pcap --output-dir charts/
```

Saves `protocol_distribution.png`, `traffic_timeline.png`, and `top_talkers.png` into the `charts/` directory.

### 6. Generate a report

```bash
python -m src.cli report samples/demo.pcap --format html --output report.html
python -m src.cli report samples/demo.pcap --format markdown --output report.md
```

Produces a comprehensive analysis report in the requested format.

## CLI Reference

```
Usage: python -m src.cli [OPTIONS] COMMAND [ARGS]...

Commands:
  parse      Parse a PCAP file and export packet metadata as JSON.
  analyze    Run statistical analysis on a PCAP file.
  detect     Run anomaly detection on a PCAP file.
  visualize  Generate charts from a PCAP file.
  report     Generate a full analysis report from a PCAP file.
```

Run any command with `--help` for detailed option descriptions.

## Configuration & Thresholds

Anomaly detection thresholds can be tuned via CLI flags or by modifying the defaults in `src/anomaly_detector.py`:

| Parameter | Default | Description |
|---|---|---|
| `port_scan_threshold` | 15 | Minimum distinct destination ports from a single IP to flag as a port scan |
| `spike_factor` | 3.0 | Multiplier above the mean packet rate to flag a traffic spike |
| `dns_entropy_threshold` | 4.0 | Shannon entropy threshold above which a DNS query is considered suspicious |
| `time_window` | 60 | Seconds per bucket when computing traffic rate for spike detection |

## Extending the Tool

- **Add new anomaly detectors:** Subclass or add methods in `src/anomaly_detector.py`. Each detector receives the parsed packet list and returns a list of finding dictionaries.
- **Add new visualizations:** Add methods to `src/visualizer.py` following the existing pattern (accept analysis results, return a matplotlib figure, save to file).
- **Custom report templates:** Modify the Jinja-style string templates in `src/reporter.py` or replace them with a full Jinja2 integration.

## Running Tests

```bash
python -m pytest tests/ -v
```

## Disclaimer

This software is provided as-is for **lawful security research and network administration purposes only**. The author assumes no liability for misuse. You are solely responsible for ensuring that your use of this tool complies with all applicable laws and regulations. Never analyze traffic on networks without explicit authorization from the network owner.

## License

This project is released under the [MIT License](LICENSE).

## Author

Created by Dr Amit Dua. Contributions and issues welcome.
