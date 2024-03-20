# Run this application

## Required configuration

Before running the project, you must create a `docker-compose.yaml` in the folder where both the repositories that this project depends on are, like this:

```
student-operations
|
|- docker-compose.yaml
|
|- studente_operations_backend
|
|- student-operations-frontend
```

The content of the docker compose must be:

```yaml
services:
  postgres:
    image: 'postgres'
    container_name: 'postgres'
    restart: always
    shm_size: 128mb
    volumes:
      - ./postgres/data:/var/lib/postgres/data
    environment:
      - 'POSTGRES_DB=student_operations'
      - 'POSTGRES_PASSWORD=1234'
    ports:
      - 5432:5432
    networks:
      - student-operations-network

  operations-backend:
    image: student-operations-backend
    container_name: student-operations-backend
    build: ./student_operations_backend
    ports:
      - "8000:8000"
    networks:
      - student-operations-network

  operations-frontend:
    image: student=operations-frontend
    container_name: student-operations-frontend
    build: ./student-operations-frontend
    ports:
      - 3000:3000
    stdin_open: true
    tty: true
    networks:
      - student-operations-network

networks:
  student-operations-network:
    driver: bridge

```

## Running

After configuring the file and repositories, just run:

`docker compose up`

## Access the database

Just run:

`docker exec -it postgres psql -h localhost -p 5432 -d student_operations -U postgres`