# 🎬 Ready-to-use Media Server Portainer Templates & Stacks

[![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?logo=docker\&logoColor=white)](https://www.docker.com/)
[![Portainer](https://img.shields.io/badge/Portainer-Compatible-13BEF9?logo=portainer\&logoColor=white)](https://www.portainer.io/)
[![LinuxServer.io](https://img.shields.io/badge/Images-LinuxServer.io-009639)](https://www.linuxserver.io/)

Ready-to-use **Portainer Application Templates** and **Docker Compose Stacks** for deploying a complete, self-hosted media server.

The repository is designed to make deploying and maintaining a Docker-based media server as simple as possible, whether you're using a Linux home server, NAS, Raspberry Pi, CasaOS, or Docker Desktop.

---

## 📖 Table of Contents

* [🇬🇧 English](#-english)

  * [About](#-about)
  * [Features](#-features)
  * [Included Applications](#-included-applications)
  * [Media Server Workflow](#-media-server-workflow)
  * [Repository Structure](#-repository-structure)
  * [Installation](#-installation)

    * [Method 1: Portainer Stack](#-method-1-portainer-stack--recommended)
    * [Method 2: Docker Desktop](#-method-2-docker-desktop--windows--macos)
    * [Method 3: Portainer App Templates](#-method-3-portainer-app-templates--individual-apps)
  * [Environment Variables](#-environment-variables)
  * [Storage & Hardlinks](#-storage--hardlinks)
  * [Updating Containers](#-updating-containers)
  * [Security](#-security)
  * [Troubleshooting](#-troubleshooting)
* [🇳🇱 Nederlands](#-nederlands)

  * [Over deze repository](#-over-deze-repository)
  * [Inbegrepen applicaties](#-inbegrepen-applicaties)
  * [Werking van de mediaserver](#-werking-van-de-mediaserver)
  * [Installatie](#-installatie-1)
  * [Omgevingsvariabelen](#-omgevingsvariabelen-1)
  * [Opslag & Hardlinks](#-opslag--hardlinks-1)
  * [Containers bijwerken](#-containers-bijwerken-1)
  * [Beveiliging](#-beveiliging-1)
* [🤝 Contributing](#-contributing)
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
* ✔ **Suitable for automated media management**

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

### Automated media management

The stack combines request management, indexer management, downloading, media organization and subtitle management into one integrated workflow.

### Subtitle automation

**Bazarr** automatically searches for and downloads subtitles for movies and TV shows managed by Radarr and Sonarr.

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
| **Bazarr**            | Automatic subtitle management       |
| **Prowlarr**          | Indexer management                  |
| **Transmission**      | Torrent client                      |

### Application roles

#### 🎬 Media Servers

* **Jellyfin**
* **Plex Media Server**

Used to stream and manage your media library.

#### 📊 Monitoring

* **Jellystat**
* **Tautulli**

Used for statistics, monitoring and usage information.

#### 📥 Media Management

* **Radarr** — Movies
* **Sonarr** — TV Shows
* **Bazarr** — Subtitles

Radarr and Sonarr organize your downloaded media, while Bazarr automatically searches for matching subtitles.

#### 🔎 Indexer Management

* **Prowlarr**

Prowlarr manages indexers and integrates with applications such as Radarr and Sonarr.

#### 📡 Downloading

* **Transmission**

Transmission handles torrent downloads.

#### 📋 Requests

* **Seerr**

Seerr provides a user-friendly interface for requesting movies and TV shows.

#### 🗄️ Database

* **PostgreSQL**

PostgreSQL provides the database backend required by Jellystat.

---

# 🔄 Media Server Workflow

The stack is designed around an automated media workflow:

```text
                         ┌───────────────┐
                         │     Seerr     │
                         │ Media Requests│
                         └───────┬───────┘
                                 │
                         ┌───────▼───────┐
                         │    Radarr     │
                         │    Movies     │
                         └───────┬───────┘
                                 │
                         ┌───────▼───────┐
                         │     Sonarr    │
                         │   TV Series   │
                         └───────┬───────┘
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
                                 ▼
                         ┌───────────────┐
                         │     /DATA     │
                         │   Downloads   │
                         └───────┬───────┘
                                 │
                    ┌────────────┴────────────┐
                    │                         │
                    ▼                         ▼
              ┌───────────┐             ┌───────────┐
              │  Radarr   │             │  Sonarr   │
              │   Movies  │             │ TV Series │
              └─────┬─────┘             └─────┬─────┘
                    │                         │
                    └────────────┬────────────┘
                                 │
                         ┌───────▼───────┐
                         │    Bazarr     │
                         │   Subtitles   │
                         └───────┬───────┘
                                 │
                                 ▼
                         ┌───────────────┐
                         │ Media Library │
                         └───────┬───────┘
                                 │
                    ┌────────────┴────────────┐
                    │                         │
              ┌─────▼─────┐             ┌────▼─────┐
              │  Jellyfin │             │   Plex   │
              │   Server  │             │  Server  │
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

### Workflow summary

```text
Seerr
  │
  ├── Request Movie ──→ Radarr
  │
  └── Request TV ─────→ Sonarr
                            │
                            ▼
                        Prowlarr
                            │
                            ▼
                      Transmission
                            │
                            ▼
                       Downloads
                            │
                 ┌──────────┴──────────┐
                 ▼                     ▼
              Radarr                 Sonarr
                 │                     │
                 └──────────┬──────────┘
                            ▼
                          Bazarr
                            │
                            ▼
                       Subtitles
                            │
                            ▼
                       Media Library
                            │
                   ┌────────┴────────┐
                   ▼                 ▼
                Jellyfin            Plex
```

---

# 📁 Repository Structure

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
| `POSTGRES_USER`     | PostgreSQL username                          | `postgres`                 |
| `POSTGRES_PASSWORD` | PostgreSQL password for Jellystat            | `Use-a-secure-password`    |
| `JWT_SECRET`        | Secret used by Jellystat                     | Random 64-character string |
| `PLEX_CLAIM`        | Plex claim token                             | `claim-xxxxxxxx`           |
| `PUID`              | Linux user ID                                | `1000`                     |
| `PGID`              | Linux group ID                               | `1000`                     |
| `TZ`                | Time zone                                    | `Europe/Brussels`          |

**Bazarr does not require a separate host path variable.** It uses the shared `MEDIA_DIR` and stores its configuration under:

```text
${APPDATA_DIR}/bazarr/config
```

### 6. Deploy

Click:

```text
Deploy the stack
```

Portainer will create the complete stack, including Bazarr.

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

POSTGRES_USER=postgres
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

This includes **Bazarr**, which can be installed independently if required.

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
├── tautulli/
├── jellystat/
├── radarr/
├── sonarr/
├── bazarr/
├── seerr/
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

Bazarr uses this location to access the movies and TV shows for which it manages subtitles.

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

### `POSTGRES_USER`

PostgreSQL username used by Jellystat.

Default:

```dotenv
POSTGRES_USER=postgres
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

**[plex.tv/claim](https://www.plex.tv/claim/)**

The token is only valid for a limited amount of time, so generate it shortly before deploying the Plex container.

---

# 💬 Bazarr — Subtitle Management

**Bazarr** is included in the stack specifically for automated subtitle management.

It works together with **Sonarr** and **Radarr**.

### What Bazarr does

Bazarr can automatically:

* Search for subtitles
* Download subtitles
* Manage subtitles for movies
* Manage subtitles for TV episodes
* Integrate with Sonarr
* Integrate with Radarr
* Use configured subtitle providers
* Automatically search for missing subtitles

### Bazarr configuration

Bazarr stores its configuration in:

```text
${APPDATA_DIR}/bazarr/config
```

The media directory is mounted as:

```text
${MEDIA_DIR}:/media
```

This gives Bazarr access to the media library.

### Default web interface

Bazarr is exposed on:

```text
http://YOUR-SERVER-IP:6767
```

For example:

```text
http://192.168.0.2:6767
```

### Integration

After installing Bazarr, configure:

```text
Bazarr
→ Settings
→ Sonarr
```

and:

```text
Bazarr
→ Settings
→ Radarr
```

Add the corresponding Sonarr and Radarr instances and API keys.

Bazarr can then automatically monitor your library and download missing subtitles.

> **Tip:** Use the same `/media` path consistently when configuring integrations where possible. This avoids path mapping problems between containers.

---

# 💾 Storage & Hardlinks

One of the most important considerations when setting up Sonarr, Radarr and Transmission is the storage structure.

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
├── AppData/
│   ├── jellyfin/
│   ├── plex/
│   ├── radarr/
│   ├── sonarr/
│   ├── bazarr/
│   ├── seerr/
│   ├── prowlarr/
│   ├── transmission/
│   ├── tautulli/
│   └── jellystat/
│
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

### Container paths

The stack uses the following important paths:

| Container    | Downloads/Data | Media    |
| ------------ | -------------- | -------- |
| Transmission | `/data`        | `/media` |
| Radarr       | `/data`        | `/media` |
| Sonarr       | `/data`        | `/media` |
| Bazarr       | —              | `/media` |
| Jellyfin     | —              | `/media` |
| Plex         | —              | `/media` |

This gives the media management applications a consistent view of the underlying storage.

---

# 🔄 Updating Containers

Container configuration is stored outside the containers through the configured application data directory.

This means containers can be recreated or updated without losing their settings.

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

To check the current status:

```bash
docker compose ps
```

To remove unused Docker resources:

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

## Bazarr cannot find my media

Check the Bazarr volume mapping:

```yaml
- ${MEDIA_DIR:-/mnt/media_storage/Media}:/media
```

Bazarr must be able to access the same media files that Sonarr and Radarr manage.

Also verify that the paths configured inside Bazarr match the paths available inside the containers.

For example:

```text
Bazarr:
    /media

Sonarr:
    /media

Radarr:
    /media
```

Using consistent container paths makes integration significantly easier.

---

## Bazarr cannot connect to Sonarr or Radarr

Verify that all three containers are connected to:

```text
media_network
```

The containers can then communicate using their Docker service/container names.

For example:

```text
http://sonarr:8989
http://radarr:7878
```

instead of relying on the host IP address.

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
* ✔ **Geautomatiseerd mediabeheer** mogelijk te maken
* ✔ **Automatisch ondertitels te downloaden** via Bazarr

---

## 📦 Inbegrepen applicaties

| Applicatie            | Functie                                          |
| --------------------- | ------------------------------------------------ |
| **Jellyfin**          | Open-source mediaserver                          |
| **Plex Media Server** | Mediaserver met host network mode                |
| **Jellystat**         | Jellyfin monitoring & statistieken               |
| **PostgreSQL**        | Database voor Jellystat                          |
| **Tautulli**          | Plex monitoring & statistieken                   |
| **Seerr**             | Beheer van mediaverzoeken                        |
| **Radarr**            | Filmbeheer                                       |
| **Sonarr**            | TV-seriebeheer                                   |
| **Bazarr**            | Automatisch beheer en downloaden van ondertitels |
| **Prowlarr**          | Indexerbeheer                                    |
| **Transmission**      | Torrentclient                                    |

### Rollen van de applicaties

#### 🎬 Mediaservers

* **Jellyfin**
* **Plex**

Voor het afspelen en streamen van films, series en andere media.

#### 📊 Monitoring

* **Jellystat**
* **Tautulli**

Voor statistieken en monitoring van het mediagebruik.

#### 📥 Mediabeheer

* **Radarr** — Films
* **Sonarr** — TV-series
* **Bazarr** — Ondertitels

Radarr en Sonarr beheren de mediabibliotheek. Bazarr zorgt voor het automatisch zoeken en downloaden van ontbrekende ondertitels.

#### 🔎 Indexerbeheer

* **Prowlarr**

Prowlarr beheert indexers en integreert met Radarr en Sonarr.

#### 📡 Downloaden

* **Transmission**

Transmission verzorgt het downloaden van torrents.

#### 📋 Aanvragen

* **Seerr**

Seerr biedt een gebruiksvriendelijke interface waarmee gebruikers films en series kunnen aanvragen.

#### 🗄️ Database

* **PostgreSQL**

PostgreSQL wordt gebruikt als database voor Jellystat.

---

# 🔄 Werking van de mediaserver

De stack is opgebouwd rond een grotendeels geautomatiseerde workflow:

```text
                         ┌───────────────┐
                         │     Seerr     │
                         │  Aanvragen    │
                         └───────┬───────┘
                                 │
                         ┌───────▼───────┐
                         │    Radarr     │
                         │    Films      │
                         └───────┬───────┘
                                 │
                         ┌───────▼───────┐
                         │    Sonarr     │
                         │  TV-series    │
                         └───────┬───────┘
                                 │
                         ┌───────▼───────┐
                         │   Prowlarr    │
                         │    Indexers   │
                         └───────┬───────┘
                                 │
                         ┌───────▼───────┐
                         │  Transmission │
                         │ Torrentclient │
                         └───────┬───────┘
                                 │
                                 ▼
                         ┌───────────────┐
                         │   Downloads   │
                         └───────┬───────┘
                                 │
                    ┌────────────┴────────────┐
                    │                         │
                    ▼                         ▼
                 Radarr                    Sonarr
                    │                         │
                    └────────────┬────────────┘
                                 │
                         ┌───────▼───────┐
                         │    Bazarr     │
                         │  Ondertitels  │
                         └───────┬───────┘
                                 │
                                 ▼
                         ┌───────────────┐
                         │  Medialibrary │
                         └───────┬───────┘
                                 │
                    ┌────────────┴────────────┐
                    │                         │
              ┌─────▼─────┐             ┌────▼─────┐
              │  Jellyfin │             │   Plex   │
              │   Server  │             │  Server  │
              └─────┬─────┘             └────┬─────┘
                    │                         │
              ┌─────▼─────┐             ┌────▼─────┐
              │ Jellystat │             │ Tautulli │
              │ Statistiek│             │Statistiek│
              └─────┬─────┘             └──────────┘
                    │
              ┌─────▼──────┐
              │ PostgreSQL │
              └────────────┘
```

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
POSTGRES_USER
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
POSTGRES_USER=postgres
PUID=1000
PGID=1000
TZ=Europe/Brussels
```

Bazarr gebruikt automatisch:

```text
${APPDATA_DIR}/bazarr/config
```

voor zijn configuratie en:

```text
${MEDIA_DIR}:/media
```

voor toegang tot de mediamap.

### 6. Deploy

Klik op:

```text
Deploy the stack
```

De volledige stack wordt vervolgens aangemaakt, inclusief Bazarr.

---

# 💬 Bazarr — Ondertiteling

**Bazarr** is verantwoordelijk voor het automatisch beheren en downloaden van ondertitels voor films en series.

Bazarr werkt samen met:

* **Radarr** voor films
* **Sonarr** voor TV-series

### Wat doet Bazarr?

Bazarr kan:

* automatisch ontbrekende ondertitels zoeken;
* ondertitels downloaden;
* ondertitels voor films beheren;
* ondertitels voor afleveringen beheren;
* integreren met Sonarr;
* integreren met Radarr;
* verschillende subtitle providers gebruiken;
* je bibliotheek automatisch controleren op ontbrekende ondertitels.

### Configuratie

De configuratie wordt opgeslagen in:

```text
/DATA/AppData/bazarr/config
```

De mediamap wordt gemount als:

```text
/media
```

Bazarr is standaard bereikbaar via:

```text
http://YOUR-SERVER-IP:6767
```

Bijvoorbeeld:

```text
http://192.168.0.2:6767
```

### Integratie met Sonarr en Radarr

Na installatie configureer je in Bazarr:

```text
Settings
→ Sonarr
```

en:

```text
Settings
→ Radarr
```

Gebruik hierbij de Docker-servicenamen wanneer de containers op hetzelfde Docker-netwerk staan:

```text
Sonarr:
http://sonarr:8989

Radarr:
http://radarr:7878
```

Voeg vervolgens de API-key van Sonarr en Radarr toe.

Bazarr kan daarna automatisch controleren welke films en afleveringen nog geen geschikte ondertiteling hebben.

---

# 💾 Opslag & Hardlinks

Voor Sonarr, Radarr en Transmission is een correcte opslagstructuur belangrijk.

Een aanbevolen structuur is:

```text
/DATA/
├── AppData/
├── Downloads/
│   └── torrents/
│       ├── movies/
│       └── tv/
└── Media/
    ├── Movies/
    └── TV Shows/
```

Door `/DATA` correct beschikbaar te maken voor Transmission, Sonarr en Radarr, kunnen hardlinks worden gebruikt.

### Waarom hardlinks?

Zonder hardlinks kan een gedownload bestand volledig worden gekopieerd naar de uiteindelijke mediamap.

Met hardlinks kunnen meerdere bestandspaden naar dezelfde fysieke data verwijzen.

Hierdoor wordt onnodig dubbel opslaggebruik voorkomen.

> **Let op:** downloads en media moeten zich hiervoor op hetzelfde filesystem bevinden.

### Container-mounts

| Container    | Data/Downloads | Media    |
| ------------ | -------------- | -------- |
| Transmission | `/data`        | `/media` |
| Radarr       | `/data`        | `/media` |
| Sonarr       | `/data`        | `/media` |
| Bazarr       | —              | `/media` |
| Jellyfin     | —              | `/media` |
| Plex         | —              | `/media` |

Door dezelfde `/media`-structuur te gebruiken in de verschillende containers worden padproblemen tussen Radarr, Sonarr, Bazarr, Jellyfin en Plex zoveel mogelijk voorkomen.

---

# 🔄 Containers bijwerken

Omdat de configuratie buiten de containers wordt opgeslagen, kunnen containers opnieuw worden aangemaakt zonder hun configuratie te verliezen.

In Portainer kun je bij het opnieuw aanmaken kiezen voor:

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

Controleer daarna de containers:

```bash
docker compose ps
```

---

# 🔐 Beveiliging

Commit nooit gevoelige informatie naar GitHub.

Dit omvat onder andere:

* PostgreSQL-wachtwoorden
* JWT secrets
* Plex claim tokens
* API keys
* Access tokens
* Gebruikersnamen/wachtwoorden

Wanneer je Docker Desktop gebruikt, bewaar deze informatie in `.env` en voeg `.env` toe aan `.gitignore`:

```gitignore
.env
```

Gebruik sterke en unieke wachtwoorden.

Wanneer je diensten van buiten je lokale netwerk beschikbaar maakt, gebruik dan geschikte beveiligingsmaatregelen zoals HTTPS, authenticatie en eventueel een reverse proxy.

---

# 🛠️ Troubleshooting

## Permission denied

Controleer je UID en GID:

```bash
id
```

Bijvoorbeeld:

```text
uid=1000(user) gid=1000(user)
```

Gebruik vervolgens:

```dotenv
PUID=1000
PGID=1000
```

Controleer ook of de Docker-gebruiker toegang heeft tot de ingestelde mappen.

---

## Hardlinks werken niet

Controleer:

1. Of downloads en media op hetzelfde filesystem staan.
2. Of de containers consistente mount paths gebruiken.
3. Of de Docker-gebruiker voldoende rechten heeft.
4. Of Sonarr/Radarr niet naar een ander filesystem schrijven.

Controleer je filesystems met:

```bash
df -h
```

---

## Bazarr kan mijn media niet vinden

Controleer of Bazarr de volgende volume mapping gebruikt:

```yaml
- ${MEDIA_DIR:-/mnt/media_storage/Media}:/media
```

Controleer daarnaast of Sonarr en Radarr dezelfde `/media`-structuur gebruiken.

Aanbevolen:

```text
Bazarr → /media
Sonarr → /media
Radarr → /media
Jellyfin → /media
Plex → /media
```

---

## Bazarr kan niet verbinden met Sonarr of Radarr

Controleer of de containers allemaal verbonden zijn met:

```text
media_network
```

Omdat ze hetzelfde Docker-netwerk gebruiken, kunnen ze elkaar bereiken via hun containernamen.

Gebruik bijvoorbeeld:

```text
http://sonarr:8989
```

voor Sonarr en:

```text
http://radarr:7878
```

voor Radarr.

Controleer ook of je de juiste API-keys hebt ingevoerd.

---

## Bazarr downloadt geen ondertitels

Controleer in Bazarr:

1. Of Sonarr en/of Radarr correct verbonden zijn.
2. Of de API-keys correct zijn.
3. Of minstens één subtitle provider geconfigureerd is.
4. Of de gewenste talen geconfigureerd zijn.
5. Of Bazarr toegang heeft tot `/media`.
6. Of de gebruiker waarmee Bazarr draait schrijfrechten heeft op de mediamap.
7. Of de bestaande media correct door Sonarr/Radarr worden herkend.

---

## Containerconfiguratie verdwenen

Controleer of de configuratiemappen naar een persistente locatie verwijzen:

```text
/DATA/AppData
```

Bijvoorbeeld voor Bazarr:

```text
/DATA/AppData/bazarr/config
```

Bewaar belangrijke configuratie nooit uitsluitend binnen het tijdelijke container-filesystem.

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
