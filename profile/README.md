# 🚲 **SpinLock – Intelligent Bike Parking & Anti-Theft System**

## 📜 **About the Project**

**SpinLock** is an IoT-driven Intelligent Bike Parking & Anti-Theft System designed to enhance bike security and solve the problem of secure parking availability in urban areas. The system integrates hardware, software, and cloud infrastructure to provide a comprehensive solution for urban bike parking challenges.

### 🎯 **Key Components:**

* **Embedded IoT Device (ESP32-based)** with GPS tracking, motion detection, and dual connectivity (Wi-Fi + GSM fallback)
* **Mobile App (Flutter)** for bike owners and parking agents to track bikes, receive alerts, and manage parking
* **Admin Dashboard (ReactJS)** for system administrators to manage users, devices, and parking infrastructure
* **Microservices Backend** with event-driven architecture using Apache Kafka and MQTT for real-time IoT communication
* **Docker-Compose Orchestration** for easy deployment and infrastructure management

---

## 🏗️ **System Architecture Overview**

### 📱 **Client Applications**
- **User & Agent Mobile App** ([User_and_Agent_App_Flutter](https://github.com/CO3302Group3/User_and_Agent_App_Flutter))
  - Real-time bike tracking with GPS
  - Motion and theft alerts
  - Parking slot discovery and reservation
  - Multi-role support (bike owners + parking agents)

- **Admin Dashboard** ([Admin-dashboard](https://github.com/CO3302Group3/Admin-dashboard))
  - User and device management
  - Parking agent oversight
  - System monitoring and analytics
  - Event logs and audit trails

### 🔧 **Embedded Hardware**
- **SpinLock Firmware v3** ([SpinLockFirmware_v3](https://github.com/CO3302Group3/SpinLockFirmware_v3))
  - ESP32-based (ESP-IDF v5.3.1)
  - NEO-M8N GPS module for location tracking
  - MPU6050 6-axis accelerometer/gyroscope for motion detection
  - Vibration sensor for theft detection
  - Wi-Fi provisioning with fallback capabilities
  - Solar charging ready
  - FreeRTOS-based task management

### ⚙️ **Backend Microservices**

| Service | Repository | Purpose |
|---------|-----------|---------|
| **API Gateway** | [APIGatewayMicroservice](https://github.com/CO3302Group3/APIGatewayMicroservice) | Single entry point, JWT validation, request routing |
| **User Authentication** | [UserAuthenticationMicroservice](https://github.com/CO3302Group3/UserAuthenticationMicroservice) | Firebase auth, JWT issuance, RBAC |
| **Admin Management** | [AdminManagementMicroservice](https://github.com/CO3302Group3/AdminManagementMicroservice) | Admin operations, user/device/parking management |
| **Device Onboarding** | [DeviceOnboardingMicroservice](https://github.com/CO3302Group3/DeviceOnboardingMicroservice) | Device registration and provisioning |
| **Device Telemetry** | [DeviceTelemetryMicroservice](https://github.com/CO3302Group3/DeviceTelemetryMicroservice) | Real-time GPS and motion data processing |
| **Device Command** | [Device-Command-Microservice](https://github.com/CO3302Group3/Device-Command-Microservice) | Send commands to IoT devices |
| **GeoLocation Service** | [GeoLocationMicroservice](https://github.com/CO3302Group3/GeoLocationMicroservice) | Location-based calculations, geo-fencing |
| **Alert & Event Service** | [AlertAndEventProcesssingMicroservice](https://github.com/CO3302Group3/AlertAndEventProcesssingMicroservice) | Theft detection, motion alerts, event processing |
| **Notification Service** | [NotificationMicroservice](https://github.com/CO3302Group3/NotificationMicroservice) | Push notifications, email, SMS delivery |
| **Parking Slot Service** | [ParkingSlotMicroservice](https://github.com/CO3302Group3/ParkingSlotMicroservice) | Parking availability, reservations, slot management |
| **Health Monitoring** | [HealthMonitoringMicroservice](https://github.com/CO3302Group3/HealthMonitoringMicroservice) | Service and device health tracking |
| **Logging Service** | [LoggingMicroservice](https://github.com/CO3302Group3/LoggingMicroservice) | Centralized logging, auditing, activity tracking |

---

## 🐋 **Deployment & Infrastructure**

### Docker Compose Stack
**Repository:** [SpinLock-Docker-Compose](https://github.com/CO3302Group3/SpinLock-Docker-Compose)

Complete orchestration for local and production deployments featuring:
- **Nginx** reverse proxy with TLS termination
- **Apache Kafka** (KRaft mode) for event streaming
- **Kafka UI** for topic monitoring
- **Eclipse Mosquitto** MQTT broker with WebSocket support
- **PowerShell automation** scripts for Windows environments
- Pre-configured security and networking

**Quick Start:**
```powershell
git clone https://github.com/CO3302Group3/SpinLock-Docker-Compose.git
cd SpinLock-Docker-Compose
.\manage.ps1 start
```

**Access Points:**
- HTTP/HTTPS Entry: `http://localhost` or `https://localhost`
- Kafka UI: `http://localhost:8001`
- MQTT Broker: `mqtt://mqtt-broker:1883` (internal)
- MQTT WebSocket: `ws://localhost/mqtt/`

---

## 🔗 **Communication Architecture**

### Data Flow
```
IoT Device (ESP32) 
    ↓ MQTT
MQTT Broker (Mosquitto)
    ↓ Kafka Producer
Apache Kafka Topics
    ↓ Kafka Consumers
Microservices (Telemetry, Alerts, Parking, etc.)
    ↓ REST/gRPC
API Gateway
    ↓ HTTPS
Mobile App / Web Dashboard
```

### Event Streaming (Kafka Topics)

| Topic Name | Purpose |
|------------|---------|
| `telemetry-data` | GPS coordinates, motion data, battery levels |
| `motion-alerts` | Theft detection, unauthorized movement |
| `geo-fence-alerts` | Geo-fence entry/exit events |
| `notification-events` | Push notifications, emails, SMS triggers |
| `device-status-events` | Device online/offline, health updates |
| `device-onboarding-events` | New device registration |
| `parking-status-updates` | Slot availability changes |
| `user-activity-logs` | User actions, logins, bookings |
| `system-audit-logs` | Security events, API usage |
| `health-check-events` | Service health monitoring |

---

## 🌐 **Tech Stack**

| Layer | Technologies |
|-------|--------------|
| **Frontend - Mobile** | Flutter (Dart) |
| **Frontend - Web** | ReactJS |
| **Backend APIs** | Python FastAPI |
| **Embedded Firmware** | ESP-IDF (C), FreeRTOS |
| **Event Streaming** | Apache Kafka (KRaft mode) |
| **IoT Messaging** | Eclipse Mosquitto (MQTT) |
| **Databases** | PostgreSQL, MongoDB |
| **Reverse Proxy** | Nginx |
| **Caching** | Redis |
| **Containerization** | Docker, Docker Compose |
| **Monitoring** | Kafka UI, Prometheus (planned), Grafana (planned) |
| **Logging** | ELK Stack (Elasticsearch, Logstash, Kibana) |

---

## 🚀 **Getting Started**

### For Developers

1. **Clone the main orchestration repository:**
   ```bash
   git clone https://github.com/CO3302Group3/SpinLock-Docker-Compose.git
   ```

2. **Set up environment variables:**
   ```bash
   cp .env.example .env
   # Edit .env with your configuration
   ```

3. **Start the infrastructure:**
   ```bash
   .\manage.ps1 start-dev  # Windows
   # or
   docker compose -f docker-compose.yml -f docker-compose.dev.yml up -d
   ```

4. **Access services:**
   - API Gateway: `http://localhost/health`
   - Kafka UI: `http://localhost:8001`
   - Admin Dashboard: Configure in docker-compose

### For Firmware Developers

1. **Clone firmware repository:**
   ```bash
   git clone https://github.com/CO3302Group3/SpinLockFirmware_v3.git
   cd SpinLockFirmware_v3
   ```

2. **Install ESP-IDF v5.3.1:**
   Follow [ESP-IDF installation guide](https://docs.espressif.com/projects/esp-idf/en/latest/get-started/index.html)

3. **Build and flash:**
   ```bash
   idf.py set-target esp32
   idf.py build
   idf.py -p <PORT> flash monitor
   ```

---

## 📊 **Project Structure**

```
CO3302Group3/
├── Frontend Applications
│   ├── User_and_Agent_App_Flutter/       # Mobile app for users & agents
│   └── Admin-dashboard/                  # ReactJS admin portal
│
├── Backend Microservices
│   ├── APIGatewayMicroservice/
│   ├── UserAuthenticationMicroservice/
│   ├── DeviceTelemetryMicroservice/
│   ├── ParkingSlotMicroservice/
│   ├── NotificationMicroservice/
│   ├── AlertAndEventProcesssingMicroservice/
│   └── ... (other microservices)
│
├── Embedded Systems
│   └── SpinLockFirmware_v3/              # ESP32 firmware (ESP-IDF)
│
├── Infrastructure
│   ├── SpinLock-Docker-Compose/          # Orchestration & deployment
│   ├── internal-services/                # Shared internal services
│   ├── kafka101/                         # Kafka examples
│   └── mongo_db/                         # MongoDB utilities
│
└── Documentation
    ├── .github/                          # Organization profile (this file)
    └── SpinLock_Documentation_Guide/     # Comprehensive docs
```

---

## 🔐 **Security Features**

- **JWT-based authentication** with Firebase integration
- **Role-Based Access Control (RBAC)** for users and agents
- **TLS/SSL encryption** for all external communications
- **MQTT authentication** for device-to-cloud security
- **API Gateway rate limiting** and request validation
- **Network isolation** via Docker bridge networks
- **Audit logging** for all critical operations

---

## 📡 **Hardware Features**

### Sensors & Components
- **GPS Module:** NEO-M8N for precise location tracking
- **Motion Detection:** MPU6050 6-axis IMU (I2C interface)
- **Vibration Sensor:** GPIO interrupt-based theft detection
- **User Interface:** Physical button for device interaction
- **Power:** Solar charging capability

### Communication
- **Primary:** Wi-Fi with provisioning support
- **Fallback:** GSM module (planned)
- **Protocol:** MQTT over Wi-Fi to cloud broker

---

## 🧪 **Development Resources**

- **Documentation:** [SpinLock_Documentation_Guide](https://github.com/CO3302Group3/SpinLock_Documentation_Guide)
- **Internal Services:** [internal-services](https://github.com/CO3302Group3/internal-services)
- **Kafka Examples:** [kafka101](https://github.com/CO3302Group3/kafka101)

---

## 🎯 **Project Roadmap**

- [x] Microservices architecture design
- [x] ESP32 firmware with GPS and motion detection
- [x] Docker Compose orchestration
- [x] Kafka event streaming backbone
- [x] MQTT broker integration
- [x] User authentication service
- [x] Mobile app (Flutter)
- [x] Admin dashboard (ReactJS)
- [ ] Production deployment with Kubernetes
- [ ] Grafana + Prometheus monitoring
- [ ] GSM fallback implementation
- [ ] Advanced geo-fencing algorithms
- [ ] Payment integration for parking

---

## 🤝 **Contributing**

This is a group project for **CO3302 - Group 3**. Internal team members can contribute by:

1. Following the established coding standards
2. Creating feature branches for new work
3. Submitting pull requests for review
4. Updating documentation alongside code changes
5. Testing locally using Docker Compose before pushing

---

## 📄 **License**

Components use various licenses:
- Firmware: Public Domain / CC0
- Microservices: Check individual repository licenses
- Dependencies: Respect third-party licenses (ESP-IDF, Flutter, etc.)

---

## 📞 **Support & Contact**

For questions or issues related to SpinLock:
- Open an issue in the relevant repository
- Contact the CO3302 Group 3 team members
- Refer to [ESP-IDF Documentation](https://docs.espressif.com/projects/esp-idf/) for firmware issues

---

## 🖼️ **System Visualization**

### Hardware PCB Design
<img width="2160" height="1629" alt="SpinLock PCB 3D View" src="https://github.com/user-attachments/assets/e5d1439a-d262-4999-8583-23744b343994" />

### Firmware PCB Prototype
<img width="2160" height="1261" alt="Firmware PCB 3D" src="https://github.com/user-attachments/assets/a07bd00d-fc45-45ac-a8e1-a3d346b02705" />

---

## 👥 **Contributors**

Meet the CO3302 Group 3 team behind SpinLock:

<table>
  <tr>
    <td align="center">
      <a href="https://github.com/ravinduHV">
        <img src="https://avatars.githubusercontent.com/u/75525974?v=4" width="100px;" alt="Ravindu Vimarshana"/><br />
        <sub><b>Ravindu Vimarshana</b></sub>
      </a><br />
      <sub>Backend Microservices & DevOps</sub>
    </td>
    <td align="center">
      <a href="https://github.com/Vithubas">
        <img src="https://avatars.githubusercontent.com/u/182329757?v=4" width="100px;" alt="Vithubas"/><br />
        <sub><b>Vithubas</b></sub>
      </a><br />
      <sub>Embedded Systems & Firmware</sub>
    </td>
    <td align="center">
      <a href="https://github.com/LochanaBandara03">
        <img src="https://avatars.githubusercontent.com/u/163895536?v=4" width="100px;" alt="Lochana Bandara"/><br />
        <sub><b>Lochana Bandara</b></sub>
      </a><br />
      <sub>Frontend Development & UI/UX</sub>
    </td>
  </tr>
</table>

### Contributions by Area:

- **Backend & Infrastructure** ([@ravinduHV](https://github.com/ravinduHV))
  - Microservices architecture (FastAPI)
  - Docker Compose orchestration
  - Kafka event streaming setup
  - API Gateway & authentication services
  - Database design & management

- **Embedded Systems** ([@Vithubas](https://github.com/Vithubas))
  - ESP32 firmware development (ESP-IDF)
  - GPS and motion sensor integration
  - MQTT communication protocols
  - Hardware PCB design
  - Device power management

- **Frontend & Mobile** ([@LochanaBandara03](https://github.com/LochanaBandara03))
  - Flutter mobile application
  - ReactJS admin dashboard
  - User interface design
  - Real-time data visualization
  - User experience optimization
 
---

**Built with ❤️ by CO3302 Group 3**  
**Project:** Smart Bike Parking & Anti-Theft System  
**Platform:** ESP32 + Microservices + Cloud Infrastructure
