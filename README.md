# Macky Merch API - DevSecOps Starter

This is the starter repository for the **LSCS DevSecOps Engineering Take-Home Exam**. 

## Getting Started Locally
1. Install dependencies:
   ```bash
   npm install
   ```
2. Run the development server:
   ```bash
   npm start
   ```
3. Run the test suite:
   ```bash
   npm test
   ```

*The server will run on `http://localhost:3000`. You can verify the health endpoint at `http://localhost:3000/health`.*

### Run with Docker

1. Build the Docker image:
   ```bash
   docker build -t express-health-app .
   ```
2. Run the container and map port 3000:
   ```bash
   docker run -p 3000:3000 express-health-app
   ```
3. Verify the health endpoint at `http://localhost:3000/health`.

## Architectural Explanation

### Base Image: `node:20-alpine`

The Dockerfile uses `node:20-alpine` instead of `node:latest` for a few reasons:

- **Minimal footprint** — Alpine Linux is a musl-based, extremely small distribution (~5 MB), which keeps the final image size and attack surface low.
- **Long-Term Support (LTS)** — Node 20 is an actively maintained LTS release, receiving regular security patches. `node:latest` is an untagged, moving target that can change underneath you, making builds non-reproducible.
- **Reduced attack surface** — a full `node` image ships a full OS with far more installed packages, increasing the number of potential vulnerabilities.

### Security Scanner: Trivy

The pipeline uses **Trivy** (via `aquasecurity/trivy-action`) for container image scanning:

- **Open-source and fast** — it scans images in seconds and is widely adopted in the DevSecOps community.
- **Comprehensive vulnerability database** — it detects known CVEs in both OS packages and language-specific dependencies (including npm packages).
- **Native GitHub Action** — integrates directly into the workflow as a step.
- **Fail-fast gating** — configured with `severity: HIGH,CRITICAL` and `exit-code: 1`, so the build fails whenever high or critical vulnerabilities are present.

## Vulnerability Demonstration

To validate that the CI pipeline actually catches vulnerabilities,
`lodash` was intentionally changed to a known-vulnerable version
(4.17.15) in package.json.

The Trivy scan step in `.github/workflows/ci.yml` detected this
and failed the build with HIGH-severity findings:

![Vulnerability 1](./images/vulnerability-check-1.png)
![Vulnerability 2](./images/vulnerability-check-2.png)

This confirms the pipeline blocks merges/builds containing known
vulnerable dependencies. After confirming detection, lodash was
updated to a patched version (^4.17.21) and the pipeline passed.

## Challenges Faced

> TODO: Replace this with a description of a challenge you encountered while learning/building this project and how you solved it.

