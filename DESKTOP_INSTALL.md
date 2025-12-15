# 💻 Installation on Windows & macOS (Docker Desktop)

If you want to run this stack on your PC or Mac using **Docker Desktop**, the installation is slightly different because `network_mode: host` (used in the standard Linux version) is not supported in the same way on Windows and macOS.

### 🛠 Prerequisites
1.  Download and install **[Docker Desktop](https://www.docker.com/products/docker-desktop/)**.
2.  Ensure Docker Desktop is running.

### 🚀 Step-by-Step Guide

**1. Create a Project Folder**
Create a new folder on your computer where you want to store your configuration and media (e.g., `Documents/MediaServer` or `C:\MediaServer`).

**2. Create the Compose File**
Inside that folder, create a new file named `docker-compose.yaml`.

**3. Copy the Desktop-Optimized Code**
Copy the code below and paste it into your `docker-compose.yaml` file.
*Note: This version uses explicit port mapping for Plex instead of host mode to ensure connectivity on Docker Desktop.*
