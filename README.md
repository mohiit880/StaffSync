# StaffSync — Employee and Workforce Management System

> **Project documentation prepared by Mohit Dhakad**\
> StaffSync is a full-stack HR management application that centralizes employee information, attendance tracking, leave management, training, company announcements, and workforce analytics in a single platform.

## Project Overview

The platform offers dedicated dashboards for HR administrators, managers, and employees. It is built with the MERN ecosystem: React and Vite form the frontend, Node.js and Express handle backend requests, and MongoDB manages persistent data. Redux Toolkit, Tailwind CSS, and Recharts are used for state management, styling, and data visualization.

Core capabilities include role-based permissions, validated attendance records, leave review and approval, employee learning tools, and dashboards for workforce operations.

## Demo Account Credentials

Demo accounts are available to explore the application from each supported role.

| **Account type**   | **Email**                 | **Password** | **Typical tasks**                                                              |
| ------------------ | ------------------------- | ------------ | ------------------------------------------------------------------------------ |
| HR / Administrator | `hr@staffsync.demo`       | `Demo@12345` | Manage employees and departments, publish notices, review organization metrics |
| Department Manager | `manager@staffsync.demo`  | `Demo@12345` | Monitor team attendance, handle leave requests, assign training                |
| Employee           | `employee@staffsync.demo` | `Demo@12345` | Record attendance, apply for leave, view notices, update training progress     |

> The credentials above are intended for demonstration only. Configure secure credentials and secrets before deploying the system for real-world use.

## Technology Stack

### Frontend

- **React 18 + Vite** — component-based frontend and development/build tooling
- **React Router DOM v6** — page navigation
- **Redux Toolkit / React-Redux** — shared application state
- **Tailwind CSS, PostCSS, Autoprefixer** — styling
- **Lucide React** — interface icons
- **Recharts** — charts for workforce statistics
- **Axios** — HTTP requests with JWT interceptors

### Backend

- **Node.js** — JavaScript runtime
- **Express.js 4** — REST API framework
- **MongoDB with Mongoose 8** — persistent data storage and object modelling
- **mongodb-memory-server** — optional in-memory database for local demonstration
- **JWT** — session authentication
- **bcryptjs** — password hashing
- **CORS and express-validator** — request access configuration and input validation
- **Morgan** — HTTP request logging

### Testing and Deployment

- **Jest and Supertest** — API integration testing
- **Vite/Rollup** — optimized frontend build
- **Vercel** — frontend hosting
- **Render** — backend hosting
- **MongoDB Atlas** — managed database hosting

## Key Features

### 1. Role-Based Dashboards

- **HR dashboard (`/dashboard/hr`)** displays workforce totals, attendance summaries, pending leave requests, recent attendance trends, department headcounts, and shortcuts to common tasks.
- **Manager dashboard (`/dashboard/manager`)** provides team information, daily attendance status, leave reviews, and training completion details.
- **Employee dashboard (`/dashboard/employee`)** gives employees access to their attendance actions, available leave balances, company notices, and assigned courses.

### 2. Attendance Management (`/attendance`)

- Employees can register the start and end of their workday using the check-in and check-out actions.
- The attendance screen shows the employee's current shift status.
- Server-side checks and a compound unique index on employee and date help prevent duplicate daily check-ins.
- When an employee checks out, the system calculates the recorded work duration and classifies shifts under four hours as half-days.
- Attendance records can be viewed at personal, team, and organization levels according to access permissions.

### 3. Leave Management (`/leaves`)

- Available leave categories include Annual, Sick, Casual, Maternity/Paternity, and Unpaid leave.
- Every leave application records the requested dates and the reason for the absence.
- Backend validation checks leave entitlement and rejects overlapping requests submitted by the same employee.
- Once paid leave is approved, the corresponding amount is deducted from the employee's remaining leave balance.
- Managers and HR can approve or reject requests; a rejection requires a reason and can trigger an in-app notification.

### 4. Employee Directory and Profiles (`/employees`, `/profile`)

- Search and filter employee records by name, employee ID, role/designation, department, and status.
- HR can register staff and assign relevant organizational details.
- HR can activate or deactivate employee accounts.
- Employees can edit supported profile information, including phone number, avatar URL, and home address.

