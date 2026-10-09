# BusinessHub — Complete System Specification

BusinessHub is a multi-business management platform that allows different businesses, such as restaurants and salons, to manage their customers, employees, transactions, loyalty rewards, and reports through one web application.

This document explains the system from two perspectives:

* Functional specification: What the system does, who uses it, what users see, and how it should behave.

* Technical specification: How the frontend, backend, database, authentication, business rules, and APIs work together.

Think of this as the product blueprint you would receive before joining a software development team. You should be able to use it to design the UI, plan the database, implement the APIs, and test whether the finished application meets its requirements.

## 1. What are we building?

Imagine a salon called Amra and a restaurant called Blue Plate.

Both want to manage customers, staff, transactions, and business reports. Instead of building two completely separate applications, BusinessHub lets both businesses use the same platform.

![Salon reception desk designs with led light | Salon interior design, Counter design, Reception desk design](https://images.openai.com/static-rsc-4/S7sNYFs0pb8diQ6xgouPLLUjyOSCjGWymH-9ep64X1qcCqPakj687t63WUh7LdsSSzzIPjIUzwegr176lWmdBWizjzhAd3qz_DApgnghgMklIp3OAvlo4qwLn-iGE7e7_Y97mqBd0t8B89tpZvP4H_NbPasFVd_ldP-fCusK8eo?purpose=inline)

Add to Favorites

### Amra Nail Salon

Business account A

Manages salon customers, visits, staff, and loyalty rewards.

![The Saffron House - Updated 2026, Indian Restaurant in Blackburn South, AU-VIC](https://images.openai.com/static-rsc-4/mucABz8g0pbLHHlFa7a7f7aTBrsheNMfU6T195AMBT8QpuL3ucRvdvtpOBupNHkmTZi2v8bp2lD1pgVF0CwLYONDRTc9pTLU5MutbDmBHnGAY_sOEpQ7uCwGjLj9b5BCqQwGjaHdVx7Y7z9AiSg1M8CPki1OXT4P7YZL8pS7HMg?purpose=inline)

Add to Favorites

### Blue Plate Restaurant

Business account B

Manages restaurant customers, transactions, employees, and reports.

One application, separate business data

Each business has its own records, staff permissions, and reports. Users cannot access another business's data without explicit authorization.

The product is not a collection of unrelated CRUD pages. It is a connected system in which one action affects other parts of the application.

For example, when a staff member records a customer's purchase:

1. The backend verifies that the staff member is authorized.

2. The backend validates the customer and purchase amount.

3. The transaction is saved in the database.

4. Loyalty points are calculated using the business's rules.

5. The points balance and transaction history are updated consistently.

6. The frontend displays the successful transaction and updated customer balance.

7. The business's reports reflect the new transaction.

That end-to-end workflow is the heart of the project.

## 2. Who uses the system?

BusinessHub has three primary user roles.

![Hiring Contractors vs Employees: Benefits and Challenges](https://images.openai.com/static-rsc-4/4Xgqw6h_i2L8lRve3fz1kEjDJjzw_P-VGjm1rk8YXPQ9-6YgpB7GUZBOt6Mv7l-RbmBqMhp4D2W2Sf4BxFGSJLZnplytxqob0zVLmoZi_rXLtEYRjZf1gbFtS_pbWxtbL6omQjTvkstPu8Dt86b9Br90EU01RMoLTd_4p9IMBtw?purpose=inline)

Add to Favorites

1. Business owner

The owner controls a business account.

* Views the business dashboard and reports.

* Creates and manages staff accounts or invitations.

* Adds and manages customers.

* Reviews transactions and reward redemptions.

* Configures loyalty rules and rewards.

* Manages business settings.

![Kekasir.id - Solusi Kasir Pintar & Manajemen Bisnis Terlengkap](https://images.openai.com/static-rsc-4/ub9Sm1ky0sH0rQbzA6VLLCGIq_bB88Aa9aQ9YVZXZ43_Yw1LLCIcrPdf4_ehSBeTKo1ztb13JLaKwRzmAlN9FrtZQmFjpgl8UAAAAE9DY3_UiRIkVxJITqXq3_SnUDIRTXbgyZ9ky8W-N3IKdEGm-KBHtsnaMBY7T74QzTfJc98?purpose=inline)

Add to Favorites

2. Employee

The employee handles everyday business operations.

* Finds or registers customers.

* Records purchases or service visits.

* Checks customer points and transaction history.

* Processes eligible reward redemptions.

* Views only the information their role permits.

Employees cannot manage business ownership or change restricted settings unless explicitly granted permission.

![Hammer Studio | Engineering the Future of Media & AI Technology](https://images.openai.com/static-rsc-4/fIU7o1ERw-rmgT7P-uKnkYB38Bxmf-FUxpcgPR6RcS6LZkd7nYWLv3iz03UNvUoBe91xkqRjJ_2EDdEBnODYRVTLwXfkVoMfocsB96fAJviFZ6vspWVbXGBRA3NjThvz3WGvrTKkmasgQeMoL9z-rHrHFFAMnD--zoF5j2Of2UY?purpose=inline)

Add to Favorites

3. Platform administrator — optional future scope

This person manages the overall BusinessHub platform rather than an individual business.

* Reviews registered businesses.

* Manages platform-level access and support.

* Suspends accounts when appropriate.

* Views platform-level operational information.

This role is not required for the first release. Do not confuse a platform administrator with a business owner.

For the initial version, implement the business owner and employee roles first.

## 3. Functional specification: What each part of the application does

### 3.1 Authentication and account access

![Fullscreen login form with image backround - Tailwind CSS Example](https://images.openai.com/static-rsc-4/wH_rjEUUs42tu1zCYs2wVdaCM0zLu0P4Arj3Dk3ij2lj-U95ORFPG_ztphx0-JY-hg7-bRIq6z2maV4gl6cyIxSUQ2YHj8GmxA6OUBEfI9ngmfm2x2oD3EYJryUg53EQNQkwtX25aVlyYKtip9BUdrPFvQomUqufVdZ3pzEasBM?purpose=inline)

Add to Favorites

This is the entry point to the application.

Pages

* `/login` — sign in.

* `/register` — create a new business account and owner.

* `/forgot-password` — optional for the first release.

* `/unauthorized` — explain when a user lacks permission.

Expected behavior

When a user registers, they provide their name, email, and password, along with the business name. The system creates a user, a business, and an owner membership linking the two.

When the user logs in, the system verifies their credentials and creates a secure session. The frontend redirects them to the dashboard.

When they log out, the session ends and protected pages and APIs become inaccessible.

Important rules

* Emails must be unique according to the chosen account model.

* Passwords must be securely hashed, never stored as plaintext.

* Invalid credentials produce a useful message without exposing sensitive details.

* Protected API requests must be authenticated on the server.

* A user may belong to more than one business in the future, but the initial release can support one active business at a time.

### 3.2 Business dashboard

![Sales dashboard — Untitled UI by Jordan Hughes® on Dribbble](https://images.openai.com/static-rsc-4/9PZ_B5frfS02klIm_rn0MqYdQEwFSJTzGEYwvuB2Kbulan4TJzCcitcv58GajqdSVhnqnWlASU3RtN-SarLIpLlSKmlXLtK8XoyGFDXeIqhx_OjUUEKTUHjUKr3ZsD0wQwMRjS0i9N4s-o1t-Ex7RXKX5rRzBto_F1s1uXB5HQk?purpose=inline)

Add to Favorites

[dribbble](https://dribbble.com/shots/20082732-Sales-dashboard-Untitled-UI)

The dashboard is the user's overview of their business.

What it displays

* Total recorded sales for the selected period.

* Number of customers.

* Number of recorded visits or transactions.

* Number of rewards redeemed.

* A sales or transaction activity chart.

* Recent transactions.

* Quick actions such as adding a customer or recording a visit.

Expected behavior

The dashboard loads data from the backend for the authenticated user's active business. Users can select a date range and see the relevant metrics update.

The dashboard should show a loading state while data is being retrieved, a helpful empty state when no transactions exist, and an error state when data cannot be loaded.

Business rules

* Every figure must be derived from actual persisted records.

* Sales totals should exclude cancelled or voided transactions if those statuses are implemented.

* Each metric must use a clearly defined date range.

* A user must never see another business's metrics.

* Changing the date filter must update the relevant cards, chart, and report consistently.

Do not hardcode dashboard numbers just to make the UI look populated.

### 3.3 Customer management

![CRM dashboard — Untitled UI by Jordan Hughes® on Dribbble](https://images.openai.com/static-rsc-4/IODmAMfJPc4RasuJCeMKzDV0WPQ7fqSt30NYpAaXEE9bcR2nNp4rzPF36Zj8JpZo9QRzbHrtx9hBWC1t7z6thGtVA17pXtc4c3ifFr1WrDyJV8mdiyAeBaJSULU78DtiDCRjCMzzdUK0k1vyoqqyBu7gJwdmvYB9c9nUkQXnTFo?purpose=inline)

Add to Favorites

This module maintains the business's customer records.

Pages

* `/customers` — customer list.

* `/customers/new` — create a customer.

* `/customers/[id]` — view customer details and activity.

* `/customers/[id]/edit` — update customer information.

Customer list

Display the customer's name, phone number, email when available, total visits, points balance, and date added.

Provide search, pagination, filters where useful, and an obvious “Add customer” action.

Creating a customer

The form should include:

* Full name — required.

* Phone number — required for the initial version.

* Email — optional.

* Notes — optional.

On submission, the backend validates the fields and creates a customer associated with the active business.

If the request succeeds, show a confirmation and update the customer list. If it fails, retain the user's input and display the relevant error.

Customer details

A customer's detail page should display:

* Contact information.

* Current points balance.

* Transaction and visit history.

* Rewards redeemed.

* Account creation date.

Important rules

* Every customer belongs to a business.

* Customers cannot be silently transferred between businesses.

* Editing a customer must not overwrite their transaction history.

* Archiving a customer should preserve historical transaction records.

* The backend must verify business ownership before reading or modifying a customer.

### 3.4 Transactions and service visits

![Salon Next – AI Powered Salon, Spa & Parlor Management System by CodeBasin](https://images.openai.com/static-rsc-4/dyDJKTDFz__Vs0u0gRiNLl43mMWKlid8LZSPrX5hoSpXqHZCN2Y0LHYRnOcPRcuOo1HLb5QgLARj8H2249VPIwnhJz74juc-dg2mb2LTgcWniYgWasxrtoXTadIcV8UK5VDyyGXgI053_TZSXDs_rHyiLvk-bAQ9xsx-71zJX3U?purpose=inline)

Add to Favorites

This module records business activity. A transaction might represent a salon visit, a restaurant purchase, or another customer purchase.

Pages

* `/transactions` — transaction history.

* `/transactions/new` — record a transaction.

* `/transactions/[id]` — view transaction details.

Creating a transaction

The employee selects a customer, enters the purchase amount, optionally chooses a service or category, and submits the form.

The backend must:

1. Authenticate the user.

2. Verify their permission to record transactions.

3. Verify that the customer belongs to the active business.

4. Validate the amount and any optional fields.

5. Calculate points using the business's active rules.

6. Persist the transaction and points earned consistently.

7. Return the created transaction and the updated points balance.

Example

Suppose a salon awards one point per 100 ETB spent, rounded down. A customer spends 850 ETB.

* Transaction amount: 850 ETB.

* Points earned: 8.

* Previous points balance: 12.

* New points balance: 20.

The rule is illustrative and must be explicitly defined in the system; it must not be inferred from frontend calculations.

Important rules

* The client cannot decide how many points a transaction earns.

* Amounts must be positive and validated.

* Transactions must belong to the correct business and customer.

* Points and transaction records must not get out of sync if a database operation fails.

* Retrying a submission must not accidentally create a duplicate transaction.

* Historical transactions should not be silently edited or deleted. If corrections are needed, use a controlled void or reversal process.

### 3.5 Loyalty points and rewards

![LoyaltyPlus: Elevate Your Rewards Experience - Next Bee Corporation - AppExchange](https://images.openai.com/static-rsc-4/cGQr9fAvVC4_-zcpZ5OnbkJX_XwbdCxaz5AwUZmXOjUVbAOJ6aYrRSmskQ0AyfhHDucgWaF51DxuZJwSwFj0_wdn-5KUtqG72-_GYm1Kp6bwiKVl1gl_JPm4WiBrGR8R0qof_Or_3EOtz9nCaeNanbQgsJB5K-OsU8QL_CphsF4?purpose=inline)

Add to Favorites

This module encourages repeat customers by awarding points and allowing eligible customers to redeem rewards.

Pages

* `/rewards` — list and manage available rewards.

* `/rewards/new` — create a reward.

* `/redemptions` — review redemption history.

* Customer details — view current points and available rewards.

Reward fields

Each reward should have:

* Name and description.

* Points cost.

* Active or inactive status.

* Optional redemption limits or expiry rules, if implemented.

Redeeming a reward

The employee selects the customer and the reward. The system checks the customer's balance, reward availability, business ownership, and user permissions.

If the redemption is valid, the system records it and deducts the required points atomically. If the operation fails, the points balance remains unchanged.

Example

A customer has 20 points. A reward costs 15 points.

After successful redemption:

* Previous balance: 20 points.

* Points spent: 15.

* Remaining balance: 5 points.

* One redemption record is created.

If two requests attempt to redeem the same available points concurrently, the backend must prevent an invalid negative balance or duplicate redemption.

Important distinction: Points earned, points spent, and current balance should be traceable. For a stronger design, use a points ledger recording earning and spending entries instead of relying solely on a mutable balance field. The balance can then be maintained transactionally and reconciled against the ledger.

### 3.6 Staff management

![Teams & Invitations | SaaSykit](https://images.openai.com/static-rsc-4/qjSUWEfpLXj-EbwGk8w16iiAzPVBWw_vNaP2pEqAGKj4cGhL7Pnpj5YAzzp5xTMmAhkGlTEQOiiOb5vFILyrRUHWc9wE43xRfdqF1w3YTCyemDP_Uek-_Y2_MjWPjUysZ6u4lb2OA7MD8HBcLtD35DG3mPcdSIXS4Q0FWIznxrs?purpose=inline)

Add to Favorites

This module lets business owners manage employees.

Pages

* `/staff` — staff list.

* `/staff/invite` — invite or add a staff member.

* `/staff/[id]` — view staff membership details.

Expected behavior

Owners can add staff, assign the employee role, and deactivate or remove a membership. An employee should see only the features and data permitted by their role.

For the initial release, you can implement adding an existing user to a business before implementing email invitations.

Important rules

* Only an authorized owner can manage staff membership.

* Employees cannot promote themselves to owners.

* Deactivating membership revokes access to that business.

* Historical transactions must retain the identity of the staff member who performed them.

* The system must not allow the last active owner to accidentally remove their own access without a recovery mechanism.

### 3.7 Reports and analytics

![PowerCentral – Power BI reporting for Business Central](https://images.openai.com/static-rsc-4/Jk1y04rW10Wh7f4F87uF45Hnp4gEaRBP50qqFRViu6ZcDMQlHUlbg9Tm7eqPpRx698gLIJci7s5R7NIxSKT7U4kWPT_cl_j1zUb94VttAWi6Z5wuyGyaIjivELp67ZlFxlp90Fm3fpV89zYqMVDOy_WN_ueqJidhI5ojGins97E?purpose=inline)

Add to Favorites

Reports answer questions such as:

* How much did the business record in sales this month?

* How many visits occurred during a selected period?

* Which services or categories were most popular?

* How many rewards were redeemed?

* Which employees recorded the most transactions?

Page: `/reports`

The user selects a start date and end date. The backend calculates metrics from the business's records and returns the results.

Important rules

* Calculations should use database queries rather than loading all records into the browser.

* Date ranges should have documented boundary behavior and use a consistent business timezone.

* Currency totals must be calculated precisely, avoiding floating-point arithmetic errors.

* Employee performance should be based on a clearly defined metric.

* Every query must be scoped to the authorized business.

Advanced exports, forecasting, and complex analytics can be postponed until the core reporting works.

## 4. How the system should look

The visual direction is a clean, professional business SaaS dashboard. It should look like software a real business owner would feel comfortable using daily—not a collection of disconnected demo screens.

![Modern Analytics Dashboard – Clean React UI/UX by Ahmed hamdy on Dribbble](https://images.openai.com/static-rsc-4/Wqjg8fyIUUNzgSgJIG5fyUBXiaLUM6vypIwycuORSY_-PgHPSZA8WkLbUuXwOsn9A2lkt_coHG8UDmLiN6pUk5rCrfUPy-JMdigcNOv0EtwuW1fwu5QSWTL9XcfNqIbYciOe-CIXVW_76HsNfKzF0BkXgfoEUTfnSLlTh3bczns?purpose=inline)

Add to Favorites

Application layout

* Fixed or collapsible left sidebar on desktop.

* Top bar containing the active business, user menu, and page title context.

* Main content area with consistent spacing.

* Responsive layout for smaller screens.

![Dashboard design using warm grey by Rahul Rao on Dribbble](https://images.openai.com/static-rsc-4/P2ihrUTuo9kqrGB-pBKYiqjfZSaSy8lYXHudyxz6nzzF-DJ7RCUqJw6zIY8Ty1PwOSq6lLx60SWiEitRm1d29NpLGgkFka5qXDiTgOBRVUFph3jQYGjmdS1r1Epa5-pHzYcYpLS_oeWz_iiKv3WhQ3kjlRTsOPD46XmvINklwEM?purpose=inline)

Add to Favorites

Visual style

* White or soft neutral surfaces.

* Dark, readable text and subtle borders.

* One restrained primary accent color.

* Consistent typography, spacing, and button styles.

* Avoid excessive gradients, shadows, and decorative animations.

![Tyro Dashboard Documentation - Beautiful Admin Dashboard for Laravel 12](https://images.openai.com/static-rsc-4/BqJrKT6X4VnQ3-32QeiWdPg8ef3Pwd1n6KmKhSl62_RRZ7NQyWQIeZ66XRl4XttKNLw0OJQuNTjITKCfHtgkytykSFrRwapclVDH5l4zxTSw9oFZ7-wMYW74W707JcxwpzXGSCz2-0Zblx4RFlXESMz2xHyRmS8tsIyWzu27T7U?purpose=inline)

Add to Favorites

Data-heavy pages

* Clear table headings and readable row spacing.

* Search and filters placed above tables.

* Visible pagination and record counts.

* Status labels that are not distinguished by color alone.

* Confirmation dialogs for destructive actions.

### Application navigation

The initial desktop sidebar should contain:

Dashboard

Customers

Transactions

Rewards

Redemptions

Reports

Staff

Settings

The visible navigation and actions depend on the user's role and permissions.

### UI behavior requirements

Every page must account for more than the happy path.

| State            | Expected behavior                                                            |
| ---------------- | ---------------------------------------------------------------------------- |
| Loading          | Show a spinner or skeleton and prevent accidental duplicate submissions.     |
| Empty            | Explain that there is no data and offer a relevant next action.              |
| Success          | Show confirmation and refresh the relevant data.                             |
| Validation error | Show the error near the relevant field where possible.                       |
| Server error     | Show a helpful message and a retry option when appropriate.                  |
| Unauthorized     | Deny the action and explain that access is unavailable.                      |
| Not found        | Show a useful message instead of a broken page.                              |
| Mobile           | Keep core tasks usable without horizontal overflow or inaccessible controls. |

Do not use alerts for every interaction. Prefer inline feedback, toasts for brief confirmations, and dialogs when users need to confirm an important action.

## 5. Technical specification: How the application works

The application has four main layers:

1. Frontend — Next.js UI

Pages, forms, tables, charts, client interactions

2. API layer — Route Handlers

HTTP requests, authentication, validation, status codes

3. Business logic — Server-side services

Permissions, points calculations, redemption rules, workflows

4. Data layer — Prisma and PostgreSQL

Queries, relationships, transactions, constraints, persistence

These are logical layers, not necessarily four separately deployed services. In this project, they can live inside one Next.js codebase.

### Example: What happens when an employee records a purchase?

Imagine an employee submits a form for a customer who spends 850 ETB.

Diagram options

![](data\:image/svg+xml;utf8,%3Csvg%20id%3D%22mermaid-_r_p1_%22%20width%3D%221418%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20height%3D%22855%22%20viewBox%3D%22-50%20-10%201418%20855%22%20role%3D%22graphics-document%20document%22%20aria-roledescription%3D%22sequence%22%3E%3Cg%3E%3Crect%20x%3D%221168%22%20y%3D%22769%22%20fill%3D%22%23eaeaea%22%20stroke%3D%22%23666%22%20width%3D%22150%22%20height%3D%2265%22%20name%3D%22DB%22%20rx%3D%223%22%20ry%3D%223%22%20class%3D%22actor%20actor-bottom%22%3E%3C%2Frect%3E%3Ctext%20x%3D%221243%22%20y%3D%22801.5%22%20dominant-baseline%3D%22central%22%20alignment-baseline%3D%22central%22%20class%3D%22actor%20actor-box%22%20style%3D%22text-anchor%3A%20middle%3B%20font-size%3A%2016px%3B%20font-weight%3A%20400%3B%20font-family%3A%20-apple-system%2C%20BlinkMacSystemFont%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Roboto%2C%20Oxygen%2C%20Ubuntu%2C%20Cantarell%2C%20%26quot%3BHelvetica%20Neue%26quot%3B%2C%20Arial%2C%20%26quot%3Bsans-serif%26quot%3B%3B%22%3E%3Ctspan%20x%3D%221243%22%20dy%3D%220%22%3EPostgreSQL%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3Cg%3E%3Crect%20x%3D%22854%22%20y%3D%22769%22%20fill%3D%22%23eaeaea%22%20stroke%3D%22%23666%22%20width%3D%22150%22%20height%3D%2265%22%20name%3D%22S%22%20rx%3D%223%22%20ry%3D%223%22%20class%3D%22actor%20actor-bottom%22%3E%3C%2Frect%3E%3Ctext%20x%3D%22929%22%20y%3D%22801.5%22%20dominant-baseline%3D%22central%22%20alignment-baseline%3D%22central%22%20class%3D%22actor%20actor-box%22%20style%3D%22text-anchor%3A%20middle%3B%20font-size%3A%2016px%3B%20font-weight%3A%20400%3B%20font-family%3A%20-apple-system%2C%20BlinkMacSystemFont%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Roboto%2C%20Oxygen%2C%20Ubuntu%2C%20Cantarell%2C%20%26quot%3BHelvetica%20Neue%26quot%3B%2C%20Arial%2C%20%26quot%3Bsans-serif%26quot%3B%3B%22%3E%3Ctspan%20x%3D%22929%22%20dy%3D%220%22%3EBusiness%20Logic%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3Cg%3E%3Crect%20x%3D%22550%22%20y%3D%22769%22%20fill%3D%22%23eaeaea%22%20stroke%3D%22%23666%22%20width%3D%22150%22%20height%3D%2265%22%20name%3D%22API%22%20rx%3D%223%22%20ry%3D%223%22%20class%3D%22actor%20actor-bottom%22%3E%3C%2Frect%3E%3Ctext%20x%3D%22625%22%20y%3D%22801.5%22%20dominant-baseline%3D%22central%22%20alignment-baseline%3D%22central%22%20class%3D%22actor%20actor-box%22%20style%3D%22text-anchor%3A%20middle%3B%20font-size%3A%2016px%3B%20font-weight%3A%20400%3B%20font-family%3A%20-apple-system%2C%20BlinkMacSystemFont%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Roboto%2C%20Oxygen%2C%20Ubuntu%2C%20Cantarell%2C%20%26quot%3BHelvetica%20Neue%26quot%3B%2C%20Arial%2C%20%26quot%3Bsans-serif%26quot%3B%3B%22%3E%3Ctspan%20x%3D%22625%22%20dy%3D%220%22%3ERoute%20Handler%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3Cg%3E%3Crect%20x%3D%22319%22%20y%3D%22769%22%20fill%3D%22%23eaeaea%22%20stroke%3D%22%23666%22%20width%3D%22150%22%20height%3D%2265%22%20name%3D%22UI%22%20rx%3D%223%22%20ry%3D%223%22%20class%3D%22actor%20actor-bottom%22%3E%3C%2Frect%3E%3Ctext%20x%3D%22394%22%20y%3D%22801.5%22%20dominant-baseline%3D%22central%22%20alignment-baseline%3D%22central%22%20class%3D%22actor%20actor-box%22%20style%3D%22text-anchor%3A%20middle%3B%20font-size%3A%2016px%3B%20font-weight%3A%20400%3B%20font-family%3A%20-apple-system%2C%20BlinkMacSystemFont%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Roboto%2C%20Oxygen%2C%20Ubuntu%2C%20Cantarell%2C%20%26quot%3BHelvetica%20Neue%26quot%3B%2C%20Arial%2C%20%26quot%3Bsans-serif%26quot%3B%3B%22%3E%3Ctspan%20x%3D%22394%22%20dy%3D%220%22%3ENext.js%20Frontend%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3Cg%3E%3C%2Fg%3E%3Cg%3E%3Cline%20id%3D%22actor4%22%20x1%3D%221243%22%20y1%3D%2265%22%20x2%3D%221243%22%20y2%3D%22769%22%20class%3D%22actor-line%20200%22%20stroke-width%3D%220.5px%22%20stroke%3D%22%23999%22%20name%3D%22DB%22%3E%3C%2Fline%3E%3Cg%20id%3D%22root-4%22%3E%3Crect%20x%3D%221168%22%20y%3D%220%22%20fill%3D%22%23eaeaea%22%20stroke%3D%22%23666%22%20width%3D%22150%22%20height%3D%2265%22%20name%3D%22DB%22%20rx%3D%223%22%20ry%3D%223%22%20class%3D%22actor%20actor-top%22%3E%3C%2Frect%3E%3Ctext%20x%3D%221243%22%20y%3D%2232.5%22%20dominant-baseline%3D%22central%22%20alignment-baseline%3D%22central%22%20class%3D%22actor%20actor-box%22%20style%3D%22text-anchor%3A%20middle%3B%20font-size%3A%2016px%3B%20font-weight%3A%20400%3B%20font-family%3A%20-apple-system%2C%20BlinkMacSystemFont%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Roboto%2C%20Oxygen%2C%20Ubuntu%2C%20Cantarell%2C%20%26quot%3BHelvetica%20Neue%26quot%3B%2C%20Arial%2C%20%26quot%3Bsans-serif%26quot%3B%3B%22%3E%3Ctspan%20x%3D%221243%22%20dy%3D%220%22%3EPostgreSQL%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fg%3E%3Cg%3E%3Cline%20id%3D%22actor3%22%20x1%3D%22929%22%20y1%3D%2265%22%20x2%3D%22929%22%20y2%3D%22769%22%20class%3D%22actor-line%20200%22%20stroke-width%3D%220.5px%22%20stroke%3D%22%23999%22%20name%3D%22S%22%3E%3C%2Fline%3E%3Cg%20id%3D%22root-3%22%3E%3Crect%20x%3D%22854%22%20y%3D%220%22%20fill%3D%22%23eaeaea%22%20stroke%3D%22%23666%22%20width%3D%22150%22%20height%3D%2265%22%20name%3D%22S%22%20rx%3D%223%22%20ry%3D%223%22%20class%3D%22actor%20actor-top%22%3E%3C%2Frect%3E%3Ctext%20x%3D%22929%22%20y%3D%2232.5%22%20dominant-baseline%3D%22central%22%20alignment-baseline%3D%22central%22%20class%3D%22actor%20actor-box%22%20style%3D%22text-anchor%3A%20middle%3B%20font-size%3A%2016px%3B%20font-weight%3A%20400%3B%20font-family%3A%20-apple-system%2C%20BlinkMacSystemFont%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Roboto%2C%20Oxygen%2C%20Ubuntu%2C%20Cantarell%2C%20%26quot%3BHelvetica%20Neue%26quot%3B%2C%20Arial%2C%20%26quot%3Bsans-serif%26quot%3B%3B%22%3E%3Ctspan%20x%3D%22929%22%20dy%3D%220%22%3EBusiness%20Logic%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fg%3E%3Cg%3E%3Cline%20id%3D%22actor2%22%20x1%3D%22625%22%20y1%3D%2265%22%20x2%3D%22625%22%20y2%3D%22769%22%20class%3D%22actor-line%20200%22%20stroke-width%3D%220.5px%22%20stroke%3D%22%23999%22%20name%3D%22API%22%3E%3C%2Fline%3E%3Cg%20id%3D%22root-2%22%3E%3Crect%20x%3D%22550%22%20y%3D%220%22%20fill%3D%22%23eaeaea%22%20stroke%3D%22%23666%22%20width%3D%22150%22%20height%3D%2265%22%20name%3D%22API%22%20rx%3D%223%22%20ry%3D%223%22%20class%3D%22actor%20actor-top%22%3E%3C%2Frect%3E%3Ctext%20x%3D%22625%22%20y%3D%2232.5%22%20dominant-baseline%3D%22central%22%20alignment-baseline%3D%22central%22%20class%3D%22actor%20actor-box%22%20style%3D%22text-anchor%3A%20middle%3B%20font-size%3A%2016px%3B%20font-weight%3A%20400%3B%20font-family%3A%20-apple-system%2C%20BlinkMacSystemFont%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Roboto%2C%20Oxygen%2C%20Ubuntu%2C%20Cantarell%2C%20%26quot%3BHelvetica%20Neue%26quot%3B%2C%20Arial%2C%20%26quot%3Bsans-serif%26quot%3B%3B%22%3E%3Ctspan%20x%3D%22625%22%20dy%3D%220%22%3ERoute%20Handler%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fg%3E%3Cg%3E%3Cline%20id%3D%22actor1%22%20x1%3D%22394%22%20y1%3D%2265%22%20x2%3D%22394%22%20y2%3D%22769%22%20class%3D%22actor-line%20200%22%20stroke-width%3D%220.5px%22%20stroke%3D%22%23999%22%20name%3D%22UI%22%3E%3C%2Fline%3E%3Cg%20id%3D%22root-1%22%3E%3Crect%20x%3D%22319%22%20y%3D%220%22%20fill%3D%22%23eaeaea%22%20stroke%3D%22%23666%22%20width%3D%22150%22%20height%3D%2265%22%20name%3D%22UI%22%20rx%3D%223%22%20ry%3D%223%22%20class%3D%22actor%20actor-top%22%3E%3C%2Frect%3E%3Ctext%20x%3D%22394%22%20y%3D%2232.5%22%20dominant-baseline%3D%22central%22%20alignment-baseline%3D%22central%22%20class%3D%22actor%20actor-box%22%20style%3D%22text-anchor%3A%20middle%3B%20font-size%3A%2016px%3B%20font-weight%3A%20400%3B%20font-family%3A%20-apple-system%2C%20BlinkMacSystemFont%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Roboto%2C%20Oxygen%2C%20Ubuntu%2C%20Cantarell%2C%20%26quot%3BHelvetica%20Neue%26quot%3B%2C%20Arial%2C%20%26quot%3Bsans-serif%26quot%3B%3B%22%3E%3Ctspan%20x%3D%22394%22%20dy%3D%220%22%3ENext.js%20Frontend%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fg%3E%3Cg%3E%3Cline%20id%3D%22actor0%22%20x1%3D%2275%22%20y1%3D%2280%22%20x2%3D%2275%22%20y2%3D%22769%22%20class%3D%22actor-line%20200%22%20stroke-width%3D%220.5px%22%20stroke%3D%22%23999%22%20name%3D%22E%22%3E%3C%2Fline%3E%3C%2Fg%3E%3Cstyle%3E%23mermaid-_r_p1_%7Bfont-family%3A%22-apple-system%22%2C%22BlinkMacSystemFont%22%2C%22Segoe%20UI%22%2C%22Roboto%22%2C%22Oxygen%22%2C%22Ubuntu%22%2C%22Cantarell%22%2C%22Helvetica%20Neue%22%2C%22Arial%22%2C%22sans-serif%22%3Bfont-size%3A14px%3Bfill%3Argb\(255%2C%20255%2C%20255\)%3B%7D%40keyframes%20edge-animation-frame%7Bfrom%7Bstroke-dashoffset%3A0%3B%7D%7D%40keyframes%20dash%7Bto%7Bstroke-dashoffset%3A0%3B%7D%7D%23mermaid-_r_p1_%20.edge-animation-slow%7Bstroke-dasharray%3A9%2C5!important%3Bstroke-dashoffset%3A900%3Banimation%3Adash%2050s%20linear%20infinite%3Bstroke-linecap%3Around%3B%7D%23mermaid-_r_p1_%20.edge-animation-fast%7Bstroke-dasharray%3A9%2C5!important%3Bstroke-dashoffset%3A900%3Banimation%3Adash%2020s%20linear%20infinite%3Bstroke-linecap%3Around%3B%7D%23mermaid-_r_p1_%20.error-icon%7Bfill%3Argb\(33%2C%2033%2C%2033\)%3B%7D%23mermaid-_r_p1_%20.error-text%7Bfill%3Argb\(255%2C%20255%2C%20255\)%3Bstroke%3Argb\(255%2C%20255%2C%20255\)%3B%7D%23mermaid-_r_p1_%20.edge-thickness-normal%7Bstroke-width%3A1px%3B%7D%23mermaid-_r_p1_%20.edge-thickness-thick%7Bstroke-width%3A3.5px%3B%7D%23mermaid-_r_p1_%20.edge-pattern-solid%7Bstroke-dasharray%3A0%3B%7D%23mermaid-_r_p1_%20.edge-thickness-invisible%7Bstroke-width%3A0%3Bfill%3Anone%3B%7D%23mermaid-_r_p1_%20.edge-pattern-dashed%7Bstroke-dasharray%3A3%3B%7D%23mermaid-_r_p1_%20.edge-pattern-dotted%7Bstroke-dasharray%3A2%3B%7D%23mermaid-_r_p1_%20.marker%7Bfill%3Argb\(205%2C%20205%2C%20205\)%3Bstroke%3Argb\(205%2C%20205%2C%20205\)%3B%7D%23mermaid-_r_p1_%20.marker.cross%7Bstroke%3Argb\(205%2C%20205%2C%20205\)%3B%7D%23mermaid-_r_p1_%20svg%7Bfont-family%3A%22-apple-system%22%2C%22BlinkMacSystemFont%22%2C%22Segoe%20UI%22%2C%22Roboto%22%2C%22Oxygen%22%2C%22Ubuntu%22%2C%22Cantarell%22%2C%22Helvetica%20Neue%22%2C%22Arial%22%2C%22sans-serif%22%3Bfont-size%3A14px%3B%7D%23mermaid-_r_p1_%20p%7Bmargin%3A0%3B%7D%23mermaid-_r_p1_%20.actor%7Bstroke%3Argb\(31%2C%2078%2C%20148\)%3Bfill%3Argb\(9%2C%2023%2C%2044\)%3B%7D%23mermaid-_r_p1_%20text.actor%26gt%3Btspan%7Bfill%3Argb\(255%2C%20255%2C%20255\)%3Bstroke%3Anone%3B%7D%23mermaid-_r_p1_%20.actor-line%7Bstroke%3Argb\(205%2C%20205%2C%20205\)%3B%7D%23mermaid-_r_p1_%20.innerArc%7Bstroke-width%3A1.5%3Bstroke-dasharray%3Anone%3B%7D%23mermaid-_r_p1_%20.messageLine0%7Bstroke-width%3A1.5%3Bstroke-dasharray%3Anone%3Bstroke%3Argb\(205%2C%20205%2C%20205\)%3B%7D%23mermaid-_r_p1_%20.messageLine1%7Bstroke-width%3A1.5%3Bstroke-dasharray%3A2%2C2%3Bstroke%3Argb\(205%2C%20205%2C%20205\)%3B%7D%23mermaid-_r_p1_%20%23arrowhead%20path%7Bfill%3Argb\(205%2C%20205%2C%20205\)%3Bstroke%3Argb\(205%2C%20205%2C%20205\)%3B%7D%23mermaid-_r_p1_%20.sequenceNumber%7Bfill%3A%23323232%3B%7D%23mermaid-_r_p1_%20%23sequencenumber%7Bfill%3Argb\(205%2C%20205%2C%20205\)%3B%7D%23mermaid-_r_p1_%20%23crosshead%20path%7Bfill%3Argb\(205%2C%20205%2C%20205\)%3Bstroke%3Argb\(205%2C%20205%2C%20205\)%3B%7D%23mermaid-_r_p1_%20.messageText%7Bfill%3Argb\(255%2C%20255%2C%20255\)%3Bstroke%3Anone%3B%7D%23mermaid-_r_p1_%20.labelBox%7Bstroke%3Argba\(255%2C%20255%2C%20255%2C%200.05\)%3Bfill%3Argb\(0%2C%200%2C%200\)%3B%7D%23mermaid-_r_p1_%20.labelText%2C%23mermaid-_r_p1_%20.labelText%26gt%3Btspan%7Bfill%3Argb\(255%2C%20255%2C%20255\)%3Bstroke%3Anone%3B%7D%23mermaid-_r_p1_%20.loopText%2C%23mermaid-_r_p1_%20.loopText%26gt%3Btspan%7Bfill%3Argb\(255%2C%20255%2C%20255\)%3Bstroke%3Anone%3B%7D%23mermaid-_r_p1_%20.loopLine%7Bstroke-width%3A2px%3Bstroke-dasharray%3A2%2C2%3Bstroke%3Argba\(255%2C%20255%2C%20255%2C%200.05\)%3Bfill%3Argba\(255%2C%20255%2C%20255%2C%200.05\)%3B%7D%23mermaid-_r_p1_%20.note%7Bstroke%3Argb\(58%2C%20132%2C%2063\)%3Bfill%3Argb\(33%2C%2033%2C%2033\)%3B%7D%23mermaid-_r_p1_%20.noteText%2C%23mermaid-_r_p1_%20.noteText%26gt%3Btspan%7Bfill%3Argb\(255%2C%20255%2C%20255\)%3Bstroke%3Anone%3B%7D%23mermaid-_r_p1_%20.activation0%7Bfill%3Argb\(33%2C%2033%2C%2033\)%3Bstroke%3Ahsl\(0%2C%200%25%2C%202.9411764706%25\)%3B%7D%23mermaid-_r_p1_%20.activation1%7Bfill%3Argb\(33%2C%2033%2C%2033\)%3Bstroke%3Ahsl\(0%2C%200%25%2C%202.9411764706%25\)%3B%7D%23mermaid-_r_p1_%20.activation2%7Bfill%3Argb\(33%2C%2033%2C%2033\)%3Bstroke%3Ahsl\(0%2C%200%25%2C%202.9411764706%25\)%3B%7D%23mermaid-_r_p1_%20.actorPopupMenu%7Bposition%3Aabsolute%3B%7D%23mermaid-_r_p1_%20.actorPopupMenuPanel%7Bposition%3Aabsolute%3Bfill%3Argb\(9%2C%2023%2C%2044\)%3Bbox-shadow%3A0px%208px%2016px%200px%20rgba\(0%2C0%2C0%2C0.2\)%3Bfilter%3Adrop-shadow\(3px%205px%202px%20rgb\(0%200%200%20%2F%200.4\)\)%3B%7D%23mermaid-_r_p1_%20.actor-man%20line%7Bstroke%3Argb\(31%2C%2078%2C%20148\)%3Bfill%3Argb\(9%2C%2023%2C%2044\)%3B%7D%23mermaid-_r_p1_%20.actor-man%20circle%2C%23mermaid-_r_p1_%20line%7Bstroke%3Argb\(31%2C%2078%2C%20148\)%3Bfill%3Argb\(9%2C%2023%2C%2044\)%3Bstroke-width%3A2px%3B%7D%23mermaid-_r_p1_%20.node%7Bcolor-scheme%3Adark%3B%7D%23mermaid-_r_p1_%20%3Aroot%7B--mermaid-font-family%3A%22-apple-system%22%2C%22BlinkMacSystemFont%22%2C%22Segoe%20UI%22%2C%22Roboto%22%2C%22Oxygen%22%2C%22Ubuntu%22%2C%22Cantarell%22%2C%22Helvetica%20Neue%22%2C%22Arial%22%2C%22sans-serif%22%3B%7D%3C%2Fstyle%3E%3Cg%3E%3C%2Fg%3E%3Cdefs%3E%3Csymbol%20id%3D%22computer%22%20width%3D%2224%22%20height%3D%2224%22%3E%3Cpath%20transform%3D%22scale\(.5\)%22%20d%3D%22M2%202v13h20v-13h-20zm18%2011h-16v-9h16v9zm-10.228%206l.466-1h3.524l.467%201h-4.457zm14.228%203h-24l2-6h2.104l-1.33%204h18.45l-1.297-4h2.073l2%206zm-5-10h-14v-7h14v7z%22%3E%3C%2Fpath%3E%3C%2Fsymbol%3E%3C%2Fdefs%3E%3Cdefs%3E%3Csymbol%20id%3D%22database%22%20fill-rule%3D%22evenodd%22%20clip-rule%3D%22evenodd%22%3E%3Cpath%20transform%3D%22scale\(.5\)%22%20d%3D%22M12.258.001l.256.004.255.005.253.008.251.01.249.012.247.015.246.016.242.019.241.02.239.023.236.024.233.027.231.028.229.031.225.032.223.034.22.036.217.038.214.04.211.041.208.043.205.045.201.046.198.048.194.05.191.051.187.053.183.054.18.056.175.057.172.059.168.06.163.061.16.063.155.064.15.066.074.033.073.033.071.034.07.034.069.035.068.035.067.035.066.035.064.036.064.036.062.036.06.036.06.037.058.037.058.037.055.038.055.038.053.038.052.038.051.039.05.039.048.039.047.039.045.04.044.04.043.04.041.04.04.041.039.041.037.041.036.041.034.041.033.042.032.042.03.042.029.042.027.042.026.043.024.043.023.043.021.043.02.043.018.044.017.043.015.044.013.044.012.044.011.045.009.044.007.045.006.045.004.045.002.045.001.045v17l-.001.045-.002.045-.004.045-.006.045-.007.045-.009.044-.011.045-.012.044-.013.044-.015.044-.017.043-.018.044-.02.043-.021.043-.023.043-.024.043-.026.043-.027.042-.029.042-.03.042-.032.042-.033.042-.034.041-.036.041-.037.041-.039.041-.04.041-.041.04-.043.04-.044.04-.045.04-.047.039-.048.039-.05.039-.051.039-.052.038-.053.038-.055.038-.055.038-.058.037-.058.037-.06.037-.06.036-.062.036-.064.036-.064.036-.066.035-.067.035-.068.035-.069.035-.07.034-.071.034-.073.033-.074.033-.15.066-.155.064-.16.063-.163.061-.168.06-.172.059-.175.057-.18.056-.183.054-.187.053-.191.051-.194.05-.198.048-.201.046-.205.045-.208.043-.211.041-.214.04-.217.038-.22.036-.223.034-.225.032-.229.031-.231.028-.233.027-.236.024-.239.023-.241.02-.242.019-.246.016-.247.015-.249.012-.251.01-.253.008-.255.005-.256.004-.258.001-.258-.001-.256-.004-.255-.005-.253-.008-.251-.01-.249-.012-.247-.015-.245-.016-.243-.019-.241-.02-.238-.023-.236-.024-.234-.027-.231-.028-.228-.031-.226-.032-.223-.034-.22-.036-.217-.038-.214-.04-.211-.041-.208-.043-.204-.045-.201-.046-.198-.048-.195-.05-.19-.051-.187-.053-.184-.054-.179-.056-.176-.057-.172-.059-.167-.06-.164-.061-.159-.063-.155-.064-.151-.066-.074-.033-.072-.033-.072-.034-.07-.034-.069-.035-.068-.035-.067-.035-.066-.035-.064-.036-.063-.036-.062-.036-.061-.036-.06-.037-.058-.037-.057-.037-.056-.038-.055-.038-.053-.038-.052-.038-.051-.039-.049-.039-.049-.039-.046-.039-.046-.04-.044-.04-.043-.04-.041-.04-.04-.041-.039-.041-.037-.041-.036-.041-.034-.041-.033-.042-.032-.042-.03-.042-.029-.042-.027-.042-.026-.043-.024-.043-.023-.043-.021-.043-.02-.043-.018-.044-.017-.043-.015-.044-.013-.044-.012-.044-.011-.045-.009-.044-.007-.045-.006-.045-.004-.045-.002-.045-.001-.045v-17l.001-.045.002-.045.004-.045.006-.045.007-.045.009-.044.011-.045.012-.044.013-.044.015-.044.017-.043.018-.044.02-.043.021-.043.023-.043.024-.043.026-.043.027-.042.029-.042.03-.042.032-.042.033-.042.034-.041.036-.041.037-.041.039-.041.04-.041.041-.04.043-.04.044-.04.046-.04.046-.039.049-.039.049-.039.051-.039.052-.038.053-.038.055-.038.056-.038.057-.037.058-.037.06-.037.061-.036.062-.036.063-.036.064-.036.066-.035.067-.035.068-.035.069-.035.07-.034.072-.034.072-.033.074-.033.151-.066.155-.064.159-.063.164-.061.167-.06.172-.059.176-.057.179-.056.184-.054.187-.053.19-.051.195-.05.198-.048.201-.046.204-.045.208-.043.211-.041.214-.04.217-.038.22-.036.223-.034.226-.032.228-.031.231-.028.234-.027.236-.024.238-.023.241-.02.243-.019.245-.016.247-.015.249-.012.251-.01.253-.008.255-.005.256-.004.258-.001.258.001zm-9.258%2020.499v.01l.001.021.003.021.004.022.005.021.006.022.007.022.009.023.01.022.011.023.012.023.013.023.015.023.016.024.017.023.018.024.019.024.021.024.022.025.023.024.024.025.052.049.056.05.061.051.066.051.07.051.075.051.079.052.084.052.088.052.092.052.097.052.102.051.105.052.11.052.114.051.119.051.123.051.127.05.131.05.135.05.139.048.144.049.147.047.152.047.155.047.16.045.163.045.167.043.171.043.176.041.178.041.183.039.187.039.19.037.194.035.197.035.202.033.204.031.209.03.212.029.216.027.219.025.222.024.226.021.23.02.233.018.236.016.24.015.243.012.246.01.249.008.253.005.256.004.259.001.26-.001.257-.004.254-.005.25-.008.247-.011.244-.012.241-.014.237-.016.233-.018.231-.021.226-.021.224-.024.22-.026.216-.027.212-.028.21-.031.205-.031.202-.034.198-.034.194-.036.191-.037.187-.039.183-.04.179-.04.175-.042.172-.043.168-.044.163-.045.16-.046.155-.046.152-.047.148-.048.143-.049.139-.049.136-.05.131-.05.126-.05.123-.051.118-.052.114-.051.11-.052.106-.052.101-.052.096-.052.092-.052.088-.053.083-.051.079-.052.074-.052.07-.051.065-.051.06-.051.056-.05.051-.05.023-.024.023-.025.021-.024.02-.024.019-.024.018-.024.017-.024.015-.023.014-.024.013-.023.012-.023.01-.023.01-.022.008-.022.006-.022.006-.022.004-.022.004-.021.001-.021.001-.021v-4.127l-.077.055-.08.053-.083.054-.085.053-.087.052-.09.052-.093.051-.095.05-.097.05-.1.049-.102.049-.105.048-.106.047-.109.047-.111.046-.114.045-.115.045-.118.044-.12.043-.122.042-.124.042-.126.041-.128.04-.13.04-.132.038-.134.038-.135.037-.138.037-.139.035-.142.035-.143.034-.144.033-.147.032-.148.031-.15.03-.151.03-.153.029-.154.027-.156.027-.158.026-.159.025-.161.024-.162.023-.163.022-.165.021-.166.02-.167.019-.169.018-.169.017-.171.016-.173.015-.173.014-.175.013-.175.012-.177.011-.178.01-.179.008-.179.008-.181.006-.182.005-.182.004-.184.003-.184.002h-.37l-.184-.002-.184-.003-.182-.004-.182-.005-.181-.006-.179-.008-.179-.008-.178-.01-.176-.011-.176-.012-.175-.013-.173-.014-.172-.015-.171-.016-.17-.017-.169-.018-.167-.019-.166-.02-.165-.021-.163-.022-.162-.023-.161-.024-.159-.025-.157-.026-.156-.027-.155-.027-.153-.029-.151-.03-.15-.03-.148-.031-.146-.032-.145-.033-.143-.034-.141-.035-.14-.035-.137-.037-.136-.037-.134-.038-.132-.038-.13-.04-.128-.04-.126-.041-.124-.042-.122-.042-.12-.044-.117-.043-.116-.045-.113-.045-.112-.046-.109-.047-.106-.047-.105-.048-.102-.049-.1-.049-.097-.05-.095-.05-.093-.052-.09-.051-.087-.052-.085-.053-.083-.054-.08-.054-.077-.054v4.127zm0-5.654v.011l.001.021.003.021.004.021.005.022.006.022.007.022.009.022.01.022.011.023.012.023.013.023.015.024.016.023.017.024.018.024.019.024.021.024.022.024.023.025.024.024.052.05.056.05.061.05.066.051.07.051.075.052.079.051.084.052.088.052.092.052.097.052.102.052.105.052.11.051.114.051.119.052.123.05.127.051.131.05.135.049.139.049.144.048.147.048.152.047.155.046.16.045.163.045.167.044.171.042.176.042.178.04.183.04.187.038.19.037.194.036.197.034.202.033.204.032.209.03.212.028.216.027.219.025.222.024.226.022.23.02.233.018.236.016.24.014.243.012.246.01.249.008.253.006.256.003.259.001.26-.001.257-.003.254-.006.25-.008.247-.01.244-.012.241-.015.237-.016.233-.018.231-.02.226-.022.224-.024.22-.025.216-.027.212-.029.21-.03.205-.032.202-.033.198-.035.194-.036.191-.037.187-.039.183-.039.179-.041.175-.042.172-.043.168-.044.163-.045.16-.045.155-.047.152-.047.148-.048.143-.048.139-.05.136-.049.131-.05.126-.051.123-.051.118-.051.114-.052.11-.052.106-.052.101-.052.096-.052.092-.052.088-.052.083-.052.079-.052.074-.051.07-.052.065-.051.06-.05.056-.051.051-.049.023-.025.023-.024.021-.025.02-.024.019-.024.018-.024.017-.024.015-.023.014-.023.013-.024.012-.022.01-.023.01-.023.008-.022.006-.022.006-.022.004-.021.004-.022.001-.021.001-.021v-4.139l-.077.054-.08.054-.083.054-.085.052-.087.053-.09.051-.093.051-.095.051-.097.05-.1.049-.102.049-.105.048-.106.047-.109.047-.111.046-.114.045-.115.044-.118.044-.12.044-.122.042-.124.042-.126.041-.128.04-.13.039-.132.039-.134.038-.135.037-.138.036-.139.036-.142.035-.143.033-.144.033-.147.033-.148.031-.15.03-.151.03-.153.028-.154.028-.156.027-.158.026-.159.025-.161.024-.162.023-.163.022-.165.021-.166.02-.167.019-.169.018-.169.017-.171.016-.173.015-.173.014-.175.013-.175.012-.177.011-.178.009-.179.009-.179.007-.181.007-.182.005-.182.004-.184.003-.184.002h-.37l-.184-.002-.184-.003-.182-.004-.182-.005-.181-.007-.179-.007-.179-.009-.178-.009-.176-.011-.176-.012-.175-.013-.173-.014-.172-.015-.171-.016-.17-.017-.169-.018-.167-.019-.166-.02-.165-.021-.163-.022-.162-.023-.161-.024-.159-.025-.157-.026-.156-.027-.155-.028-.153-.028-.151-.03-.15-.03-.148-.031-.146-.033-.145-.033-.143-.033-.141-.035-.14-.036-.137-.036-.136-.037-.134-.038-.132-.039-.13-.039-.128-.04-.126-.041-.124-.042-.122-.043-.12-.043-.117-.044-.116-.044-.113-.046-.112-.046-.109-.046-.106-.047-.105-.048-.102-.049-.1-.049-.097-.05-.095-.051-.093-.051-.09-.051-.087-.053-.085-.052-.083-.054-.08-.054-.077-.054v4.139zm0-5.666v.011l.001.02.003.022.004.021.005.022.006.021.007.022.009.023.01.022.011.023.012.023.013.023.015.023.016.024.017.024.018.023.019.024.021.025.022.024.023.024.024.025.052.05.056.05.061.05.066.051.07.051.075.052.079.051.084.052.088.052.092.052.097.052.102.052.105.051.11.052.114.051.119.051.123.051.127.05.131.05.135.05.139.049.144.048.147.048.152.047.155.046.16.045.163.045.167.043.171.043.176.042.178.04.183.04.187.038.19.037.194.036.197.034.202.033.204.032.209.03.212.028.216.027.219.025.222.024.226.021.23.02.233.018.236.017.24.014.243.012.246.01.249.008.253.006.256.003.259.001.26-.001.257-.003.254-.006.25-.008.247-.01.244-.013.241-.014.237-.016.233-.018.231-.02.226-.022.224-.024.22-.025.216-.027.212-.029.21-.03.205-.032.202-.033.198-.035.194-.036.191-.037.187-.039.183-.039.179-.041.175-.042.172-.043.168-.044.163-.045.16-.045.155-.047.152-.047.148-.048.143-.049.139-.049.136-.049.131-.051.126-.05.123-.051.118-.052.114-.051.11-.052.106-.052.101-.052.096-.052.092-.052.088-.052.083-.052.079-.052.074-.052.07-.051.065-.051.06-.051.056-.05.051-.049.023-.025.023-.025.021-.024.02-.024.019-.024.018-.024.017-.024.015-.023.014-.024.013-.023.012-.023.01-.022.01-.023.008-.022.006-.022.006-.022.004-.022.004-.021.001-.021.001-.021v-4.153l-.077.054-.08.054-.083.053-.085.053-.087.053-.09.051-.093.051-.095.051-.097.05-.1.049-.102.048-.105.048-.106.048-.109.046-.111.046-.114.046-.115.044-.118.044-.12.043-.122.043-.124.042-.126.041-.128.04-.13.039-.132.039-.134.038-.135.037-.138.036-.139.036-.142.034-.143.034-.144.033-.147.032-.148.032-.15.03-.151.03-.153.028-.154.028-.156.027-.158.026-.159.024-.161.024-.162.023-.163.023-.165.021-.166.02-.167.019-.169.018-.169.017-.171.016-.173.015-.173.014-.175.013-.175.012-.177.01-.178.01-.179.009-.179.007-.181.006-.182.006-.182.004-.184.003-.184.001-.185.001-.185-.001-.184-.001-.184-.003-.182-.004-.182-.006-.181-.006-.179-.007-.179-.009-.178-.01-.176-.01-.176-.012-.175-.013-.173-.014-.172-.015-.171-.016-.17-.017-.169-.018-.167-.019-.166-.02-.165-.021-.163-.023-.162-.023-.161-.024-.159-.024-.157-.026-.156-.027-.155-.028-.153-.028-.151-.03-.15-.03-.148-.032-.146-.032-.145-.033-.143-.034-.141-.034-.14-.036-.137-.036-.136-.037-.134-.038-.132-.039-.13-.039-.128-.041-.126-.041-.124-.041-.122-.043-.12-.043-.117-.044-.116-.044-.113-.046-.112-.046-.109-.046-.106-.048-.105-.048-.102-.048-.1-.05-.097-.049-.095-.051-.093-.051-.09-.052-.087-.052-.085-.053-.083-.053-.08-.054-.077-.054v4.153zm8.74-8.179l-.257.004-.254.005-.25.008-.247.011-.244.012-.241.014-.237.016-.233.018-.231.021-.226.022-.224.023-.22.026-.216.027-.212.028-.21.031-.205.032-.202.033-.198.034-.194.036-.191.038-.187.038-.183.04-.179.041-.175.042-.172.043-.168.043-.163.045-.16.046-.155.046-.152.048-.148.048-.143.048-.139.049-.136.05-.131.05-.126.051-.123.051-.118.051-.114.052-.11.052-.106.052-.101.052-.096.052-.092.052-.088.052-.083.052-.079.052-.074.051-.07.052-.065.051-.06.05-.056.05-.051.05-.023.025-.023.024-.021.024-.02.025-.019.024-.018.024-.017.023-.015.024-.014.023-.013.023-.012.023-.01.023-.01.022-.008.022-.006.023-.006.021-.004.022-.004.021-.001.021-.001.021.001.021.001.021.004.021.004.022.006.021.006.023.008.022.01.022.01.023.012.023.013.023.014.023.015.024.017.023.018.024.019.024.02.025.021.024.023.024.023.025.051.05.056.05.06.05.065.051.07.052.074.051.079.052.083.052.088.052.092.052.096.052.101.052.106.052.11.052.114.052.118.051.123.051.126.051.131.05.136.05.139.049.143.048.148.048.152.048.155.046.16.046.163.045.168.043.172.043.175.042.179.041.183.04.187.038.191.038.194.036.198.034.202.033.205.032.21.031.212.028.216.027.22.026.224.023.226.022.231.021.233.018.237.016.241.014.244.012.247.011.25.008.254.005.257.004.26.001.26-.001.257-.004.254-.005.25-.008.247-.011.244-.012.241-.014.237-.016.233-.018.231-.021.226-.022.224-.023.22-.026.216-.027.212-.028.21-.031.205-.032.202-.033.198-.034.194-.036.191-.038.187-.038.183-.04.179-.041.175-.042.172-.043.168-.043.163-.045.16-.046.155-.046.152-.048.148-.048.143-.048.139-.049.136-.05.131-.05.126-.051.123-.051.118-.051.114-.052.11-.052.106-.052.101-.052.096-.052.092-.052.088-.052.083-.052.079-.052.074-.051.07-.052.065-.051.06-.05.056-.05.051-.05.023-.025.023-.024.021-.024.02-.025.019-.024.018-.024.017-.023.015-.024.014-.023.013-.023.012-.023.01-.023.01-.022.008-.022.006-.023.006-.021.004-.022.004-.021.001-.021.001-.021-.001-.021-.001-.021-.004-.021-.004-.022-.006-.021-.006-.023-.008-.022-.01-.022-.01-.023-.012-.023-.013-.023-.014-.023-.015-.024-.017-.023-.018-.024-.019-.024-.02-.025-.021-.024-.023-.024-.023-.025-.051-.05-.056-.05-.06-.05-.065-.051-.07-.052-.074-.051-.079-.052-.083-.052-.088-.052-.092-.052-.096-.052-.101-.052-.106-.052-.11-.052-.114-.052-.118-.051-.123-.051-.126-.051-.131-.05-.136-.05-.139-.049-.143-.048-.148-.048-.152-.048-.155-.046-.16-.046-.163-.045-.168-.043-.172-.043-.175-.042-.179-.041-.183-.04-.187-.038-.191-.038-.194-.036-.198-.034-.202-.033-.205-.032-.21-.031-.212-.028-.216-.027-.22-.026-.224-.023-.226-.022-.231-.021-.233-.018-.237-.016-.241-.014-.244-.012-.247-.011-.25-.008-.254-.005-.257-.004-.26-.001-.26.001z%22%3E%3C%2Fpath%3E%3C%2Fsymbol%3E%3C%2Fdefs%3E%3Cdefs%3E%3Csymbol%20id%3D%22clock%22%20width%3D%2224%22%20height%3D%2224%22%3E%3Cpath%20transform%3D%22scale\(.5\)%22%20d%3D%22M12%202c5.514%200%2010%204.486%2010%2010s-4.486%2010-10%2010-10-4.486-10-10%204.486-10%2010-10zm0-2c-6.627%200-12%205.373-12%2012s5.373%2012%2012%2012%2012-5.373%2012-12-5.373-12-12-12zm5.848%2012.459c.202.038.202.333.001.372-1.907.361-6.045%201.111-6.547%201.111-.719%200-1.301-.582-1.301-1.301%200-.512.77-5.447%201.125-7.445.034-.192.312-.181.343.014l.985%206.238%205.394%201.011z%22%3E%3C%2Fpath%3E%3C%2Fsymbol%3E%3C%2Fdefs%3E%3Cdefs%3E%3Cmarker%20id%3D%22arrowhead%22%20refX%3D%227.9%22%20refY%3D%225%22%20markerUnits%3D%22userSpaceOnUse%22%20markerWidth%3D%2212%22%20markerHeight%3D%2212%22%20orient%3D%22auto-start-reverse%22%3E%3Cpath%20d%3D%22M%20-1%200%20L%2010%205%20L%200%2010%20z%22%3E%3C%2Fpath%3E%3C%2Fmarker%3E%3C%2Fdefs%3E%3Cdefs%3E%3Cmarker%20id%3D%22crosshead%22%20markerWidth%3D%2215%22%20markerHeight%3D%228%22%20orient%3D%22auto%22%20refX%3D%224%22%20refY%3D%224.5%22%3E%3Cpath%20fill%3D%22none%22%20stroke%3D%22%23000000%22%20stroke-width%3D%221pt%22%20d%3D%22M%201%2C2%20L%206%2C7%20M%206%2C2%20L%201%2C7%22%20style%3D%22stroke-dasharray%3A%200%2C%200%3B%22%3E%3C%2Fpath%3E%3C%2Fmarker%3E%3C%2Fdefs%3E%3Cdefs%3E%3Cmarker%20id%3D%22filled-head%22%20refX%3D%2215.5%22%20refY%3D%227%22%20markerWidth%3D%2220%22%20markerHeight%3D%2228%22%20orient%3D%22auto%22%3E%3Cpath%20d%3D%22M%2018%2C7%20L9%2C13%20L14%2C7%20L9%2C1%20Z%22%3E%3C%2Fpath%3E%3C%2Fmarker%3E%3C%2Fdefs%3E%3Cdefs%3E%3Cmarker%20id%3D%22sequencenumber%22%20refX%3D%2215%22%20refY%3D%2215%22%20markerWidth%3D%2260%22%20markerHeight%3D%2240%22%20orient%3D%22auto%22%3E%3Ccircle%20cx%3D%2215%22%20cy%3D%2215%22%20r%3D%226%22%3E%3C%2Fcircle%3E%3C%2Fmarker%3E%3C%2Fdefs%3E%3Cg%20class%3D%22actor-man%20actor-top%22%20name%3D%22E%22%3E%3Cline%20id%3D%22actor-man-torso0%22%20x1%3D%2275%22%20y1%3D%2225%22%20x2%3D%2275%22%20y2%3D%2245%22%3E%3C%2Fline%3E%3Cline%20id%3D%22actor-man-arms0%22%20x1%3D%2257%22%20y1%3D%2233%22%20x2%3D%2293%22%20y2%3D%2233%22%3E%3C%2Fline%3E%3Cline%20x1%3D%2257%22%20y1%3D%2260%22%20x2%3D%2275%22%20y2%3D%2245%22%3E%3C%2Fline%3E%3Cline%20x1%3D%2275%22%20y1%3D%2245%22%20x2%3D%2291%22%20y2%3D%2260%22%3E%3C%2Fline%3E%3Ccircle%20cx%3D%2275%22%20cy%3D%2210%22%20r%3D%2215%22%20width%3D%22150%22%20height%3D%2265%22%3E%3C%2Fcircle%3E%3Ctext%20x%3D%2275%22%20y%3D%2267.5%22%20dominant-baseline%3D%22central%22%20alignment-baseline%3D%22central%22%20class%3D%22actor%20actor-man%22%20style%3D%22text-anchor%3A%20middle%3B%20font-size%3A%2016px%3B%20font-weight%3A%20400%3B%20font-family%3A%20-apple-system%2C%20BlinkMacSystemFont%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Roboto%2C%20Oxygen%2C%20Ubuntu%2C%20Cantarell%2C%20%26quot%3BHelvetica%20Neue%26quot%3B%2C%20Arial%2C%20%26quot%3Bsans-serif%26quot%3B%3B%22%3E%3Ctspan%20x%3D%2275%22%20dy%3D%220%22%3EEmployee%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3Ctext%20x%3D%22233%22%20y%3D%2280%22%20text-anchor%3D%22middle%22%20dominant-baseline%3D%22middle%22%20alignment-baseline%3D%22middle%22%20class%3D%22messageText%22%20dy%3D%221em%22%20style%3D%22font-family%3A%20-apple-system%2C%20BlinkMacSystemFont%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Roboto%2C%20Oxygen%2C%20Ubuntu%2C%20Cantarell%2C%20%26quot%3BHelvetica%20Neue%26quot%3B%2C%20Arial%2C%20%26quot%3Bsans-serif%26quot%3B%3B%20font-size%3A%2016px%3B%20font-weight%3A%20400%3B%22%3ESubmit%20purchase%20form%3C%2Ftext%3E%3Cline%20x1%3D%2276%22%20y1%3D%22117%22%20x2%3D%22390%22%20y2%3D%22117%22%20class%3D%22messageLine0%22%20stroke-width%3D%222%22%20stroke%3D%22none%22%20marker-end%3D%22url\(%23arrowhead\)%22%20style%3D%22fill%3A%20none%3B%22%3E%3C%2Fline%3E%3Ctext%20x%3D%22508%22%20y%3D%22132%22%20text-anchor%3D%22middle%22%20dominant-baseline%3D%22middle%22%20alignment-baseline%3D%22middle%22%20class%3D%22messageText%22%20dy%3D%221em%22%20style%3D%22font-family%3A%20-apple-system%2C%20BlinkMacSystemFont%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Roboto%2C%20Oxygen%2C%20Ubuntu%2C%20Cantarell%2C%20%26quot%3BHelvetica%20Neue%26quot%3B%2C%20Arial%2C%20%26quot%3Bsans-serif%26quot%3B%3B%20font-size%3A%2016px%3B%20font-weight%3A%20400%3B%22%3EPOST%20%2Fapi%2Ftransactions%3C%2Ftext%3E%3Cline%20x1%3D%22395%22%20y1%3D%22169%22%20x2%3D%22621%22%20y2%3D%22169%22%20class%3D%22messageLine0%22%20stroke-width%3D%222%22%20stroke%3D%22none%22%20marker-end%3D%22url\(%23arrowhead\)%22%20style%3D%22fill%3A%20none%3B%22%3E%3C%2Fline%3E%3Ctext%20x%3D%22626%22%20y%3D%22184%22%20text-anchor%3D%22middle%22%20dominant-baseline%3D%22middle%22%20alignment-baseline%3D%22middle%22%20class%3D%22messageText%22%20dy%3D%221em%22%20style%3D%22font-family%3A%20-apple-system%2C%20BlinkMacSystemFont%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Roboto%2C%20Oxygen%2C%20Ubuntu%2C%20Cantarell%2C%20%26quot%3BHelvetica%20Neue%26quot%3B%2C%20Arial%2C%20%26quot%3Bsans-serif%26quot%3B%3B%20font-size%3A%2016px%3B%20font-weight%3A%20400%3B%22%3EVerify%20session%3C%2Ftext%3E%3Cpath%20d%3D%22M%20626%2C221%20C%20686%2C211%20686%2C251%20626%2C241%22%20class%3D%22messageLine0%22%20stroke-width%3D%222%22%20stroke%3D%22none%22%20marker-end%3D%22url\(%23arrowhead\)%22%20style%3D%22fill%3A%20none%3B%22%3E%3C%2Fpath%3E%3Ctext%20x%3D%22776%22%20y%3D%22266%22%20text-anchor%3D%22middle%22%20dominant-baseline%3D%22middle%22%20alignment-baseline%3D%22middle%22%20class%3D%22messageText%22%20dy%3D%221em%22%20style%3D%22font-family%3A%20-apple-system%2C%20BlinkMacSystemFont%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Roboto%2C%20Oxygen%2C%20Ubuntu%2C%20Cantarell%2C%20%26quot%3BHelvetica%20Neue%26quot%3B%2C%20Arial%2C%20%26quot%3Bsans-serif%26quot%3B%3B%20font-size%3A%2016px%3B%20font-weight%3A%20400%3B%22%3EValidate%20request%20and%20permissions%3C%2Ftext%3E%3Cline%20x1%3D%22626%22%20y1%3D%22303%22%20x2%3D%22925%22%20y2%3D%22303%22%20class%3D%22messageLine0%22%20stroke-width%3D%222%22%20stroke%3D%22none%22%20marker-end%3D%22url\(%23arrowhead\)%22%20style%3D%22fill%3A%20none%3B%22%3E%3C%2Fline%3E%3Ctext%20x%3D%221085%22%20y%3D%22318%22%20text-anchor%3D%22middle%22%20dominant-baseline%3D%22middle%22%20alignment-baseline%3D%22middle%22%20class%3D%22messageText%22%20dy%3D%221em%22%20style%3D%22font-family%3A%20-apple-system%2C%20BlinkMacSystemFont%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Roboto%2C%20Oxygen%2C%20Ubuntu%2C%20Cantarell%2C%20%26quot%3BHelvetica%20Neue%26quot%3B%2C%20Arial%2C%20%26quot%3Bsans-serif%26quot%3B%3B%20font-size%3A%2016px%3B%20font-weight%3A%20400%3B%22%3EVerify%20business%20and%20customer%3C%2Ftext%3E%3Cline%20x1%3D%22930%22%20y1%3D%22355%22%20x2%3D%221239%22%20y2%3D%22355%22%20class%3D%22messageLine0%22%20stroke-width%3D%222%22%20stroke%3D%22none%22%20marker-end%3D%22url\(%23arrowhead\)%22%20style%3D%22fill%3A%20none%3B%22%3E%3C%2Fline%3E%3Ctext%20x%3D%221088%22%20y%3D%22370%22%20text-anchor%3D%22middle%22%20dominant-baseline%3D%22middle%22%20alignment-baseline%3D%22middle%22%20class%3D%22messageText%22%20dy%3D%221em%22%20style%3D%22font-family%3A%20-apple-system%2C%20BlinkMacSystemFont%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Roboto%2C%20Oxygen%2C%20Ubuntu%2C%20Cantarell%2C%20%26quot%3BHelvetica%20Neue%26quot%3B%2C%20Arial%2C%20%26quot%3Bsans-serif%26quot%3B%3B%20font-size%3A%2016px%3B%20font-weight%3A%20400%3B%22%3ECustomer%20is%20valid%3C%2Ftext%3E%3Cline%20x1%3D%221242%22%20y1%3D%22407%22%20x2%3D%22933%22%20y2%3D%22407%22%20class%3D%22messageLine1%22%20stroke-width%3D%222%22%20stroke%3D%22none%22%20marker-end%3D%22url\(%23arrowhead\)%22%20style%3D%22stroke-dasharray%3A%203%2C%203%3B%20fill%3A%20none%3B%22%3E%3C%2Fline%3E%3Ctext%20x%3D%22930%22%20y%3D%22422%22%20text-anchor%3D%22middle%22%20dominant-baseline%3D%22middle%22%20alignment-baseline%3D%22middle%22%20class%3D%22messageText%22%20dy%3D%221em%22%20style%3D%22font-family%3A%20-apple-system%2C%20BlinkMacSystemFont%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Roboto%2C%20Oxygen%2C%20Ubuntu%2C%20Cantarell%2C%20%26quot%3BHelvetica%20Neue%26quot%3B%2C%20Arial%2C%20%26quot%3Bsans-serif%26quot%3B%3B%20font-size%3A%2016px%3B%20font-weight%3A%20400%3B%22%3ECalculate%20points%3C%2Ftext%3E%3Cpath%20d%3D%22M%20930%2C459%20C%20990%2C449%20990%2C489%20930%2C479%22%20class%3D%22messageLine0%22%20stroke-width%3D%222%22%20stroke%3D%22none%22%20marker-end%3D%22url\(%23arrowhead\)%22%20style%3D%22fill%3A%20none%3B%22%3E%3C%2Fpath%3E%3Ctext%20x%3D%221085%22%20y%3D%22504%22%20text-anchor%3D%22middle%22%20dominant-baseline%3D%22middle%22%20alignment-baseline%3D%22middle%22%20class%3D%22messageText%22%20dy%3D%221em%22%20style%3D%22font-family%3A%20-apple-system%2C%20BlinkMacSystemFont%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Roboto%2C%20Oxygen%2C%20Ubuntu%2C%20Cantarell%2C%20%26quot%3BHelvetica%20Neue%26quot%3B%2C%20Arial%2C%20%26quot%3Bsans-serif%26quot%3B%3B%20font-size%3A%2016px%3B%20font-weight%3A%20400%3B%22%3ESave%20transaction%20and%20points%20ledger%3C%2Ftext%3E%3Cline%20x1%3D%22930%22%20y1%3D%22541%22%20x2%3D%221239%22%20y2%3D%22541%22%20class%3D%22messageLine0%22%20stroke-width%3D%222%22%20stroke%3D%22none%22%20marker-end%3D%22url\(%23arrowhead\)%22%20style%3D%22fill%3A%20none%3B%22%3E%3C%2Fline%3E%3Ctext%20x%3D%221088%22%20y%3D%22556%22%20text-anchor%3D%22middle%22%20dominant-baseline%3D%22middle%22%20alignment-baseline%3D%22middle%22%20class%3D%22messageText%22%20dy%3D%221em%22%20style%3D%22font-family%3A%20-apple-system%2C%20BlinkMacSystemFont%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Roboto%2C%20Oxygen%2C%20Ubuntu%2C%20Cantarell%2C%20%26quot%3BHelvetica%20Neue%26quot%3B%2C%20Arial%2C%20%26quot%3Bsans-serif%26quot%3B%3B%20font-size%3A%2016px%3B%20font-weight%3A%20400%3B%22%3ECommit%20successful%3C%2Ftext%3E%3Cline%20x1%3D%221242%22%20y1%3D%22593%22%20x2%3D%22933%22%20y2%3D%22593%22%20class%3D%22messageLine1%22%20stroke-width%3D%222%22%20stroke%3D%22none%22%20marker-end%3D%22url\(%23arrowhead\)%22%20style%3D%22stroke-dasharray%3A%203%2C%203%3B%20fill%3A%20none%3B%22%3E%3C%2Fline%3E%3Ctext%20x%3D%22779%22%20y%3D%22608%22%20text-anchor%3D%22middle%22%20dominant-baseline%3D%22middle%22%20alignment-baseline%3D%22middle%22%20class%3D%22messageText%22%20dy%3D%221em%22%20style%3D%22font-family%3A%20-apple-system%2C%20BlinkMacSystemFont%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Roboto%2C%20Oxygen%2C%20Ubuntu%2C%20Cantarell%2C%20%26quot%3BHelvetica%20Neue%26quot%3B%2C%20Arial%2C%20%26quot%3Bsans-serif%26quot%3B%3B%20font-size%3A%2016px%3B%20font-weight%3A%20400%3B%22%3ECreated%20transaction%3C%2Ftext%3E%3Cline%20x1%3D%22928%22%20y1%3D%22645%22%20x2%3D%22629%22%20y2%3D%22645%22%20class%3D%22messageLine1%22%20stroke-width%3D%222%22%20stroke%3D%22none%22%20marker-end%3D%22url\(%23arrowhead\)%22%20style%3D%22stroke-dasharray%3A%203%2C%203%3B%20fill%3A%20none%3B%22%3E%3C%2Fline%3E%3Ctext%20x%3D%22511%22%20y%3D%22660%22%20text-anchor%3D%22middle%22%20dominant-baseline%3D%22middle%22%20alignment-baseline%3D%22middle%22%20class%3D%22messageText%22%20dy%3D%221em%22%20style%3D%22font-family%3A%20-apple-system%2C%20BlinkMacSystemFont%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Roboto%2C%20Oxygen%2C%20Ubuntu%2C%20Cantarell%2C%20%26quot%3BHelvetica%20Neue%26quot%3B%2C%20Arial%2C%20%26quot%3Bsans-serif%26quot%3B%3B%20font-size%3A%2016px%3B%20font-weight%3A%20400%3B%22%3E201%20Created%20%2B%20JSON%3C%2Ftext%3E%3Cline%20x1%3D%22624%22%20y1%3D%22697%22%20x2%3D%22398%22%20y2%3D%22697%22%20class%3D%22messageLine1%22%20stroke-width%3D%222%22%20stroke%3D%22none%22%20marker-end%3D%22url\(%23arrowhead\)%22%20style%3D%22stroke-dasharray%3A%203%2C%203%3B%20fill%3A%20none%3B%22%3E%3C%2Fline%3E%3Ctext%20x%3D%22236%22%20y%3D%22712%22%20text-anchor%3D%22middle%22%20dominant-baseline%3D%22middle%22%20alignment-baseline%3D%22middle%22%20class%3D%22messageText%22%20dy%3D%221em%22%20style%3D%22font-family%3A%20-apple-system%2C%20BlinkMacSystemFont%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Roboto%2C%20Oxygen%2C%20Ubuntu%2C%20Cantarell%2C%20%26quot%3BHelvetica%20Neue%26quot%3B%2C%20Arial%2C%20%26quot%3Bsans-serif%26quot%3B%3B%20font-size%3A%2016px%3B%20font-weight%3A%20400%3B%22%3EShow%20success%20and%20updated%20balance%3C%2Ftext%3E%3Cline%20x1%3D%22393%22%20y1%3D%22749%22%20x2%3D%2279%22%20y2%3D%22749%22%20class%3D%22messageLine1%22%20stroke-width%3D%222%22%20stroke%3D%22none%22%20marker-end%3D%22url\(%23arrowhead\)%22%20style%3D%22stroke-dasharray%3A%203%2C%203%3B%20fill%3A%20none%3B%22%3E%3C%2Fline%3E%3Cg%20class%3D%22actor-man%20actor-bottom%22%20name%3D%22E%22%3E%3Cline%20id%3D%22actor-man-torso4%22%20x1%3D%2275%22%20y1%3D%22794%22%20x2%3D%2275%22%20y2%3D%22814%22%3E%3C%2Fline%3E%3Cline%20id%3D%22actor-man-arms4%22%20x1%3D%2257%22%20y1%3D%22802%22%20x2%3D%2293%22%20y2%3D%22802%22%3E%3C%2Fline%3E%3Cline%20x1%3D%2257%22%20y1%3D%22829%22%20x2%3D%2275%22%20y2%3D%22814%22%3E%3C%2Fline%3E%3Cline%20x1%3D%2275%22%20y1%3D%22814%22%20x2%3D%2291%22%20y2%3D%22829%22%3E%3C%2Fline%3E%3Ccircle%20cx%3D%2275%22%20cy%3D%22779%22%20r%3D%2215%22%20width%3D%22150%22%20height%3D%2265%22%3E%3C%2Fcircle%3E%3Ctext%20x%3D%2275%22%20y%3D%22836.5%22%20dominant-baseline%3D%22central%22%20alignment-baseline%3D%22central%22%20class%3D%22actor%20actor-man%22%20style%3D%22text-anchor%3A%20middle%3B%20font-size%3A%2016px%3B%20font-weight%3A%20400%3B%20font-family%3A%20-apple-system%2C%20BlinkMacSystemFont%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Roboto%2C%20Oxygen%2C%20Ubuntu%2C%20Cantarell%2C%20%26quot%3BHelvetica%20Neue%26quot%3B%2C%20Arial%2C%20%26quot%3Bsans-serif%26quot%3B%3B%22%3E%3Ctspan%20x%3D%2275%22%20dy%3D%220%22%3EEmployee%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fsvg%3E)

If validation or authorization fails, the server stops the workflow before making unauthorized changes. If the database operation fails, the transaction must roll back so that the purchase and associated points do not become inconsistent.

This illustrates the main engineering principle: the frontend requests an action; the backend decides whether it is valid; the database stores the result.

### 5.1 Frontend responsibilities

The frontend is responsible for presenting information and collecting user input.

For example, the transaction form:

* Displays customers the user is permitted to select.

* Accepts an amount and service selection.

* Performs basic validation to help the user.

* Submits the request to the API.

* Displays loading, success, and failure states.

* Refreshes transaction history after success.

The frontend must not be trusted to enforce security or calculate authoritative loyalty points. A user can manipulate browser code or send a request without using your UI.

### 5.2 Backend responsibilities

The backend is responsible for enforcing rules that must remain correct regardless of how a request is sent.

Every sensitive endpoint should:

1. Parse and validate the request.

2. Authenticate the caller where required.

3. Verify permissions and resource ownership.

4. Execute the relevant business rules.

5. Perform database operations safely.

6. Return an appropriate response.

You should keep complicated business logic in reusable server-side functions or service modules instead of duplicating it across multiple route handlers.

### 5.3 Database responsibilities

PostgreSQL is the source of truth for persisted business data.

It should enforce appropriate constraints, including:

* Primary keys and foreign keys.

* Required fields and valid data types.

* Unique constraints where needed.

* Valid numeric ranges.

* Relationships between users, businesses, customers, and transactions.

Prisma provides a convenient way to query the database and manage migrations, but it does not replace the need to understand SQL, constraints, or transactions.

## 6. Database design: What information the system stores

A reasonable starting schema is below. It is a conceptual design, not the final Prisma schema.

### User

id, name, email, passwordHash, createdAt

Represents a person who can authenticate.

### Business

id, name, timezone, currency, createdAt

Represents an independent business account.

### Membership

id, userId, businessId, role, status

Connects a user to a business and defines their role.

### Customer

id, businessId, name, phone, email, archivedAt, createdAt

Belongs to one business.

### Transaction

id, businessId, customerId, createdByMembershipId, amount, status, createdAt

Records a purchase or service visit. Store currency amounts precisely, using an appropriate decimal type or integer minor units.

### Reward

id, businessId, name, pointsCost, isActive

Defines a reward offered by a business.

### Redemption

id, businessId, customerId, rewardId, createdByMembershipId, pointsSpent, status, createdAt

Records a reward redemption and preserves the points cost at redemption time.

### PointsLedger

id, businessId, customerId, transactionId, redemptionId, pointsDelta, createdAt

Records points earned or spent so balances can be traced and reconciled.

### How the tables relate

Diagram options

![](data\:image/svg+xml;utf8,%3Csvg%20id%3D%22mermaid-_r_p2_%22%20width%3D%22548.8909912109375%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20class%3D%22erDiagram%22%20height%3D%22681%22%20viewBox%3D%224%204%20548.8909912109375%20681%22%20role%3D%22graphics-document%20document%22%20aria-roledescription%3D%22er%22%3E%3Cg%20class%3D%22edges%20edgePaths%22%3E%3Cpath%20d%3D%22M62%2C91L62%2C187.21704397470535Q62%2C189%2063.08578643762691%2C190.4142135623731L63.08578643762691%2C190.4142135623731Q64.17157287525382%2C191.82842712474618%2065.58578643762691%2C192.9142135623731L65.58578643762691%2C192.9142135623731Q67%2C194%2068.78295602529465%2C194L73.16007782887203%2C194Q74.94303385416669%2C194%2076.35724741653978%2C195.0857864376269L76.35724741653978%2C195.0857864376269Q77.77146097891287%2C196.17157287525382%2078.85724741653978%2C197.5857864376269L78.85724741653978%2C197.5857864376269Q79.94303385416669%2C199%2079.94303385416669%2C200.78295602529465L79.94303385416669%2C214%22%20id%3D%22id_entity-USER-0_entity-MEMBERSHIP-1_0%22%20class%3D%22edge-thickness-normal%20edge-pattern-solid%20relationshipLine%22%20style%3D%22undefined%3B%3B%3Bundefined%22%20data-edge%3D%22true%22%20data-et%3D%22edge%22%20data-id%3D%22id_entity-USER-0_entity-MEMBERSHIP-1_0%22%20data-points%3D%22W3sieCI6NjIsInkiOjkxfSx7IngiOjYyLCJ5IjoxOTR9LHsieCI6NzkuOTQzMDMzODU0MTY2NjksInkiOjE5NH0seyJ4Ijo3OS45NDMwMzM4NTQxNjY2OSwieSI6MjE0fV0%3D%22%20marker-start%3D%22url\(%23mermaid-_r_p2__er-onlyOneStart\)%22%20marker-end%3D%22url\(%23mermaid-_r_p2__er-zeroOrMoreEnd\)%22%3E%3C%2Fpath%3E%3Cpath%20d%3D%22M313.19173177083337%2C91L313.19173177083337%2C104.21704397470535Q313.19173177083337%2C106%20312.1059453332065%2C107.41421356237309L312.1059453332065%2C107.41421356237309Q311.0201588955796%2C108.82842712474618%20309.6059453332065%2C109.91421356237308L309.6059453332065%2C109.91421356237309Q308.19173177083337%2C111%20306.4087757455387%2C111L128.61696624936548%2C111Q121.5458984375%2C111%20121.5458984375%2C118.07106781186548L121.54589843750003%2C214%22%20id%3D%22id_entity-BUSINESS-2_entity-MEMBERSHIP-1_1%22%20class%3D%22edge-thickness-normal%20edge-pattern-solid%20relationshipLine%22%20style%3D%22undefined%3B%3B%3Bundefined%22%20data-edge%3D%22true%22%20data-et%3D%22edge%22%20data-id%3D%22id_entity-BUSINESS-2_entity-MEMBERSHIP-1_1%22%20data-points%3D%22W3sieCI6MzEzLjE5MTczMTc3MDgzMzM3LCJ5Ijo5MX0seyJ4IjozMTMuMTkxNzMxNzcwODMzMzcsInkiOjExMX0seyJ4IjoxMjEuNTQ1ODk4NDM3NSwieSI6MTExfSx7IngiOjEyMS41NDU4OTg0Mzc1MDAwMywieSI6MjE0fV0%3D%22%20marker-start%3D%22url\(%23mermaid-_r_p2__er-onlyOneStart\)%22%20marker-end%3D%22url\(%23mermaid-_r_p2__er-zeroOrMoreEnd\)%22%3E%3C%2Fpath%3E%3Cpath%20d%3D%22M353.69173177083337%2C91L353.69173177083337%2C214%22%20id%3D%22id_entity-BUSINESS-2_entity-CUSTOMER-3_2%22%20class%3D%22edge-thickness-normal%20edge-pattern-solid%20relationshipLine%22%20style%3D%22undefined%3B%3B%3Bundefined%22%20data-edge%3D%22true%22%20data-et%3D%22edge%22%20data-id%3D%22id_entity-BUSINESS-2_entity-CUSTOMER-3_2%22%20data-points%3D%22W3sieCI6MzUzLjY5MTczMTc3MDgzMzM3LCJ5Ijo5MX0seyJ4IjozNTMuNjkxNzMxNzcwODMzMzcsInkiOjIxNH1d%22%20marker-start%3D%22url\(%23mermaid-_r_p2__er-onlyOneStart\)%22%20marker-end%3D%22url\(%23mermaid-_r_p2__er-zeroOrMoreEnd\)%22%3E%3C%2Fpath%3E%3Cpath%20d%3D%22M333.44173177083337%2C91L333.44173177083337%2C124.21704397470535Q333.44173177083337%2C126%20332.3559453332065%2C127.41421356237309L332.3559453332065%2C127.41421356237309Q331.2701588955796%2C128.82842712474618%20329.8559453332065%2C129.91421356237308L329.8559453332065%2C129.9142135623731Q328.44173177083337%2C131%20326.6587757455387%2C131L234.931719046128%2C131Q233.14876302083334%2C131%20231.73454945846026%2C132.0857864376269L231.73454945846026%2C132.0857864376269Q230.32033589608716%2C133.17157287525382%20229.23454945846026%2C134.5857864376269L229.23454945846026%2C134.5857864376269Q228.14876302083334%2C136%20228.14876302083334%2C137.78295602529465L228.14876302083334%2C162.5L228.14876302083334%2C344.5L228.14876302083334%2C375.521484375Q228.14876302083334%2C376%20228.62727864583334%2C376L228.62727864583334%2C376Q229.10579427083334%2C376%20229.10579427083334%2C376.478515625L229.10579427083334%2C396%22%20id%3D%22id_entity-BUSINESS-2_entity-TRANSACTION-4_3%22%20class%3D%22edge-thickness-normal%20edge-pattern-solid%20relationshipLine%22%20style%3D%22undefined%3B%3B%3Bundefined%22%20data-edge%3D%22true%22%20data-et%3D%22edge%22%20data-id%3D%22id_entity-BUSINESS-2_entity-TRANSACTION-4_3%22%20data-points%3D%22W3sieCI6MzMzLjQ0MTczMTc3MDgzMzM3LCJ5Ijo5MX0seyJ4IjozMzMuNDQxNzMxNzcwODMzMzcsInkiOjEzMX0seyJ4IjoyMjguMTQ4NzYzMDIwODMzMzQsInkiOjEzMX0seyJ4IjoyMjguMTQ4NzYzMDIwODMzMzQsInkiOjE2Mi41fSx7IngiOjIyOC4xNDg3NjMwMjA4MzMzNCwieSI6MzQ0LjV9LHsieCI6MjI4LjE0ODc2MzAyMDgzMzM0LCJ5IjozNzZ9LHsieCI6MjI5LjEwNTc5NDI3MDgzMzM0LCJ5IjozNzZ9LHsieCI6MjI5LjEwNTc5NDI3MDgzMzM0LCJ5IjozOTZ9XQ%3D%3D%22%20marker-start%3D%22url\(%23mermaid-_r_p2__er-onlyOneStart\)%22%20marker-end%3D%22url\(%23mermaid-_r_p2__er-zeroOrMoreEnd\)%22%3E%3C%2Fpath%3E%3Cpath%20d%3D%22M325.7874348958333%2C293L325.78743489583337%2C305.9289321881345Q325.78743489583337%2C313%20318.7163670839679%2C313L268.931719046128%2C313Q267.14876302083337%2C313%20265.73454945846026%2C314.0857864376269L265.73454945846026%2C314.0857864376269Q264.32033589608716%2C315.1715728752538%20263.23454945846026%2C316.5857864376269L263.23454945846026%2C316.5857864376269Q262.14876302083337%2C318%20262.14876302083337%2C319.78295602529465L262.14876302083337%2C396%22%20id%3D%22id_entity-CUSTOMER-3_entity-TRANSACTION-4_4%22%20class%3D%22edge-thickness-normal%20edge-pattern-solid%20relationshipLine%22%20style%3D%22undefined%3B%3B%3Bundefined%22%20data-edge%3D%22true%22%20data-et%3D%22edge%22%20data-id%3D%22id_entity-CUSTOMER-3_entity-TRANSACTION-4_4%22%20data-points%3D%22W3sieCI6MzI1Ljc4NzQzNDg5NTgzMzMsInkiOjI5M30seyJ4IjozMjUuNzg3NDM0ODk1ODMzMzcsInkiOjMxM30seyJ4IjoyNjIuMTQ4NzYzMDIwODMzMzcsInkiOjMxM30seyJ4IjoyNjIuMTQ4NzYzMDIwODMzMzcsInkiOjM5Nn1d%22%20marker-start%3D%22url\(%23mermaid-_r_p2__er-onlyOneStart\)%22%20marker-end%3D%22url\(%23mermaid-_r_p2__er-zeroOrMoreEnd\)%22%3E%3C%2Fpath%3E%3Cpath%20d%3D%22M100.74446614583337%2C293L100.74446614583337%2C369.21704397470535Q100.74446614583337%2C371%20101.83025258346028%2C372.4142135623731L101.8302525834603%2C372.4142135623731Q102.91603902108719%2C373.8284271247462%20104.33025258346028%2C374.9142135623731L104.33025258346028%2C374.9142135623731Q105.74446614583337%2C376%20107.52742217112802%2C376L189.2798694955387%2C376Q191.06282552083334%2C376%20192.47703908320642%2C377.0857864376269L192.47703908320642%2C377.0857864376269Q193.89125264557953%2C378.1715728752538%20194.97703908320642%2C379.5857864376269L194.97703908320642%2C379.5857864376269Q196.06282552083334%2C381%20196.06282552083334%2C382.78295602529465L196.06282552083334%2C396%22%20id%3D%22id_entity-MEMBERSHIP-1_entity-TRANSACTION-4_5%22%20class%3D%22edge-thickness-normal%20edge-pattern-solid%20relationshipLine%22%20style%3D%22undefined%3B%3B%3Bundefined%22%20data-edge%3D%22true%22%20data-et%3D%22edge%22%20data-id%3D%22id_entity-MEMBERSHIP-1_entity-TRANSACTION-4_5%22%20data-points%3D%22W3sieCI6MTAwLjc0NDQ2NjE0NTgzMzM3LCJ5IjoyOTN9LHsieCI6MTAwLjc0NDQ2NjE0NTgzMzM3LCJ5IjozNzZ9LHsieCI6MTk2LjA2MjgyNTUyMDgzMzM0LCJ5IjozNzZ9LHsieCI6MTk2LjA2MjgyNTUyMDgzMzM0LCJ5IjozOTZ9XQ%3D%3D%22%20marker-start%3D%22url\(%23mermaid-_r_p2__er-onlyOneStart\)%22%20marker-end%3D%22url\(%23mermaid-_r_p2__er-zeroOrMoreEnd\)%22%3E%3C%2Fpath%3E%3Cpath%20d%3D%22M373.94173177083337%2C91L373.94173177083337%2C104.21704397470535Q373.94173177083337%2C106%20375.02751820846026%2C107.41421356237309L375.02751820846026%2C107.41421356237309Q376.11330464608716%2C108.82842712474618%20377.52751820846026%2C109.91421356237308L377.52751820846026%2C109.91421356237309Q378.94173177083337%2C111%20380.724687796128%2C111L490.4126819955387%2C111Q492.19563802083337%2C111%20493.6098515832065%2C112.08578643762691L493.6098515832065%2C112.08578643762692Q495.0240651455796%2C113.17157287525382%20496.1098515832065%2C114.58578643762691L496.1098515832065%2C114.58578643762691Q497.19563802083337%2C116%20497.19563802083337%2C117.78295602529465L497.19563802083337%2C214%22%20id%3D%22id_entity-BUSINESS-2_entity-REWARD-5_6%22%20class%3D%22edge-thickness-normal%20edge-pattern-solid%20relationshipLine%22%20style%3D%22undefined%3B%3B%3Bundefined%22%20data-edge%3D%22true%22%20data-et%3D%22edge%22%20data-id%3D%22id_entity-BUSINESS-2_entity-REWARD-5_6%22%20data-points%3D%22W3sieCI6MzczLjk0MTczMTc3MDgzMzM3LCJ5Ijo5MX0seyJ4IjozNzMuOTQxNzMxNzcwODMzMzcsInkiOjExMX0seyJ4Ijo0OTcuMTk1NjM4MDIwODMzMzcsInkiOjExMX0seyJ4Ijo0OTcuMTk1NjM4MDIwODMzMzcsInkiOjIxNH1d%22%20marker-start%3D%22url\(%23mermaid-_r_p2__er-onlyOneStart\)%22%20marker-end%3D%22url\(%23mermaid-_r_p2__er-zeroOrMoreEnd\)%22%3E%3C%2Fpath%3E%3Cpath%20d%3D%22M381.5960286458334%2C293L381.59602864583337%2C305.9289321881345Q381.59602864583337%2C313%20388.66709645769885%2C313L410.5181507455387%2C313Q412.30110677083337%2C313%20413.7153203332065%2C314.0857864376269L413.7153203332065%2C314.0857864376269Q415.1295338955796%2C315.1715728752538%20416.2153203332065%2C316.5857864376269L416.2153203332065%2C316.5857864376269Q417.30110677083337%2C318%20417.30110677083337%2C319.78295602529465L417.30110677083337%2C369.21704397470535Q417.30110677083337%2C371%20418.38689320846026%2C372.4142135623731L418.38689320846026%2C372.4142135623731Q419.47267964608716%2C373.8284271247462%20420.88689320846026%2C374.9142135623731L420.88689320846026%2C374.9142135623731Q422.30110677083337%2C376%20424.084062796128%2C376L448.79289032887203%2C376Q450.5758463541667%2C376%20451.9900599165398%2C377.0857864376269L451.9900599165398%2C377.0857864376269Q453.4042734789129%2C378.1715728752538%20454.4900599165398%2C379.5857864376269L454.4900599165398%2C379.5857864376269Q455.5758463541667%2C381%20455.5758463541667%2C382.78295602529465L455.5758463541667%2C396%22%20id%3D%22id_entity-CUSTOMER-3_entity-REDEMPTION-6_7%22%20class%3D%22edge-thickness-normal%20edge-pattern-solid%20relationshipLine%22%20style%3D%22undefined%3B%3B%3Bundefined%22%20data-edge%3D%22true%22%20data-et%3D%22edge%22%20data-id%3D%22id_entity-CUSTOMER-3_entity-REDEMPTION-6_7%22%20data-points%3D%22W3sieCI6MzgxLjU5NjAyODY0NTgzMzQsInkiOjI5M30seyJ4IjozODEuNTk2MDI4NjQ1ODMzMzcsInkiOjMxM30seyJ4Ijo0MTcuMzAxMTA2NzcwODMzMzcsInkiOjMxM30seyJ4Ijo0MTcuMzAxMTA2NzcwODMzMzcsInkiOjM3Nn0seyJ4Ijo0NTUuNTc1ODQ2MzU0MTY2NywieSI6Mzc2fSx7IngiOjQ1NS41NzU4NDYzNTQxNjY3LCJ5IjozOTZ9XQ%3D%3D%22%20marker-start%3D%22url\(%23mermaid-_r_p2__er-onlyOneStart\)%22%20marker-end%3D%22url\(%23mermaid-_r_p2__er-zeroOrMoreEnd\)%22%3E%3C%2Fpath%3E%3Cpath%20d%3D%22M497.19563802083337%2C293L497.19563802083337%2C396%22%20id%3D%22id_entity-REWARD-5_entity-REDEMPTION-6_8%22%20class%3D%22edge-thickness-normal%20edge-pattern-solid%20relationshipLine%22%20style%3D%22undefined%3B%3B%3Bundefined%22%20data-edge%3D%22true%22%20data-et%3D%22edge%22%20data-id%3D%22id_entity-REWARD-5_entity-REDEMPTION-6_8%22%20data-points%3D%22W3sieCI6NDk3LjE5NTYzODAyMDgzMzM3LCJ5IjoyOTN9LHsieCI6NDk3LjE5NTYzODAyMDgzMzM3LCJ5IjozOTZ9XQ%3D%3D%22%20marker-start%3D%22url\(%23mermaid-_r_p2__er-onlyOneStart\)%22%20marker-end%3D%22url\(%23mermaid-_r_p2__er-zeroOrMoreEnd\)%22%3E%3C%2Fpath%3E%3Cpath%20d%3D%22M353.69173177083337%2C293L353.69173177083337%2C344.5L353.69173177083337%2C526.5L353.69173177083337%2C551.2170439747053Q353.69173177083337%2C553%20354.77751820846026%2C554.4142135623731L354.77751820846026%2C554.4142135623731Q355.86330464608716%2C555.8284271247462%20357.27751820846026%2C556.9142135623731L357.27751820846026%2C556.9142135623731Q358.69173177083337%2C558%20360.474687796128%2C558L434.08325491220535%2C558Q435.8662109375%2C558%20437.2804244998731%2C559.0857864376269L437.2804244998731%2C559.0857864376269Q438.6946380622462%2C560.1715728752538%20439.7804244998731%2C561.5857864376269L439.7804244998731%2C561.5857864376269Q440.8662109375%2C563%20440.8662109375%2C564.7829560252947L440.8662109375%2C598%22%20id%3D%22id_entity-CUSTOMER-3_entity-POINTS_LEDGER-7_9%22%20class%3D%22edge-thickness-normal%20edge-pattern-solid%20relationshipLine%22%20style%3D%22undefined%3B%3B%3Bundefined%22%20data-edge%3D%22true%22%20data-et%3D%22edge%22%20data-id%3D%22id_entity-CUSTOMER-3_entity-POINTS_LEDGER-7_9%22%20data-points%3D%22W3sieCI6MzUzLjY5MTczMTc3MDgzMzM3LCJ5IjoyOTN9LHsieCI6MzUzLjY5MTczMTc3MDgzMzM3LCJ5IjozNDQuNX0seyJ4IjozNTMuNjkxNzMxNzcwODMzMzcsInkiOjUyNi41fSx7IngiOjM1My42OTE3MzE3NzA4MzMzNywieSI6NTU4fSx7IngiOjQ0MC44NjYyMTA5Mzc1LCJ5Ijo1NTh9LHsieCI6NDQwLjg2NjIxMDkzNzUsInkiOjU5OH1d%22%20marker-start%3D%22url\(%23mermaid-_r_p2__er-onlyOneStart\)%22%20marker-end%3D%22url\(%23mermaid-_r_p2__er-zeroOrMoreEnd\)%22%3E%3C%2Fpath%3E%3Cpath%20d%3D%22M229.10579427083334%2C475L229.10579427083334%2C571.2170439747053Q229.10579427083334%2C573%20230.19158070846026%2C574.4142135623731L230.19158070846026%2C574.4142135623731Q231.27736714608716%2C575.8284271247462%20232.69158070846026%2C576.9142135623731L232.69158070846026%2C576.9142135623731Q234.10579427083334%2C578%20235.888750296128%2C578L398.56372366220535%2C578Q400.3466796875%2C578%20401.7608932498731%2C579.0857864376269L401.7608932498731%2C579.0857864376269Q403.1751068122462%2C580.1715728752538%20404.2608932498731%2C581.5857864376269L404.2608932498731%2C581.5857864376269Q405.3466796875%2C583%20405.3466796875%2C584.7829560252947L405.3466796875%2C598%22%20id%3D%22id_entity-TRANSACTION-4_entity-POINTS_LEDGER-7_10%22%20class%3D%22edge-thickness-normal%20edge-pattern-solid%20relationshipLine%22%20style%3D%22undefined%3B%3B%3Bundefined%22%20data-edge%3D%22true%22%20data-et%3D%22edge%22%20data-id%3D%22id_entity-TRANSACTION-4_entity-POINTS_LEDGER-7_10%22%20data-points%3D%22W3sieCI6MjI5LjEwNTc5NDI3MDgzMzM0LCJ5Ijo0NzV9LHsieCI6MjI5LjEwNTc5NDI3MDgzMzM0LCJ5Ijo1Nzh9LHsieCI6NDA1LjM0NjY3OTY4NzUsInkiOjU3OH0seyJ4Ijo0MDUuMzQ2Njc5Njg3NSwieSI6NTk4fV0%3D%22%20marker-start%3D%22url\(%23mermaid-_r_p2__er-zeroOrOneStart\)%22%20marker-end%3D%22url\(%23mermaid-_r_p2__er-zeroOrMoreEnd\)%22%3E%3C%2Fpath%3E%3Cpath%20d%3D%22M476.3857421875%2C475L476.3857421875%2C598%22%20id%3D%22id_entity-REDEMPTION-6_entity-POINTS_LEDGER-7_11%22%20class%3D%22edge-thickness-normal%20edge-pattern-solid%20relationshipLine%22%20style%3D%22undefined%3B%3B%3Bundefined%22%20data-edge%3D%22true%22%20data-et%3D%22edge%22%20data-id%3D%22id_entity-REDEMPTION-6_entity-POINTS_LEDGER-7_11%22%20data-points%3D%22W3sieCI6NDc2LjM4NTc0MjE4NzUsInkiOjQ3NX0seyJ4Ijo0NzYuMzg1NzQyMTg3NSwieSI6NTk4fV0%3D%22%20marker-start%3D%22url\(%23mermaid-_r_p2__er-zeroOrOneStart\)%22%20marker-end%3D%22url\(%23mermaid-_r_p2__er-zeroOrMoreEnd\)%22%3E%3C%2Fpath%3E%3C%2Fg%3E%3Cstyle%3E%23mermaid-_r_p2_%7Bfont-family%3A%22-apple-system%22%2C%22BlinkMacSystemFont%22%2C%22Segoe%20UI%22%2C%22Roboto%22%2C%22Oxygen%22%2C%22Ubuntu%22%2C%22Cantarell%22%2C%22Helvetica%20Neue%22%2C%22Arial%22%2C%22sans-serif%22%3Bfont-size%3A14px%3Bfill%3Argb\(255%2C%20255%2C%20255\)%3B%7D%40keyframes%20edge-animation-frame%7Bfrom%7Bstroke-dashoffset%3A0%3B%7D%7D%40keyframes%20dash%7Bto%7Bstroke-dashoffset%3A0%3B%7D%7D%23mermaid-_r_p2_%20.edge-animation-slow%7Bstroke-dasharray%3A9%2C5!important%3Bstroke-dashoffset%3A900%3Banimation%3Adash%2050s%20linear%20infinite%3Bstroke-linecap%3Around%3B%7D%23mermaid-_r_p2_%20.edge-animation-fast%7Bstroke-dasharray%3A9%2C5!important%3Bstroke-dashoffset%3A900%3Banimation%3Adash%2020s%20linear%20infinite%3Bstroke-linecap%3Around%3B%7D%23mermaid-_r_p2_%20.error-icon%7Bfill%3Argb\(33%2C%2033%2C%2033\)%3B%7D%23mermaid-_r_p2_%20.error-text%7Bfill%3Argb\(255%2C%20255%2C%20255\)%3Bstroke%3Argb\(255%2C%20255%2C%20255\)%3B%7D%23mermaid-_r_p2_%20.edge-thickness-normal%7Bstroke-width%3A1px%3B%7D%23mermaid-_r_p2_%20.edge-thickness-thick%7Bstroke-width%3A3.5px%3B%7D%23mermaid-_r_p2_%20.edge-pattern-solid%7Bstroke-dasharray%3A0%3B%7D%23mermaid-_r_p2_%20.edge-thickness-invisible%7Bstroke-width%3A0%3Bfill%3Anone%3B%7D%23mermaid-_r_p2_%20.edge-pattern-dashed%7Bstroke-dasharray%3A3%3B%7D%23mermaid-_r_p2_%20.edge-pattern-dotted%7Bstroke-dasharray%3A2%3B%7D%23mermaid-_r_p2_%20.marker%7Bfill%3Argb\(205%2C%20205%2C%20205\)%3Bstroke%3Argb\(205%2C%20205%2C%20205\)%3B%7D%23mermaid-_r_p2_%20.marker.cross%7Bstroke%3Argb\(205%2C%20205%2C%20205\)%3B%7D%23mermaid-_r_p2_%20svg%7Bfont-family%3A%22-apple-system%22%2C%22BlinkMacSystemFont%22%2C%22Segoe%20UI%22%2C%22Roboto%22%2C%22Oxygen%22%2C%22Ubuntu%22%2C%22Cantarell%22%2C%22Helvetica%20Neue%22%2C%22Arial%22%2C%22sans-serif%22%3Bfont-size%3A14px%3B%7D%23mermaid-_r_p2_%20p%7Bmargin%3A0%3B%7D%23mermaid-_r_p2_%20.entityBox%7Bfill%3Argb\(9%2C%2023%2C%2044\)%3Bstroke%3Argb\(31%2C%2078%2C%20148\)%3B%7D%23mermaid-_r_p2_%20.relationshipLabelBox%7Bfill%3Argb\(33%2C%2033%2C%2033\)%3Bopacity%3A0.7%3Bbackground-color%3Argb\(33%2C%2033%2C%2033\)%3B%7D%23mermaid-_r_p2_%20.relationshipLabelBox%20rect%7Bopacity%3A0.5%3B%7D%23mermaid-_r_p2_%20.labelBkg%7Bbackground-color%3Argba\(33%2C%2033%2C%2033%2C%200.5\)%3B%7D%23mermaid-_r_p2_%20.edgeLabel%20.label%7Bfill%3Argb\(31%2C%2078%2C%20148\)%3Bfont-size%3A14px%3B%7D%23mermaid-_r_p2_%20.label%7Bfont-family%3A%22-apple-system%22%2C%22BlinkMacSystemFont%22%2C%22Segoe%20UI%22%2C%22Roboto%22%2C%22Oxygen%22%2C%22Ubuntu%22%2C%22Cantarell%22%2C%22Helvetica%20Neue%22%2C%22Arial%22%2C%22sans-serif%22%3Bcolor%3Argb\(255%2C%20255%2C%20255\)%3B%7D%23mermaid-_r_p2_%20.edge-pattern-dashed%7Bstroke-dasharray%3A8%2C8%3B%7D%23mermaid-_r_p2_%20.node%20rect%2C%23mermaid-_r_p2_%20.node%20circle%2C%23mermaid-_r_p2_%20.node%20ellipse%2C%23mermaid-_r_p2_%20.node%20polygon%7Bfill%3Argb\(9%2C%2023%2C%2044\)%3Bstroke%3Argb\(31%2C%2078%2C%20148\)%3Bstroke-width%3A1px%3B%7D%23mermaid-_r_p2_%20.relationshipLine%7Bstroke%3Argb\(205%2C%20205%2C%20205\)%3Bstroke-width%3A1%3Bfill%3Anone%3B%7D%23mermaid-_r_p2_%20.marker%7Bfill%3Anone!important%3Bstroke%3Argb\(205%2C%20205%2C%20205\)!important%3Bstroke-width%3A1%3B%7D%23mermaid-_r_p2_%20.node%7Bcolor-scheme%3Adark%3B%7D%23mermaid-_r_p2_%20%3Aroot%7B--mermaid-font-family%3A%22-apple-system%22%2C%22BlinkMacSystemFont%22%2C%22Segoe%20UI%22%2C%22Roboto%22%2C%22Oxygen%22%2C%22Ubuntu%22%2C%22Cantarell%22%2C%22Helvetica%20Neue%22%2C%22Arial%22%2C%22sans-serif%22%3B%7D%3C%2Fstyle%3E%3Cg%3E%3Cdefs%3E%3Cmarker%20id%3D%22mermaid-_r_p2__er-onlyOneStart%22%20class%3D%22marker%20onlyOne%20er%22%20refX%3D%220%22%20refY%3D%229%22%20markerWidth%3D%2218%22%20markerHeight%3D%2218%22%20orient%3D%22auto%22%3E%3Cpath%20d%3D%22M9%2C0%20L9%2C18%20M15%2C0%20L15%2C18%22%3E%3C%2Fpath%3E%3C%2Fmarker%3E%3C%2Fdefs%3E%3Cdefs%3E%3Cmarker%20id%3D%22mermaid-_r_p2__er-onlyOneEnd%22%20class%3D%22marker%20onlyOne%20er%22%20refX%3D%2218%22%20refY%3D%229%22%20markerWidth%3D%2218%22%20markerHeight%3D%2218%22%20orient%3D%22auto%22%3E%3Cpath%20d%3D%22M3%2C0%20L3%2C18%20M9%2C0%20L9%2C18%22%3E%3C%2Fpath%3E%3C%2Fmarker%3E%3C%2Fdefs%3E%3Cdefs%3E%3Cmarker%20id%3D%22mermaid-_r_p2__er-zeroOrOneStart%22%20class%3D%22marker%20zeroOrOne%20er%22%20refX%3D%220%22%20refY%3D%229%22%20markerWidth%3D%2230%22%20markerHeight%3D%2218%22%20orient%3D%22auto%22%3E%3Ccircle%20fill%3D%22white%22%20cx%3D%2221%22%20cy%3D%229%22%20r%3D%226%22%3E%3C%2Fcircle%3E%3Cpath%20d%3D%22M9%2C0%20L9%2C18%22%3E%3C%2Fpath%3E%3C%2Fmarker%3E%3C%2Fdefs%3E%3Cdefs%3E%3Cmarker%20id%3D%22mermaid-_r_p2__er-zeroOrOneEnd%22%20class%3D%22marker%20zeroOrOne%20er%22%20refX%3D%2230%22%20refY%3D%229%22%20markerWidth%3D%2230%22%20markerHeight%3D%2218%22%20orient%3D%22auto%22%3E%3Ccircle%20fill%3D%22white%22%20cx%3D%229%22%20cy%3D%229%22%20r%3D%226%22%3E%3C%2Fcircle%3E%3Cpath%20d%3D%22M21%2C0%20L21%2C18%22%3E%3C%2Fpath%3E%3C%2Fmarker%3E%3C%2Fdefs%3E%3Cdefs%3E%3Cmarker%20id%3D%22mermaid-_r_p2__er-oneOrMoreStart%22%20class%3D%22marker%20oneOrMore%20er%22%20refX%3D%2218%22%20refY%3D%2218%22%20markerWidth%3D%2245%22%20markerHeight%3D%2236%22%20orient%3D%22auto%22%3E%3Cpath%20d%3D%22M0%2C18%20Q%2018%2C0%2036%2C18%20Q%2018%2C36%200%2C18%20M42%2C9%20L42%2C27%22%3E%3C%2Fpath%3E%3C%2Fmarker%3E%3C%2Fdefs%3E%3Cdefs%3E%3Cmarker%20id%3D%22mermaid-_r_p2__er-oneOrMoreEnd%22%20class%3D%22marker%20oneOrMore%20er%22%20refX%3D%2227%22%20refY%3D%2218%22%20markerWidth%3D%2245%22%20markerHeight%3D%2236%22%20orient%3D%22auto%22%3E%3Cpath%20d%3D%22M3%2C9%20L3%2C27%20M9%2C18%20Q27%2C0%2045%2C18%20Q27%2C36%209%2C18%22%3E%3C%2Fpath%3E%3C%2Fmarker%3E%3C%2Fdefs%3E%3Cdefs%3E%3Cmarker%20id%3D%22mermaid-_r_p2__er-zeroOrMoreStart%22%20class%3D%22marker%20zeroOrMore%20er%22%20refX%3D%2218%22%20refY%3D%2218%22%20markerWidth%3D%2257%22%20markerHeight%3D%2236%22%20orient%3D%22auto%22%3E%3Ccircle%20fill%3D%22white%22%20cx%3D%2248%22%20cy%3D%2218%22%20r%3D%226%22%3E%3C%2Fcircle%3E%3Cpath%20d%3D%22M0%2C18%20Q18%2C0%2036%2C18%20Q18%2C36%200%2C18%22%3E%3C%2Fpath%3E%3C%2Fmarker%3E%3C%2Fdefs%3E%3Cdefs%3E%3Cmarker%20id%3D%22mermaid-_r_p2__er-zeroOrMoreEnd%22%20class%3D%22marker%20zeroOrMore%20er%22%20refX%3D%2239%22%20refY%3D%2218%22%20markerWidth%3D%2257%22%20markerHeight%3D%2236%22%20orient%3D%22auto%22%3E%3Ccircle%20fill%3D%22white%22%20cx%3D%229%22%20cy%3D%2218%22%20r%3D%226%22%3E%3C%2Fcircle%3E%3Cpath%20d%3D%22M21%2C18%20Q39%2C0%2057%2C18%20Q39%2C36%2021%2C18%22%3E%3C%2Fpath%3E%3C%2Fmarker%3E%3C%2Fdefs%3E%3C%2Fg%3E%3Cg%20class%3D%22subgraphs%22%3E%3C%2Fg%3E%3Cg%20class%3D%22nodes%22%3E%3Cg%20class%3D%22node%20default%22%20id%3D%22entity-USER-0%22%20transform%3D%22translate\(62%2C%2051.5\)%22%3E%3Crect%20class%3D%22basic%20label-container%22%20style%3D%22%22%20x%3D%22-50%22%20y%3D%22-39.5%22%20width%3D%22100%22%20height%3D%2279%22%3E%3C%2Frect%3E%3Cg%20class%3D%22label%22%20style%3D%22%22%20transform%3D%22translate\(0%2C%20-9.5\)%22%3E%3Crect%3E%3C%2Frect%3E%3Cg%3E%3Crect%20class%3D%22background%22%20style%3D%22stroke%3A%20none%22%3E%3C%2Frect%3E%3Ctext%20y%3D%22-10.1%22%20style%3D%22%22%20transform%3D%22translate\(-16.56884765625%2C%200\)%22%3E%3Ctspan%20class%3D%22text-outer-tspan%22%20x%3D%220%22%20y%3D%22-0.1em%22%20dy%3D%221.1em%22%3E%3Ctspan%20font-style%3D%22normal%22%20class%3D%22text-inner-tspan%22%20font-weight%3D%22normal%22%3EUSER%3C%2Ftspan%3E%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fg%3E%3C%2Fg%3E%3Cg%20class%3D%22node%20default%22%20id%3D%22entity-MEMBERSHIP-1%22%20transform%3D%22translate\(100.74446614583337%2C%20253.5\)%22%3E%3Crect%20class%3D%22basic%20label-container%22%20style%3D%22%22%20x%3D%22-62.404296875%22%20y%3D%22-39.5%22%20width%3D%22124.80859375%22%20height%3D%2279%22%3E%3C%2Frect%3E%3Cg%20class%3D%22label%22%20style%3D%22%22%20transform%3D%22translate\(0%2C%20-9.5\)%22%3E%3Crect%3E%3C%2Frect%3E%3Cg%3E%3Crect%20class%3D%22background%22%20style%3D%22stroke%3A%20none%22%3E%3C%2Frect%3E%3Ctext%20y%3D%22-10.1%22%20style%3D%22%22%20transform%3D%22translate\(-42.404296875%2C%200\)%22%3E%3Ctspan%20class%3D%22text-outer-tspan%22%20x%3D%220%22%20y%3D%22-0.1em%22%20dy%3D%221.1em%22%3E%3Ctspan%20font-style%3D%22normal%22%20class%3D%22text-inner-tspan%22%20font-weight%3D%22normal%22%3EMEMBERSHIP%3C%2Ftspan%3E%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fg%3E%3C%2Fg%3E%3Cg%20class%3D%22node%20default%22%20id%3D%22entity-BUSINESS-2%22%20transform%3D%22translate\(343.56673177083337%2C%2051.5\)%22%3E%3Crect%20class%3D%22basic%20label-container%22%20style%3D%22%22%20x%3D%22-50.625%22%20y%3D%22-39.5%22%20width%3D%22101.25%22%20height%3D%2279%22%3E%3C%2Frect%3E%3Cg%20class%3D%22label%22%20style%3D%22%22%20transform%3D%22translate\(0%2C%20-9.5\)%22%3E%3Crect%3E%3C%2Frect%3E%3Cg%3E%3Crect%20class%3D%22background%22%20style%3D%22stroke%3A%20none%22%3E%3C%2Frect%3E%3Ctext%20y%3D%22-10.1%22%20style%3D%22%22%20transform%3D%22translate\(-30.625%2C%200\)%22%3E%3Ctspan%20class%3D%22text-outer-tspan%22%20x%3D%220%22%20y%3D%22-0.1em%22%20dy%3D%221.1em%22%3E%3Ctspan%20font-style%3D%22normal%22%20class%3D%22text-inner-tspan%22%20font-weight%3D%22normal%22%3EBUSINESS%3C%2Ftspan%3E%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fg%3E%3C%2Fg%3E%3Cg%20class%3D%22node%20default%22%20id%3D%22entity-CUSTOMER-3%22%20transform%3D%22translate\(353.69173177083337%2C%20253.5\)%22%3E%3Crect%20class%3D%22basic%20label-container%22%20style%3D%22%22%20x%3D%22-55.80859375%22%20y%3D%22-39.5%22%20width%3D%22111.6171875%22%20height%3D%2279%22%3E%3C%2Frect%3E%3Cg%20class%3D%22label%22%20style%3D%22%22%20transform%3D%22translate\(0%2C%20-9.5\)%22%3E%3Crect%3E%3C%2Frect%3E%3Cg%3E%3Crect%20class%3D%22background%22%20style%3D%22stroke%3A%20none%22%3E%3C%2Frect%3E%3Ctext%20y%3D%22-10.1%22%20style%3D%22%22%20transform%3D%22translate\(-35.80859375%2C%200\)%22%3E%3Ctspan%20class%3D%22text-outer-tspan%22%20x%3D%220%22%20y%3D%22-0.1em%22%20dy%3D%221.1em%22%3E%3Ctspan%20font-style%3D%22normal%22%20class%3D%22text-inner-tspan%22%20font-weight%3D%22normal%22%3ECUSTOMER%3C%2Ftspan%3E%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fg%3E%3C%2Fg%3E%3Cg%20class%3D%22node%20default%22%20id%3D%22entity-TRANSACTION-4%22%20transform%3D%22translate\(229.10579427083334%2C%20435.5\)%22%3E%3Crect%20class%3D%22basic%20label-container%22%20style%3D%22%22%20x%3D%22-66.0859375%22%20y%3D%22-39.5%22%20width%3D%22132.171875%22%20height%3D%2279%22%3E%3C%2Frect%3E%3Cg%20class%3D%22label%22%20style%3D%22%22%20transform%3D%22translate\(0%2C%20-9.5\)%22%3E%3Crect%3E%3C%2Frect%3E%3Cg%3E%3Crect%20class%3D%22background%22%20style%3D%22stroke%3A%20none%22%3E%3C%2Frect%3E%3Ctext%20y%3D%22-10.1%22%20style%3D%22%22%20transform%3D%22translate\(-46.0859375%2C%200\)%22%3E%3Ctspan%20class%3D%22text-outer-tspan%22%20x%3D%220%22%20y%3D%22-0.1em%22%20dy%3D%221.1em%22%3E%3Ctspan%20font-style%3D%22normal%22%20class%3D%22text-inner-tspan%22%20font-weight%3D%22normal%22%3ETRANSACTION%3C%2Ftspan%3E%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fg%3E%3C%2Fg%3E%3Cg%20class%3D%22node%20default%22%20id%3D%22entity-REWARD-5%22%20transform%3D%22translate\(497.19563802083337%2C%20253.5\)%22%3E%3Crect%20class%3D%22basic%20label-container%22%20style%3D%22%22%20x%3D%22-47.6953125%22%20y%3D%22-39.5%22%20width%3D%2295.390625%22%20height%3D%2279%22%3E%3C%2Frect%3E%3Cg%20class%3D%22label%22%20style%3D%22%22%20transform%3D%22translate\(0%2C%20-9.5\)%22%3E%3Crect%3E%3C%2Frect%3E%3Cg%3E%3Crect%20class%3D%22background%22%20style%3D%22stroke%3A%20none%22%3E%3C%2Frect%3E%3Ctext%20y%3D%22-10.1%22%20style%3D%22%22%20transform%3D%22translate\(-27.6953125%2C%200\)%22%3E%3Ctspan%20class%3D%22text-outer-tspan%22%20x%3D%220%22%20y%3D%22-0.1em%22%20dy%3D%221.1em%22%3E%3Ctspan%20font-style%3D%22normal%22%20class%3D%22text-inner-tspan%22%20font-weight%3D%22normal%22%3EREWARD%3C%2Ftspan%3E%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fg%3E%3C%2Fg%3E%3Cg%20class%3D%22node%20default%22%20id%3D%22entity-REDEMPTION-6%22%20transform%3D%22translate\(476.3857421875%2C%20435.5\)%22%3E%3Crect%20class%3D%22basic%20label-container%22%20style%3D%22%22%20x%3D%22-62.4296875%22%20y%3D%22-39.5%22%20width%3D%22124.859375%22%20height%3D%2279%22%3E%3C%2Frect%3E%3Cg%20class%3D%22label%22%20style%3D%22%22%20transform%3D%22translate\(0%2C%20-9.5\)%22%3E%3Crect%3E%3C%2Frect%3E%3Cg%3E%3Crect%20class%3D%22background%22%20style%3D%22stroke%3A%20none%22%3E%3C%2Frect%3E%3Ctext%20y%3D%22-10.1%22%20style%3D%22%22%20transform%3D%22translate\(-42.4296875%2C%200\)%22%3E%3Ctspan%20class%3D%22text-outer-tspan%22%20x%3D%220%22%20y%3D%22-0.1em%22%20dy%3D%221.1em%22%3E%3Ctspan%20font-style%3D%22normal%22%20class%3D%22text-inner-tspan%22%20font-weight%3D%22normal%22%3EREDEMPTION%3C%2Ftspan%3E%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fg%3E%3C%2Fg%3E%3Cg%20class%3D%22node%20default%22%20id%3D%22entity-POINTS_LEDGER-7%22%20transform%3D%22translate\(440.8662109375%2C%20637.5\)%22%3E%3Crect%20class%3D%22basic%20label-container%22%20style%3D%22%22%20x%3D%22-71.0390625%22%20y%3D%22-39.5%22%20width%3D%22142.078125%22%20height%3D%2279%22%3E%3C%2Frect%3E%3Cg%20class%3D%22label%22%20style%3D%22%22%20transform%3D%22translate\(0%2C%20-9.5\)%22%3E%3Crect%3E%3C%2Frect%3E%3Cg%3E%3Crect%20class%3D%22background%22%20style%3D%22stroke%3A%20none%22%3E%3C%2Frect%3E%3Ctext%20y%3D%22-10.1%22%20style%3D%22%22%20transform%3D%22translate\(-51.0390625%2C%200\)%22%3E%3Ctspan%20class%3D%22text-outer-tspan%22%20x%3D%220%22%20y%3D%22-0.1em%22%20dy%3D%221.1em%22%3E%3Ctspan%20font-style%3D%22normal%22%20class%3D%22text-inner-tspan%22%20font-weight%3D%22normal%22%3EPOINTS_LEDGER%3C%2Ftspan%3E%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fg%3E%3C%2Fg%3E%3C%2Fg%3E%3Cg%20class%3D%22edgeLabels%22%3E%3Cg%20class%3D%22edgeLabel%22%20transform%3D%22translate\(61.52294921875%2C%20162.5\)%22%3E%3Cg%20class%3D%22label%22%20data-id%3D%22id_entity-USER-0_entity-MEMBERSHIP-1_0%22%20transform%3D%22translate\(-12.52294921875%2C%20-11.5\)%22%3E%3Cg%3E%3Crect%20class%3D%22background%22%20style%3D%22%22%20x%3D%22-2%22%20y%3D%22-2.9999990463256836%22%20width%3D%2225.0458984375%22%20height%3D%2223%22%3E%3C%2Frect%3E%3Ctext%20y%3D%22-10.1%22%20style%3D%22%22%3E%3Ctspan%20class%3D%22text-outer-tspan%22%20x%3D%220%22%20y%3D%22-0.1em%22%20dy%3D%221.1em%22%3E%3Ctspan%20font-style%3D%22normal%22%20class%3D%22text-inner-tspan%22%20font-weight%3D%22normal%22%3Ehas%3C%2Ftspan%3E%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fg%3E%3C%2Fg%3E%3Cg%20class%3D%22edgeLabel%22%20transform%3D%22translate\(121.37548828125%2C%20162.5\)%22%3E%3Cg%20class%3D%22label%22%20data-id%3D%22id_entity-BUSINESS-2_entity-MEMBERSHIP-1_1%22%20transform%3D%22translate\(-27.32958984375%2C%20-11.5\)%22%3E%3Cg%3E%3Crect%20class%3D%22background%22%20style%3D%22%22%20x%3D%22-2%22%20y%3D%22-2.9999990463256836%22%20width%3D%2254.6591796875%22%20height%3D%2223%22%3E%3C%2Frect%3E%3Ctext%20y%3D%22-10.1%22%20style%3D%22%22%3E%3Ctspan%20class%3D%22text-outer-tspan%22%20x%3D%220%22%20y%3D%22-0.1em%22%20dy%3D%221.1em%22%3E%3Ctspan%20font-style%3D%22normal%22%20class%3D%22text-inner-tspan%22%20font-weight%3D%22normal%22%3Eincludes%3C%2Ftspan%3E%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fg%3E%3C%2Fg%3E%3Cg%20class%3D%22edgeLabel%22%20transform%3D%22translate\(353.58821614583337%2C%20162.5\)%22%3E%3Cg%20class%3D%22label%22%20data-id%3D%22id_entity-BUSINESS-2_entity-CUSTOMER-3_2%22%20transform%3D%22translate\(-29.896484375%2C%20-11.5\)%22%3E%3Cg%3E%3Crect%20class%3D%22background%22%20style%3D%22%22%20x%3D%22-2%22%20y%3D%22-2.9999990463256836%22%20width%3D%2259.79296875%22%20height%3D%2223%22%3E%3C%2Frect%3E%3Ctext%20y%3D%22-10.1%22%20style%3D%22%22%3E%3Ctspan%20class%3D%22text-outer-tspan%22%20x%3D%220%22%20y%3D%22-0.1em%22%20dy%3D%221.1em%22%3E%3Ctspan%20font-style%3D%22normal%22%20class%3D%22text-inner-tspan%22%20font-weight%3D%22normal%22%3Emanages%3C%2Ftspan%3E%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fg%3E%3C%2Fg%3E%3Cg%20class%3D%22edgeLabel%22%20transform%3D%22translate\(228.13362630208334%2C%20253.5\)%22%3E%3Cg%20class%3D%22label%22%20data-id%3D%22id_entity-BUSINESS-2_entity-TRANSACTION-4_3%22%20transform%3D%22translate\(-24.98486328125%2C%20-11.5\)%22%3E%3Cg%3E%3Crect%20class%3D%22background%22%20style%3D%22%22%20x%3D%22-2%22%20y%3D%22-2.9999990463256836%22%20width%3D%2249.9697265625%22%20height%3D%2223%22%3E%3C%2Frect%3E%3Ctext%20y%3D%22-10.1%22%20style%3D%22%22%3E%3Ctspan%20class%3D%22text-outer-tspan%22%20x%3D%220%22%20y%3D%22-0.1em%22%20dy%3D%221.1em%22%3E%3Ctspan%20font-style%3D%22normal%22%20class%3D%22text-inner-tspan%22%20font-weight%3D%22normal%22%3Erecords%3C%2Ftspan%3E%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fg%3E%3C%2Fg%3E%3Cg%20class%3D%22edgeLabel%22%20transform%3D%22translate\(261.67171223958337%2C%20344.5\)%22%3E%3Cg%20class%3D%22label%22%20data-id%3D%22id_entity-CUSTOMER-3_entity-TRANSACTION-4_4%22%20transform%3D%22translate\(-12.52294921875%2C%20-11.5\)%22%3E%3Cg%3E%3Crect%20class%3D%22background%22%20style%3D%22%22%20x%3D%22-2%22%20y%3D%22-2.9999990463256836%22%20width%3D%2225.0458984375%22%20height%3D%2223%22%3E%3C%2Frect%3E%3Ctext%20y%3D%22-10.1%22%20style%3D%22%22%3E%3Ctspan%20class%3D%22text-outer-tspan%22%20x%3D%220%22%20y%3D%22-0.1em%22%20dy%3D%221.1em%22%3E%3Ctspan%20font-style%3D%22normal%22%20class%3D%22text-inner-tspan%22%20font-weight%3D%22normal%22%3Ehas%3C%2Ftspan%3E%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fg%3E%3C%2Fg%3E%3Cg%20class%3D%22edgeLabel%22%20transform%3D%22translate\(100.66634114583337%2C%20344.5\)%22%3E%3Cg%20class%3D%22label%22%20data-id%3D%22id_entity-MEMBERSHIP-1_entity-TRANSACTION-4_5%22%20transform%3D%22translate\(-23.921875%2C%20-11.5\)%22%3E%3Cg%3E%3Crect%20class%3D%22background%22%20style%3D%22%22%20x%3D%22-2%22%20y%3D%22-2.9999990463256836%22%20width%3D%2247.84375%22%20height%3D%2223%22%3E%3C%2Frect%3E%3Ctext%20y%3D%22-10.1%22%20style%3D%22%22%3E%3Ctspan%20class%3D%22text-outer-tspan%22%20x%3D%220%22%20y%3D%22-0.1em%22%20dy%3D%221.1em%22%3E%3Ctspan%20font-style%3D%22normal%22%20class%3D%22text-inner-tspan%22%20font-weight%3D%22normal%22%3Ecreates%3C%2Ftspan%3E%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fg%3E%3C%2Fg%3E%3Cg%20class%3D%22edgeLabel%22%20transform%3D%22translate\(496.77327473958337%2C%20162.5\)%22%3E%3Cg%20class%3D%22label%22%20data-id%3D%22id_entity-BUSINESS-2_entity-REWARD-5_6%22%20transform%3D%22translate\(-19.57763671875%2C%20-11.5\)%22%3E%3Cg%3E%3Crect%20class%3D%22background%22%20style%3D%22%22%20x%3D%22-2%22%20y%3D%22-2.9999990463256836%22%20width%3D%2239.1552734375%22%20height%3D%2223%22%3E%3C%2Frect%3E%3Ctext%20y%3D%22-10.1%22%20style%3D%22%22%3E%3Ctspan%20class%3D%22text-outer-tspan%22%20x%3D%220%22%20y%3D%22-0.1em%22%20dy%3D%221.1em%22%3E%3Ctspan%20font-style%3D%22normal%22%20class%3D%22text-inner-tspan%22%20font-weight%3D%22normal%22%3Eoffers%3C%2Ftspan%3E%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fg%3E%3C%2Fg%3E%3Cg%20class%3D%22edgeLabel%22%20transform%3D%22translate\(416.99837239583337%2C%20344.5\)%22%3E%3Cg%20class%3D%22label%22%20data-id%3D%22id_entity-CUSTOMER-3_entity-REDEMPTION-6_7%22%20transform%3D%22translate\(-26.697265625%2C%20-11.5\)%22%3E%3Cg%3E%3Crect%20class%3D%22background%22%20style%3D%22%22%20x%3D%22-2%22%20y%3D%22-2.9999990463256836%22%20width%3D%2253.39453125%22%20height%3D%2223%22%3E%3C%2Frect%3E%3Ctext%20y%3D%22-10.1%22%20style%3D%22%22%3E%3Ctspan%20class%3D%22text-outer-tspan%22%20x%3D%220%22%20y%3D%22-0.1em%22%20dy%3D%221.1em%22%3E%3Ctspan%20font-style%3D%22normal%22%20class%3D%22text-inner-tspan%22%20font-weight%3D%22normal%22%3Ereceives%3C%2Ftspan%3E%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fg%3E%3C%2Fg%3E%3Cg%20class%3D%22edgeLabel%22%20transform%3D%22translate\(497.04720052083337%2C%20344.5\)%22%3E%3Cg%20class%3D%22label%22%20data-id%3D%22id_entity-REWARD-5_entity-REDEMPTION-6_8%22%20transform%3D%22translate\(-33.3515625%2C%20-11.5\)%22%3E%3Cg%3E%3Crect%20class%3D%22background%22%20style%3D%22%22%20x%3D%22-2%22%20y%3D%22-2.9999990463256836%22%20width%3D%2266.703125%22%20height%3D%2223%22%3E%3C%2Frect%3E%3Ctext%20y%3D%22-10.1%22%20style%3D%22%22%3E%3Ctspan%20class%3D%22text-outer-tspan%22%20x%3D%220%22%20y%3D%22-0.1em%22%20dy%3D%221.1em%22%3E%3Ctspan%20font-style%3D%22normal%22%20class%3D%22text-inner-tspan%22%20font-weight%3D%22normal%22%3Eredeemed%3C%2Ftspan%3E%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fg%3E%3C%2Fg%3E%3Cg%20class%3D%22edgeLabel%22%20transform%3D%22translate\(353.31331380208337%2C%20435.5\)%22%3E%3Cg%20class%3D%22label%22%20data-id%3D%22id_entity-CUSTOMER-3_entity-POINTS_LEDGER-7_9%22%20transform%3D%22translate\(-18.12158203125%2C%20-11.5\)%22%3E%3Cg%3E%3Crect%20class%3D%22background%22%20style%3D%22%22%20x%3D%22-2%22%20y%3D%22-2.9999990463256836%22%20width%3D%2236.2431640625%22%20height%3D%2223%22%3E%3C%2Frect%3E%3Ctext%20y%3D%22-10.1%22%20style%3D%22%22%3E%3Ctspan%20class%3D%22text-outer-tspan%22%20x%3D%220%22%20y%3D%22-0.1em%22%20dy%3D%221.1em%22%3E%3Ctspan%20font-style%3D%22normal%22%20class%3D%22text-inner-tspan%22%20font-weight%3D%22normal%22%3Eowns%3C%2Ftspan%3E%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fg%3E%3C%2Fg%3E%3Cg%20class%3D%22edgeLabel%22%20transform%3D%22translate\(228.72298177083334%2C%20526.5\)%22%3E%3Cg%20class%3D%22label%22%20data-id%3D%22id_entity-TRANSACTION-4_entity-POINTS_LEDGER-7_10%22%20transform%3D%22translate\(-18.6171875%2C%20-11.5\)%22%3E%3Cg%3E%3Crect%20class%3D%22background%22%20style%3D%22%22%20x%3D%22-2%22%20y%3D%22-2.9999990463256836%22%20width%3D%2237.234375%22%20height%3D%2223%22%3E%3C%2Frect%3E%3Ctext%20y%3D%22-10.1%22%20style%3D%22%22%3E%3Ctspan%20class%3D%22text-outer-tspan%22%20x%3D%220%22%20y%3D%22-0.1em%22%20dy%3D%221.1em%22%3E%3Ctspan%20font-style%3D%22normal%22%20class%3D%22text-inner-tspan%22%20font-weight%3D%22normal%22%3Eearns%3C%2Ftspan%3E%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fg%3E%3C%2Fg%3E%3Cg%20class%3D%22edgeLabel%22%20transform%3D%22translate\(476.21533203125%2C%20526.5\)%22%3E%3Cg%20class%3D%22label%22%20data-id%3D%22id_entity-REDEMPTION-6_entity-POINTS_LEDGER-7_11%22%20transform%3D%22translate\(-23.82958984375%2C%20-11.5\)%22%3E%3Cg%3E%3Crect%20class%3D%22background%22%20style%3D%22%22%20x%3D%22-2%22%20y%3D%22-2.9999990463256836%22%20width%3D%2247.6591796875%22%20height%3D%2223%22%3E%3C%2Frect%3E%3Ctext%20y%3D%22-10.1%22%20style%3D%22%22%3E%3Ctspan%20class%3D%22text-outer-tspan%22%20x%3D%220%22%20y%3D%22-0.1em%22%20dy%3D%221.1em%22%3E%3Ctspan%20font-style%3D%22normal%22%20class%3D%22text-inner-tspan%22%20font-weight%3D%22normal%22%3Espends%3C%2Ftspan%3E%3C%2Ftspan%3E%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fg%3E%3C%2Fg%3E%3C%2Fg%3E%3C%2Fsvg%3E)

The critical design idea is that business ownership must be explicit. A transaction has a business, a customer, and a staff membership. The backend must verify that these relationships are consistent, not merely that each ID exists.

The schema will need careful constraints and indexes. For example, a transaction referencing a customer from another business must be rejected, even if both records exist.

You should also decide how customer phone numbers are treated: if the same phone can exist in different businesses, enforce uniqueness within a business rather than globally.

## 7. API specification: How the frontend communicates with the backend

The following endpoint list defines the initial API surface. It is the contract between the frontend and backend, even though both may be implemented in the same Next.js repository.

| Method   | Endpoint                    | Purpose                                         |
| -------- | --------------------------- | ----------------------------------------------- |
| `POST`   | `/api/auth/register`        | Create an account and business                  |
| `POST`   | `/api/auth/login`           | Authenticate a user                             |
| `POST`   | `/api/auth/logout`          | End the session                                 |
| `GET`    | `/api/auth/me`              | Get current user and active business context    |
| `GET`    | `/api/customers`            | List customers                                  |
| `POST`   | `/api/customers`            | Create a customer                               |
| `GET`    | `/api/customers/[id]`       | Get customer details                            |
| `PATCH`  | `/api/customers/[id]`       | Update customer details                         |
| `DELETE` | `/api/customers/[id]`       | Archive a customer, not erase history           |
| `GET`    | `/api/transactions`         | List transactions                               |
| `POST`   | `/api/transactions`         | Record a transaction                            |
| `GET`    | `/api/transactions/[id]`    | Get transaction details                         |
| `GET`    | `/api/rewards`              | List available rewards                          |
| `POST`   | `/api/rewards`              | Create a reward                                 |
| `PATCH`  | `/api/rewards/[id]`         | Update a reward                                 |
| `POST`   | `/api/redemptions`          | Redeem a reward                                 |
| `GET`    | `/api/redemptions`          | List redemptions                                |
| `GET`    | `/api/staff`                | List staff memberships                          |
| `POST`   | `/api/staff`                | Add an existing user to the business            |
| `PATCH`  | `/api/staff/[id]`           | Change an allowed membership property or status |
| `GET`    | `/api/reports/summary`      | Get dashboard metrics                           |
| `GET`    | `/api/reports/transactions` | Get filtered reporting data                     |

You may add endpoints for creating businesses, managing settings, or retrieving dropdown options as the design develops. The final endpoint contract should be documented before each module is implemented.

### Example: Creating a transaction

Request:

http

```
POST /api/transactions
Content-Type: application/json
```

JSON

```
{
  "customerId": "customer_123",
  "amount": "850.00",
  "service": "Manicure"
}
```

The authenticated session establishes who is making the request and which business context they are operating in. The client should not be allowed to assign an arbitrary `businessId`, staff identity, or points total to gain privileges.

Successful response, illustrated:

JSON

```
{
  "data": {
    "id": "txn_456",
    "customerId": "customer_123",
    "amount": "850.00",
    "pointsEarned": 8,
    "status": "COMPLETED"
  }
}
```

A successful creation should return HTTP `201 Created`. Validation errors might return `400 Bad Request` or `422 Unprocessable Content`, depending on your documented convention. An unauthenticated request should return `401 Unauthorized`; an authenticated user without permission should receive `403 Forbidden`; and a missing or inaccessible resource may return `404 Not Found`.

Choose a consistent error format and use it across the API. Do not return database stack traces or secrets to the browser.

## 8. Core business rules the system must enforce

These rules are more important than the visual design because they determine whether the application is trustworthy.

| Rule                 | Required behavior                                                                 |
| -------------------- | --------------------------------------------------------------------------------- |
| Business isolation   | Every business-owned resource is scoped to the authorized business.               |
| User permissions     | Every protected operation checks the user's current permissions on the server.    |
| Customer ownership   | A transaction cannot reference a customer belonging to another business.          |
| Points calculation   | Points are calculated by backend logic, not accepted from client input.           |
| Atomic redemption    | Deducting points and recording a redemption succeed or fail together.             |
| Duplicate prevention | Retrying a transaction or redemption cannot unintentionally repeat the operation. |
| Auditability         | Historical transactions and redemptions retain their original context.            |
| Reporting accuracy   | Metrics are calculated from authoritative database records.                       |
| Session security     | Logging out or revoking access prevents subsequent protected operations.          |
| Validation           | Invalid input is rejected before it can corrupt persisted data.                   |

### How the system should behave when something goes wrong

Consider these scenarios:

* An employee changes a customer ID in the browser: The backend checks ownership and refuses unauthorized access.

* A database connection fails during redemption: No partial points deduction or incomplete redemption is committed.

* A user clicks Submit twice: The interface prevents accidental double submission, and the backend also handles retries safely.

* A staff member is deactivated: They lose access to the relevant business, but their historical transactions remain available to authorized users.

* A business has no transactions: The dashboard displays zero values and helpful empty states rather than fake data or errors.

* A customer's points are insufficient: The redemption is rejected without changing the balance.

* An employee tries to open an owner-only endpoint directly: The server denies the request even if the employee manually enters the endpoint URL.

These scenarios should become part of your test suite, not just things you remember to check manually.

## 9. Non-functional requirements

These describe the quality of the system, not individual features.

Security

* Store passwords using an appropriate password-hashing algorithm.

* Use secure session cookies and appropriate CSRF protections for cookie-based authentication.

* Validate and authorize every sensitive request.

* Protect against cross-tenant access and common web vulnerabilities.

* Keep secrets in server-side environment variables.

Reliability

* Use database transactions for operations that update multiple related records.

* Add appropriate database constraints and handle failures consistently.

* Use migrations to manage schema changes.

* Back up production data through the hosting provider's available backup mechanisms.

Performance

* Paginate large lists.

* Index frequently queried columns where justified.

* Aggregate reports in SQL instead of retrieving all records.

* Avoid unnecessary database queries and repeated frontend requests.

Accessibility and usability

* Make forms keyboard accessible.

* Associate labels with inputs.

* Provide visible focus states and readable contrast.

* Use clear validation messages.

* Make core workflows usable on mobile screens.

Maintainability

* Keep route handlers focused.

* Reuse business logic instead of duplicating it.

* Use TypeScript types and runtime validation together.

* Document important architecture decisions.

* Keep business logic testable without rendering the frontend.

You do not need enterprise-scale infrastructure for this project. You do need to demonstrate good engineering habits at an appropriate scale.

## 10. What the codebase should look like

A reasonable Next.js project structure would be:

```
business-hub/
├── app/
│   ├── (auth)/
│   │   ├── login/
│   │   └── register/
│   ├── (dashboard)/
│   │   ├── dashboard/
│   │   ├── customers/
│   │   ├── transactions/
│   │   ├── rewards/
│   │   ├── redemptions/
│   │   ├── staff/
│   │   ├── reports/
│   │   └── settings/
│   └── api/
│       ├── auth/
│       ├── customers/
│       ├── transactions/
│       ├── rewards/
│       ├── redemptions/
│       ├── staff/
│       └── reports/
├── components/
│   ├── ui/
│   ├── layout/
│   ├── forms/
│   └── tables/
├── features/
│   ├── customers/
│   ├── transactions/
│   ├── rewards/
│   └── reports/
├── lib/
│   ├── auth/
│   ├── db/
│   ├── validation/
│   └── errors/
├── services/
├── prisma/
│   ├── schema.prisma
│   └── migrations/
├── tests/
├── public/
├── PROJECT_CONTRACT.md
├── LEARNING_LOG.md
└── README.md
```

This is a starting structure, not a requirement to create every folder immediately.

* `app/` contains pages and API route handlers.

* `components/` contains reusable UI components.

* `features/` groups feature-specific frontend components and helpers.

* `lib/` contains shared infrastructure, such as database and authentication utilities.

* `services/` contains reusable server-side business operations.

* `prisma/` contains the database schema and migrations.

* `tests/` contains automated tests.

Keep server-only code out of client bundles. For example, database access, password verification, and privileged business operations must never be exposed as browser-side code.

## 11. The implementation order

Although the specification describes the complete system, you should build it in this order:

## 1

Build the smallest vertical slice

One page, one API endpoint, one database table, and one successful request. Understand the whole path before adding more modules.

## 2

Add the database and customer module

Learn SQL, relationships, migrations, validation, CRUD, and error handling.

## 3

Add authentication and business ownership

Protect the data before building sensitive workflows.

## 4

Add transactions and rewards

Practice business rules, consistency, and concurrency handling.

## 5

Finish the UI, reports, tests, and deployment

Turn the working backend into a polished product you can demonstrate and maintain.

## 12. What is deliberately not decided yet?

A real specification should distinguish confirmed requirements from decisions that still need to be made.

Before implementing the relevant features, define:

* Loyalty policy: How many points does a customer earn, and what happens after a refund or transaction reversal?

* Customer identity: Is a phone number unique within a business? Can duplicate customer records exist?

* Staff invitations: Will the first version add existing users, or send invitations by email?

* Business context: Can a user belong to multiple businesses, and how will they switch between them?

* Transaction lifecycle: Can transactions be cancelled, reversed, or refunded?

* Reporting periods: Which timezone defines a business day?

* Currency handling: Will each business have its own currency, and how will amounts be represented precisely?

* Reward lifecycle: Can a reward be deactivated while existing redemptions remain in the history?

You do not need to solve all of these on Day 1. Write down the decision before implementing the feature so the database, API, and UI all follow the same rules.

## 13. Final acceptance criteria

BusinessHub is ready for final delivery when a reviewer can perform the following complete journey:

1. Register a business and owner account.

2. Sign in and reach the dashboard.

3. Add a customer and view their profile.

4. Add an employee and verify the employee's permissions.

5. Record a transaction and see the correct points earned.

6. Redeem an eligible reward and see the correct remaining balance.

7. Verify that invalid redemptions and unauthorized actions are rejected.

8. Open the reports page and see accurate business-specific metrics.

9. Refresh the application and confirm that data persists.

10. Run the automated tests, inspect the API documentation, and follow the README to understand how the application is set up.

The reviewer should also be able to verify that a second business cannot access the first business's private records.

## The most important thing to understand

You are not building seven separate pages. You are building one system in which users, businesses, customers, transactions, and rewards are connected through explicit rules and relationships.

The frontend makes those capabilities accessible. The backend enforces the rules. The database preserves the truth. The tests prove that the system behaves correctly.

Your first implementation target should be the complete customer-creation flow: a user fills out a form, Next.js sends a request, the backend validates it, PostgreSQL stores the customer under the correct business, and the frontend displays the saved customer. Once you understand that flow end to end, you can apply the same engineering principles to the rest of BusinessHub.
