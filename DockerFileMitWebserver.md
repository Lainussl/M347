```dockerfile
# Datei Dockerfile
FROM ubuntu:latest

LABEL maintainer "name@meine-webseite.ch"
LABEL description "Webserver Image für www.meine-webseite.ch"

# Umgebungsvariablen und Zeitzone einstellen
# (erspart interaktive Rückfragen)
ENV TZ="Europe/Zuerich" \
    APACHE_RUN_USER=www-data \
    APACHE_RUN_GROUP=www-data \
    APACHE_LOG_DIR=/var/log/apache2

# Zeitzone einstellen, Apache installieren, unnötige Dateien
# aus dem Paket-Cache gleich wieder entfernen, HTTPS aktivieren
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && \
    echo $TZ > /etc/timezone && \
    apt-get update && \
    apt-get install -y apt-utils apache2 && \
    apt-get -y clean && \
    rm -r /var/cache/apt /var/lib/apt/lists/* && \
    a2ensite default-ssl && \
    a2enmod ssl

# Ports 80 und 443 freigeben
EXPOSE 80 443

# Das Webroot-Verzeichnis als Volume definieren
VOLUME /var/www/html

# Startkommando für den Apache Webserver
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
```

Das Image kann nun mit folgendem Befehl erstellt werden:

```sh
docker build -t meine-webseite-image .
```

Das Argument `.` gibt an, dass sich das Dockerfile im aktuellen Verzeichnis befindet. Wechseln Sie also zuerst ins Verzeichnis mit dem Dockerfile.

Liegen keine Fehler im Dockerfile vor, wird das Image schrittweise erstellt und mit folgender Ausgabe abgeschlossen:

```sh
Successfully built e2a828e60be7
Successfully tagged meine-webseite-image:latest
```

Überprüfen Sie das vorhandene Image mit:

```sh
docker images
```

Beispielausgabe:

```sh
vmadmin@ubuntu:~/meine-webseite$ docker images
REPOSITORY              TAG        IMAGE ID       CREATED          SIZE
meine-webseite-image    latest     e2a828e60be7   3 minutes ago    223MB
```

Sollte das Image noch nicht den Wünschen entsprechen, kann es mit folgendem Befehl gelöscht werden:

```sh
docker rmi meine-webseite-image
```

Ein aus dem Image abgeleiteter Container kann nun mit folgendem Kommando gestartet werden:

```sh
docker run -d --name meine-webseite-container -p 8888:80 \
-v /home/vmadmin/meine-webseite/site:/var/www/html meine-webseite-image
```

Das Verzeichnis `site` muss vorgängig erstellt werden und wird auf das Webroot-Verzeichnis `/var/www/html` abgebildet. Wenn Sie darin eine Indexdatei `index.html` erstellen, wird diese nun beim Aufruf von `http://localhost:8888` angezeigt.