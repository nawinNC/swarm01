## Ref
- https://github.com/docker/awesome-compose/tree/master/plex

## URL wakatime swarm01
- https://wakatime.com/@spcn15/projects/dvkdfnxive 

## URL plexapplication
- https://winncplex.xops.ipv9.me

## สร้าง VM
 * OS Ubuntu 22.04
 * CPU 2 core
 * Memory 2024 MB
 * Storege 32 GB
 
## ตั้ง timezone
 ```
 sudo -i        // เข้า root
 ```
 ```
 timedatectl set-timezone Asia/Bangkok       // ตั้ง timezone
 ```

## Clone VM มา 3 node ได้แก่ Manager Work1 Work2
* เปลี่ยน Hostname VM แต่ละ node 
   * Node 1 คือ "manager"
   * Node 2 คือ "work1"
   * Node 3 คือ "work2"
 ``` 
 sudo -i        // เข้า root
 ``` 
 ``` 
 hostnamectl set-hostname “ ชื่อที่เราต้องการ ”      // ตั้งชื่อ hostname 
 ```
* เช็คและเปลี่ยน ip ของแต่ละ node ให้ไม่ซ้ำกัน
 ```
 ip a          // เช็ด ip
 ```
 ```
 cp /dev/null /etc/machine-id
 rm /var/lib/dbus/machine-id
 ln -s /etc/machine-id /var/lib/dbus/machine-id
 init 0
 ```
## ติดตั้ง Docker for Ubuntu ลงในแต่ละ node
 ``` 
 apt update ; apt upgrade -y 
 apt-get install \
    ca-certificates \
    curl wget \
    gnupg \
    lsb-release -y
 ```
 ```
 mkdir -m 0755 -p /etc/apt/keyrings
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
 ```
 ```
 echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" |  tee /etc/apt/sources.list.d/docker.list > /dev/null
 ```
 ```
 apt-get update
 apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
 ```  
 ```
 sudo usermod -aG docker $USER
 docker ps
 ```
# ติดตั้ง Swarm
``` 
 docker swarm init      // สร้าง token ของ swarm ในเครี่อง manager
```
* และนำ token ที่ได้มาไปใช้ใน work node
``` 
 docker node ls        // ดู node ที่เชื่อมต่อกับ swarm
```
## Deploy portainer สำหรับใช้กับ swarm
