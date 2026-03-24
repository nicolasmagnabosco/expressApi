##Express API with MySQL and JWT

A RESTful API built with Express.js for managing courses and reviews in an online coding academy platform. Features user authentication, course listings, and a comprehensive review system.
--------------------------------------------------------------------------------------------------------------------------
## Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [API Endpoints](#api-endpoints)
- [Authentication](#authentication)
- [Project Structure](#project-structure)
- [Contributing](#contributing)
- [Support](#support)
- [License](#license)

## Features

✨ **Core Functionality:**

- 🔐 **User Authentication**: Register and login with JWT-based authentication
- 📚 **Course Management**: Browse available courses with detailed information
- ⭐ **Review System**: Create, read, update, and delete course reviews
- 🛡️ **Secure API**: Password hashing with bcryptjs, JWT token validation
- 🚀 **Vercel Ready**: Pre-configured for easy deployment to Vercel

## Tech Stack

- **Framework**: [Express.js](https://expressjs.com/) - Node.js web framework
- **Database**: [MySQL](https://www.mysql.com/) - Relational database
- **Authentication**: [jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken) - JWT token generation and verification
- **Security**: [bcryptjs](https://www.npmjs.com/package/bcryptjs) - Password hashing
- **Environment**: [dotenv](https://www.npmjs.com/package/dotenv) - Environment variable management

## Prerequisites

Before you begin, ensure you have the following installed:

- [Node.js](https://nodejs.org/) (v14 or higher)
- [npm](https://www.npmjs.com/) (comes with Node.js)
- [MySQL](https://www.mysql.com/) (v5.7 or higher)
- A code editor (VS Code recommended)

## Installation

1. **Clone the repository**

   ```bash
   git clone https://github.com/yourusername/expressApi.git
   cd expressApi
   ```

2. **Install dependencies**

   ```bash
   npm install
   ```

3. **Set up the database**

   ```bash
   mysql -u root -p < api/db/nickam_code_academy.sql
   ```

   Replace `root` with your MySQL username if different.

4. **Create a `.env` file** in the project root
   ```bash
   cp .env.example .env
   ```
   Or create `.env` manually with the required variables (see [Configuration](#configuration) section).

## Configuration

Create a `.env` file in the root directory with the following variables:

```env
# Database Configuration
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=your_password
DB_NAME=nickam_code_academy
DB_PORT=3306

# JWT Configuration
SECRET_KEY=your_secret_key_here

# Server Configuration
PORT=3000
NODE_ENV=development
```


## Usage

### Development

Start the development server with file watching enabled:

```bash
npm start
```

The server will run on `http://localhost:3000`

### Production

For production deployment, use:

```bash
node index.js
```

### Testing Endpoints

Use a tool like [Postman](https://www.postman.com/) or [curl](https://curl.se/) to test the API endpoints:

**Example: Register a new user**

```bash
curl -X POST http://localhost:3000/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "securepassword123"
  }'
```

**Response:**

```json
{
  "auth": true,
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

## API Endpoints

### Authentication (`/auth`)

| Method | Endpoint           | Description                          | Auth Required |
| ------ | ------------------ | ------------------------------------ | ------------- |
| POST   | `/auth/register`   | Register a new user                  | ❌            |
| POST   | `/auth/login`      | Login and receive JWT token          | ❌            |
| GET    | `/auth/restricted` | Test endpoint (requires valid token) | ✅            |

**Token Expiration**: JWT tokens expire after 1 hour

### Courses (`/cursos`)

| Method | Endpoint      | Description                 | Auth Required |
| ------ | ------------- | --------------------------- | ------------- |
| GET    | `/cursos`     | Get all available courses   | ❌            |
| GET    | `/cursos/:id` | Get a specific course by ID | ❌            |

### Reviews (`/resenas`)

| Method | Endpoint               | Description                     | Auth Required |
| ------ | ---------------------- | ------------------------------- | ------------- |
| GET    | `/resenas`             | Get all reviews                 | ❌            |
| GET    | `/resenas/:id`         | Get a specific review by ID     | ❌            |
| POST   | `/resenas`             | Create a new review             | ✅            |
| GET    | `/resenas/usuario/all` | Get all reviews by current user | ✅            |
| PUT    | `/resenas/:id`         | Update a review                 | ✅            |
| DELETE | `/resenas/:id`         | Delete a review                 | ✅            |

## Authentication

### How Authentication Works

1. **Register**: Create a new account with email and password
2. **Login**: Authenticate with credentials to receive a JWT token
3. **Authorize**: Include the token in the `Authorization` header for protected endpoints

### Using the JWT Token

Include the token in the `Authorization` header of your requests:

```bash
curl -X GET http://localhost:3000/resenas/usuario/all \
  -H "Authorization: Bearer your_jwt_token_here"
```

**Token Format**: `Bearer <token_here>`

### Token Features

- ⏱️ **Expires in**: 1 hour
- 🔒 **Contains**: User ID
- 🛡️ **Validated on**: Every protected endpoint request

## Project Structure

```
expressApi/
├── api/
│   ├── controllers/          # Business logic for each resource
│   │   ├── auth.controller.js
│   │   ├── cursos.controller.js
│   │   └── reseñas.controller.js
│   ├── db/                   # Database connection and schema
│   │   ├── db.js
│   │   └── nickam_code_academy.sql
│   ├── middlewares/          # Express middleware
│   │   └── auth.middleware.js
│   └── routes/               # API route definitions
│       ├── auth.router.js
│       ├── cursos.router.js
│       └── reseñas.router.js
├── index.js                  # Application entry point
├── package.json              # Project dependencies and scripts
├── vercel.json               # Vercel deployment configuration
├── .env                      # Environment variables (not in repo)
└── README.md                 # This file
```

## Contributing

We welcome contributions! Here's how you can help:

1. **Fork the repository** on GitHub
2. **Create a feature branch**
   ```bash
   git checkout -b feature/amazing-feature
   ```
3. **Make your changes** with clear, descriptive commits
4. **Test your changes** thoroughly
5. **Push to your branch**
   ```bash
   git push origin feature/amazing-feature
   ```
6. **Open a Pull Request** with a description of your changes

### Coding Standards

- Use consistent naming conventions (camelCase for variables, UPPER_CASE for constants)
- Write meaningful commit messages
- Test all API endpoints before submitting a PR
- Keep controller logic clean and maintainable
- Add comments for complex business logic

## Support

### Getting Help

- 📖 **Documentation**: Check the [API Endpoints](#api-endpoints) section above
- 📧 **Email**: Contact me.

### Common Issues

**Issue**: "Error: Can't find module"

- **Solution**: Run `npm install` to ensure all dependencies are installed

**Issue**: "ECONNREFUSED" (database connection error)

- **Solution**: Verify MySQL is running and `.env` database credentials are correct

**Issue**: "Invalid token" error

- **Solution**: Ensure token is properly formatted in Authorization header as `Bearer <token>`

## License

This project is licensed under the ISC License - see the LICENSE file for details.

---
