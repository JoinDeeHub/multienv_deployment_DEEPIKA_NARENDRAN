
🚀 Multi-Environment Ticket Management Application
==================================================

Complete Dockerized Deployment (Dev Backend + Prod Backend + React Frontend + MongoDB)

This project demonstrates a complete **multi-environment application deployment** featuring:

- **Dev Backend (Flask)**
- **Prod Backend (Flask + Gunicorn)**
- **Frontend (React)**
- **MongoDB Database**
- **Docker & Docker Compose Multi-Service Orchestration**
- **Networking, Environment Handling & Service Communication**

This repository fulfills the **Multi-Environment Deployment Assessment** requirements and includes complete troubleshooting notes.

---

📌 Features
===========

### Backend (Dev)

- Flask development server
- Auto-reload
- Accessible at: `http://localhost:3001`

### Backend (Prod)

- Flask app served using **Gunicorn**
- Optimized for production
- Accessible at: `http://localhost:3002`

### Frontend

- React application
- Communicates with both dev & prod backends
- Accessible at: `http://localhost:3005`

### Database

- MongoDB running as a dedicated container
- Shared across dev and prod environments

### Health Endpoints

Both environments include:

`GET /health → {"status":"ok"}`

---

🗂 Folder Structure
===================

<pre class="overflow-visible!" data-start="1910" data-end="2115"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre!"><span><span>
MultienvApp/
│── docker-compose.yml
│── backend/
│   ├── dev/
│   │   ├── app.py
│   │   ├── Dockerfile
│   │   ├── requirements.txt
│   │   └── .env (NOT committed)
│   └── prod/
│       ├── app.py
│       ├── Dockerfile
│       ├── requirements.txt
│       └── .env (NOT committed)
│
└── frontend/
    ├── Dockerfile
    ├── public/
    ├── src/
    └── package.json
</span></span></code></div></div></pre>

---

🧩 System Architecture
======================

<img width="1171" height="436" alt="Screenshot from 2025-11-15 22-41-47" src="https://github.com/user-attachments/assets/620c274d-bede-409d-9cc8-aa956df3bb37" />

---

⚙️ Prerequisites
==================

Ensure your machine has:

- **Docker** ≥ 20.x
- **Docker Compose** ≥ v2.x
- (Optional) Python 3.11+ for local testing

---

🚀 Deployment Steps
===================

1️⃣ Clone the Repository
--------------------------

`git clone https://github.com/YOUR-USERNAME/MultienvApp.git cd MultienvApp`

<img width="1366" height="621" alt="Screenshot from 2025-11-13 23-35-06" src="https://github.com/user-attachments/assets/20972e2f-59fb-452e-93ed-8a3163b2adac" />

<img width="1366" height="621" alt="Screenshot from 2025-11-13 23-35-06" src="https://github.com/user-attachments/assets/98bea049-fc1a-48cd-a206-06c8f4ac8665" />

---

2️⃣ 🌩 MongoDB Atlas Configuration
====================================

You are using **MongoDB Atlas** instead of a local Mongo container.

✔ Step 1 --- Create MongoDB Atlas Cluster
------------------------------------------

