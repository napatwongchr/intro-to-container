# Deploy Django Application To Digital Ocean

DigitalOcean คือ cloud hosting provider

## Let's Deploy Django App

1. ให้เราไปสมัคร [Digital Ocean](https://www.digitalocean.com/) ก่อน

**การสมัครอาจจะต้องใส่บัตรเครดิต**

2. จากนั้นให้เราลอง Login เข้าระบบ เราจะเจอกับ Dashboard

3. เราจะไป Setup Project กันก่อน ด้วยการสร้าง Folder `my-first-deploy-django`

4. จากนั้นสร้าง virtual environment ด้วย `python -m venv env`

5. ทำการ activate virtual environment

   - ใน MacOS ใช้คำสั่ง "source env/bin/activate"
   - ใน Windows ใช้คำสั่ง "env/Scripts/activate"

6. จากนั้นทำการ install packages ด้วยคำสั่ง `pip install django gunicorn psycopg2-binary dj-database-url django-environ`

Packages ที่เรา install มีทั้งหมดดังนี้

django - django framework
gunicorn - เครื่องมือสำหรับ deploy django ด้วย WSGI
dj-database-url - django tool สำหรับ parse database url ให้เป็น format ที่ทำงานได้
psycopg2 - เป็นตัว PostgreSQL adapter เอาไว้ต่อกับ PostgreSQL database
django-environ - เป็นตัวอ่านไฟล์ .env

7. ต่อไปเราจะทำการ freeze packages ด้วยคำสั่ง `pip freeze > requirements.txt`

**เวลาเรา Install packages อะไรก็แล้วแต่ให้เรา freeze packages ทุกครั้ง**

8. จากนั้นทำการ create project `django-admin startproject django_app`

9. เราจะทำการปรับแต่ง `settings.py` กันสักหน่อย

   9.1 เราจะทำการ `import environ` เพื่อที่จะอ่าน Environment Variables จากไฟล์ `.env` หน้าตาจะเป็นแบบนี้

   ```
    DJANGO_SECRET_KEY=this-is-your-generated-secret-key
    DEBUG=True
    DJANGO_ALLOWED_HOSTS=127.0.0.1,localhost
   ```

   จากนั้นทำการอ่านด้วย code แบบนี้

   ```
   import environ

    env = environ.Env()
    environ.Env.read_env()
   ```

   9.2 ทำการอ่าน secret key จาก Env Vars

```
SECRET_KEY = env("DJANGO_SECRET_KEY", default=get_random_secret_key())
```

**- ถ้าเราไม่ใส่ DJANGO_SECRET_KEY ที่ .env secret key จะเปลี่ยนเรื่อย ๆ**

- เราสามารถ gen secret key ได้ที่นี่ https://djecrety.ir ถ้าอยากได้ secret key ใหม่

  9.3 setup DEBUG

  ```
  DEBUG = env("DEBUG", default=False)
  ```

  9.4 setup ALLOW_HOSTS เพื่อกำหนดว่า request มาจากแหล่งไหนที่สามารถใช้งาน django ได้

  ```
  ALLOWED_HOSTS = env("DJANGO_ALLOWED_HOSTS", default="127.0.0.1,localhost").split(",")
  ```

  9.5 setup DATABASES

  - ตรงนี้ข้ามไปได้ถ้าเรายังไม่ได้ต่อ Database
    **- ทำไมต้องใช้ dj_database_url ?**

  เนื่องจากว่าการต่อ PostgreSQL จะใช้ url ที่มีลักษณะหน้าตาประมาณนี้ `postgres://user:password@localhost:5432/database` เพื่อให้ django อ่านได้เราจำเป็นต้องใช้ dj_database_url ช่วยแปล

  ```
  import dj_database_url

  DATABASES = {
    'default': dj_database_url.config(default=os.getenv("DATABASE_URL"))
  }
  ```

  จากนั้นเพิ่ม `DATABASE_URL` เข้าไปที่ `.env` ด้วยน

10. ให้ทำการสร้าง repository ใน folder `django_app`

11. Github repository สำหรับ django app

12. เมื่อสร้าง Github repository เสร็จแล้ว ให้ทำการเพิ่ม `.gitignore` ไปที่ Root folder แล้วใส่ files และ folder ตามนี้

```
db.sqlite3
*.pyc
env/
.env
```

13. ทำการ push code ขึ้น Git repository

14. เข้าไปที่ Dashboard จากนั้นเลือก App ที่ Sidebar

![Select App Menu](./images/do-deploy-django-dashboard.png)

15. จากนั้นกด Create App

![Create App On DigitalOcean](./images/do-deploy-create-app.png)

16. เลือก Github

![Link source repo](./images/do-deploy-link-source.png)

17. เลือก Github Repository

![Link source repo](./images/do-deploy-import-repo.png)

18. ต่อไปเราจะสามารถ Config Django App เราได้

- สี่งที่สำคัญคือ Environment Variables ในที่นี้ให้เราใส่ให้ครบ ยกเว้น `DJANGO_ALLOW_HOSTS` เราจะได้ domain มาหลังจากเรา deploy เสร็จแล้ว

- อีกหนึ่งอย่างให้เรา Edit run command ใส่ `gunicorn --worker-tmp-dir /dev/shm django_app.wsgi`

![Config Django App](./images/do-deploy-config.png)

19. จากนั้นให้กด Next เราจะได้เลือก region ที่เราจะ deploy ก็ให้เลือก region ที่ user ของเราอยู่

![Setup deploy region](./images/do-deploy-area.png)

20. กด Next ต่อไปเราจะเจอกับ Plan ก็ให้เราเลือกตามความเหมาะสม

![Select plan](./images/do-deploy-plan.png)

21. เลือกเสร็จแล้วก็ให้กด Deploy ได้เลย

![Launch](./images/do-deploy-launch-app.png)

22. ถ้าเสร็จเรียบร้อยเราจะสามารถเข้าที่ url ที่เค้าให้มาได้

![Complete](./images/do-complete.png)
