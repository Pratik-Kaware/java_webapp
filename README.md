Enterprise Java Web Application

[cite_start]This repository contains the foundational source code for our intentionally simple Spring Boot application[cite: 298]. [cite_start]It is designed specifically to act as the "patient" for our enterprise CI/CD pipeline [cite: 327][cite_start], strictly adhering to 12-Factor App principles[cite: 406].

## Architecture & Tech Stack
* [cite_start]**Language/Framework:** Java 17+, Spring Boot 3.x [cite: 300, 391]
* [cite_start]**Build Tool:** Maven [cite: 391]
* [cite_start]**Database:** In-memory H2 (Allows for robust unit testing without needing a standalone database container) [cite: 301, 392]
* [cite_start]**Endpoints:** * `/api/users` (Standard CRUD operations for future DAST and Performance testing) [cite: 399]
  * [cite_start]`/actuator/health` (Crucial for Kubernetes liveness and readiness probes) [cite: 302, 397]
* [cite_start]**Containerization:** Multi-stage Dockerfile [cite: 394]

## Enterprise Design Decisions
* **Repository Segregation:** We decouple application code from deployment manifests. [cite_start]This prevents a simple Helm chart typo from triggering a full Java rebuild[cite: 287, 376].
* **Multi-Stage Dockerfile:** We use a heavy Maven image for compiling, but only copy the compiled `.jar` into a lightweight Alpine JRE image. [cite_start]This minimizes our attack surface and speeds up container pull times in production[cite: 310, 408, 410, 434].
* [cite_start]**Git Ignore:** We never commit compiled binaries (`/target`) or IDE settings to Git to prevent repository bloat[cite: 405, 412].

## Local Development & Validation
To build and test this application locally:

1. **Run Unit Tests:**
   ```bash
   mvn clean test

    Build Container Image:
    Bash

    docker build -t local-java-webapp:v1 .

    Run the Application:
    Bash

    docker run -p 8080:8080 local-java-webapp:v1

    Validate Health:
    Bash

    curl http://localhost:8080/actuator/health
