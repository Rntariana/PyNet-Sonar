# PyNet-Sonar
PyNet is a Python network scanner that finds devices on your local network and shows them on a live sonar-style radar dashboard.

Find every device on your network and watch it appear on a live radar.

PyNet Sonar scans a local network range, detects which devices are online, and reports each device's IP address, MAC address, hostname and open ports. It identifies common services such as SSH, HTTP, HTTPS, RDP and SMB. Use it from the command line, a desktop window, or a browser dashboard with an animated sonar display. Results export to CSV or a text report.

Built with the Python standard library only. Nothing to install.

Use responsibly. Only scan networks you own or have explicit permission to test.

Features
Multithreaded scan of any IPv4 range (for example 192.168.1.0/24)
Online/offline detection with cross-platform ping (Windows, Linux, macOS)
IP, MAC (from the ARP table) and hostname (reverse DNS) discovery
Selectable TCP port scan with service identification
Three interfaces: command line, Tkinter desktop GUI, and a web dashboard
CSV export and a formatted text report
Requirements
Python 3.8 or newer
Tkinter (only for --gui; included with most Python installers. On Debian/Ubuntu: sudo apt install python3-tk)
Quick start
bash
git clone https://github.com/<your-username>/pynet-sonar.git
cd pynet-sonar
python pynet_web.py

The dashboard opens at http://127.0.0.1:8765. Press Scan network and devices appear as blips on the radar. Click a blip or a row to see its details.

Command line
bash
python pynet.py                                   # auto-detects your /24 network
python pynet.py 192.168.1.0/24 -p 22,80,443       # choose ports
python pynet.py 192.168.1.0/24 --csv scan.csv --report report.txt --online-only
python pynet.py --gui                             # desktop window
Option	Description
network	CIDR range to scan. Defaults to your local /24.
-p, --ports	Comma-separated ports. Defaults to 14 common ports.
--csv FILE	Export results to CSV.
--report FILE	Save the text report.
--online-only	Hide addresses that did not answer.
--gui	Launch the Tkinter interface.
Example output
╔══════════════════════════════════════╗
║        PYNET NETWORK SCANNER         ║
╚══════════════════════════════════════╝

Scanning : 192.168.1.0/24

IP Address      Status   MAC                Host                  Open ports
==============================================================================
192.168.1.1     ONLINE   AA:BB:CC:11:22:33  router.lan            53/DNS, 80/HTTP
192.168.1.10    ONLINE   AA:BB:CC:44:55:66  desktop.lan           445/SMB, 3389/RDP
192.168.1.20    OFFLINE  -                  Unknown
==============================================================================
Scan completed: 254 addresses checked, 2 online, 252 offline (6.1s)
Project layout
pynet-sonar/
├── pynet.py        # scanner engine, CLI and Tkinter GUI
├── pynet_web.py    # web dashboard (serves the HTML/CSS/JS UI)
└── README.md
How it works
Step	Technique	Module
Expand the range	CIDR parsing	ipaddress
Detect online devices	ICMP ping, run in parallel	subprocess, concurrent.futures
Find MAC addresses	Read the OS ARP cache	subprocess, re
Find hostnames	Reverse DNS lookup	socket
Scan ports	TCP connect check	socket
Web dashboard	Local HTTP server with JSON API	http.server
Export	CSV and text report	csv
Limitations
MAC addresses are only visible for devices on your own subnet, because ARP does not cross routers.
Devices that block ping may show as offline even though they are connected.
Hostnames appear only if your DNS or the device provides one.
The web dashboard limits scans to /22 ranges (1,024 addresses) and listens on 127.0.0.1 only.
Roadmap
 MAC vendor lookup (Apple, TP-Link, and so on)
 Scan history with "new device joined" alerts
 TCP fallback for devices that block ping
License

MIT. Add a LICENSE file before publishing.
