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
   ```

3. **Portainer Agent mit Secrets-Mount starten:**
   
   Stelle sicher, dass der Portainer Agent mit dem Secrets-Volume gestartet wird:
   ```bash
   docker run -d \
     --name portainer_agent \
     --restart=always \
     -v /var/run/docker.sock:/var/run/docker.sock \
     -v /var/lib/docker/volumes:/var/lib/docker/volumes \
     -v /data/secrets:/secrets \
     portainer/agent:latest
   ```

4. **In Portainer verwenden:**
   
   - Gehe zu "Stacks" → "Add Stack"
   - Wähle "Repository" als Methode
   - Gib die Repository URL ein
   - Wähle den gewünschten Stack-Ordner aus
   - Portainer liest automatisch die Secrets aus `/secrets/`

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
