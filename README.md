MESHCORE_WEB_UI_Communicator_EN
this is a WEB interface for the Meshcore network, with serial companion

LATEST - V2.1

## Why this exists — and why it's different

This WEB UI was developed out of the need for a reliable connection when working from the shack or the office, for the convenience of using a real keyboard instead of a tiny touch display. I prefer the serial connection over Bluetooth for stability — the BLE connection is constantly interrupted by drivers, Windows, software, or the companion itself; with serial I have never had a single problem.

**The single biggest advantage of this project over other MeshCore clients: zero installation.** No Python, no `.bat` files, no scripts, no dependencies to `pip install`, no companion app, no server, nothing to compile or configure — just **one `.html` file**. This matters most for regular users and beginners who just want to talk on the mesh without becoming a developer first. Every other requirement (`pyserial`, WebSocket bridges, Electron apps, CLI tools) is one more thing that can break on someone's PC and one more reason they give up.

Using this interface is extremely simple (which is exactly why I built it):

1. Open the `.html` file in Chrome or Edge
2. Click **Connect**
3. Plug in the USB cable, pick the detected port
4. **… SUCCESS!!** You now have a secure, robust and efficient digital communications terminal.

If you like it, please give a star to the project.

---

## Description

**MeshCore WebUI — Companion Radio Interface**
YU\YO2LDK BaseStation KN05HQ · EU_868 / US_915

### What it is

MeshCore WebUI is a complete web interface for the MeshCore radio network (open-source firmware for LoRa devices — Heltec, TTGO, RAK, Waveshare RP2040-LoRa etc.), developed and extended through an iterative process of debugging, optimization and addition of new features beyond the original version. The application runs directly in the browser (Chrome/Edge with Web Serial API) and connects via USB to the local MeshCore node, providing a full control panel for monitoring and communication across the LoRa mesh network.

Two ready-to-use builds are provided:
- **EU_868** — 869.618 MHz, SF8, BW62.5, CR8
- **US_915** — 910.525 MHz, SF7, BW62.5, CR5 (USA/Canada preset)

Both are the same interface; only the default radio plan differs. Frequency/SF/CR/BW/TX power can all be changed live from either build (see **Radio Parameters** below), so you're not actually locked into one region.

---

## Main Features

### Network and Nodes

- Node list with automatic sync on connect and manual refresh
- Per-node badges: direct (green), 2 rpt / 3 rpt (amber), prop (blue — flood propagation), RPT (purple — MeshCore repeater)
- Persistent hop count — the repeater count is saved and does not disappear on new adverts
- Detailed info: SNR, RSSI, uptime, battery, GPS coordinates, full TRACE route
- Leaflet map with markers for GPS-equipped nodes, automatic cleanup of orphaned markers
- TXT export with all nodes, TRACE routes, signal statistics, Google Maps link per node
- JSON export/import for node list backup and restore

### Messaging with per-channel tabs

- Dynamic tabs above the message area: ALL · SYS · DM · CH0–CH13 · private channels
- Bidirectional sync tab ↔ TX selector — changing the tab automatically changes the TX channel and vice versa
- Unread message badge (red) per tab
- Public channels + 6 private channel slots with AES-128 key
- Direct Messages (DM) with node selector and ACK delivery confirmation
- Message history with JSON/CSV export, previous session restore

### Automatic bot on private channel

- Bot configurable on any private channel with AES-128 key
- Configurable keyword from the UI (default: `cq`) — automatically responds to the keyword
- Auto-response includes: RSSI, SNR, Noise floor, hop count, route, uptime, battery, UTC time
- Anti-flood cooldown (8 seconds between responses)
- Ignores own TX messages (anti-echo)

### Radio Parameters — live SF / CR / BW / Frequency / TX Power (new)

A dedicated **📻 RADIO** panel lets you change the radio's operating parameters directly from the browser, no firmware reflash needed:

- Frequency, Bandwidth, Spreading Factor (SF7–SF12), Coding Rate (CR5–CR8), TX Power
- One-click **EU_868** and **USA/Canada** presets
- Sends the real `CMD_SET_RADIO_PARAMS` (11) + `CMD_SET_RADIO_TX_POWER` (12) companion commands and **waits for the device's actual `RESP_OK`/`RESP_ERR`** before confirming success — no blind "it probably worked"
- One-click **🔁 Reboot Device** (`CMD_REBOOT`, 19): MeshCore firmware persists new radio parameters to NodePrefs but only applies them to the live radio after a reboot — the panel reminds you and can trigger it directly
- Field-tested: switching a EU_868 node live to the US_915 preset and rebooting correctly moved it off 868 MHz and onto 915 MHz

