version: '3.8'

services:
  analytics-app:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "3000:3000"  # Forward host port 3000 to container port 3000
    environment:
      - DB_HOST=postgres
      - DB_PORT=5432
      - DB_USER=myuser
      - DB_PASSWORD=mypassword
      - DB_NAME=mydatabase
    depends_on:
      - postgres

  postgres:
    image: postgres:13
    restart: always
    environment:
      POSTGRES_USER: myuser
      POSTGRES_PASSWORD: mypassword
      POSTGRES_DB: mydatabase
    ports:
      - "5432:5432"  # Forward host port 5432 to container port 5432
