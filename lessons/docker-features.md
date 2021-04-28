# Docker Features

- Docker bind mount
- Docker volume mount
- Docker network
- Docker compose

<br><hr><br>

## Docker Bind Mount

Docker bind mount คือ ฟีเจอร์ที่ทำให้เราสามารถที่เอาไฟล์ข้างนอก Contianer เข้าไป Run ใน Container ได้

🌟 มีประโยชน์ในการสร้าง Environment Dev โดยทีเราไม่ต้อง Setup Environment ในการพัฒนา Software ใหม่ขึ้นมาเองตั้งแต่ต้น

`docker run -dit -p 3001:3001 --mount type=bind,src="$(pwd)",target=/app --rm --name my-first-node-app my-first-node-app`

<br><hr><br>

## Docker Volume Mount

Docker volume mount คือ ฟีเจอร์ที่ทำให้เราสามารถที่จะเก็บข้อมูลไว้ ระหว่างการ Run container ได้ **เนื่องจาก Container ที่รันขึ้นมาจะไม่สามารถที่จะเก็บข้อมูลหรืออะไรไว้ได้เลยเวลาถูกปิดลงไป**

🌟 มีประโยชน์ในการเก็บข้อมูล Database เช่น Container ของ MySQL

`docker run -dit -p 3306:3306 -e MYSQL_ROOT_PASSWORD="123456789" --mount type=volume,src=mysql_data,target=/var/lib/mysql --rm --name mysql_db mysql`

<br><hr><br>

## Docker Compose

Docker compose คือ ฟีเจอร์ที่ทำให้เราสามารถรัน Container หลาย ๆ ตัวพร้อมกันได้ แล้วหยุดพร้อมกันหลาย ๆ ตัวได้เช่นเดียวกัน อีกทั้งยังสามารถสร้าง Network ให้กับ Containers อัตโนมัติ

```
version: "3"
services:
  node_app:
    container_name: "my-first-node-app"
    build: ./backend
    ports:
      - "3001:3001"
    volumes:
      - type: bind
        source: ./backend
        target: /app
    environment:
      MYSQL_CONNECTION_STRING: localhost:3306
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

<br><hr><br>

[Table of content](https://github.com/napatwongchr/intro-to-container)
