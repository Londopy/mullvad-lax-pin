# mullvad-lax-ping

A Python desktop tool for benchmarking Mullvad VPN's WireGuard servers in the Los Angeles region. Pings all 31 LAX servers in parallel, visualizes latency statistics with Seaborn/Matplotlib, and plots datacenter locations on an interactive map.

![Python](https://img.shields.io/badge/python-3.8%2B-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Platform](https://img.shields.io/badge/platform-Windows%20%7C%20macOS%20%7C%20Linux-lightgrey)

---

## Features

- **Live results table** — pings all 31 `us-lax-wg-*` servers sequentially, showing avg/min/max latency, packet loss, and a color-coded status per server
- **Statistics dashboard** — five Seaborn/Matplotlib charts auto-generated after each test:
  - Sorted bar chart with min/max error bars across all servers
  - KDE latency distribution curves per datacenter group
  - Box plot comparing quartiles across groups
  - Min vs. Max scatter plot (highlights jittery servers)
  - Packet loss heatmap across the full server grid
- **Static in-app map** — Matplotlib scatter plot of the five LA-area datacenter locations, with bubble size scaled to server count and color updated by live ping results
- **Interactive browser map** — opens a Folium/Leaflet dark map in your browser with zoomable datacenter circles, individual server dots, hover tooltips, and click popups
- **Summary stat cards** — best server, worst server, average latency, and reachable server count shown at a glance
- **Cross-platform** — adjusts `ping` flags automatically for Windows (`-n`) and macOS/Linux (`-c`)

---

## Screenshots

> Run the tool and screenshots will appear here — contributions welcome!

---

## Requirements

- Python 3.8 or newer
- The following packages (all installable via pip):

```
tkinter       (included with most Python distributions)
matplotlib
seaborn
pandas
numpy
folium
```

---

## Installation

### Option A — GUI Installer (recommended for most users)

Download `MullvadPingInstaller.exe` from the [Releases](../../releases) page and run it. No Python required — the installer bundles its own runtime and handles everything.

The installer will:
- Detect whether Python 3 is already installed on your system
- Offer to download the official Python installer if it isn't
- Install all required packages (`matplotlib`, `seaborn`, `pandas`, `numpy`, `folium`) via pip
- Let you choose an install directory
- Create a launcher script (`.bat` on Windows, `.sh` on macOS/Linux)
- Optionally create a desktop shortcut and Start Menu entry
- Launch the app automatically when done

### Option B — Manual install

```bash
# 1. Clone the repo
git clone https://github.com/YOUR_USERNAME/mullvad-lax-ping.git
cd mullvad-lax-ping

# 2. Install dependencies
pip install matplotlib seaborn pandas numpy folium

# 3. Run
python mullvad_ping.py
```

> **Note for Linux users:** If tkinter is missing, install it via your package manager:
> ```bash
> sudo apt install python3-tk      # Debian / Ubuntu
> sudo dnf install python3-tkinter # Fedora
> ```

### Building the installer exe yourself

Requires Python and PyInstaller:

```bash
pip install pyinstaller
pyinstaller --onefile --windowed --name "MullvadPingInstaller" installer.py
```

The resulting exe will be in the `dist/` folder. Place `mullvad_ping.py` in the same folder as the exe before distributing, or users can select "Download from GitHub" inside the installer.

---

## Usage

1. Launch the script — a dark-themed window opens.
2. Click **Run Ping Test** in the top-right corner.
3. The tool pings each server one at a time (4 packets per server). A progress bar and live status line show which server is currently being tested.
4. Once complete:
   - The **Results** tab shows the full table, sortable by any column.
   - The **Statistics** tab renders five charts automatically.
   - The **Map** tab updates the static LA-area scatter map with live latency data.
   - The **Open Interactive Map** button opens a Folium map in your default browser.
5. Click **Run Again** to re-run the test at any time.

### Interpreting results

| Color | Meaning |
|-------|---------|
| 🟢 Green | Excellent — under 30 ms |
| 🔵 Cyan | Good — 30–70 ms |
| 🟡 Yellow | Slow — 70–120 ms |
| 🔴 Red | High latency — over 120 ms |
| ⚫ Grey | Unreachable / 100% packet loss |

### Datacenter groups

Mullvad's LAX WireGuard servers are grouped by their numbering prefix, each corresponding to a different LA-area carrier-neutral facility:

| Group | Servers | Approximate location |
|-------|---------|----------------------|
| LAX-A | 001–008 | CoreSite LA1, Downtown LA |
| LAX-B | 201–203 | Equinix LA3, El Segundo |
| LAX-C | 402–409 | Zayo Burbank, Media District |
| LAX-D | 601–608 | Equinix LA4, Irvine / Orange County |
| LAX-E | 701–704 | CoreSite LA2, Long Beach |

> Datacenter assignments are inferred from Mullvad's public infrastructure and may not reflect their exact internal routing.

---

## Extending the tool

**Adding more servers:** Edit the `SERVERS` list at the top of `mullvad_ping.py`. The rest of the tool adapts automatically.

**Other Mullvad regions:** Replace or extend `SERVERS` with servers from other regions (e.g. `us-nyc-wg-*`) and update `DATACENTER_INFO` with the appropriate coordinates and datacenter names.

**Changing ping count:** Modify the `count=4` default in `ping_server()`.

---

## Notes

- This tool uses the system `ping` command and does not require root/administrator privileges.
- No data leaves your machine — all pings go directly from your system to Mullvad's servers.
- This project is not affiliated with or endorsed by Mullvad VPN AB.

---

## License

MIT — see [LICENSE](LICENSE) for details.
