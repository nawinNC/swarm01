# Ref
- [https://github.com/docker/awesome-compose/tree/master/plex]

# URL wakatime swarm01
- [https://wakatime.com/@spcn15/projects/dvkdfnxive] 

# สร้าง VM template
  * Os Ubuntu 22.04
  * Cpu 2 core
  * Memory 2024 MB
  * Storege 32 GB
  
## หลังจากสร้าง VM
 * ตั้งเวลาไทม์โซน
``` 
 timedatectl set-timezone Asia/Bangkok
```
# Clone VM มา 3 node ได้แก่ Manager Work1 Work2
* เปลี่ยน Hostname VM แต่ละ node 
   * Node 1 คือ "manager"
   * Node 2 คือ "work1"
   * Node 3 คือ "work2"
``` 
 sudo -i 
``` 
``` 
 hostnamectl set-hostname “Change-me”
```
* เช็คและเปลี่ยน ip ของแต่ละ node ให้ไม่ซ้ำกัน
```
ip a
```
```
cp /dev/null /etc/machine-id
rm /var/lib/dbus/machine-id
ln -s /etc/machine-id /var/lib/dbus/machine-id
init 0
```
 * ติดตั้ง Docker for Ubuntu ลงในแต่ละ node
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

### Run this code for docker authorited
```
sudo usermod -aG docker $USER
docker ps
```
 
## เตรียม node Ubuntu 22.04 ที่โคลนจาก template / Prepare node Ubuntu 22.04 clon from template
  * Node1 manager node 
  * Node2 work1
  * Node3 work2
 
### หลังจากโคลน / After clone node
 * Use sudo mode for set time and hostname (all three node) / ใช้ sudo mode เพื่อตั้งเวลา และ hostname
``` 
 sudo -i 
``` 
 * ตั้ง hostname สำหรับในแต่ละโหนด / Set hostname for in each node
   * Node 1 set hostname to "manager"
   * Node 2 set hostname to "work01"
   * Node 3 set hostname to "work2"
``` 
 hostnamectl set-hostname “Change-me”
``` 
## ใน manager node 
#### สามารถเรียกใช้คำสั่งนี้เพื่อดูโหนด ip สำหรับ remote ssh จาก VS code (manager) และ VS code ต้องใช้ส่วนขยาย ssh remote / Can run this command to see node ip for remote ssh with VS code (manager node) and Vs code requires extension ssh remote 
``` 
 ip a
```
#### หลังจาก เชื่อม ssh remote กับ vs code ให้ติดตั้งส่วนขยาย ดังนี้ / After ssh remote with vs code. Install this extensions
  * Docker
  * Git hub
  * Wakatime and enter api key
#### ใช้คำสั่งนี้เพื่อสร้าง token / Run this command to gennarate token for run on work node
``` 
 docker swarm init
```
  * And copy token for run on work node
#### หลังจากใช้ token บน work node แล้วใช้คำสั่งนี้เพื่อเช็คสมาชิก node / After enter token on work node can run this code for check member node
``` 
 docker node ls
```
#### ปรับใช้ portainer สำหรับ swarm / Deploy portainer for swarm 
``` 
 curl -L https://downloads.portainer.io/ce2-17/portainer-agent-stack.yml -o portainer-agent-stack.yml
docker stack deploy -c portainer-agent-stack.yml portainer
```
  * go to this port https://localhost:9443
## In work node ( work1,work2 )
  * Can switch manager node to work node by this code ( on Vs code )
``` 
 ssh target_ip_local
```
  * Paste token on work node (all two node)

## Switch to manager mode agian 
  * Create file git config
``` 
 git config --global user.name "github_username"
 git config --global user.email "github_useremail"
```
  * Create folder swarm01 and swarm02
``` 
mrkdir swarm01
mrkdir swarm02
```
  * Git init in swarm01 and swarm02
``` 
Git init
```
  * ไปที่ swarm01 เพื่อเลือก application potainer / Go to swarm01 for choose application potainer frome (https://github.com/docker/awesome-compose/tree/master/portainer)
  * สร้าง README.md และ compose.yaml / Create README.md and compose.yaml file
  * เรียกใช้คำสั่งนี้เพื่อส่งไฟล์ / Run this command to commit file
``` 
git commit -m "openProject"
```
  * Run this command for push to github
``` 
git push origin master
```

## ยังไม่เสร็จสมบูรณ์นะครับ
