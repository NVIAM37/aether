# AETHER: Real-Time Location Tracker (V2)

A production-ready, intelligence-driven real-time location tracking system. Aether V2 enhances the original "Real-Time Tracker" with a futuristic "Glassmorphic" UI, predictive intelligence, offline resilience, and integrated pathfinding routing engine.

![Aether UI](https://via.placeholder.com/800x400?text=Aether+Pathfinder+Module)

---

## 🚀 Key Features

### 🧠 Aether Intelligence Layer

- **Contextual GPS Analysis**: Automatically grades GPS signal confidence (High/Medium/Low) based on accuracy metrics.
- **Speed Trend Detection**: Monitors velocity changes to detect acceleration, deceleration, or stable movement.
- **Connection Heuristics**: Distinguishes between "Live", "Recovering (Syncing)", and "Offline" states with recovering buffer counts.

### 📡 Offline Resilience (Smart Buffer)

- **Zero Data Loss**: When connectivity drops, Aether buffers location packets locally.
- **Auto-Sync**: Automatically flushes and synchronizes stored telemetry to the server upon reconnection.

### 🗺️ Integrated Pathfinding

- **OSRM Routing Engine**: Built-in turn-by-turn navigation (Distance, ETA, and Maneuvers).
- **Dynamic Rerouting**: Real-time route updates based on user movement towards the destination.

### 📱 Mobile Agent Ecosystem

- **Instant Pairing**: Generate QR codes for instant mobile-to-desktop bridging.
- **Guest Mode**: Mobile devices can join as "Agents" simply by scanning the host's QR code.

---

## 🏗️ Architecture

### **Frontend (Vite + React)**

- **Core Module**: `PathFinder.jsx` (Interactive Map & HUD)
- **Visual Style**: "Hex-Grid" Sci-Fi styling with Glassmorphism and Neon accents.
- **State Management**: React `useState` + `useRef` for high-frequency telemetry.
- **Maps**: Leaflet + `leaflet-routing-machine` + OSRM.

### **Backend (Node.js + Socket.IO)**

- **Room Isolation**: Strict session isolation for privacy (users in Room A cannot see Room B).
- **Event-Driven**: Socket.IO for sub-second latency updates (`agent-active`, `update-location`).
- **Telemetry Processing**: Server-side distance calculation (Haversine) and state management.

---

## 🚀 Getting Started

### **Prerequisites**

- Node.js 18+
- Modern Browser (Chrome/Edge/Firefox) with Geolocation API.

### **Installation**

1. **Clone the repository:**

   ```bash
   git clone <your-repo-url>
   cd aether-tracker
   ```

2. **Install Dependencies:**

   ```bash
   # Backend
   cd backend
   npm install

   # Frontend
   cd ../frontend
   npm install
   ```

3. **Running the System:**

   **Backend:**

   ```bash
   cd backend
   npm run dev
   # Runs on http://0.0.0.0:5000 (Accessible via LAN)
   ```

   **Frontend:**

   ```bash
   cd frontend
   npm run dev
   # Runs on http://localhost:5173
   ```

4. **Accessing the UI:**
   - Open `http://localhost:5173/pathfinder`
   - To connect a mobile agent, click "Add New Agent" and scan the QR code.

---

## 📦 Project Structure

```bash
aether-tracker/
├── backend/
│   ├── server.js          # Socket.IO Signalling Server
│   └── ...
├── frontend/
│   └── src/
│       ├── components/
│       │   └── Home/
│       │       └── PathFinder.jsx  # CORE MODULE: Map & Intelligence
│       ├── services/
│       │   ├── AlertEngine.js      # Logic for smart alerts
│       │   └── OfflineBuffer.js    # Data resilience service
│       └── ...
└── README.md
```

---

## 🤝 Legacy & Credits

This project (AETHER V2) is the advanced evolution of the original _Real-Time Tracker_.

- **Original Concept**: Real-Time Tracker V1
- **V2 enhancements**: Aether Intelligence, Offline Buffer, UI Overhaul.

**Maintained by:** NVIAM
