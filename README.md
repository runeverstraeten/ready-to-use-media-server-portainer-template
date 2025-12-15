# 🎬 Ready-to-use Media Server Portainer Templates

## 🇬🇧 English
This repository contains **Portainer Application Templates** and **Docker Compose Stacks** for deploying a complete self-hosted media server using Docker.

The templates are based on **LinuxServer.io** images and are designed to be:
✔ **Fully configurable** (paths, ports, PUID / PGID / TZ)
✔ **Portable** across different Docker hosts
✔ **Easy to deploy** via Portainer (App Templates OR Stacks)
✔ **Suitable** for home servers, NAS setups, CasaOS, and Docker Desktop

### 📦 Available Apps
* **Plex Media Server** (Media Player)
* **Overseerr** (Request Management)
* **Tautulli** (Plex Monitoring & Statistics)
* **Radarr** (Movie Manager)
* **Sonarr** (TV Series Manager)
* **Bazarr** (Subtitle Manager)
* **Jackett** (Indexer Proxy)
* **Transmission** (Torrent Client)

---

### 🚀 Method 1: Portainer App Templates (Single Apps)
*Best if you want to install specific apps one by one via the "App Templates" menu (GUI).*

1.  **Open Portainer** → **Settings** → **App Templates**.
2.  Select **"Use custom templates"**.
3.  **Add the following URL** to the "URL" field:
    ```text
    [https://raw.githubusercontent.com/runeverstraeten/ready-to-use-media-server-portainer-template/refs/heads/main/templates/mediaserver-template.json](https://raw.githubusercontent.com/runeverstraeten/ready-to-use-media-server-portainer-template/refs/heads/main/templates/mediaserver-template.json)
    ```
4.  Click **Save Settings**.
5.  Go to **App Templates** in the left menu. You will now see the Media Server templates. Click one to deploy it.

### 🚀 Method 2: Portainer Stack / CasaOS (Server/NAS)
*Best if you want to deploy the entire suite at once on a Linux Server or NAS.*

1.  **Open the link below** to view the raw Compose file:
    [View mediaserver-compose-template.yaml](https://raw.githubusercontent.com/runeverstraeten/ready-to-use-media-server-portainer-template/refs/heads/main/templates/mediaserver-compose-template.yaml)
2.  **Copy** the entire content of that file.
3.  **Open Portainer** → **Stacks** → **Add Stack** (or "Custom Install" in CasaOS).
4.  Name your stack (e.g., `media-stack`).
5.  **Paste** the copied content into the **Web Editor**.
6.  **Important:** Adjust the Volume paths (left side of the `:`) to match your host storage locations.
7.  Click **Deploy the stack**.

### 💻 Method 3: Docker Desktop (Windows / macOS)
*Use this version if you are running Docker on Windows or Mac. It uses distinct port mapping instead of host-mode networking.*

1.  **Open the link below** to view the Desktop-optimized Compose file:
    [View mediaserver-desktop-template.yaml](https://raw.githubusercontent.com/runeverstraeten/ready-to-use-media-server-portainer-template/refs/heads/main/templates/mediaserver-desktop-template.yaml)
2.  **Copy** the content.
3.  Create a `docker-compose.yaml` file on your computer.
4.  **Paste** the content and run `docker compose up -d` via your terminal.

---

## 🇳🇱 Nederlands
Deze repository bevat **Portainer Application Templates** en **Docker Compose Stacks** om een volledige zelf-gehoste mediaserver te deployen via Docker.

De templates zijn gebaseerd op **LinuxServer.io** images en zijn ontworpen om:
✔ **Volledig configureerbaar** te zijn (paden, poorten, PUID / PGID / TZ)
✔ **Draagbaar** te zijn naar verschillende Docker hosts
✔ **Makkelijk te deployen** via Portainer (App Templates OF Stacks)
✔ **Geschikt** voor thuisservers, NAS setups, CasaOS en Docker Desktop

### 📦 Beschikbare Apps
* **Plex Media Server** (Mediaspeler)
* **Overseerr** (Beheer van verzoeken)
* **Tautulli** (Plex monitoring & statistieken)
* **Radarr** (Filmbeheer)
* **Sonarr** (TV-seriebeheer)
* **Bazarr** (Ondertitelbeheer)
* **Jackett** (Indexers)
* **Transmission** (Torrent client)

---

### 🚀 Methode 1: Portainer App Templates (Losse Apps)
*Het beste als je specifieke apps één voor één wilt installeren via het "App Templates" menu (GUI).*

1.  **Open Portainer** → **Settings** → **App Templates**.
2.  Vink de optie **"Use custom templates"** aan.
3.  **Voeg de volgende URL toe** aan het "URL" veld:
    ```text
    [https://raw.githubusercontent.com/runeverstraeten/ready-to-use-media-server-portainer-template/refs/heads/main/templates/mediaserver-template.json](https://raw.githubusercontent.com/runeverstraeten/ready-to-use-media-server-portainer-template/refs/heads/main/templates/mediaserver-template.json)
    ```
4.  Klik op **Save Settings**.
5.  Ga naar **App Templates** in het linkermenu. Je ziet nu de Media Server templates. Klik erop om te installeren.

### 🚀 Methode 2: Portainer Stack / CasaOS (Server/NAS)
*Het beste als je de volledige suite in één keer wilt deployen op een Linux Server of NAS.*

1.  **Open onderstaande link** om het ruwe Compose bestand te bekijken:
    [Bekijk mediaserver-compose-template.yaml](https://raw.githubusercontent.com/runeverstraeten/ready-to-use-media-server-portainer-template/refs/heads/main/templates/mediaserver-compose-template.yaml)
2.  **Kopieer** de volledige inhoud van dat bestand.
3.  **Open Portainer** → **Stacks** → **Add Stack** (of "Custom Install" in CasaOS).
4.  Geef je stack een naam (bijv. `media-stack`).
5.  **Plak** de gekopieerde tekst in de **Web Editor**.
6.  **Belangrijk:** Pas de Volume-paden (linkerkant van de `:`) aan naar jouw opslaglocaties.
7.  Klik op **Deploy the stack**.

### 💻 Methode 3: Docker Desktop (Windows / macOS)
*Gebruik deze versie als je Docker op Windows of Mac draait. Deze versie gebruikt specifieke poort-mapping in plaats van host-mode netwerken.*

1.  **Open onderstaande link** om het Desktop-geoptimaliseerde bestand te bekijken:
    [Bekijk mediaserver-desktop-template.yaml](https://raw.githubusercontent.com/runeverstraeten/ready-to-use-media-server-portainer-template/refs/heads/main/templates/mediaserver-desktop-template.yaml)
2.  **Kopieer** de inhoud.
3.  Maak een `docker-compose.yaml` bestand aan op je computer.
4.  **Plak** de inhoud erin en start de stack via je terminal met `docker compose up -d`.

---

### 📌 Extra info
* **Best practices:** Gebruik bij voorkeur hardlinks & correcte volume mapping om dubbele opslagruimte te voorkomen.
* **Validatie:** De bestanden zijn gevalideerd voor compatibiliteit met Portainer CE en BE.
