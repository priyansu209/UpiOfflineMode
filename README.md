# UPI offline mode via Bluetooth Hooping

> Secure Offline UPI Payment System using Mesh Networking, Hybrid Encryption, and Idempotent Settlement.

![Java](https://img.shields.io/badge/Java-17-orange)
![Spring Boot](https://img.shields.io/badge/SpringBoot-3.x-brightgreen)
![Maven](https://img.shields.io/badge/Maven-Build-red)
![Security](https://img.shields.io/badge/Security-RSA%20%2B%20AES-blue)

---

## 📌 Overview

UPI Offline Mesh is a Spring Boot-based simulation of an offline digital payment system that enables users to initiate transactions without internet connectivity.

Instead of requiring an active network connection, encrypted payment packets are propagated through nearby devices in a mesh network until an internet-connected bridge node uploads them to the backend for settlement.

This project demonstrates how secure offline payment routing, encryption, replay protection, and duplicate transaction prevention can be implemented in a distributed environment.

---

## 🚀 Key Features

### 📡 Offline Payment Routing
- Internet-free transaction initiation
- Multi-hop packet forwarding
- Mesh-based communication simulation
- Store-and-forward architecture

### 🔒 End-to-End Encryption
- RSA-2048 key exchange
- AES-256-GCM payload encryption
- Secure packet transmission
- Tamper-proof transactions

### 🛡 Duplicate Transaction Prevention
- SHA-256 packet hashing
- Idempotent transaction processing
- Atomic packet claim mechanism
- Concurrent settlement protection

### ⏳ Replay Attack Protection
- Unique transaction nonce
- Timestamp verification
- Expiry-based validation
- Freshness checks before settlement

### 📊 Interactive Dashboard
- Real-time mesh visualization
- Device monitoring
- Account balance tracking
- Transaction ledger view

---

# 🏗 System Architecture

```text
Sender Device
      │
      ▼
Encrypted Payment Packet
      │
      ▼
Mesh Network
(Device A → Device B → Device C)
      │
      ▼
Bridge Node
      │
      ▼
Spring Boot Backend
      │
      ▼
Settlement Engine
      │
      ▼
Transaction Ledger
```

---

# 🔄 Transaction Flow

## Step 1: Payment Creation

A sender initiates a transaction by entering:

- Sender Account
- Receiver Account
- Amount
- UPI PIN

The payment request is converted into a secure transaction payload.

---

## Step 2: Packet Formation

A MeshPacket is generated containing:

```java
packetId
ttl
createdAt
ciphertext
```

The transaction details remain encrypted and hidden from intermediate devices.

---

## Step 3: Mesh Propagation

The packet is propagated through nearby devices using a gossip-based routing mechanism.

Features:

- Device-to-device communication
- Packet forwarding
- TTL management
- Multi-hop routing

---

## Step 4: Bridge Upload

When a bridge node gains internet access:

- Stored packets are collected
- Encrypted packets are uploaded
- Backend verification begins

---

## Step 5: Settlement

The backend performs:

1. Packet Hashing
2. Duplicate Detection
3. Decryption
4. Freshness Validation
5. Account Settlement

---

# 🔐 Security Problems Solved

## 1️⃣ Untrusted Intermediate Devices

### Problem

Intermediate devices carry payment packets and should not access transaction details.

### Solution

Hybrid Encryption:

```text
RSA-OAEP + AES-256-GCM
```

Benefits:

- Confidentiality
- Integrity
- Authentication
- Tamper Detection

---

## 2️⃣ Duplicate Transaction Problem

### Problem

Multiple bridge nodes may upload the same packet simultaneously.

### Solution

The backend computes:

```text
SHA-256(ciphertext)
```

and performs an atomic claim operation before processing.

Result:

```text
One Packet → One Settlement
```

---

## 3️⃣ Replay Attack Prevention

### Problem

Attackers may resend previously captured packets.

### Solution

- Timestamp Verification
- Nonce Validation
- Expiration Checks

Result:

```text
Old packets are automatically rejected.
```

---

## 4️⃣ Packet Tampering Detection

### Problem

A malicious node modifies transaction contents.

### Solution

AES-GCM authentication tags verify packet integrity.

Any modification causes decryption failure and rejection.

---

# 🧩 Core Components

## Crypto Layer

### HybridCryptoService
Responsible for:

- AES Encryption
- RSA Key Wrapping
- Decryption
- Packet Hashing

### ServerKeyHolder

Responsible for:

- RSA Key Pair Generation
- Public Key Distribution
- Private Key Management

---

## Mesh Layer

### MeshSimulatorService

Responsible for:

- Packet Broadcasting
- Device Discovery
- Gossip Routing
- TTL Handling

### VirtualDevice

Represents a simulated mobile device participating in the mesh.

---

## Settlement Layer

### BridgeIngestionService

Processing Pipeline:

```text
Packet Received
      ↓
Hash Packet
      ↓
Check Duplicate
      ↓
Decrypt
      ↓
Validate Freshness
      ↓
Settle Transaction
```

### SettlementService

Responsible for:

- Debit Sender
- Credit Receiver
- Create Ledger Entry
- Database Transaction Management

---

# 🗂 Project Structure

```text
src
├── controller
│   ├── ApiController
│   └── DashboardController
│
├── service
│   ├── DemoService
│   ├── MeshSimulatorService
│   ├── SettlementService
│   └── BridgeIngestionService
│
├── crypto
│   ├── HybridCryptoService
│   └── ServerKeyHolder
│
├── model
│   ├── Account
│   ├── Transaction
│   ├── MeshPacket
│   └── PaymentInstruction
│
├── config
│   └── AppConfig
│
└── resources
    ├── application.properties
    └── dashboard.html
```

---

# 🛠 Technology Stack

## Backend

- Java 17
- Spring Boot 3
- Spring Data JPA
- Hibernate

## Database

- H2 Database

## Security

- RSA-2048
- AES-256-GCM
- SHA-256

## Build Tool

- Maven

---

# 📡 API Endpoints

| Method | Endpoint | Description |
|----------|------------|-------------|
| GET | `/` | Dashboard UI |
| GET | `/api/accounts` | Fetch account balances |
| GET | `/api/transactions` | Transaction history |
| GET | `/api/mesh/state` | Current mesh status |
| POST | `/api/demo/send` | Create payment packet |
| POST | `/api/mesh/gossip` | Deliver Packet |
| POST | `/api/mesh/flush` | Upload packets via bridge |
| POST | `/api/bridge/ingest` | Process incoming packet |

---

# 🧪 Testing

Implemented tests for:

✅ Encryption & Decryption Validation

✅ Duplicate Transaction Prevention

✅ Concurrent Packet Processing

✅ Replay Attack Detection

✅ Packet Tampering Rejection

---

# 📈 Future Enhancements

- Real Bluetooth Low Energy (BLE)
- Wi-Fi Direct Communication
- Redis-based Distributed Idempotency
- PostgreSQL Integration
- Android Application
- Real Banking / NPCI Integration
- Digital Signature Support
- Push Notification System

---

# 📸 Demo

### Dashboard

![Dashboard](screenshots/dashboard.png)

### Packet Creation

![Packet Creation](screenshots/Packet_created.png)

### Packet Transfer

![Transfer Packet](screenshots/transfer_packet.png)

### Complete Transaction

![Complete Transaction](screenshots/complete_transaction.png)

---

# 🎯 Learning Outcomes

This project demonstrates practical implementation of:

- Distributed Systems
- Mesh Networking Concepts
- Secure Communication
- Hybrid Encryption
- Idempotency Design
- Spring Boot Backend Development
- Concurrent Transaction Processing
- System Design Principles

---

# 👨‍💻 Author

**Priyansu Tripathi**

B.Tech Computer Science & Engineering (2026)

Skills:
- Java
- Spring Boot
- SQL
- REST APIs
- Data Structures & Algorithms

---

⭐ If you found this project interesting, consider giving it a star.
