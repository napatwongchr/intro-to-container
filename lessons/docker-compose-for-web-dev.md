# Setup front end with docker compose

1. ทำการสร้าง Project Frontend ด้วย create-react-app

```
npx create-react-app frontend
```

2. ทำการเขียน Dockerfile ใน frontend project

```
FROM node:alpine

WORKDIR /app
COPY . .
RUN npm ci && npm run build

FROM nginx:latest
COPY --from=0 /app/build /usr/share/nginx/html
```

3. ทำการ Run my-web-app container แล้วลองเข้าไปที่ `http:localhost:3000` ถ้าขึ้นเป็นหน้าเว็บแสดงว่าทำงานได้ปกติ

```
docker run -dit -p 3000:80 --rm --name my-web-app my-web-app
```

4. ต่อไปเราจะทำการ run web app, node app, mongo database, และ mysql database พร้อม ๆ กันด้วยการเขียน docker-compose.yml ไว้ที่ root folder

```
version: "3"
services:
  web_app:
    container_name: "my_web_app"
    build: ./frontend
    ports:
      - "3000:80"
  node_app:
    container_name: "my_first_node-app"
    build: ./backend
    ports:
      - "3001:3001"
    environment:
      MONGO_CONNECTION_STRING: mongodb://localhost:27017
  db:
    container_name: "mongo_db"
    image: mongo:3
    ports:
      - "27017:27017"
  db_sql:
    container_name: "mysql_db"
    image: mysql
    command: --default-authentication-plugin=mysql_native_password
    restart: always
    volumes:
      - "mysql_data:/var/lib/mysql"
    environment:
      MYSQL_ROOT_PASSWORD: "123456789"
    ports:
      - "3306:3306"
    cap_add:
      - SYS_NICE
volumes:
  mysql_data:
```

5. ทำการ run `docker-compose up` เราจะมี Containers ขึ้นมาทั้งหมด 4 ตัว web, node server,
   mongodb, mysql

<br><hr><br>

## Development workflow for docker application

![Development workflow for docker app](https://docs.microsoft.com/en-us/dotnet/architecture/microservices/docker-application-development-process/media/docker-app-development-workflow/life-cycle-containerized-apps-docker-cli.png)

[Table of content](https://github.com/napatwongchr/intro-to-container)
