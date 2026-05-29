# TaskFlow — Task Management Application

A full-stack task management web application built with **React.js**, **Node.js + Express.js**, and **MongoDB**.

---

## Tech Stack

| Layer      | Technology                                      |
|------------|-------------------------------------------------|
| Frontend   | React 18, React Router v6, Context API, CSS     |
| Backend    | Node.js, Express.js, JWT Auth, Mongoose         |
| Database   | MongoDB (via Mongoose ODM)                      |
| DevOps     | Docker, Docker Compose, Nginx                   |
| Docs       | Swagger / OpenAPI 3.0                           |

---

## Features

### Frontend
- User authentication (Login / Register)
- Protected routes with JWT
- Dashboard with live stats + progress bar
- Create, edit, delete, and toggle tasks
- Filter by status (All / Pending / Completed)
- Filter by priority (High / Medium / Low)
- Debounced live search
- Responsive layout (mobile, tablet, desktop)
- Dark mode toggle (persisted)
- Toast notifications
- Form validation
- Lazy-loaded pages (code splitting)

### Backend
- JWT authentication (register / login / profile)
- CRUD REST API for tasks
- Pagination + search + filters
- Role-based access control (user / admin)
- Rate limiting (100 req / 15 min)
- Helmet security headers
- Centralized error handling
- Input validation (express-validator)
- Swagger API docs at `/api/docs`
- Unit tests (Jest + Supertest)

---

## Folder Structure

```
taskflow/
├── backend/
│   ├── config/
│   │   ├── database.js         # MongoDB connection
│   │   └── swagger.js          # Swagger config
│   ├── controllers/
│   │   ├── auth.controller.js
│   │   ├── task.controller.js
│   │   └── user.controller.js
│   ├── middleware/
│   │   ├── auth.middleware.js  # JWT protect + authorize
│   │   ├── error.middleware.js # Global error handler
│   │   └── validate.middleware.js
│   ├── models/
│   │   ├── User.model.js
│   │   └── Task.model.js
│   ├── routes/
│   │   ├── auth.routes.js
│   │   ├── task.routes.js
│   │   └── user.routes.js
│   ├── tests/
│   │   └── auth.test.js
│   ├── utils/
│   │   ├── error.utils.js
│   │   └── jwt.utils.js
│   ├── app.js
│   ├── server.js
│   └── package.json
│
├── frontend/
│   ├── public/
│   │   └── index.html
│   └── src/
│       ├── components/
│       │   ├── auth/           # LoginForm, RegisterForm
│       │   ├── common/         # Button, Input, Modal, Badge, Spinner, EmptyState, ConfirmDialog
│       │   ├── dashboard/      # StatsCard, RecentTasks
│       │   ├── layout/         # AppLayout, Sidebar, TopBar
│       │   └── tasks/          # TaskCard, TaskForm, TaskFilters, SearchBar
│       ├── context/
│       │   ├── AuthContext.js
│       │   ├── ThemeContext.js
│       │   └── TaskContext.js
│       ├── hooks/
│       │   ├── useForm.js
│       │   ├── useDebounce.js
│       │   └── useLocalStorage.js
│       ├── pages/
│       │   ├── LoginPage.jsx
│       │   ├── RegisterPage.jsx
│       │   ├── DashboardPage.jsx
│       │   ├── TasksPage.jsx
│       │   ├── ProfilePage.jsx
│       │   └── NotFoundPage.jsx
│       ├── services/
│       │   ├── api.js          # Axios instance + interceptors
│       │   ├── auth.service.js
│       │   └── task.service.js
│       ├── styles/
│       │   ├── global.css
│       │   └── variables.css   # CSS custom properties + dark mode
│       ├── utils/
│       │   ├── helpers.js
│       │   └── validators.js
│       ├── App.js
│       └── index.js
│
├── docker-compose.yml
├── Dockerfile.backend
├── Dockerfile.frontend
├── nginx.conf
└── package.json
```

---

## API Reference

### Auth Endpoints