1. Go to [https://cloud.mongodb.com](https://cloud.mongodb.com)
2. Create a free/shared cluster
3. Create a new database user
4. Assign password
5. Copy your connection string:

`mongodb+srv://<username>:<password>@<cluster>.mongodb.net/`

---

✔ Step 2 --- Allow Network Access
----------------------------------

Go to **Network Access** → Add IP:

### For development:

`0.0.0.0/0   (Allow all)`

### Or more secure:

`<Your IP>/32`

---

✔ Step 3 --- Create databases
------------------------------

Dev DB:

`multienv_dev`

Prod DB:

`multienv_prod`

(Atlas auto-creates databases on the first write.)

---

🔐 Environment Variables (.env)
===============================

### Important: DO NOT COMMIT `.env` FILES

Add to `.gitignore`:

`backend/*/.env`

---

backend/dev/.env
----------------

`FLASK_DEBUG=1 PORT=3001 MONGO_URI="mongodb+srv://<username>:<password>@<cluster>.mongodb.net/multienv_dev?retryWrites=true&w=majority&appName=MultiEnvApp"`

backend/prod/.env
-----------------

`FLASK_DEBUG=0 PORT=3002 MONGO_URI="mongodb+srv://<username>:<password>@<cluster>.mongodb.net/multienv_prod?retryWrites=true&w=majority&appName=MultiEnvApp"`

<img width="1366" height="697" alt="image" src="https://github.com/user-attachments/assets/ae315186-d9ef-4e8a-af36-c8925c1d1da3" />

<img width="1366" height="697" alt="image" src="https://github.com/user-attachments/assets/daa73086-211f-41b2-a00c-26be3d53f30a" />

---

3️⃣ Build and Start All Services
----------------------------------

`docker compose up --build -d`

---

4️⃣ Verify Running Containers
-------------------------------

`docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"`

Expected:

`multienv_dev_backend     Up   0.0.0.0:3001->3001/tcp multienv_prod_backend    Up   0.0.0.0:3002->3002/tcp multienv_frontend        Up   0.0.0.0:3005->3000/tcp multienv_mongo           Up   0.0.0.0:27017->27017/tcp`

<img width="1366" height="502" alt="Screenshot from 2025-11-13 23-39-11" src="https://github.com/user-attachments/assets/8901e31e-5121-4345-a14d-ba80148446da" />

<img width="1101" height="697" alt="Screenshot from 2025-11-14 01-04-41" src="https://github.com/user-attachments/assets/63f213f6-2edb-4821-9375-1103df141716" />

---

5️⃣ Test Application
----------------------

### Backend Health Checks

`curl http://localhost:3001/health `

<img width="462" height="237" alt="Screenshot from 2025-11-15 23-47-57" src="https://github.com/user-attachments/assets/bb00117d-0612-49d3-b581-d97876176f6f" />

`curl http://localhost:3002/health`

<img width="462" height="237" alt="Screenshot from 2025-11-15 23-47-39" src="https://github.com/user-attachments/assets/97e54ad7-1d02-448e-98f8-1294c817bc1a" />

### Frontend

Open in browser:

`http://localhost:3005`

<img width="1366" height="374" alt="Screenshot from 2025-11-14 00-10-16" src="https://github.com/user-attachments/assets/d71e2ff8-53c2-4c69-8925-678f80b8d6f3" />

### Dev View

`http://localhost:3005/dev`

<img width="1366" height="374" alt="Screenshot from 2025-11-14 00-10-27" src="https://github.com/user-attachments/assets/28a4dca4-cc06-4dba-83ae-799bd8cd38e1" />

### Prod View

`http://localhost:3005/prod`

<img width="1366" height="374" alt="Screenshot from 2025-11-14 00-10-38" src="https://github.com/user-attachments/assets/9640b9f1-fea4-4f92-ad43-1ffdae026476" />

---

🛠️ Full Troubleshooting Guide
===============================

(All real issues encountered during this deployment)

---

❌ Issue 1: **Port 3000 already in use**
------------------------------------

**Error:**

`bind: address already in use`

### ✔ Fix:

Changed frontend host port to 3005 in `docker-compose.yml`:

`ports:

- "3005:3000"`

---

❌ Issue 2: **Backend cannot connect to MongoDB**
---------------------------------------------

Mongo logs showed:

`Client sent an HTTP request over a native MongoDB connection`

This happened because `curl http://localhost:27017` was used --- MongoDB uses binary protocol.

### ✔ Fix:

Correct mongo connectivity testing:

`docker run --rm -it --network multienvapp_multinet mongo:6.0 mongosh "mongodb://mongo:27017" --eval 'db.runCommand({ ping: 1 })'`

Result:

`{ ok: 1 }`

---

❌ Issue 3: **Backend crashed: missing MONGO_URI**
----------------------------------------------

`ValueError: You must specify a URI or set the MONGO_URI`

### ✔ Fix:

Added `.env` files with `MONGO_URI`
Ensured `env_file:` is configured for each backend service.

docker run --rm -it --network multienvapp_multinet mongo:6.0 mongosh --host mongo --eval 'db.runCommand({ ping: 1 })'
{ ok: 1 }

---

❌ Issue 4: **Production backend failed to start --- gunicorn not found**
---------------------------------------------------------------------

`exec: "gunicorn": executable file not found in $PATH`

### ✔ Fix:

Added `gunicorn` to backend/prod/requirements.txt:

`gunicorn>=20.1.0`

Rebuild:

`docker compose build prod-backend`

---

❌ Issue 5: **404 on /health**
--------------------------

Backends had no `/health` endpoint.

### ✔ Fix:

Added this route:

`@app.route("/health") def health():     return jsonify({"status": "ok"}), 200`

---

❌ Issue 6: **Pylance warning --- mongo.db might be None**
------------------------------------------------------

`OptionalMemberAccess`

### ✔ Fix:

Added:

`assert mongo.db is not None`

Or:

`# type: ignore`

---

🧪 Final Verification Checklist
===============================

| Component         | Status     | Test                                        |
| ----------------- | ---------- | ------------------------------------------- |
| MongoDB           | ✅ running | `{ ok: 1 }`                               |
| Dev Backend       | ✅ running | `curl /health`                            |
| Prod Backend      | ✅ running | `curl /health`                            |
| Frontend          | ✅ running | [http://localhost:3005](http://localhost:3005) |
| API Communication | ✅ working | Frontend → Backends → Mongo               |
| Data Persistence  | ✅ working | add/complete/delete todos                   |


<img width="1366" height="697" alt="image" src="https://github.com/user-attachments/assets/b58185e4-d236-4d65-b91b-1082a02f721e" />

<img width="1366" height="697" alt="image" src="https://github.com/user-attachments/assets/b4ac1bf3-f63c-4fa6-a782-00ea9aef4667" />

<img width="1366" height="697" alt="image" src="https://github.com/user-attachments/assets/4ad2ab50-352b-4346-937d-0b3243a0abb8" />

<img width="1366" height="697" alt="image" src="https://github.com/user-attachments/assets/525153c0-5550-4e08-9dc0-ce68fdb7a7d4" />


<img width="1366" height="738" alt="Screenshot from 2025-11-15 22-01-55" src="https://github.com/user-attachments/assets/1291b717-7e94-4133-ba4f-a9d3d10c198b" />

A fully functioning multi-environment Dockerized application backed by MongoDB Atlas, with professional-grade documentation and troubleshooting.

<img width="1366" height="699" alt="Screenshot from 2025-11-15 22-28-15" src="https://github.com/user-attachments/assets/9b8553a8-14ab-49d7-b0cd-7d8f33d4b310" />

---

📌 Author
---------

**DEEPIKA NARENDRAN**
Project: Multi-Environment Ticket Management Application

GitHub: @JoinDeeHub
