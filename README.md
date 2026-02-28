# VW-MQB-Wireless-CAN-Bridge-Security-Research
VW Golf Sportswagon 2016 "Car Hacking"
# VW MQB Wireless CAN Bridge & Security Research
**Hardware:** ESP32, SN65HVD230 Transceiver  
**Target Vehicle:** 2016 VW Golf Sportswagon (MQB Platform)  
**Security Focus:** Authenticated Packet Injection & Signal Reverse Engineering

---

## 🛠 Project Methodology

<details>
<summary><b>Phase 1: Research & Physical Layer Access</b></summary>
<br>
  
* **Environment:** Configured a Kali Linux workstation with `can-utils`, `SavvyCAN`, and `Wireshark` for deep-packet inspection.
* **Access Point:** Identified and tapped the <b>Infotainment CAN</b> bus (High/Low) via the MIB2 Quadlock connector (Pins 12 & 6) behind the glovebox head unit.
* **Hardware Bridge:** Interfaced an ESP32 microcontroller with an SN65HVD230 transceiver to bridge the 500kbps differential CAN signals to a wireless 802.11 stack.
</details>

<details>
<summary><b>Phase 2: Signal Reverse Engineering (Sniffing)</b></summary>
<br>

* **Data Acquisition:** Utilized `candump` and `cansniffer` to monitor real-time bus traffic during specific user actions (e.g., door locks, steering wheel volume adjustment).
* **ID Mapping:** Cross-referenced proprietary hexadecimal IDs with the **OpenDBC** (comma.ai) VW repository to isolate IDs like `0x290` (Comfort/Convenience) and `0x390` (Lighting/Signals).
* **Payload Isolation:** Conducted "Divide and Conquer" analysis on captured log files to determine specific byte-shifts responsible for vehicle responses.
</details>

<details>
<summary><b>Phase 3: Wireless Implementation & Firmware</b></summary>
<br>

* **Firmware:** Developed C++ firmware utilizing the `arduino-CAN` library to initialize the ESP32's built-in TWAI (Two-Wire Automotive Interface) controller.
* **Wireless Protocol:** Built a WebSocket/REST API bridge, allowing the vehicle to be controlled via a custom smartphone interface.
* **Authenticated Injection:** To mitigate "War Driving" risks, I implemented a **Challenge-Response** authentication layer, ensuring the car only accepts CAN frames from an authorized, cryptographically-verified device.
</details>

<details>
<summary><b>Phase 4: Results & Security Findings</b></summary>
<br>

* **Proof of Concept:** Successfully demonstrated remote wireless triggering of door locks and turn signal sequences via a custom Python (Kivy) mobile app.
* **Vulnerability Analysis:** Demonstrated that physical access to internal wiring allows for a total bypass of the OBD-II "Gateway" firewall commonly found on 2016+ VW MQB models.
* **Portfolio Proof:** Documentation includes logic analyzer captures and packet
