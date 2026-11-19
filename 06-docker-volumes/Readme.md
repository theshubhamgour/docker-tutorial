# 🐳 Docker Volume Demonstration (Hands-On Guide)

## 📌 What is Docker Volume?

Applications can be: - **Stateful** → Data is persistent\
- **Stateless** → Data is NOT persistent

👉 By default, Docker containers are **stateless**.

If a container is deleted → **ALL data is lost**

------------------------------------------------------------------------

## ❗ Problem Statement

Example without volume:

### Nginx Example

``` bash
docker pull nginx
docker exec -it nginx-app /bin/bash
cd /usr/share/nginx/html/
echo "Index from html" > index.html
docker rm -f nginx-app
```

👉 Data is LOST after container deletion

------------------------------------------------------------------------

### MySQL Example (No Persistence)

``` bash
docker run --name mysql-dev -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql
docker exec -it mysql-dev /bin/bash
mysql -u root -p
create database docker-learn;
show databases;
docker rm -f mysql-dev
```

👉 Database is DELETED

------------------------------------------------------------------------

## 💡 Solution: Docker Volume

Docker Volume = A folder on Docker Host mapped to container

### Syntax:

``` bash
docker run -v <local-path>:<container-path> <image>
```

------------------------------------------------------------------------

## 🔧 Practical 1: Basic Volume with Nginx

``` bash
mkdir -m 777 /data

docker run -d --name=webapp -v /data:/usr/share/nginx/html nginx:latest

docker exec -it webapp /bin/bash
echo "My web app" > /usr/share/nginx/html/index.html
exit

cat /data/index.html
```

👉 Output:

    My web app

------------------------------------------------------------------------

### Delete Container & Verify Persistence

``` bash
docker rm -f webapp

docker run -d --name=newapp -v /data:/usr/share/nginx/html nginx:latest
```

👉 Data is STILL PRESENT ✅

------------------------------------------------------------------------

## 🔁 Practical 2: Shared Volume Across Multiple Containers

``` bash
mkdir /data
echo "Hello World" > /data/index.html

docker run -d --name=app1 -v /data:/usr/share/nginx/html nginx
docker run -d --name=app2 -v /data:/usr/share/nginx/html nginx
docker run -d --name=app3 -v /data:/usr/share/nginx/html nginx
```

------------------------------------------------------------------------

### Access All Containers

``` bash
curl <IP-of-app1>
curl <IP-of-app2>
curl <IP-of-app3>
```

👉 Output:

    Hello World

------------------------------------------------------------------------

### Real-Time Sync

``` bash
echo "New Data" > /data/index.html
```

👉 All containers reflect:

    New Data

------------------------------------------------------------------------

## 🗄️ Practical 3: MySQL Persistent Storage

``` bash
mkdir /database

docker run -d --name=db -e MYSQL_ROOT_PASSWORD=admin123 -v /database:/var/lib/mysql mysql

docker exec -it db /bin/bash
mysql -u root -p
```

------------------------------------------------------------------------

### Create Data

``` sql
create database docker-dock;
use docker-dock;

CREATE TABLE student (
  id INT(4) PRIMARY KEY,
  name varchar(20)
);

INSERT INTO student (id, name) VALUES
(1, 'Rahul'),
(2, 'Priya'),
(3, 'Amit'),
(4, 'Sneha');

SELECT * FROM student;
```

------------------------------------------------------------------------

### Delete Container

``` bash
docker rm -f db
```

------------------------------------------------------------------------

### Recreate Container

``` bash
docker run -d --name=mydb -e MYSQL_ROOT_PASSWORD=admin123 -v /database:/var/lib/mysql mysql

docker exec -it mydb /bin/bash
mysql -u root -padmin123

show databases;
select * from student;
```

👉 Data is PRESERVED ✅

------------------------------------------------------------------------

## 🚀 Key Takeaways

-   Containers are **ephemeral**
-   Volumes provide **data persistence**
-   Data survives container deletion
-   Multiple containers can share same data
-   Real-time synchronization is possible

------------------------------------------------------------------------

## 🎯 Interview Questions

1.  What is Docker Volume?
2.  Difference between bind mount and volume?
3.  What happens to container data after deletion?
4.  How do you persist MySQL data in Docker?
5.  Can multiple containers share same volume?

------------------------------------------------------------------------

## 📌 Conclusion

Docker Volumes solve the biggest problem: 👉 **Data Loss in Containers**

They are essential for: - Databases - Web apps - Production workloads

------------------------------------------------------------------------

🔥 That's it! You now understand Docker Volumes with real examples.
