# Setup docker-compose for web development

1. สร้าง Folder `backend` แล้วทำการ cd เข้าไปใน `backend` แล้วเขียนคำสั่ง

```
npm init -y
```

2. ลง Dependencies ของ Backend

```
npm install --save express body-parser cors
```

```
npm install --save-dev nodemon
```

2. ทำการสร้าง Project Frontend ด้วย create-react-app

```
npx create-react-app frontend
```

3. ทำการสร้าง File `backend/index.js` แล้วเขียน Code server เล็กๆ ลงไป

```javascript
const express = require("express");
const app = express();
const port = 3001;

app.get("/", (req, res) => {
  res.send("Hello World!");
});

app.listen(port, () => {
  console.log(`Example app listening at http://localhost:${port}`);
});
```

4. แก้ script ใน package.json

```javascript
  "scripts": {
    "start": "nodemon index.js"
  },
```

5. ทำการลอง Run server ดูหน่อยว่าทำงานได้มั้ย

```
npm run start
```

6. เขียน Dockerfile ของ Backend `backend/Dockerfile` ดังนี้

```docker
FROM node:alpine

WORKDIR /app

COPY package.json package-lock.json ./

RUN npm ci
COPY . .

CMD ["npm", "start"]
```

7. ลอง build node app แล้วลอง run container ดูก่อน

```
docker build -t my-node-app

docker run -dit -p 3001:3001 --rm --name my-node-app my-node-app
```

8. ลองเข้าไปที่ `http://localhost:3001` ถ้าขึ้นหน้าเว็บก็แสดงว่าใช้งานได้

9. ทำการ Kill container ทิ้ง

```
docker ps

docker kill <container_id>
```

10. ทำการเขียน Dockerfile ใน frontend project

```
FROM node:alpine

WORKDIR /app
COPY . .
RUN npm ci && npm run build

FROM nginx:latest
COPY --from=0 /app/build /usr/share/nginx/html
```

11. ทำการ Run my-web-app container แล้วลองเข้าไปที่ `http:localhost:3000` ถ้าขึ้นเป็นหน้าเว็บแสดงว่าทำงานได้ปกติ

```
docker run -dit -p 3000:80 --rm --name my-web-app my-web-app
```

12. ต่อไปเราจะทำการ run web app, node app, mongo database, และ mysql database พร้อม ๆ กันด้วยการเขียน docker-compose.yml ไว้ที่ root folder

```
version: "3"
services:
  web_app:
    container_name: "my_web_app"
    build: ./frontend
    ports:
      - "3000:80"
  node_app:
    container_name: "my_node_app"
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

13. ทำการ run `docker-compose up`
