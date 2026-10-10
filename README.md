# Student Leave Workflow — Camunda 8 + Spring Boot

A starter project for the Camunda Modeler exercise. The BPMN model can be opened directly in **Camunda Modeler**.

## Workflow
1. `POST /leave` starts the `student-leave` process.
2. `validate-leave` checks the dates and calculates inclusive leave days.
3. Invalid dates are rejected; valid leave of up to 2 days is auto-approved; longer leave waits for the Faculty Advisor in Tasklist.
4. `notify-student` logs the decision and writes `leaveStatus`.

## Files
- `src/main/resources/bpmn/student-leave.bpmn` — BPMN process model
- `src/main/resources/bpmn/fa-approval.form` — Faculty Advisor form
- `src/main/resources/application.yaml` — Camunda SaaS credentials (placeholders only)
- `src/main/java/...` — Spring Boot REST endpoint and job workers

## Before running
- JDK 17+
- Maven 3.9+
- Camunda 8.8 SaaS cluster and API client with Zeebe / Orchestration Cluster scope
- Set your own credentials in `application.yaml` or environment variables. Never commit secrets.

Environment variables:
- `CAMUNDA_CLIENT_ID`
- `CAMUNDA_CLIENT_SECRET`
- `CAMUNDA_CLUSTER_ID`
- `CAMUNDA_CLUSTER_REGION`

## Run
```bash
mvn spring-boot:run
```

## Test request
```bash
curl -X POST http://localhost:8080/leave \
  -H "Content-Type: application/json" \
  -d '{"studentName":"Priya","regNo":"RA2311003010001","fromDate":"2026-10-20","toDate":"2026-10-21","reason":"Medical appointment"}'
```

Use ISO dates (`YYYY-MM-DD`). The example dates are future-dated for the lab; update them as needed.

## Open the BPMN
Open `src/main/resources/bpmn/student-leave.bpmn` in Camunda Modeler. If you edit the diagram, save it in the same location so Spring Boot can deploy it at startup.

## GitHub safety
Do not commit real Client Secrets. Keep only placeholders in the repository.
