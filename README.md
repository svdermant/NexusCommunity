# Nexus V0.2 – Planungsdokumentation

Diese Datei fasst die bisher besprochenen Planungen für **Nexus V0.2** zusammen. Sie dient als Arbeitsgrundlage, Referenz für GitHub und als Wiedereinstiegspunkt für spätere Unterhaltungen.

> **Wichtig:** V0.2 baut auf dem funktionalen Prototypen V0.1 auf. Sicherheits- und Stabilitätsfunktionen wie `NexusInit`, `NexusShutdown`, `NexusRecovery` und die spätere Runtime-/Checkpoint-Logik gehören bereits zur V0.1-Basis und sind nicht bloß Komfortfunktionen von V0.2.

---

## 1. Grundidee von Nexus V0.2

V0.2 soll Nexus deutlich stärker in Richtung eines eigenständigen Chat-Systems mit Profilen, Channels, privaten Nachrichten, erweiterten Nutzerinteraktionen und einem stärker ausgearbeiteten William-System entwickeln.

Die Oberfläche bleibt desktop-first, soll aber moderner, stärker JavaScript-zentriert und visuell konsistenter mit dem Nexus-Stil werden.

Geplante Schwerpunkte:

- Channel-Auswahl beim Login
- überarbeitete Chat-Oberfläche
- linke Sidebar mit Profil-/Avatarbereich und Channel-Liste
- rechte Nutzerliste bleibt bestehen
- private Nachrichten
- WHOIS-/Profil-System
- WebWhois
- Avatar- und Profilfunktionen
- Textformatierungssystem
- Source-View und Kopierfunktionen
- Flood-/Spam-Schutz
- William als natürlicher Dialogpartner mit späteren Informationsdiensten

---

## 2. Chat-Oberfläche und Layout

### 2.1 Grundaufbau

Für V0.2 ist eine modernisierte Chat-Oberfläche geplant.

Geplante Struktur:

- **linke Sidebar**
  - eigenes Profil / Avatar
  - später weitere Profilinformationen
  - darunter System-/Channel-Liste
- **mittlerer Bereich**
  - Chat-History
  - Eingabefeld
- **rechte Sidebar**
  - Nutzerliste
  - William weiterhin besonders platziert
  - Hover-Effekte
  - spätere Profil- und PM-Interaktion

### 2.2 Design-Stil

Nexus soll einen eigenen Sci-Fi-/Cyber-Glow-Stil behalten bzw. ausbauen.

Geplante Merkmale:

- dunkler bis schwarzer Hintergrund
- Neon-Cyan, Neon-Grün und Magenta als Akzentfarben
- Glow-Effekte
- halbtransparente Flächen
- `backdrop-filter`
- klare Karten-/Panel-Struktur
- visuell konsistent mit Hauptseite und Nexus-Webdesign

### 2.3 Textselektion im Hauptchat

Im normalen Chatbereich soll versehentliche Textselektion weitgehend verhindert werden:

```css
user-select: none;
```

Dies ist nur eine UI-Entscheidung und keine Sicherheitsmaßnahme.

Für Bereiche, in denen Kopieren ausdrücklich erlaubt ist, wird die Selektion gezielt wieder aktiviert.

---

## 3. Channels

Für V0.2 ist eine Channel-Struktur geplant.

### Geplante Funktionen

- Channel-Auswahl beim Login
- System-Channels
- Channel-Liste in der linken Sidebar
- aktuelle Nutzer werden kanalbezogen angezeigt
- spätere Channel-Moderatoren

### William in Channels

William ist ein Systemdienst und kein normaler Nutzer.

Daher gilt langfristig:

- William ist nicht nur in einem einzelnen Channel sichtbar
- William ist grundsätzlich in allen relevanten Nexus-Channels verfügbar
- seine Präsenz ist systemweit

---

## 4. Private Nachrichten

Für V0.2 ist ein eigenes PM-System vorgesehen.

### Geplante Zugänge

- über Hover/Klick in der Nutzerliste
- über Befehl:

```text
/m username
```

### Geplanter Aufbau

Ein Formular soll enthalten:

- Empfänger
- Betreff
- Nachrichtentext

### Speicherung

