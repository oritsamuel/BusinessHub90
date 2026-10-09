# BUSINESSHUB — PROJECT CONTRACT

**Project:** Multi-Business Management Platform
**Project duration:** 90 days
**Project type:** Full-stack development and practical learning immersion
**Developer:** Project Owner
**Technology stack:** Next.js, TypeScript, PostgreSQL, Prisma, Zod, Tailwind CSS
**Contract period:** Day 1–Day 90

---

## 1. Project Purpose

BusinessHub is a web-based platform that enables multiple businesses to manage customers, staff, transactions, loyalty rewards, and business reports from one application.

This project will serve as my primary practical learning environment for developing backend and full-stack engineering skills.

The objective is to progress from a frontend-focused developer with limited backend knowledge to a developer who can independently design, implement, test, and deploy a functional full-stack application.

The application must solve a realistic business problem and demonstrate the engineering practices expected of a professional developer.

## 2. Project Objectives

By the end of the project, I must be able to:

1. Design relational database schemas and write SQL queries.
2. Build REST APIs using Next.js Route Handlers.
3. Implement CRUD operations, validation, and business logic.
4. Implement authentication, authorization, and role-based access control.
5. Build secure, multi-tenant applications with business data isolation.
6. Handle database transactions, failures, and duplicate requests.
7. Integrate backend APIs with a functional frontend.
8. Write and execute automated tests.
9. Deploy and maintain a database-backed application.
10. Explain my architecture, code, and technical decisions without relying on AI to explain the entire application for me.

## 3. Project Scope

### 3.1 Core features

**Module 1: Authentication and user management**

* User registration and login.
* Secure password handling and session management.
* Protected pages and API endpoints.
* Owner and employee roles.
* Business membership and permission checks.

**Module 2: Business management**

* Create and manage business profiles.
* Associate users with businesses.
* Manage staff memberships.
* Enforce business-level data isolation.

**Module 3: Customer management**

* Create, view, update, search, filter, and archive customers.
* Maintain customer records within the correct business.
* Support pagination and customer details.

**Module 4: Transactions and service visits**

* Record customer purchases or service visits.
* Store amounts, dates, services, and responsible staff.
* Calculate loyalty points using server-side business rules.
* Maintain transaction history.
* Prevent invalid or inconsistent records.

**Module 5: Loyalty and rewards**

* Define available rewards.
* Track customer points and eligibility.
* Redeem eligible rewards.
* Maintain redemption history.
* Prevent duplicate redemptions and incorrect balance updates.

**Module 6: Reporting and analytics**

* Display transaction totals and customer activity.
* Filter reports by date range.
* Show reward redemptions and relevant business metrics.
* Restrict reports to the authenticated user's authorized business.

### 3.2 Supporting features

* Input validation and consistent error responses.
* Search, filtering, sorting, and pagination.
* Automated tests.
* API documentation.
* Database migrations.
* Logging and environment configuration.
* Deployment and project documentation.

### 3.3 Out of scope for the initial 90 days

The following will not be required for project completion:

* Real payment gateway integration.
* Native mobile applications.
* Microservices architecture.
* Complex event-driven infrastructure.
* AI-powered analytics.
* Advanced subscription billing.
* Production-scale distributed systems.

These features may be considered after the core application is complete.

## 4. Technical Requirements

The application will use the following technologies:

| Area              | Technology                                    |
| ----------------- | --------------------------------------------- |
| Frontend          | Next.js, React, TypeScript                    |
| Styling           | Tailwind CSS                                  |
| Backend           | Next.js Route Handlers                        |
| Database          | PostgreSQL                                    |
| ORM               | Prisma                                        |
| Validation        | Zod                                           |
| API testing       | Postman                                       |
| Automated testing | Vitest and suitable integration-testing tools |
| Version control   | Git and GitHub                                |
| Deployment        | Vercel or an appropriate alternative          |
| Database hosting  | Managed PostgreSQL provider                   |

I must understand the purpose of each technology and the problems it solves.

I must learn SQL independently of the ORM and understand how requests, validation, business logic, database operations, and responses work together.

## 5. Development Milestones

### Milestone 1 — Backend Foundations

**Deadline:** End of Week 2

Deliverables:

* Project repository and initial application structure.
* Working API endpoints.
* PostgreSQL database connection.
* Initial relational schema.
* Persistent CRUD functionality.
* Basic API testing.

Acceptance criteria:

* Data persists after restarting the application.
* Database relationships and constraints work correctly.
* API requests return appropriate status codes.
* I can explain the request/response lifecycle and database schema.

### Milestone 2 — Core Backend and Authentication

**Deadline:** End of Week 4

Deliverables:

* Complete customer CRUD API.
* Server-side input validation.
* Error handling.
* Registration and login.
* Session management.
* Initial role-based access control.

Acceptance criteria:

* Invalid requests are rejected.
* Protected endpoints require authentication.
* Unauthorized actions are rejected by the server.
* Passwords are not stored in plaintext.
* Automated or repeatable tests cover the core endpoints.

### Milestone 3 — Business Workflows

**Deadline:** End of Week 6

Deliverables:

* Transaction and service-visit management.
* Server-side loyalty point calculations.
* Reward eligibility and redemption.
* Transaction history.

Acceptance criteria:

* Business rules are enforced on the server.
* Invalid transactions cannot modify customer balances.
* Failed multi-step operations do not leave inconsistent records.
* Duplicate or concurrent redemptions are handled safely.

