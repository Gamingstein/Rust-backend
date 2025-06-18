# Rust Backend Starter

A modern, production-ready backend API boilerplate written in Rust, using [Axum](https://github.com/tokio-rs/axum), [SQLx](https://github.com/launchbadge/sqlx), and PostgreSQL. Features user authentication (JWT, email verification, password reset), role-based access, and modular architecture.

## Features

- User registration with email verification
- Secure login with JWT authentication (cookie & bearer)
- Password reset via email
- Role-based access control (Admin/User)
- User profile management (update name, password, role)
- Pagination for user listing
- Modular, testable code structure
- Async database access with SQLx
- Email sending via SMTP (lettre)
- Strong password hashing (Argon2)
- Input validation (validator)
- CORS, error handling, and tracing

## Project Structure

```
src/
  config.rs         # Environment config loader
  db.rs             # Database client & queries
  dtos.rs           # Data transfer objects (request/response)
  error.rs          # Error types & HTTP error responses
  handler/          # Route handlers (auth, users)
  mail/             # Email sending logic & templates
  middleware.rs     # Auth & role middleware
  models.rs         # Database models (User, UserRole)
  routes.rs         # Axum router setup
  utils/            # Password hashing, JWT utils
  main.rs           # Entrypoint
```

## Getting Started

### Prerequisites

- Rust (1.70+ recommended)
- PostgreSQL
- [cargo-watch](https://crates.io/crates/cargo-watch) (optional, for live reload)
- SMTP credentials for email sending

### Environment Variables

Create a `.env` file in the project root:

```
DATABASE_URL=postgres://user:password@localhost:5432/dbname
JWT_SECRET_KEY=your_jwt_secret
JWT_MAXAGE=60
SMTP_USERNAME=your@email.com
SMTP_PASSWORD=your_smtp_password
SMTP_SERVER=smtp.yourprovider.com
SMTP_PORT=587
```

### Database Setup

1. Create a PostgreSQL database.
2. Create the `users` table and `user_role` enum:

```sql
CREATE TYPE user_role AS ENUM ('admin', 'user');

CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name TEXT NOT NULL,
    email TEXT NOT NULL UNIQUE,
    password TEXT NOT NULL,
    role user_role NOT NULL DEFAULT 'user',
    verified BOOLEAN NOT NULL DEFAULT FALSE,
    verification_token TEXT,
    token_expires_at TIMESTAMP,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
```

### Running the Server

```bash
cargo run
```

Server will start on [http://localhost:8000](http://localhost:8000).

### API Endpoints

#### Auth

- `POST /api/auth/register` — Register new user
- `POST /api/auth/login` — Login (returns JWT in cookie & body)
- `GET /api/auth/verify?token=...` — Email verification
- `POST /api/auth/forgot-password` — Request password reset
- `POST /api/auth/reset-password` — Reset password

#### Users

- `GET /api/users/me` — Get current user (auth required)
- `GET /api/users/users?page=1&limit=10` — List users (admin only)
- `PUT /api/users/name` — Update name
- `PUT /api/users/role` — Update role (admin only)
- `PUT /api/users/password` — Change password

### Email Templates

HTML templates are in `src/mail/templates/`. Update as needed for branding.

### CORS

CORS is enabled for `http://localhost:3000` (frontend). Change in `main.rs` if needed.

## Development

- Hot reload: `cargo watch -x run`
- Logging/tracing enabled by default

## License

MIT

## Credits

- [Axum](https://github.com/tokio-rs/axum)
- [SQLx](https://github.com/launchbadge/sqlx)
- [lettre](https://github.com/lettre/lettre)
- [validator](https://github.com/Keats/validator)
