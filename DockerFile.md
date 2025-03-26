# Verwende Ubuntu als Basis-Image
FROM ubuntu:latest

# Setze das Arbeitsverzeichnis
WORKDIR /app

# Kopiere eine Datei vom Host in den Container
COPY localfile.txt /app/localfile.txt

# Füge eine Datei hinzu und setze Benutzerrechte
ADD --chown=user:group config.tar.gz /app/config/

# Führe ein Update durch und installiere notwendige Pakete
RUN apt-get update && apt-get install -y curl

# Definiere eine Umgebungsvariable
ENV PORT=3000

# Erstelle und mounte Volumes für persistente Daten
VOLUME ["/var/lib/mysql", "/var/log/mysql"]

# Exponiere einen Port für externe Verbindungen
EXPOSE 6379

# Setze Metadaten für das Image
LABEL "website.name"="geeksforgeeks website"

# Setze den Benutzer für nachfolgende Befehle
USER username

# Definiere den Befehl, der standardmäßig ausgeführt wird
CMD ["/bin/bash", "-c", "echo Hello, World!"]

# Definiere ein Entrypoint-Skript
ENTRYPOINT ["docker-entrypoint.sh"]