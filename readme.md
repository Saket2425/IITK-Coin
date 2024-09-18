# IITK-Coin

IITK-Coin is a web-based application designed to handle pseudo-currency transactions for IIT Kanpur students. The application provides APIs for user authentication, currency transactions, balance checking, and profile management. The project is built using Go and the [Gin Web Framework](https://github.com/gin-gonic/gin).

## Project Structure

- **Controllers**: Handles API requests and responses.
- **Middlewares**: Provides authentication and authorization logic.
- **Database**: Manages connections and interactions with the database (User, Account, Transaction tables).
- **Models**: Defines the data structures used throughout the application.
  
## Technologies Used

- **Golang**: Backend development
- **Gin**: HTTP web framework for building APIs
- **JWT**: Token-based authentication for secure endpoints
- **Database**: SQL-based database for managing user, account, and transaction data
- **Postman**: For API testing and interaction

## Endpoints

### User Authentication

1. **Sign Up a New User**
   - **Method**: `POST`
   - **Endpoint**: `/signup`
   - **Description**: Registers a new user with a roll number and password.
   - **Request Body**:
     ```json
     {
       "roll_no": "190460",
       "password": "pass"
     }
     ```

2. **Login an Existing User**
   - **Method**: `POST`
   - **Endpoint**: `/login`
   - **Description**: Authenticates a user and returns a JWT token.
   - **Request Body**:
     ```json
     {
       "roll_no": "190460",
       "password": "pass"
     }
     ```
   - **Response**:
     ```json
     {
       "token": "jwt_token_string"
     }
     ```

### Protected Routes (JWT Required)

For the following routes, a JWT token must be included in the request header:

```http
Authorization: Bearer <jwt_token>
```

#### Access Profile (Secret Page)

- **Method**: `GET`
- **Endpoint**: `/secretpage`
- **Description**: Access a protected route that shows the user's profile data.

#### Currency Transactions

##### Initialize an Account and Add Balance

- **Method**: `POST`
- **Endpoint**: `/init`
- **Description**: Initializes an account for a user or updates the balance if the account already exists.
- **Request Body**:
  ```json
  {
    "roll_no": "190460",
    "balance": 1000
  }
  ```
- **Response**: Account is created or updated with the balance.

##### Transfer Coins Between Users

- **Method**: `POST`
- **Endpoint**: `/transfer`
- **Description**: Transfers a specified amount of pseudo-coins from one roll number to another.
- **Request Body**:
  ```json
  {
    "from_roll_no": "190981",
    "to_roll_no": "190982",
    "amount": 1000
  }
  ```

##### Check Balance

- **Method**: `GET`
- **Endpoint**: `/balance`
- **Description**: Retrieves the balance of the logged-in user.
- **Request Body**:
  ```json
  {
    "roll_no": "190460"
  }
  ```
- **Response**:
  ```json
  {
    "roll_no": "190460",
    "balance": 5000
  }
  ```

## Running the Application

### Prerequisites

- Golang (Version 1.18 or higher)
- A running instance of your SQL database (PostgreSQL, MySQL, etc.)
- Postman (optional) for testing the APIs

### Steps

1. Clone the repository:
   ```bash
   git clone https://github.com/Saket2425/IITK-Coin.git
   cd iitk-coin
   ```

2. Install dependencies:
   ```bash
   go mod tidy
   ```

3. Set up the database:
   - Ensure your database is running and that the connection details are properly configured in the database package.

4. Run the application:
   ```bash
   go run main.go
   ```

   The application will start on `http://localhost:8080`.

### Testing APIs with Postman

1. Check if the server is running:
   - Send a GET request to `http://localhost:8080/check`. You should receive the response "good to go".

2. Register a new user:
   - POST `http://localhost:8080/signup`
   - Include the following in the request body:
     ```json
     {
       "roll_no": "190460",
       "password": "pass"
     }
     ```

3. Log in and get the token:
   - POST `http://localhost:8080/login`
   - Include the same request body as sign-up. The response will contain the JWT token.

4. Use the token to access protected routes:
   - Copy the token from the login response.
   - Use it in the Authorization header as a Bearer token for the `/balance`, `/transfer`, and `/secretpage` routes.

#### Example Request Headers

For protected routes like `/balance` or `/transfer`, include the JWT token in the header:

```http
Authorization: Bearer <jwt_token>
```

## Data Models

### User Model

```go
type User struct {
    RollNo   string `json:"roll_no"`
    Password string `json:"password"`
}
```

### Account Model

```go
type Account struct {
    Owner   string `json:"roll_no"`
    Balance int64  `json:"balance"`
}
```

### Transaction Model

```go
type Transaction struct {
    FromAccountID string `json:"from_roll_no"`
    ToAccountID   string `json:"to_roll_no"`
    Amount        int64  `json:"amount"`
}
```

