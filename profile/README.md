# 🚲 **SpinLock – Intelligent Bike Parking & Anti-Theft System**

## 📜 **About the Project**

**SpinLock** is an IoT-driven Intelligent Bike Parking & Anti-Theft System designed to enhance bike security and solve the problem of secure parking availability in urban areas. The system is composed of:

* **Embedded IoT devices** installed on bikes with GPS, accelerometer-based motion detection, and communication modules (Wi-Fi-first, GSM fallback).
* A **Mobile App (Flutter)** for users and parking agents to track bikes, receive alerts, and manage parking.
* An **Admin Web Portal** for administrators to manage users, devices, and parking agents.
* A scalable **Microservices-based Backend** with event-driven communication using **Apache Kafka** and **MQTT Broker** for device communication.

---

## 🚀 **System Architecture Overview**

### 🔧 **Key Components:**

1. **Embedded Device (SpinLock Device)**

   * GPS for real-time tracking
   * Accelerometer for motion/theft detection
   * Wi-Fi (Primary) + GSM (Fallback) communication
   * Solar charging for sustainability

2. **Mobile App (Flutter)**

   * Real-time bike tracking
   * Theft/motion alerts
   * Parking slot discovery and reservation
   * For both bike users and parking agents

3. **Admin Web Portal**

   * User, device, and parking agent management
   * Monitor system events, logs, and platform health

4. **Backend (Microservices with Kafka)**

   * Microservices communicate asynchronously using Kafka
   * MQTT handles device-to-cloud telemetry
   * REST/gRPC for synchronous operations

---

## 🏗️ **Backend Microservices**

| Service                   | Purpose                                                     |
| ------------------------- | ----------------------------------------------------------- |
| **API Gateway**           | Single entry point for clients; routes requests to services |
| **User Authentication**   | Manages users, authentication, and authorization            |
| **Admin Management**      | Admin operations (user, device, parking management)         |
| **Device Onboarding**     | Registers and authenticates devices                         |
| **Device Telemetry**      | Processes incoming device data (GPS, accelerometer)         |
| **GeoLocation Service**   | Handles location-based calculations and mapping             |
| **Alert & Event Service** | Detects events (theft, motion), triggers alerts             |
| **Notification Service**  | Sends push notifications, emails, SMS                       |
| **Parking Slot Service**  | Manages parking slots, reservations, and availability       |
| **MQTT Broker**           | Lightweight real-time messaging between devices and backend |
| **Health Monitoring**     | Tracks device and service health                            |
| **Logging Service**       | Centralized logs, activity, and audits                      |

---

## 🔗 **Communication Flow**

* **MQTT**: Device → DeviceTelemetry → Kafka
* **Kafka Topics**: Microservices exchange events
* **REST/gRPC**: Client ↔ API Gateway ↔ Services (for synchronous actions)

---

## 🔥 **Kafka in the System**

Kafka acts as an **event streaming platform** that decouples services and ensures high availability and fault tolerance for data pipelines.

---

## 📄 **Kafka Topic Design Document**

### 🔧 **Kafka Topics Overview**

| **Topic Name**             | **Purpose**                                              |
| -------------------------- | -------------------------------------------------------- |
| `telemetry-data`           | Streams GPS and motion data from devices                 |
| `motion-alerts`            | Generated when motion or theft is detected               |
| `geo-fence-alerts`         | Enter/exit geo-fence events                              |
| `notification-events`      | Trigger for notification dispatch (push/SMS/email)       |
| `device-status-events`     | Device online/offline, health updates                    |
| `device-onboarding-events` | New device onboarding, updates                           |
| `parking-status-updates`   | Parking slot changes (occupied, available, reserved)     |
| `user-activity-logs`       | Logs of user activities (login, booking, payments, etc.) |
| `system-audit-logs`        | Security, API usage, and audit logs                      |
| `health-check-events`      | Service and device health status                         |

---

