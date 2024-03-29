# Docker

### Move JAR From Local to Server

```bash
scp -i <sertificat.pem> <name file> <username>@<ip address>:<directory location>

scp -i trainer.pem x-food-0.0.1-SNAPSHOT.jar ubuntu@43.218.113.28:~/.
```

### Stop Container

```bash
docker stop container <name container>

docker stop container xfood-db
```

### Remove Container

```bash
// view container
docker container ls -a

// remove container
docker container rm <container id>
```

### Step By Step

DB

1. Buat Volume
2. Buat Network
3. Buat container baru untuk xfood
4. Masuk container BD untuk buat database

API

1. Buat Jar
2. Transfer Jar xfood ke Server
3. Buat Dockerfile
4. Buat Image xfood
5. Buat Container xfood

### Docker Volume

```bash
// create volume
docker volume create <name volume>

docker volume create xfood-volume

// view volume
docker volume ls

// inspect volume
docker inspect volume <name volume>
```

### Docker Run Postgres

```bash
// create docker run and set volume
docker run --name <name container> -e POSTGRES_PASSWORD=<password>
-p 5433:5432 -v <name volume>:/var/lib/postgresql/data -d <name image>

docker run --name xfood-db -e POSTGRES_PASSWORD=sayang -p 5433:5432 -v xfood-volume:/var/lib/postgresql/data -d postgres
```

masuk execx xfood-db bash

```bash
docker exec -it xfood-db bash
```

### Docker Run API

```bash
// exp
docker run --name xfood-api --network <name network> 
-p <port publish>:<port docker>
-e SERVER_PORT=8081
-e DB_USERNAME=<username container> 
-e DB_PASSWORD=<password container> 
-e DB_HOST=<name container> 
-e DB_PORT=<port container> 
-e JWT_SECRET=<secret>
-e EMAIL_SUPER_ADMIN=superadmin@gmail.com
-e PASSWORD_SUPER_ADMIN=superadmin
-e EMAIL_TEMPORARY_ADMIN=temporaryadmin@gmail.com
-e PASSWORD_TEMPORARY_ADMIN=temporaryadmin
<name image>:<version>

docker run --name xfood-api --network xfood-network -p 8081:8081 -e SERVER_PORT=8081 -e DB_USERNAME=postgres -e DB_PASSWORD=sayang -e DB_HOST=xfood-db -e DB_PORT=5432 -e JWT_SECRET=rahasiasayang -e EMAIL_SUPER_ADMIN=superadmin@gmail.com -e PASSWORD_SUPER_ADMIN=superadmin -e EMAIL_TEMPORARY_ADMIN=temporaryadmin@gmail.com -e PASSWORD_TEMPORARY_ADMIN=temporaryadmin -e LOCATION=/mnt/ -v /home/ubuntu:/mnt/ -d xfood-image:latest
```

### Move Jar from Local to Server

```bash
scp -i <name certificat.pem> <name file jar> <server>@<ip>:<location>

scp -i trainer.pem xfood-0.0.0.1-SNAPSHOT.jar ubuntu@43.218.113.28:~/.
```

### Docker File

```bash
nano Dockerfile
```

```docker
FROM <libary>
COPY <name jar di server> <name jar di docker>
CMD ["java", "-jar", "/<name jar di docker>"]

FROM openjdk:19-jdk-alpine
COPY x-food-0.0.1-SNAPSHOT.jar x-food-0.0.1-SNAPSHOT.jar
CMD ["java", "-jar", "x-food-0.0.1-SNAPSHOT.jar"]
```

```docker
FROM node:latest AS builder
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci

COPY . .
RUN npm run build

FROM nginx:stable-alpine
COPY --from=builder /app/build /usr/share/nginx/html
COPY nginx/nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 8080
CMD ["nginx", "-g", "daemon off;"]

===

FROM node:latest
WORKDIR '/app'

COPY package.json .
RUN npm install -g npm@10.3.0
COPY . .
CMD ["npm","start"]
```

### Docker Image

```bash
docker build -t <name aplikasi> <lokasi docker saat ini>

docker build -t xfood-image:latest .

docker build -t xfood-fe-image .
```

### Docker Network

```bash
docker network create -d bridge <name network>

docker network create -d bridge xfood-network

// view network
docker network ls

// connect network
docker network connect <name network> <name container>

docker network connect xfood-netword xfood-db
```
