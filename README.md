For monitoring and administering meshtastic nodes connected by Serial, USB, or TCP to Raspberry Pi. 

Specifically for my planned deployments of Raspberry Pi Zero 2 W Pi OS Lite as monitor OS. 
Pi and node powered seperately with a Waveshare UPS 3S & 5V 5A Output, fed by a wall charger powered usbc to usbc to pd trigger 12v output

Next steps:
1) Standardize logging
2) Generalize UPS logging


Meshtastic Telemetry Pipeline (planned architecture)

Edge Node (per deployment site)

* Hardware: Meshtastic radio + Raspberry Pi
* Pi runs a Meshtastic exporter that reads the radio over serial and converts packets/telemetry into structured JSON events.
* Events include timestamp, node_id, event_type, location, and payload data (RSSI, SNR, battery, environment, etc).

Edge Transport

* Vector agent runs on the Pi.
* Vector reads the JSON event stream, buffers locally, optionally normalizes/enriches fields, and forwards events over HTTPS.

Central Platform

* Grafana Cloud used for ingestion, storage, and dashboards.
* Vector sends logs/metrics to Grafana (Loki/Mimir).
* Grafana provides visualization, maps, time-series graphs, and alerting.

Data Flow
Meshtastic node → Pi exporter → JSON events → Vector agent → Internet → Grafana Cloud → dashboards/analysis

Goal
Centralize telemetry from all deployed nodes to graph RF metrics, node health, geographic distribution, and mesh performance over time.
