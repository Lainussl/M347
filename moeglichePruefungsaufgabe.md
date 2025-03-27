# Schritt 0: Mit der ZIP-Datei arbeiten
# Zuerst die ZIP-Datei (z.B. "test.zip") von Ihrem Laufwerk in den Home-Ordner der Maschine.

# Wechsle in den Home-Ordner der VM, falls du dich nicht bereits dort befindest:
cd ~ 

# ZIP-Datei entpacken
unzip test.zip

# In das entpackte Verzeichnis wechseln
cd test

# Überprüfen der entpackten Dateien und Ordner
ls

# Schritt 1: Dockerfile erstellen
# Erstellen Sie die Dockerfile-Datei mit folgendem Befehl:

touch Dockerfile

# Schritt 2: Dockerfile bearbeiten
# Bearbeiten Sie die Dockerfile mit einem Texteditor wie nano oder vim

nano Dockerfile


# Schritt 1: Dockerfile erstellen
# Erstellen Sie eine Datei namens Dockerfile und fügen Sie folgenden Inhalt ein:

FROM node:its-alpine

# Arbeitsverzeichnis setzen
WORKDIR /app

# index.js ins Container-Verzeichnis kopieren
COPY index.js /app/

# Alle Dateien aus publicfiles kopieren
COPY publicfiles /app/publicfiles

# Installieren Sie express
RUN npm install express

# Freigeben des Ports 3001
EXPOSE 3001

# Starten Sie den Webserver
CMD ["node", "index.js"]

# Schritt 2: Docker-Image erstellen
docker build -t test-image .

# Schritt 3: Docker-Container erstellen und starten
docker run -d -p 8081:3001 --name test-container -v test-data:/app/publicfiles test-image

# Schritt 4: Ablageort des Volumes bestimmen
docker volume inspect test-data

# Schritt 5: Begrüßung in der index.html ändern (bei laufendem Container)
docker exec -it test-container bash
cd /app/publicfiles
sed -i 's/Hello, test!/Hello, test!/' index.html
