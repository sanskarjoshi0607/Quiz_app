# 🎓 QuizPortal — Full-Stack MERN Quiz Application

A modern, responsive, and beginner-friendly Quiz Portal built with **React**, **Express.js**, and **MongoDB** (MERN stack).

---

## 🌟 Key Features

- **🔐 Role-Based Authentication (Student & Teacher)**:
  - Secure password hashing using **`bcryptjs`** and **JWT session tokens**.
  - **🎓 Students**: Browse quizzes, take timed tests, review detailed explanations, and track personal score history under *"My Attempts"*.
  - **👨‍🏫 Teachers**: Build custom quizzes, delete quizzes, and monitor class-wide submissions.
  - **⚡ 1-Click Demo Logins**: Test both roles instantly with pre-loaded demo buttons (no manual typing needed).
- **🎯 Interactive Quiz Player**:
  - Live countdown timer with visual warning when time is running out.
  - Quick-jump question palette with visual answered/unanswered states.
  - Progress bar tracking quiz completion.
  - Secure server-side grading (answers are never exposed in browser network inspection).
- **📊 Comprehensive Results & Review**:
  - Overall score percentage badge with pass/review grading.
  - Stats for correct, incorrect, and time spent.
  - Question-by-question review highlighting your choice, correct answer, and an educational **explanation**.
- **🛠️ Custom Quiz Creator**:
  - Interactive form to create your own quizzes with customizable categories, difficulty levels, and timers.
  - Dynamic question and option fields with live validation.
  - Instantly saved and playable!
- **🏆 Scoreboard / Submissions History**:
  - Track recent attempts, completion dates, and scores.
- **🛡️ Beginner-Proof Zero-Crash Architecture**:
  - Includes an **automatic in-memory fallback mode**: if you haven't installed or connected MongoDB yet, the application still works immediately with 4 pre-loaded quizzes (React, JavaScript, Web Dev, Python)!
  - Real-time MongoDB status pill and setup modal in the navbar.
  - Standalone seeder (`npm run seed`) to populate MongoDB with 1 command.

---

## 🗂️ Project Structure

```text
Quiz/
├── package.json              # Root config with concurrently script (npm run dev)
├── README.md                 # Documentation and architecture guide
├── backend/                  # Express.js REST API & MongoDB
│   ├── config/
│   │   └── db.js             # Mongoose connection & in-memory fallback store
│   ├── controllers/
│   │   ├── authController.js # JWT registration, login, profile
│   │   └── quizController.js # REST API handlers (CRUD, grading, history)
│   ├── data/
│   │   └── sampleQuizzes.js  # Pre-seeded rich sample quizzes
│   ├── middleware/
│   │   └── authMiddleware.js # JWT protect & authorize middleware
│   ├── models/
│   │   ├── User.js           # Mongoose Schema with bcrypt password hashing
│   │   ├── Quiz.js           # Mongoose Schema for Quizzes & Questions
│   │   └── Submission.js     # Mongoose Schema for User Scores
│   ├── routes/
│   │   ├── authRoutes.js     # Auth API routes (/api/auth)
│   │   └── quizRoutes.js     # Quiz API routes (/api/quizzes, /api/submissions)
│   ├── scripts/
│   │   └── seed.js           # Standalone MongoDB database seeder
│   ├── test-integration.js   # Automated API test suite
│   ├── .env.example          # Environment variables template
│   ├── .env                  # Active environment variables
│   ├── server.js             # Express application entrypoint
│   └── package.json
└── frontend/                 # React SPA (Vite + React Router)
    ├── public/
    ├── src/
    │   ├── components/
    │   │   ├── AuthGateway.jsx       # Auth landing card (Student & Teacher)
    │   │   ├── AuthModal.jsx         # Login/register modal
    │   │   ├── ProtectedRoute.jsx    # URL route access guard
    │   │   ├── DbStatusModal.jsx     # Database guide modal
    │   │   ├── FilterBar.jsx         # Search, category & difficulty filters
    │   │   ├── Icons.jsx             # Clean, dependency-free SVG icons
    │   │   ├── Navbar.jsx            # Header & database status indicator
    │   │   ├── QuizCard.jsx          # Quiz preview card
    │   │   ├── QuizCreator.jsx       # Custom quiz creation form
    │   │   ├── QuizPlayer.jsx        # Quiz player with timer & progress
    │   │   ├── QuizResults.jsx       # Results breakdown & explanations
    │   │   └── SubmissionsHistory.jsx# Scoreboard / past attempts
    │   ├── services/
    │   │   └── api.js                # Centralized backend API client
    │   ├── App.jsx                   # Main React Routes & state controller
    │   ├── index.css                 # Modern CSS styles & responsive layout
    │   └── main.jsx                  # React DOM root with BrowserRouter
    ├── index.html
    ├── vite.config.js                # Vite config with API proxy to port 5000
    └── package.json
```

