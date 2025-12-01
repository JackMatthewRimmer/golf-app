# GolfRoundPro – Project Specification

This project is a local-only golf round tracking platform designed to help develop skills in Kubernetes, Helm, Kafka, Redis, Rust, React, and overall software architecture. The application tracks rounds, hole-by-hole scores, and generates lightweight analytics.

---

## 1. Purpose of the Project

The goal is to build a real, end-to-end system that mimics a modern distributed architecture.
The scope is intentionally small to allow learning without requiring months of development.

This project will cover:
- Microservice design
- Event-driven architecture
- Containerization and deployment through Kubernetes & Helm
- Frontend and backend development
- Distributed caching and real-time analytics

---

## 2. High-Level Architecture

### Components:
- **Rust Round Service**
  REST API for creating rounds, recording hole data, and retrieving round information.

- **Rust Analytics Service**
  Kafka consumer that updates analytics and writes them into Redis for fast retrieval.

- **PostgreSQL**
  Primary data store for all permanent round and course data.

- **Kafka / Redpanda**
  Event streaming layer used to publish events like `RoundStarted`, `HoleScored`, and `RoundFinished`.

- **Redis**
  Acts as a caching layer and stores materialized analytics views.

- **React Frontend**
  Web interface for viewing rounds, entering scores, and viewing analytics.

- **Kubernetes (kind/minikube)**
  Local deployment environment.

- **Helm Charts**
  Used to package and deploy each service along with shared dependencies.

---

## 3. Backend Services

### 3.1 Round Service (Rust)
**Responsibilities:**
- Create a round
- Record per-hole scores
- Retrieve round summaries
- Persist data to PostgreSQL
- Publish events to Kafka
- Invalidate Redis cache entries when round data changes

**Skills Learned:**
- Rust web APIs
- Database migration & schema design
- Event publishing
- Cache-aside patterns
- Containerization for K8s

---

### 3.2 Analytics Service (Rust)
**Responsibilities:**
- Consume Kafka events
- Maintain computed metrics such as:
  - Best round per course
  - Average score per round
  - Recent performance trends
- Store analytics results in Redis as materialized views

**Skills Learned:**
- Kafka consumer groups
- Async Rust
- Data modeling for analytics
- Distributed cache writing
- Designing real-time aggregated data pipelines

---

## 4. Datastores

### 4.1 PostgreSQL
**Purpose:**
Permanent system-of-record. Stores:
- Rounds
- Holes & scores
- Courses
- Optional analytics history

**Skills Learned:**
- Schema design
- Integration with Rust (sqlx/diesel)
- Running stateful workloads locally in Kubernetes

---

### 4.2 Redis
**Purpose:**
Fast access store for:
- Cached round summaries
- Precomputed analytics (materialized views)
- Optional session storage or rate limiting

**Skills Learned:**
- Cache-aside pattern
- Redis keys & TTL strategy
- High-speed data access
- Designing aggregated views for performance

---

## 5. Event Streaming (Kafka / Redpanda)

### Events Published:
- `RoundStarted`
- `HoleScored`
- `RoundFinished`

**Purpose:**
- Enables asynchronous compute
- Decouples round recording from analytics
- Reflects modern event-driven production systems

**Skills Learned:**
- Topic design
- Serialization (JSON/Avro)
- Producer/consumer lifecycle
- Local Kafka cluster deployment in Kubernetes

---

## 6. Frontend (React)

### Responsibilities:
- Create new rounds
- Enter hole-by-hole scores
- Display past rounds
- Show analytics stored in Redis via the analytics API

**Skills Learned:**
- Modern React with hooks
- API consumption
- UI design for simple dashboards
- Real-time updates (optional WebSockets)

---

## 7. Kubernetes + Helm

### Kubernetes Responsibilities:
- Run all services locally (API, analytics, Kafka, Redis, PostgreSQL)
- Handle networking, ingress, environment config, scaling options

### Helm Responsibilities:
- Package each service as a chart
- Provide templates for:
  - Deployments
  - Services
  - ConfigMaps
  - Secrets
  - PersistentVolumeClaims

**Skills Learned:**
- Real-world deployment patterns
- Managing local microservices
- Versioned infrastructure as code
- Understanding service discovery & scaling

---

## 8. Suggested Technology Interactions

### Data Write Path:
1. Frontend submits a score → Round Service
2. Round Service writes to PostgreSQL
3. Round Service invalidates Redis cache
4. Round Service publishes event to Kafka

### Data Read Path:
1. Frontend requests round summary
2. Round Service checks Redis
3. If cached → return value
4. If miss → read from Postgres → store in Redis → return

### Analytics Flow:
1. Kafka event consumed by Analytics Service
2. Analytics Service computes stats
3. Stats stored into Redis for fast frontend reads

---

## 9. Project Scope Summary

### Minimum Viable Product:
- Create a round
- Enter hole scores
- View round summary
- Basic analytics (average round score)
- Deployed fully on local Kubernetes

### Optional Extensions:
- User profiles
- Course builder UI
- Handicap simulation
- WebSocket live updates
- Achievements (e.g., personal-best notifications)

---

## 10. Skills Gained by Completing the Project

### Backend / Systems:
- Rust microservices
- Event-driven architecture
- Kafka producers/consumers
- Redis caching & materialized views
- PostgreSQL schema design

### DevOps:
- Dockerization
- Kubernetes workloads
- Helm charts
- Local cluster debugging & logs

### Frontend:
- React UI
- State management
- Visualization of analytics

### Architecture / Design:
- Separation of concerns
- Asynchronous pipelines
- Caching strategies
- Real-time vs batch analytics
- Designing services that scale

---

This project is intentionally small but covers a broad range of technologies and design concepts. Completing it results in practical, modern skills while avoiding year-long development time.
