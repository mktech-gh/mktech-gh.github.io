# Web-Apps von Markus (GitHub Pages)

Dieses Repository ist die **einzige gültige Fassung (Master)** aller Web-Apps von Markus.
Es wird über GitHub Pages unter https://mktech-gh.github.io/ veröffentlicht.
Claude-Artifacts sind nicht mehr Master und werden für diese Apps nicht mehr verwendet
(die Artifact-Umgebung sperrt u. a. die Sprachausgabe).

## Ablauf bei jeder Änderung

1. Vor jeder Änderung den aktuellen Stand holen (`git pull`). Markus kann Dateien auch direkt auf GitHub ändern.
2. Änderung machen.
3. Testen: Script-Syntax prüfen (`node --check` auf das extrahierte `<script>`), Seite mit Playwright bei 400 px Breite laden und auf Konsolenfehler prüfen.
4. Commit mit kurzer deutscher Nachricht, die sagt, was sich für den Nutzer ändert.
5. Direkt auf `main` pushen. Nach etwa 1 Minute ist die Änderung live.
6. Markus die Adresse der App nennen und sagen, dass er die Seite neu laden soll.

## Struktur

- Eine App pro Ordner: `<app-name>/index.html`, Ordnername klein, mit Bindestrichen.
  Adresse: `https://mktech-gh.github.io/<app-name>/`
- **Ordnernamen nie umbenennen oder verschieben.** Die gespeicherten Browserdaten hängen an der Adresse; eine neue Adresse startet leer.
- `index.html` im Hauptordner ist die Übersicht aller Apps. Bei einer neuen App dort einen Eintrag ergänzen und die Tabelle unten nachführen.
- `.nojekyll` bleibt bestehen, damit GitHub die Dateien unverändert ausliefert.
- `vorlagen/` enthält Vorlagen zum Übernehmen in Apps. Das ist keine App und kommt nicht in die Übersicht.

## Regeln für die Apps

- **Öffentliches Repository: keine Geheimnisse.** Keine Passwörter, Tokens, API-Schlüssel, internen IP-Adressen oder Hausdaten im Code. Zugangsdaten gibt der Nutzer in der App ein, sie bleiben nur im Browser (localStorage).
- Jede App ist eine eigenständige Datei: CSS und JavaScript inline. Extern nur Google Fonts oder Bibliotheken von cdnjs/jsDelivr mit fester Versionsnummer.
- **Alle Apps teilen sich eine Origin** (`mktech-gh.github.io`) und damit denselben localStorage. Jede App verwendet einen eigenen Schlüssel mit App-Präfix, z. B. `<app-name>.v1`.
- Speicherschlüssel nicht umbenennen. Falls doch nötig, beim Laden den alten Schlüssel lesen und übernehmen, damit nichts verloren geht. Ändert sich das Datenformat, alte Daten beim Laden weiterhin lesen und umwandeln.
- Jede App, die Einstellungen oder Daten speichert, hat den **Standard-Mechanismus zur Sicherung und Wiederherstellung** (siehe unten).
- Zielgerät: Android-Handy, Standardbrowser Edge (Chrome geht auch), als „Zum Startbildschirm hinzufügen“ genutzt. Mobile first, helles und dunkles Farbschema. Sprachausgabe funktioniert auf GitHub Pages in Edge und Chrome.
- Sprache der Oberfläche: Deutsch in Schweizer Schreibweise (ss statt ß).
- Texte ohne Gedankenstriche formulieren (Wunsch von Markus).
- Bestätigungen (Löschen, Ersetzen) in der Seite selbst lösen, nicht mit `confirm()`.
- Jede App zeigt ganz unten `Version JJJJ-MM-TT` (Element `#app-version`, lokales Datum der letzten Änderung). Bei jeder Änderung an der App nachführen, damit Markus sieht, ob die neue Version geladen ist.

## Sicherung und Wiederherstellung (Standard)

Verbindlich für jede App mit gespeicherten Einstellungen oder Daten.
**Referenz-Umsetzung: `vorlagen/sicherung.html`.** Die mit `SICHERUNG` markierten Teile (CSS, HTML, JS)
unverändert übernehmen und nur den Block `APP-SPEZIFISCH` anpassen.

Bedienung, in einem Abschnitt „Sicherung“ weit unten in der App:
- **Als .json-Datei speichern**: lädt die Sicherung als Datei herunter.
- **Als Text kopieren**: kopiert dieselbe Sicherung als Text in die Zwischenablage. Klappt das nicht, wird der Text im Feld darunter angezeigt und markiert.
- **Datei importieren**: öffnet die Dateiauswahl mit `accept=".json,application/json,text/plain"`. Nicht weglassen, sonst bietet Android auch Kamera und Video an.
- **Text einfügen und importieren**: aufklappbares Textfeld für eingefügten Sicherungstext.
- Vor dem Wiederherstellen eine Rückfrage in der Seite mit kurzer Zusammenfassung des Inhalts und den Knöpfen „Ersetzen“ und „Abbrechen“. Wiederherstellen ersetzt den aktuellen Stand vollständig.

Format der Sicherung (JSON):
```json
{ "app": "<app-name>", "version": 1, "exported": "2026-11-08T15:00:00.000Z", "title": "…", "data": { … } }
```
- `app` ist der Ordnername der App. Sicherungen einer anderen App werden mit einer klaren Meldung abgelehnt.
- `version` ist die Formatversion. Bei Änderungen am Datenformat erhöhen und ältere Versionen in `migrate()` umwandeln. Sicherungen aus einer neueren Version werden abgelehnt.
- `data` enthält alles, was die App gespeichert hat, inklusive eigener Bilder (als data-URL). Beim Import immer prüfen und bereinigen (`validate()`), nie ungeprüft übernehmen.
- Nur die Daten der eigenen App sichern, nie den ganzen localStorage (alle Apps teilen ihn).

Dateiname: `<titel-als-slug>-JJJJ-MM-TT.json`, mit lokalem Datum. Umlaute werden ersetzt (ä→ae, ö→oe, ü→ue, ß→ss), alles andere ausser a–z und 0–9 wird zu Bindestrichen. Ohne Titel der App-Name.

Die Vorlage setzt die Grundfunktionen `$(id)` und `toast(text)` sowie `<div class="toast" id="toast" hidden></div>` voraus. Hat die App sie schon, nicht doppelt definieren.

## Apps

| Ordner | Titel | Zweck | Speicherschlüssel |
|---|---|---|---|
| `workout/` | Mein Workout | Intervall-Training mit Durchläufen, Sets, Pausen, Bildern, Sprachansage, Sicherung | `workout.v1` (übernimmt beim ersten Start einmalig den alten Schlüssel `mein-workout.v1`) |