### Region Scope — real per-channel flood filtering (new)

Not just a label: this sends the actual companion-protocol command (`CMD_SET_FLOOD_SCOPE_KEY`, code **54**) that MeshCore repeaters use to decide whether to relay a flood packet.

- Click the **🌐** button next to the channel selector (or set it per-slot in the 🔑 Private Channel panel — both are kept in sync) and type a region name, e.g. `ro-cs`
- The region key is derived exactly the way the firmware does it: the **first 16 bytes of SHA-256("#" + region name)** (verified against `MyMesh.cpp` and the independent `meshcore-decoder` library)
- Right before sending a message on a scoped channel, the WebUI: sets the flood-scope key on the device → sends the message → clears the override back to the device's default, all with `RESP_OK`/`RESP_ERR` confirmation at each step (visible in the RAW panel)
- Only repeaters that are themselves configured for the same region (`region put <name>` / `region allowf <name>` on the repeater's own CLI) will forward that message — everyone else silently drops it. This is genuine network-level segmentation, not client-side filtering.
- **Requirements:** companion firmware v12+ (MeshCore v1.15.0+) with region support. Leave the field blank for classic, unscoped flood (reaches the whole mesh, as before).

### Map and SDR-like waterfall

- Interactive Leaflet map with GPS nodes
- Real-time waterfall spectrum, tracking the live frequency/SF/BW/CR shown in the header (updates instantly if you change Radio Parameters)
- TRACE route visualized graphically on the map

### Configuration

- 3 themes: Dark · Slate · Light
- Private channels: 6 slots with name, index, AES-128 key (32 hex chars) and optional region scope
- BOT panel: channel, name, keyword, AES key, random key generator button
- WX weather: configurable weather station with data display
- Radio panel: frequency / bandwidth / SF / CR / TX power, with EU_868 and USA/Canada presets

### Monitoring

- SERIAL RAW with filtering and search — every command sent and every `RESP_OK`/`RESP_ERR` reply is visible here, useful for verifying Radio Parameters and Region Scope actually landed
- SNR/RSSI/Noise in real time in the bottom status bar
- Uptime, battery, firmware errors displayed in the topbar
- Ping/Trace with time measurement and full route display

---

## Technical Requirements

- **Browser:** Chrome or Edge (Web Serial API — does not work in Firefox)
- **Connection:** USB to the local MeshCore node (Heltec V3, TTGO LoRa32, Waveshare RP2040-LoRa etc.)
- **Firmware:** MeshCore v11+ (v1.15.0+) — v12+ recommended if you want to use Region Scope
- **Band:** EU_868 (869.618 MHz, SF8, BW62.5, CR8) or US_915 (910.525 MHz, SF7, BW62.5, CR5) out of the box — changeable live from the Radio panel
- **File:** a single `.html` file — no installation, no server, no external dependencies


The YU/YO CS (Caraș-Severin Romania) Network
The interface was developed and tested within the active MeshCore network of Caraș-Severin County, Romania, with active nodes at:

ROU-CS-SemenicMt-N / SE — repeater on Semenic Peak   * TNX YO2LYN - STEFAN ! *
🌍 RS-Vojvodina-01 and 
🌍 RS-Vojvodina-02 NW  — cross-border repeater, Serbia
ROU-CS-Resita-Bot — node with active bot
YU\YO2LDK BaseStation KN05HQ — local base station

The network currently counts 777+ active nodes across the Western Balkans region, and continues to grow.

<img width="1919" height="1079" alt="web ui" src="https://github.com/user-attachments/assets/be52b6ef-279e-4ec0-a83e-ea33320ffa5e" />

<img width="1918" height="1076" alt="web ui1" src="https://github.com/user-attachments/assets/f7b30248-6771-49a3-9569-0d7b73e29d2f" />


<img width="2048" height="932" alt="COMM" src="https://github.com/user-attachments/assets/0975eb3f-361b-476c-9d71-789c738341c2" />

<img width="1746" height="872" alt="DESK" src="https://github.com/user-attachments/assets/4bc30047-992b-47be-85c4-b044c5cf1a81" />



Developed by YO2LDK · MeshCore EU_868 · May 2026 · SDR-like waterfall interface, animated background mesh network, and many bugs removed — none of it would have been possible without AI help.
