# Capstone: AI-Powered API Spec → Code → Test Pipeline

## Documentation

---

## 1. Problem Statement

### The Problem

When building backend APIs, developers manually repeat three tedious, error-prone steps for every new endpoint:

1. **Write the API specification** — Manually author OpenAPI/JSON schemas describing endpoints, data models, and validation rules.
2. **Scaffold the implementation** — Hand-write route handlers, validation logic, and boilerplate server code that mirrors the spec.
3. **Create the test suite** — Write integration tests that verify each endpoint against the spec's expected behaviors.

These steps are **tightly coupled**: a change in the spec should cascade to the code and tests, yet developers manually copy-paste and re-adapt between disconnected tools. This results in:

- ⏱️ **3-4 hours** of manual work per API module
- 🐛 Inconsistencies between spec, code, and tests
- 😓 Repetitive boilerplate that discourages thoroughness

### The Solution

An **AI-powered pipeline** that chains three different AI UX types, where each stage's output becomes the next stage's input — reducing the entire process from hours to **~5 minutes**.

---

## 2. Workflow Overview

| Stage | AI UX Type | Tool Used                | Input                        | Output                                  |
| ----- | ---------- | ------------------------ | ---------------------------- | --------------------------------------- |
| **1** | Chat AI    | Antigravity Chat         | Natural language description | OpenAPI 3.0 JSON spec                   |
| **2** | IDE AI     | Antigravity IDE Agent    | JSON spec file               | Express.js server + routes + validation |
| **3** | CLI AI     | Antigravity CLI/Terminal | Source code files            | Jest test suite + execution results     |

### Data Flow

```
Natural Language  ──→  [CHAT AI]  ──→  api_spec.json
                                            │
                                            ▼
                       [IDE AI]   ──→  server.js
                                       validation.js
                                       routes/tasks.js
                                            │
                                            ▼
                       [CLI AI]   ──→  tests/tasks.test.js
                                       ✅ 21/21 tests pass
```

---

## 3. Step-by-Step Workflow Instructions

### Stage 1: Chat AI → Generate OpenAPI Spec

**Tool:** Antigravity Chat (Chat UX)  
**Action:** Provide a natural language description of the desired API. The Chat AI produces a structured, machine-readable OpenAPI 3.0 JSON specification.

**Prompt Used:**

```
Generate a complete OpenAPI 3.0 JSON specification for a task management
API called TaskFlow.

It should support:
- Creating tasks with title (required), description, priority
  (low/medium/high/critical), assignee email, and due date
- Listing all tasks with optional status/priority filters
- Getting a single task by ID
- Updating any task field
- Deleting a task

Include proper validation constraints (min/max length, email format,
date-time format) and standardized error response schemas.
```

**Output:** `api_spec.json` — A 165-line OpenAPI 3.0 specification with:

- 5 endpoint definitions (GET, POST, PUT, DELETE)
- 4 component schemas (Task, CreateTaskInput, UpdateTaskInput, Error)
- Validation constraints derived from natural language requirements

**Handoff:** The JSON file is saved to the project directory, ready for the IDE AI.

---

### Stage 2: IDE AI → Scaffold Express.js Application

**Tool:** Antigravity IDE Agent (IDE UX)  
**Action:** The IDE AI reads the `api_spec.json` file and generates a complete Express.js application with route handlers and Joi validation schemas.

**Prompt Used:**

```
Read the api_spec.json OpenAPI specification and generate a complete
Express.js application:

1. server.js — Express app with middleware and route mounting
2. validation.js — Joi schemas derived from the OpenAPI component
   schemas (CreateTaskInput, UpdateTaskInput, query params)
3. routes/tasks.js — Route handlers for each operationId
   (listTasks, createTask, getTask, updateTask, deleteTask)

Use an in-memory Map as the data store. Include proper error handling
with the Error schema format from the spec.
```

**Output:** 4 files generated:

- `server.js` — Express app setup with middleware, health check, error handler
- `validation.js` — 3 Joi schemas mirroring the OpenAPI component schemas
- `routes/tasks.js` — 5 CRUD handlers mapped from operationIds
- `package.json` — Dependencies (express, joi, uuid, cors, jest, supertest)

**Handoff:** The source code files are available in the project directory for CLI analysis.

---

### Stage 3: CLI AI → Generate & Execute Tests

**Tool:** Antigravity CLI / Terminal (CLI UX)  
**Action:** The CLI AI analyzes the generated source code files and produces a comprehensive test suite, then executes it.

**Prompt Used:**

```
Analyze the source code in server.js, routes/tasks.js, and validation.js.

Generate a comprehensive Jest + Supertest integration test suite that covers:
- Happy path for each CRUD operation
- Validation error paths (missing title, invalid priority, bad email, etc.)
- Edge cases (empty list, 404s, empty update body)
- Status/priority query filtering

Then run 'npx jest --verbose' and report results.
```

**Output:**

- `tests/tasks.test.js` — 21 test cases across 6 describe blocks
- **Execution Result:** ✅ **21 passed, 0 failed**

---

## 4. Results & Evaluation

### Functionality (40%)

✅ The workflow runs completely end-to-end. Natural language input produces a working API with a passing test suite. All 21 tests pass.

### Efficiency (20%)

✅ The entire pipeline completes in **~5 minutes** vs. **3-4 hours** manually. That's a **~97% reduction** in effort.

| Metric        | Manual       | AI Pipeline |
| ------------- | ------------ | ----------- |
| Time to spec  | ~45 min      | ~30 sec     |
| Time to code  | ~2 hours     | ~1 min      |
| Time to tests | ~1.5 hours   | ~1 min      |
| **Total**     | **~4 hours** | **~5 min**  |

### Documentation (20%)

✅ Interactive HTML flowchart with animated connectors and expandable stage details. Written documentation covers problem statement, workflow, and exact prompts.

### Adaptability (20%)

✅ The workflow is **tool-agnostic** — the same 3-stage pattern works with:

- **Chat AI:** ChatGPT, Gemini, Claude, or any chat interface
- **IDE AI:** GitHub Copilot, Cursor, Cody, or any code-aware agent
- **CLI AI:** Aider, Shell-GPT, or any terminal-based AI tool

The key design principle is that **each stage produces a standard, portable artifact** (JSON spec → source files → test files) that any tool in that category can consume.

---

## 5. Files Produced

```
taskflow-docs/
├── workflow_diagram.html          ← Interactive flowchart (Deliverable 1)
├── Capstone_Documentation.md      ← This document (Deliverable 2)
└── demo-api/                      ← Working demo project (Deliverable 3)
    ├── api_spec.json              ← Stage 1 output (Chat AI)
    ├── package.json
    ├── server.js                  ← Stage 2 output (IDE AI)
    ├── validation.js              ← Stage 2 output (IDE AI)
    ├── routes/
    │   └── tasks.js               ← Stage 2 output (IDE AI)
    └── tests/
        └── tasks.test.js          ← Stage 3 output (CLI AI)
```

---

_Abraham Jimah Zorwi — AI-Powered Workflow Capstone — February 2026_
