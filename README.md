# Portainer Stacks

Diese Repository enthält meine Docker Compose Stacks für Portainer.

## Setup

1. **Repository klonen:**
   ```bash
   git clone <repository-url>
   cd portainer-stacks
   ```

2. **In Portainer verwenden:**
   
   - Gehe zu "Stacks" → "Add Stack"
   - Wähle "Repository" als Methode
   - Gib die Repository URL ein
   - Wähle den gewünschten Stack-Ordner aus
   - Setze die erforderlichen Environment Variables (siehe Stack-Beschreibungen)

## Stacks

### beszel/

BESZEL monitoring stack mit Agent.
**Environment Variables:** `BESZEL_KEY`

### fr24/

FlightRadar24 Feeder.
**Environment Variables:** `FR24_KEY`

### homebridge/

Homebridge für HomeKit Integration.
**Keine Environment Variables erforderlich**

### minidlna/

MiniDLNA Media Server.
**Keine Environment Variables erforderlich**

### pihole/

Pi-hole DNS Server.
**Keine Environment Variables erforderlich**

### tado_aa/

Tado Auto-Assist Integration.
**Keine Environment Variables erforderlich**

### timecapsule/

Time Machine Server.
**Keine Environment Variables erforderlich**
