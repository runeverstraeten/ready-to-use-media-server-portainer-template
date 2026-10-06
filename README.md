# 🎬 Ready-to-use Media Server Portainer Templates & Stacks

[![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?logo=docker\&logoColor=white)](https://www.docker.com/)
[![Portainer](https://img.shields.io/badge/Portainer-Compatible-13BEF9?logo=portainer\&logoColor=white)](https://www.portainer.io/)
[![LinuxServer.io](https://img.shields.io/badge/Images-LinuxServer.io-009639)](https://www.linuxserver.io/)

Ready-to-use **Portainer Application Templates** and **Docker Compose Stacks** for deploying a complete, self-hosted media server.

The repository is designed to make deploying and maintaining a Docker-based media server as simple as possible, whether you're using a Linux home server, NAS, Raspberry Pi, CasaOS, or Docker Desktop.

---

## 📖 Table of Contents

* [🇬🇧 English](#-english)

  * [Features](#-features)
  * [Included Applications](#-included-applications)
  * [Repository Structure](#-repository-structure)
  * [Installation](#-installation)

    * [Method 1: Portainer Stack](#-method-1-portainer-stack-recommended)
    * [Method 2: Docker Desktop](#-method-2-docker-desktop-windows--macos)
    * [Method 3: Portainer App Templates](#-method-3-portainer-app-templates-single-apps)
  * [Environment Variables](#-environment-variables)
  * [Storage & Hardlinks](#-storage--hardlinks)
  * [Updating Containers](#-updating-containers)
  * [Security](#-security)
* [🇳🇱 Nederlands](#-nederlands)

  * [Kenmerken](#-kenmerken)
  * [Inbegrepen applicaties](#-inbegrepen-applicaties)
  * [Installatie](#-installatie-1)
  * [Omgevingsvariabelen](#-omgevingsvariabelen)
  * [Opslag & Hardlinks](#-opslag--hardlinks)
  * [Containers bijwerken](#-containers-bijwerken)
  * [Beveiliging](#-beveiliging)
* [📄 License](#-license)

---

# 🇬🇧 English

## 📌 About

This repository contains **Portainer Application Templates** and **Docker Compose Stacks** for deploying a complete, self-hosted media server using Docker.

The templates are heavily based on images from **LinuxServer.io** and are designed to be:

* ✔ **Fully configurable** through environment variables
* ✔ **Portable** across different Docker hosts
* ✔ **Easy to deploy** through Portainer
* ✔ **Suitable for Linux home servers**
* ✔ **Suitable for NAS systems**
* ✔ **Compatible with Raspberry Pi**
* ✔ **Compatible with CasaOS**
* ✔ **Compatible with Docker Desktop**
* ✔ **Designed with persistent configuration storage in mind**

You can either deploy the complete media stack at once or install individual applications through Portainer's **App Templates**.

---

## ✨ Features

### Easy deployment

Deploy the complete media server with a single Docker Compose stack, or install individual applications separately.

### Environment-based configuration

Paths, user IDs, time zone settings, database passwords and other configuration options can be supplied through environment variables instead of modifying the Compose files directly.

### Portable configuration

The same Compose files can be used on different Docker hosts by changing the environment variables.

### Persistent configuration

Application configuration is stored outside the containers, allowing containers to be recreated or updated without losing their settings.

### Hardlink-friendly storage layout

The stack is designed around a shared `/DATA` directory structure, allowing applications such as Sonarr and Radarr to create hardlinks between downloads and the final media library.

---

## 📦 Included Applications

| Application           | Purpose                             |
| --------------------- | ----------------------------------- |
| **Jellyfin**          | Open-source media server            |
| **Plex Media Server** | Media server with host network mode |
| **Jellystat**         | Jellyfin monitoring & statistics    |
| **PostgreSQL**        | Database used by Jellystat          |
| **Tautulli**          | Plex monitoring & statistics        |
| **Seerr**             | Media request management            |
| **Radarr**            | Movie management                    |
| **Sonarr**            | TV series management                |
| **Prowlarr**          | Indexer management                  |
| **Transmission**      | Torrent client                      |

### Application overview

```text
                         ┌───────────────┐
                         │     Seerr     │
                         │ Media Requests│
                         └───────┬───────┘
                                 │
                    ┌────────────┴────────────┐
                    │                         │
              ┌─────▼─────┐             ┌────▼─────┐
              │   Radarr  │             │  Sonarr  │
              │   Movies  │             │ TV Series│
              └─────┬─────┘             └────┬─────┘
                    │                         │
                    └────────────┬────────────┘
                                 │
                         ┌───────▼───────┐
                         │    Prowlarr   │
                         │    Indexers   │
                         └───────┬───────┘
                                 │
                         ┌───────▼───────┐
                         │  Transmission │
                         │ Torrent Client│
                         └───────┬───────┘
                                 │
                         ┌───────▼───────┐
                         │     /DATA     │
                         │   Downloads   │
                         │     Media     │
                         └───────┬───────┘
                                 │
                    ┌────────────┴────────────┐
                    │                         │
              ┌─────▼─────┐             ┌────▼─────┐
              │  Jellyfin │             │   Plex   │
              │  Server   │             │  Server  │
              └─────┬─────┘             └────┬─────┘
                    │                         │
              ┌─────▼─────┐             ┌────▼─────┐
              │ Jellystat │             │ Tautulli │
              │ Statistics│             │Statistics│
              └─────┬─────┘             └──────────┘
                    │
              ┌─────▼──────┐
              │ PostgreSQL │
              └────────────┘
```

---

## 📁 Repository Structure

The repository contains the following main deployment files:

```text
.
├── templates/
│   ├── mediaserver-template.json
│   └── mediaserver-compose-template.yaml
│
└── README.md
```

### Portainer App Templates

```text
templates/mediaserver-template.json
```

Contains the individual application templates used by Portainer.

### Docker Compose Stack

```text
templates/mediaserver-compose-template.yaml
```

Contains the complete media server stack and can be deployed directly through Portainer or Docker Compose.

---

# 🚀 Installation

There are three ways to deploy the media server.

| Method                      | Best for                   | Configuration                   |
| --------------------------- | -------------------------- | ------------------------------- |
| **Portainer Stack**         | Linux / NAS / Raspberry Pi | Portainer environment variables |
| **Docker Desktop**          | Windows / macOS            | `.env` file                     |
| **Portainer App Templates** | Individual applications    | Portainer GUI                   |

---

## 🚀 Method 1: Portainer Stack — Recommended

This method is recommended if you want to deploy the **entire media server suite at once**.

The Compose file uses environment variables, so you do not need to modify the YAML configuration manually.

### 1. Open the Compose template

Open the raw Compose file:

**[View `mediaserver-compose-template.yaml`](https://raw.githubusercontent.com/runeverstraeten/ready-to-use-media-server-portainer-template/refs/heads/main/templates/mediaserver-compose-template.yaml)**

### 2. Copy the complete file

Copy the entire contents of the YAML file.

### 3. Open Portainer

Navigate to:

```text
Portainer
→ Stacks
→ Add Stack
```

### 4. Create the stack

Give your stack a name, for example:

```text
media-stack
```

Paste the copied Compose configuration into the **Web Editor**.

### 5. Configure environment variables

Scroll down to **Environment variables** and use **Add environment variable**.

The most important variables are:

| Variable            | Description                                  | Example                    |
| ------------------- | -------------------------------------------- | -------------------------- |
| `APPDATA_DIR`       | Root directory for application configuration | `/DATA/AppData`            |
| `MEDIA_DIR`         | Location of the media library                | `/mnt/media_storage/Media` |
| `DATA_DIR`          | Root directory used for downloads/media      | `/DATA`                    |
| `POSTGRES_PASSWORD` | PostgreSQL password for Jellystat            | `Use-a-secure-password`    |
| `JWT_SECRET`        | Secret used by Jellystat                     | Random 64-character string |
| `PLEX_CLAIM`        | Plex claim token                             | `claim-xxxxxxxx`           |
| `PUID`              | Linux user ID                                | `1000`                     |
| `PGID`              | Linux group ID                               | `1000`                     |
| `TZ`                | Time zone                                    | `Europe/Brussels`          |

### 6. Deploy

Click:

```text
Deploy the stack
```

Portainer will create the containers and associated resources.

---

## 💻 Method 2: Docker Desktop — Windows / macOS

Use this method when running Docker through **Docker Desktop** rather than Portainer.

Instead of entering environment variables through the Portainer interface, you will use a `.env` file.

### 1. Get the Compose file

Download or copy the Compose file into a directory on your computer.

For example:

```text
media-server/
├── docker-compose.yaml
└── .env
```

### 2. Create the `.env` file

Create a file named:

```text
.env
```

### 3. Add your configuration

Example:

```dotenv
APPDATA_DIR=C:\Docker\AppData
MEDIA_DIR=D:\Media
DATA_DIR=D:\Downloads

POSTGRES_PASSWORD=MySecurePassword123
JWT_SECRET=your-random-secret-string
PLEX_CLAIM=claim-xxxxxxxxx

PUID=1000
PGID=1000
TZ=Europe/Brussels
```

> **Important:** Do not commit your `.env` file to GitHub if it contains passwords, claim tokens or other secrets.

### 4. Open a terminal

Navigate to the directory containing your Compose file:

```bash
cd /path/to/media-server
```

### 5. Start the stack

Run:

```bash
docker compose up -d
```

To check the running containers:

```bash
docker compose ps
```

---

## 🧩 Method 3: Portainer App Templates — Individual Apps

Use this method if you only want to install specific applications instead of deploying the entire stack.

### 1. Open Portainer

Navigate to:

```text
Portainer
→ Settings
→ App Templates
```

### 2. Enable custom templates

Select:

```text
Use custom templates
```

### 3. Add the template URL

Use the following URL:

```text
https://raw.githubusercontent.com/runeverstraeten/ready-to-use-media-server-portainer-template/refs/heads/main/templates/mediaserver-template.json
```

### 4. Save the settings

Click:

```text
Save Settings
```

### 5. Deploy applications

Navigate to:

```text
App Templates
```

The available media server templates should now be displayed.

You can install the applications individually through the Portainer GUI.

---

# ⚙️ Environment Variables

The stack is designed to avoid hard-coded paths wherever possible.

## Core variables

### `APPDATA_DIR`

Location where application configuration files are stored.

Example:

```text
/DATA/AppData
```

The applications will create their own configuration directories below this location.

Example:

```text
/DATA/AppData/
├── jellyfin/
├── plex/
├── jellystat/
├── tautulli/
├── seerr/
├── radarr/
├── sonarr/
├── prowlarr/
└── transmission/
```

---

### `MEDIA_DIR`

Location of your media library.

Example:

```text
/mnt/media_storage/Media
```

Possible structure:

```text
Media/
├── Movies/
├── TV Shows/
└── Music/
```

---

### `DATA_DIR`

Root directory used for downloads and media-related data.

Example:

```text
/DATA
```

A recommended structure is:

```text
/DATA/
├── AppData/
├── Downloads/
│   └── torrents/
└── Media/
    ├── Movies/
    └── TV Shows/
```

The exact structure can be adapted to your own storage setup.

---

### `PUID` and `PGID`

These variables determine which Linux user and group the containers run as.

The default is:

```dotenv
PUID=1000
PGID=1000
```

You can find your user and group IDs with:

```bash
id
```

Example output:

```text
uid=1000(user) gid=1000(user)
```

In that case:

```dotenv
PUID=1000
PGID=1000
```

Using the correct IDs helps prevent permission problems between Docker containers and the host filesystem.

---

### `TZ`

Sets the time zone used by the containers.

For Belgium:

```dotenv
TZ=Europe/Brussels
```

---

### `POSTGRES_PASSWORD`

Password used by the PostgreSQL database for Jellystat.

Example:

```dotenv
POSTGRES_PASSWORD=Use-A-Strong-Password
```

Use a strong, unique password.

---

### `JWT_SECRET`

Secret used by Jellystat for authentication/security-related functions.

Use a randomly generated string rather than an easily guessable password.

Example:

```dotenv
JWT_SECRET=your-random-64-character-secret
```

---

### `PLEX_CLAIM`

Plex provides a temporary claim token for linking a new Plex server to your account.

Generate a fresh token through:

```text
https://www.plex.tv/claim/
```

The token is only valid for a limited amount of time, so generate it shortly before deploying the Plex container.

---

# 💾 Storage & Hardlinks

One of the most important considerations when setting up Sonarr, Radarr and a torrent client is the storage structure.

A properly configured directory layout allows **hardlinks** to be used.

## Why hardlinks?

Without hardlinks, moving a downloaded file into your media library may result in the file being copied.

For example:

```text
Downloads/Movie.mkv
Media/Movies/Movie/Movie.mkv
```

could result in the same file occupying storage twice.

With hardlinks, both paths can reference the same underlying data on the filesystem.

This can significantly reduce unnecessary disk usage.

## Recommended structure

Keep downloads and media on the **same filesystem** and preferably underneath the same root directory:

```text
/DATA/
├── Downloads/
│   └── torrents/
│       ├── movies/
│       └── tv/
│
└── Media/
    ├── Movies/
    └── TV Shows/
```

Then ensure the relevant containers see the directories using compatible paths.

> **Important:** Hardlinks generally require the source and destination to be on the same filesystem. Crossing filesystem boundaries will prevent hardlinks from working.

---

# 🔄 Updating Containers

Container configuration is stored outside the containers through the configured application data directory.

This means containers can be recreated or updated without losing their configuration.

In Portainer, you can update a stack/container by using the appropriate **Recreate** or **Update the stack** functionality and enabling:

```text
Pull latest image
```

After updating, the containers will be recreated using the newest available images while retaining their persistent configuration.

## Docker Compose

To pull the latest images:

```bash
docker compose pull
```

Then recreate the containers:

```bash
docker compose up -d
```

To remove unused Docker resources afterward:

```bash
docker system prune
```

> Be careful with Docker cleanup commands. Always verify what will be removed before confirming.

---

# 🔐 Security

This repository contains templates intended for self-hosted environments, but security remains the responsibility of the administrator.

## Never commit secrets

Do **not** commit sensitive values such as:

* PostgreSQL passwords
* JWT secrets
* Plex claim tokens
* API keys
* Access tokens
* Usernames/passwords

If using Docker Desktop, keep sensitive values in `.env` and add it to `.gitignore`:

```gitignore
.env
```

## Use strong passwords

Use unique and sufficiently complex passwords for:

* Jellystat/PostgreSQL
* Plex
* Jellyfin
* Portainer
* Other services exposed to your network

## Limit external access

If these services are exposed to the internet, consider using a reverse proxy, HTTPS and appropriate authentication/access controls.

Do not expose services directly to the public internet unless you understand the security implications.

---

# 🛠️ Troubleshooting

## Permission denied

Check the user and group IDs:

```bash
id
```

Then verify your environment variables:

```dotenv
PUID=1000
PGID=1000
```

Also verify that the Docker user has access to the configured directories.

---

## Hardlinks are not working

Check that:

1. Downloads and media are on the same filesystem.
2. The containers use compatible mount paths.
3. The Docker user has the required permissions.
4. Sonarr/Radarr are not configured to use a separate filesystem for the destination.

You can check your mounted filesystems with:

```bash
df -h
```

---

## Container configuration disappeared

Make sure your configuration directories are mapped to a persistent location such as:

```text
/DATA/AppData
```

Do not store important application configuration exclusively inside the container filesystem.

---

# 🇳🇱 Nederlands

## 📌 Over deze repository

Deze repository bevat **Portainer Application Templates** en **Docker Compose Stacks** voor het eenvoudig opzetten van een volledige, zelf-gehoste mediaserver met Docker.

De templates zijn grotendeels gebaseerd op images van **LinuxServer.io** en zijn ontworpen om:

* ✔ **Volledig configureerbaar** te zijn via Environment Variables
* ✔ **Draagbaar** te zijn tussen verschillende Docker hosts
* ✔ **Eenvoudig te deployen** te zijn via Portainer
* ✔ **Geschikt** te zijn voor Linux thuisservers
* ✔ **Geschikt** te zijn voor NAS-systemen
* ✔ **Geschikt** te zijn voor Raspberry Pi
* ✔ **Geschikt** te zijn voor CasaOS
* ✔ **Compatibel** te zijn met Docker Desktop
* ✔ **Persistente configuratie** te gebruiken

Je kunt de volledige mediaserver in één keer deployen via een Stack, of afzonderlijke applicaties installeren via Portainer App Templates.

---

## 📦 Inbegrepen applicaties

| Applicatie            | Functie                            |
| --------------------- | ---------------------------------- |
| **Jellyfin**          | Open-source mediaserver            |
| **Plex Media Server** | Mediaserver met host network mode  |
| **Jellystat**         | Jellyfin monitoring & statistieken |
| **PostgreSQL**        | Database voor Jellystat            |
| **Tautulli**          | Plex monitoring & statistieken     |
| **Seerr**             | Beheer van mediaverzoeken          |
| **Radarr**            | Filmbeheer                         |
| **Sonarr**            | TV-seriebeheer                     |
| **Prowlarr**          | Indexerbeheer                      |
| **Transmission**      | Torrentclient                      |

---

# 🚀 Installatie

Er zijn drie manieren om de mediaserver te installeren:

| Methode                     | Aanbevolen voor            | Configuratie                    |
| --------------------------- | -------------------------- | ------------------------------- |
| **Portainer Stack**         | Linux / NAS / Raspberry Pi | Portainer Environment Variables |
| **Docker Desktop**          | Windows / macOS            | `.env` bestand                  |
| **Portainer App Templates** | Losse applicaties          | Portainer GUI                   |

---

## 🚀 Methode 1: Portainer Stack — Aanbevolen

Deze methode is ideaal wanneer je de **volledige mediaserver in één keer** wilt installeren.

### 1. Open het Compose-bestand

**[Bekijk `mediaserver-compose-template.yaml`](https://raw.githubusercontent.com/runeverstraeten/ready-to-use-media-server-portainer-template/refs/heads/main/templates/mediaserver-compose-template.yaml)**

### 2. Kopieer de volledige inhoud

Kopieer de volledige YAML-configuratie.

### 3. Open Portainer

Ga naar:

```text
Portainer
→ Stacks
→ Add Stack
```

### 4. Maak de Stack aan

Geef de Stack bijvoorbeeld de naam:

```text
media-stack
```

Plak vervolgens de YAML in de **Web Editor**.

### 5. Stel Environment Variables in

Belangrijke variabelen zijn:

```text
APPDATA_DIR
MEDIA_DIR
DATA_DIR
POSTGRES_PASSWORD
JWT_SECRET
PLEX_CLAIM
PUID
PGID
TZ
```

Bijvoorbeeld:

```text
APPDATA_DIR=/DATA/AppData
MEDIA_DIR=/mnt/media_storage/Media
DATA_DIR=/DATA
PUID=1000
PGID=1000
TZ=Europe/Brussels
```

### 6. Deploy

Klik op:

```text
Deploy the stack
```

Portainer zal vervolgens de benodigde containers aanmaken.

---

# 💻 Methode 2: Docker Desktop — Windows / macOS

Gebruik deze methode wanneer je Docker via Docker Desktop gebruikt.

### 1. Plaats het Compose-bestand

Bijvoorbeeld:

```text
media-server/
├── docker-compose.yaml
└── .env
```

### 2. Maak `.env` aan

Maak in dezelfde map een bestand met de naam:

```text
.env
```

### 3. Voeg je configuratie toe

Bijvoorbeeld:

```dotenv
APPDATA_DIR=C:\Docker\AppData
MEDIA_DIR=D:\Media
DATA_DIR=D:\Downloads

POSTGRES_PASSWORD=MijnVeiligWachtwoord123
JWT_SECRET=een-random-geheime-string
PLEX_CLAIM=claim-xxxxxxxxx

PUID=1000
PGID=1000
TZ=Europe/Brussels
```

### 4. Open een terminal

Ga naar de map:

```bash
cd /path/to/media-server
```

### 5. Start de containers

```bash
docker compose up -d
```

Controleer vervolgens de status:

```bash
docker compose ps
```

---

# 🧩 Methode 3: Portainer App Templates — Losse apps

Deze methode is handig wanneer je slechts bepaalde applicaties wilt installeren.

### 1. Open Portainer

Ga naar:

```text
Portainer
→ Settings
→ App Templates
```

### 2. Gebruik Custom Templates

Selecteer:

```text
Use custom templates
```

### 3. Voeg de template-URL toe

Gebruik:

```text
https://raw.githubusercontent.com/runeverstraeten/ready-to-use-media-server-portainer-template/refs/heads/main/templates/mediaserver-template.json
```

### 4. Sla de instellingen op

Klik op:

```text
Save Settings
```

### 5. Installeer applicaties

Ga vervolgens naar:

```text
App Templates
```

Je kunt nu de afzonderlijke mediaserver-applicaties installeren.

---

# ⚙️ Omgevingsvariabelen

De belangrijkste variabelen zijn:

| Variabele           | Betekenis                    | Voorbeeld                  |
| ------------------- | ---------------------------- | -------------------------- |
| `APPDATA_DIR`       | Configuratiemappen           | `/DATA/AppData`            |
| `MEDIA_DIR`         | Mediamap                     | `/mnt/media_storage/Media` |
| `DATA_DIR`          | Hoofdmap voor data/downloads | `/DATA`                    |
| `POSTGRES_PASSWORD` | PostgreSQL-wachtwoord        | Sterk uniek wachtwoord     |
| `JWT_SECRET`        | Jellystat secret             | Willekeurige string        |
| `PLEX_CLAIM`        | Plex claim token             | `claim-xxxxxxxx`           |
| `PUID`              | Linux User ID                | `1000`                     |
| `PGID`              | Linux Group ID               | `1000`                     |
| `TZ`                | Tijdzone                     | `Europe/Brussels`          |

Je kunt je Linux UID en GID controleren met:

```bash
id
```

---

# 💾 Opslag & Hardlinks

Voor Sonarr, Radarr en Transmission is een correcte opslagstructuur belangrijk.

Een aanbevolen structuur is:

```text
/DATA/
├── AppData/
├── Downloads/
│   └── torrents/
└── Media/
    ├── Movies/
    └── TV Shows/
```

Door `/DATA` correct beschikbaar te maken voor de relevante containers, kunnen Sonarr en Radarr hardlinks gebruiken.

### Voordelen van hardlinks

Zonder hardlinks kan een gedownload bestand volledig worden gekopieerd naar de uiteindelijke mediamap.

Met hardlinks kunnen beide locaties naar dezelfde fysieke data verwijzen.

Dit voorkomt onnodig dubbel opslaggebruik.

> **Let op:** downloads en media moeten zich hiervoor doorgaans op hetzelfde filesystem bevinden.

---

# 🔄 Containers bijwerken

Omdat de configuratie buiten de containers wordt opgeslagen, kunnen containers veilig opnieuw worden aangemaakt zonder hun configuratie te verliezen.

In Portainer kun je bij het opnieuw aanmaken van een container/stack kiezen voor:

```text
Recreate
```

en:

```text
Pull latest image
```

Bij Docker Compose:

```bash
docker compose pull
docker compose up -d
```

---

# 🔐 Beveiliging

Commit nooit gevoelige informatie naar GitHub.

Dit omvat onder andere:

* Wachtwoorden
* PostgreSQL credentials
* JWT secrets
* Plex claim tokens
* API keys
* Access tokens

Wanneer je Docker Desktop gebruikt, bewaar deze informatie in `.env` en voeg `.env` toe aan `.gitignore`:

```gitignore
.env
```

Gebruik daarnaast sterke, unieke wachtwoorden en stel diensten niet zonder verdere beveiligingsmaatregelen rechtstreeks bloot aan het internet.

---

# 📌 Best Practices

## 1. Gebruik persistente configuratie

Bewaar applicatieconfiguratie buiten de containers:

```text
/DATA/AppData
```

Zo blijft de configuratie behouden wanneer containers worden verwijderd of opnieuw aangemaakt.

## 2. Gebruik een consistente opslagstructuur

Bijvoorbeeld:

```text
/DATA/
├── AppData/
├── Downloads/
│   └── torrents/
└── Media/
    ├── Movies/
    └── TV Shows/
```

Dit maakt beheer en hardlinks eenvoudiger.

## 3. Gebruik correcte PUID / PGID

Controleer met:

```bash
id
```

en gebruik de juiste waarden in je Docker-configuratie.

## 4. Gebruik sterke secrets

Gebruik voor:

```text
POSTGRES_PASSWORD
JWT_SECRET
```

altijd unieke en voldoende lange waarden.

## 5. Update images regelmatig

Controleer regelmatig of er nieuwe versies van de gebruikte images beschikbaar zijn.

Bij Docker Compose:

```bash
docker compose pull
docker compose up -d
```

---

# 🤝 Contributing

Pull requests, verbeteringen en suggesties zijn welkom.

Wanneer je een verbetering wilt voorstellen:

1. Fork deze repository.
2. Maak een nieuwe branch.
3. Maak je wijzigingen.
4. Test de wijzigingen.
5. Maak een Pull Request.

---

# 📄 License

This project is provided as-is for personal and self-hosted use.

Please review the individual licenses and terms of use of the Docker images and applications included in this repository.

The applications themselves are **not owned or distributed by this repository**. This repository provides deployment templates and configuration examples for those applications.
