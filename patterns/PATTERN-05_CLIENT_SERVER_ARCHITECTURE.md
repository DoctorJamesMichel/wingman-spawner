# PATTERN-05 — Client-Server Architecture

Wingman Architecture Pattern Library

---

## Summary

Client-Server Architecture is a foundational software pattern where a **client** requests services or data, and a **server** fulfills those requests.

The client is responsible for:
- user interaction
- presentation
- request formatting

The server is responsible for:
- business logic
- computation
- data storage
- authorization

This is one of the oldest and most reliable architectural models, and it remains the default structure for most web-based systems.

---

## The Core Idea

Instead of building one program that does everything, you split responsibilities into two roles:

- **Client** = the requester (front-end, UI, consumer)
- **Server** = the provider (backend, API, data owner)

The client asks.
The server decides.

This separation enables scalability, centralized security, and cleaner system boundaries.

---

## The Canonical Model

Client  →  Request  →  Server  
Client  ← Response ←  Server  

Where:

- The **client** initiates communication
- The **server** listens for requests
- The server performs work and returns results

---

## Canonical Diagram

```text
     +------------------+
     |      CLIENT      |
     |  UI / App / Web  |
     +------------------+
              |
              | Request (HTTP, RPC, etc.)
              v
     +------------------+
     |      SERVER      |
     | API + Logic + DB |
     +------------------+
              |
              | Response (data / action result)
              v
     +------------------+
     |      CLIENT      |
     +------------------+
```

---

## In Wingman Terms

Client-Server is what happens when a system declares:

> "The human-facing interface must not also be the authority."

In Wingman terms:

- the **Client** is the interaction layer  
- the **Server** is the responsibility engine  

The client asks for action.

The server applies:
- permissions
- validation
- responsibility boundaries
- execution rules

The client never gets to “do whatever it wants.”

This makes Client-Server a **natural governance-friendly architecture**.

---

## Why It Works for Agentic Systems

Agentic systems fail when:

- UI logic becomes execution logic
- the interface bypasses permission checks
- the system confuses “request” with “authority”
- tool execution happens without auditability

Client-Server prevents this by enforcing:

- explicit request boundaries
- centralized enforcement of rules
- traceable execution paths
- consistent security and policy logic

It makes autonomy controllable.

---

## When to Use Client-Server

Use this pattern when:

- you want centralized control over rules
- you need stable authorization enforcement
- you have multiple clients (web + mobile + API consumers)
- you need to scale the backend separately from the UI
- you want to support future integrations safely

Common examples:

- websites and SaaS applications
- API-based agent orchestration
- mobile apps with cloud backends
- command consoles for tool-based automation

---

## When NOT to Use It

Avoid pure Client-Server when:

- you require fully offline operation
- the system must operate without centralized infrastructure
- you need high local autonomy with minimal server dependency

In those cases, you may want:

- Peer-to-Peer Architecture
- Local-first / Edge Computing Architecture
- Distributed Ledger / Consensus Models

---

## Strengths

### 1. Clear Responsibility Separation
Clients request.
Servers enforce.

This prevents accidental authority drift.

### 2. Centralized Governance
Security rules, permission checks, and audit logging live on the server.

### 3. Easier Scaling
You can scale the server independently from the clients.

### 4. Multi-Client Support
One server can support many clients:
- web
- mobile
- agent clients
- integrations

### 5. Security Boundary Clarity
The server is the authority layer.
The client is never trusted.

---

## Weaknesses

### 1. Server Bottleneck
If everything depends on one server, it can become overloaded.

Mitigation: Use load balancing, caching layers, and horizontal scaling.

### 2. Single Point of Failure
If the server goes down, clients may become useless.

Mitigation: Redundancy, failover servers, and graceful degradation.

### 3. Latency and Network Dependence
Clients depend on network round trips to function.

Mitigation: Cache responses, use edge servers, and reduce chatty requests.

### 4. Centralized Control Risks
If governance policy is wrong, the entire system inherits that mistake.

Mitigation: Policy versioning, audit logs, and rollback capability.

### 5. Trust Boundary Misconfiguration
If authentication is weak, a malicious client can impersonate a valid one.

Mitigation: Strong auth, signed requests, and rate limiting.

---

## Key Governance Alignments

Client-Server naturally supports Wingman governance primitives:

- **P-01 Scope Lock**
  (clients cannot access what the server does not expose)

- **P-02 Permission Boundary**
  (server enforces authorization rules)

- **P-03 Interrupt**
  (server can stop or refuse unsafe execution)

- **P-05 Audit Stamp**
  (server logs all requests and responses)

- **P-06 Rollback Covenant**
  (server can revert changes or invalidate actions)

This pattern supports safe autonomy by making the server the final arbiter.

---

## Minimal Example (Wingman Request Cycle)

```text
User clicks button in UI
   ↓
Client sends structured request:
   { action: "deploy", target: "prod", ticket: "ABC-123" }
   ↓
Server validates:
   - permissions
   - scope
   - policy compliance
   ↓
Server executes tool
   ↓
Server returns result + audit stamp
   ↓
Client displays output to user
```

---

## Example: Wingman Client-Server Structure

```text
+----------------------+
|  Wingman Client UI   |
|  (human interaction) |
+----------------------+
           |
           v
+----------------------+
| Wingman API Gateway  |
| (server entry point) |
+----------------------+
           |
           v
+----------------------+
| Governance Pipeline  |
| - scope lock         |
| - permission checks  |
| - interrupt control  |
| - audit stamping     |
| - rollback covenant  |
+----------------------+
           |
           v
+----------------------+
| Tool Execution Layer |
| (safe invocation)    |
+----------------------+
           |
           v
+----------------------+
| Audit Log + Records  |
+----------------------+
```

---

## Wingman Implementation Rule

If a system accepts user requests, it must clearly separate:

- **request**
- **authority**
- **execution**

Client-Server is the cleanest way to enforce this separation.

A client can request anything.

But the server decides what is real.

---

## Default Template (Pattern Stub)

Use this when defining a new client-server boundary:

CLIENT ROLE:
- User-facing interaction surface
- Request formatting
- Response display

SERVER ROLE:
- Permission enforcement
- Validation + governance pipeline
- Execution control
- Audit stamping
- Rollback covenant

REQUEST CONTRACT:
- input schema
- auth requirement
- scope limitation
- expected response format

---

## Field Note

Client-Server is not just an architecture pattern.

It is a **civilization pattern**.

It is the structure behind:

- courts
- libraries
- governments
- hospitals
- banking systems

A client requests.

A trusted authority decides.

If the authority is corrupt, the system collapses.

If the authority is transparent, the system becomes scalable.

That is why governance is inseparable from architecture.

---

## Status

Recommended foundational pattern.

Client-Server is one of the default structural baselines for Wingman systems, especially for any system involving tool execution, permissions, and auditability.  

---

