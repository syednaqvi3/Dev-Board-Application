DevBoard – 3‑Tier Application with Reverse Proxy
A production‑style 3‑tier application built with React (Vite) frontend, Go backend, and PostgreSQL database, orchestrated using Docker Compose. An NGINX reverse proxy provides a single entry point and clean routing between services.

🚀 Features
Frontend: React + Vite with client‑side routing and API proxy.
Backend: Go REST API exposing endpoints for projects and tasks.
Database: PostgreSQL with initialization scripts for default data.
Reverse Proxy: NGINX routes traffic:
/ → frontend
/api → backend (with /api prefix stripped)
Healthcheck: Postgres monitored with pg_isready before backend starts.
Persistent Storage: Named Docker volume (db-backup) for database data.
🛠️ Tech Stack
Frontend: React, Vite
Backend: Go
Database: PostgreSQL
Reverse Proxy: NGINX
Containerization: Docker & Docker Compose
📂 Project Structure
.
├── backend/              # Go API (main.go + Dockerfile)
├── frontend/             # React app (Vite). Serves the UI, forwards /api
├── init/postgres/        # Schema + example data, loaded on first start
├── nginx.conf            # Reverse proxy configuration
├── docker-compose.yml    # Service orchestration
├── Makefile              # Short commands (make up, make down, ...)
├── .env.example          # Template for settings (copy to .env)
└── README.md             # Project documentation
⚙️ Setup & Usage Clone the repository

git clone https://github.com/nikscool24/3-tier-Devboard-application.git cd 3-tier-Devboard-application

Configure environment variables

POSTGRES_USER=youruser POSTGRES_PASSWORD=yourpassword POSTGRES_DB=yourdb POSTGRES_HOST_PORT=5432 POSTGRES_CONTAINER_PORT=5432

BACKEND_HOST_PORT=8081 BACKEND_CONTAINER_PORT=8080

FRONTEND_HOST_PORT=8080 FRONTEND_CONTAINER_PORT=4173

NGINX_HOST_PORT=80 NGINX_CONTAINER_PORT=80

Note: Dockerfiles for both the frontend and backend are present in their respective folders and are used to build the images.

Build and run

docker compose up --build

Access the app Frontend: http://localhost Backend API: http://localhost/api Database: PostgreSQL on localhost:5432

NGINX Reverse Proxy Routing configuration file nginx.conf is present at the root directory:

http { upstream frontend { server frontend:4173; }

upstream backend { server backend:8080; }

server { listen 80;

location / {
  proxy_pass http://frontend;
}

location /api {
  rewrite ^/api/(.*)$ /$1 break;
  proxy_pass http://backend;
}
} }