### 5. Training and Development (`/training`)

- Courses are grouped into Technical, Compliance, Leadership, and Soft Skills categories.
- Course entries can include links to learning materials, videos, or external resources.
- HR and managers can assign courses to multiple employees.
- Employees can update their completion percentage, mark courses complete, and submit feedback.

### 6. Company Announcements (`/announcements`)

- Announcements can be used to share company updates, holidays, policy revisions, events, and urgent information.
- Important announcements can be pinned and labelled by priority.
- Publishing a notice can send in-app notifications to active team members.

### 7. Reports and Analytics (`/reports`)

- Visual reports present attendance trends, department-wise employee distribution, and leave-related statistics.
- Managers can download workforce data as a CSV file for analysis and record keeping.
- The generated report filename is `staffsync-hrms-report.csv`.

## Project Directory Structure

```
staffsync/
├── client/                         # React/Vite frontend
│   ├── public/
│   ├── src/
│   │   ├── assets/                 # Static assets
│   │   ├── components/             # Shared interface components
│   │   ├── layouts/                # Dashboard shell and navigation
│   │   ├── pages/                  # Dashboard and feature pages
│   │   ├── routes/                 # Protected routes and role checks
│   │   ├── services/               # Axios API setup
│   │   ├── store/                  # Redux state slices
│   │   ├── App.jsx                 # Application routes
│   │   ├── index.css               # Global styles
│   │   └── main.jsx                # React entry point
│   ├── index.html
│   ├── package.json
│   ├── tailwind.config.js
│   ├── vercel.json                 # Vercel SPA fallback rules
│   └── vite.config.js
├── server/                         # Express API
│   ├── config/                     # Database setup
│   ├── controllers/                # Request handlers
│   ├── middleware/                 # Authentication and error handling
│   ├── models/                     # Mongoose schemas
│   ├── routes/                     # API route definitions
│   ├── scripts/                    # Database seed utilities
│   ├── services/                   # Notification helpers
│   ├── tests/                      # Jest/Supertest tests
│   ├── validators/                 # Request validation rules
│   ├── .env.example
│   ├── package.json
│   └── server.js                   # API entry point and health route
├── render.yaml                     # Render deployment configuration
├── .gitignore
├── .env.example
└── README.md

```

## Local Setup and Installation

### Prerequisites

- Node.js v18 or newer (Node.js v20+ LTS is recommended)
- npm v9 or newer

### Step 1: Clone the Repository

```
git clone https://github.com/en24cs3010063-blip/StaffSync.git
cd StaffSync
```

### Step 2: Configure and Run the Backend

```
cd server
npm install
cp .env.example .env
```

Configure the environment variables in `.env` if needed. If `MONGO_URI` is left empty, the project is configured to use an in-memory MongoDB instance and seed demonstration data, so a separate local database may not be required for a quick demo.

Run the backend tests:

```
npm test
```

Launch the server in development mode:

```
npm run dev
```

The API is expected to run at `http://localhost:5000`.

### Step 3: Configure and Run the Frontend

Open a second terminal from the project directory:

```
cd client
npm install
npm run dev
```

Vite normally serves the frontend at `http://localhost:5173`. Open that address in a browser and select one of the demo login options.

## API Endpoint Reference

