# 🐳 Docker Compose Multi-Container App

This project demonstrates how to build and connect **three containers** using **Docker Compose** — a full-stack environment that includes:

- 🌐 **Frontend**: HTTPD server (Apache) showing your name and connecting to the backend.
- ⚙️ **Backend**: Flask API that connects to the MariaDB database.
- 🗄️ **Database**: MariaDB for storing and retrieving user data.

---

## 🧱 **Architecture**

```
 Frontend (HTTPD)
        │
        ▼
 Backend (Flask)
        │
        ▼
Database (MariaDB)
```

### Networks:

- `frontend_network` → connects frontend ↔ backend
- `backend_network` → connects backend ↔ database

---

## 📁 **Project Structure**

```
docker-compose-project/
│
├── docker-compose.yml
│
├── frontend/
│   └── index.html
│
├── backend/
│   ├── Dockerfile
│   ├── app.py
│   └── requirements.txt
│
└── init.sql
```

---

## ⚙️ **Services**

### 🔹 Frontend (HTTPD)

- Image: `httpd:latest`
- Displays your name (e.g., **Hello from Kamal Sabry! 👋**)
- Communicates with backend via fetch API

### 🔹 Backend (Flask)

- Image: `kamalsabry/backend-app:1.0`
- Connects to MariaDB
- Has 2 endpoints:
  - `/` → returns “Hello from Flask Backend! 🚀”
  - `/users` → fetches users from database

### 🔹 Database (MariaDB)

- Image: `mariadb:latest`
- Environment Variables:
  ```yaml
  DB_HOST: database
  DB_USER: root
  MARIADB_ROOT_PASSWORD: root123
  MARIADB_DATABASE: mydb
  ```
- Automatically initialized by `init.sql`

---

## 🧩 **Networks**

```yaml
networks:
  frontend_network:
    name: frontend_network
  backend_network:
    name: backend_network
```

---

## 🚀 **How to Run**

### 1️⃣ Build and Start Containers

```bash
docker compose up -d --build
```

### 2️⃣ Check Running Containers

```bash
docker ps
```

### 3️⃣ Access the Application

- Frontend → [http://localhost:8080](http://localhost:8080)
- Backend → [http://localhost:5000](http://localhost:5000)

---

## 🧪 **Testing Connectivity**

### 🔹 Check Frontend ↔ Backend

```bash
docker exec -it frontend ping backend
```

### 🔹 Check Backend ↔ Database

```bash
docker exec -it backend ping database
```

### 🔹 Fetch from Flask API

```bash
curl http://localhost:5000/users
```

---

## 🧰 **Useful Commands**

| Description        | Command                                 |
| ------------------ | --------------------------------------- |
| List networks      | `docker network ls`                     |
| Inspect network    | `docker network inspect <network_name>` |
| Stop containers    | `docker compose down`                   |
| Rebuild containers | `docker compose up --build`             |

---

## 🧹 **Cleanup**

To remove everything (containers, volumes, and networks):

```bash
docker compose down -v
```

---

## 👤 **Author**

### **Kamal Sabry**

📧 [kamalsabry1275ks@gmail.com](mailto:kamalsabry1275ks@gmail.com)

---

## 🏁 **Result Preview**

When you open `http://localhost:8080`, you should see:

```
Hello from Kamal Sabry! 👋

👥 Users from database!
```

---

## 🧠 **Notes**

- Make sure Docker and Docker Compose are installed.
- If backend fails to connect to DB on first run, wait a few seconds (MariaDB needs time to start).
- You can edit `init.sql` to pre-load data into the database.
