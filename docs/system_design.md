# 🏗️ System Design Document: Streamly OTT Backend

This document details the architecture, core components, and data flow for the Streamly OTT Backend API, built for scalability and security.

---

## 1. System Overview and Goals

The Streamly Backend is a secure, scalable API designed to manage user authentication, paid subscriptions (via **Stripe**), and secure video playback authorization (via **Mux**). It utilizes a modern containerized architecture deployed on Google Cloud Platform (GCP).

### Key Goals:
* **Security:** Implement robust JWT-based authentication and secure handling of sensitive data (e.g., Stripe webhooks).
* **Scalability:** Utilize containerized microservices (FastAPI) deployed on Cloud Run for automatic scaling.
* **Monetization:** Fully integrate Stripe for reliable subscription lifecycle management.
* **Content Protection:** Authorize video playback dynamically to prevent unauthorized access.

---

## 2. Technology Stack (Core Components)

The system relies on the following technologies:

| Component | Technology | Rationale |
| :--- | :--- | :--- |
| **API Framework** | **FastAPI (Python)** | High performance, asynchronous (async/await), automatic OpenAPI documentation, and robust dependency injection. |
| **Database** | **PostgreSQL (via Cloud SQL)** | Mature, reliable, and supports complex relational data crucial for tracking users and subscriptions. |
| **Subscription Mgmt.** | **Stripe** | Industry-standard for payment gateway and recurring billing; provides secure webhook functionality. |
| **Video Processing** | **Mux / Cloudflare Stream** | Handles video transcoding, storage, and generating time-limited, signed/tokenized playback URLs. |
| **Authentication** | **JWT (JSON Web Tokens)** | Stateless authentication mechanism for API requests. |
| **Deployment** | **Docker + GCP Cloud Run** | Simplifies deployment and provides serverless container execution with auto-scaling. |

---

## 3. Architecture Overview (Component Diagram)

The system leverages managed services on GCP and external APIs to maintain a lean and focused backend logic.



### Component Breakdown:

1.  **Client:** Web or mobile application consuming the FastAPI API.
2.  **FastAPI Backend (Cloud Run):**
    * **Public API:** Handles Auth (Login/Register), Subscription initiation, and Video access requests.
    * **Webhook Listener:** Dedicated endpoint to securely receive and process asynchronous events from Stripe.
3.  **PostgreSQL (Cloud SQL):** Stores persistent data (Users, Subscriptions, Video Metadata).
4.  **Stripe API:** Handles billing, payments, and subscription state changes.
5.  **Mux/Stream API:** Provides the CDN, video assets, and the mechanism to generate secure **Playback IDs** or signed URLs.

---

## 4. Data Flow Diagram (The Core Workflow)

This sequence outlines the critical path for a user to gain and maintain access to content.

### A. Authentication & Subscription Initiation:

1.  **User Login:** Client sends credentials to FastAPI.
2.  **Token Issuance:** FastAPI validates credentials and issues a **JWT**.
3.  **Create Checkout:** Client (with JWT) requests FastAPI to start a subscription.
4.  **Stripe Session:** FastAPI calls Stripe API to create a **Checkout Session** URL.
5.  **Payment:** Client is redirected to Stripe to complete the payment.

### B. Webhook Handling (Subscription Status Update):

1.  **Stripe Event:** Upon successful payment, Stripe sends a **Webhook Event** (e.g., `checkout.session.completed` or `invoice.paid`) to the secure FastAPI Webhook Listener endpoint.
2.  **Verification:** FastAPI verifies the webhook signature using the Stripe secret key.
3.  **Database Update:** FastAPI processes the payload, finds the associated user, and updates the `subscriptions` table status to `'active'` in **PostgreSQL**.
4.  **Acknowledge:** FastAPI returns a `200 OK` to Stripe.

### C. Secure Video Playback Authorization:

1.  **Access Request:** Client sends a request for a specific video asset (e.g., `/videos/{video_id}`) using the **JWT** in the header.
2.  **Authorization Check:** FastAPI performs two checks:
    * **Authentication:** Is the JWT valid?
    * **Authorization:** Is the user's `subscription.status` `'active'` in the DB?
3.  **Generate Secured URL:** If authorized, FastAPI calls **Mux/Stream** to generate a time-limited, **signed playback URL** for the video asset's Playback ID.
4.  **Content Delivery:** FastAPI returns the secured URL to the Client. The Client then uses this URL to stream content directly from the Mux CDN.

---

## 5. Database Design Summary (ERD Logic)

The persistence layer is anchored by four key tables (Entities):

| Entity | Primary Role | Relationships |
| :--- | :--- | :--- |
| **Users** | Stores identity and authentication details. | **1-to-Many** with Subscriptions, Videos (uploader), and PaymentLogs. |
| **Subscriptions** | Tracks the active status and Stripe ID for billing continuity. | **Many-to-One** with Users. |
| **Videos** | Stores video metadata, description, and the Mux `asset_id`/`playback_id`. | **Many-to-One** with Users (uploader/admin). |
| **PaymentLogs** | Stores raw Stripe webhook events for auditing and tracing. | **Many-to-One** with Users (optional link). |

*(For the detailed SQL Schema and ERD visualization, see the **`database.md`** file in this directory.)*