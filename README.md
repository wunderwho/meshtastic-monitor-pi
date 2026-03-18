Specifically for Raspberry Pi Zero 2 W Pi OS Lite as monitor OS.

For monitoring and administering meshtastic nodes connected by Serial, USB, or TCP to the Pi.

Pi and node powered seperately with a Waveshare UPS 3S & 5V 5A Output, fed by a wall charger powered usbc to usbc to pd trigger 12v output


Meshtastic Monitor Service

This service runs on a Raspberry Pi (tested on Pi Zero 2 W, Pi OS Lite) and continuously monitors a Meshtastic node connected via serial, USB, or TCP.

It collects, normalizes, and logs data from both the local node and remote nodes on the mesh, writing structured .json logs and human-readable .log files.


What the Service Does

At runtime, the service:

- Connects to a Meshtastic interface (serial or TCP)
- Subscribes to all incoming mesh packets
- Routes each packet to a specific module based on its type
- Logs structured data to per-category files
- Applies per-module enable/disable and interval controls


Log Outputs

Each data type is written to its own pair of files:

device.*        Local node device metrics (battery, voltage, uptime, utilization)
environment.*   Local sensor data (BME680: temp, humidity, pressure, gas)
telemetry.*     Remote node device metrics
nodeinfo.*      Remote node identity (name, hardware, role)
position.*      GPS/location data (all nodes, including local)
text.*          Mesh text messages
system.*        Service lifecycle events (start, restart, module load, connect)
unknown.*       Unrecognized but decoded packet types

Each .json file is machine-readable (one JSON object per line)
Each .log file is human-readable with consistent formatting and local timezone


Configuration

All behavior is controlled via:

host_vars/<hostname>.yml


Module Enable/Disable

device_enabled: true
environment_enabled: true
telemetry_enabled: true
position_enabled: true
nodeinfo_enabled: true
text_enabled: true
unknown_enabled: true


Logging Intervals (seconds)

device_interval: 300
environment_log_interval: 300
telemetry_interval: 300
position_interval: 300
nodeinfo_interval: 300

text_interval: 0
unknown_interval: 0

0 = log every event (real-time)
>0 = rate-limited per node


Connection Settings

meshtastic_connection: serial
meshtastic_port: /dev/ttyACM0

For TCP:

meshtastic_connection: tcp
meshtastic_host: <node_ip>
meshtastic_port: <port>


Local Node ID

local_node_id: "!49b44a44"

Used to:
- separate local vs remote data
- prevent duplication across modules


Behavior Summary

- Each module handles exactly one type of Meshtastic packet
- No data overlap between logs
- All logs share the same schema and formatting
- Unknown or unsupported packet types are captured separately
- Logging can be fully tuned (or disabled) per module
- System events are tracked independently from telemetry data

The result is a clean, structured, and extensible monitoring system suitable for long-term logging, analysis, and troubleshooting of a Meshtastic mesh network.
