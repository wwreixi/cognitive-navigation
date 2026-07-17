# Quick Start · Cognitive Navigation

Get Cognitive Navigation running in 5 minutes.

---

## Prerequisites

- Python 3.11+
- Docker (optional, recommended)
- Any Agent framework (CrewAI, LangChain, etc.)

---

## Method 1: Docker (Quickest)

```bash
# 1. Pull the image
docker pull cognitive-nav:latest

# 2. Run the container
docker run -d --name cognitive-nav -p 8000:8000 cognitive-nav:latest

# 3. Verify it's running
curl http://localhost:8000/health
```

Expected response:
```json
{"status":"healthy","version":"4.0.0"}
```

---

## Method 2: Local Python

```bash
# 1. Clone the repository
git clone https://github.com/wwreixi/agent-trace-diagnostics.git
cd agent-trace-diagnostics

# 2. Install dependencies
pip install -r requirements.txt

# 3. Start the API server
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

---

## First Diagnosis Call

```bash
curl -X POST http://localhost:8000/diagnosis \
  -H "Content-Type: application/json" \
  -d '{
    "query": "My agent just completed a financial compliance analysis.",
    "session_id": "demo_001"
  }'
```

### Response

```json
{
  "status": "success",
  "trace_id": "test-xxx-xxx",
  "result": "Diagnostic analysis complete.",
  "S": 0.47,
  "T": 0.75,
  "C": 0.18,
  "D": 0.10,
  "zone": "YELLOW",
  "nav_instruction": "RESET_CONTEXT",
  "S_memory": 0.82,
  "compression_stats": {
    "items_kept": 1,
    "l1_count": 1,
    "l2_count": 0,
    "l3_count": 0
  }
}
```

---

## Interpreting the Results

| Field | What it tells you |
|-------|-------------------|
| **S** | Health (0.47 = moderate, needs attention) |
| **T** | Transformative power (0.75 = good) |
| **C** | Constraint awareness (0.18 = weak, needs improvement) |
| **D** | Internal friction (0.10 = low) |
| **zone** | YELLOW → perform RESET_CONTEXT |
| **nav_instruction** | Actionable guidance for intervention |

---

## Next Steps

- [x] Run your first diagnosis
- [ ] Integrate with your Agent's workflow
- [ ] Explore [architecture](architecture.md) for deeper integration
- [ ] Review [case studies](../examples/) for real-world patterns

👉 [Back to README](../README.md)
