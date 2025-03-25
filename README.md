
# 🐳 Frappe + ERPNext Docker Setup

This guide documents the process of setting up **Frappe with ERPNext** using Docker, including site creation, app installation, and saving the environment with a committed image.


---

## ✅ Step 1: Build the Dockerfile

Run this in the root directory containing your Dockerfile:

```bash
docker build -t frappe_docker .
```

---

## ✅ Step 2: Set up `docker-compose.yml`

Create a `docker-compose.yml` file with services for:
- Redis (cache, queue, socketio)
- MariaDB
- Frappe app

Then run:

```bash
docker-compose up -d
```

---

## ✅ Step 3: Enter the Frappe container

```bash
docker exec -it <container-name> bash
```

Example:

```bash
docker exec -it frappe-docker bash
```

---

## ✅ Step 4: Create a new site

Inside the container:

```bash
cd ~/bench-version-15
bench new-site <your-site-name>
```

Example:

```bash
bench new-site frappe-docker.local
```

---

## ✅ Step 5: Get and install ERPNext

Still inside the container:

```bash
bench get-app erpnext https://github.com/frappe/erpnext --branch version-15
bench --site frappe-docker.local install-app erpnext
```

---

## ✅ Step 6: Save the setup as a Docker image

Exit the container and run:

```bash
docker commit frappe-docker my-frappe-image:v1.0
```

---

## ✅ Step 7: Use your committed image

Update your `docker-compose.yml` file to use the committed image:

```yaml
image: my-frappe-image:v1.0
```

Then restart:

```bash
docker-compose down
docker-compose up -d
```

---

## ✅ Step 8: Start the Frappe server

```bash
docker exec -it frappe-docker bash
cd ~/bench-version-15
bench start
```

---

## 🖥️ Access the App

Visit:

```
http://localhost:8000
```
