🎬 Ready-to-use Media Server Portainer Templates

🇬🇧 English
This repository contains Portainer Application Templates for deploying a complete self-hosted media server stack using Docker.

The templates are based on LinuxServer.io images and are designed to be:

✔ Fully configurable (paths, ports, PUID / PGID)  
✔ Portable across different Docker hosts  
✔ Easy to deploy via Portainer  
✔ Suitable for home servers and NAS setups

📦 Available Templates
- Plex Media Server
- Transmission
- Radarr
- Sonarr
- Jackett

🛠 Requirements
- Docker
- Portainer (Community or Business Edition)
- Linux-based Docker host (amd64 or arm64)

🚀 Installation (Portainer)
1️⃣ Add the template repository  
Open Portainer → Settings → App Templates  
Add the following URL:  
https://raw.githubusercontent.com/runeverstraeten/ready-to-use-media-server-portainer-template/refs/heads/main/templates/mediaserver-compose-template.yaml

2️⃣ Go to App Templates and deploy the applications you want.

📌 Release Notes / Additional Notes
- **Initial public release** of ready-to-use Portainer templates for a self-hosted media server stack  
- **Best practices for hardlinks & storage** are recommended when mapping your media directories to avoid duplicate storage  
- **Validation of templates.json** has been done to ensure compatibility with Portainer and correct deployment of all containers

---

🇳🇱 Nederlands
Deze repository bevat Portainer Application Templates om een volledige zelf-gehoste mediaserver stack te deployen via Docker.

De templates zijn gebaseerd op LinuxServer.io images en zijn ontworpen om:

✔ Volledig configureerbaar (paden, poorten, PUID / PGID)  
✔ Draagbaar naar verschillende Docker hosts  
✔ Makkelijk te deployen via Portainer  
✔ Geschikt voor thuisservers en NAS setups

📦 Beschikbare Templates
- Plex Media Server
- Transmission
- Radarr
- Sonarr
- Jackett

🛠 Vereisten
- Docker
- Portainer (Community of Business Edition)
- Linux-based Docker host (amd64 of arm64)

🚀 Installatie (Portainer)
1️⃣ Voeg de template repository toe  
Open Portainer → Instellingen → App Templates  
Voeg de volgende URL toe:  
https://raw.githubusercontent.com/runeverstraeten/ready-to-use-media-server-portainer-template/refs/heads/main/templates/mediaserver-compose-template.yaml

2️⃣ Ga naar App Templates en deploy de applicaties die je wilt.

📌 Release Notes / Extra info
- **Eerste publieke release** van ready-to-use Portainer templates voor een zelf-gehoste mediaserver stack  
- **Best practices voor hardlinks & storage** worden aangeraden bij het mappen van je mediapaden om dubbele opslag te voorkomen  
- **Validatie van templates.json** is uitgevoerd om compatibiliteit met Portainer te garanderen en correcte deployment van alle containers
