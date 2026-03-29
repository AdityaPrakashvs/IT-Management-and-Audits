# 🌐 Network Traffic Analyzer (P03)

This project focuses on analyzing network traffic using Python to identify patterns, detect anomalies, and generate security insights from PCAP files.

---

## 🎯 Objective

* Capture and analyze network traffic data
* Understand packet structure and protocols
* Detect anomalies such as port scans and traffic spikes
* Visualize network traffic patterns
* Generate professional analysis reports

---

## 🧠 Key Concepts

* Network Traffic Analysis
* TCP/IP & OSI Model
* Packet Structure (IP, TCP/UDP headers)
* Protocol Analysis (TCP, UDP, ICMP, DNS)
* Anomaly Detection (Port Scan, DDoS, DNS Tunneling)
* Traffic Visualization
* Cybersecurity Monitoring

---

## ⚙️ Tools & Technologies

* Python (3.8+)
* Scapy (Packet Analysis)
* Wireshark (Traffic Capture)
* matplotlib (Visualization)
* CLI-based Analysis Tool

---

## 📂 Project Files

* `P03_Network_Traffic_Analyzer_LabManual.pdf` → Complete lab manual and implementation guide 

---

## 🚀 What This Project Covers

### 🔹 1. Environment Setup

* Clone network traffic analyzer repository
* Set up Python virtual environment
* Install dependencies and run CLI tool

---

### 🔹 2. Traffic Capture & Parsing

* Generate sample PCAP file
* Parse packets to extract:

  * Source & Destination IP
  * Protocol (TCP/UDP/ICMP)
  * Ports & Packet Size

👉 As shown in the lab, ~1500 packets were generated with TCP dominating traffic (~59%) 

---

### 🔹 3. Traffic Analysis

* Protocol distribution analysis
* Top source/destination IPs
* Port usage analysis
* Bandwidth consumption

---

### 🔹 4. Anomaly Detection

* Detect suspicious network behavior:

  * Port Scans
  * Traffic Spikes
  * Suspicious DNS Queries

👉 Example: Port scan detected when a single source hits multiple ports in a short time 

---

### 🔹 5. Visualization

* Generate charts:

  * Protocol Distribution (Pie Chart)
  * Top Talkers (Bar Chart)
  * Traffic Timeline

---

### 🔹 6. Report Generation

* Generate terminal summary
* Generate HTML report with:

  * Traffic statistics
  * Anomaly results
  * Charts & insights

---

### 🔹 7. Custom Traffic Analysis

* Capture real network traffic using Wireshark
* Analyze custom PCAP file
* Compare real vs sample traffic patterns

---

## 📊 Sample Output

* Total Packets: ~1500
* Protocol Distribution:

  * TCP: ~55–60%
  * UDP: ~25–30%
  * ICMP: ~10%
* Detected anomalies:

  * Port Scan (HIGH)
  * Traffic Spike (MEDIUM)
  * Suspicious DNS Activity

---

## 🌍 Real-World Application

This project is highly relevant for:

* Cybersecurity Analysts
* SOC (Security Operations Center) Teams
* Network Engineers
* IT Auditors

Organizations use network traffic analysis to detect:

* DDoS attacks
* Unauthorized access
* Data exfiltration
* Malware communication

👉 As per CERT-In guidelines, cyber incidents must be reported within **6 hours** 

---

## 💡 Key Learning

* Network traffic reveals security threats in real-time
* TCP ensures reliability, while UDP prioritizes speed
* DNS anomalies can indicate data exfiltration
* Visualization helps in faster decision-making

---

## 📝 Future Improvements

* Integrate real-time packet capture
* Add machine learning-based anomaly detection
* Enhance dashboard visualization
* Automate alerting system

---

## 👨‍💻 Author

**Aditya Prakash**
