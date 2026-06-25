# Human Density Detection System: Radar-Based Occupancy Tracking & Analytics

An enterprise-grade, IoT-enabled human occupancy monitoring and spatial analysis platform. Leveraging millimeter-wave RD03D radar sensors, ESP32 microcontrollers, MQTT telemetry pipelines, and a reactive canvas dashboard to track and visualize real-time occupant coordinates, density thresholds, and historical space utilization patterns.

---

## Executive Summary
The Human Density Detection System is designed to solve real-time workspace optimization, safety occupancy enforcement, and energy automation needs. By utilizing high-precision RD03D radar sensors, the platform captures absolute human coordinate locations inside designated zones. The ingestion pipeline processes raw sensor telemetry through a secure MQTT broker, logs time-series data to MongoDB, and pushes live updates via Socket.io to a high-performance React-Konva canvas dashboard, enabling sub-second tracking and event-driven alerting.

---

## System Architecture

The platform utilizes a decoupled IoT ingestion and web presentation design:

```
                  +-----------------------------------+
                  |          React Frontend           |
                  |     (Vite + Konva + Recharts)     |
                  +-----------------+-----------------+
                                    ^
                                    | WebSockets (Socket.io)
                                    v
                  +-----------------+-----------------+
                  |       Node.js Express Server      |
                  +--------+-----------------+--------+
                           |                 ^
      Queries occupancy    |                 | MQTT Telemetry
      & alert logs         v                 |
                  +--------+--------+  +-----+-----------------+
                  |  MongoDB Atlas  |  |      MQTT Broker      |
                  | (Mongoose ORM)  |  |      (Mosquitto)      |
                  +-----------------+  +-----------+-----------+
                                                   ^
                                                   | MQTT Publish
                                                   | (Coordinates)
                                       +-----------+-----------+
                                       |     ESP32 Gateway     |
                                       +-----------+-----------+
                                                   ^
                                                   | Serial/GPIO
                                       +-----------+-----------+
                                       |     RD03D Sensors     |
                                       +-----------------------+
```

### Decoupled Service Components
* **Fidelity Dashboard (frontend/)**: A responsive React SPA engineered with Vite, React Router DOM, and Outfit typography. Built with React-Konva to dynamically map physical space boundaries and render occupancy positions based on real-time coordinate offsets.
* **API Gateway & Telemetry Broker (backend/)**: Node.js Express server administering MQTT client subscriptions, client connections via Socket.io, Firebase Admin SDK user authorization, and system management APIs.
* **IoT Telemetry Broker (Mosquitto)**: High-throughput message queuing infrastructure routing sensor telemetry from physical ESP32 modules to the backend gateway.
* **Persistence Layer (MongoDB)**: Scalable database using Mongoose ODM to track static room dimensions, coordinate boundaries, transient live states, historical occupancy trends, and system alerts.

---

## Functional Specifications

### 1. High-Fidelity Spatial Canvas Tracking
Implements an interactive visual grid using `react-konva`. Real-time sensor coordinates (`x_mm` and `y_mm`) received from RD03D radar nodes are scaled proportionally to canvas boundaries, rendering smooth, live indicators of occupant locations, paths, and local clustering.

### 2. Stream-Based IoT Telemetry Pipeline
Ingests telemetry data packets from ESP32 modules over the `building/device/{espId}/radar` MQTT topic. Raw sensor measurements are validated, translated, and broadcast instantly to connected clients via Socket.io to ensure minimal latency.

### 3. Historical Occupancy & Trend Analytics
Aggregates time-series sensor data to compile room-level analytics. Users can visualize workspace utilization, peak density times, and average duration metrics using interactive Recharts graphs, supporting data-driven facilities management.

### 4. Event-Driven Alerting Engine
Monitors live occupancy counts against pre-configured density thresholds. If safety limits are exceeded or sensor telemetry heartbeats fail, the backend logs the violation and triggers real-time alerts to the user dashboard for immediate response.
