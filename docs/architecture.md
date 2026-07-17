# Cognitive Navigation · Architecture

## Overview

Cognitive Navigation is designed as a lightweight, platform-agnostic Sidecar service that integrates with any Agent orchestration framework.

```
┌─────────────────────────────────────────────────────────────┐
│                   Agent Orchestration Layer                │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐                │
│  │  Agent 1 │  │  Agent 2 │  │  Agent N │                │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘                │
│       │             │             │                       │
│       └─────────────┼─────────────┘                       │
│                     │ HTTP POST /diagnosis                 │
│                     ▼                                      │
├─────────────────────────────────────────────────────────────┤
│                Cognitive Navigation Sidecar                │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  FastAPI Server (port 8000)                        │   │
│  │  ├── /diagnosis (single)                          │   │
│  │  ├── /batch-diagnosis (bulk)                      │   │
│  │  ├── /health                                      │   │
│  │  └── /stats                                       │   │
│  └─────────────────────────────────────────────────────┘   │
│                     │                                      │
│                     ▼                                      │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  V4.0 Context Compressor                           │   │
│  │  ├── L1 (irreversible): permanent retention        │   │
│  │  ├── L2 (semi-reversible): summarized retention    │   │
│  │  └── L3 (reversible): full deletion                │   │
│  └─────────────────────────────────────────────────────┘   │
│                     │                                      │
│                     ▼                                      │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  Yiyongxue Engine                                  │   │
│  │  └── S = T - C - D                                 │   │
│  └─────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

---

## Core Components

### 1. Diagnosis API (FastAPI)

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/diagnosis` | POST | Single agent health assessment |
| `/batch-diagnosis` | POST | Bulk assessment (≤10 queries) |
| `/health` | GET | Service health check |
| `/stats` | GET | System statistics |

### 2. V4.0 Context Compressor

| Level | Definition | Strategy |
|-------|------------|----------|
| **L1** | Confirmed decisions, core constraints, key entities | Permanent retention |
| **L2** | Intermediate reasoning, referenced information | Structured summary |
| **L3** | Drafts, chats, outdated info | Full deletion into loss pool |

**Key Metric: S_memory** (0~1) — monitors memory health after compression.

### 3. Integration Modes

| Mode | Deployment | Use Case |
|------|------------|----------|
| **Sidecar** | Co-located with Agent pod | Production agent clusters |
| **Standalone** | Separate server | Development/testing |
| **Embedded** | Within Agent code | Lightweight PoC |

---

## Deployment Options

### Docker (Recommended)

```bash
docker run -d -p 8000:8000 cognitive-nav:latest
```

### Python (Local Development)

```bash
uvicorn main:app --host 0.0.0.0 --port 8000
```

### Platform Integrations

| Platform | Integration Status |
|----------|-------------------|
| Alibaba Qoder CN | ✅ Validated |
| WorkBuddy | ✅ Validated |
| CrewAI | ✅ Validated |
| LangChain | 🔄 Planned |

👉 [Back to README](../README.md)
