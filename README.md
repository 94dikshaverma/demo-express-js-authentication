# Demo Express.js Authentication (MVC Pattern)

This project is a simple demonstration of implementing user authentication using **Express.js**, **Mongoose**, and **JWT (JSON Web Tokens)**. It follows the **MVC (Model-View-Controller)** architecture to structure the application cleanly and make it scalable.

### Key Features:
- **User Registration**: Allows users to create an account with their email and password.
- **User Login**: Users can log in to the application using email and password.
- **JWT Authentication**: Secure authentication via JWT for managing user sessions.
- **Protected Routes**: Certain routes require authentication and can only be accessed by logged-in users.
- **Password Hashing**: User passwords are hashed securely using **bcrypt**.
- **MVC Architecture**: The application is structured using the **Model-View-Controller (MVC)** design pattern.

---

## Technologies Used

- **Backend**:
  - **Express.js** (web framework)
  - **Mongoose** (MongoDB ODM)
  - **JWT** (JSON Web Tokens for authentication)
  - **bcrypt** (for hashing passwords)
  - **dotenv** (for managing environment variables)

- **Architecture**:
  - **Model-View-Controller (MVC)** pattern:
    - **Model**: Defines the MongoDB schema and data structure (e.g., User model).
    - **Controller**: Handles incoming requests and business logic (e.g., registration, login).
    - **Service**: Contains core business logic such as JWT generation and authentication processes.

---

## Installation

Follow these steps to set up the project locally:

### 1. Clone the repository

```bash
git clone https://github.com/94dikshaverma/demo-express-js-authentication.git
cd demo-express-js-authentication
```

### 2. Install dependencies

Use **npm** or **yarn** to install the project dependencies.

```bash
npm install
```

Or if you're using **Yarn**:

```bash
yarn install
```

### 3. Set up environment variables

Create a `.env` file at the root of your project to store sensitive environment variables like your JWT secret and MongoDB URI:

```plaintext
JWT_SECRET=your_jwt_secret_key
MONGO_URI=mongodb://localhost:27017/your_database_name
```

### 4. Start the application

Run the app using the following command:

```bash
npm start
```

Your app will be running on `http://localhost:5000` by default.

---

## How It Works

### MVC Architecture
- **Model**: Mongoose models define the structure of your data. For example, the `User` model contains fields for `email` and `password`, and provides methods like `save()` and `findOne()` for interacting with the database.
  
- **Controller**: The controller functions handle the business logic. For example, the `AuthController` handles user registration, login, and token generation.

- **Service**: Services encapsulate the core logic of your application. For example, the `AuthService` contains the logic for generating JWT tokens and verifying user credentials.

### Authentication Flow:
1. **User Registration**: 
   - When a user registers, their email and password are validated, and their password is hashed using **bcrypt**.
   - The user data is saved to the MongoDB database, and a JWT token is generated and sent to the client for subsequent requests.

2. **User Login**: 
   - Upon login, the system validates the user credentials, compares the hashed password with the stored hash, and returns a JWT token upon successful login.

3. **Protected Routes**: 
   - Certain routes (e.g., user profile) are protected and only accessible if the user provides a valid JWT token.

---

# API Endpoints

## 1. **POST /authenticate**
Authenticate a user and issue a JWT token upon successful login.

### Request Body:
```json
{
  "email": "user@example.com",
  "password": "yourpassword"
}
```

### Response:
```json
{
  "token": "your.jwt.token"
}
```

### Status Codes:
- **200 OK**: Successful authentication.
- **400 Bad Request**: Invalid email or password.
- **401 Unauthorized**: Invalid credentials or missing token.

---

## 2. **POST /register**
Register a new user with their email and password.

### Request Body:
```json
{
  "email": "user@example.com",
  "password": "yourpassword"
}
```

### Response:
```json
{
  "message": "User registered successfully"
}
```

### Status Codes:
- **201 Created**: User successfully created.
- **400 Bad Request**: Missing or invalid input data.
- **409 Conflict**: User with the given email already exists.

---

## 3. **GET /**  
Retrieve a list of all users (admin-only or authorized users).

### Response:
```json
[
  {
    "id": "user_id_1",
    "email": "user1@example.com"
  },
  {
    "id": "user_id_2",
    "email": "user2@example.com"
  }
]
```

### Status Codes:
- **200 OK**: Successfully fetched users.
- **401 Unauthorized**: Missing or invalid authentication token.

---

## 4. **GET /current**
Get the currently authenticated user's details.

### Response:
```json
{
  "id": "current_user_id",
  "email": "current_user@example.com"
}
```

### Status Codes:
- **200 OK**: Successfully fetched current user's data.
- **401 Unauthorized**: Missing or invalid authentication token.

---

## 5. **GET /:id**
Get a specific user by their ID.

### Response:
```json
{
  "id": "user_id",
  "email": "user@example.com"
}
```

### Status Codes:
- **200 OK**: Successfully fetched user data.
- **404 Not Found**: User with the given ID not found.
- **401 Unauthorized**: Missing or invalid authentication token.

---

## 6. **PUT /:id**
Update a user's information (email, password, etc.).

### Request Body:
```json
{
  "email": "updateduser@example.com",
  "password": "newpassword"
}
```

### Response:
```json
{
  "message": "User updated successfully"
}
```

### Status Codes:
- **200 OK**: Successfully updated user data.
- **400 Bad Request**: Invalid input data.
- **404 Not Found**: User with the given ID not found.
- **401 Unauthorized**: Missing or invalid authentication token.

---

## 7. **DELETE /:id**
Delete a user by their ID.

### Response:
```json
{
  "message": "User deleted successfully"
}
```

### Status Codes:
- **200 OK**: Successfully deleted the user.
- **404 Not Found**: User with the given ID not found.
- **401 Unauthorized**: Missing or invalid authentication token.

---

### Notes:
- **Authentication & Authorization**: Many of the endpoints (like `GET /`, `PUT /:id`, `DELETE /:id`) will require the user to be authenticated using a **JWT token** passed in the `Authorization` header.
- **JWT Token**: When logging in via `/authenticate` or registering via `/register`, the response will return a **JWT token**. This token must be included in the `Authorization` header (e.g., `Authorization: Bearer your.jwt.token`) for accessing protected routes.

---

