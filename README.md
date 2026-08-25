# 🗳️ CivicPulse — E-Consultation Sentiment Analysis

A full-stack **MERN** application that captures public feedback during government e-consultations and automatically classifies each comment as **Positive**, **Negative**, or **Neutral** using AFINN-based sentiment analysis.

![React](https://img.shields.io/badge/REACT-18-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Vite](https://img.shields.io/badge/VITE-BUNDLER-646CFF?style=for-the-badge&logo=vite&logoColor=white)
![Node.js](https://img.shields.io/badge/NODE.JS-18+-339933?style=for-the-badge&logo=node.js&logoColor=white)
![Express](https://img.shields.io/badge/EXPRESS-4-000000?style=for-the-badge&logo=express&logoColor=white)
![MongoDB](https://img.shields.io/badge/MONGODB-ATLAS-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![JWT](https://img.shields.io/badge/JWT-AUTHENTICATION-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/TAILWIND%20CSS-3-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)
![License](https://img.shields.io/badge/LICENSE-MIT-green?style=for-the-badge)

---

## 📑 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [Environment Variables](#-environment-variables)
- [Installation](#-installation)
- [Running the App](#-running-the-app)
- [Admin Access](#-admin-access)
- [API Reference](#-api-reference)
- [Pages & Routes](#-pages--routes)
- [How Sentiment Analysis Works](#-how-sentiment-analysis-works)
- [Screenshots](#-screenshots)
- [License](#-license)

---

## 📌 Overview

**CivicPulse** replaces manual triage of public consultation responses with an automated, transparent, and auditable sentiment pipeline. Citizens register, submit comments tied to a consultation topic, and instantly see how their feedback is scored. Administrators get an analytics dashboard with charts, trend lines, and a searchable table of all submissions.

---

## ✨ Features

### 🧑‍🤝‍🧑 Citizen-facing
- Register and log in with a verified account
- Submit comments against predefined consultation topics
- Instant sentiment classification on submission (Positive / Negative / Neutral)
- Personal dashboard with:
  - Summary stat cards (total, positive share, breakdown)
  - 14-day sentiment trend line chart
  - Donut chart of sentiment mix
  - Top keyword cloud extracted from scored words
  - Recent comment history with sentiment badges
- Detailed result view for every submitted comment

### 🛡️ Admin-facing
- Role-protected Admin Panel (`/admin`)
- Aggregate stats: total comments, unique respondents, positive share
- Public mood gauge (arc visualisation)
- Sentiment distribution pie chart
- 14-day trend line chart (system-wide)
- Searchable, filterable table of all submissions with user info

### ⚙️ Platform
- JWT authentication with role-based access control (user / admin)
- Password hashing with bcrypt
- Rate limiting on all `/api` routes (300 req / 15 min)
- Input validation and sanitisation on both client and server
- CORS configuration via environment variable
- Seed script to bootstrap an admin account

---

## 🚀 Tech Stack

| Layer | Technology |
|---|---|
| **Frontend** | React 18, Vite, Tailwind CSS, Recharts, Lucide React |
| **Backend** | Node.js, Express 4 |
| **Database** | MongoDB with Mongoose |
| **Auth** | JWT (jsonwebtoken), bcryptjs |
| **Sentiment Engine** | AFINN lexicon via `sentiment` npm package |
| **HTTP Client** | Axios |
| **Routing** | React Router v6 |

---

## 📁 Project Structure

```
econsult-sentiment/
├── client/                   # React frontend (Vite)
│   ├── src/
│   │   ├── api/              # Axios instance
│   │   ├── components/       # Reusable UI components
│   │   │   ├── AppLayout.jsx
│   │   │   ├── Loader.jsx
│   │   │   ├── MoodFace.jsx
│   │   │   ├── ProtectedRoute.jsx
│   │   │   ├── PublicHeader.jsx
│   │   │   ├── SentimentGauge.jsx
│   │   │   ├── Sidebar.jsx
│   │   │   └── TopBar.jsx
│   │   ├── context/
│   │   │   └── AuthContext.jsx   # Auth state (login / signup / logout)
│   │   ├── pages/
│   │   │   ├── Home.jsx          # Landing page
│   │   │   ├── Login.jsx
│   │   │   ├── Signup.jsx
│   │   │   ├── Dashboard.jsx     # Personal feedback dashboard
│   │   │   ├── SubmitFeedback.jsx
│   │   │   ├── Results.jsx       # Single comment result view
│   │   │   ├── AdminPanel.jsx    # Admin analytics
│   │   │   └── NotFound.jsx
│   │   ├── App.jsx
│   │   └── main.jsx
│   ├── index.html
│   ├── vite.config.js
│   └── tailwind.config.js
│
└── server/                   # Express backend
    ├── config/
    │   └── db.js             # Mongoose connection
    ├── middleware/
    │   └── auth.js           # protect + adminOnly middleware
    ├── models/
    │   ├── User.js
    │   └── Feedback.js
    ├── routes/
    │   ├── auth.js           # /api/auth
    │   └── feedback.js       # /api/feedback
    ├── utils/
    │   ├── generateToken.js
    │   ├── seedAdmin.js      # One-off admin bootstrap script
    │   └── sentimentAnalyzer.js
    └── server.js
```

---

## 🏁 Getting Started

### Prerequisites
- Node.js v18 or later
- MongoDB — local instance (`mongodb://127.0.0.1:27017`) or a MongoDB Atlas connection string

---

## 🔐 Environment Variables

**`server/.env`** — create by copying `server/.env.example`:

```env
MONGO_URI=mongodb://127.0.0.1:27017/econsult_sentiment
PORT=5000
JWT_SECRET=replace_this_with_a_long_random_secret_key
JWT_EXPIRES_IN=7d
CLIENT_ORIGIN=http://localhost:5173
```

**`client/.env`** — create by copying `client/.env.example`:

```env
VITE_API_URL=http://localhost:5000/api
```

---

## 📦 Installation

```bash
# Install server dependencies
cd server
npm install

# Install client dependencies
cd ../client
npm install
```

---

## ▶️ Running the App

Open two terminals:

**Terminal 1 — Backend**
```bash
cd server
npm run dev
# Server starts on http://localhost:5000
```

**Terminal 2 — Frontend**
```bash
cd client
npm run dev
# Client starts on http://localhost:5173
```

---

## 👑 Admin Access

Admin accounts are not created through the public signup form. Use the seed script:

```bash
cd server
npm run seed:admin
```

This creates (or promotes) an account with these default credentials:

| Field | Value |
|---|---|
| Email | `admin@econsult.gov` |
| Password | `Admin@123` |

To use custom credentials, add these to `server/.env` before running the script:

```env
SEED_ADMIN_NAME=Your Name
SEED_ADMIN_EMAIL=you@example.com
SEED_ADMIN_PASSWORD=YourPassword123
```

> If the email already exists in the database, the script promotes that account to admin rather than creating a duplicate.

After logging in as admin, navigate to `/admin` to access the analytics panel.

---

## 🔌 API Reference

All routes are prefixed with `/api`. Protected routes require a `Bearer <token>` header.

### Auth — `/api/auth`

| Method | Endpoint | Access | Description |
|---|---|---|---|
| POST | `/signup` | Public | Register a new user |
| POST | `/login` | Public | Log in and receive a JWT |
| GET | `/me` | Private | Get the current user's profile |

### Feedback — `/api/feedback`

| Method | Endpoint | Access | Description |
|---|---|---|---|
| POST | `/` | Private | Submit a comment (runs sentiment analysis) |
| POST | `/analyze` | Private | Analyze text without saving (live preview) |
| GET | `/my` | Private | Get all feedback submitted by the logged-in user |
| GET | `/` | Admin | Get all feedback in the system |
| GET | `/stats` | Admin | Get aggregated sentiment statistics |
| GET | `/:id` | Private | Get a single feedback entry (owner or admin) |

### Health Check
```
GET /api/health
```

---

## 🗺️ Pages & Routes

| Path | Access | Description |
|---|---|---|
| `/` | Public | Landing / marketing page |
| `/login` | Public | Login form |
| `/signup` | Public | Registration form |
| `/dashboard` | Private | Personal sentiment dashboard |
| `/submit-feedback` | Private | Submit a new comment |
| `/results/:id` | Private | Sentiment result for a single submission |
| `/admin` | Admin only | System-wide analytics panel |

---

## 🧠 How Sentiment Analysis Works

The server uses the `sentiment` npm package, which implements the **AFINN-165 lexicon** — a list of English words each assigned a valence score from **-5** (very negative) to **+5** (very positive).

For every submitted comment:

1. Each word is looked up in the AFINN lexicon and its score is summed.
2. The **comparative score** (total ÷ word count) normalises for comment length.
3. A **±0.15 neutral band** is applied — comparative scores within that band are labelled *Neutral* to avoid tipping mildly-worded comments on a single word.

```
comparative > +0.15  →  Positive
comparative < -0.15  →  Negative
otherwise            →  Neutral
```

The response also includes the matched `positiveWords` and `negativeWords` arrays so the UI can surface which specific words drove the score.

---

## 🖼️ Screenshots

| Home Page | Dashboard |
|---|---|
| ![Home](./screenshots/home.png) | ![Dashboard](./screenshots/dashboard.png) |

| Submit Feedback | Admin Panel |
|---|---|
| ![Submit Feedback](./screenshots/submit-feedback.png) | ![Admin Panel](./screenshots/admin-panel.png) |

> 📌 Replace the paths above with your actual screenshots. Create a `screenshots/` folder in your repo root, drop your images in (`home.png`, `dashboard.png`, `submit-feedback.png`, `admin-panel.png`), and they'll render automatically on GitHub.

---

## 📄 License

This project is intended for academic and government demonstration purposes, licensed under **MIT**.

---

<p align="center">Made with ❤️ for smarter civic engagement</p>
