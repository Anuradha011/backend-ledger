
# 💳 Backend Ledger System

### A Secure and Transaction-Based Financial Backend Application

![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Express.js](https://img.shields.io/badge/Express.js-000000?style=for-the-badge&logo=express&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Mongoose](https://img.shields.io/badge/Mongoose-880000?style=for-the-badge&logo=mongoose&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)

## 📌 Overview

**Backend Ledger** is a Node.js and Express.js-based financial transaction management system designed to manage user accounts, maintain transaction records, and process secure money transfers.

The application follows a ledger-based architecture where every financial transaction generates corresponding debit and credit entries.

Instead of directly modifying account balances, the system calculates the current balance using ledger entries. This approach provides better transaction traceability, consistency, and financial record management.

The project also implements JWT-based authentication, password hashing, idempotency handling, MongoDB transactions, and automated email notifications.

## 🎯 Project Objectives

- Build a secure backend authentication system.
- Manage multiple user accounts.
- Implement debit and credit transaction processing.
- Maintain immutable financial ledger records.
- Prevent duplicate transactions using idempotency keys.
- Calculate account balances dynamically.
- Implement transactional consistency using MongoDB sessions.
- Send automated email notifications for important activities.

## ✨ Key Features

### 1. User Authentication
- User registration and login.
- Password hashing using bcrypt.
- JWT-based authentication.
- Secure logout functionality.
- Token blacklist management.
- Authentication middleware for protected routes.

### 2. Account Management
- Create financial accounts for authenticated users.
- Retrieve all accounts belonging to a user.
- Check individual account balances.
- Maintain account status:
  - ACTIVE
  - FROZEN
  - CLOSED
- Default account currency: INR.

### 3. Transaction Management
- Transfer funds between accounts.
- Implement debit and credit ledger entries.
- Validate sender and receiver accounts.
- Check account status before processing transactions.
- Prevent transactions when the sender has insufficient balance.
- Track transaction statuses:
  - PENDING
  - COMPLETED
  - FAILED
  - REVERSED

### 4. Ledger Management
- Maintain separate debit and credit entries.
- Generate balances using MongoDB aggregation.
- Associate every ledger entry with its transaction.
- Prevent modification and deletion of existing ledger records.
- Maintain a traceable financial transaction history.

### 5. Idempotency Handling
- Every transaction requires a unique idempotency key.
- Prevent duplicate transaction processing.
- Identify previously processed requests.
- Handle pending, completed, failed, and reversed transaction states.

### 6. Email Notifications
- Send registration confirmation emails.
- Send successful transaction notifications.
- Integrate Gmail using Nodemailer and OAuth2 authentication.

## 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| Node.js | JavaScript runtime environment |
| Express.js | Backend framework and REST API development |
| MongoDB | NoSQL database |
| Mongoose | MongoDB object data modeling |
| JavaScript | Application logic |
| JWT | Authentication and authorization |
| bcryptjs | Password hashing |
| Cookie Parser | Reading authentication cookies |
| Dotenv | Environment variable management |
| Nodemailer | Email notifications |
| MongoDB Transactions | Atomic database operations |

## 🏗️ Project Architecture

The application follows a modular backend architecture.

```text
backend-ledger/
│
├── src/
│   │
│   ├── config/
│   │   └── db.js
│   │
│   ├── controllers/
│   │   ├── auth.controller.js
│   │   ├── account.controller.js
│   │   └── transaction.controller.js
│   │
│   ├── middleware/
│   │   └── auth.middleware.js
│   │
│   ├── models/
│   │   ├── user.model.js
│   │   ├── account.model.js
│   │   ├── transaction.model.js
│   │   ├── ledger.model.js
│   │   └── blackList.model.js
│   │
│   ├── routes/
│   │   ├── auth.routes.js
│   │   ├── account.routes.js
│   │   └── transaction.routes.js
│   │
│   ├── services/
│   │   └── email.service.js
│   │
│   └── app.js
│
├── server.js
├── package.json
├── package-lock.json
├── .gitignore
└── README.md
```

## ⚙️ Installation and Setup

### Prerequisites

Make sure you have installed:

- Node.js
- npm
- MongoDB
- Git

### Step 1: Clone the Repository

```bash
git clone https://github.com/Anuradha011/backend-ledger.git
```

### Step 2: Navigate to Project Directory

```bash
cd backend-ledger
```

### Step 3: Install Dependencies

```bash
npm install
```

### Step 4: Configure Environment Variables

Create a `.env` file in the root directory.

```env
PORT=3000

MONGO_URI=your_mongodb_connection_string

JWT_SECRET=your_secret_key

EMAIL_USER=your_email@gmail.com

CLIENT_ID=your_google_oauth_client_id

CLIENT_SECRET=your_google_oauth_client_secret

REFRESH_TOKEN=your_google_oauth_refresh_token
```

**Important:** Never upload your `.env` file or expose your database credentials, JWT secret, or OAuth credentials on GitHub.

### Step 5: Start the Application

For development:

```bash
npm run dev
```

For production:

```bash
npm start
```

The server runs on:

```text
http://localhost:3000
```

Health check:

```http
GET /
```

Expected response:

```text
Ledger Service is up and running
```

## 🔗 API Documentation

Base URL:

```text
http://localhost:3000
```

### 1. Authentication APIs

| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/auth/register` | Register a new user |
| POST | `/api/auth/login` | Authenticate an existing user |
| POST | `/api/auth/logout` | Logout and blacklist token |

**User Registration**

```http
POST /api/auth/register
```

Request Body:

```json
{
  "name": "Anuradha",
  "email": "anu@example.com",
  "password": "password123"
}
```

**User Login**

```http
POST /api/auth/login
```

Request Body:

```json
{
  "email": "anu@example.com",
  "password": "password123"
}
```

### 2. Account APIs

| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/accounts/` | Create a new account |
| GET | `/api/accounts/` | Get logged-in user's accounts |
| GET | `/api/accounts/balance/:accountId` | Retrieve account balance |

These routes require authentication.

### 3. Transaction APIs

| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/transactions/` | Transfer funds between accounts |
| POST | `/api/transactions/system/initial-funds` | Create initial funds transaction |

The normal transaction endpoint requires user authentication. The initial-funds endpoint requires system-user authentication.

**Money Transfer Example**

```http
POST /api/transactions/
```

Request Body:

```json
{
  "fromAccount": "SENDER_ACCOUNT_ID",
  "toAccount": "RECEIVER_ACCOUNT_ID",
  "amount": 500,
  "idempotencyKey": "transfer-unique-001"
}
```

Example successful response:

```json
{
  "message": "Transaction completed successfully",
  "transaction": {
    "status": "PENDING"
  }
}
```

Note: The transaction response contains the transaction document created during processing. The database status is updated to COMPLETED before the successful response is returned.

## 🔄 Transaction Processing Workflow

One of the most important parts of this project is its ledger-based transaction processing mechanism.

The transaction processing workflow follows these steps:

1. Validate sender account, receiver account, amount, and idempotency key.
2. Check whether the transaction already exists.
3. Verify that both accounts are ACTIVE.
4. Calculate the sender's current balance from ledger entries.
5. Validate sufficient balance.
6. Create a transaction with PENDING status.
7. Generate a DEBIT ledger entry for the sender.
8. Generate a CREDIT ledger entry for the receiver.
9. Update transaction status to COMPLETED and commit the MongoDB transaction.
10. Send an email notification.

### Transaction Flow Diagram

```text
          Client Request
                |
                v
       Validate Request Data
                |
                v
      Check Idempotency Key
                |
                v
       Validate Both Accounts
                |
                v
       Check Account Status
                |
                v
       Calculate Sender Balance
                |
                v
       Check Sufficient Balance
                |
                v
       Create PENDING Transaction
                |
                v
        Create DEBIT Entry
                |
                v
       Create CREDIT Entry
                |
                v
       Update Transaction Status
                |
                v
        Commit DB Transaction
                |
                v
       Send Email Notification
                |
                v
        Transaction Completed
```

## 📊 Ledger Balance Calculation

The system does not directly update the account balance after every transaction.

Instead, it calculates the balance using debit and credit ledger entries.

**Balance Formula:**

```text
Account Balance = Total Credits - Total Debits
```

Example:

| Transaction Type | Amount |
|---|---:|
| Initial Credit | ₹10,000 |
| Credit | ₹2,000 |
| Debit | ₹3,000 |
| Debit | ₹1,000 |

Final Balance:

```text
10000 + 2000 - 3000 - 1000 = ₹8,000
```

MongoDB aggregation is used to calculate the balance dynamically.

## 🔐 Security Implementation

The project includes several security-related mechanisms:

- Password hashing with bcryptjs.
- JWT-based authentication.
- Protected routes using authentication middleware.
- Token blacklist verification during authentication.
- System-user authorization for initial-funds transactions.
- Environment variables for sensitive configuration.
- Immutable ledger entries to protect financial records.
- Idempotency keys to prevent duplicate transaction processing.

## 🗄️ Database Models

The application uses MongoDB with Mongoose schemas.

| Model | Responsibility |
|---|---|
| User | Stores user details and hashed passwords |
| Account | Stores account ownership, status, and currency |
| Transaction | Stores transfer details and transaction status |
| Ledger | Stores debit and credit entries |
| Blacklist | Stores invalidated authentication tokens |

### Database Relationships

```text
        USER
         |
         | 1 : Many
         |
      ACCOUNT
         |
         | 1 : Many
         |
       LEDGER
         |
         | Many : 1
         |
    TRANSACTION
```

## 📈 Future Enhancements

Potential improvements for the project include:

- Transaction history and filtering APIs.
- Pagination and sorting for transaction records.
- Account-to-account transfer receipts.
- Transaction reversal and refund functionality.
- Rate limiting and request validation.
- Role-based access control.
- Comprehensive API documentation using Swagger.
- Automated unit and integration testing.
- Docker-based deployment.
- Production monitoring and structured logging.

## 🎓 Learning Outcomes

Through this project, I explored and implemented:

- RESTful API development using Express.js.
- Authentication and authorization using JWT.
- MongoDB schema design using Mongoose.
- Financial ledger architecture.
- Debit and credit accounting concepts.
- MongoDB sessions and transactional operations.
- Idempotency in financial transactions.
- Middleware-based request authentication.
- Email integration using Nodemailer.
- Modular backend application architecture.

## 👩‍💻 Author

**Anuradha**

GitHub: [@Anuradha011](https://github.com/Anuradha011)

Project Repository: [Backend Ledger](https://github.com/Anuradha011/backend-ledger)

---

⭐ If you find this project interesting, consider giving the repository a star!

**Built with Node.js, Express.js, MongoDB, and JavaScript.**
