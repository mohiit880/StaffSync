# StaffSync — Workforce Management Platform

> **Project documentation prepared by Mohammad Farhan Qureshi**  
> StaffSync is a full-stack Human Resource Management System designed to bring employee records, attendance, leave requests, learning activities, announcements, and workforce reporting into one web application.

## Overview

StaffSync provides separate workspaces for HR administrators, department managers, and employees. The application uses the MERN stack, with React and Vite powering the client and Node.js/Express providing the API. MongoDB stores application data, while Redux Toolkit, Tailwind CSS, and Recharts support the user interface and reporting experience.

The system includes role-aware access, attendance validation, leave approval workflows, employee training tools, and operational dashboards.

## Demo Login Details

The login page provides quick-login options for reviewing the main user roles.

| Account type | Email | Password | Typical tasks |
|---|---|---|---|
| HR / Administrator | `hr@staffsync.demo` | `Demo@12345` | Manage employees and departments, publish notices, review organization metrics |
| Department Manager | `manager@staffsync.demo` | `Demo@12345` | Monitor team attendance, handle leave requests, assign training |
| Employee | `employee@staffsync.demo` | `Demo@12345` | Record attendance, apply for leave, view notices, update training progress |

> These are demonstration credentials. Replace demo credentials and secrets before using the application in a real environment.

## Technology Used

### Client application
- **React 18 + Vite** — component-based frontend and development/build tooling
- **React Router DOM v6** — page navigation
- **Redux Toolkit / React-Redux** — shared application state
- **Tailwind CSS, PostCSS, Autoprefixer** — styling
- **Lucide React** — interface icons
- **Recharts** — charts for workforce statistics
- **Axios** — HTTP requests with JWT interceptors

### Server application
- **Node.js** — JavaScript runtime
- **Express.js 4** — REST API framework
- **MongoDB with Mongoose 8** — persistent data storage and object modelling
- **mongodb-memory-server** — optional in-memory database for local demonstration
- **JWT** — session authentication
- **bcryptjs** — password hashing
- **CORS and express-validator** — request access configuration and input validation
- **Morgan** — HTTP request logging

### Quality and deployment tools
- **Jest and Supertest** — API integration testing
- **Vite/Rollup** — optimized frontend build
- **Vercel** — frontend hosting
- **Render** — backend hosting
- **MongoDB Atlas** — managed database hosting

## Main Features

### 1. Role-specific dashboards
- **HR dashboard (`/dashboard/hr`)** displays workforce totals, attendance summaries, pending leave requests, recent attendance trends, department headcounts, and shortcuts to common tasks.
- **Manager dashboard (`/dashboard/manager`)** provides team information, daily attendance status, leave reviews, and training completion details.
- **Employee dashboard (`/dashboard/employee`)** gives employees access to their attendance actions, available leave balances, company notices, and assigned courses.

### 2. Attendance tracking (`/attendance`)
- Employees can check in and check out for the workday.
- The interface displays the current shift state.
- Server-side checks and a compound unique index on employee and date help prevent duplicate daily check-ins.
- Checkout calculates the recorded working duration; shifts shorter than four hours are marked as half-days.
- Attendance records can be viewed at personal, team, and organization levels according to access permissions.

### 3. Leave requests (`/leaves`)
- Employees can request Annual, Sick, Casual, Maternity/Paternity, or Unpaid leave.
- Each request includes a date range and a reason.
- The server checks entitlement and prevents overlapping requests for the same employee.
- Approved paid leave is deducted from the employee's available balance.
- Managers and HR can approve or reject requests; a rejection requires a reason and can trigger an in-app notification.

### 4. Employee directory and profiles (`/employees`, `/profile`)
- Search and filter employee records by name, employee ID, role/designation, department, and status.
- HR can register staff and assign relevant organizational details.
- HR can activate or deactivate employee accounts.
- Employees can edit supported profile information, including phone number, avatar URL, and home address.

### 5. Training and development (`/training`)
- Courses are grouped into Technical, Compliance, Leadership, and Soft Skills categories.
- Course entries can include links to learning materials, videos, or external resources.
- HR and managers can assign courses to multiple employees.
- Employees can update their completion percentage, mark courses complete, and submit feedback.

### 6. Announcements (`/announcements`)
- Notices can cover company news, holidays, policy changes, events, or urgent updates.
- Important announcements can be pinned and labelled by priority.
- Publishing a notice can send in-app notifications to active team members.

### 7. Reports and analytics (`/reports`)
- Charts summarize attendance patterns, department distribution, and leave information.
- Management can export a CSV report for review and record-keeping.
- The generated report filename is `staffsync-hrms-report.csv`.

## Repository Layout

```text
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

## Run the Project on Your Computer

### Requirements
- Node.js v18 or newer (Node.js v20+ LTS is recommended)
- npm v9 or newer

### Step 1 — Get the source code

```bash
git clone https://github.com/en24cs3010063-blip/StaffSync.git
cd StaffSync
```

### Step 2 — Install and start the API

```bash
cd server
npm install
cp .env.example .env
```

Configure the environment variables in `.env` if needed. If `MONGO_URI` is left empty, the project is configured to use an in-memory MongoDB instance and seed demonstration data, so a separate local database may not be required for a quick demo.

Run the backend tests:

```bash
npm test
```

Launch the server in development mode:

```bash
npm run dev
```

The API is expected to run at `http://localhost:5000`.

