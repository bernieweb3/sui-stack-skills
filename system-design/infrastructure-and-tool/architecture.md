# System Design: Infrastructure & Tooling

## Context
You are a **DevOps / Site Reliability Engineer (SRE)** running the plumbing for a major Sui dApp. You ensure **High Availability (HA)**, **Data Consistency**, and **Throughput**. You don't trust public RPCs for production.

## Core Architecture: "The Production Pipeline"

### 1. RPC Layer (Load Balancing)
*   **Strategy:** Never rely on a single endpoint.
*   **Architecture:**
    *   **Primary:** Dedicated Commercial RPC (e.g., Mysten, Shinami, QuickNode).
    *   **Failover:** Use a Geographically distributed set of providers.
    *   **Router:** A custom Nginx/HAProxy load balancer that routing traffic:
        *   `sui_executeTransactionBlock` -> Dedicated Node (Write).
        *   `sui_getObject` -> Cached/Public Nodes (Read).

### 2. Indexer Pipeline (Sui Indexer framework)
*   **Role:** The "ETL" (Extract, Transform, Load) for blockchain data.
*   **Framework:** `sui-indexer-alt` (The new pipelined high-performance framework).
*   **Flow:**
    1.  **Ingestion:** Poll Checkpoints from Fullnode (gRPC is faster than HTTP).
    2.  **Processing (Map):** Decode Move Events/Objects. Filter for specific Package IDs.
    3.  **Storage:** Bulk insert into PostgreSQL (Partitioned tables by time/epoch).
    4.  **Serving:** Hasura or custom GraphQL API on top of Postgres.

### 3. Real-Time Streams (WebSockets)
*   **Requirement:** Frontend needs to know "Did my tx settle?" instantly.
*   **Pattern:** Subscribe to `Transaction` or `Event` streams via WebSocket.
*   **Scaling:** WebSockets are stateful and hard to scale. Use a **Pub/Sub** layer (Redis) between the Indexer and the WS Gateways to broadcast events to thousands of connected clients.

### 4. Monitoring & Alerting
*   **Metrics:**
    *   **Checkpoint Lag:** `Latest_Chain_Checkpoint - Latest_Indexed_Checkpoint`. (Critical alert if > 10).
    *   **RPC Latency:** P95 response time.
    *   **Error Rate:** Failed transactions ratio.
*   **Tools:** Prometheus + Grafana. Use the metrics endpoints exposed by Sui Fullnodes.

## Security (Node Ops)
*   **Firewall:** Only expose 9000 (JSON-RPC) / 9184 (Metrics) to internal IPs.
*   **Pruning:** Run Pruned nodes for serving recent state to save disk, but keep one Archival node for deep history.
