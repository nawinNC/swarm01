## อธิบาย Docker-compose.yaml
**กำหนด version ของ Docker-compose ที่เราต้องการ โดยต้องเป็น version " 3 " ขึ้นไป**

- version: '3.3' 

**กำหนด Images ที่ต้องการ โดย images ที่ใช้ในนี้คือ Images ที่อยู่ใน dockerhub ชื่อ " nawinnc/plex:0700 "**

- image: nawinnc/plex:0700 

**กำหนด network ให้คอนเทนเนอร์โดย network ที่ใช้คือ " webproxy "**

- networks: - webproxy 

**กำหนด logging ว่าเราจะใช้ driver อะไรในการเก็บข้อมูล โดย driver ที่กำหนดคือ " json-file "**

- logging: driver: json-file 

**กำหนดการ deploy โดยกำหนดไว้ 1 replicas**

- deploy: replicas: 1 

**กำหนด traefik ให้ใช้ network ชื่อ " webproxy "**

- traefik.docker.network=webproxy

**กำหนดให้ Traefik ใช้ entrypoint เป็น " websecure "**

- traefik.http.routers.${APPNAME}-https.entrypoints=websecure

**กำหนด Traefik ให้ใช้ router นี้คือ " winncplex.xops.ipv9.me " ในการ handle request**

- traefik.http.routers.${APPNAME}-https.rule=Host("${APPNAME}.xops.ipv9.me")

**กำหนด Traefik ให้ใช้ certresolver ชื่อ " default " ในการดึง TLS**

- traefik.http.routers.${APPNAME}-https.tls.certresolver=default

**กำหนดให้ Traefik ใช้  port : " 32400 "**

- traefik.http.services.${APPNAME}.loadbalancer.server.port=32400

**กำหนด  resources ที่ใช้**

- resources: <br>
         reservations: <br>
           cpus: '0.1'<br>
           memory: 10M<br>
         limits: <br>
           cpus: '0.4'<br>
           memory: 150M
- กำหนดขั้นต่ำ cpu "0.1" memory "10M"
- กำหนดสูงสุด cpu "0.4" memory "150M"
