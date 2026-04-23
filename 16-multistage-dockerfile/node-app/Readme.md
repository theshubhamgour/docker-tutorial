# 🚀 Multi-Stage Docker for Node.js 

## 📌 Application Overview

This is a simple Node.js application using Express:

-   A basic server that runs on port 3000
-   Returns: "Hello from Node App"

------------------------------------------------------------------------

## 📦 Project Files

### package.json

Defines dependencies like Express

### app.js

Creates a simple Express server

------------------------------------------------------------------------

## 🐳 Existing Dockerfile (Common Pattern)

``` dockerfile
FROM node:18

WORKDIR /app
COPY package.json .
RUN npm install

COPY . .

CMD ["node", "app.js"]
```

------------------------------------------------------------------------

## ❌ Issues with Existing Dockerfile

-   Uses full Node image → large size
-   Installs all dependencies (including unnecessary ones)
-   No separation between build & runtime
-   Slower builds and heavier images

------------------------------------------------------------------------

## 🤔 What is Multi-Stage Build?

Multi-stage builds separate:
- Build stage (install dependencies)
- Runtime stage (run app efficiently)

------------------------------------------------------------------------

## ⚖️ Comparison

| Feature      | Single Stage | Multi-Stage |
|--------------|--------------|-------------|
| Image Size   | Large ❌     | Small ✅     |
| Security     | Lower ❌     | Higher ✅    |
| Performance  | Slower ❌    | Faster ✅    |

------------------------------------------------------------------------

## ✅ Multi-Stage Dockerfile

``` dockerfile
# Stage 1: Dependencies
FROM node:18-alpine AS builder

WORKDIR /app
COPY package.json .
RUN npm install --production

# Stage 2: Runtime
FROM node:18-alpine

WORKDIR /app
COPY --from=builder /app/node_modules ./node_modules
COPY app.js .

EXPOSE 3000
CMD ["node", "app.js"]
```

------------------------------------------------------------------------

## � Build Result Comparison

Before and after building the images, the size difference is visible from the Docker image listing:

```text
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
node-app2    latest    13a918aaa8bd   14 seconds ago   128MB
node-app     latest    78299c2233a1   2 minutes ago    1.1GB
```

------------------------------------------------------------------------

## �🔍 Explanation

### Stage 1 (Builder)

-   Installs only production dependencies
-   Keeps build isolated

### Stage 2 (Runtime)

-   Copies only required modules
-   Uses lightweight image
-   Final image is much smaller

------------------------------------------------------------------------

## ▶️ How to Run

``` bash
docker build -t node-app .
docker run -p 3000:3000 node-app
```

------------------------------------------------------------------------

## 🧠 Key Takeaways

-   Avoid large base images
-   Use production dependencies only
-   Separate build and runtime
-   Multi-stage builds are production-ready approach

------------------------------------------------------------------------

Happy Learning Docker 🐳
