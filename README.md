# 🎬 Ready-to-use Media Server Portainer Templates & Stacks

Ready-to-use Portainer Application Templates and Docker Compose Stacks for deploying a complete, self-hosted media server.

This repository is designed to make deploying and maintaining a Docker-based media server as simple as possible, whether you're using a Linux home server, NAS, Raspberry Pi, CasaOS, or Docker Desktop.

The complete stack combines media streaming, automated media management, subtitle management, torrent downloading, indexer management, request management, monitoring, database services, Cloudflare challenge handling and Docker management.

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
    * [Method 3: Portainer App Templates](#-method-3-portainer-app-templates--individual-applications)
  * [Environment Variables](#-environment-variables)
  * [Application Details](#-application-details)
  * [Storage & Hardlinks](#-storage--hardlinks)
  * [Updating Containers](#-updating-containers)
  * [Security](#-security)
  * [Troubleshooting](#-troubleshooting)
* [🇳🇱 Nederlands](#-nederlands)

  * [Over deze repository](#-over-deze-repository)
  * [Inbegrepen applicaties](#-inbegrepen-applicaties)
  * [Werking van de mediaserver](#-werking-van-de-mediaserver)
  * [Installatie](#-installatie-1)
  * [Omgevingsvariabelen](#-omgevingsvariabelen)
  * [Applicaties](#-applicaties)
  * [Opslag & Hardlinks](#-opslag--hardlinks-1)
  * [Containers bijwerken](#-containers-bijwerken)
  * [Beveiliging](#-beveiliging)
  * [Probleemoplossing](#-probleemoplossing)

---

# 🇬🇧 English

## 📌 About

This repository contains Portainer Application Templates and Docker Compose Stacks for deploying a complete, self-hosted media server using Docker.

The templates are heavily based on images from LinuxServer.io and are designed to be:

* ✔ Fully configurable through environment variables
* ✔ Portable across different Docker hosts
* ✔ Easy to deploy through Portainer
* ✔ Suitable for Linux home servers
* ✔ Suitable for NAS systems
* ✔ Compatible with Raspberry Pi
* ✔ Compatible with CasaOS
* ✔ Compatible with Docker Desktop
* ✔ Designed with persistent configuration storage
* ✔ Suitable for automated media management
* ✔ Suitable for automated subtitle management
* ✔ Suitable for centralized Docker management

You can either deploy the complete media stack at once or install individual applications through Portainer App Templates.

---

## ✨ Features

### Easy deployment

Deploy the complete media server using a single Docker Compose Stack, or install individual applications separately through Portainer App Templates.

### Environment-based configuration

Paths, user IDs, time zone settings, database credentials and other configuration options are supplied through environment variables instead of being hard-coded into the Compose file.

### Portable configuration

The same Compose configuration can be used on different Docker hosts by changing the environment variables.

### Persistent configuration

Application configuration is stored outside the containers, allowing containers to be recreated or updated without losing their settings.

### Automated media management

The stack combines:

* Media requests
* Movie management
* TV series management
* Indexer management
* Torrent downloading
* Media organization
* Subtitle management
* Media streaming
* Usage statistics

into one integrated workflow.

### Hardware transcoding

Jellyfin and Plex are configured with access to:

```text
/dev/dri
```

This allows compatible Intel systems to use hardware acceleration and hardware transcoding.

Jellyfin additionally uses the LinuxServer Intel OpenCL Docker modification.

### Subtitle automation

Bazarr automatically searches for and downloads subtitles for movies and TV episodes managed by Radarr and Sonarr.

### Indexer challenge handling

FlareSolverr can be used by Prowlarr to handle certain Cloudflare and browser-based anti-bot challenges encountered by supported indexers.

### Docker management

Portainer provides a web-based interface for managing Docker containers, images, networks, volumes and stacks.

---

# 📦 Included Applications

| Application       | Purpose                               |
| ----------------- | ------------------------------------- |
| Jellyfin          | Open-source media server              |
| Plex Media Server | Media server with host network mode   |
| Jellystat         | Jellyfin monitoring & statistics      |
| PostgreSQL        | Database used by Jellystat            |
| Tautulli          | Plex monitoring & statistics          |
| Seerr             | Media request management              |
| Radarr            | Movie management                      |
| Sonarr            | TV series management                  |
| Bazarr            | Automatic subtitle management         |
| Prowlarr          | Indexer management                    |
| Transmission      | Torrent client                        |
| FlareSolverr      | Cloudflare / anti-bot challenge proxy |
| Portainer         | Docker management interface           |

---

## Application Roles

### 🎬 Media Servers

#### Jellyfin

Jellyfin is the open-source media server.

It provides:

* Movie streaming
* TV series streaming
* Media library management
* Hardware transcoding
* Web and mobile clients
* Local network streaming

Jellyfin uses:

```text
${MEDIA_DIR}:/media
```

and stores its configuration under:

```text
${APPDATA_DIR}/jellyfin/config
```

The default web interface is:

```text
http://YOUR-SERVER-IP:8096
```

---

#### Plex Media Server

Plex provides an alternative media streaming platform.

It supports:

* Movies
* TV shows
* Music
* Remote streaming
* Plex clients
* Hardware transcoding

Plex uses host networking:

```yaml
network_mode: host
```

This means Plex does not use the normal `media_network` Docker network.

The default Plex web interface is normally available through:

```text
http://YOUR-SERVER-IP:32400/web
```

Plex requires a temporary claim token during initial setup.

Generate a token shortly before deployment:

https://www.plex.tv/claim/

Set it in your environment:

```text
PLEX_CLAIM=claim-xxxxxxxxxxxxxxxx
```

---

### 📊 Monitoring & Statistics

#### Jellystat

Jellystat provides statistics and monitoring for Jellyfin.

It can provide information such as:

* Playback statistics
* Users
* Watch history
* Media usage
* Viewing activity

Jellystat uses PostgreSQL as its database backend.

The web interface is exposed on:

```text
http://YOUR-SERVER-IP:3001
```

Jellystat stores backup data under:

```text
${APPDATA_DIR}/jellystat/backup
```

---

#### PostgreSQL

PostgreSQL is the database server used by Jellystat.

It is not intended to be accessed directly by normal users.

Jellystat connects to the database using:

```text
POSTGRES_IP=jellystat-db
POSTGRES_PORT=5432
```

The PostgreSQL data is stored persistently under:

```text
${APPDATA_DIR}/jellystat/postgres
```

The PostgreSQL container also includes a health check.

Jellystat waits for PostgreSQL to become healthy before starting.

---

#### Tautulli

Tautulli provides monitoring and statistics for Plex.

It can provide information about:

* Playback history
* Users
* Streaming sessions
* Bandwidth usage
* Transcoding
* Plex activity

The web interface is available on:

```text
http://YOUR-SERVER-IP:8181
```

Configuration is stored under:

```text
${APPDATA_DIR}/tautulli/config
```

---

### 📋 Media Requests

#### Seerr

Seerr provides a user-friendly interface for requesting movies and TV shows.

A typical workflow is:

```text
User
  ↓
Seerr
  ↓
Radarr / Sonarr
```

Seerr can be connected to:

* Jellyfin
* Plex
* Radarr
* Sonarr

The web interface is available on:

```text
http://YOUR-SERVER-IP:5055
```

Configuration is stored under:

```text
${APPDATA_DIR}/seerr/config
```

---

### 🎬 Media Management

#### Radarr

Radarr manages movies.

It can:

* Monitor wanted movies
* Search for releases
* Send downloads to Transmission
* Import completed downloads
* Rename files
* Organize movies
* Manage quality profiles

The web interface is:

```text
http://YOUR-SERVER-IP:7878
```

Configuration:

```text
${APPDATA_DIR}/radarr/config
```

Radarr has access to:

```text
/data
/media
```

---

#### Sonarr

Sonarr performs the same type of automation for TV series.

It can:

* Monitor TV series
* Search for episodes
* Send downloads to Transmission
* Import completed downloads
* Rename episodes
* Organize TV series
* Manage quality profiles

The web interface is:

```text
http://YOUR-SERVER-IP:8989
```

Configuration:

```text
${APPDATA_DIR}/sonarr/config
```

Sonarr has access to:

```text
/data
/media
```

---

#### Bazarr

Bazarr is responsible for automated subtitle management.

It works together with:

* Radarr for movies
* Sonarr for TV series

Bazarr can:

* Search for missing subtitles
* Download subtitles
* Manage movie subtitles
* Manage TV episode subtitles
* Monitor your media library
* Use configured subtitle providers

The web interface is:

```text
http://YOUR-SERVER-IP:6767
```

Configuration:

```text
${APPDATA_DIR}/bazarr/config
```

Media:

```text
${MEDIA_DIR}:/media
```

After installation, configure the Sonarr and Radarr connections inside:

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

Use the same container paths wherever possible:

```text
/media
```

This prevents path mapping problems.

---

### 🔎 Indexer Management

#### Prowlarr

Prowlarr manages indexers and integrates them with applications such as Radarr and Sonarr.

Instead of configuring indexers separately in each application, Prowlarr can act as the central indexer manager.

Typical workflow:

```text
Prowlarr
   ↓
Radarr
   ↓
Sonarr
```

The web interface is:

```text
http://YOUR-SERVER-IP:9696
```

Configuration:

```text
${APPDATA_DIR}/prowlarr/config
```

Prowlarr is connected to the Docker network:

```text
media_network
```

---

### 🛡️ Cloudflare / Anti-Bot Handling

#### FlareSolverr

FlareSolverr is a proxy server designed to help applications handle certain anti-bot and Cloudflare browser challenges.

In this stack it is primarily intended to work together with Prowlarr when an indexer requires additional browser-based challenge handling.

The typical relationship is:

```text
Prowlarr
   ↓
FlareSolverr
   ↓
Indexer
```

FlareSolverr is **not a download client** and does not replace Prowlarr.

The web interface/API is available on:

```text
http://YOUR-SERVER-IP:8191
```

The container uses:

```text
LOG_LEVEL=info
LOG_HTML=false
CAPTCHA_SOLVER=none
```

Configuration is mapped to:

```text
${APPDATA_DIR}/flaresolverr/config
```

Not every indexer requires FlareSolverr.

Only configure it in Prowlarr when an indexer specifically requires it.

---

### 📡 Downloading

#### Transmission

Transmission is the torrent download client.

It receives downloads from Radarr and Sonarr and stores them in the configured data directory.

The web interface is:

```text
http://YOUR-SERVER-IP:9091
```

Configuration:

```text
${APPDATA_DIR}/transmission/config
```

Transmission has access to:

```text
/data
/media
```

Torrent ports:

```text
51413/tcp
51413/udp
```

---

### 🛠️ Docker Management

#### Portainer

Portainer provides a graphical web interface for managing Docker.

It can be used to manage:

* Containers
* Images
* Networks
* Volumes
* Docker Compose stacks
* Logs
* Container consoles
* Environment variables

Portainer exposes:

```text
9000/tcp
```

for HTTP access and:

```text
9443/tcp
```

for HTTPS access.

The default HTTPS interface is:

```text
https://YOUR-SERVER-IP:9443
```

Portainer also exposes:

```text
8000/tcp
```

for Portainer Edge Agent functionality.

Configuration is stored under:

```text
${APPDATA_DIR}/portainer/data
```

Portainer requires access to the Docker socket:

```text
/var/run/docker.sock:/var/run/docker.sock
```

### ⚠️ Important when using Portainer

If you are deploying this stack **from an existing Portainer installation**, you may already have a Portainer container running.

In that case, you normally do not need the Portainer service included in this Compose stack.

Running a second Portainer instance can cause port conflicts on:

```text
9000
9443
8000
```

If you already have Portainer installed, remove or disable the `portainer` service from the Compose stack before deployment.

---

# 🔄 Media Server Workflow

The stack is designed around an automated media workflow:

```text
                         ┌───────────────┐
                         │     Seerr     │
                         │    Requests   │
                         └───────┬───────┘
                                 │
                  ┌──────────────┴──────────────┐
                  │                             │
                  ▼                             ▼
             ┌─────────┐                   ┌─────────┐
             │ Radarr  │                   │ Sonarr  │
             │ Movies  │                   │TV Shows │
             └────┬────┘                   └────┬────┘
                  │                             │
                  └──────────────┬──────────────┘
                                 ▼
                         ┌───────────────┐
                         │   Prowlarr   │
                         │   Indexers   │
                         └───────┬───────┘
                                 │
                         ┌───────▼───────┐
                         │ FlareSolverr  │
                         │  if required  │
                         └───────┬───────┘
                                 │
                                 ▼
                         ┌───────────────┐
                         │  Transmission │
                         │ Torrent Client│
                         └───────┬───────┘
                                 │
                                 ▼
                            /DATA/Downloads
                                 │
                       ┌─────────┴─────────┐
                       │                   │
                       ▼                   ▼
                    Radarr               Sonarr
                       │                   │
                       └─────────┬─────────┘
                                 ▼
                              Bazarr
                                 │
                                 ▼
                            Subtitles
                                 │
                                 ▼
                           Media Library
                                 │
                       ┌─────────┴─────────┐
                       │                   │
                       ▼                   ▼
                   Jellyfin              Plex
                       │                   │
                       ▼                   ▼
                   Jellystat            Tautulli
                       │
                       ▼
                   PostgreSQL
```

### Workflow summary

```text
Seerr
 │
 ├── Movie request ──→ Radarr
 │
 └── TV request ─────→ Sonarr
                         │
                         ▼
                      Prowlarr
                         │
                         ├──→ FlareSolverr
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
                │                 │
                ▼                 ▼
             Jellystat          Tautulli
                │
                ▼
            PostgreSQL
```

Portainer sits alongside the workflow and provides Docker management for the complete environment.

---

# 📁 Repository Structure

The repository contains the following deployment files:

```text
.
├── templates/
│   ├── mediaserver-compose-template.yaml
│   ├── mediaserver-desktop-template.yaml
│   └── mediaserver-template.json
│
├── .env.example
├── .gitignore
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

Contains the complete media server stack.

This is primarily intended for:

* Portainer Stacks
* Linux Docker hosts
* NAS systems
* Home servers
* Raspberry Pi
* CasaOS

### Docker Desktop Template

```text
templates/mediaserver-desktop-template.yaml
```

Contains the Docker Desktop-oriented version of the stack.

This can be used with:

* Windows
* macOS
* Docker Desktop

### Environment Template

```text
.env.example
```

Contains an example configuration for the environment variables used by the stack.

Copy it to:

```text
.env
```

and replace the example values with your own configuration.

The real `.env` file should not be committed to GitHub.

---

# 🚀 Installation

There are three primary ways to deploy the media server.

| Method                  | Best for                            | Configuration                   |
| ----------------------- | ----------------------------------- | ------------------------------- |
| Portainer Stack         | Linux / NAS / Raspberry Pi / CasaOS | Portainer environment variables |
| Docker Desktop          | Windows / macOS                     | `.env` file                     |
| Portainer App Templates | Individual applications             | Portainer GUI                   |

---

## 🚀 Method 1: Portainer Stack — Recommended

This method is recommended if you want to deploy the complete media server suite at once.

### 1. Open the Compose template

Open:

```text
templates/mediaserver-compose-template.yaml
```

### 2. Open Portainer

Navigate to:

```text
Portainer
→ Stacks
→ Add Stack
```

### 3. Create the stack

Give your stack a name, for example:

```text
media-stack
```

Paste the complete Compose configuration into the Web Editor.

### 4. Configure environment variables

Add the required variables under:

```text
Environment variables
```

The main variables are:

| Variable            | Description                    | Example                    |
| ------------------- | ------------------------------ | -------------------------- |
| `APPDATA_DIR`       | Application configuration root | `/DATA/AppData`            |
| `MEDIA_DIR`         | Media library                  | `/mnt/media_storage/Media` |
| `DATA_DIR`          | Downloads/data root            | `/DATA`                    |
| `POSTGRES_USER`     | PostgreSQL username            | `postgres`                 |
| `POSTGRES_PASSWORD` | PostgreSQL password            | Strong password            |
| `JWT_SECRET`        | Jellystat secret               | Random long string         |
| `PLEX_CLAIM`        | Plex claim token               | `claim-xxxxxxxx`           |
| `PUID`              | Linux user ID                  | `1000`                     |
| `PGID`              | Linux group ID                 | `1000`                     |
| `TZ`                | Time zone                      | `Europe/Brussels`          |

### 5. Deploy

Click:

```text
Deploy the stack
```

Docker will create the complete stack.

---

## 💻 Method 2: Docker Desktop — Windows / macOS

Use this method when running Docker through Docker Desktop.

### 1. Copy the Compose template

Copy:

```text
templates/mediaserver-desktop-template.yaml
```

to a directory on your computer.

You can rename it to:

```text
docker-compose.yaml
```

Your directory can look like:

```text
media-server/
├── docker-compose.yaml
└── .env
```

### 2. Create `.env`

Copy:

```text
.env.example
```

to:

```text
.env
```

### 3. Configure `.env`

Example:

```env
APPDATA_DIR=C:\Docker\AppData
MEDIA_DIR=D:\Media
DATA_DIR=D:\Data

POSTGRES_USER=postgres
POSTGRES_PASSWORD=CHANGE_ME
JWT_SECRET=CHANGE_ME
PLEX_CLAIM=

PUID=1000
PGID=1000
TZ=Europe/Brussels
```

Use paths appropriate for your own system.

### 4. Validate the configuration

Run:

```bash
docker compose config
```

If no configuration errors are reported, continue.

### 5. Start the stack

```bash
docker compose up -d
```

### 6. Check the containers

```bash
docker compose ps
```

### 7. View logs

For the complete stack:

```bash
docker compose logs
```

For a specific container:

```bash
docker compose logs bazarr
```

or:

```bash
docker compose logs prowlarr
```

---

## 🧩 Method 3: Portainer App Templates — Individual Applications

Use this method if you only want to install individual applications rather than the complete stack.

In Portainer:

```text
Settings
→ App Templates
```

Enable custom templates and use:

```text
https://raw.githubusercontent.com/runeverstraeten/ready-to-use-media-server-portainer-template/refs/heads/main/templates/mediaserver-template.json
```

Then navigate to:

```text
Portainer
→ App Templates
```

Available applications depend on the current contents of `mediaserver-template.json`.

The repository currently provides templates for applications such as:

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
* FlareSolverr
* Portainer

---

# ⚙️ Environment Variables

The Compose stack is designed to avoid hard-coded host paths wherever possible.

## `APPDATA_DIR`

Root directory for application configuration.

Example:

```env
APPDATA_DIR=/DATA/AppData
```

The applications create their own directories underneath it:

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
├── transmission/
├── flaresolverr/
└── portainer/
```

---

## `MEDIA_DIR`

Location of the media library.

Example:

```env
MEDIA_DIR=/mnt/media_storage/Media
```

Example structure:

```text
Media/
├── Movies/
└── TV Shows/
```

Jellyfin, Plex and Bazarr use this directory.

---

## `DATA_DIR`

Root directory for downloads and other media-related data.

Example:

```env
DATA_DIR=/DATA
```

Recommended structure:

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

Transmission, Radarr and Sonarr use this directory.

---

## `PUID` and `PGID`

These variables determine which Linux user and group the containers use.

Example:

```env
PUID=1000
PGID=1000
```

Find your IDs with:

```bash
id
```

Correct IDs help prevent permission problems between Docker containers and the host filesystem.

---

## `TZ`

Sets the time zone used by the containers.

For Belgium:

```env
TZ=Europe/Brussels
```

---

## `POSTGRES_USER`

PostgreSQL username used by Jellystat.

Example:

```env
POSTGRES_USER=postgres
```

---

## `POSTGRES_PASSWORD`

Password used by PostgreSQL.

Example:

```env
POSTGRES_PASSWORD=USE_A_STRONG_PASSWORD
```

Use a strong, unique password.

---

## `JWT_SECRET`

Secret used by Jellystat.

Use a long, randomly generated value.

Example:

```env
JWT_SECRET=USE_A_LONG_RANDOM_SECRET
```

Do not publish this value.

---

## `PLEX_CLAIM`

Temporary Plex claim token.

Generate a new token shortly before deployment:

```text
https://www.plex.tv/claim/
```

Example:

```env
PLEX_CLAIM=claim-xxxxxxxxxxxxxxxx
```

A Plex claim token is temporary and should not be committed to GitHub.

---

# 💾 Storage & Hardlinks

One of the most important considerations when using Transmission, Radarr and Sonarr is the storage layout.

A correctly configured directory structure allows hardlinks to be used.

## Why hardlinks?

Without hardlinks, importing a downloaded file into the media library may require copying the file.

This can result in:

```text
Downloads/Movie.mkv
Media/Movies/Movie/Movie.mkv
```

occupying storage twice.

With hardlinks, both paths can reference the same underlying filesystem data.

This can significantly reduce unnecessary disk usage.

---

## Recommended structure

Keep downloads and media on the same filesystem and preferably under the same root:

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
│   ├── jellystat/
│   ├── flaresolverr/
│   └── portainer/
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

Hardlinks generally require the source and destination to be on the same filesystem.

---

## Container paths

The stack uses consistent container paths:

| Container    | Data / Downloads | Media    |
| ------------ | ---------------- | -------- |
| Transmission | `/data`          | `/media` |
| Radarr       | `/data`          | `/media` |
| Sonarr       | `/data`          | `/media` |
| Bazarr       | —                | `/media` |
| Jellyfin     | —                | `/media` |
| Plex         | —                | `/media` |

Using consistent container paths makes integrations between the applications much easier.

---

# 🔄 Updating Containers

Configuration is stored outside the containers using persistent volumes.

This means containers can be recreated or updated without losing their application configuration.

## Portainer

When updating the stack, enable:

```text
Pull latest image
```

and redeploy/recreate the stack.

## Docker Compose

Pull the newest images:

```bash
docker compose pull
```

Then recreate the containers:

```bash
docker compose up -d
```

Check the status:

```bash
docker compose ps
```

### Logs

View all logs:

```bash
docker compose logs
```

View one container:

```bash
docker compose logs sonarr
```

Follow logs live:

```bash
docker compose logs -f sonarr
```

---

# 🔐 Security

Security remains the responsibility of the Docker host administrator.

## Never commit secrets

Never commit:

* PostgreSQL passwords
* JWT secrets
* Plex claim tokens
* API keys
* Access tokens
* Usernames/passwords
* Other sensitive credentials

Your real `.env` file should be excluded through `.gitignore`.

Example:

```gitignore
.env
.env.*
!.env.example
```

## Use strong passwords

Use unique passwords for:

* PostgreSQL
* Jellystat
* Plex
* Jellyfin
* Portainer
* Other services exposed to your network

## Limit external access

Do not expose every service directly to the public internet.

If external access is required, consider using:

* HTTPS
* Reverse proxy
* Authentication
* Firewall rules
* VPN
* Access control

Only expose the services that actually need external access.

---

# 🛠️ Troubleshooting

## Permission denied

Check your user and group IDs:

```bash
id
```

Then verify:

```env
PUID=1000
PGID=1000
```

Also verify that the Docker user has permission to access:

```text
${APPDATA_DIR}
${DATA_DIR}
${MEDIA_DIR}
```

---

## Hardlinks are not working

Check:

1. Downloads and media are on the same filesystem.
2. Transmission, Radarr and Sonarr use compatible paths.
3. The Docker user has sufficient permissions.
4. Radarr and Sonarr are not moving files across filesystem boundaries.

Check mounted filesystems:

```bash
df -h
```

---

## Bazarr cannot find media

Make sure Bazarr uses:

```text
/media
```

and that Sonarr and Radarr also use:

```text
/media
```

Example:

```text
Bazarr  → /media
Sonarr  → /media
Radarr  → /media
```

---

## Bazarr cannot connect to Sonarr or Radarr

All three containers are connected to:

```text
media_network
```

Therefore, inside Docker, use the service/container names.

Sonarr:

```text
http://sonarr:8989
```

Radarr:

```text
http://radarr:7878
```

Do not use the host IP when Docker's internal DNS can be used.

---

## Prowlarr cannot connect to FlareSolverr

Verify that both containers are connected to:

```text
media_network
```

FlareSolverr is available to other containers through:

```text
http://flaresolverr:8191
```

Configure FlareSolverr in Prowlarr only for indexers that require it.

---

## FlareSolverr does not solve an indexer problem

FlareSolverr does not bypass every type of protection.

Some indexers may:

* Not require FlareSolverr
* Not support FlareSolverr
* Use protection that FlareSolverr cannot solve
* Require additional configuration

Only enable it when Prowlarr reports that it is required.

---

## Jellystat cannot connect to PostgreSQL

Check that:

```text
jellystat-db
```

is running and healthy.

Check:

```bash
docker compose ps
```

Then inspect the database logs:

```bash
docker compose logs jellystat-db
```

Jellystat uses:

```text
POSTGRES_IP=jellystat-db
POSTGRES_PORT=5432
```

---

## Plex cannot start

If Plex is being deployed for the first time, verify that your `PLEX_CLAIM` value is valid and was generated shortly before deployment.

Also remember that Plex uses:

```yaml
network_mode: host
```

and therefore does not use the normal Docker network configuration.

---

## Portainer cannot start

If Portainer reports that ports such as:

```text
9000
9443
8000
```

are already in use, check whether another Portainer instance is already running.

For example:

```bash
docker ps
```

If you are already using Portainer to deploy this stack, you generally do not need to run another Portainer container from inside the same stack.

---

## Container configuration disappeared

Make sure application configuration is stored in persistent host directories such as:

```text
/DATA/AppData
```

Do not store important configuration exclusively inside the container filesystem.

---

# 🇳🇱 Nederlands

## 📌 Over deze repository

Deze repository bevat Portainer Application Templates en Docker Compose Stacks voor het eenvoudig opzetten van een volledige, zelf-gehoste mediaserver met Docker.

De stack is ontworpen voor:

* Linux thuisservers
* NAS-systemen
* Raspberry Pi
* CasaOS
* Docker Desktop
* Andere Docker-hosts

De configuratie gebruikt environment variables zodat hostpaden, gebruikers-ID's, tijdzone en andere instellingen eenvoudig aangepast kunnen worden.

De volledige stack combineert:

* Mediaservers
* Automatisch mediabeheer
* Ondertitelbeheer
* Torrentdownloads
* Indexerbeheer
* Mediaverzoeken
* Monitoring
* PostgreSQL
* FlareSolverr
* Portainer

---

# 📦 Inbegrepen applicaties

| Applicatie   | Functie                               |
| ------------ | ------------------------------------- |
| Jellyfin     | Open-source mediaserver               |
| Plex         | Mediaserver                           |
| Jellystat    | Jellyfin monitoring en statistieken   |
| PostgreSQL   | Database voor Jellystat               |
| Tautulli     | Plex monitoring en statistieken       |
| Seerr        | Beheer van mediaverzoeken             |
| Radarr       | Filmbeheer                            |
| Sonarr       | TV-seriebeheer                        |
| Bazarr       | Automatisch ondertitelbeheer          |
| Prowlarr     | Indexerbeheer                         |
| Transmission | Torrentclient                         |
| FlareSolverr | Cloudflare / anti-bot challenge proxy |
| Portainer    | Docker beheerinterface                |

---

# 🎬 Applicaties

## Jellyfin

Jellyfin is de open-source mediaserver.

Jellyfin wordt gebruikt voor:

* Films
* TV-series
* Media libraries
* Streaming
* Hardware transcoding

Webinterface:

```text
http://YOUR-SERVER-IP:8096
```

Configuratie:

```text
${APPDATA_DIR}/jellyfin/config
```

Media:

```text
${MEDIA_DIR}:/media
```

Jellyfin heeft toegang tot:

```text
/dev/dri
```

waardoor compatibele Intel-systemen hardwareversnelling kunnen gebruiken.

---

## Plex

Plex is een alternatieve mediaserver voor het streamen van films, series, muziek en andere media.

Plex gebruikt:

```yaml
network_mode: host
```

Webinterface:

```text
http://YOUR-SERVER-IP:32400/web
```

Configuratie:

```text
${APPDATA_DIR}/plex/config
```

Transcoding:

```text
${APPDATA_DIR}/plex/transcode
```

Plex gebruikt eveneens:

```text
/dev/dri
```

voor hardwaretranscoding op compatibele systemen.

Voor een nieuwe Plex-installatie kan een tijdelijke claim token gebruikt worden:

```text
https://www.plex.tv/claim/
```

Deze wordt ingesteld via:

```env
PLEX_CLAIM=claim-xxxxxxxx
```

---

## Jellystat

Jellystat geeft statistieken en monitoring voor Jellyfin.

Voorbeelden:

* Kijkgeschiedenis
* Gebruikers
* Playback
* Kijkactiviteit
* Media statistics

Webinterface:

```text
http://YOUR-SERVER-IP:3001
```

Configuratie/backup:

```text
${APPDATA_DIR}/jellystat/backup
```

Jellystat gebruikt PostgreSQL als database.

---

## PostgreSQL

PostgreSQL is de database die door Jellystat wordt gebruikt.

De databasegegevens worden persistent opgeslagen in:

```text
${APPDATA_DIR}/jellystat/postgres
```

Jellystat maakt verbinding via:

```text
jellystat-db:5432
```

De database beschikt over een healthcheck zodat Jellystat pas start wanneer PostgreSQL beschikbaar is.

---

## Tautulli

Tautulli monitort Plex.

Het biedt informatie over:

* Gebruikers
* Streams
* Kijkgeschiedenis
* Bandbreedte
* Transcoding
* Plex-gebruik

Webinterface:

```text
http://YOUR-SERVER-IP:8181
```

Configuratie:

```text
${APPDATA_DIR}/tautulli/config
```

---

## Seerr

Seerr is de interface voor mediaverzoeken.

Gebruikers kunnen bijvoorbeeld een film of TV-serie aanvragen.

De workflow is:

```text
Seerr
 ↓
Radarr / Sonarr
 ↓
Prowlarr
 ↓
Transmission
```

Webinterface:

```text
http://YOUR-SERVER-IP:5055
```

Configuratie:

```text
${APPDATA_DIR}/seerr/config
```

---

## Radarr

Radarr beheert films.

Radarr kan:

* Films monitoren
* Releases zoeken
* Downloads starten
* Downloads importeren
* Films hernoemen
* Films organiseren

Webinterface:

```text
http://YOUR-SERVER-IP:7878
```

Configuratie:

```text
${APPDATA_DIR}/radarr/config
```

Containerpaden:

```text
/data
/media
```

---

## Sonarr

Sonarr beheert TV-series.

Sonarr kan:

* Series monitoren
* Afleveringen zoeken
* Downloads starten
* Downloads importeren
* Afleveringen hernoemen
* Series organiseren

Webinterface:

```text
http://YOUR-SERVER-IP:8989
```

Configuratie:

```text
${APPDATA_DIR}/sonarr/config
```

Containerpaden:

```text
/data
/media
```

---

## Bazarr

Bazarr beheert automatisch ondertitels.

Bazarr werkt samen met:

* Radarr voor films
* Sonarr voor TV-series

Bazarr kan:

* Ontbrekende ondertitels zoeken
* Ondertitels downloaden
* Ondertitels voor films beheren
* Ondertitels voor afleveringen beheren
* Sonarr en Radarr controleren

Webinterface:

```text
http://YOUR-SERVER-IP:6767
```

Configuratie:

```text
${APPDATA_DIR}/bazarr/config
```

Media:

```text
${MEDIA_DIR}:/media
```

Configureer vervolgens:

```text
Bazarr
→ Settings
→ Sonarr
```

en:

```text
Bazarr
→ Settings
→ Radarr
```

Gebruik bij voorkeur overal hetzelfde containerpad:

```text
/media
```

---

## Prowlarr

Prowlarr beheert indexers.

Prowlarr kan indexers centraal beheren en deze vervolgens beschikbaar maken voor Radarr en Sonarr.

Webinterface:

```text
http://YOUR-SERVER-IP:9696
```

Configuratie:

```text
${APPDATA_DIR}/prowlarr/config
```

Prowlarr gebruikt:

```text
media_network
```

om met andere containers te communiceren.

---

## FlareSolverr

FlareSolverr is een proxy die gebruikt kan worden bij bepaalde Cloudflare- en anti-bot uitdagingen.

Binnen deze stack is FlareSolverr voornamelijk bedoeld om samen te werken met Prowlarr.

De workflow kan bijvoorbeeld zijn:

```text
Prowlarr
 ↓
FlareSolverr
 ↓
Indexer
```

Webinterface/API:

```text
http://YOUR-SERVER-IP:8191
```

Andere containers kunnen FlareSolverr bereiken via:

```text
http://flaresolverr:8191
```

FlareSolverr is niet zelf een indexer of downloadclient.

Niet iedere indexer heeft FlareSolverr nodig.

---

## Transmission

Transmission is de torrentclient.

Transmission ontvangt downloads van Radarr en Sonarr.

Webinterface:

```text
http://YOUR-SERVER-IP:9091
```

Configuratie:

```text
${APPDATA_DIR}/transmission/config
```

Data:

```text
${DATA_DIR}:/data
```

Media:

```text
${MEDIA_DIR}:/media
```

Torrentpoorten:

```text
51413/tcp
51413/udp
```

---

## Portainer

Portainer is een grafische beheerinterface voor Docker.

Via Portainer kun je onder andere beheren:

* Containers
* Images
* Volumes
* Networks
* Stacks
* Logs
* Environment variables

Portainer gebruikt:

```text
9000
```

voor HTTP en:

```text
9443
```

voor HTTPS.

De aanbevolen webinterface is:

```text
https://YOUR-SERVER-IP:9443
```

Configuratie:

```text
${APPDATA_DIR}/portainer/data
```

Portainer heeft toegang tot de Docker socket:

```text
/var/run/docker.sock
```

### ⚠️ Belangrijk

Als je de stack via een **bestaande Portainer-installatie** deployt, heb je normaal gezien al een Portainer-container.

In dat geval is de Portainer-service in deze stack niet nodig.

Als poorten:

```text
8000
9000
9443
```

al gebruikt worden, kan de extra Portainer-container niet starten.

---

# 🔄 Werking van de mediaserver

De volledige workflow ziet er ongeveer zo uit:

```text
                    Seerr
                      │
          ┌───────────┴───────────┐
          ▼                       ▼
       Radarr                  Sonarr
       Films                 TV-series
          │                       │
          └───────────┬───────────┘
                      ▼
                  Prowlarr
                      │
                      ▼
                FlareSolverr
             indien noodzakelijk
                      │
                      ▼
                Transmission
                      │
                      ▼
                  Downloads
                      │
             ┌────────┴────────┐
             ▼                 ▼
          Radarr             Sonarr
             │                 │
             └────────┬────────┘
                      ▼
                    Bazarr
                      │
                      ▼
                 Ondertitels
                      │
                      ▼
                Media Library
                      │
             ┌────────┴────────┐
             ▼                 ▼
          Jellyfin            Plex
             │                 │
             ▼                 ▼
         Jellystat          Tautulli
             │
             ▼
         PostgreSQL
```

Portainer staat hier los van als beheerlaag voor de Dockeromgeving.

---

# 📁 Repositorystructuur

```text
.
├── templates/
│   ├── mediaserver-compose-template.yaml
│   ├── mediaserver-desktop-template.yaml
│   └── mediaserver-template.json
│
├── .env.example
├── .gitignore
└── README.md
```

### Compose Stack

```text
templates/mediaserver-compose-template.yaml
```

De volledige stack voor Portainer, Linux, NAS, Raspberry Pi en andere Docker-hosts.

### Docker Desktop

```text
templates/mediaserver-desktop-template.yaml
```

De Docker Desktop-versie voor Windows en macOS.

### Portainer App Templates

```text
templates/mediaserver-template.json
```

Individuele applicaties die via Portainer geïnstalleerd kunnen worden.

### Environment Template

```text
.env.example
```

Voorbeeld van alle environment variables.

Kopieer deze naar:

```text
.env
```

en pas de waarden aan.

---

# 🚀 Installatie

## Methode 1: Portainer Stack

Ga in Portainer naar:

```text
Stacks
→ Add Stack
```

Gebruik:

```text
templates/mediaserver-compose-template.yaml
```

Configureer:

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

Klik vervolgens op:

```text
Deploy the stack
```

---

## Methode 2: Docker Desktop

Gebruik:

```text
templates/mediaserver-desktop-template.yaml
```

en een `.env` bestand.

Bijvoorbeeld:

```text
media-server/
├── docker-compose.yaml
└── .env
```

Controleer eerst:

```bash
docker compose config
```

Start daarna:

```bash
docker compose up -d
```

Controleer:

```bash
docker compose ps
```

---

## Methode 3: Portainer App Templates

Ga naar:

```text
Portainer
→ Settings
→ App Templates
```

Gebruik als Custom Template URL:

```text
https://raw.githubusercontent.com/runeverstraeten/ready-to-use-media-server-portainer-template/refs/heads/main/templates/mediaserver-template.json
```

Sla de instellingen op en ga vervolgens naar:

```text
Portainer
→ App Templates
```

Daar kun je individuele applicaties installeren.

---

# ⚙️ Omgevingsvariabelen

Belangrijkste variabelen:

```env
APPDATA_DIR=/DATA/AppData
MEDIA_DIR=/mnt/media_storage/Media
DATA_DIR=/DATA

POSTGRES_USER=postgres
POSTGRES_PASSWORD=CHANGE_ME
JWT_SECRET=CHANGE_ME
PLEX_CLAIM=

PUID=1000
PGID=1000
TZ=Europe/Brussels
```

Gebruik voor een echte installatie altijd sterke en unieke secrets.

---

# 💾 Opslag & Hardlinks

Een aanbevolen structuur:

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

Transmission, Radarr en Sonarr gebruiken:

```text
/data
```

voor downloads en:

```text
/media
```

voor de mediabibliotheek.

Downloads en media bevinden zich bij voorkeur op hetzelfde filesystem zodat hardlinks gebruikt kunnen worden.

---

# 🔄 Containers bijwerken

Met Docker Compose:

```bash
docker compose pull
docker compose up -d
```

Status:

```bash
docker compose ps
```

Logs:

```bash
docker compose logs
```

Live logs:

```bash
docker compose logs -f
```

---

# 🔐 Beveiliging

Commit nooit:

```text
.env
```

naar GitHub.

De `.env` kan bevatten:

* PostgreSQL-wachtwoord
* JWT secret
* Plex claim token
* API keys
* Andere gevoelige gegevens

Gebruik `.gitignore`:

```gitignore
.env
.env.*
!.env.example
```

Gebruik sterke wachtwoorden en stel alleen services bloot aan het internet wanneer dit echt noodzakelijk is.

---

# 🛠️ Probleemoplossing

## Permission denied

Controleer:

```bash
id
```

en:

```env
PUID=1000
PGID=1000
```

Controleer ook de rechten op:

```text
APPDATA_DIR
DATA_DIR
MEDIA_DIR
```

---

## Hardlinks werken niet

Controleer:

1. Downloads en media staan op hetzelfde filesystem.
2. Transmission, Radarr en Sonarr gebruiken de juiste paden.
3. De Docker-gebruiker heeft voldoende rechten.
4. Er wordt niet tussen verschillende filesystems verplaatst.

Gebruik:

```bash
df -h
```

---

## Bazarr vindt mijn media niet

Controleer of Bazarr:

```text
/media
```

gebruikt.

Sonarr en Radarr moeten eveneens:

```text
/media
```

gebruiken.

---

## Bazarr kan Sonarr/Radarr niet bereiken

Controleer of de containers op:

```text
media_network
```

zitten.

Gebruik binnen Docker:

```text
http://sonarr:8989
```

en:

```text
http://radarr:7878
```

---

## Prowlarr kan FlareSolverr niet bereiken

Gebruik:

```text
http://flaresolverr:8191
```

en controleer dat beide containers verbonden zijn met:

```text
media_network
```

---

## Jellystat kan PostgreSQL niet bereiken

Controleer:

```bash
docker compose ps
```

en:

```bash
docker compose logs jellystat-db
```

Jellystat gebruikt:

```text
jellystat-db:5432
```

---

## Portainer kan niet starten

Controleer of poorten:

```text
8000
9000
9443
```

al gebruikt worden.

Bij gebruik van een bestaande Portainer-installatie is de Portainer-service in deze stack doorgaans niet nodig.

---

# 🤝 Contributing

Contributions, improvements and suggestions are welcome.

When modifying the templates:

* Keep environment variables configurable.
* Keep persistent storage outside containers.
* Avoid hard-coded host paths where possible.
* Keep container paths consistent.
* Do not commit secrets.
* Test the Compose configuration before submitting changes.

---

# 📄 License

This project is provided as-is for personal and self-hosted use.

Always review the individual licenses and terms of service of the applications and Docker images included in this repository.
