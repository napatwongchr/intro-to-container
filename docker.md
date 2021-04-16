# Docker

Docker คือ **Command line tools** ที่ไว้ใช้สำหรับจัดการ Containers

Docker Desktop คือ **โปรแกรมที่เอาไว้จัดการ Containers**

[ติดตั้งได้ที่นี่](https://www.docker.com/products/docker-desktop)

<br><hr><br>

## Docker Command Line

1. `docker run --interactive --tty alpine`

2. `docker run alpine ls`

3. `docker run ubuntu:bionic ls`

4. `docker run --detach -it ubuntu:bionic`
   run linux alpine container แบบ background และทำการสั่งคำสั่ง ls

5. `docker ps`
   list containers ในเครื่องคอมของเรา

6. `docker kill <ids / name of container>`
   ทำการหยุดการทำงานของ container

7. `docker run -dit --name my-ubuntu ubuntu:bionic`
   ทำการ run container แล้วตั้งชื่อให้ container my-ubuntu

8. `docker kill my-ubuntu`
   ทำการหยุดการทำงานของ container แต่สังเกตว่าถ้าเรา run container อีกทีมันจะบอกว่ายัง run ไม่ได้

9. `docker run --rm -dit --name my-ubuntu ubuntu:bionic`
   ทำการ run แล้วใส่ --rm เพื่อ kill container แล้วจะได้ไม่ต้อง cleanup ตาม

<br><hr><br>

## Exercise

ให้ลองเล่น Docker CLI ตามนี้

- Run alpine linux container ซึ่งตั้งชื่อ container ให้เป็น “my-alpine”
- Run node container ซึ่งตั้งชื่อ container ให้เป็น “my-node-app”
- ทำการ List containers บนเครื่องเรา แบบ background
- ทำการ List images บนเครื่องเรา
- ทำการ Run bash ใน “my-alpine” container
- ทำการ Kill “my-alpine” ทิ้ง
- ทำการ run “my-alpine” ขึ้นมาใหม่อีกรอบ
