## Docker Befehle

### Images

| Befehl | Erklärung | Parameter |
|--------|-----------|-----------|
| `docker pull` | Holt Image von Dockerhub | z. B. `ubuntu:latest` (oder andere Images) |
| `docker rmi ubuntu:latest` | Löscht Image (Keine laufenden/gestoppten Container dürfen existieren) | |
| `docker build -t <imagename:tag> imagepath` | Baut ein Image aus einem Dockerfile | |

#### Image Namen
- **Format:** `source/imagename:tag`
  - `source` = Creator
  - `imagename` = Name des Images
  - `tag` = Version

---

### Container

| Befehl | Erklärung | Parameter |
|--------|-----------|-----------|
| `docker run <name>` | Startet Container | 
| | | `--name XXX` = weist Container Namen zu |
| | | `--rm` = löscht Container nach Ausführung |
| | | `--network xxxx` = lässt sich zu Netzwerk zuordnen |
| | | `--ip="10.10.10.10"` = definiert eine spezifische IP |
| | | `-d` = führt Container detached aus |
| | | `-p hostport:containerport` = mappt Containerport zu Hostport |
| | | `-v volumename:containerverzeichnis` = dort werden Daten gespeichert |
| | | `-e variablename=wert` = setzt Umgebungsvariablen |
| `docker stop <name>` | Stoppt Container | |
| `docker rm <name>` | Löscht Container (muss gestoppt sein) | |
| `docker ps` | Zeigt alle laufenden Container | `-a` = zeigt auch gestoppte Container an |
| `docker exec -it <containername> /bin/bash` | Ruft CMD auf Container auf | `--interactive/-i`, `--tty/-t` oder zusammen als `-it` |
| `docker inspect` | Zeigt Volumes an | `-f {{.Mounts}} containername` |

---

### Volumes

- **Anonymous Volumes**: Wird automatisch erstellt, wenn kein `-v` angegeben wird.
- **Named Volumes**: Erstellt mit `-v` beim `docker run` Befehl.
- **Mounted Volumes**: Statt Volumename wird ein Hostverzeichnis angegeben, um ein Mounting zu erzeugen.

---

### Netzwerke

- Standardmäßig verwendet Docker eine **Bridge** für Container.
- Es können verschiedene Netzwerke/Bridges erstellt werden, um Container voneinander zu isolieren.

#### Netzwerk-Befehle

| Befehl | Erklärung |
|--------|-----------|
| `docker network ls` | Zeigt vorhandene Netzwerke |
| `docker network inspect <netzwerk>` | Inspiziert ein Netzwerk |
| `docker network inspect bridge` | Zeigt die Bridge an |
| ```
docker network create \
--driver=bridge \
--subnet=10.10.10.0/24 \
--gateway=10.10.10.1 \
my_net
``` | Erstellt ein Netzwerk |
| `docker network disconnect bridge <containername>` | Entfernt einen Container aus einer spezifischen Bridge (Container muss ausgeschaltet sein) |
| `docker network connect <bridgename> <containername>` | Verbindet einen Container mit einer Bridge/einem Netzwerk (Container muss ausgeschaltet sein) |

---

### Dockerfile Befehle

| Befehl | Erklärung |
|--------|-----------|
| `FROM ubuntu:latest` | Legt Basis-Image fest |
| `COPY filefromhost dirfromcontainer` | Kopiert Datei vom Host in den Container |
| `ADD --chown=user:group filefromhost dirfromcontainer` | Kopiert Datei vom Host in den Container, kann URL als Quelle benutzen und Archive entpacken |
| `CMD ["Pfad_zu_programm", "parameter"]` | Definiert das Standard-Kommando |
| `ENTRYPOINT ["docker-entrypoint.sh"]` | Setzt das Startkommando für den Container |
| `RUN command` | Führen Befehle während des Build-Prozesses aus (z. B. `apt-get update`) |
| `VOLUME ["/var/lib/mysql", "/var/log/mysql"]` | Mountet Verzeichnisse ins Hostsystem |
| `ENV PORT=3000` | Definiert eine Umgebungsvariable |
| `EXPOSE 6379` | Öffnet einen Port für andere Container |
| `LABEL "website.name"="geeksforgeeks website"` | Fügt Meta-Daten hinzu |
| `USER username` | Setzt den Benutzer für `RUN`, `CMD` und `ENTRYPOINT` |
| `WORKDIR /app` | Setzt das Arbeitsverzeichnis für `RUN`, `CMD` und `ENTRYPOINT` |

---
