# Wall Drop Benchmark v1.0
by corruptcache

**Physical Link & WAN Throughput Auditor for Shark Jack Display**

A lightweight, zero-storage diagnostic payload designed for rapid infrastructure triage. It audits physical Fast Ethernet negotiation speed and WAN throughput (Download & Upload) using native Hak5 helpers. This payload actively identifies degraded Layer 1 links and confirms Layer 3 routing before you assume a wider network failure.

---

## Features

### 🔌 Layer 1 Hardware Fault Detection
- Bypasses standard logical link checks by querying `swconfig` directly to read the switch0 port 0 link speed.
- Instantly detects if an Ethernet drop has auto-negotiated down to 10 Mbps due to damaged copper, faulty punch-downs, or legacy switch limitations.
- Visual LED overrides (Yellow Slow Blink to Solid) immediately alert the user to a degraded physical link.

### 🚀 Zero-Storage Bandwidth Benchmarking
- Performs a robust WAN download and upload speed test (defaulting to 20MB) against Cloudflare infrastructure.
- Utilizes the Linux `/dev/zero` virtual device to pipe an infinite stream of null characters directly into `curl` for the upload phase, bypassing the Shark Jack's limited flash memory entirely.
- Calculates byte-transfer times natively via `awk` and prints precise Megabit per second (Mbps) metrics directly to the OLED screen.

### 🌐 Native Subnet Discovery
- Automatically extracts and formats the assigned IPv4 CIDR notation (e.g., `192.168.1.x/24`) using native `awk` string manipulation, displaying the exact local subnet alongside the bandwidth results.

---

## Configuration Variables

You can easily adjust the behavior of the payload by modifying the variables at the top of `payload.txt`:

*   `ENABLE_BANDWIDTH_TEST=true`: Set to `false` if you only want to test the Layer 1 negotiation speed.
*   `RESOURCE_TO_PING=8.8.8.8`: The external IP address used to verify outbound Layer 3 routing before starting the throughput test.
*   `BANDWIDTH_TEST_MB=20`: The size of the payload in Megabytes used for both the download and upload benchmarks.

---

## Usage & Display Output

1. Plug the Shark Jack Display into the target wall drop or switch port.
2. The payload will initialize DHCP via native Hak5 helpers and begin physical layer triage.
3. If the physical link is active and the ping test succeeds, it will automatically test outbound WAN speeds.
4. Results are displayed in real-time on the OLED screen.

### Expected OLED Format
```text
Link: 100 Mbps
DL: 84.20 Mbps
UL: 76.50 Mbps
Net: 192.168.1.100/24
```
*(Note: If the link is degraded, the OLED will report "Link: 10 Mbps" or "Link: Offline")*

---

## LED Status Indicators

*   **Magenta (SETUP):** Initializing payload variables and UI.
*   **Yellow (STAGE1):** Execution Phase 1 – Querying Layer 1 switch configuration.
*   **Yellow (Slow Blink to Solid):** Physical Layer 1 fault detected (Link degraded to 10 Mbps).
*   **Yellow (STAGE2):** Execution Phase 2 – Layer 1 healthy, beginning WAN throughput benchmarking.
*   **Green (FINISH):** Fast Ethernet 100 Mbps link established and payload complete.
*   **Red (FAIL / FAIL2):** No physical link detected, or WAN access is completely blocked.

---

## Author

**corruptcache**

## License

Standard Hak5 Payload License
