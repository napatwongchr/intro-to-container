# Docker

Docker คือ **Command line tools** ที่ไว้ใช้สำหรับจัดการ **Containers**

Docker Desktop คือ **โปรแกรมที่เอาไว้จัดการ Containers**

[ติดตั้งได้ที่นี่](https://www.docker.com/products/docker-desktop)

<br><hr><br>

## Start Container

เราสามารถที่จะสร้าง Container ขึ้นมาบนเครื่องเราได้ด้วยคำสั่ง `docker run <options> <container_image>`

**container_image เราสามารถหาได้จาก [Docker Hub](https://hub.docker.com/)**

`docker run --interactive --tty alpine`

- `--interactive` หรือ `-i` และ `--tty` เป็น Options ที่ทำให้เราสามารถเข้าไป run คำสั่งใน Container ได้

- `docker run alpine ls` เป็นการ run คำสั่งจากข้างนอกเข้าไปใน Container

เราสามารถ Run container แบบ Background ได้ ด้วยการใส่ `--detach` หรือ `-d`

`docker run --detach -it ubuntu:bionic`

ถ้าเราเขียนแบบ `-d` เราสามารถเอาไปรวมกับ `-it` ได้เป็น `-dit`

`docker run -dit ubuntu:bionic`

<br><hr><br>

## List Containers

เมื่อเราสร้าง Container บนเครื่องเราแล้วเราสามารถที่จะดู Containers บนเครื่องเราได้ว่ามีอะไรที่รันอยู่บ้าง ด้วยคำสั่ง

`docker ps`

<br><hr><br>

## Naming Container

Container สามารถตั้งชื่อให้มันได้ด้วย Option `--name`

`docker run -dit --name my-ubuntu ubuntu:bionic`

<br><hr><br>

## Kill Container

เราสามารถหยุดการทำงานของ container ได้ด้วยคำสั่ง `docker kill <container_id หรือ container_name>`

`docker kill my-ubuntu`

หลังจาก kill container เรียบร้อยแล้ว ให้เราลอง run container ใหม่เราจะไม่สามารถ run container ขึ้นมาได้เนื่องจากว่า container ที่ kill ไปแล้วมันยังมี state ที่ค้างอยู่ในเครื่องให้เราทำการ rm

`docker rm my-ubuntu`

🌟 แนะนำว่าเวลาเรา start container ให้เราใส่ option `--rm` เสมอ ๆ เพื่อให้มัน clear state หลังจากเรา kill container

`docker run --rm -dit --name my-ubuntu ubuntu:bionic`

<br><hr><br>

## Test running server

ข้อดีอย่างแรกเลยของการที่มี Docker คือ เราสามารถที่สร้างสร้าง Container ขึ้นมาแล้วใช้งานได้เลย เช่น เราจะ run container postapp server ขึ้นมาได้เลย โดยที่เราไม่ต้องไปรู้ว่า postapp นั้นเขียนด้วยอะไร มี environment หรือ package อะไรที่เราต้อง install เพื่อ run server บ้าง

`docker run -dit -p 8000:8000 --rm --name mypostapp napatwongchr/postapp`

`-p` เป็นการ map port ข้างใน container ออกมาให้เครื่อง computer ของเราสามารถ access เข้าไปได้อยู่ในรูปแบบของ `<computer_port>:<container_port>`

<br><hr><br>

## Exercise

ให้ลองเล่น Docker CLI ตามนี้

- Run alpine linux container ซึ่งตั้งชื่อ container ให้เป็น "my-alpine"
- Run node container ซึ่งตั้งชื่อ container ให้เป็น "my-node-app"
- ทำการ list containers บนเครื่องคอมพิวเตอร์
- ทำการ run ls ใน “my-alpine” container
- ทำการ kill “my-alpine” ทิ้ง
- ทำการ run “my-alpine” ขึ้นมาใหม่อีกรอบ

<br><hr><br>

[Table of content](https://github.com/napatwongchr/intro-to-container)
