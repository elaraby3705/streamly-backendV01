# Streamly Backend V01 

Secure OTT (Over-The-Top) Backend built with FastAPI, PostgreSQL, Stripe, and Mux — featuring user subscriptions, video upload, and protected video playback streaming.

---

## 🎯 Project Overview
Streamly Backend is a training project designed to simulate a real-world OTT platform backend similar to Netflix or Shahid.  
It handles user authentication, payment subscriptions via Stripe, and secure video management using Mux or Cloudflare Stream.

This project is meant for backend & DevOps training — including full documentation, ERD design, CI/CD, and deployment to Google Cloud Platform (GCP).

---

## 🧱 Tech Stack

- **Backend Framework:** FastAPI (Python)
- **Database:** PostgreSQL
- **Payment Gateway:** Stripe API
- **Video Streaming:** Mux API or Cloudflare Stream
- **Authentication:** JWT-based Auth
- **Docs:** Postman API collection + Markdown system design docs
- **Deployment:** Docker, Docker Compose, and GCP (Cloud Run + Cloud SQL)

---

## 📂 Project Structure

streamly-backend/
│
├── app/
│ ├── api/ # API endpoints
│ ├── core/ # config, JWT, etc.
│ ├── models/ # ORM models
│ └── services/ # business logic
│
├── database/
│ ├── schema.sql # PostgreSQL schema
│ └── migrations/ # Alembic (optional)
│
├── docs/
│ ├── erd.png # ERD diagram
│ ├── database.md # DB documentation
│ └── system-design.md # architecture overview
│
├── tests/ # unit tests
├── Dockerfile
├── docker-compose.yml
└── README.md


---

## 📘 Features

- ✅ User registration & login (JWT authentication)
- 💳 Stripe payment integration for subscriptions
- 🎥 Mux or Cloudflare secure video upload and playback
- 🗃️ PostgreSQL relational data model (Users, Subscriptions, Videos, Payments)
- 🧾 Webhook processing from Stripe events
- 🧩 Postman API documentation
- ☁️ GCP deployment (Cloud Run + Cloud SQL)

---

## 🧩 Database Design (ERD Summary)

- **User** → has many **Subscriptions**
- **User** → uploads many **Videos**
- **User** → has many **Payment Logs**

Tables:
1. `users`
2. `subscriptions`
3. `videos`
4. `payment_logs`

All diagrams and schema files are located in `/docs/`.

---

## 🚀 Project Roadmap (Milestones)

**Milestone 1:** Authentication & User Management  
- Register/Login endpoints  
- JWT setup  
- Basic PostgreSQL integration  

**Milestone 2:** Stripe Integration  
- Checkout session creation  
- Webhook handling  
- Subscription activation  

**Milestone 3:** Video Management  
- Admin video upload to Mux  
- Secure token-based playback  

**Milestone 4:** Docs & Deployment  
- Postman API documentation  
- Docker setup  
- GCP Cloud Run deployment  

---

## 🧰 Setup Instructions

1. Clone the repository:
- bash
````
git clone https://github.com/yourusername/streamly-backend.git

cd streamly-backend
````

2. Create a `.env` file:
````
DATABASE_URL=postgresql://user:password@db:5432/streamly
STRIPE_SECRET_KEY=your_stripe_key
MUX_TOKEN_ID=your_mux_token
MUX_TOKEN_SECRET=your_mux_secret
JWT_SECRET=your_secret
````

3. Build and run containers:

4. Access the API at:


---

## 🤝 Contribution
This project is open for learning collaboration.  
Feel free to fork, submit pull requests, or contribute with documentation and improvements.

---

## 📄 License
This project is licensed under the **MIT License**.

---

## 👤 Author
**Hammad Ibrahim Muhammed**  
Backend & DevOps Engineer  
📧 h.ibrahim3705@example.com  
🌍 [GitHub Profile](https://github.com/hmmad-ibrahim)



