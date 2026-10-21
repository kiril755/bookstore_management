# Bookstore Management API

A simple REST API for managing a collection of books, written in Go. It supports full CRUD operations (create, read, update, delete) and stores data in a MySQL database.

This is a learning/pet project built to practice building a backend service in Go.

## Tech stack

- **[Go](https://go.dev/)** — language
- **[Gorilla Mux](https://github.com/gorilla/mux)** — HTTP router
- **[GORM v1](https://github.com/jinzhu/gorm)** — ORM
- **[MySQL](https://www.mysql.com/)** — database

## Requirements

- Go 1.26 or newer
- A running MySQL server (default: `localhost:3306`)

## Getting started

### 1. Clone the repository

```bash
git clone https://github.com/kiril755/bookstore_management.git
cd bookstore_management
```

### 2. Create the database

The app connects to a database named `simpletest`. Create it in MySQL:

```bash
mysql -uroot -e "CREATE DATABASE IF NOT EXISTS simpletest;"
```

The `books` table is created automatically on startup via GORM's `AutoMigrate`, so you don't need to create any tables yourself.

### 3. Configure the database connection

The connection string is in `pkg/config/app.go`:

```go
gorm.Open("mysql", "root:@tcp(127.0.0.1:3306)/simpletest?charset=utf8&parseTime=True&loc=Local")
```

By default it connects as user `root` with **no password**. If your MySQL root user has a password, update the string to `root:YOURPASSWORD@tcp(...)`. If the password contains special characters (like `@`), URL-encode them (`@` becomes `%40`).

### 4. How to run

```bash
go run main.go
```

The server starts on **http://localhost:9010**.

## API endpoints

Base path: `/book/`

| Method | Endpoint          | Description              | Request body |
|--------|-------------------|--------------------------|--------------|
| GET    | `/book/`          | Get all books            | —            |
| GET    | `/book/{bookId}`  | Get a single book by ID  | —            |
| POST   | `/book/`          | Create a new book        | JSON (see below) |
| PUT    | `/book/{bookId}`  | Update an existing book  | JSON (fields to change) |
| DELETE | `/book/{bookId}`  | Delete a book by ID      | —            |

### Book object

```json
{
  "name": "The Go Programming Language",
  "author": "Donovan & Kernighan",
  "publication": "Addison-Wesley"
}
```

Each book also has auto-managed fields from GORM: `ID`, `CreatedAt`, `UpdatedAt`, `DeletedAt`.

## Example requests

Create a book:

```bash
curl -X POST http://localhost:9010/book/ \
  -H "Content-Type: application/json" \
  -d '{"name":"The Go Programming Language","author":"Donovan & Kernighan","publication":"Addison-Wesley"}'
```

Get all books:

```bash
curl http://localhost:9010/book/
```

Get one book:

```bash
curl http://localhost:9010/book/1
```

Update a book:

```bash
curl -X PUT http://localhost:9010/book/1 \
  -H "Content-Type: application/json" \
  -d '{"author":"Alan Donovan and Brian Kernighan"}'
```

Delete a book:

```bash
curl -X DELETE http://localhost:9010/book/1
```
