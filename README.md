# 🎬 Ready-to-use Media Server Portainer Templates

## 🇬🇧 English
This repository contains **Portainer Application Templates** and a **Docker Compose Stack** for deploying a complete self-hosted media server using Docker.

The templates are based on **LinuxServer.io** images and are designed to be:
✔ **Fully configurable** (paths, ports, PUID / PGID / TZ)
✔ **Portable** across different Docker hosts
✔ **Easy to deploy** via Portainer (App Templates OR Stacks)
✔ **Suitable** for home servers, NAS setups, and CasaOS

### 📦 Available Apps
* **Plex Media Server** (Media Player)
* **Overseerr** (Request Management)
* **Tautulli** (Plex Monitoring & Statistics)
* **Radarr** (Movie Manager)
* **Sonarr** (TV Series Manager)
* **Bazarr** (Subtitle Manager)
* **Jackett** (Indexer Proxy)
* **Transmission** (Torrent Client)

### 🛠 Requirements
* Docker
* Portainer (Community or Business Edition)
* Linux-based Docker host (amd64 or arm64)

---

### 🚀 Installation Method 1: Portainer App Templates (Single Apps)
*Best if you want to install specific apps one by one via the "App Templates" menu (GUI).*

1.  **Open Portainer** → **Settings** → **App Templates**.
2.  Select **"Use custom templates"**.
3.  **Add the following URL** to the "URL" field:
    ```text
    https://raw.githubusercontent.com/runeverstraeten/ready-to-use-media-server-portainer-template/refs/heads/main/templates/mediaserver-template.json
    ```
4.  Click **Save Settings**.
5.  Go to **App Templates** in the left menu. You will now see the Media Server templates. Click one to deploy it.

### 🚀 Installation Method 2: Portainer Stack / CasaOS (All-in-One)
*Best if you want to deploy the entire suite at once via "Stacks" or "Custom Install".*

1.  **Open the link below** to view the raw Compose file:
    [View mediaserver-compose-template.yaml](https://raw.githubusercontent.com/runeverstraeten/ready-to-use-media-server-portainer-template/refs/heads/main/templates/mediaserver-compose-template.yaml)
2.  **Copy** the entire content of that file.
3.  **Open Portainer** → **Stacks** → **Add Stack** (or "Custom Install" in CasaOS).
4.  Name your stack (e.g., `media-stack`).
5.  **Paste** the copied content into the **Web Editor**.
6.  **Important:** Adjust the Volume paths (left side of the `:`) to match your host storage locations.
7.  Click **Deploy the stack**.

---

## 🇳🇱 Nederlands
Deze repository bevat **Portainer Application Templates** en een **Docker Compose Stack** om een volledige zelf-gehoste mediaserver te deployen via Docker.

De templates zijn gebaseerd op **LinuxServer.io** images en zijn ontworpen om:
✔ **Volledig configureerbaar** te zijn (paden, poorten, PUID / PGID / TZ)
✔ **Draagbaar** te zijn naar verschillende Docker hosts
✔ **Makkelijk te deployen** via Portainer (App Templates OF Stacks)
✔ **Geschikt** voor thuisservers, NAS setups en CasaOS

### 📦 Beschikbare Apps
* **Plex Media Server** (Mediaspeler)
* **Overseerr** (Beheer van verzoeken)
* **Tautulli** (Plex monitoring & statistieken)
* **Radarr** (Filmbeheer)
* **Sonarr** (TV-seriebeheer)
* **Bazarr** (Ondertitelbeheer)
* **Jackett** (Indexers)
* **Transmission** (Torrent client)

### 🛠 Vereisten
* Docker
* Portainer (Community of Business Edition)
* Linux-based Docker host (amd64 of arm64)

---

### 🚀 Installatie Methode 1: Portainer App Templates (Losse Apps)
*Het beste als je specifieke apps één voor één wilt installeren via het "App Templates" menu (GUI).*

1.  **Open Portainer** → **Settings** → **App Templates**.
2.  Vink de optie **"Use custom templates"** aan.
3.  **Voeg de volgende URL toe** aan het "URL" veld:
    ```text
    https://raw.githubusercontent.com/runeverstraeten/ready-to-use-media-server-portainer-template/refs/heads/main/templates/mediaserver-template.json
    ```
4.  Klik op **Save Settings**.
5.  Ga naar **App Templates** in het linkermenu. Je ziet nu de Media Server templates. Klik erop om te installeren.

### 🚀 Installatie Methode 2: Portainer Stack / CasaOS (Alles-in-één)
*Het beste als je de volledige suite in één keer wilt deployen via "Stacks" of "Custom Install".*

1.  **Open onderstaande link** om het ruwe Compose bestand te bekijken:
    [Bekijk mediaserver-compose-template.yaml](https://raw.githubusercontent.com/runeverstraeten/ready-to-use-media-server-portainer-template/refs/heads/main/templates/mediaserver-compose-template.yaml)
2.  **Kopieer** de volledige inhoud van dat bestand.
3.  **Open Portainer** → **Stacks** → **Add Stack** (of "Custom Install" in CasaOS).
4.  Geef je stack een naam (bijv. `media-stack`).
5.  **Plak** de gekopieerde tekst in de **Web Editor**.
6.  **Belangrijk:** Pas de Volume-paden (linkerkant van de `:`) aan naar jouw opslaglocaties.
7.  Klik op **Deploy the stack**.

---

### 📌 Release Notes / Extra info
* **Best practices:** Gebruik bij voorkeur hardlinks & correcte volume mapping om dubbele opslagruimte te voorkomen.
* **Validatie:** De `templates.json` is gevalideerd voor compatibiliteit met Portainer CE en BE.