| Method | Endpoint                    | Auth | Description         |
|--------|-----------------------------|------|---------------------|
| POST   | /api/auth/register          | No   | Register user       |
| POST   | /api/auth/login             | No   | Login user          |
| GET    | /api/auth/me                | Yes  | Get current user    |
| PUT    | /api/auth/profile           | Yes  | Update profile      |
| PUT    | /api/auth/change-password   | Yes  | Change password     |

### Task Endpoints

| Method | Endpoint               | Auth | Description              |
|--------|------------------------|------|--------------------------|
| GET    | /api/tasks             | Yes  | Get all tasks (filtered) |
| GET    | /api/tasks/:id         | Yes  | Get single task          |
| POST   | /api/tasks             | Yes  | Create task              |
| PUT    | /api/tasks/:id         | Yes  | Update task              |
| DELETE | /api/tasks/:id         | Yes  | Delete task              |
| PATCH  | /api/tasks/:id/toggle  | Yes  | Toggle status            |
| DELETE | /api/tasks/bulk        | Yes  | Bulk delete              |

### Query Parameters for GET /api/tasks

| Param   | Type   | Example             |
|---------|--------|---------------------|
| status  | string | pending/completed/all |
| priority| string | high/medium/low     |
| search  | string | "fix bug"           |
| page    | number | 1                   |
| limit   | number | 10                  |
| sortBy  | string | createdAt/dueDate   |
| order   | string | asc/desc            |

### User Endpoints (Admin)

| Method | Endpoint         | Auth  | Description      |
|--------|------------------|-------|------------------|
| GET    | /api/users       | Admin | List all users   |
| GET    | /api/users/stats | Yes   | User task stats  |

Full interactive docs: `http://localhost:5000/api/docs`

---

## Setup Instructions

### Prerequisites

- Node.js 18+
- MongoDB (local or Atlas)
- Docker + Docker Compose (optional)

### 1. Clone the repo

```bash
git clone https://github.com/yourusername/taskflow.git
cd taskflow
```

### 2. Backend Setup

```bash
cd backend
cp .env.example .env
# Edit .env — set MONGODB_URI and JWT_SECRET
npm install
npm run dev          # Runs on http://localhost:5000
```

### 3. Frontend Setup

```bash
cd frontend
cp .env.example .env
npm install
npm start            # Runs on http://localhost:3000
```

### 4. Run Both (from root)

```bash
npm install          # installs concurrently
npm run install:all  # installs all sub-packages
npm run dev          # starts both servers
```

### 5. Docker (Recommended for production)

```bash
cp backend/.env.example backend/.env
# Set JWT_SECRET in .env
docker-compose up --build
# App: http://localhost
# API: http://localhost:5000
# Docs: http://localhost:5000/api/docs
```

### 6. Run Tests

```bash
cd backend
npm test
```

---

## Environment Variables

### Backend (`backend/.env`)

| Variable      | Description                  | Default                          |
|---------------|------------------------------|----------------------------------|
| PORT          | Server port                  | 5000                             |
| MONGODB_URI   | MongoDB connection string     | mongodb://localhost:27017/taskflow |
| JWT_SECRET    | JWT signing secret           | (required)                       |
| JWT_EXPIRE    | Token expiry                 | 7d                               |
| NODE_ENV      | Environment                  | development                      |
| FRONTEND_URL  | Allowed CORS origin          | http://localhost:3000            |

### Frontend (`frontend/.env`)

| Variable            | Description     | Default                    |
|---------------------|-----------------|----------------------------|
| REACT_APP_API_URL   | Backend API URL | /api (proxied via CRA)     |

---

## Assumptions

- MongoDB is used as the primary database (Mongoose ODM)
- JWT stored in localStorage (suitable for assignment scope; use HttpOnly cookies in production)
- Each user only sees their own tasks (task isolation by user ID)
- Tags stored as string arrays on the task document
- Role-based access: `user` (default) and `admin` (manual DB update)
- Dark mode preference persisted in localStorage
- No email verification flow (can be added with nodemailer)

---

## Bonus Features Implemented

- [x] Docker setup (docker-compose + Nginx)
- [x] Role-based access (user / admin)
- [x] Pagination & search
- [x] Unit testing (Jest + Supertest)
- [x] Dark mode UI
- [x] Swagger API documentation

---

## License

MIT