### 🏗️ **Topic Details & Schema Example**

#### 1️⃣ **telemetry-data**

* **Producers**: MQTT Broker → DeviceTelemetry Service
* **Consumers**: AlertAndEventProcessing, GeoLocation, Logging

**Schema Example:**

```json
{
  "device_id": "device_123",
  "timestamp": "2025-06-24T14:35:00Z",
  "location": {
    "latitude": 7.4876,
    "longitude": 80.3621
  },
  "motion": {
    "x": 0.015,
    "y": -0.020,
    "z": 0.005
  },
  "battery_level": 85
}
```

---

#### 2️⃣ **motion-alerts**

* **Producers**: AlertAndEventProcessing
* **Consumers**: Notification, Logging, AdminPortal

**Schema Example:**

```json
{
  "device_id": "device_123",
  "alert_type": "MOTION_DETECTED",
  "timestamp": "2025-06-24T14:36:10Z",
  "location": {
    "latitude": 7.4876,
    "longitude": 80.3621
  },
  "severity": "HIGH"
}
```

---

#### 3️⃣ **notification-events**

* **Producers**: AlertAndEventProcessing, ParkingSlot, AdminManagement
* **Consumers**: Notification Service

**Schema Example:**

```json
{
  "recipient_id": "user_123",
  "event_type": "PARKING_RESERVED",
  "message": "Your parking slot is reserved successfully.",
  "timestamp": "2025-06-24T14:40:00Z"
}
```

---

#### 4️⃣ **device-status-events**

* **Producers**: DeviceTelemetry, DeviceOnboarding, HealthMonitoring
* **Consumers**: AdminPortal, Notification, Logging

**Schema Example:**

```json
{
  "device_id": "device_123",
  "status": "ONLINE",
  "timestamp": "2025-06-24T14:38:00Z"
}
```

---

#### 5️⃣ **parking-status-updates**

* **Producers**: ParkingSlot
* **Consumers**: Notification, AdminManagement, Logging

**Schema Example:**

```json
{
  "parking_slot_id": "slot_456",
  "status": "OCCUPIED",
  "timestamp": "2025-06-24T14:45:00Z",
  "user_id": "user_123"
}
```

---

#### 6️⃣ **user-activity-logs**

* **Producers**: API Gateway
* **Consumers**: Logging, AdminManagement

**Schema Example:**

```json
{
  "user_id": "user_123",
  "action": "LOGIN",
  "timestamp": "2025-06-24T14:46:00Z",
  "ip_address": "192.168.1.1"
}
```

---

## 🔥 **Kafka Configuration Suggestions:**

* **Replication Factor:** 3 (Recommended for production)
* **Partitions:** Depends on throughput; e.g., telemetry-data → 6-10
* **Retention Policy:**

  * Telemetry: 7 days
  * Logs & Audits: 30-90 days
  * Alerts: 14 days
* **Compression:** Snappy or GZIP for telemetry and logs

---

## 🌐 **Updated Tech Stack Overview**

| Layer                    | Technologies                                        |
| ------------------------ | --------------------------------------------------- |
| **Frontend - Mobile**    | Flutter                                             |
| **Frontend - Web**       | React (or preferred framework)                      |
| **Backend APIs**         | FastAPI (Python) + Kafka Event Bus                  |
| **Microservices**        | Dockerized services with REST, gRPC, Kafka          |
| **Device Communication** | MQTT Broker                                         |
| **Event Streaming**      | **Apache Kafka**, Schema Registry, Kafka Connect    |
| **Database**             | PostgreSQL / MongoDB (database-per-service pattern) |
| **Monitoring**           | Prometheus + Grafana                                |
| **Logging**              | ELK Stack (Elasticsearch, Logstash, Kibana)         |

---

<img width="2160" height="1629" alt="3D_PCB1_2_2026-01-02 (6)" src="https://github.com/user-attachments/assets/e5d1439a-d262-4999-8583-23744b343994" />

