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

![image](https://user-images.githubusercontent.com/115439255/224511759-6dbef96d-f1c8-4211-8ef9-6d14014e45df.png)

 
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
 
 ![image](https://user-images.githubusercontent.com/115439255/224511745-572a2da8-c51a-469f-8b4a-754475e73769.png)

 
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
![image](https://user-images.githubusercontent.com/115439255/224511718-c4b7fd44-6b76-4097-b807-2da6a87749ef.png)


## Deploy portainer สำหรับใช้กับ swarm
``` 
 curl -L https://downloads.portainer.io/ce2-17/portainer-agent-stack.yml -o portainer-agent-stack.yml
 docker stack deploy -c portainer-agent-stack.yml portainer
```
## ติดตั้ง Traefik 
**สร้าง Folder ของ traefik และ สร้าง File ใน Folder ชื่อ traefik-host.yml ดังรูป**

![image](https://user-images.githubusercontent.com/115439255/224510864-f3337066-9976-4004-9f79-c328dcd8176c.png)

**โดยข้อมูลใน File อ้างอิงมาจาก [ https://github.com/pitimon/dockerswarm-inhoure/tree/main/ep03-traefik ]**

**เข้าไปที่ C:\Windows\System32\drivers\etc และเข้าไปทำการเพิ่ม domain ของ traefik ใน file hosts**

![image](https://user-images.githubusercontent.com/115439255/224511257-6156c833-74ac-4799-b9ce-56b75353dd68.png)
![image](https://user-images.githubusercontent.com/115439255/224511390-9e17114c-9e77-41ac-994c-82d764d5ab08.png)

**Create a network**
``` 
 docker network create --driver=overlay traefik-public
```
**Create a tag**
```
 export NODE_ID=$(docker info -f '{{.Swarm.NodeID}}')
 echo $NODE_ID
```
```
 docker node update --label-add traefik-public.traefik-public-certificates=true $NODE_ID
```
**สามารถเปลี่ยน Email Domain Username Password ได้ตามที่เราต้องการ
```
export EMAIL=user@smtp.com
export DOMAIN=traefik.cpedemo.local
export USERNAME=admin
export PASSWORD=changeMe
export HASHED_PASSWORD=$(openssl passwd -apr1 $PASSWORD)
echo $HASHED_PASSWORD
```
**deploy traefik**
```
docker stack deploy -c traefik-host.yml traefik
```
**สามารถทดลองเข้าเว็บได้โดย : traefik.cpedemo.local**
![image](https://user-images.githubusercontent.com/115439255/224511599-14da61f8-9473-4c42-ac3a-a320ff8b67d9.png)

## 
