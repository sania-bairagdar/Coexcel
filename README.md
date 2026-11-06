CoExcel — Equipment Management System
A web-based equipment and maintenance management application for tracking equipment inventory, machines, customers, repair orders, employee accounts, and operational reports.

The application uses a Node.js/Express backend, Supabase for data storage, and a set of browser-based HTML pages for its user interface.

Features
Equipment management — create, view, update, and delete equipment records, including quantities, categories, models, and serial numbers.
Machine management — maintain machine records, locations, models, and operational status.
Repair tracking — create repair orders, associate equipment requirements, and track repair progress.
Customer management — maintain customer details and view machines associated with a customer.
Employee and access management — authenticate users and apply role- and account-status-based access checks.
Reports and analytics — retrieve equipment stock levels, repair status summaries, repair trends over time, and repairs by customer.
Input validation — validate selected request fields on the server before processing them.
Technology Stack
Layer	Technologies
Backend	Node.js, Express 5
Database / data platform	Supabase
Authentication	JSON Web Tokens (JWT), bcrypt
Request validation	express-validator
Additional database dependency	mysql2
Frontend	HTML, CSS, JavaScript
Development	Nodemon, npm
CI	GitHub Actions
Application Structure
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
Prerequisites
Node.js and npm
A Supabase project
Supabase tables and relationships matching the queries used by the backend
The GitHub Actions workflow currently configures Node.js 24 for backend CI.

Getting Started
1. Clone the repository
git clone <repository-url>
cd Coexcel-main
2. Install backend dependencies
cd backend
npm ci
3. Configure environment variables
Create a .env file inside the backend/ directory:

PORT=5000
SUPABASE_URL=your_supabase_project_url
SUPABASE_KEY=your_supabase_key
JWT_SECRET=replace_with_a_long_random_secret
Use the appropriate Supabase key for your deployment. Keep secrets out of source control and never expose a privileged service-role key in browser-side code.

4. Configure the database
Create and configure the Supabase tables referenced by the application:

equipment
machines
repair_orders
equipment_orders
customers
The user/account table used by the authentication and employee-management routes
Set up the required foreign-key relationships. In particular, repair orders reference machines, equipment orders reference repair orders and equipment, and machines can be associated with customers.

The exact schema, column types, constraints, and policies should be aligned with the queries and fields in backend/server.js. This repository does not currently include a database migration or schema file.

5. Start the application
From the backend/ directory:

npm start
For development with automatic server restarts:

npm run dev
The backend listens on:

http://localhost:5000
The Express server serves the static frontend from the public/ directory. Open the application at http://localhost:5000.

API Overview
All application API routes are prefixed with /api. Protected endpoints expect a JWT in the request header:

Authorization: Bearer <token>
Resource	Endpoint	Supported operations
Authentication	/api/auth/login	POST
Authentication	/api/auth/signup	POST
Equipment	/api/equipment	GET, POST
Equipment item	/api/equipment/:id	GET, PUT, DELETE
Machines	/api/machines	GET, POST
Machine	/api/machines/:id	PUT, DELETE
Repair orders	/api/repairs	GET, POST
Repair order	/api/repairs/:id	GET, PUT
Dashboard statistics	/api/stats	GET
Employees	/api/users	GET
Employee	/api/users/:id	PUT, DELETE
Employee status	/api/users/:id/status	PUT
Customers	/api/customers	GET, POST
Customer	/api/customers/:id	GET, PUT, DELETE
Customer machines	/api/customers/:id/machines	GET
Equipment stock report	/api/reports/equipment-stock	GET
Repair status report	/api/reports/repair-status	GET
Repair trend report	/api/reports/repairs-over-time	GET
Repairs by customer report	/api/reports/repairs-per-customer	GET
Access control
The backend includes JWT authentication middleware, an admin-role check, and an active-account check. Access requirements vary by route. Review the middleware applied to each endpoint in backend/server.js before changing permissions or exposing the API.

Continuous Integration
The GitHub Actions workflow in .github/workflows/ci-cd.yml runs on pushes and pull requests targeting main. It:

Checks out the repository.
Sets up Node.js 24.
Installs dependencies using npm ci.
Runs node --check server.js to verify backend JavaScript syntax.
This workflow is a CI syntax check; it does not currently run an automated test suite or deploy the application.

Security and Deployment Notes
Before using this application in production:

Rotate any credentials that may have been included in a shared archive or exposed in version control.
Keep .env files and all secrets out of Git.
Use a strong, unique JWT_SECRET and manage it through the deployment platform's secret manager.
Review Supabase Row Level Security (RLS), database permissions, and the privileges associated with the configured Supabase key.
Restrict CORS to trusted origins rather than allowing every origin.
Add rate limiting and appropriate authentication protections to login and signup routes.
Validate and authorize every state-changing operation, including account-status changes and deletions.
Add automated tests for authentication, authorization, validation, and core workflows.
Use HTTPS and configure production logging and error handling.
Current Project Notes
The backend code is contained in backend/server.js.
The frontend is served as static files from public/.
Database schema/migrations are not included in the supplied repository archive.
The CI workflow checks syntax and dependency installation; it is not a full test or deployment pipeline.
Contributing
Fork the repository.

Create a feature branch.

Make focused changes and verify the backend starts successfully.

Run the available CI checks locally:

cd backend
npm ci
node --check server.js
Open a pull request describing the change and any database or environment updates required.