Private Nachrichten sollen in SQL gespeichert werden, damit sie auch zugestellt werden können, wenn der Empfänger gerade offline ist.

---

## 5. Eingabeverlauf

Für das Chat-Eingabefeld ist ein kleiner lokaler Verlauf geplant.

### Verhalten

- `Arrow Up` → ältere Eingabe anzeigen
- `Arrow Down` → neuere Eingabe anzeigen
- ungefähr 5–10 letzte Eingaben
- besonders nützlich für Slash-Commands
- beim Durchblättern wird **nicht automatisch gesendet**

Die Funktion ist clientseitig geplant.

---

## 6. WHOIS / Profil-System

V0.1 erhält bereits eine funktionale WHOIS als Prototyp.

V0.2 erweitert diese später zu einem vollwertigen Profil-System.

### 6.1 WHOIS V0.1 als Basis

Geplante bzw. bereits vorbereitete Daten:

- Nexus-Name
- Realname
- Alter
- Geschlecht
- Rang
- Registrierungsdatum
- gesamte Onlinezeit
- Online-/Offline-Status
- aktueller Channel
- William-spezifische Systeminformationen

### 6.2 V0.2 Profil-Erweiterungen

Später geplant:

- Avatar
- Motto
- README-/Statusfeld
- weitere Profilinformationen
- Bilder
- Fotoalben
- zusätzliche Profilmodule

### README-/Statusfeld

Bei normalen Nutzern soll es später ein frei pflegbares Feld geben, beispielsweise für:

- momentane Stimmung
- kurze Statusmeldung
- Zitat
- „Bin kurz weg“
- persönliche Notiz

Bei William kann dieser Bereich mit festen systembezogenen Texten belegt werden.

---

## 7. WebWhois

Die **Nexus WebWhois** ist als V0.2-Erweiterung geplant.

Sie ist eine externe, autarke Profilansicht im Browser und funktioniert unabhängig von einem aktiven Chat-Login.

### 7.1 URL-Struktur

Geplant:

```text
https://nexus.igamerpg.de/profile/[Nutzername]
```

Beispiel:

```text
https://nexus.igamerpg.de/profile/William
```

### 7.2 Eigenschaften

- kein Login-Zwang
- direkt im Browser aufrufbar
- externe Besucher können Profile sehen
- Suchmaschinen optional über `robots.txt` steuerbar
- WebWhois-Aufruf erzeugt **keinen Eintrag** in einer späteren Profilbesucher-/„Vermisst“-Liste
- freie Textselektion möglich
- Inhalte können normal kopiert werden

### 7.3 Architektur

Die WebWhois soll **keine zweite Profildatenbank** sein.

Stattdessen:

```text
gemeinsame Profildaten
        ↓
WHOIS im Chat
        ↓
WebWhois im Browser
```

Die Datenquelle bleibt gemeinsam, nur Darstellung und Zugriffslogik unterscheiden sich.

### 7.4 Technische Umsetzung

Da Nexus aktuell aus PHP-Website + Java/Jetty-Server besteht, soll die spätere WebWhois nicht auf Express.js aufbauen.

Mögliche spätere Umsetzung:

- PHP-Routing / Rewrite
- PHP-Router
- REST-Schnittstelle zum Java-Server
- serverseitiges Rendering oder API-basierte Darstellung

### 7.5 WebWhois-Design

Aktuelle Designrichtung:

- Grundidee basierend auf einer größeren Profilkarte
- Cyber-/Neon-Optik
- nicht nur schwarzer Hintergrund, sondern atmosphärische Nexus-Welt
- Avatar oben prominent
- Identitätsbereich im Kopf
- Status und Rang direkt sichtbar
- darunter technische und persönliche Datenblöcke
- Desktop-Breite ungefähr 500 px
- mobil responsive

Wichtig:

- **WebWhois-Profil und Forum-Mitgliederprofil sind nicht dasselbe System**
- Design darf verwandt sein, Aufbau und Zweck unterscheiden sich

### 7.6 William-Avatar

Für William soll später wieder der freundliche Droiden-/Roboter-Avatar aus der Registrierung verwendet werden.

---

## 8. Source-View und Kopierfunktionen

Für V0.2 ist ein System geplant, das zwischen Rohtext und gerenderter Darstellung unterscheidet.

