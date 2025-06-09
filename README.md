# CI/CD Pipeline Setup with GitHub Actions

> **Challenge 19: CI-CD – GitHub Actions CI/CD Setup**

This repository demonstrates a full CI/CD pipeline using GitHub Actions for a React + Express + MongoDB quiz application.  
Pull Requests targeting the `develop` branch automatically run Cypress component tests. Merges to `master` automatically trigger a new deployment to Render via a deploy hook.

---

## 🔗 Live Sites

- **Frontend (Static Site)**  
  https://challenge-20-deployment-ci-cd.onrender.com

- **Backend (Web Service)**  
  https://challenge-20-deployment-ci-cd-server.onrender.com

---

## 📂 Repository

https://github.com/Doom825/challenge-20-deployment-ci-cd
---

## 📝 Table of Contents

1. [Project Overview](#project-overview)  
2. [Branching Strategy](#branching-strategy)  
3. [CI Pipeline (Pull Requests → develop)](#ci-pipeline-pull-requests--develop)  
4. [CD Pipeline (Merges → master)](#cd-pipeline-merges--master)  
5. [Local Setup & Testing](#local-setup--testing)  


---

## 🔍 Project Overview

As applications grow, it’s critical to enforce quality checks and automate deployments. This project implements:

- **Continuous Integration (CI)**  
  Runs Cypress component tests on every Pull Request to `develop`.  
- **Continuous Deployment (CD)**  
  Automatically redeploys the static frontend and backend on each merge to `master`.  

The result is a fail-fast workflow: buggy code never reaches production, and approved changes go live immediately.

---

## 🌲 Branching Strategy

- **`master`**  
  Production-ready code. Merges here trigger a full redeploy.  
- **`develop`**  
  Integrates feature branches. Pull Requests here run the CI pipeline.  
- **`feature/*`**  
  Short-lived branches branched off `develop` for new work. Merge back into `develop` when ready.

---

## ⚙️ CI Pipeline (Pull Requests → develop)

Defined in **`.github/workflows/ci.yml`**:

```yaml
name: CI – Cypress Component Tests

on:
  pull_request:
    branches: [ develop ]

jobs:
  cypress-component:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: node-version: '18'
      - run: npm ci
      - run: npm run build --if-present    # build static client if present
      - run: npx cypress run --component    # execute component tests