| **Method** | **Route**                          | **Access**   | **Purpose**                              |
| ---------- | ---------------------------------- | ------------ | ---------------------------------------- |
| `GET`      | `/api/health`                      | Public       | Check server health and uptime           |
| `POST`     | `/api/auth/login`                  | Public       | Sign in and receive a JWT                |
| `GET`      | `/api/auth/me`                     | Signed in    | Get the current account profile          |
| `PUT`      | `/api/auth/profile`                | Signed in    | Change profile details                   |
| `PUT`      | `/api/auth/change-password`        | Signed in    | Change the account password              |
| `GET`      | `/api/employees`                   | Signed in    | Search the employee directory            |
| `POST`     | `/api/employees`                   | HR only      | Add an employee                          |
| `PATCH`    | `/api/employees/:id/status`        | HR only      | Change employee account status           |
| `POST`     | `/api/attendance/check-in`         | Signed in    | Record a check-in                        |
| `POST`     | `/api/attendance/check-out`        | Signed in    | Record a check-out                       |
| `GET`      | `/api/attendance/today`            | Signed in    | View today's shift state                 |
| `GET`      | `/api/attendance/my-history`       | Signed in    | View personal attendance history         |
| `GET`      | `/api/attendance/team`             | Manager / HR | Review team attendance                   |
| `GET`      | `/api/attendance/organization`     | HR only      | View organization-wide attendance        |
| `POST`     | `/api/leaves`                      | Signed in    | Submit a leave request                   |
| `GET`      | `/api/leaves/my-leaves`            | Signed in    | View personal leave records and balances |
| `GET`      | `/api/leaves`                      | Manager / HR | Retrieve requests awaiting review        |
| `PATCH`    | `/api/leaves/:id/review`           | Manager / HR | Approve or reject a request              |
| `GET`      | `/api/announcements`               | Signed in    | Read published announcements             |
| `POST`     | `/api/announcements`               | HR only      | Publish a company announcement           |
| `GET`      | `/api/trainings`                   | Signed in    | Browse available training                |
| `POST`     | `/api/trainings`                   | Manager / HR | Create a training item                   |
| `POST`     | `/api/trainings/:id/assign`        | Manager / HR | Assign training to employees             |
| `PATCH`    | `/api/trainings/:id/progress`      | Signed in    | Update course progress                   |
| `GET`      | `/api/analytics/admin`             | HR only      | Retrieve HR dashboard metrics            |
| `GET`      | `/api/analytics/manager`           | Manager / HR | Retrieve team metrics                    |
| `GET`      | `/api/analytics/employee`          | Signed in    | Retrieve personal statistics             |
| `GET`      | `/api/notifications`               | Signed in    | List in-app notifications                |
| `PATCH`    | `/api/notifications/mark-all-read` | Signed in    | Mark notifications as read               |

## Deployment Guide

### Database Setup: MongoDB Atlas

1. Create a MongoDB Atlas cluster.
2. Add a database user with an appropriate password.
3. Configure network access for the deployment environment.
4. Copy the connection URI and set it as `MONGO_URI`.

Example URI format:

```
mongodb+srv://<user>:<password>@cluster0.mongodb.net/staffsync?retryWrites=true&w=majority

```

For production, restrict network access where possible instead of allowing unrestricted access.

### Backend Deployment: Render

1. Create a Render Web Service connected to the repository.
2. Set the root directory to `server`.
3. Set the build command to `npm install`.
4. Set the start command to `npm start`.
5. Add the environment variables below, using your actual deployment values.

```
NODE_ENV=production
MONGO_URI=<your MongoDB Atlas URI>
JWT_SECRET=<a strong random secret>
CLIENT_URL=<your deployed frontend URL>

```

After deployment, copy the public backend URL for the frontend configuration.

### Frontend Deployment: Vercel

1. Import the repository into Vercel.
2. Set the root directory to `client`.
3. Choose the Vite framework preset.
4. Set the following environment variable to your deployed API base URL:

```
VITE_API_URL=<your deployed backend URL>

```

Deploy the frontend. The included `vercel.json` is intended to route client-side paths back through the single-page application.

## Testing and Build Verification

The original project documentation reports the following checks:

- **Backend integration tests:** 1 test suite passed; 13 tests passed.
- Coverage described includes health/public routes, authentication and role permissions, attendance duplicate prevention, leave validation/review, announcements, and analytics.
- **Frontend production build:** reported as successful, with Vite/Rollup producing optimized assets.

Re-run the test suite and production build in your own environment to confirm these results before submission or deployment.

## Future Enhancements

- Connect biometric attendance devices through a real-time integration.
- Add payroll processing, tax calculations, and downloadable payslips.
- Send approval and announcement alerts through Slack or Microsoft Teams.
- Introduce structured performance reviews and 360-degree feedback.

## License Information

The source README identifies the project as being released under the MIT License. Check that the repository contains the corresponding license file before redistributing the project.

---

**Prepared by:** Mohit Dhakad\
**Project:** StaffSync — Human Resource Management System