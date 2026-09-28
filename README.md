# SE Labs: OpenAPI Mock Server Generator

Software Engineering lab submissions for **Problem Statement #41: OpenAPI Mock Server Generator**.

| | |
|---|---|
| **Name** | Sameeksha Prasanna Kumar |
| **SRN** | PES1UG24CS410 |
| **University** | PES University, Bengaluru |
| **Course** | Software Engineering |

## About the Project

The OpenAPI Mock Server Generator lets an API developer upload an OpenAPI 3.0 spec (YAML/JSON) and instantly get a working mock server. It generates endpoints that return randomized, schema-compliant responses, validates incoming requests against the spec, simulates configurable latency, and logs every request.

**Target actors:** API Developer, QA Engineer, CI/CD Pipeline

## Repository Structure

```
SE-Labs-PES1UG24CS410/
├── LAB1/   Requirements & use cases
├── LAB2/   Agile planning with Jira
├── LAB3/   Component modelling & architecture
└── README.md
```

## Lab 1: Requirements & Use Cases

- **Requirements table:** 5 functional and 2 non-functional requirements, each with priority, acceptance criteria and rationale.
- **Use case flow specification:** UC-05 *Query Mock Endpoint* (main scenario plus the schema-validation-failure alternate flow).
- **Use case diagram:** UC-01 to UC-07 with the API Developer, QA Engineer and CI/CD Pipeline actors.

| ID | Requirement (short) | Priority |
|---|---|---|
| FR-001 | Parse OpenAPI 3.0 and generate mock endpoints | High |
| FR-002 | Upload spec via web UI or CLI | High |
| FR-003 | Validate requests against schema (HTTP 422 on failure) | High |
| FR-004 | Configurable per-endpoint latency | Medium |
| FR-005 | Request log listing | Medium |
| NFR-001 | Sustain 1,000 req/s with latency accurate to ±10 ms | High |
| NFR-002 | AES-256 encryption at rest for specs and logs | High |

## Lab 2: Agile Planning (Jira)

- 3 epics and 6 user stories derived from the Lab 1 requirements, estimated in Fibonacci story points.
- Two simulated sprints: Sprint 1 (13 points) and Sprint 2 (9 points), with board screenshots and burndown charts.
- Written reflection on estimation, prioritization, sprint alignment and team capacity.
- **Jira board:** https://sameekshakumar.atlassian.net/jira/software/projects/SCRUM/boards/1/backlog?epics=visible

## Lab 3: Component Modelling & Architecture

**Chosen style: Layered Architecture** (Presentation / Business / Data).

The UML component diagram has 5 components and 6 interfaces:

| Layer | Component | Role |
|---|---|---|
| Presentation | API Gateway / Request Handler | Single entry point for UI, CLI and REST clients |
| Business | Spec Parser & Validator | Parses OpenAPI specs, validates requests against the schema |
| Business | Mock Response Generator | Generates endpoints, applies latency, builds mock responses |
| Business | Request Logger | Records request/response metadata |
| Data | Secure Data Store | Persists specs and logs, AES-256 encrypted at rest |

| Interface | Provided by | Required by |
|---|---|---|
| `IParseSpec` | Spec Parser & Validator | API Gateway |
| `IGenerateMock` | Mock Response Generator | API Gateway |
| `ISchemaValidate` | Spec Parser & Validator | Mock Response Generator |
| `ILogRequest` | Request Logger | Mock Response Generator |
| `ISpecStorage` | Secure Data Store | Spec Parser & Validator |
| `ILogStorage` | Secure Data Store | Request Logger |

The one-page justification covers the reasons for choosing a layered architecture, its security advantage (encryption enforced in a single Data-layer component) and its performance benefit (validation and generation stay in memory on the hot path).

## Tools Used

- **Diagrams:** UML component and use case diagrams
- **Project management:** Jira (Scrum)
- **Docs:** Word / PDF
