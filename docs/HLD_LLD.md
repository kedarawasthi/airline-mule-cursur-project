# American Airlines Info API - HLD and LLD

## 1) High-Level Design (HLD)

### 1.1 Context
`american-airlines-info-app` is a MuleSoft API-led implementation exposing airline flight operations and DataWeave transformation demos over HTTP on CloudHub 2.0.

### 1.2 Logical Components

- **Experience/API Layer**
  - APIKit router flow: `american-airlines-info-api-main` in `interface.xml`
  - Error mapping and HTTP response shaping
- **Business/Implementation Layer**
  - `getAllFlights`, `getFlightById`, `postFlights`, `updateFlightById`, `deleteFlightById`, `postFlightsBatch`
  - Circuit breaker orchestration and resilience logic
- **Integration Layer**
  - MySQL database connector
  - Outbound HTTP requests for notifications and external logging
- **Operational Layer**
  - `scheduledOperationalHeartbeat` scheduler flow in `ops.xml`
  - Correlated logging and transaction tracing
- **Transformation Layer**
  - DataWeave scripts in `src/main/resources/dwl/usecases`
  - Demo listener flows in `dw-demo-flows.xml`

### 1.3 Deployment Topology

```mermaid
flowchart TB
    U[API Consumers] --> CH[CloudHub 2.0 Endpoint]
    CH --> APP[Mule App: american-airlines-info-app-dev]
    APP --> DB[(MySQL)]
    APP --> NOTIF[Notification Service]
    APP --> EXTLOG[External Logging Service]
    APP --> OBS[Anypoint Monitoring]
```

### 1.4 End-to-End Request Sequence (CRUD)

```mermaid
sequenceDiagram
    participant Client
    participant CH2 as CloudHub Ingress
    participant API as APIKit Main Flow
    participant Impl as Implementation Flow
    participant DB as MySQL
    participant Ext as Notification/External APIs

    Client->>CH2: HTTP /api/flights...
    CH2->>API: Route request
    API->>Impl: flow-ref by method/resource
    Impl->>DB: Query/Insert/Update/Delete
    Impl-->>Ext: Optional outbound call
    Impl-->>API: Payload + status vars
    API-->>Client: Standardized JSON response
```

### 1.5 Scheduler and Operational Flow Sequence

```mermaid
sequenceDiagram
    participant Scheduler
    participant Ops as scheduledOperationalHeartbeat
    participant DB as MySQL
    participant Log as Mule Logger
    participant Ext as External Logging Endpoint

    Scheduler->>Ops: Trigger every N seconds
    Ops->>DB: select 1 as ok
    DB-->>Ops: heartbeat status
    Ops->>Log: txId + event=ops.heartbeat.success/failure
    Ops-->>Ext: POST operational log (if enabled)
```

### 1.6 Batch Processing Design View

```mermaid
flowchart LR
    R[POST /api/flights/batch] --> B[postFlightsBatch]
    B --> S[Split batch records]
    S --> P[Per-record processing]
    P --> OK[Success counter]
    P --> ER[Failure capture store]
    OK --> RES[Consolidated response]
    ER --> RES
```

### 1.7 Non-Functional Characteristics

- Runtime: Mule 4.6.28, Java 17
- Environment-based configuration (`mule.env=dev`)
- Secure properties encryption using `mule.key`
- Structured logging with transaction identifiers

## 2) Low-Level Design (LLD)

### 2.1 Interface Flows

- `american-airlines-info-api-main`
  - Listener: `/api/*`
  - Sets `transactionId` from `correlationId`
  - Routes to APIKit and mapped implementation flows
- `american-airlines-info-api-console`
  - Listener: `/console/*`
  - API console rendering and not-found handling

### 2.2 Core Business Flows

- `getAllFlights`: list flights, optional query filtering path handled by implementation logic
- `getFlightById`: retrieve one flight by ID
- `postFlights`: create flight and send async downstream notification
- `updateFlightById`: update existing flight and invoke outbound notification
- `deleteFlightById`: delete existing flight; returns controlled not-found response
- `postFlightsBatch`: batch payload processing with consolidated result handling

### 2.3 DataWeave Demo Flows

- `/dw/simple` -> `simple-transformation-json-to-json.dwl`
- `/dw/complex` -> `complex-transformation-xml-to-json.dwl`
- `/dw/multi1` -> `multi-scenario-1-json-to-xml.dwl`
- `/dw/multi2` -> `multi-scenario-2-xml-to-json.dwl`

### 2.4 Scheduler Flow

- Flow: `scheduledOperationalHeartbeat`
- Trigger: fixed frequency (`ops.heartbeat.frequencySeconds`)
- Steps:
  1. Generate `transactionId`
  2. DB health query (`select 1 as ok`)
  3. Log heartbeat success/failure
  4. Optional external logging POST when enabled

### 2.5 Configuration Model

- Normal properties: `config-${mule.env}.properties`
- Secure properties: `config-${mule.env}-secure.properties`
- Secure values include DB secrets, runtime keys, and platform client credentials.

### 2.6 Error Handling and Observability

- APIKit + custom error handler paths for HTTP status consistency
- `logger` components include tx/correlation context
- Rolling + JSON console logging in `log4j2.xml`
