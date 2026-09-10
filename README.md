# Lightweight Authentication for LIN/CAN Probes

Classic automotive buses (LIN/CAN) lack built-in message authentication, so any ECU on the bus can inject or replay frames. This research prototype implements and evaluates a lightweight message-authentication scheme for resource-constrained probes.

## What it does

- **15-bit truncated MAC** (`mac_implementation.py`): SHA-256 truncation with HOTP-style offset selection, binding the probe identity, a nonce, and a sequence number into the tag — small enough to fit within LIN/CAN frame payload constraints.
- **Attack simulator** (`attack_simulator.py`): demonstrates replay (and injection) attacks against an unauthenticated bus.
- **Virtual CAN network** (`virtual_can.py`): a software CAN with ECUs and probes for reproducible experiments.
- **Performance analysis** (`performance_analyzer.py`, `performance_charts.py`): measures latency, throughput, and per-frame security cost; charts are generated under `public/images/`.
- **Web dashboard** (`public/index.html` + FastAPI `api/index.py`): renders the architecture, protocol flow, attack-prevention, and performance results.

## Running it

```bash
pip install -r requirements.txt
python quick_test.py            # seeded end-to-end demo (replay + verification across cycles)
uvicorn server:app --reload     # or: python server.py — dashboard + API
```

## Structure

| File | Purpose |
| --- | --- |
| `mac_implementation.py` | Truncated-SHA-256 MAC (compute + verify) |
| `virtual_can.py` | Virtual CAN network, ECUs, probes |
| `attack_simulator.py` | Replay/injection attack simulation |
| `performance_analyzer.py` | Latency/throughput/security-cost measurement |
| `server.py`, `api/index.py` | FastAPI backend for the dashboard |
| `public/` | Static dashboard + generated charts |

## Scope

Research prototype — a controlled, simulated evaluation of the scheme, not a production CAN security product.