### 8.1 Rohtext getrennt speichern

Wichtiges Architekturprinzip:

```text
rawText
→ Originaltext mit Nexus-Formatierung

renderedText
→ gerenderte Darstellung
```

Der Rohtext darf nicht aus dem DOM zurückrekonstruiert werden.

### 8.2 Source-View

Geplant:

- `SHIFT + Linksklick` auf geeignete Nachricht/Profilelemente
- rohe Nexus-Formatierung anzeigen
- Quelltext kopierbar machen

### 8.3 Clipboard

Geplant:

- Kopieren über Clipboard API
- anschließend kleine Neon-Toastmeldung:

```text
Quellcode kopiert!
```

### 8.4 Chat-Log

Geplant:

```text
/log
/log 20
```

öffnet eine reduzierte, frei markierbare Textansicht.

Dort gilt:

- Textselektion erlaubt
- `Ctrl+C` funktioniert normal
- keine Chat-UI-Sperre

### 8.5 Nutzerlisten-Interaktionen

Geplant:

- Linksklick auf Nutzername → reduzierte Cyber-WHOIS
- `SHIFT + Rechtsklick` auf Nutzername → exakten Nickname ins Eingabefeld übernehmen

---

## 9. Nexus Text-Formatierungssystem

V0.2 soll eine eigene Nexus-Textsyntax erhalten.

Die Syntax basiert auf einfachen Steuerzeichen und soll bewusst nicht wie HTML wirken.

---

## 10. Farbcodierung

### 10.1 Farben einschalten

Großbuchstaben aktivieren eine Farbe.

Beispiele:

```text
°R° → Rot
°B° → Blau
°G° → Grün
°C° → Cyan / Türkis
```

Weitere Farben können später ergänzt werden.

### 10.2 Einzelne Farbe ausschalten

Bei Farben außer Rot kann ein Kleinbuchstabe die jeweilige Farbe wieder ausschalten.

Beispiele:

```text
°B° → Blau an
°b° → Blau aus

°G° → Grün an
°g° → Grün aus

°C° → Cyan an
°c° → Cyan aus
```

### 10.3 Sonderfall Rot / globaler Reset

Rot ist die Ausnahme:

```text
°R° → Rot an
°r° → globaler Reset
```

`°r°` ist **nicht nur „Rot aus“**, sondern setzt den gesamten Formatierungszustand auf Systemstandard zurück.

Dazu gehören:

- Farbe zurück auf Standard
- Schriftgröße zurück auf Standard
- Fett aus
- Kursiv aus
- Unterstrichen aus
- sonstige aktive Formatierungen zurücksetzen

Beispiel:

```text
°G°°12°__Hallo Welt__°r°
```

Nach `°r°` ist wieder alles auf Standard.

---

## 11. Schriftgrößen

### 11.1 Absolute Schriftgröße

```text
°12° → Schriftgröße 12
°20° → Schriftgröße 20
```

Eine maximale und minimale erlaubte Größe muss noch festgelegt werden.

Es soll ausdrücklich **keine unbegrenzte Schriftgröße** geben, damit das Chat-Layout nicht zerstört werden kann.

### 11.2 Relative Schriftgröße

Geplant:

```text
°+4° → aktuelle Schriftgröße um 4 erhöhen
°-2° → aktuelle Schriftgröße um 2 verringern
```

### 11.3 Reset

```text
°r°
```

setzt die Schriftgröße wieder auf den Systemstandard zurück.

---

## 12. Fett, Unterstrichen und weitere Textstile

### Fett

```text
__Hallo Welt__
```

ergibt fett formatierten Text.

### Unterstrichen

```text
_Hallo Welt_
```

ergibt unterstrichenen Text.

### Kombinationen

Formatierungen sollen kombinierbar sein.

Beispiel:

```text
°R°°10°_Hallo Welt_°r°
```

Bedeutung:

- Rot
- Schriftgröße 10
- unterstrichen
- danach globaler Reset

Weitere Kombinationen wie Farbe + Größe + Fett + Unterstrichen sollen möglich sein.

---

## 13. Zeilenumbruch mit `#`

