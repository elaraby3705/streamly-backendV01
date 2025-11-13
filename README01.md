# Streamly Backend: Secure OTT API with FastAPI, Stripe, and Mux

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python Version](https://img.shields.io/badge/Python-3.11%2B-blue)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/Framework-FastAPI-009688)](https://fastapi.tiangolo.com/)

## 💡 Project Idea & Goal

Streamly Backend is a highly secure and production-ready reference architecture for building modern Over-The-Top (OTT) or Subscription Video-on-Demand (SVOD) platforms.

The core goal is to provide robust API endpoints that handle:
1.  **Authentication:** Secure registration and login using JWT.
2.  **Monetization:** Full integration with **Stripe** for recurring subscriptions.
3.  **Content Protection:** Secure delivery of video assets using tokenized URLs from services like **Mux** or **Cloudflare Stream**.

## 🛠️ Tech Stack

| Category | Technology | Purpose |
| :--- | :--- | :--- |
| **Backend** | Python, FastAPI | High-performance API development. |
| **Database** | PostgreSQL (via Cloud SQL) | Reliable relational data storage. |
| **Containers** | Docker, docker-compose | Local development and Cloud Run deployment. |
| **Payments** | Stripe API & Webhooks | Subscription management and billing logic. |
| **Streaming** | Mux / Cloudflare Stream | Video encoding, hosting, and playback security. |
| **Deployment** | Google Cloud Run, Cloud SQL | Serverless and scalable infrastructure. |

## 🏗️ System Design Summary

The system architecture is divided into three main service areas:

* **Authentication Service:** Handles user creation and JWT generation/validation.
* **Subscription Service:** Manages Stripe API calls (checkout creation) and listens for Webhooks to update the local database.
* **Video Service:** Admin endpoints for uploading and secure user endpoints that check subscription status before generating a signed playback URL.



[Image of Streamly backend system architecture diagram]


### Data Flow Logic (Simplified)

User $\rightarrow$ Authenticate (JWT) $\rightarrow$ Initiate Stripe Payment $\rightarrow$ Stripe sends Webhook to Backend $\rightarrow$ Backend updates Subscription Status $\rightarrow$ User requests Video $\rightarrow$ Backend confirms `status='active'` $\rightarrow$ Backend returns secure Mux Playback URL.

*(For a full breakdown, see the **System Design Document** in the `/docs` folder.)*

## ⚙️ Installation and Local Setup

### Prerequisites

* Docker and Docker Compose
* Python 3.11+
* Stripe Account (for API Keys)
* Mux Account (for API Keys)

### Steps

1.  **Clone the Repository:**
    ```bash
    git clone [https://github.com/YourUsername/streamly-backend.git](https://github.com/YourUsername/streamly-backend.git)
    cd streamly-backend
    ```
2.  **Configure Environment Variables:**
    Copy the sample environment file:
    ```bash
    cp .env.example .env
    # Edit .env with your PostgreSQL credentials, Stripe keys, and Mux keys.
    ```
3.  **Run with Docker Compose:**
    ```bash
    docker-compose up --build
    ```
    The FastAPI application will be accessible at `http://localhost:8000`. The database will be running on the internal network.

## 🚀 Milestones & Development Roadmap

We are tracking progress through GitHub Milestones:

| Milestone | Target | Status |
| :--- | :--- | :--- |
| **1: Authentication & Users** | Register/Login API, JWT Integration, Basic DB setup. | ⚪ In Progress |
| **2: Stripe & Subscription Logic** | Checkout Session APIs, Webhook Handler, Subscription status. | ⚪ To Do |
| **3: Video Management** | Admin Upload API (Mux Integration), Secure Playback API. | ⚪ To Do |
| **4: Docs + Deployment** | Final Documentation, Postman Collection, CI/CD to GCP. | ⚪ To Do |

## 🤝 Contribution Guidelines

We welcome contributions! This project is intended to be a collaborative learning resource.

### How to Contribute

1.  **Check Issues:** Review the [Issues tab](YOUR_REPO_URL/issues) for tasks labeled **`good first issue`** or **`enhancement`**.
2.  **Fork the Repo:** Create a fork of `streamly-backend`.
3.  **Branch:** Create a feature branch: `git checkout -b feature/your-feature-name`.
4.  **Develop:** Write your code, ensuring tests pass.
5.  **Commit:** Use clear, descriptive commit messages.
6.  **Pull Request (PR):** Open a PR against the `main` branch. A project maintainer will review it.

*(For detailed PR and code style guidelines, please refer to the **`CONTRIBUTING.md`** file.)*

## 🐛 Reporting Issues

If you find a bug, please open a new issue.
When reporting:
* Use the **Bug Report template**.
* Describe the steps to reproduce the error clearly.
* Include your environment details (OS, Python version, etc.).

## 📚 Documentation and API Endpoints

### Project Documentation
All non-code documentation (ERD, System Design) is located in the `/docs` directory.

### API Documentation
FastAPI provides automatic OpenAPI documentation. Once the server is running:
* **Interactive Docs (Swagger UI):** `http://localhost:8000/docs`
* **Redoc Documentation:** `http://localhost:8000/redoc`

**Postman Collection:**
A public Postman Collection is available for testing endpoints:
[API Documentation Link (Placeholder for your Public Link)]

---

## ⚖️ License

This project is licensed under the **MIT License**. See the `LICENSE` file for details.