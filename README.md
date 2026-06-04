
# Travel Management System (Corporate Travel Hub)

A full-stack corporate travel management application that streamlines the entire travel lifecycle—from request submission to reimbursement. Built with **Spring Boot (Java 17)** on the backend and **React 19 (Vite)** on the frontend.

---

## Tech Stack

### Backend
| Technology   | Version |
|-------------|---------|
| Spring Boot | 4.0.6   |
| Java        | 17      |
| Database    | MySQL (via JPA/Hibernate) |
| Security    | Spring Security + JWT (jjwt 0.11.5) |
| Build Tool  | Maven   |
| ORM         | Spring Data JPA / Hibernate |

### Frontend
| Technology      | Version |
|----------------|---------|
| React          | 19      |
| Vite           | 8       |
| State Mgmt     | Redux Toolkit 2.12 |
| Routing        | React Router DOM 7.15 |
| CSS Framework  | Tailwind CSS 4.3 |
| Charts         | Recharts 3.8 |
| HTTP Client    | Axios 1.16 |
| Notifications  | React Hot Toast 2.6 |

---

## Features

- **JWT-based Authentication** with role-based authorization
- **Multi-role Dashboards** — Admin, Employee, Manager, Finance (each with distinct views & metrics)
- **Travel Request Management** — Create, edit, delete, submit requests with source/destination, dates, budget, transport mode, accommodation
- **Automatic Policy Validation** — Checks budget against active travel policies on submission
- **Two-Tier Approval Workflow** — Manager approves first, then Finance reviews & approves
- **Itinerary Management** — Day-by-day planning with locations, activities, hotels
- **Expense Management** — Submit expenses with receipt file upload; Finance approves/reimburses
- **Reimbursement Tracking** — End-to-end tracking from submission to payout
- **Travel Policy CRUD** — Admin creates/edits/deletes/toggles policies (max budget, trip days, per-diem, hotel limits)
- **User Management** — Admin creates/edits/deletes/toggles users with role assignment
- **Audit Logging** — All major actions logged with timestamp, performer, status
- **Reporting & Analytics** — Admin, Finance (category-wise, department-wise, monthly), and Manager reports
- **File Uploads** — Receipt uploads stored on disk (max 10MB)


## User Roles

| Role      | Responsibilities |
|-----------|----------------|
| **Admin**    | Manage users, travel policies; view all requests, audit logs, reports; override request status |
| **Employee** | Create & submit travel requests, manage itineraries, submit expenses, track reimbursements |
| **Manager**  | Review & approve/reject team travel requests, view team activity & reports |
| **Finance**  | Approve/reject travel requests (post-manager), approve/reimburse expenses, view financial reports |


## Travel Request Lifecycle

DRAFT → SUBMITTED → POLICY_VALIDATION → MANAGER_REVIEW →
MANAGER_APPROVED → FINANCE_REVIEW → FINANCE_APPROVED →
ITINERARY_CREATED → TRAVEL_IN_PROGRESS → EXPENSE_SUBMITTED →
EXPENSE_REVIEW → REIMBURSED → COMPLETED

Requests can be **REJECTED**, **CANCELLED**, or **DISMISSED** at any review stage.


## Project Structure

## Project Structure

### Backend

| Directory | Description |
|-----------|-------------|
| `src/main/java/com/travel/config/` | Security, file upload, data initializer configuration |
| `src/main/java/com/travel/controller/` | REST controllers (Auth, Admin, Employee, Manager, Finance, Itinerary, Policy, AuditLog) |
| `src/main/java/com/travel/dto/` | Request/Response DTOs |
| `src/main/java/com/travel/entity/` | JPA entities (User, TravelRequest, Expense, Itinerary, TravelPolicy, AuditLog) |
| `src/main/java/com/travel/enums/` | Enums (Role, RequestStatus, ExpenseStatus, ExpenseCategory) |
| `src/main/java/com/travel/repository/` | JPA repositories |
| `src/main/java/com/travel/service/` | Business logic services |
| `src/main/java/com/travel/util/` | JWT utility & filter |
| `src/main/resources/` | `application.properties` |
| `pom.xml` | Maven project configuration |

### Frontend

| Directory | Description |
|-----------|-------------|
| `src/app/` | Redux store configuration |
| `src/components/` | Layout components (Navbar, Sidebars) |
| `src/features/` | Redux slices (auth) |
| `src/layouts/` | Dashboard layout |
| `src/pages/admin/` | Admin pages (Dashboard, User Mgmt, Policy Mgmt, Travel Requests, Reports, Audit Logs, Profile) |
| `src/pages/employee/` | Employee pages (Dashboard, Travel Requests, Itineraries, Expenses, Reimbursements, History, Profile) |
| `src/pages/manager/` | Manager pages (Dashboard, Pending Approvals, Team Requests, Team History, Reports, Profile) |
| `src/pages/finance/` | Finance pages (Dashboard, Travel Requests, Pending Approvals, Expenses, Reimbursements, Reports, Profile) |
| `src/routes/` | ProtectedRoute guard |
| `src/services/` | Axios API client |
| `package.json` | Dependencies & scripts |
| `vite.config.js` | Vite configuration |

## Getting Started

### Prerequisites

- **Java 17+**
- **Node.js 18+**
- **MySQL** (running on localhost:3306)
- **Maven** (or use the bundled `mvnw`)

### Backend Setup

1. Clone the repository.
2. Ensure MySQL is running and create a database:
  
   CREATE DATABASE travel_management_system;
  
3. Navigate to `Backend/` and build:
   ./mvnw clean install
 
4. Run the application:
   ./mvnw spring-boot:run
   
The backend starts at `http://localhost:8080`.

### Frontend Setup

1. Navigate to `Frontend/`:
   cd Frontend
   npm install
 
2. Start the development server:
   npm run dev
  

The frontend starts at `http://localhost:5173`.



## API Endpoints Overview

| Method | Endpoint                  | Role     | Description |
|--------|---------------------------|----------|-------------|
| POST   | `/api/auth/login`         | Public   | Login & get JWT |
| GET    | `/api/admin/**`           | Admin    | User, policy, report management |
| GET    | `/api/admin/requests`     | Admin    | View all travel requests |
| PUT    | `/api/admin/requests/override/{id}` | Admin | Override request status |
| GET    | `/api/admin/audit`        | Admin    | View audit logs |
| GET/POST/PUT | `/api/employee/**`  | Employee | Travel requests, expenses, profile, dashboard |
| GET/PUT | `/api/manager/**`        | Manager  | Approvals, team requests, reports, dashboard |
| GET/PUT | `/api/finance/**`        | Finance  | Request/expense approval, reports, dashboard |
| POST/GET/PUT | `/api/itinerary/**` | Employee | Itinerary management |
| GET    | `/api/policies`           | All      | List active policies |


## Default Credentials

| Role  | Email             | Password |
|-------|-------------------|----------|
| Admin | admin@test.com    | admin123 |


## Configuration Notes

- **CORS**: Only `http://localhost:5173` is allowed (Vite dev server)
- **JWT Secret**: `mySuperSecretKeyForTravelManagementSystem1234567890` (HMAC-SHA256, 10h expiry)
- **Database**: MySQL at `localhost:3306`, database `travel_management_system`, user `root`, no password
- **File Uploads**: Max 10MB, stored in `Backend/file_uploads/`
- **JPA**: `ddl-auto=update` — tables are auto-created on startup
