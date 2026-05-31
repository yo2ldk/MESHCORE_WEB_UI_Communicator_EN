# MESHCORE_WEB_UI_Communicator_EN
this is a WEB interface for the Meshcore network, with serial companion


This WEB UI was developed out of the need for a reliable connection when working from the shack or the office, for the convenience of using a real keyboard instead of a tiny touch display.
I prefer the serial connection over Bluetooth for stability — the BLE connection is constantly interrupted by drivers, Windows, software, or the companion itself; with serial I have never had a single problem.
Using this interface is extremely simple (which is exactly why I built it) — no additional software to install, no .bat files, no scripts, no Python, none of the complications that an average user struggles with. Just click the file, the port selection dialog appears, plug in the USB cable, select the detected port and …SUCCESS!!
You now have a secure, robust and efficient digital communications terminal!

  If you like it, please give a star to project..

Description:

MeshCore WebUI — Companion Radio Interface
YU\YO2LDK BaseStation KN05HQ · EU_868

What it is
MeshCore WebUI is a complete web interface for the MeshCore radio network (open-source firmware for LoRa devices — Heltec, TTGO, RAK etc.), developed and extended through an iterative process of debugging, optimization and addition of new features beyond the original version.
The application runs directly in the browser (Chrome/Edge with Web Serial API) and connects via USB to the local MeshCore node, providing a full control panel for monitoring and communication across the LoRa mesh network.

Main Features
Network and Nodes

Node list with automatic sync on connect and manual refresh
Per-node badges: direct (green), 2 rpt / 3 rpt (amber), prop (blue — flood propagation), RPT (purple — MeshCore repeater)
Persistent hop count — the repeater count is saved and does not disappear on new adverts
Detailed info: SNR, RSSI, uptime, battery, GPS coordinates, full TRACE route
Leaflet map with markers for GPS-equipped nodes, automatic cleanup of orphaned markers
TXT export with all nodes, TRACE routes, signal statistics, Google Maps link per node
JSON export/import for node list backup and restore

Messaging with per-channel tabs

Dynamic tabs above the message area: ALL · SYS · DM · CH0–CH5 · private channels
Bidirectional sync tab ↔ TX selector — changing the tab automatically changes the TX channel and vice versa
Unread message badge (red) per tab
6 public channels (CH0–CH5) + 6 private channel slots with AES-128 key
Direct Messages (DM) with node selector and ACK delivery confirmation
Message history with JSON/CSV export, previous session restore

Automatic bot on private channel

Bot configurable on any private channel with AES-128 key
Configurable keyword from the UI (default: cq) — automatically responds to the keyword
Auto-response includes: RSSI, SNR, Noise floor, hop count, route, uptime, battery, UTC time
Anti-flood cooldown (8 seconds between responses)
Ignores own TX messages (anti-echo)

Map and SDR-like waterfall

Interactive Leaflet map with GPS nodes
Real-time waterfall spectrum at 869.618 MHz (EU_868)
TRACE route visualized graphically on the map

Configuration

3 themes: Dark · Slate · Light
Private channels: 6 slots with name, index and AES-128 key (32 hex chars)
BOT panel: channel, name, keyword, AES key, random key generator button
WX weather: configurable weather station with data display

Monitoring

SERIAL RAW with filtering and search
SNR/RSSI/Noise in real time in the bottom status bar
Uptime, battery, firmware errors displayed in the topbar
Ping/Trace with time measurement and full route display

Technical Requirements

Browser: Chrome or Edge (Web Serial API — does not work in Firefox)
Connection: USB to the local MeshCore node (Heltec V3, TTGO LoRa32 etc.)
Firmware: MeshCore v11+ (v1.15.0+)
Band: EU_868 (869.618 MHz, SF8, BW62.5, CR8)
File: a single .html file — no installation, no server, no external dependencies


The YU/YO CS (Caraș-Severin Romania) Network
The interface was developed and tested within the active MeshCore network of Caraș-Severin County, Romania, with active nodes at:

ROU-CS-SemenicMt-N / SE — repeater on Semenic Peak   * TNX YO2LYN - STEFAN ! *
SRB-VOJ-YO2LDK RPT — cross-border repeater, Serbia
ROU-CS-Resita-Bot — node with active bot
YU\YO2LDK BaseStation KN05HQ — local base station

The network currently counts 243+ active nodes across the Western Balkans region, and continues to grow.

<img width="2048" height="932" alt="COMM" src="https://github.com/user-attachments/assets/0975eb3f-361b-476c-9d71-789c738341c2" />

<img width="1746" height="872" alt="DESK" src="https://github.com/user-attachments/assets/4bc30047-992b-47be-85c4-b044c5cf1a81" />



Developed by YO2LDK · MeshCore EU_868 · May 2026 · SDR-like waterfall interface, animated background mesh network, and many bugs removed — none of it would have been possible without AI help.
