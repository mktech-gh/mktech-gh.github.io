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

## Regeln für die Apps

- **Öffentliches Repository: keine Geheimnisse.** Keine Passwörter, Tokens, API-Schlüssel, internen IP-Adressen oder Hausdaten im Code. Zugangsdaten gibt der Nutzer in der App ein, sie bleiben nur im Browser (localStorage).
- Jede App ist eine eigenständige Datei: CSS und JavaScript inline. Extern nur Google Fonts oder Bibliotheken von cdnjs/jsDelivr mit fester Versionsnummer.
- **Alle Apps teilen sich eine Origin** (`mktech-gh.github.io`) und damit denselben localStorage. Jede App verwendet einen eigenen Schlüssel mit App-Präfix, z. B. `<app-name>.v1`.
- Speicherschlüssel nie umbenennen. Ändert sich das Datenformat, alte Daten beim Laden weiterhin lesen und umwandeln.
- Apps, die Daten speichern, bieten Export und Import als JSON an (Datensicherung, Übertragung auf andere Geräte). Dateiname enthält den Titel und das Datum.
- Zielgerät: Android-Handy mit Chrome, als „Zum Startbildschirm hinzufügen“ genutzt. Mobile first, helles und dunkles Farbschema. Edge kann auf dem Gerät keine Sprachausgabe.
- Sprache der Oberfläche: Deutsch in Schweizer Schreibweise (ss statt ß).
- Texte ohne Gedankenstriche formulieren (Wunsch von Markus).
- Bestätigungen (Löschen, Ersetzen) in der Seite selbst lösen, nicht mit `confirm()`.

## Apps

| Ordner | Titel | Zweck | Speicherschlüssel |
|---|---|---|---|
| `workout/` | Mein Workout | Intervall-Training mit Durchläufen, Sets, Pausen, Bildern, Sprachansage, Import/Export | `mein-workout.v1` (älterer Name ohne Präfix, nicht ändern) |
