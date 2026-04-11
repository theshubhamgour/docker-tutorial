# Dockerfile Guide & Demos

> “Docker run is temporary… Dockerfile is permanent.”

**Quick Comparison:**

| `docker run` | `Dockerfile` |
| :--- | :--- |
| One command | Step-by-step process |
| Hard to repeat | Easy to version control |
| Manual | Automated |
| 🍜 Cooking instantly | 📖 A recipe |

---

## Part 1: Beginner-Friendly Demos (No Code Needed)

If you don’t have any app code, you can still demonstrate `Dockerfile` vs `docker run` easily.

### 🚀 Example 1: Run NGINX

**🔹 Docker Run (The Quick Way)**
```bash
docker run -d -p 8080:80 nginx
```
👉 Open browser → `localhost:8080` (You’ll see the default NGINX page).

**🔹 Equivalent Dockerfile (The Structured Way)**
```dockerfile
FROM nginx:latest
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

**🧠 Explain simply:**
* `FROM nginx:latest` → Ready-made web server
* `EXPOSE 80` → App runs on port 80
* `CMD` → Start nginx

**🔹 Build & Run**
```bash
docker build -t my-nginx .
docker run -d -p 8080:80 my-nginx
```

---

### 🚀 Example 2: Create Your Own HTML (No coding skills needed)

**🔹 Step 1: Create a simple file**
```bash
echo "<h1>Hello from Docker 🚀</h1>" > index.html
```

**🔹 Docker Run (The Temporary Way)**
```bash
docker run -d -p 8080:80 -v $(pwd):/usr/share/nginx/html nginx
```
**🧠 Explain:**
* `-v $(pwd):/usr/share/nginx/html` → Mounts your local folder into the container. Your HTML replaces the default page.

**🔹 Dockerfile (The Proper Way)**
```dockerfile
FROM nginx:latest

RUN rm -rf /usr/share/nginx/html/*

COPY index.html /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

**🔹 Build & Run**
```bash
docker build -t my-html-site .
docker run -d -p 8080:80 my-html-site
```

---

