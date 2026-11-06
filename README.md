# CoExcel — Equipment Management System

CoExcel is a web-based Equipment and Maintenance Management System designed to manage equipment inventory, machines, customers, repair orders, employee accounts, and operational reports through a centralized application.

The application uses a Node.js and Express backend, Supabase for database management, and browser-based HTML, CSS, and JavaScript for its frontend.

## Features

1. Equipment Management: Create, view, update, and delete equipment records, including quantities, categories, models, and serial numbers.
2. Machine Management: Maintain machine records, locations, models, and operational status.
3. Repair Tracking: Create repair orders, associate equipment requirements, and monitor repair progress.
4. Customer Management: Maintain customer information and view machines associated with each customer.
5. Employee and Access Management: Authenticate users and implement role-based and account-status-based access control.
6. Reports and Analytics: Generate equipment stock reports, repair status summaries, repair trends over time, and customer-wise repair reports.
7. Input Validation: Validate selected API request fields on the server before processing them.

## Technology Stack

| Layer                          | Technologies                  |
| ------------------------------ | ----------------------------- |
| Backend                        | Node.js, Express 5            |
| Database / Data Platform       | Supabase                      |
| Authentication                 | JSON Web Tokens (JWT), bcrypt |
| Request Validation             | express-validator             |
| Additional Database Dependency | mysql2                        |
| Frontend                       | HTML, CSS, JavaScript         |
| Development Tools              | Nodemon, npm                  |
| CI                             | GitHub Actions                |

## Application Structure

```
Coexcel-main/
├── .github/
│   └── workflows/
│       └── ci-cd.yml
├── backend/
│   ├── .gitignore
│   ├── package.json
│   ├── package-lock.json
│   └── server.js
└── public/
    ├── index.html
    ├── login.html
    ├── signup.html
    ├── dashboard.html
    ├── customers.html
    ├── employees.html
    └── reports.html
```

## Prerequisites

- Node.js and npm
- A Supabase project
- Supabase tables and relationships matching the backend queries

The GitHub Actions workflow currently configures Node.js 24 for backend CI.

## Getting Started

1\. Clone the repository

```
git clone <repository-url>
cd Coexcel-main
```

2\. Install backend dependencies

```
cd backend
npm ci
```

3\. Configure environment variables

Create a `.env` file inside the `backend/` directory:

```
PORT=5000
SUPABASE_URL=your_supabase_project_url
SUPABASE_KEY=your_supabase_key
JWT_SECRET=replace_with_a_long_random_secret
```

Keep credentials and secrets out of source control. Never expose a privileged service-role key in browser-side code.

4\. Configure the database

Create and configure the following Supabase tables:

- equipment
- machines
- repair_orders
- equipment_orders
- customers
- User/account table for authentication and employee management

Set up the required foreign-key relationships between repair orders, machines, equipment orders, equipment, and customers.

The exact schema, column types, constraints, and policies should match the queries and fields used in `backend/server.js`. Database migrations and schema files are not currently included in the repository.

5\. Start the application

For production-style execution:

```
npm start
```

For development with automatic server restarts:

```
npm run dev
```

The backend runs at:

`http://localhost:5000`

The Express server serves the frontend from the `public/` directory. Open the application in your browser at the above address.

## API Overview

All application API routes are prefixed with `/api`.

Protected endpoints require a JWT in the request header:

```
Authorization: Bearer <token>
```

| Resource               | Endpoint                            | Supported Operations |
| ---------------------- | ----------------------------------- | -------------------- |
| Authentication         | `/api/auth/login`                   | POST                 |
| Authentication         | `/api/auth/signup`                  | POST                 |
| Equipment              | `/api/equipment`                    | GET, POST            |
| Equipment Item         | `/api/equipment/:id`                | GET, PUT, DELETE     |
| Machines               | `/api/machines`                     | GET, POST            |
| Machine                | `/api/machines/:id`                 | PUT, DELETE          |
| Repair Orders          | `/api/repairs`                      | GET, POST            |
| Repair Order           | `/api/repairs/:id`                  | GET, PUT             |
| Dashboard Statistics   | `/api/stats`                        | GET                  |
| Employees              | `/api/users`                        | GET                  |
| Employee               | `/api/users/:id`                    | PUT, DELETE          |
| Employee Status        | `/api/users/:id/status`             | PUT                  |
| Customers              | `/api/customers`                    | GET, POST            |
| Customer               | `/api/customers/:id`                | GET, PUT, DELETE     |
| Customer Machines      | `/api/customers/:id/machines`       | GET                  |
| Equipment Stock Report | `/api/reports/equipment-stock`      | GET                  |
| Repair Status Report   | `/api/reports/repair-status`        | GET                  |
| Repair Trend Report    | `/api/reports/repairs-over-time`    | GET                  |
| Repairs by Customer    | `/api/reports/repairs-per-customer` | GET                  |

## Access Control

The backend implements:

- JWT authentication middleware.
- Admin-role authorization.
- Active-account verification.

Access requirements vary depending on the endpoint. Each route's middleware in `backend/server.js` determines its access permissions.

## Continuous Integration (CI)

The GitHub Actions workflow is configured in:

`.github/workflows/ci-cd.yml`

It runs automatically on pushes and pull requests targeting the `main` branch.

The workflow performs the following steps:

1. Checks out the repository.
2. Sets up Node.js 24.
3. Installs dependencies using `npm ci`.
4. Runs `node --check server.js` to verify backend JavaScript syntax.

Current limitation: The workflow performs dependency installation and syntax validation. It does not currently include an automated test suite or deployment pipeline.

## Security and Deployment Considerations

Before deploying the application in production:

- Rotate credentials that may have been exposed in version control or shared archives.
- Keep `.env` files and sensitive information out of Git.
- Use a strong, unique JWT secret.
- Store secrets securely using the deployment platform's secret manager.
- Review Supabase Row Level Security (RLS) policies and database permissions.
- Restrict CORS to trusted origins.
- Implement rate limiting and authentication protections for login and signup routes.
- Validate and authorize all state-changing operations.
- Add automated tests for authentication, authorization, validation, and core workflows.
- Use HTTPS and configure production logging and error handling.

## Current Project Notes

- Backend implementation is contained in `backend/server.js`.
- Frontend pages are served as static files from `public/`.
- Database schema and migration files are not included in the repository.
- GitHub Actions currently performs CI checks but does not handle automated deployment.

## Contributing

1. Fork the repository.
2. Create a feature branch.
3. Make focused changes and verify that the backend starts successfully.
4. Run the available CI checks locally:

```
cd backend
npm ci
node --check server.js
```

5\. Open a pull request describing the changes and any required database or environment updates.

cd backend
npm ci
node --check server.js
Open a pull request describing the change and any database or environment updates required.
