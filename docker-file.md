# Docker File

![Docker File](./lessons/images/docker-file.png)

<br><hr><br>

## Building NodeJS Images

1. สร้าง Dockerfile ที่ root Project

```
FROM node:alpine

WORKDIR /app

COPY package.json package-lock.json ./
RUN npm install
COPY ./ ./

CMD ["npm", "start"]
```

2. ทำการสร้าง Image ด้วย Docker Build ด้วยคำสั่ง (รันที่ Root Folder)

```
docker build -t my-first-node-app .
```

3. Push my-first-node-app เข้าไปใน docker hub

```
docker push <docker_hub_username>/my-first-node-app
```

เมื่อเรา Push Image ขึ้นไปบน Docker Hub แล้ว เราสามารถที่จะ Pull ลงมา Run Container บนเครื่องเราได้เลย