---

## 🚀 Quick Start Guide

### 1. Prerequisites
Ensure you have **Node.js** (v18 or higher) installed on your system. Check with:
```bash
node -v
npm -v
```

### 2. Start the Full Application
From the root `Quiz` directory, run:
```bash
npm run dev
```

This single command uses `concurrently` to start:
- 🚀 **Express Server**: `http://localhost:5000`
- 💻 **React Frontend**: `http://localhost:5173`

Open `http://localhost:5173` in your browser!

---

## 🍃 Connecting MongoDB (Optional but Recommended)

By default, the application runs out of the box in **Graceful Fallback Mode** with sample quizzes preloaded in memory.

When you're ready to persist quizzes and scores in real MongoDB:

### Option A: Free MongoDB Atlas Cloud (Recommended for beginners)
1. Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) and create a free M0 cluster.
2. Under **Database Access**, create a database user (e.g., `quizadmin` and a password).
3. Under **Network Access**, allow access from anywhere (`0.0.0.0/0`).
4. Click **Connect** -> **Connect your application** and copy the URI.
5. In [server/.env](file:///c:/Users/HP/OneDrive/Desktop/Quiz/server/.env), update `MONGODB_URI`:
   ```env
   PORT=5000
   MONGODB_URI=mongodb+srv://<username>:<password>@cluster0.abcde.mongodb.net/quiz_portal?retryWrites=true&w=majority
   ```
6. Populate sample quizzes into MongoDB:
   ```bash
   npm run seed
   ```

### Option B: Local MongoDB Server
1. Download and start MongoDB Community Server locally.
2. In [server/.env](file:///c:/Users/HP/OneDrive/Desktop/Quiz/server/.env):
   ```env
   PORT=5000
   MONGODB_URI=mongodb://127.0.0.1:27017/quiz_portal
   ```
3. Run the seeder:
   ```bash
   npm run seed
   ```

---

## 📡 REST API Reference

| Method | Endpoint | Description |
| :--- | :--- | :--- |
| `POST` | `/api/auth/register` | Register new account (Student or Teacher) with hashed password |
| `POST` | `/api/auth/login` | Authenticate user and return JWT session token |
| `GET` | `/api/auth/me` | Fetch authenticated user profile |
| `GET` | `/api/quizzes` | List all quizzes (supports `?category=...&difficulty=...&search=...`) |
| `GET` | `/api/quizzes/:id` | Fetch quiz for playing (correct answers hidden for security) |
| `POST` | `/api/quizzes/:id/submit` | Grade submitted answers and record score under student account |
| `POST` | `/api/quizzes` | Create a new quiz (Teacher role) |
| `DELETE`| `/api/quizzes/:id` | Delete a quiz by ID (Teacher role) |
| `GET` | `/api/submissions` | Retrieve submissions (supports `?mine=true` for logged-in user) |
| `GET` | `/api/status` | Get database connection status |

---

## 🧪 Testing

To run the automated backend API test suite:
```bash
npm run test
```

This verifies:
- Fetching quizzes and details
- Scoring calculation and percentage grading
- Custom quiz creation
- Quiz deletion and score logging
