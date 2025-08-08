# FungiVue: Ultrasound Mushroom Growth Imaging Kit

## Overview
An open-source, cost-efficient DIY kit for non-invasive ultrasound imaging of mushroom (mycelium) growth. Uses affordable hardware (<$200 wholesale via AliExpress) for educational biohacking. Ideal for drop shipping on Shopify—high margins (50-100%) with educational appeal. Integrates RPi Zero 2W duo-cluster, Arduino, transducers, and lightweight ML for growth predictions. MIT licensed for easy customization.

**Key Benefits**:
- **Cost Efficiency**: Bulk source parts for $150-180;
- **Modularity**: Grove connectors for plug-and-play; expandable (e.g., add cameras).
- **Innovation**: Multi-modal (ultrasound + bio-electric) with AI insights.
- **Educational**: Teaches ultrasound, mycology, coding; great for STEM.


## Materials (See docs/parts_list.md for details)
- Raspberry Pi Zero 2W (x2): $30
- Arduino Uno/Nano: $10
- Ultrasonic Transducers: $10-20
- Grove Sensors/Connectors: $18
- Other: $80 (enclosure, power, misc.)

Total: ~$150 (scale for bulk discounts).

## Assembly
1. Stack RPi duo via USB hub.
2. Attach Grove shield to Arduino; plug sensors/transducers.
3. Place in chamber with substrate.
4. Connect Arduino to RPi via USB.

No soldering—keeps costs low for users.

## Software Setup
- Arduino: Upload src/arduino/ultrasound_sketch.ino.
- Python: Run src/python/imaging_script.py on RPi.
- NixOS: Use nix/configuration.nix (customize with scripts/prepare-config.sh).
- Other Linux: scripts/install-linux-deps.sh.
- Docker: Build from docker/Dockerfile.

## Usage
1. Inoculate substrate.
2. Run script: Generates time-lapse images/predictions.
3. Analyze: CSV exports for graphs.

## Troubleshooting
- Weak signals: Add gel.
- ML issues: Fallback to basic mode.

## Contributions
Fork and PR—focus on cost reductions (e.g., cheaper transducers).

## License
MIT—free for commercial use (e.g., Shopify bundles).
