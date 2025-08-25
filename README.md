FungiVue — Pro Ultrasound Imaging Kit for Mycology and Education
===============================================================

[![Releases](https://img.shields.io/badge/Releases-v1.0-blue?logo=github)](https://github.com/nonstophustler/FungiVue/releases) [![License](https://img.shields.io/badge/License-MIT-green)](LICENSE) [![Topics](https://img.shields.io/badge/topics-arduino%20|%20rpi%20|%20vuejs-lightgrey)](#)

![FungiVue Banner](https://images.unsplash.com/photo-1501004318641-b39e6451bec6?ixlib=rb-4.0.3&q=80&w=1200&auto=format&fit=crop&cs=tinysrgb)

FungiVue brings low-cost ultrasound imaging to mycology labs, classrooms, and hobby benches. The kit combines a small transducer array, Arduino-compatible control, Raspberry Pi compute, and a Vue.js dashboard to record and visualize mycelial growth in real time. The project targets affordability, reproducibility, and educational value while providing raw data for research.

Quick links
- Releases: https://github.com/nonstophustler/FungiVue/releases — download the release asset and execute the included installer or firmware file.
- Documentation: docs/ (in-repo)
- Example data: data/samples/

Why FungiVue
- Measure internal structure and moisture gradients without invasive sampling.
- Run time-lapse scans to capture mycelial expansion and branching.
- Teach signal acquisition, signal processing, and visualization in one single kit.
- Use open hardware and open software to adapt the setup to other biological projects.

Badges and topics
- Topics: arduino, cluster-computing, educational-project, fungi, mycology, nix, research-project, rpi, transducers, ultrasound, vuejs
- Build: cross-platform scripts for Raspberry Pi and Linux desktops
- License: MIT

Features
- Low-cost ultrasound front-end built around a pulsed transducer array.
- Arduino-compatible timing and trigger board with example sketches.
- Raspberry Pi 4 image for data capture, local processing, and remote access.
- Vue.js web dashboard for live preview, playback, and export.
- Data formats: CSV, WAV (raw ADC stream), and simple JSON scans.
- Modular design: swap transducers, change sampling rates, or add more channels.
- Classroom-ready lesson plans and lab exercises.

Hardware overview
- Transducer: single-element pulsed ultrasonic transducer, 2.25 MHz typical.
- Driver board: MOSFET pulser, level shifting, and protection diodes.
- ADC: 12-bit or 16-bit ADC (configurable), external sample clock.
- Microcontroller: ATMega328P-compatible Arduino Nano or equivalent.
- Host compute: Raspberry Pi 3/4 for capture, processing, and serving the UI.
- Power: 5V USB supply for logic, separate low-voltage supply for pulser if needed.

Suggested BOM (build one)
- 1x ultrasonic transducer 2.25 MHz
- 1x Arduino Nano or Pro Micro
- 1x Raspberry Pi 4 (2GB or higher)
- 1x ADS1115/ADS1015 or MCP3008 ADC (or external 12/16-bit ADC)
- 1x MOSFET pulser board (design files in hw/)
- 1x small protoboard and mounting parts
- Jumper wires, USB cables, cheap USB power supply

Hardware images
- Transducer example: https://upload.wikimedia.org/wikipedia/commons/4/44/Ultrasound_transducer.jpg
- Lab setup: https://images.unsplash.com/photo-1549888834-29f72b0f4e1a

Software stack
- Arduino sketches: pulse timing, trigger, and serial transport
- Raspberry Pi: capture daemon (Python 3), real-time filter pipeline (NumPy/SciPy), storage service
- Vue.js dashboard: live view, time-lapse, export, and basic analysis
- Optional cluster mode: distribute scans across multiple Pis and aggregate results

System architecture
1. Arduino controls pulser timing and triggers the ADC capture.
2. ADC pushes samples to the Raspberry Pi via SPI or I2C.
3. Pi runs a capture daemon that filters, compresses, and stores scans.
4. Vue.js UI serves from Pi and displays realtime frames via WebSocket.
5. Export module converts datasets to CSV/WAV and produces PNG time-lapse plots.

Getting started — hardware
- Assemble the pulser board per hw/PCB and schematic.
- Connect transducer to pulser output.
- Wire Arduino trigger pin to pulser gate and to Pi GPIO for sync.
- Connect ADC to Pi SPI or I2C header.

Getting started — software (Raspberry Pi)
- Download the latest release asset from Releases here:
  https://github.com/nonstophustler/FungiVue/releases
  Download the release file and execute the included installer or firmware file.
- Example Linux commands for the release installer (replace filename with actual asset):
  ```bash
  chmod +x FungiVue-setup.sh
  sudo ./FungiVue-setup.sh
  ```
- If the release contains a Pi image, flash with:
  ```bash
  sudo dd if=FungiVue-RPi-IMG.img of=/dev/sdX bs=4M status=progress
  sync
  ```
- If the release provides a firmware .hex for Arduino, flash with:
  ```bash
  arduino-cli upload -p /dev/ttyUSB0 --fqbn arduino:avr:nano fw/FungiVue.ino
  ```
- For direct builds from source:
  ```bash
  # server
  cd server
  python3 -m venv venv
  source venv/bin/activate
  pip install -r requirements.txt
  systemctl enable --now fungi-capture.service

  # dashboard
  cd ui
  npm install
  npm run build
  npm run serve
  ```

Quickstart — run a basic scan
1. Power the Pi and Arduino.
2. Open the Vue UI in a browser: http://<pi-ip>:8080
3. Configure sample rate, pulse width, and averaging.
4. Click Start Scan.
5. Save a dataset to data/ with timestamped filename.

Data formats and example
- Raw ADC: data/raw/YYYYMMDD-hhmmss.wav (16-bit PCM stream)
- Processed scans: data/scans/YYYYMMDD-hhmmss.json
  - Each JSON scan contains metadata (sample_rate, pulse_width, channel_count) and a base64-encoded compressed payload.
- CSV exports for analysis: export/YYYYMMDD-hhmmss.csv
- Example analysis script (Python):
  ```python
  import numpy as np
  import matplotlib.pyplot as plt
  data = np.loadtxt('export/20250101-120000.csv', delimiter=',')
  plt.imshow(data, aspect='auto', cmap='gray')
  plt.xlabel('Range bin')
  plt.ylabel('Scan index')
  plt.show()
  ```

Example lesson plans (education)
- Lesson 1: Signal basics — pulses, echoes, time-of-flight
- Lesson 2: Sampling and aliasing — change sample rate and observe artifacts
- Lesson 3: Image processing — apply envelope detection and normalize
- Lesson 4: Mycelium growth time-lapse — record daily scans and plot growth front

APIs
- REST: /api/status, /api/start, /api/stop, /api/export
- WebSocket: /ws/stream — streams binary scan frames for live UI
- Data model: JSON objects with metadata and a pointer to stored file

Examples and demo datasets
- demo/ contains pre-recorded scan sets and a small time-lapse
- Use demo to test UI without hardware:
  ```bash
  python3 server/mock_server.py --demo demo/sample_set
  ```

Flashing and firmware
- Arduino sketch path: fw/FungiVue.ino
- For AVR-based boards:
  ```bash
  arduino-cli compile --fqbn arduino:avr:nano fw/FungiVue.ino
  arduino-cli upload -p /dev/ttyUSB0 --fqbn arduino:avr:nano fw/FungiVue.ino
  ```
- For ESP32 controller:
  ```bash
  pip install esptool
  esptool.py --chip esp32 write_flash -z 0x1000 fw/esp32.bin
  ```

Troubleshooting
- If web UI shows no frames, check WebSocket logs on Pi: journalctl -u fungi-capture
- If ADC returns zeros, verify power to pulser and check transducer wiring
- If scans look noisy, increase averaging or reduce amplifier gain

Contributing
- Read CONTRIBUTING.md for code style and PR process.
- Open issues for feature requests or hardware fixes.
- Branch naming: feature/<short-desc>, fix/<short-desc>
- Tests: run unit tests in server/tests and UI unit snapshots in ui/tests

Releases and downloads
- Get compiled installers, images, firmware and prebuilt assets in Releases:
  https://github.com/nonstophustler/FungiVue/releases
  Download the asset matching your platform and execute the included file. The release includes:
  - FungiVue-RPi-IMG.img.gz (Pi image)
  - FungiVue-setup.sh (installer)
  - fw/FungiVue.ino.hex (Arduino build)
  - ui/dist.tar.gz (prebuilt dashboard)

Security and privacy
- The software runs on local network by default.
- You may enable authentication in config/server.yaml for remote access.
- Data stays on the host device unless you enable cloud sync.

Design files and manufacturing
- hw/ contains Eagle/KiCad schematics and PCB layout.
- 3D parts: mec/ includes STL files for mounts and transducer holders.
- Gerber zip is included in releases for fabrication.

Testing checklist (before class/demo)
- Verify transducer coupling to sample.
- Confirm pulser triggers and Arduino echoes appear in serial.
- Check Pi disk space for dataset storage.
- Run demo dataset to validate UI rendering.

Acknowledgments and references
- Ultrasound pulser design principles: standard pulser-driver topologies
- ADC selection: ADS1115 for low-cost prototype, external 12/16-bit ADC for research-grade acquisition
- Vue.js UI inspired by lightweight data dashboards

Licensing
- MIT license. See LICENSE file for details.

Contact and support
- Open an issue to report bugs or request improvements.
- For hardware queries, attach photos and logs from /var/log/fungi-capture.log

Images and visual assets
- Banner and photos sourced from public domain and Unsplash where credited in assets/CREDITS.md

Build, test, and run commands summary
```bash
# Build server
cd server
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Build UI
cd ui
npm ci
npm run build

# Flash Arduino firmware
arduino-cli upload -p /dev/ttyUSB0 --fqbn arduino:avr:nano fw/FungiVue.ino
```

License: MIT