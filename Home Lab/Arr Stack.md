# 🎬 ARR Stack Installation & Configuration Tutorial 🎵

> [!info] Welcome  
> This guide will walk you through the **installation and configuration** of the ARR stack apps: **Radarr, Sonarr, Lidarr, Readarr, Bazarr, and Prowlarr**. Each step is clear, visual, and designed for Obsidian with callouts.

---

## 🗂 Step 1: Configure Root Folders

> [!tip] Root Folder Setup  
> All ARR apps that manage media (Radarr, Sonarr, Lidarr, Readarr) require setting up a **root folder** where media will be stored.

- Go to **Settings > Media Management > Root Folders**  
- Add the folder path where you want your media saved.  
- This step is similar across Radarr, Sonarr, Lidarr, and Readarr.

> 💡 Use a shared folder if you want all apps to save media in the same place.

---

## 📥 Step 2: Configure Download Clients

> [!tip] Connect Download Clients  
> Connect your download client (qBittorrent, NZBGet, Transmission, etc.) to each media app.

- Go to **Settings > Media Management > Download Clients** in each app.  
- Add your download client with host, port, username, and password.  
- Repeat this for Radarr, Sonarr, Lidarr, and Readarr.

---

## 🌐 Step 3: Configure Bazarr & Connect Radarr and Sonarr

> [!tip] Subtitle Language & API Key Setup  
> Bazarr manages subtitles. You must select languages and link it with Radarr and Sonarr.

- In Bazarr, go to **Settings > Languages** and select your subtitle language(s).  
- Get API keys from Radarr and Sonarr: **Settings > General > API Key**.  
- Use these keys to connect Bazarr to Radarr and Sonarr.

---

## ⚙️ Step 4: Setup Prowlarr with FlareSolverr & Torrent Indexers

> [!tip] Indexer Management  
> Prowlarr acts as a universal indexer manager.

### Configure FlareSolverr

- Go to **Settings > Indexers** in Prowlarr.  
- Add FlareSolverr configuration to solve captchas.  
- Assign a tag to FlareSolverr (important for later use).

### Add Torrent Indexers

- In **Settings > Indexers > Add indexer**, add torrent indexers like 1337x or The Pirate Bay.  
- If prompted, assign the FlareSolverr tag to these indexers.  
- After adding indexers, click **Sync App Indexers**.

> 🔄 These indexers will now appear in Radarr, Sonarr, Lidarr, and Readarr under **Settings > Indexers**, enabling shared indexers across all apps.

---

## 🎉 All Set!

> [!success] Congratulations  
> Your ARR stack is now configured with:

- Shared root media folders  
- Unified download clients  
- Centralized indexers with captcha solving  
- Subtitle management via Bazarr

> [!note] 💬 **Pro Tip:**  
> Use Prowlarr tags and centralized API keys to simplify maintenance and ensure all apps stay in sync.

---
# Troubleshooting

## DNS/LDAP after Jellyfin is up

	•	Once the server is actually running, fix LDAP resolution inside gluetun:
	•	Add FIREWALL_OUTBOUND_SUBNETS=192.168.1.0/24 to gluetun so containers can reach the host/LAN. 