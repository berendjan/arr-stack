# arr-stack

## Instructions

### Prepare folders on your MacBook Pro (in the folder where you run this compose file)

```sh
mkdir -p config/jellyfin config/radarr config/sonarr config/lidarr config/readarr config/qbittorrent config/prowlarr
mkdir -p media/movies media/tv media/music media/books downloads
chmod -R 775 downloads media/*
```

### Adjust the timezone (TZ) and your user id/group id (PUID/PGID)

To find your user id and group id on macOS:

```sh
id -u
id -g
```

### Run Docker Compose

```sh
docker compose up -d
```

### Access the services in your browser

Jellyfin: http://localhost:8096
Radarr: http://localhost:7878
Sonarr: http://localhost:8989
Lidarr: http://localhost:8686
Readarr: http://localhost:8787
qBittorrent: http://localhost:8080


### Find qbittorent password in the logs for qBittorrent container

### Configure Radarr, Sonarr, Lidarr, Readarr to use qBittorrent
Inside each app’s web UI (e.g., Radarr at http://localhost:7878):

Go to Settings → Download Clients
Add a new download client
Choose qBittorrent
Fill in:
Host: qbittorrent (this is the Docker service name so Docker networking works)
Port: 8080 (the Web UI port for qBittorrent)
Username: admin (or your changed username)
Password: your qBittorrent password
Category: arr (or any label you want to identify arr downloads)
Use SSL: No (unless you configure HTTPS in qBittorrent)
Test the connection and save

### Adjust qBittorrent’s settings for smooth operation

Inside the qBittorrent Web UI:

Go to Tools → Options → Downloads
Set the Save files to location: /downloads (this path matches your Docker volume)
Optionally create a category named arr (matching what you entered in Radarr/Sonarr) — qBittorrent will then put downloads in /downloads/arr/

### Enable Completed Download Handling
In Radarr and Sonarr:

Go to Settings → Download Clients
Scroll down to Completed Download Handling
Enable “Remove” (if you want to delete the original download in the client) — optional, but recommended
Enable “Import” (this makes Radarr/Sonarr move the file to the correct media folder)
Leave “Failed Download Handling” enabled (optional, it handles failed downloads)

### In Prowlarr
add indexer for piratebay
add app with secret from each other app, settings -> general -> API ey

### In Sonarr, Lidarr, Radarr
add Download client qbittorrent
set root folder downloads for all apps,
set root folder music, movies and tv for corresponding apps.

### How to view your arr media in Jellyfin

Open Jellyfin UI at http://localhost:8096
Go to Dashboard → Libraries → Add Library
Add each media type folder as a library:
Movies → /media/movies
TV Shows → /media/tv
Music → /media/music
Books → /media/books (if you use Readarr)
Jellyfin will scan these folders and index your media.
You can then browse and stream your content from Jellyfin.


