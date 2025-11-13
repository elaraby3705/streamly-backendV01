# 🗂️ Database Design: Streamly OTT Backend

This document outlines the database schema, entities, and relationships for the Streamly project. The database is designed using **PostgreSQL** to handle user data, subscription states, and video metadata.

---

## 1. Entity Relationship Diagram (ERD)

### Visual Diagram Placeholder


*(**Note:** Please replace the above tag with an actual visual diagram image, such as `![ERD Visualization](erd.png)`, once you've generated it.)*

### Relationship Logic Summary
The core relationships ensure data integrity and track access rights:

| Entity A | Relationship | Entity B | Details |
| :--- | :--- | :--- | :--- |
| **Users** | One-to-Many (1:∞) | **Subscriptions** | Tracks current and historical subscription records for each user. |
| **Users** | One-to-Many (1:∞) | **Videos** | Links video assets back to the specific admin user who uploaded them. |
| **Users** | One-to-Many (1:∞) | **PaymentLogs** | Associates webhook events with the relevant user account. |

---

## 2. Table & Column Descriptions

### `users`
Stores user identity, authentication credentials, and roles.

| Column | Type | Description | Constraints |
| :--- | :--- | :--- | :--- |
| **id** | `SERIAL` | Unique User ID (Primary Key). | `PRIMARY KEY` |
| **full_name** | `VARCHAR(100)` | User's full name. | |
| **email** | `VARCHAR(255)` | User's unique email address. | `UNIQUE`, `NOT NULL` |
| **password_hash** | `VARCHAR(255)` | Hashed and salted password. | `NOT NULL` |
| **role** | `user_role` | User role (`'user'` or `'admin'`). | Default `'user'` |

### `subscriptions`
Tracks the user's billing status and relationship with **Stripe**.

| Column | Type | Description | Constraints |
| :--- | :--- | :--- | :--- |
| **id** | `SERIAL` | Unique Subscription Record ID (Primary Key). | `PRIMARY KEY` |
| **user_id** | `INT` | Foreign key linking to the `users` table. | `NOT NULL`, `REFERENCES users` |
| **stripe_subscription_id** | `VARCHAR(255)` | The unique ID from Stripe for this subscription. | `UNIQUE` |
| **status** | `subscription_status` | The current state (`'active'`, `'canceled'`, etc.). | `NOT NULL` |
| **current_period_end** | `TIMESTAMP WITH TIME ZONE` | When the current paid period ends. | |

### `videos`
Stores metadata for content assets managed by **Mux/Cloudflare Stream**.

| Column | Type | Description | Constraints |
| :--- | :--- | :--- | :--- |
| **id** | `SERIAL` | Unique Video ID (Primary Key). | `PRIMARY KEY` |
| **title** | `VARCHAR(255)` | Public title of the video. | `NOT NULL` |
| **mux_asset_id** | `VARCHAR(255)` | The Asset ID within the streaming provider. | |
| **playback_id** | `VARCHAR(255)` | The secure Playback ID used for URL generation. | |
| **uploaded_by** | `INT` | Foreign key linking to the admin `users` table. | `REFERENCES users` |

### `payment_logs`
An audit trail for all incoming **Stripe webhooks**.

| Column | Type | Description | Constraints |
| :--- | :--- | :--- | :--- |
| **id** | `SERIAL` | Unique Log ID (Primary Key). | `PRIMARY KEY` |
| **stripe_event_id** | `VARCHAR(255)` | Unique ID of the event from Stripe. | `UNIQUE` |
| **event_type** | `VARCHAR(100)` | The type of event (e.g., `invoice.paid`). | |
| **payload** | `JSONB` | The full, raw JSON payload from the webhook. | |
| **user_id** | `INT` | User associated with the event (if identifiable). | `REFERENCES users` |

---

## 3. SQL Schema (PostgreSQL DDL)

This DDL script creates the necessary types, tables, and constraints.

```sql
-- Create custom ENUM types for roles and status
CREATE TYPE user_role AS ENUM ('user', 'admin');
CREATE TYPE subscription_status AS ENUM ('active', 'canceled', 'expired', 'incomplete');

-- Table: users
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    full_name VARCHAR(100),
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    role user_role DEFAULT 'user',
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Table: subscriptions
CREATE TABLE subscriptions (
    id SERIAL PRIMARY KEY,
    user_id INT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    stripe_subscription_id VARCHAR(255) UNIQUE,
    status subscription_status NOT NULL,
    current_period_start TIMESTAMP WITH TIME ZONE,
    current_period_end TIMESTAMP WITH TIME ZONE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Table: videos
CREATE TABLE videos (
    id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    mux_asset_id VARCHAR(255),
    playback_id VARCHAR(255),
    uploaded_by INT REFERENCES users(id) ON DELETE SET NULL, -- Admin who uploaded
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Table: payment_logs (Audit trail for webhooks)
CREATE TABLE payment_logs (
    id SERIAL PRIMARY KEY,
    user_id INT REFERENCES users(id) ON DELETE SET NULL, -- Can be null if user isn't identified
    event_type VARCHAR(100),
    stripe_event_id VARCHAR(255) UNIQUE, -- Prevents duplicate processing
    payload JSONB,
    received_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Create indexes for faster lookups
CREATE INDEX idx_subscriptions_user_id ON subscriptions(user_id);
CREATE INDEX idx_videos_uploaded_by ON videos(uploaded_by);