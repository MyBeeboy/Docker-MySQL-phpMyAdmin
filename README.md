# Docker-MySQL-phpMyAdmin
## การกำหนด Networks:
```
networks:
  dev_network:
```
สร้างเครือข่ายเพื่อให้ MySQL และ phpMyAdmin อยู่ใน network เดียวกัน (dev_network) เพื่อให้สามารถเชื่อมต่อระหว่าง containers ได้
## การกำหนด Volumes:
```
volumes:
  icoop_data:
```
กำหนด Volume เพื่อเก็บข้อมูลของ MySQL ภายใน container

## การกำหนด MySQL Container:
```
icoop: 
  image: mysql:latest
  container_name: icoop
  volumes:
    - icoop_data:/var/lib/mysql
  restart: always
  command: mysqld --sql_mode="IGNORE_SPACE" --log_bin_trust_function_creators=1 --low-priority-updates --max_allowed_packet=2G --innodb-buffer-pool-size=200M --net_write_timeout=3600 --net_read_timeout=3600
  environment:
    MYSQL_ROOT_PASSWORD: pass123
    TZ: Asia/Bangkok
  ports:
    - '3309:3306'
  networks:
    - dev_network
```
* **image:** กำหนดให้ใช้ image mysql:latest สำหรับสร้าง container ของ MySQL
* **container_name:** ชื่อของ container MySQL จะถูกตั้งเป็น icoop
* **volumes:** Mount โฟลเดอร์ icoop_data เข้าไปที่ /var/lib/mysql ใน container เพื่อเก็บข้อมูล
* **restart:** กำหนดให้ MySQL container รันอัตโนมัติเมื่อมีการ shutdown
* **command:** กำหนดคำสั่งที่จะให้ MySQL server รันเมื่อ container ถูกสร้างขึ้น
* **environment:** กำหนด environment variables เช่น MYSQL_ROOT_PASSWORD และ TZ (time zone)
* **ports:** กำหนดการเชื่อมโยงพอร์ตเพื่อให้สามารถเข้าถึง MySQL ได้ผ่านพอร์ต 3309 ของเครื่อง Host
## การกำหนด phpMyAdmin Container:
```
phpmyadmin:
  image: phpmyadmin/phpmyadmin
  container_name: myadmin
  restart: always
  ports:
    - '8888:80'
  environment:
    PMA_ARBITRARY: 1
    UPLOAD_LIMIT: 50G
    MEMORY_LIMIT: 50G
  networks:
    - dev_network
```
* **image:** กำหนดให้ใช้ image phpmyadmin/phpmyadmin สำหรับสร้าง container ของ phpMyAdmin
* **container_name:** ชื่อของ container phpMyAdmin จะถูกตั้งเป็น myadmin
* **restart:** กำหนดให้ phpMyAdmin container รันอัตโนมัติเมื่อมีการ shutdown
* **ports:** กำหนดการเชื่อมโยงพอร์ตเพื่อให้สามารถเข้าถึง phpMyAdmin ผ่านพอร์ต 8888 ของเครื่อง Host
* **environment:** กำหนด environment variables เช่น PMA_ARBITRARY, UPLOAD_LIMIT, MEMORY_LIMIT สำหรับการกำหนดค่าต่างๆ ของ phpMyAdmin
