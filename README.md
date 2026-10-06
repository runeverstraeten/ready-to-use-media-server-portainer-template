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

De Compose-stack bevat alle onderdelen van de mediaserver, waaronder:

* Jellyfin
* Plex
* Jellystat
* PostgreSQL
* Tautulli
* Seerr
* Radarr
* Sonarr
* Bazarr
* Prowlarr
* Transmission

De Compose-file maakt gebruik van environment variables, waardoor je de YAML-code zelf niet hoeft aan te passen.

### 1. Open het Compose-bestand

**[Bekijk `mediaserver-compose-template.yaml`](https://raw.githubusercontent.com/runeverstraeten/ready-to-use-media-server-portainer-template/refs/heads/main/templates/mediaserver-compose-template.yaml)**

### 2. Kopieer de volledige inhoud

Kopieer de volledige inhoud van het YAML-bestand.

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

Plak vervolgens de gekopieerde YAML-code in de **Web Editor**.

### 5. Stel Environment Variables in

Scroll naar beneden naar **Environment variables** en klik op **Add environment variable**.

De belangrijkste variabelen zijn:

| Variabele           | Beschrijving                         | Voorbeeld                      |
| ------------------- | ------------------------------------ | ------------------------------ |
| `APPDATA_DIR`       | Locatie van configuratiemappen       | `/DATA/AppData`                |
| `MEDIA_DIR`         | Locatie van de mediamap              | `/mnt/media_storage/Media`     |
| `DATA_DIR`          | Hoofdmap voor downloads/data         | `/DATA`                        |
| `POSTGRES_USER`     | PostgreSQL-gebruikersnaam            | `postgres`                     |
| `POSTGRES_PASSWORD` | PostgreSQL-wachtwoord voor Jellystat | `Gebruik-een-sterk-wachtwoord` |
| `JWT_SECRET`        | Secret voor Jellystat                | Willekeurige lange string      |
| `PLEX_CLAIM`        | Plex claim token                     | `claim-xxxxxxxx`               |
| `PUID`              | Linux User ID                        | `1000`                         |
| `PGID`              | Linux Group ID                       | `1000`                         |
| `TZ`                | Tijdzone                             | `Europe/Brussels`              |

Voor een typische Linux-server kun je bijvoorbeeld gebruiken:

```dotenv
APPDATA_DIR=/DATA/AppData
MEDIA_DIR=/mnt/media_storage/Media
DATA_DIR=/DATA

POSTGRES_USER=postgres
POSTGRES_PASSWORD=Gebruik-Hier-Een-Sterk-Wachtwoord
JWT_SECRET=een-lange-willekeurige-geheime-string

PUID=1000
PGID=1000
TZ=Europe/Brussels
```

Voor Plex genereer je een **nieuwe claim token** via:

**[plex.tv/claim](https://www.plex.tv/claim/)**

Vul deze in als:

```dotenv
PLEX_CLAIM=claim-xxxxxxxxxxxxxxxx
```

> **Let op:** een Plex claim token is tijdelijk geldig. Genereer de token daarom vlak voordat je de stack deployt.

### 6. Deploy de Stack

Klik op:

```text
Deploy the stack
```

Portainer zal vervolgens alle containers aanmaken.

Bazarr wordt hierbij automatisch als onderdeel van de stack geïnstalleerd.

---

# 💻 Methode 2: Docker Desktop — Windows / macOS

Gebruik deze methode wanneer je Docker via **Docker Desktop** gebruikt en de stack niet via Portainer wilt beheren.

In plaats van de environment variables in Portainer in te vullen, gebruik je een `.env` bestand.

## 1. Plaats het Compose-bestand

Download of kopieer:

```text
mediaserver-compose-template.yaml
```

naar een map op je computer.

Je kunt het bestand bijvoorbeeld hernoemen naar:

```text
docker-compose.yaml
```

Je map ziet er vervolgens bijvoorbeeld zo uit:

```text
media-server/
├── docker-compose.yaml
└── .env
```

## 2. Maak een `.env` bestand

Maak in dezelfde map een nieuw tekstbestand met exact de naam:

```text
.env
```

> **Let op:** zorg ervoor dat Windows het bestand niet automatisch opslaat als `.env.txt`.

## 3. Voeg je configuratie toe

Voorbeeld voor Windows:

```dotenv
APPDATA_DIR=C:\Docker\AppData
MEDIA_DIR=D:\Media
DATA_DIR=D:\Downloads

POSTGRES_USER=postgres
POSTGRES_PASSWORD=MijnVeiligWachtwoord123
JWT_SECRET=een-lange-willekeurige-geheime-string
PLEX_CLAIM=claim-xxxxxxxxx

PUID=1000
PGID=1000
TZ=Europe/Brussels
```

Op macOS kun je bijvoorbeeld gebruiken:

```dotenv
APPDATA_DIR=/Users/username/Docker/AppData
MEDIA_DIR=/Volumes/Media/Media
DATA_DIR=/Volumes/Media/Data

POSTGRES_USER=postgres
POSTGRES_PASSWORD=MijnVeiligWachtwoord123
JWT_SECRET=een-lange-willekeurige-geheime-string
PLEX_CLAIM=claim-xxxxxxxxx

PUID=1000
PGID=1000
TZ=Europe/Brussels
```

> **Belangrijk:** de exacte paden zijn afhankelijk van waar je Docker-data en media op je computer staan.

### 4. Open een terminal

Open een terminal of opdrachtprompt in de map waarin je `docker-compose.yaml` en `.env` staan.

Bijvoorbeeld:

```bash
cd /path/to/media-server
```

Op Windows kan dit bijvoorbeeld zijn:

```powershell
cd C:\Docker\media-server
```

### 5. Controleer de Compose-configuratie

Je kunt eerst controleren of Docker Compose de configuratie correct kan verwerken:

```bash
docker compose config
```

Als er geen configuratiefouten worden weergegeven, kun je de stack starten.

### 6. Start de mediaserver

Voer uit:

```bash
docker compose up -d
```

Docker zal nu de benodigde images downloaden en de containers starten.

### 7. Controleer de containers

Gebruik:

```bash
docker compose ps
```

Je zou vervolgens de verschillende containers moeten zien, waaronder:

```text
jellyfin
plex
jellystat-db
jellystat
tautulli
radarr
sonarr
bazarr
seerr
prowlarr
transmission
```

### 8. Logs bekijken

Wanneer een container problemen geeft, kun je de logs bekijken met:

```bash
docker compose logs
```

Voor één specifieke container:

```bash
docker compose logs bazarr
```

Of bijvoorbeeld:

```bash
docker compose logs sonarr
```

### 🔐 Belangrijk: `.env` niet uploaden naar GitHub

Je `.env` bestand kan gevoelige informatie bevatten, zoals:

* PostgreSQL-wachtwoord
* JWT secret
* Plex claim token
* API keys

Voeg daarom `.env` toe aan `.gitignore`:

```gitignore
.env
```

---

# 🧩 Methode 3: Portainer App Templates — Losse applicaties

Gebruik deze methode wanneer je niet de volledige mediaserver wilt installeren, maar individuele applicaties één voor één wilt deployen.

Dit is bijvoorbeeld handig wanneer je alleen:

* Jellyfin
* Sonarr
* Radarr
* Bazarr
* Prowlarr
* Transmission

of een andere specifieke applicatie wilt installeren.

## 1. Open Portainer

Ga naar:

```text
Portainer
→ Settings
→ App Templates
```

## 2. Gebruik Custom Templates

Zoek naar de optie:

```text
Use custom templates
```

Schakel deze optie in.

## 3. Voeg de template-URL toe

Gebruik de volgende URL:

```text
https://raw.githubusercontent.com/runeverstraeten/ready-to-use-media-server-portainer-template/refs/heads/main/templates/mediaserver-template.json
```

Plak deze URL in het daarvoor bestemde **URL**-veld.

## 4. Sla de instellingen op

Klik op:

```text
Save Settings
```

## 5. Open App Templates

Ga vervolgens naar:

```text
Portainer
→ App Templates
```

Je zou nu de templates uit deze repository moeten zien.

Je kunt vervolgens een individuele applicatie selecteren en installeren.

### 📦 Beschikbare applicaties

Afhankelijk van de huidige inhoud van `mediaserver-template.json` kunnen onder andere de volgende applicaties beschikbaar zijn:

* Jellyfin
* Plex
* Jellystat
* Tautulli
* Seerr
* Radarr
* Sonarr
* Bazarr
* Prowlarr
* Transmission

### 💬 Bazarr afzonderlijk installeren

Wanneer je alleen Bazarr wilt gebruiken, kun je deze via de App Templates afzonderlijk installeren.

Bazarr gebruikt:

```text
Configuratie:
/DATA/AppData/bazarr/config
```

en:

```text
Media:
/mnt/media_storage/Media
```

of de paden die je via de environment variables instelt.

De standaard webinterface van Bazarr is beschikbaar via:

```text
http://YOUR-SERVER-IP:6767
```

Bijvoorbeeld:

```text
http://192.168.0.2:6767
```

---

# 💬 Bazarr — Ondertiteling

**Bazarr** is verantwoordelijk voor het automatisch beheren en downloaden van ondertitels voor films en TV-series.

Bazarr werkt samen met:

* **Radarr** voor films
* **Sonarr** voor TV-series

### Wat doet Bazarr?

Bazarr kan:

* automatisch ontbrekende ondertitels zoeken;
* ondertitels downloaden;
* ondertitels voor films beheren;
* ondertitels voor TV-afleveringen beheren;
* integreren met Sonarr;
* integreren met Radarr;
* verschillende subtitle providers gebruiken;
* je bibliotheek automatisch controleren op ontbrekende ondertitels.

### Configuratie

De configuratie wordt opgeslagen in:

```text
${APPDATA_DIR}/bazarr/config
```

De mediamap wordt gemount als:

```text
${MEDIA_DIR}:/media
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

Omdat de containers allemaal onderdeel zijn van hetzelfde Docker-netwerk `media_network`, kunnen ze elkaar rechtstreeks bereiken via hun containernamen.

Gebruik bijvoorbeeld:

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
│   ├── jellyfin/
│   ├── plex/
│   ├── jellystat/
│   ├── tautulli/
│   ├── radarr/
│   ├── sonarr/
│   ├── bazarr/
│   ├── seerr/
│   ├── prowlarr/
│   └── transmission/
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

Door `/DATA` correct beschikbaar te maken voor Transmission, Sonarr en Radarr, kunnen hardlinks worden gebruikt.

### Waarom hardlinks?

Zonder hardlinks kan een gedownload bestand volledig worden gekopieerd naar de uiteindelijke mediamap.

Met hardlinks kunnen meerdere bestandspaden naar dezelfde fysieke data verwijzen.

Hierdoor wordt onnodig dubbel opslaggebruik voorkomen.

> **Let op:** downloads en media moeten zich hiervoor op hetzelfde filesystem bevinden.

### Container-mounts

| Container    | Data / Downloads | Media    |
| ------------ | ---------------- | -------- |
| Transmission | `/data`          | `/media` |
| Radarr       | `/data`          | `/media` |
| Sonarr       | `/data`          | `/media` |
| Bazarr       | —                | `/media` |
| Jellyfin     | —                | `/media` |
| Plex         | —                | `/media` |

Door dezelfde `/media`-structuur te gebruiken in de verschillende containers worden padproblemen tussen Radarr, Sonarr, Bazarr, Jellyfin en Plex zoveel mogelijk voorkomen.

---

# 🔄 Containers bijwerken

Omdat de configuratie buiten de containers wordt opgeslagen, kunnen containers opnieuw worden aangemaakt zonder hun configuratie te verliezen.

## Portainer

Bij het opnieuw aanmaken van een container of Stack kun je:

```text
Recreate
```

gebruiken en:

```text
Pull latest image
```

inschakelen.

## Docker Compose

Download eerst de nieuwste images:

```bash
docker compose pull
```

Start vervolgens de containers opnieuw:

```bash
docker compose up -d
```

Controleer daarna:

```bash
docker compose ps
```

Om logs van een specifieke container te bekijken:

```bash
docker compose logs -f bazarr
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
* Gebruikersnamen en wachtwoorden

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
Bazarr   → /media
Sonarr   → /media
Radarr   → /media
Jellyfin → /media
Plex     → /media
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
6. Of Bazarr schrijfrechten heeft op de mediamap.
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
