# Portainer Stacks

Diese Repository enthält meine Docker Compose Stacks für Portainer.

## Setup

1. **Repository klonen:**
   ```bash
   git clone <repository-url>
   cd portainer-stacks
   ```

2. **Secrets auf Host kopieren:**
   
   Kopiere die Secrets vom `secrets/` Ordner nach `/data/secrets/` auf dem Host:

   ```bash
   # Auf dem Docker Host ausführen:
   sudo mkdir -p /data/secrets
   sudo cp secrets/* /data/secrets/
   sudo chmod 600 /data/secrets/*.env
   sudo chown root:root /data/secrets/*.env
   ```

3. **Portainer Agent mit Secrets-Mount starten:**
   
   Stelle sicher, dass der Portainer Agent mit dem Secrets-Volume gestartet wird:
   ```bash
   docker run -d \
     --name portainer_agent \
     --restart=always \
     -p 9001:9001 \
     -v /var/run/docker.sock:/var/run/docker.sock \
     -v /var/lib/docker/volumes:/var/lib/docker/volumes \
     -v /data:/data \
     -v /media:/media \
     portainer/agent:latest
   ```

4. **In Portainer verwenden:**
   
   - Gehe zu "Stacks" → "Add Stack"
   - Wähle "Repository" als Methode
   - Gib die Repository URL ein
   - Wähle den gewünschten Stack-Ordner aus
   - Portainer liest automatisch die Secrets aus `/secrets/`

## Troubleshooting

### "env file /secrets/beszel.env not found"

Wenn Sie diese Fehlermeldung erhalten:

1. **Prüfe ob Secrets auf Host existieren:**
   ```bash
   ls -la /data/secrets/
   cat /data/secrets/beszel.env
   ```

2. **Prüfe ob Agent auf Secrets zugreifen kann:**
   ```bash
   docker exec portainer_agent ls -la /data/secrets/
   docker exec portainer_agent cat /data/secrets/beszel.env
   ```

3. **Falls Agent die Secrets nicht sieht:**
   ```bash
   # Agent neu starten:
   docker restart portainer_agent
   
   # Oder Agent komplett neu deployen mit korrekten Mounts
   docker stop portainer_agent
   docker rm portainer_agent
   # Dann den Agent-Befehl aus Setup Schritt 3 ausführen
   ```

4. **Berechtigung prüfen:**
   ```bash
   sudo chown -R root:root /data/secrets/
   sudo chmod -R 644 /data/secrets/*.env
   ```

## Stacks

### beszel/

BESZEL monitoring stack mit Agent.
**Benötigt:** `/secrets/beszel.env` auf dem Host

### fr24/

FlightRadar24 Feeder.
**Benötigt:** `/secrets/fr24.env` auf dem Host

### homebridge/

Homebridge für HomeKit Integration.
**Keine Secrets erforderlich**

### minidlna/

MiniDLNA Media Server.
**Keine Secrets erforderlich**

### pihole/

Pi-hole DNS Server.
**Keine Secrets erforderlich**

### tado_aa/

Tado Auto-Assist Integration.
**Benötigt:** `/secrets/tado.env` auf dem Host

### timecapsule/

Time Machine Server.
**Keine Secrets erforderlich** (lokale Credentials)

## Sicherheit

- Der `secrets/` Ordner ist in der `.gitignore` und wird nicht committet
- Secrets liegen sicher auf dem Host unter `/secrets/`
- Jede Anwendung hat ihre eigenen Secret-Files
- Absoluter Pfad `/secrets/xyz.env` funktioniert garantiert in Portainer

## Backup

Die Secrets unter `/data/secrets/` auf dem Host sollten separat gesichert werden.
