# Real-Time Cybersecurity Threat Detection Dashboard

A lightweight, real-time network monitoring system that detects suspicious traffic, blocks malicious IPs, visualizes attack paths, and generates detailed reports. Built with Python, Flask, and Scapy, this dashboard provides an all-in-one solution for small-scale intrusion detection and response.

---

## Description

This project implements a cybersecurity monitoring tool that captures live network packets, flags traffic involving predefined suspicious IP addresses, automatically blocks those IPs using Windows Firewall, and logs every event. It features:

- A web-based dashboard with real-time log streaming.
- An interactive attack graph built with NetworkX and Matplotlib.
- Automated email alerts for each suspicious packet.
- PDF report generation containing logs, root cause analysis, and the attack graph.
- Simple user authentication to protect the dashboard.

The tool is designed for educational purposes, penetration testing labs, or as a foundation for more advanced intrusion detection systems.

---

## Features

- **Live Packet Sniffing** – Captures IP packets using Scapy and filters for suspicious sources or destinations.
- **Automatic IP Blocking** – Executes Windows Firewall rules to block malicious IPs immediately upon detection.
- **Root Cause Analysis** – Examines packet details (ports, protocols) to suggest possible vulnerabilities (e.g., FTP exposure, SMB abuse).
- **Attack Graph Visualization** – Builds a directed graph of attack paths and updates it in real time.
- **Email Alerts** – Sends detailed email notifications for every detected suspicious packet.
- **PDF Reporting** – Generates a downloadable PDF containing all logs, root cause traces, and the current attack graph.
- **Real-Time Dashboard** – Displays logs, a pie chart of top suspicious IPs, and the live attack graph with auto-refresh.
- **User Authentication** – Simple login page with a matrix-style animation; session-based protection for all routes.

---

## Technologies Used

- **Python 3** – Core language.
- **Flask** – Web framework for the dashboard and API endpoints.
- **Scapy** – Packet manipulation and sniffing.
- **NetworkX & Matplotlib** – Graph creation and visualization.
- **ReportLab** – PDF generation.
- **HTML/CSS/JavaScript** – Frontend with Chart.js for charts.
- **smtplib** – Email alerts via Gmail’s SMTP server.
- **netsh** – Windows Firewall integration for IP blocking.

---

## Installation

### Prerequisites

- Python 3.8 or higher.
- Windows OS (for firewall commands; blocking uses `netsh`).
- Administrator privileges (required to add firewall rules and capture raw packets).
- A Gmail account with an app-specific password (for email alerts).

### Steps

1. Clone the repository:
   ```bash
   git clone https://github.com/Syed-Abdul-Mateen/Automated-Threat-Detection-Engine.git
   cd Automated-Threat-Detection-Engine
   ```

2. Create and activate a virtual environment (optional but recommended):
   ```bash
   python -m venv venv
   venv\Scripts\activate   # On Windows
   ```

3. Install required packages:
   ```bash
   pip install -r requirements.txt
   ```

4. Configure your suspicious IP list and email credentials (see Configuration below).

5. Run the application:
   ```bash
   python app.py
   ```

6. Open your browser and navigate to `http://127.0.0.1:5000`. You will be prompted to log in (default credentials: admin / 1234).

---

## Configuration

### Suspicious IPs

Edit `monitor.py` and modify the `SUSPICIOUS_IPS` set. Add IP addresses you consider malicious, e.g.:

```python
SUSPICIOUS_IPS = {"10.20.118.53", "192.168.1.100"}
```

### Email Alerts

In `email_alert.py`, update the following variables with your Gmail credentials:

```python
EMAIL_SENDER = "your_email@gmail.com"
EMAIL_PASSWORD = "your_app_password"   # Not your regular password; generate an app password
EMAIL_RECEIVER = "alert_destination@example.com"
```

> **Note:** If you use a different email provider, adjust the SMTP server and port accordingly.

### Login Credentials

The default username and password are set in `app.py`:

```python
USERNAME = "admin"
PASSWORD = "1234"
```

Change these to something more secure for production use.

---

## Usage

1. **Start the Flask server** – Run `python app.py`.
2. **Log in** – Use the configured credentials.
3. **Start monitoring** – Click the "Start Monitoring" button. The sniffer begins capturing packets in a background thread.
4. **Observe logs** – The dashboard updates every two seconds with new log entries.
5. **View the attack graph** – The graph image refreshes every five seconds, showing connections between suspicious IPs.
6. **Check top attackers** – A pie chart displays the five most active suspicious IPs.
7. **Block IPs automatically** – When a suspicious IP is detected, a Windows Firewall rule is added to block it.
8. **Receive email alerts** – Each detection triggers an email with source/destination IP and suspected root cause.
9. **Generate a report** – Click "Generate PDF Report" to download a comprehensive PDF containing all logs, root cause traces, and the attack graph.
10. **Stop monitoring** – Use the "Stop Monitoring" button to halt packet capture.
11. **Clear all data** – The "Clear All" button erases logs, resets the graph, and empties the attack counts.

All logs are also saved to `logs/detected_logs.txt` and `logs/root_cause_report.txt` for persistent storage.

---

## Project Structure

```
.
├── app.py                  # Flask application with routes and authentication
├── attack_graph.py         # NetworkX graph management and image generation
├── email_alert.py          # Email notification function
├── monitor.py              # Packet sniffer, IP blocking, root cause analysis
├── pdf_report.py           # PDF report generation using ReportLab
├── requirements.txt        # Python dependencies
├── .gitignore
├── logs/                   # Directory for log files and generated PDFs
│   ├── Attack_Report.pdf
│   ├── detected_logs.txt
│   └── root_cause_report.txt
├── static/                 # Static assets (CSS, images)
│   ├── graph.png           # Current attack graph image
│   └── styles.css          # Dashboard styling
└── templates/              # HTML templates
    ├── index.html          # Main dashboard
    └── login.html          # Login page with matrix animation
```

---

## Customization

- **Root Cause Analysis** – Extend the `analyze_root_cause()` function in `monitor.py` to add more sophisticated detection logic (e.g., checking for specific payloads or unusual packet sizes).
- **Slack Alerts** – The code includes a commented import for `slack_alert`. You can implement a similar function to send alerts to a Slack channel.
- **Graph Styling** – Modify `attack_graph.py` to change the appearance of the attack graph (colors, layout, node size).
- **PDF Layout** – Adjust `pdf_report.py` to include additional sections or format the report differently.

---

## Limitations & Notes

- This tool is designed for **Windows** due to the use of `netsh` for IP blocking. For Linux/macOS, you would need to replace the blocking mechanism (e.g., using `iptables` or `pfctl`).
- Running Scapy requires **administrator/root privileges** on most systems.
- The root cause analysis is simplistic and only inspects well-known ports. It does not perform deep packet inspection.
- Email alerts use Gmail’s SMTP; ensure you have allowed less secure apps or use an app-specific password.
- The dashboard is not intended for production deployment without additional security measures (HTTPS, stronger authentication, input validation).

---

## License

This project is provided for educational and research purposes only. Use it at your own risk. The authors are not responsible for any misuse or damage caused by this software.