Das Zeichen `#` soll im Chat einen Zeilenumbruch innerhalb derselben Nachricht erzeugen.

### Beispiel

Eingabe:

```text
Dieser Tag war heute sehr angenehm#Morgen wird es schlechter
```

Darstellung:

```text
Max: Dieser Tag war heute sehr angenehm
     Morgen wird es schlechter
```

Bei längeren Nutzernamen soll die zweite Zeile optisch am Beginn des Nachrichtentextes ausgerichtet werden.

Beispiel:

```text
MaxUndMoritz: Heute war es toll
              Morgen wird es Schnitzel geben.
```

Die sichtbaren Punkte aus früheren Skizzen sind **keine echten Zeichen**, sondern nur eine Darstellung des Abstandes.

Technisch soll die Einrückung über CSS erfolgen.

---

## 14. Parser-Architektur

Für das vollständige Nexus-Formatierungssystem ist langfristig ein zustandsbasierter Parser sinnvoller als eine reine Kette aus Regex-Ersetzungen.

### Mögliche Parser-Tokens

```text
°R°
°B°
°G°
°C°
°12°
°+4°
°-2°
°r°
__...__
_..._
#
```

### Formatierungszustand

Der Parser verwaltet intern beispielsweise:

```text
Farbe
Schriftgröße
Fett
Unterstrichen
Kursiv
```

Jeder Token verändert nur den passenden Zustand.

Regex kann weiterhin zum Erkennen einzelner Tokens verwendet werden, soll aber nicht als alleinige Gesamtarchitektur festgeschrieben werden.

### Sicherheit

- keine freien HTML-Tags von Nutzern
- keine freie CSS-Eingabe
- nur definierte Farben
- nur erlaubte Schriftgrößen
- Nutzereingaben werden sicher geparst
- Rohtext bleibt separat erhalten

---

## 15. Flood-/Spam-Schutz

Für V0.2 ist ein serverseitiger Flood-Schutz geplant.

### Grundidee

Beispiel:

```text
maximal 5 Nachrichten in 5 Sekunden
```

Bei Überschreitung wird das Flood-System ausgelöst.

### Wichtig

Der Schutz läuft **immer**, auch wenn ein Channel-Moderator anwesend ist.

Er ist kein Ersatz für Moderatoren, sondern eine technische Grundsicherung.

### Aufgabenverteilung

```text
Automatische Schutzsysteme
→ messbare technische Verstöße
→ Flooding
→ schnelle Wiederholungen
→ eventuell identische Copy-Paste-Nachrichten

Channel-Moderatoren
→ Kontext
→ Streit
→ Beleidigungen
→ Provokationen
→ Regelverstöße
→ Chat-Klima
```

Ein Moderator ist ein realer Mensch und muss den Chat nicht 24/7 permanent beobachten.

### Mögliche spätere Staffelung

- erste Überschreitung → Warnung
- weitere Überschreitungen → kurze Sendesperre
- dauerhafter Missbrauch → längere Sperre oder Moderationslog

Die genaue Staffelung ist noch offen.

### Technische Umsetzung

Serverseitig, zum Beispiel über eine eigene Komponente wie:

```text
NexusFloodProtection
```

oder:

```text
NexusRateLimiter
```

Die Prüfung soll pro `NexusSession` erfolgen.

---

## 16. William – zukünftiges Dialogsystem

William soll später nicht nur auf Slash-Commands reagieren, sondern sich wie ein echter Chat-Assistent verhalten.

### 16.1 Natürliche Sprache statt Befehl

Beispiel:

```text
Hey William, wie ist das Wetter morgen in Dresden?
```

William soll daraus erkennen:

```text
Absicht: Wetter
Ort: Dresden
Zeit: morgen
```

Oder:

```text
Guten Morgen William, was kommt heute Abend auf RTL?
```

Erkennung:

```text
Absicht: TV-Programm
Sender: RTL
Zeit: heute Abend
```

### 16.2 Adressierungsregel

William reagiert **nur**, wenn sein Name im Text vorkommt.

Beispiele:

```text
William, wie wird das Wetter morgen?
→ William reagiert
```

```text
Wie wird das Wetter morgen?
→ William reagiert NICHT
```

Auch natürliche Anreden sind möglich:

