# Nexus Changelog

## 2026-10-08

### Runtime / Recovery / Shutdown

- `NexusRecovery` eingeführt.
- Veraltete `ONLINE`-Status werden beim Serverstart auf `OFFLINE` gesetzt.
- William wird beim Recovery zunächst auf `OFFLINE` gesetzt und nach erfolgreichem Serverstart wieder auf `ONLINE`.
- `NexusRuntime` eingeführt.
- Laufzeit-Checkpoints alle 5 Minuten.
- Nutzer speichern nur noch die seit dem letzten Checkpoint offene Onlinezeit.
- Disconnect speichert nur noch die Restzeit über `getUnsavedChatSeconds()`.
- Doppelzählung zwischen Checkpoint und Disconnect durch Session-Lock verhindert.
- William besitzt eigene Checkpoint-Logik.
- William bleibt während Runtime-Checkpoints `ONLINE`.
- Nur beim Shutdown wird William auf `OFFLINE` gesetzt.
- William-Checkpoint und Shutdown sind ebenfalls durch einen Lock abgesichert.
- Runtime-Scheduler mit `try/catch` geschützt, damit Exceptions nicht alle zukünftigen Checkpoints stoppen.
- `NexusRuntime.stop()` ergänzt und in den Shutdown eingebunden.

### Neue / geänderte Zeitmethoden

#### `NexusSession`

- `lastSavedAt`
- `onlineTimeSaveLock`
- `getUnsavedChatSeconds()`
- `markChatSecondsSaved(...)`
- `getOnlineTimeSaveLock()`
- `getCurrentChatSessionSeconds()` bleibt für spätere WHOIS-Nutzung erhalten.

#### `WilliamInit`

- `lastSavedAt`
- `onlineTimeSaveLock`
- `getUnsavedServerSessionSeconds()`
- `markServerSecondsSaved(...)`
- `getOnlineTimeSaveLock()`
- `getCurrentServerSessionSeconds()` bleibt bestehen.

### Datenbank

Neue bzw. angepasste Methoden:

- `addOnlineTime(...)`
- `addOnlineTimeAndSetLastSeen(...)`
- `addSysAdminOnlineTime(...)`
- `addSysAdminOnlineTimeAndSetOffline(...)`
- `resetAllUserPresenceToOffline()`

### Erfolgreiche Tests

- Nutzer-Checkpoint mit 5-Sekunden-Testintervall.
- Restzeit beim Disconnect korrekt gespeichert.
- Keine Doppelzählung.
- `last_seen_at` nur beim Disconnect aktualisiert.
- Nutzer nach Disconnect korrekt `OFFLINE`.
- William-Checkpoint mit 5-Sekunden-Testintervall.
- William während Runtime weiterhin `ONLINE`.
- William beim Shutdown korrekt `OFFLINE`.
- Restzeit beim Shutdown korrekt gespeichert.

---

## WHOIS V0.1 begonnen

### Befehl

`/whois <NexusName>`

- Verarbeitung über `handleWhoisCommand(...)`.
- Namen mit Leerzeichen bleiben vollständig erhalten.
- Namen wie `Tom & Jerry` können als kompletter Zielname verarbeitet werden.

### Namensprüfung

WHOIS verwendet den bereits initialisierten Filter:

`NexusInit.getNameFilter()`

Geprüft werden:

- gültige Länge
- gültige Zeichen
- geschützter Name
- blockierter Name

### Fehler behoben

WHOIS erzeugte zunächst einen neuen `NexusNameFilter`.

Dadurch waren interne Filterlisten nicht initialisiert und eine Exception führte zum WebSocket-Abbruch.

Behoben durch Verwendung von:

`NexusInit.getNameFilter()`

### William-Ausnahme

`William` ist im WHOIS ein Sonderfall.

Erlaubt:

- `/whois William`
- `/whois william`
- `/whois WILLIAM`

Weiterhin geschützt:

- `/whois Super William`
- `/whois William123`
- `/whois KleinerWilliam`

Die Ausnahme gilt nur für den exakten Namen `William`, unabhängig von Groß-/Kleinschreibung.

### William-Textpools

Bereits vorhanden:

- `chat_whois_missing_name`
- `chat_whois_invalid_length`

`chat_whois_invalid_length` enthält aktuell 7 Varianten.

Der Platzhalter `{user}` wird über die bestehende William-Replace-Logik durch den aufrufenden Nutzer ersetzt.

Noch geplant:

- `chat_whois_invalid_characters`
- `chat_whois_protected_name`
- `chat_whois_blocked_name`
- `chat_whois_user_not_found`
- optional `chat_whois_william_self`

---

## WHOIS – geplanter Aufbau

Die Chat-WHOIS soll dynamisch sein.

Es werden nur Daten angezeigt, die tatsächlich vorhanden sind.

Geplante Felder:

- Nexus-Name
- Realname
- Alter
- Geschlecht
- Rang
- Registrierungsdatum
- gesamte Onlinezeit
- ONLINE / OFFLINE
- `last_seen_at`
- später aktueller / letzter Channel
- optionale Statusdaten
- William-Sonderinformationen

Beispiele:

- `motto = NULL` → keine Motto-Zeile
- `rank_key = CHANNEL_MODERATOR` → Rang anzeigen
- `last_channel = NULL` → keine Channel-Zeile

### Onlinezeitformat

- 46 Sekunden → 46 Sekunden
- 60 Sekunden → 1 Minute
- 119 Sekunden → 1 Minute
- 180 Sekunden → 3 Minuten

Bei Online-Nutzern soll später gelten:

`gespeicherte Gesamtzeit + aktuell laufende Sessionzeit`

---

## Nächster Schritt

1. weitere WHOIS-Textpools anbinden
2. Zielnutzer aus der Datenbank laden
3. William separat behandeln
4. WHOIS-Datenstruktur aufbauen
5. dynamische Ausgabe erzeugen
6. Chat-Overlay erstellen

Die Web-WHOIS bleibt ein separates V0.2-Thema.