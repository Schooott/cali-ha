# Karl für Home Assistant

Gegenstelle für [Karl](https://github.com/schooott), den macOS-Desktop-Assistenten:
Entities gezielt freigeben, Aktionen mit serverseitiger Whitelist ausführen und
Karl als Benachrichtigungsziel in Automationen nutzen.

## Installation

**Via HACS (empfohlen):**
1. HACS → drei Punkte oben rechts → *Benutzerdefinierte Repositories*
2. `https://github.com/schooott/karl-ha` als Typ *Integration* hinzufügen
3. „Karl" installieren, Home Assistant neu starten
4. Einstellungen → Geräte & Dienste → *Integration hinzufügen* → **Karl**

**Manuell:** `custom_components/karl/` nach `config/custom_components/karl/`
kopieren und neu starten.

## Authentifizierung von Karl (der Mac-App)

1. In HA einen eigenen Benutzer **karl** anlegen (Einstellungen → Personen →
   Benutzer, *kein* Administrator, „Nur lokales Netzwerk" aus, falls Nabu Casa
   genutzt wird).
2. Als dieser Benutzer einmal anmelden → Profil → Sicherheit →
   **Langlebiges Zugriffstoken** erstellen.
3. Token + HA-URL(s) in Karls Einstellungen eintragen.

## Entities freigeben

Zwei Wege, kombinierbar:

- **Label `karl`** auf Entities oder ganze Geräte setzen (gut für Bulk).
- Integrations-**Optionen** → Entity-Picker (gut für Einzelfälle).

Alles andere ist für Karl unsichtbar. Schlösser und Alarmanlagen sind
grundsätzlich **read-only**, auch wenn sie freigegeben sind – die
Aktions-Whitelist liegt serverseitig in dieser Integration.

## Karl als Benachrichtigungsziel

In jeder Automation als Aktion **„Karl sprechen lassen"** (`karl.speak`) wählen:

```yaml
action: karl.speak
data:
  message: "Die Waschmaschine ist fertig."
  emotion: happy   # optional: neutral, happy, excited, sad, bored, surprised, confused, tired, annoyed, proud
  wake: true       # Karl einblenden, falls versteckt
```

## API (für den Karl-Client)

- `karl/entities` – freigegebene Entities, kompakt (Name, Bereich, Zustand, erlaubte Aktionen)
- `karl/action` – `{entity_id, action, params?}`, serverseitig whitelisted
- `karl/subscribe` – pusht `notify`-Events (aus `karl.speak`) und `state`-Updates freigegebener Entities