### Step 3 — Install and launch the frontend

Open a second terminal from the project directory:

```bash
cd client
npm install
npm run dev
```

Vite normally serves the frontend at `http://localhost:5173`. Open that address in a browser and select one of the demo login options.

## API Quick Reference

| Method | Route | Access | Purpose |
|---|---|---|---|
| `GET` | `/api/health` | Public | Check server health and uptime |
| `POST` | `/api/auth/login` | Public | Sign in and receive a JWT |
| `GET` | `/api/auth/me` | Signed in | Get the current account profile |
| `PUT` | `/api/auth/profile` | Signed in | Change profile details |
| `PUT` | `/api/auth/change-password` | Signed in | Change the account password |
| `GET` | `/api/employees` | Signed in | Search the employee directory |
| `POST` | `/api/employees` | HR only | Add an employee |
| `PATCH` | `/api/employees/:id/status` | HR only | Change employee account status |
| `POST` | `/api/attendance/check-in` | Signed in | Record a check-in |
| `POST` | `/api/attendance/check-out` | Signed in | Record a check-out |
| `GET` | `/api/attendance/today` | Signed in | View today's shift state |
| `GET` | `/api/attendance/my-history` | Signed in | View personal attendance history |
| `GET` | `/api/attendance/team` | Manager / HR | Review team attendance |
| `GET` | `/api/attendance/organization` | HR only | View organization-wide attendance |
| `POST` | `/api/leaves` | Signed in | Submit a leave request |
| `GET` | `/api/leaves/my-leaves` | Signed in | View personal leave records and balances |
| `GET` | `/api/leaves` | Manager / HR | Retrieve requests awaiting review |
| `PATCH` | `/api/leaves/:id/review` | Manager / HR | Approve or reject a request |
| `GET` | `/api/announcements` | Signed in | Read published announcements |
| `POST` | `/api/announcements` | HR only | Publish a company announcement |
| `GET` | `/api/trainings` | Signed in | Browse available training |
| `POST` | `/api/trainings` | Manager / HR | Create a training item |
| `POST` | `/api/trainings/:id/assign` | Manager / HR | Assign training to employees |
| `PATCH` | `/api/trainings/:id/progress` | Signed in | Update course progress |
| `GET` | `/api/analytics/admin` | HR only | Retrieve HR dashboard metrics |
| `GET` | `/api/analytics/manager` | Manager / HR | Retrieve team metrics |
| `GET` | `/api/analytics/employee` | Signed in | Retrieve personal statistics |
| `GET` | `/api/notifications` | Signed in | List in-app notifications |
| `PATCH` | `/api/notifications/mark-all-read` | Signed in | Mark notifications as read |

## Deployment Notes

### Database — MongoDB Atlas
1. Create a MongoDB Atlas cluster.
2. Add a database user with an appropriate password.
3. Configure network access for the deployment environment.
4. Copy the connection URI and set it as `MONGO_URI`.

Example URI format:

```text
mongodb+srv://<user>:<password>@cluster0.mongodb.net/staffsync?retryWrites=true&w=majority
```

For production, restrict network access where possible instead of allowing unrestricted access.

### Backend — Render
1. Create a Render Web Service connected to the repository.
2. Set the root directory to `server`.
3. Set the build command to `npm install`.
4. Set the start command to `npm start`.
5. Add the environment variables below, using your actual deployment values.

```text
NODE_ENV=production
MONGO_URI=<your MongoDB Atlas URI>
JWT_SECRET=<a strong random secret>
CLIENT_URL=<your deployed frontend URL>
```

After deployment, copy the public backend URL for the frontend configuration.

### Frontend — Vercel
1. Import the repository into Vercel.
2. Set the root directory to `client`.
3. Choose the Vite framework preset.
4. Set the following environment variable to your deployed API base URL:

```text
VITE_API_URL=<your deployed backend URL>
```

Deploy the frontend. The included `vercel.json` is intended to route client-side paths back through the single-page application.

## Verification

The supplied project README records the following results:

- **Backend integration tests:** 1 test suite passed; 13 tests passed.
- Coverage described includes health/public routes, authentication and role permissions, attendance duplicate prevention, leave validation/review, announcements, and analytics.
- **Frontend production build:** reported as successful, with Vite/Rollup producing optimized assets.

Run the tests and production build again in your own environment before relying on these results for a submission or deployment.

## Possible Future Improvements

- Connect biometric attendance devices through a real-time integration.
- Add payroll processing, tax calculations, and downloadable payslips.
- Send approval and announcement alerts through Slack or Microsoft Teams.
- Introduce structured performance reviews and 360-degree feedback.

## License

The source README identifies the project as being released under the MIT License. Check that the repository contains the corresponding license file before redistributing the project.

---

**Prepared by:** Mohammad Farhan Qureshi  
**Project:** StaffSync — Human Resource Management System
