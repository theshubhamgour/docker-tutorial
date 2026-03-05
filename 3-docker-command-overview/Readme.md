# Docker Operations Practice

This README documents basic Docker operations demonstrated in the video
including:

-   Pulling Docker images
-   Running containers
-   Inspecting images and containers
-   Checking container processes and logs
-   Copying files between host and container
-   Cleaning up Docker resources

------------------------------------------------------------------------

# 1. Pull Docker Images

``` bash
docker pull nginx:latest
docker pull fedora:latest
docker pull mysql:latest
```

------------------------------------------------------------------------

# 2. List Docker Images

``` bash
docker images
```

------------------------------------------------------------------------

# 3. Inspect Docker Images

``` bash
docker image inspect <image-name>
```

Example:

``` bash
docker image inspect nginx
```

------------------------------------------------------------------------

# 4. Inspect Docker Containers

``` bash
docker inspect <container-name-or-id>
```

Example:

``` bash
docker inspect nginx-app
```

------------------------------------------------------------------------

# 5. Run Docker Containers

### Run Nginx

``` bash
docker run -d --name=nginx-app nginx:latest
```

### Run MySQL

``` bash
docker run -d --name=mysql-app -e MYSQL_ROOT_PASSWORD=admin123 mysql:latest
```

### Run Fedora Container

``` bash
docker run -itd --name=fedora-app fedora:latest
```

------------------------------------------------------------------------

# 6. List Docker Containers

``` bash
docker ps
docker ps -a
```

------------------------------------------------------------------------

# 7. Access Inside a Container

``` bash
docker exec -it fedora-app /bin/sh
```

------------------------------------------------------------------------

# Service Containers vs Base OS Containers

### Service Containers

Example: Nginx, Apache

    Daemon image (-d) → first command → service → background non‑interactive

Access shell using:

    docker exec -it <container> /bin/bash

------------------------------------------------------------------------

### Base OS Containers

Example: Fedora, Ubuntu, CentOS

    BaseOS (-itd) → first command → shell (/bin/bash) → interactive

Allows:

-   docker attach
-   docker exec

------------------------------------------------------------------------

# 8. Delete All Containers

``` bash
docker rm -f $(docker ps -aq)
```

------------------------------------------------------------------------

# 9. Delete All Images

``` bash
docker rmi -f $(docker images -q)
```

------------------------------------------------------------------------

# 10. Pull Additional Images

``` bash
docker pull centos:7
docker pull httpd
```

------------------------------------------------------------------------

# 11. Run HTTPD Container

``` bash
docker run -itd --name=httpd-app httpd:latest
```

------------------------------------------------------------------------

# 12. Check Running Processes Inside Container

``` bash
ps aux
```

------------------------------------------------------------------------

# 13. Check Processes Without Entering Container

``` bash
docker top centos-app
docker top httpd-app
```

------------------------------------------------------------------------

# 14. View Container Logs

``` bash
docker logs centos-app
docker logs httpd-app
```

------------------------------------------------------------------------

# 15. Copy File from Local to Container

``` bash
docker cp index.html httpd-app:/usr/local/apache2/htdocs/
```

Verify:

``` bash
docker exec -it httpd-app /bin/bash
cat htdocs/index.html
```

------------------------------------------------------------------------

# 16. Copy File from Container to Local

``` bash
docker cp eda0669e2928:/usr/local/apache2/htdocs/index.html /home/ubuntu
```

------------------------------------------------------------------------

# Summary

This guide covered the most common Docker operations used in DevOps:

-   Pulling images
-   Running containers
-   Inspecting Docker resources
-   Accessing containers
-   Viewing processes and logs
-   Copying files between host and container
-   Cleaning Docker environment