```text
Hey William
Hallo William
Guten Morgen William
Sag mal William
```

Der Name `William` bleibt der eigentliche Trigger.

### 16.3 Mögliche spätere Architektur

```text
WilliamMessageAnalyzer
→ erkennt Thema / Intent

WilliamWeatherService
→ Wetterdaten

WilliamTvService
→ Fernsehprogramm

WilliamResponseBuilder
→ formuliert Antwort im William-Stil
```

### 16.4 Spätere Dienste

Geplant bzw. angedacht:

- Wettervorhersage
- TV-Programm
- weitere Informationsdienste
- Witze / Humor
- systembezogene Hilfe

### 16.5 William-Humor

William soll verschiedene Humor-Pools erhalten können, zum Beispiel:

```text
william/jokes/classic
william/jokes/story
william/jokes/absurd
william/jokes/chaotic
william/jokes/adult
```

Ziel ist, seinen Humor steuerbar zu machen, ohne dass er permanent Witze ausgibt.

---

## 17. William als Systemkonto

William ist kein normaler Nutzer.

### Besonderheiten

- Systembot / Systemdienst
- eigener Eintrag in `nexus_sysadmins`
- kein Passwort-Hash nötig
- Status grundsätzlich `REGISTERED`
- Rang `SYSADMIN`
- eigene Serverlaufzeit statt normaler Chat-Sitzungszeit
- systemweite Präsenz
- später in allen Channels verfügbar

### Aktuelle William-Datenbasis

Geplant bzw. bereits vorbereitet:

```text
name
age
gender
nexus_name
status
rank_key
registered_at
total_online_seconds
last_seen_at
presence_status
```

### Aktuell festgelegte William-Daten

```text
Name: William Olaf Butler
Alter: 77
Nexus-Name: William
Geschlecht: MALE
Status: REGISTERED
Rang: SYSADMIN
Registrierungsdatum: 26.09.2026 21:20:00
Historischer Startwert Onlinezeit: 910800 Sekunden
```

Der historische Startwert ist eine einmalige Näherung und keine exakt rekonstruierte Server-Uptime.

---

## 18. Forum und Profile – klare Trennung

Wichtig für V0.2:

**WebWhois/Chat-Profil und Forum-Mitgliederliste sind nicht dasselbe System.**

### Nexus / Chat

zuständig für globale Identität:

- Nexus-Name
- Account-Status
- globaler Rang
- Avatar
- spätere Profilinformationen

### Forum

zuständig für forumsspezifische Aktivität:

- Forenmitgliedschaft
- Forenbesuche
- Beiträge
- Themen
- Forum-Aktivität

Das Forum soll globale Nexus-Daten nicht unnötig duplizieren.

Konzept:

```text
nexus_users
→ globale Identität

forum_users
→ nexus_user_id als Referenz
→ forumspezifische Daten
```

---

## 19. Sicherheit und Stabilität – bereits V0.1-Basis

Diese Punkte sind zwar kein Komfortfeature von V0.2, müssen aber bei der weiteren Planung berücksichtigt werden.

### 19.1 NexusInit

Aufgaben:

- Konfiguration laden
- DB-Verbindung prüfen
- Tabellen prüfen/anlegen
- William prüfen/anlegen
- weitere Komponenten initialisieren

### 19.2 NexusShutdown

Geplante bzw. teilweise bereits umgesetzte Aufgaben:

- laufende Zustände sauber abschließen
- Onlinezeiten speichern
- Presence auf OFFLINE setzen
- William-Laufzeit sichern
- temporäre Zustände bereinigen
- erst danach Server beenden

### 19.3 NexusRecovery

Geplant:

- veraltete ONLINE-Zustände nach Crash bereinigen
- William nach unsauberem Shutdown korrigieren
- verwaiste temporäre Zustände prüfen
- spätere Session-Recovery

### 19.4 NexusRuntime / Checkpoint-System

Eine eigene Klasse ist noch nicht endgültig festgelegt, aber eine Laufzeit-Komponente ist vorgesehen.

Geplante Checkpoint-Zeit:

```text
alle 5 Minuten
```

Zweck:

- laufende Onlinezeiten regelmäßig sichern
- Williams Serverlaufzeit sichern
- bei hartem Crash maximal etwa 5 Minuten Verlust statt Stunden

