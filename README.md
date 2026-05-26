# Human Density Detection System

A Radar-Based Human Density Detection and Monitoring System with a Web Control Interface, designed for real-time occupancy tracking and alerting using the **RD03D Sensor**.

---

## 🚀 Key Features

- **Real-time Monitoring**: Instant updates of people count and location within a room.
- **Interactive Map**: A visual rendering of the room using `react-konva` for precise tracking.
- **Attendance & Trends**: Historical data visualization showing occupancy patterns over time.
- **Smart Alerts**: Automatic notifications when density thresholds are crossed or sensors stop sending data.
- **Secure Access**: User authentication managed via Firebase for safe dashboard access.
- **Device Management**: Easily setup and manage multiple rooms and ESP32 devices.

---

## 🛠️ Technology Stack

### Frontend
- **Framework**: React.js (Vite)
- **UI & Visualization**: Recharts (Graphs), React Konva (Map Rendering)
- **State & Routing**: React Router DOM
- **Authentication**: Firebase Authentication
- **Real-time Communication**: Socket.io-client

### Backend
- **Runtime**: Node.js
- **Framework**: Express.js
- **Database**: MongoDB with Mongoose
- **IoT Communication**: MQTT (Message Queuing Telemetry Transport)
- **Real-time Communication**: Socket.io (Server)
- **Authentication**: Firebase Admin SDK

---

## 🏗️ System Architecture

1.  **Sensor Layer**: **RD03D Sensors** connected to an ESP32 device detect human presence and location.
2.  **Data Ingestion**: The ESP32 publishes data to an **MQTT Broker** (e.g., Mosquitto).
3.  **Backend Processing**: The Node.js server subscribes to MQTT topics, processes coordinates, saves logs to **MongoDB**, and triggers alerts.
4.  **Real-time Push**: The backend pushes live updates to the dashboard via **Socket.io**.
5.  **User Dashboard**: React frontend displays the interactive map and occupancy trends.

---

## 📦 Installation & Setup

### Prerequisites
- Node.js (v18+)
- MongoDB (Running locally or on Atlas)
- MQTT Broker (e.g., Mosquitto)
- Firebase Project (for Authentication)

### Backend Setup
1.  Navigate to the `Backend` directory:
    ```bash
    cd Backend
    ```
2.  Install dependencies:
    ```bash
    npm install
    ```
3.  Create a `.env` file and add your credentials:
    ```env
    MONGO_URL=your_mongodb_connection_string
    ```
4.  Start the server:
    ```bash
    npm start 
    ```

### Frontend Setup
1.  Navigate to the `Frontend` directory:
    ```bash
    cd Frontend
    ```
2.  Install dependencies:
    ```bash
    npm install
    ```
3.  Configure Firebase in `src/firebase.js`.
4.  Start the development server:
    ```bash
    npm run dev
    ```

---

## 📡 API Documentation

### Authentication
- `POST /api/auth/register`: Create a new user account.
- `POST /api/auth/login`: Validate credentials.

### Rooms
- `GET /api/rooms`: List all rooms with live occupancy summary.
- `GET /api/rooms/:id`: Detailed data for a specific room.
- `POST /api/rooms`: Create a new room setup.
- `GET /api/rooms/:id/history`: Fetch historical trend data.

### Alerts
- `GET /api/alerts`: Retrieve all system alerts.
- `PUT /api/alerts/:id/acknowledge`: Mark an alert as read.

---

## 🗄️ Database Schemas

### 1. Room
Stores static room configuration (dimensions, ESP32 metadata, sensor positions).
### 2. Live Room
Maintains the ephemeral real-time state (current coordinates of detected people).
### 3. Room History
Stores time-series data for historical occupancy analysis.
### 4. Alerts
Logs threshold violations and sensor connectivity issues.

---

## 🔌 Hardware Integration Notes

This software logic is specifically optimized for the **RD03D Sensor**. While the hardware firmware is not included here, the system is design-ready for:
- **MQTT Integration**: Subscribe to `building/device/{espId}/radar`.
- **Coordinate Mapping**: Handles `x_mm` and `y_mm` data from sensors and maps them to the interactive UI proportionally.
- **Easy Deployment**: The `espId` in the room setup allows for instant pairing with physical devices.

---

