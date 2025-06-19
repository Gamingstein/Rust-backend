# Rust Backend Starter

A minimal backend API boilerplate written in Rust, using [Actix Web](https://github.com/actix/actix-web). This project demonstrates a simple HTTP server with basic endpoints.

## Features

- Simple HTTP server using Actix Web
- Example GET and POST endpoints
- Easy to extend for your own backend logic

## Project Structure

```
src/
  main.rs           # Entrypoint and route definitions
```

## Getting Started

### Prerequisites

- Rust (edition 2024)
- [cargo-watch](https://crates.io/crates/cargo-watch) (optional, for live reload)

### Running the Server

```bash
cargo run
```

Server will start on [http://localhost:8080](http://localhost:8080).

### API Endpoints

#### Root

- `GET /` — Returns "Hello world!"

#### Echo

- `POST /echo` — Returns the request body as the response

#### Hey

- `GET /hey` — Returns "Hey there!"

## Development

- Hot reload: `cargo watch -x run`

## License

MIT

## Credits

- [Actix Web](https://github.com/actix/actix-web)
