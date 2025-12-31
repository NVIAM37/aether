# 📍 AETHER: Project Documentation (V2.0.0)

**Project Name:** AETHER (Advanced Entity Tracking & Heuristic Evaluation Resource)
**Version:** 2.0.0 (Pathfinder Update)
**Status:** ✅ Production Ready
**Codebase:** V2 (Enhanced)

---

## 📋 Table of Contents

1. [Executive Summary](#executive-summary)
2. [What's Reuse from V1 vs New in V2](#reuse-vs-new)
3. [Architecture & Intelligence Layer](#architecture--intelligence-layer)
4. [Services Breakdown](#services-breakdown)
5. [Frontend (Pathfinder Module)](#frontend-pathfinder-module)
6. [Backend (Socket Core)](#backend-socket-core)
7. [Installation & Deployment](#installation--deployment)

---

## 🎯 Executive Summary

**Aether V2** is a significant leap forward from the original "Real-Time Tracker". While the core premise (socket-based location sharing) remains, V2 introduces an **Intelligence Layer** that processes raw telemetry into actionable insights (GPS confidence, speed trends, connectivity health). The UI has been completely overhauled into a "Glassmorphic Sci-Fi" interface designed for high-visibility operational contexts.

---

## 🔄 Reuse vs New

| Feature             | V1 (Original)               | V2 (Aether)                             |
| :------------------ | :-------------------------- | :-------------------------------------- |
| **Core Connection** | Socket.IO Basic             | Socket.IO + **Offline Buffering**       |
| **UI Design**       | Standard Material/Bootstrap | **Custom Glassmorphism + Hex Grid**     |
| **Map Engine**      | Leaflet Basic               | Leaflet + **OSRM Routing & Waypoints**  |
| **Data Flow**       | Direct Pass-through         | **Heuristic Processing (Alert Engine)** |
| **Mobile Support**  | Responsive CSS              | **Dedicated "Agent Mode" + QR Pairing** |

---

## 🧠 Architecture & Intelligence Layer

Aether V2 introduces two client-side services that process data _before_ visual rendering.

### 1. The Alert Engine (`AlertEngine.js`)

Instead of just showing markers, Aether evaluates telemetry to generate Alerts.

- **Rules Evaluated:**
  - **Speed**: Detects abnormal velocity (e.g., > 100km/h).
  - **Proximity**: Alerts if a target is within 50 meters ("ARRIVAL IMMINENT").
  - **Signal Loss**: Flags if a user hasn't updated in > 10 seconds.

### 2. The Offline Buffer (`OfflineBuffer.js`)

Mobile networks are unstable. Aether V2 implements a "Store-and-Forward" mechanism.

- **Offline**: If the socket disconnects, location packets are pushed to an in-memory queue.
- **Recovering**: When connection restores, the buffer flushes all stored points to the server in a batch, ensuring no path data is lost.

### 3. Connection Heuristics

The HUD displays a computed "Link Quality" status:

- **LIVE**: Socket connected, latency < 200ms.
- **SYNCING**: Socket connected, buffer flushing.
- **OFFLINE**: Socket disconnected, buffer accumulating.

---

## 📡 Services Breakdown

### **Frontend Services**

#### `OfflineBuffer.js`

```javascript
// Queues payloads when socket is down
add(payload) {
    if (this.queue.length < this.maxSize) {
        this.queue.push(payload);
    }
}

// Flushes to server on reconnect
flush(socket) {
    while(this.queue.length > 0) {
        const payload = this.queue.shift();
        socket.emit('update-location', payload);
    }
}
```

#### `AlertEngine.js`

```javascript
// Evaluates raw inputs into UI Alerts
evaluate(agentData, routeInfo, destPos) {
    // Logic to check distances, speeds, and signal ages
    return alerts; // [{ level: 'danger', message: 'SIGNAL LOST' }, ...]
}
```

---

## 🖥️ Frontend (Pathfinder Module)

**File:** `src/components/Home/PathFinder.jsx`
The "Brain" of the operation. This single component manages:

1. **Map Layer**: Rendering tiles and markers.
2. **Routing Machine**: Calculating OSRM routes between User & Target.
3. **HUD Overlay**: Displaying the stats (Speed, Connection, Alerts) on top of the map.
4. **QR Generator**: Creating dynamic join links for mobile agents.

**Key UI Elements:**

- **Control Panel (Left)**: Collapsible glass panel with Search, Agent List, and Connectivity Status.
- **Map View (Right)**: Full-screen interactive map with animated "FlyTo" focus.
- **Floating Nav Box**: Pop-up turn-by-turn directions that appear when a route is active.

---

## ⚙️ Backend (Socket Core)

**File:** `backend/server.js`
The backend has been optimized for **Room Isolation**.

- **`join-room`**: Users join a specific room via URL query param (`?room=xyz`).
- **Data Privacy**: Updates (`update-location`) are broadcast _only_ to `socket.to(room)`.
- **Legacy Support**: Maintains the `/api/user-distances` endpoint for the dashboard view, but the primary action happens via Sockets.

**Socket Events:**

- `agent-active`: A new user/agent announces presence.
- `start-tracking`: Host initiates a tracked session.
- `agent-update`: Server broadcasts the latest state of all agents in the room.

---

## 🚀 Installation & Deployment

### Recommended: Docker (V2)

Aether V2 is stateless and container-ready.

```dockerfile
# Simlified Dockerfile
FROM node:18-alpine
WORKDIR /app
COPY . .
RUN npm install
CMD ["node", "backend/server.js"]
```

### Manual Run

1. **Backend**: `npm run dev` (Port 5000)
2. **Frontend**: `npm run dev` (Port 5173)
3. **LAN Access**: Use `http://<YOUR_PC_IP>:5173` on mobile devices to act as Agents.
