# 🇬🇧 The Ultimate Automated Media Stack: How it Works

This stack isn't just a collection of apps; it's a fully automated ecosystem. Once configured, you can request a movie or TV show from your phone, and within minutes (depending on your internet speed), it will be ready to watch on your Plex client, fully organized with subtitles and metadata.

### 🧩 The Components

1.  **Plex Media Server (The Player)**
    * **What it does:** This is the "Netflix" of your setup. It scans your media files, downloads posters, cast info, and plot summaries. It organizes everything into a beautiful interface.
    * **Key Feature:** It streams your content to any device (Smart TV, Phone, Tablet) inside or outside your house. If your internet is slow or the file format isn't supported by your TV, Plex automatically converts (transcodes) it in real-time.

2.  **Overseerr (The Requester)**
    * **What it does:** A beautiful "App Store" style interface for your media. You (and your family/friends) browse trending movies or search for specific titles here.
    * **The Magic:** You click "Request". Overseerr checks if you already have it. If not, it tells Radarr or Sonarr to go get it. You don't need to open any other app.

3.  **Radarr (Movies) & Sonarr (TV Shows) (The Managers)**
    * **What they do:** The brains of the operation. They maintain your library database.
    * **Automation:** When they receive a request (from Overseerr), they search your indexers (via Jackett) for the best possible quality release that meets your settings (e.g., 1080p or 4K). They send the download link to Transmission.
    * **Organization:** Once the download is done, they pick up the file, rename it cleanly (e.g., `Movie Name (2024).mkv`), move it to the correct folder, and tell Plex to update the library.

4.  **Jackett (The Connector)**
    * **What it does:** Radarr and Sonarr don't "know" website URLs. Jackett acts as a translator. It connects to various Torrent Tracker websites and turns them into a standard feed that Radarr and Sonarr can read.

5.  **Transmission (The Downloader)**
    * **What it does:** A lightweight, efficient BitTorrent client. It simply receives the "magnet links" or torrent files from Radarr/Sonarr and downloads the actual data blocks from other peers on the internet.

6.  **Bazarr (The Translator)**
    * **What it does:** It runs in the background. As soon as a new movie or episode appears in your library, Bazarr checks if it has subtitles in your preferred language(s). If not, it searches subtitle providers (like OpenSubtitles) and downloads them automatically so they match your video file perfectly.

7.  **Tautulli (The Analyst)**
    * **What it does:** It keeps an eye on Plex. It shows you who is watching what, when, and on which device. It keeps a history of plays and can send you notifications (e.g., "Plex is down" or "New movie added").

---

### 🔄 The "Zero-Effort" Workflow
Here is what happens when you use this stack:

1.  **Request:** You are on the bus and see a trailer for a new movie. You open **Overseerr** on your phone and click "Request".
2.  **Search:** **Overseerr** tells **Radarr** to monitor this movie. **Radarr** asks **Jackett** to search all torrent sites.
3.  **Download:** **Radarr** finds a match and sends it to **Transmission**.
4.  **Import:** When **Transmission** finishes, **Radarr** moves the file to your `/media` folder and renames it.
5.  **Subtitles:** **Bazarr** sees the new file and downloads Dutch/English subtitles.
6.  **Watch:** **Plex** detects the new file. You get home, open Plex on your TV, and the movie is there, ready to play.

---
---

# 🇳🇱 De Ultieme Automatische Media Stack: Hoe het werkt

Deze stack is niet zomaar een verzameling apps; het is een volledig geautomatiseerd ecosysteem. Eenmaal ingesteld, kun je vanaf je telefoon een film of serie aanvragen. Binnen enkele minuten (afhankelijk van je internetsnelheid) staat deze klaar in Plex, netjes gesorteerd met covers en ondertitels.

### 🧩 De Componenten

1.  **Plex Media Server (De Speler)**
    * **Wat het doet:** Dit is de "Netflix" van je installatie. Het scant je mediabestanden, downloadt posters, acteursinformatie en samenvattingen. Het presenteert alles in een prachtige interface.
    * **Belangrijkste functie:** Het streamt je content naar elk apparaat (Smart TV, telefoon, tablet) binnen of buiten je huis. Als je internet traag is of je TV het bestand niet ondersteunt, zet Plex het live om (transcoding) naar een speelbaar formaat.

2.  **Overseerr (De Aanvrager)**
    * **Wat het doet:** Een mooie interface die lijkt op een App Store. Jij (en je familie/vrienden) bladeren hier door populaire films of zoeken specifieke titels.
    * **De Magie:** Je klikt op "Request". Overseerr checkt of je het al hebt. Zo niet, dan geeft het Radarr of Sonarr de opdracht om het te halen. Je hoeft geen enkele andere app te openen.

3.  **Radarr (Films) & Sonarr (Series) (De Managers)**
    * **Wat ze doen:** Het brein van de operatie. Zij beheren je bibliotheek.
    * **Automatisering:** Wanneer ze een verzoek krijgen (van Overseerr), zoeken ze via Jackett op al je torrent-sites naar de beste kwaliteit (bijv. 1080p of 4K). Ze sturen de downloadlink naar Transmission.
    * **Organisatie:** Zodra de download klaar is, pakken ze het bestand op, geven het een nette naam (bijv. `Film Naam (2024).mkv`), verplaatsen het naar de juiste map en geven Plex een seintje.

4.  **Jackett (De Tolk)**
    * **Wat het doet:** Radarr en Sonarr "kennen" geen websites. Jackett fungeert als tussenpersoon. Het maakt verbinding met diverse Torrent Tracker-websites en vertaalt de zoekresultaten naar een formaat dat Radarr en Sonarr begrijpen.

5.  **Transmission (De Downloader)**
    * **Wat het doet:** Een lichtgewicht, efficiënte BitTorrent-client. Hij ontvangt simpelweg de opdracht van Radarr/Sonarr en downloadt de daadwerkelijke data van het internet.

6.  **Bazarr (De Vertaler)**
    * **Wat het doet:** Dit draait op de achtergrond. Zodra er een nieuwe film of aflevering in je map staat, checkt Bazarr of er ondertitels zijn in jouw gewenste taal. Zo niet, zoekt hij op sites (zoals OpenSubtitles) en downloadt de juiste versie die perfect synchroon loopt.

7.  **Tautulli (De Analist)**
    * **Wat het doet:** Het houdt Plex in de gaten. Het laat zien wie wat kijkt, wanneer en op welk apparaat. Het houdt geschiedenis bij en kan je notificaties sturen (bijv. "Plex is offline" of "Nieuwe film toegevoegd").

---

### 🔄 De "Zonder Moeite" Workflow
Dit is wat er gebeurt als je dit systeem gebruikt:

1.  **Aanvragen:** Je zit in de bus en ziet een trailer voor een nieuwe film. Je opent **Overseerr** op je mobiel en klikt "Request".
2.  **Zoeken:** **Overseerr** geeft **Radarr** opdracht. **Radarr** vraagt **Jackett** om alle torrentsites af te speuren.
3.  **Downloaden:** **Radarr** vindt een match en stuurt deze naar **Transmission**.
4.  **Importeren:** Als **Transmission** klaar is, verplaatst **Radarr** het bestand naar je `/media` map en hernoemt het netjes.
5.  **Ondertitels:** **Bazarr** ziet het nieuwe bestand en downloadt automatisch Nederlandse/Engelse ondertitels.
6.  **Kijken:** **Plex** detecteert de nieuwe film. Jij komt thuis, opent Plex op je TV, en de film staat klaar om te starten.
