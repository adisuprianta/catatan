# Aplikasi D-Distance

Repository ini berisi kode sumber untuk aplikasi D-Distance. Aplikasi ini terdiri dari backend Java menggunakan Spring Boot untuk layanan backend, dan frontend React yang dibangun dengan Vite untuk antarmuka pengguna.

## Backend (Java)

### Dockerfile untuk Backend

```Dockerfile
FROM openjdk:17-alpine
ADD D-Distance-Mobile-0.0.1-SNAPSHOT.jar D-Distance-Mobile-0.0.1-SNAPSHOT.jar
CMD ["java", "-jar", "/D-Distance-Mobile-0.0.1-SNAPSHOT.jar"]

# Bangun image dengan nama 'ddistance-api:latest'
docker build -t ddistance-api:latest .

# Jalankan container dari image yang telah dibuat
docker run -d -p 8081:8081 --name ddistance-api --network ddistance-network \
  -e APP_PORT=8081 -e APP_HOST=http://43.218.88.7 -e DB_HOST=postgres \
  -e DB_USERNAME=postgres -e DB_PASSWORD=password -e DB_PORT=5432 \
  -e JWT_SECRET=ddistance -e DIRECTORY_PATH=/mnt/ \
  -v /home/ubuntu/ddistance/image:/mnt/ ddistance-api
```
## Frontend (React dengan Vite)
### Dockerfile untuk Frontend
```Dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
CMD ["npm", "run", "dev"]
```

# Bangun image dengan nama 'ddistance-front-end:latest'
docker build -t ddistance-front-end:latest .

# Jalankan container dari image yang telah dibuat
docker run -p 5173:5173 -d --name ddistance-front-end ddistance-front-end



Flag --host pada skrip Vite memungkinkan Vite untuk mendengarkan koneksi dari alamat IP mana pun, bukan hanya dari localhost atau IP lokal mesin virtual (VM).
