# ⚡ GridWise — LLM-Assisted Smart Campus Energy Optimization API.

BUP CSE Fest 2026 · Hackathon · Online Preliminary Round  
**Team Solution Repository**

---

## 01. Architecture Overview

This service implements a robust, secure, and production-grade pipeline to solve the GridWise campus energy optimization challenge:

1. **LLM Interpreter (`llm_interpreter.py`)**: Takes 1–3 natural-language campus operator notes in a single batched request and utilizes an LLM to parse them into structured, machine-checkable JSON directives.
2. **Deterministic Guardrails (`guardrails.py`)**: Treats LLM output as untrusted data. Validates directive types, ensures hourly windows contain unique ascending integers ($0$–$23$), enforces valid numeric bounds (factors between $0$ and $1$, valid reserves/caps), and safely coerces malformed outputs into `no_op` directives[cite: 7, 8, 10].
3. **Math Optimizer (`optimizer.py`)**: Applies validated directives to a 24-hour Linear Programming (LP) model using **PuLP** (CBC solver) to minimize grid electricity cost while strictly respecting campus demand, effective solar bounds, battery rate limits, reserve constraints, and end-of-day battery neutrality (`batt_e[23] == initial_energy_kwh`)[cite: 4, 7].
4. **API Application (`main.py`)**: Exposes readiness and optimization endpoints via **FastAPI** with controlled error handling[cite: 9].

---

## 02. Repository Structure

```text
├── main.py              # FastAPI app, route handlers, schema validation, and error management
├── llm_interpreter.py   # LLM integration client for natural-language operator note interpretation
├── guardrails.py        # Strict deterministic validation layer for LLM-produced interpretations
├── optimizer.py         # PuLP-based linear programming optimization solver and energy replayer
├── requirements.txt     # Python project dependencies
├── Dockerfile           # Fallback containerization blueprint
└── README.md            # Self-contained setup and execution guide
