## อธิบาย Compose.yaml

**กำหนด images ที่ plex จะใช้งานโดย images ที่ใช้งาน ชื่อ " linuxserver/plex "**

- image: linuxserver/plex

**กำหนดชื่อของ container โดยชื่อที่กำหนด คือ " plex "**

- container_name: plex

**กำหนด mode ของ network โดยชื่อที่กำหนด คือ " host "**

- network_mode: host

**กำหนด environment และกำหนด version ให้เป็น " docker "**

- environment:- VERSION=docker

**กำหนดให้ service นั้น restart ตัวเองอัตโนมัติเมื่อมีข้อผิดพลาด หรือสั่งให้เริ่มต้นทำงานอัตโนมัติเมื่อเปิดเครื่องขึ้นมาใหม่**

- restart: always

**เชื่อม volume นี้กับ service โดยใช้**

- volumes:- ${PLEX_MEDIA_PATH}:/media/