### 19.5 Checkpoint ersetzt Disconnect-Speicherung NICHT

Beispiel:

```text
00:00 Login
00:05 Checkpoint → +5 Minuten
00:08 Disconnect → +3 Minuten Rest
```

Gesamt:

```text
8 Minuten
```

Oder:

```text
00:00 Login
00:05 Checkpoint → +5
00:10 Checkpoint → +5
00:13 Disconnect → +3
```

Gesamt:

```text
13 Minuten
```

Der Checkpoint speichert nur die Zeit seit dem letzten Speichern.

`onWebSocketClose()` bleibt erhalten und speichert beim Trennen den Rest.

### 19.6 Datenklassen

Wichtige Nutzerdaten dürfen nicht erst beim Shutdown gespeichert werden.

Sofort speichern:

- Profilbild
- Motto / README
- Passwort
- Profilinformationen
- PMs
- Forenbeiträge
- Einstellungen
- spätere Fotoalben

Checkpoint-/Shutdown-Daten:

- laufende Sessionzeit
- Presence
- Serverlaufzeit
- flüchtige Runtime-Zustände

---

## 20. Noch offene Entscheidungen

Folgende Punkte sind bewusst noch nicht endgültig festgelegt:

- minimale und maximale erlaubte Schriftgröße
- vollständige Farbtabelle
- Kursiv-Syntax
- genaue Flood-Sperrzeiten
- genaue Channel-Struktur
- genaue REST-/WebWhois-Architektur
- finale Profilfelder für V0.2
- genaue Avatar-Upload-Architektur
- genaue Fotoalbum-Funktion
- exakte Moderator-Rechte
- endgültige Klasse bzw. Struktur für `NexusRuntime`
- konkretes Session-Recovery nach hartem Crash

---

## 21. Aktuelle Entwicklungsreihenfolge

Der aktuelle Fokus liegt noch auf V0.1 und der stabilen Basis.

Geplante Reihenfolge ab aktuellem Stand:

```text
1. NexusRecovery weiterbauen
2. Crash-Zustände sauber bereinigen
3. NexusRuntime / 5-Minuten-Checkpoint planen und implementieren
4. gesamte Sicherheits-/Persistenzstruktur prüfen
5. danach zurück zur WHOIS V0.1
6. später Forum weiterentwickeln
7. V0.2-Funktionen schrittweise umsetzen
```

---

## 22. Entwicklungsprinzipien

Für Nexus soll weiterhin gelten:

- kleine Schritte
- eine Änderung nach der anderen
- nach jedem Schritt testen
- Datenbankänderungen vorsichtig migrieren
- bestehende Methoden vor neuen Methoden prüfen
- globale Identität nicht unnötig duplizieren
- wichtige Nutzerdaten sofort persistieren
- Serverzustände durch Shutdown + Recovery + Checkpoints absichern
- Rohtext und gerenderten Text getrennt behandeln
- Sicherheit serverseitig erzwingen

---

## 23. Kurzfassung für einen späteren Neustart der Unterhaltung

Falls dieses Dokument in einer neuen Unterhaltung verwendet wird, ist der wichtigste Kontext:

- Nexus ist ein eigener Java/Jetty-WebSocket-Chat mit PHP-Website und MariaDB.
- V0.1 ist der funktionale Prototyp.
- V0.2 erweitert um Channels, Profile, WebWhois, PMs, Textformatierung, Source-View, Flood-Schutz, Avatar-/Profilfunktionen und intelligentere William-Dialoge.
- William ist ein eigener SysAdmin-Systembot mit eigener Tabelle und eigener Serverlaufzeit.
- V0.1 erhält bereits stabile Init-/Shutdown-/Recovery-/Checkpoint-Grundlagen.
- WHOIS V0.1 ist der direkte nächste funktionale Profilbaustein, nachdem Recovery/Runtime abgeschlossen sind.
- WebWhois ist eine V0.2-Erweiterung der WHOIS.
- Forum und WebWhois/Profile sind klar getrennte Systeme.
- Nutzeränderungen sollen sofort gespeichert werden; Checkpoints sichern nur laufende Runtime-Zustände.

