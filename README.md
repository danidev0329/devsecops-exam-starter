# Macky Merch API - DevSecOps Starter

This is the starter repository for the **LSCS DevSecOps Engineering Take-Home Exam**. 

## Your Task
You are tasked with taking this baseline Express.js application and building a robust, secure, and automated CI/CD pipeline around it using **Docker** and **GitHub Actions**.

## Getting Started Locally
1. Clone your forked repository.
2. Install dependencies:
   ```bash
   npm install
   ```
3. Run the development server:
   ```bash
   npm start
   ```
4. Run the test suite:
   ```bash
   npm test
   ```

*The server will run on `http://localhost:3000`. You can verify the health endpoint at `http://localhost:3000/health`.*

## Guidelines
* **Do NOT modify** the core functionality of `server.js` or `server.test.js` unless fixing the deliberate vulnerability you introduce for testing your security scanner.
* Follow the instructions provided in your exam PDF strictly.
* Ensure you document your architecture, security choices, and challenges in your own `README.md` (you may overwrite this file for your final submission).

Good luck!

## Security Scanning Demo

To validate that the CI pipeline actually catches vulnerabilities,
`lodash` was intentionally changed to a known-vulnerable version
(4.17.15) in package.json.

The Trivy scan step in `.github/workflows/ci.yml` detected this
and failed the build with HIGH-severity findings:

[screenshot or pasted log of the failed Actions run]

This confirms the pipeline blocks merges/builds containing known
vulnerable dependencies. After confirming detection, lodash was
updated to a patched version (^4.17.21) and the pipeline passed.
