# Finance Management System (FMS) Documentation

## Table of Contents

1. [Introduction](#introduction)
2. [Features](#features)
3. [Technology Stack](#technology-stack)
4. [Getting Started](#getting-started)
    1. [Installation](#installation)
    2. [Running the Application](#running-the-application)
5. [Project Structure](#project-structure)
6. [User Guide](#user-guide)
    1. [Dashboard](#dashboard)
    2. [Managing Accounts](#managing-accounts)
    3. [Transactions](#transactions)
    4. [Categories](#categories)
    5. [CSV Imports](#csv-imports)
7. [API Documentation](#api-documentation)
    1. [Authentication](#authentication)
    2. [Account Management](#account-management)
    3. [Transaction Management](#transaction-management)
    4. [Category Management](#category-management)
8. [FAQ](#faq)
9. [Support](#support)

## Introduction

Welcome to the Finance Management System (FMS) - a powerful SaaS solution designed to help individuals and businesses manage their finances efficiently. This documentation provides a comprehensive guide to using and developing the FMS platform.

## Features

- **Dashboard**: Overview of financial status.
- **Account Management**: Create and manage multiple financial accounts.
- **Transaction Tracking**: Record and categorize income and expenses.
- **Categories**: Organize transactions into categories.
- **CSV Imports**: Import transactions from CSV files.
- **API Access**: Integrate with external systems through RESTful APIs.

## Technology Stack

- Frontend: Next.js, Tailwind CSS, TypeScript
- Backend: Node.js, Next.js API Routes, Hono.js
- Database: Neon Database (PostgreSQL), Drizzle ORM
- Authentication: Clerk
- Deployment: Vercel

## Getting Started

### Installation

1. Clone the repository:

```bash
git clone https://github.com/your-username/fms.git
cd fms
```

2. Install dependencies::

```bash
bun add
```

3. Set up the database

- Create a new database in your SQL server (PostgreSQL, MySQL, etc.).
- Update the database configuration in the .env file:

```bash
DATABASE_URL=your_database_url
```

4. Run database migrations:

```bash
bun run db:migrate
```

5. Configure Clerk for authentication:

- Sign up for a Clerk account and create a new application.
- Update the Clerk configuration in the .env file

```bash
CLERK_PUBLISHABLE_KEY=<your_publishable_key>
CLERK_SECRET_KEY=<your_clerk_secret_key>
NEXT_PUBLIC_CLERK_SIGN_IN_URL=<your_clerk_sign-in_url>
NEXT_PUBLIC_CLERK_SIGN_UP_URL=<your_clerk_sign-up_url>
```

### Running the Application

1. Start the development server:

```bash
bun run dev
```

2. Open your browser and navigate to <http://localhost:3000>.

## Project Structure

```bash
finance/
├── components/            # React components
├── app/                   # Next.js pages
│   ├── api/               # API routes
│   ├── (auth)             # auth group
│   │   ├── sign-in/       # sign-in routes
│   │   └── sign-up/       # sign-up routes
│   ├── (dashboard)        # Dashboard page
│   │   ├── accounts/      # Accounts pages
│   │   ├── categories/    # Categories pages
│   │   ├── transactions/  # Transactions pages
│   │   ├── layout.tsx
│   │   └── page.tsx
│   ├── globals.css
│   └── layout.tsx
├── db/
├── drizzle/
├── features/
├── hooks/
├── providers/
├── scripts/
├── public/                # Static files
├── styles/                # Tailwind CSS styles
├── lib/                   # Utility functions
├── .env                   # Environment variables
├── drizzle.config.ts      # Drizzle ORM configuration
├── middleware.ts          # Clerk middleware
├── next.config.js         # Next.js configuration
├── tsconfig.json          # TypeScript configuration
└── package.json           # NPM dependencies and scripts
```

## User Guide

### Dashboard

- Navigate to the dashboard to get an overview of your financial status.
- View summaries of accounts, transactions, and budgets.
- Access quick links to different sections.

### Managing Accounts

1. Navigate to the "Accounts" section.
2. Click "Add Account" to create a new financial account.
3. Fill in the account details such as account name, type, and initial balance.
4. Click "Save" to add the account.

### Transactions

1. Navigate to the "Transactions" section.
2. Click "Add Transaction" to record a new transaction.
3. Enter the transaction details including date, description, category, and amount.
4. Select the account to which this transaction belongs.
5. Click "Save" to record the transaction.

### Categories

1. Navigate to the "Categories" section.
2. Click "Add Category" to create a new category.
3. Enter the category name and description.
4. Click "Save" to add the category.

### CSV Imports

1. Navigate to the "Transactions" section.
2. Click "Import CSV" to import transactions from a CSV file.
3. Select the CSV file and map the columns to the appropriate fields.
4. Click "Upload" to import the transactions.

## API Documentation

### Account Management

### Transaction Management

Create Transaction

`Endpoint: /transactions`

Method: POST

Description: Creates a new transaction.

Request Body:

- `date` (string, required): Date of the transaction (format: yyyy-MM-dd).
- `categoryId` (string, required): ID of the category for the transaction.
- `payee` (string, required): Payee or recipient of the transaction.
- `amount` (number, required): Amount of the transaction.
- `notes` (string, optional): Additional notes or description of the transaction.
- `accountId` (string, required): ID of the account for which the transaction is recorded.

Request:

```bash
POST /transactions
Content-Type: application/json

{
  "date": "2023-05-15",
  "categoryId": "456",
  "payee": "Supermarket",
  "amount": 50.00,
  "notes": "Weekly grocery shopping",
  "accountId": "123"
}
```

Request:

```bash
{
  "data": {
    "id": "1",
    "date": "2023-05-15",
    "categoryId": "456",
    "payee": "Supermarket",
    "amount": 50.00,
    "notes": "Weekly grocery shopping",
    "accountId": "123"
  }
}
```

Status Codes:

- `201 Created`: Transaction created successfully.
- `400 Bad Request`: Invalid request body or missing required fields.
- `401 Unauthorized`: Authentication credentials not provided or invalid.

Get Transactions

`Endpoint: /transactions`

Method: `GET`

Description: Fetches transactions based on optional query parameters such as `from`, `to`, and `accountId`.

Parameters:

- `from` (optional): Start date of the transactions (format: yyyy-MM-dd).
- `to` (optional): End date of the transactions (format: yyyy-MM-dd).
- `accountId` (optional): ID of the account for which transactions are to be fetched.

```bash
GET /transactions?from=2023-01-01&to=2023-12-31&accountId=123
```

```bash
{
  "data": [
    {
      "id": "1",
      "date": "2023-05-15",
      "category": "Groceries",
      "categoryId": "456",
      "payee": "Supermarket",
      "amount": 50.00,
      "notes": "Weekly grocery shopping",
      "account": "Checking Account",
      "accountId": "123"
    },
    {
      "id": "2",
      "date": "2023-05-16",
      "category": "Utilities",
      "categoryId": "789",
      "payee": "Electricity Company",
      "amount": 100.00,
      "notes": "Electricity bill payment",
      "account": "Savings Account",
      "accountId": "456"
    }
  ]
}
```

Status Codes:

- `200 OK`: Successful retrieval of transactions.
- `400 Bad Request`: Invalid query parameters provided.
- `401 Unauthorized`: Authentication credentials not provided or invalid.

### Category Management

Get Categories

Endpoint: `/categories`

Method: `GET`

Description: Fetches categories associated with the authenticated user.

Response

```bash
{
  "data": [
    {
      "id": "1",
      "name": "Groceries"
    },
    {
      "id": "2",
      "name": "Utilities"
    }
  ]
}
```

Status Codes:

- `200 OK`: Successful retrieval of transactions.
- `400 Bad Request`: Invalid query parameters provided.
- `401 Unauthorized`: Authentication credentials not provided or invalid.

Get Category by ID

Endpoint: `/categories/:id`

Method: `GET`

Description: Fetches a category by its ID.

Response

```bash
{
  "data": {
    "id": "1",
    "name": "Groceries"
  }
}
```

Status Codes:

- `200 OK`: Successful retrieval of the category.
- `400 Bad Request`: Invalid request parameters.
- `401 Unauthorized`: Authentication credentials not provided or invalid.
- `404 Not Found`: Category with the specified ID not found.

Create Category

Endpoint: `/categories`

Method: `POST`

Description: Creates a new category.

Request Body:

- name (string, required): Name of the category.

Response:

```bash
{
  "data": {
    "id": "3",
    "name": "Entertainment"
  }
}
```

Status Codes:

- `201 Created:` Category created successfully.
- `400 Bad Request:` Invalid request body or missing required fields.
- `401 Unauthorized:` Authentication credentials not provided or invalid.

Bulk Delete Categories

Endpoint: `/categories/bulk-delete`

Method: `POST`

Description: Deletes multiple categories in bulk.

Request Body:

```bash
{
  "ids": ["1", "2"]
}
```

Response:

```bash
{
  "data": [
    {
      "id": "1"
    },
    {
      "id": "2"
    }
  ]
}
```

Status Codes:

- `201 Created:` Category created successfully.
- `400 Bad Request:` Invalid request body or missing required fields.
- `401 Unauthorized:` Authentication credentials not provided or invalid.

Update Category

Endpoint: `/categories/:id`

Method: `PATCH`

Description: Updates an existing category.

Parameters:

- `id` (string, required): ID of the category to update.

Request Body:

- `name` (string, required): Updated name of the category.

Response:

```bash
{
  "data": {
    "id": "1",
    "name": "Groceries"
  }
}
```

Status Codes:

- `200 OK`: Category updated successfully.
- `400 Bad Request`: Invalid request parameters or missing required fields.
- `401 Unauthorized`: Authentication credentials not provided or invalid.
- `404 Not Found`: Category with the specified ID not found.

Delete Category

Endpoint: `/categories/:id`

Method: `DELETE`

Description: Deletes a category by its ID.

Parameters:

- `id` (string, required): ID of the category to delete.

Response:

```bash
{
  "data": {
    "id": "1"
  }
}
```

Status Codes:

- `200 OK`: Category deleted successfully.
- `400 Bad Request`: Invalid request parameters.
- `401 Unauthorized`: Authentication credentials not provided or invalid.
- `404 Not Found`: Category with the specified ID not found.

## Images

Sign-up page

![Sign-up](https://i.ibb.co/kmMJj2c/sign-up.png)

Sign-in Page

![Sign-in](https://i.ibb.co/qMd76Zr/sign-in-page.png)

Overview Page

![Overview pages](https://i.ibb.co/Wf1KQCF/01.png)

Transactions Page

![Transaction pages](https://i.ibb.co/9NfnrDq/transactions-page.png)

Account Page

![Account page](https://i.ibb.co/dcRVRPy/account-page.png)

Categories Page

![Categories page](https://i.ibb.co/nnb7fsP/categories-page.png)