### Milestone 4 — Multi-Tenant Full-Stack Application

**Deadline:** End of Week 8

Deliverables:

* Business and staff management.
* Tenant-level data isolation.
* Functional customer, transaction, and rewards interfaces.
* Search, pagination, and error states.

Acceptance criteria:

* Multiple businesses can use the same application.
* Users cannot access another business's records without authorization.
* Frontend actions are connected to persistent backend data.
* Refreshing the application does not erase saved data.

### Milestone 5 — Reporting and Quality Assurance

**Deadline:** End of Week 10

Deliverables:

* Reporting endpoints and dashboard.
* SQL aggregation and date filters.
* Unit and integration tests.
* Permission and failure-case testing.
* Bug fixes.

Acceptance criteria:

* Report totals match verified sample data.
* Date filters and tenant restrictions work correctly.
* Core business rules and authorization have automated tests.
* Test results are reproducible.

### Milestone 6 — Deployment and Final Delivery

**Deadline:** End of Week 12, with final submission by Day 90

Deliverables:

* Deployed application.
* Production database configuration and migrations.
* API documentation.
* README and setup instructions.
* Database relationship diagram.
* Architecture diagram.
* Portfolio case study and application demo.

Acceptance criteria:

* The application works in its deployed environment.
* Secrets are not committed to the repository or exposed to the client.
* A new developer can follow the setup instructions.
* Core tests pass.
* I can explain the application's architecture and demonstrate its main workflows.

## 6. Work Schedule

The project will follow a six-day workweek with one rest or optional review day.

**Daily commitment:** At least four focused hours where practical.

A typical study day will include:

* Concept study and documentation review.
* Hands-on feature implementation.
* Testing and debugging.
* Code review, notes, and Git commits.

The primary measure of progress will be completed and understood deliverables, not hours spent watching tutorials.

## 7. Learning and Engineering Rules

Throughout the project, I agree to the following rules:

1. **Understand before implementing.** I will learn the concepts needed for a feature before attempting to build it.
2. **Design before coding.** I will define requirements, database changes, API contracts, permissions, and edge cases.
3. **Use AI as a mentor.** I may use AI for explanations, code reviews, debugging, and test ideas, but I will not blindly copy code I cannot explain.
4. **Test every important feature.** I will verify normal behavior, invalid input, authorization, and failure cases.
5. **Commit work regularly.** Each meaningful milestone will be recorded in Git.
6. **Document decisions.** I will record important technical decisions and explain why I made them.
7. **Fix problems before expanding scope.** I will prioritize correctness and understanding over adding features.
8. **Avoid unnecessary complexity.** I will not introduce new frameworks, services, or architectural patterns without a clear reason.
9. **Review weekly.** I will assess progress, identify knowledge gaps, and adjust the next week's plan.
10. **Build independently.** By the end of the project, I will implement at least one additional small feature without following a step-by-step tutorial.

## 8. Definition of Done

A feature is not considered complete merely because it works once in the browser.

A feature is complete when:

* Requirements are defined.
* The data model and API contract are understood.
* Server-side validation is implemented.
* Authorization is enforced where applicable.
* Normal and failure cases have been tested.
* The frontend handles loading, success, empty, and error states where relevant.
* The feature is committed to Git.
* Important decisions are documented.
* I can explain the implementation and its limitations.

## 9. Final Evaluation

At the end of Day 90, the project will be evaluated against these criteria:

| Category                                              |   Weight |
| ----------------------------------------------------- | -------: |
| Backend fundamentals and API design                   |      20% |
| Database design and SQL                               |      15% |
| Business logic and data consistency                   |      15% |
| Authentication, authorization, and tenant isolation   |      15% |
| Frontend and backend integration                      |      10% |
| Automated testing and debugging                       |      10% |
| Deployment, documentation, and portfolio presentation |      10% |
| Independent technical explanation                     |       5% |
| **Total**                                             | **100%** |

**Passing target:** 80/100, provided that the essential security, data-integrity, and deployment requirements are also satisfied.

A high score does not excuse critical security failures. If tenant isolation or authorization is broken, that issue must be fixed before the project can be considered complete.

## 10. Final Deliverables

By the end of the contract, I will deliver:

* [ ] A functional, deployed BusinessHub application.
* [ ] A documented PostgreSQL schema.
* [ ] A set of tested REST API endpoints.
* [ ] Authentication and role-based authorization.
* [ ] Customer, transaction, and loyalty workflows.
* [ ] Multi-business data isolation.
* [ ] A reporting dashboard.
* [ ] Automated tests for important business rules and permissions.
* [ ] API documentation.
* [ ] A complete GitHub repository and README.
* [ ] An architecture and database relationship diagram.
* [ ] A portfolio case study and recorded demonstration.
* [ ] A personal technical review documenting what I learned and what remains to improve.

## 11. Change Management

New features may be added only when they support the learning objectives or are necessary to complete the core application.

If a task takes longer than expected, I will first reduce optional scope or simplify the implementation. I will not sacrifice the fundamentals of security, data consistency, testing, or understanding merely to meet a deadline.

The core scope and final acceptance criteria will remain the priority.

## 12. Commitment

I commit to treating this project as a professional engineering assignment rather than a tutorial exercise.

My responsibility is not simply to make the software work. It is to understand why it works, know how to verify that it works, and be able to maintain and improve it.

**Project Owner / Developer:** ______________________

**Start Date:** ______________________

**Target Completion Date:** ______________________

**Final Review Date:** ______________________
