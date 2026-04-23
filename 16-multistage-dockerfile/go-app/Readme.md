# 🚀 Multi-Stage Docker for Go 

------------------------------------------------------------------------

## 📌 Application Overview

This is a simple Go application:

``` go
package main

import "fmt"

func main() {
    fmt.Println("Hello from Go")
}
```

This application: - Is written in Go - Compiles into a single binary -
Prints a simple message when executed

------------------------------------------------------------------------

## 🐳 Existing Dockerfile (Typical Beginner Approach)

``` dockerfile
FROM golang:1.22

WORKDIR /app
COPY . .

RUN go build -o app

CMD ["./app"]
```

------------------------------------------------------------------------

## ❌ Problems in the Current Dockerfile

1.  **Large Image Size**
    -   Uses full `golang` image (\~1GB+)
2.  **Unnecessary Tools in Final Image**
    -   Go compiler is present even during runtime
3.  **No Separation of Build and Runtime**
    -   Everything happens in one stage
4.  **Not Production Ready**
    -   Slower deployments
    -   Security risk due to extra tools

------------------------------------------------------------------------

## 🤔 What is Multi-Stage Docker Build?

Multi-stage builds allow splitting the process into:

-   🏗️ Build Stage → compile the application\
-   🚀 Runtime Stage → run the application

### Simple Analogy:

Cooking food in kitchen → Serving only final dish

------------------------------------------------------------------------

## ⚖️ Single Stage vs Multi-Stage

| Feature           | Single Stage | Multi-Stage |
|-------------------|--------------|-------------|
| Image Size        | Large ❌     | Very Small ✅ |
| Security          | Low ❌       | High ✅      |
| Performance       | Basic ❌     | Optimized ✅ |
| Production Ready  | No ❌        | Yes ✅       |

------------------------------------------------------------------------

## 📊 Build Result Comparison

```text
REPOSITORY   TAG       IMAGE ID       CREATED              SIZE
go-app2      latest    3fc9b16af92c   About a minute ago   10.6MB
go-app       latest    cd6f735e575a   2 minutes ago        849MB
```

------------------------------------------------------------------------

## ✅ Optimized Multi-Stage Dockerfile

``` dockerfile
# Stage 1: Build the Go binary
FROM golang:1.22-alpine AS builder

WORKDIR /app
COPY main.go .

RUN go build -o app main.go

# Stage 2: Minimal runtime image
FROM alpine:latest

WORKDIR /app
COPY --from=builder /app/app .

CMD ["./app"]
```

------------------------------------------------------------------------

## 🔍 Line-by-Line Explanation

### Stage 1 (Builder)

-   Uses Go image
-   Compiles the application
-   Produces binary (`app`)

### Stage 2 (Runtime)

-   Uses very small Alpine image
-   Copies only compiled binary
-   No Go compiler present

------------------------------------------------------------------------

## 🎯 Why Multi-Stage is Important

-   Final image becomes very small (\~5--10MB)
-   Faster Docker pulls
-   Reduced attack surface
-   Clean and production-ready setup

------------------------------------------------------------------------

## 🧠 Key Takeaways

-   Go apps compile into a single binary
-   No need for Go in production image
-   Multi-stage builds remove unnecessary tools
-   Always optimize Docker images for production

------------------------------------------------------------------------

Happy Learning Docker 🐳🚀
