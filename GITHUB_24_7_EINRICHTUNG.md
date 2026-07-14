# Kostenlose 24/7-Einrichtung mit GitHub Actions

Damit der Bot auch bei ausgeschaltetem PC läuft, wird er in einem öffentlichen GitHub-Repository ausgeführt. Die E-Mail-Zugangsdaten werden nicht in Dateien hochgeladen, sondern als geschützte Repository-Secrets gespeichert.

## 1. GitHub-Konto und öffentliches Repository

1. Auf GitHub anmelden oder kostenlos registrieren.
2. Oben rechts auf **+** und danach **New repository** klicken.
3. Als Namen zum Beispiel `midea-knuffelwuff-bot` eintragen.
4. **Public** auswählen.
5. **Create repository** anklicken.

Wichtig: Die Datei `.env` darf niemals hochgeladen werden. Sie ist bereits über `.gitignore` ausgeschlossen.

## 2. Dateien hochladen

1. Auf der neuen Repository-Seite **uploading an existing file** anklicken.
2. Alle Dateien und Unterordner aus dem Ordner `midea-stock-bot` in das Upload-Feld ziehen.
3. Prüfen, dass auch der versteckte Ordner `.github` hochgeladen wurde.
4. Prüfen, dass `.env` und `state.json` nicht in der Dateiliste stehen.
5. Unten **Commit changes** anklicken.

Falls Windows den Ordner `.github` nicht sichtbar anzeigt, im Explorer unter **Ansicht → Einblenden → Ausgeblendete Elemente** aktivieren.

## 3. E-Mail-Daten als GitHub-Secrets speichern

Im Repository:

1. **Settings** öffnen.
2. Links **Secrets and variables → Actions** auswählen.
3. Unter **Repository secrets** jeweils **New repository secret** anklicken.
4. Die folgenden acht Secrets einzeln anlegen:

| Name | Wert für Gmail |
|---|---|
| `SMTP_HOST` | `smtp.gmail.com` |
| `SMTP_PORT` | `587` |
| `SMTP_USERNAME` | vollständige Gmail-Adresse |
| `SMTP_PASSWORD` | 16-stelliges Google-App-Passwort ohne Leerzeichen |
| `EMAIL_FROM` | dieselbe Gmail-Adresse |
| `EMAIL_TO` | gewünschte Empfängeradresse |
| `SMTP_STARTTLS` | `true` |
| `SMTP_SSL` | `false` |

Das normale Google-Passwort darf nicht verwendet werden.

## 4. Schreibberechtigung für den monatlichen Aktivitäts-Workflow

1. Im Repository **Settings → Actions → General** öffnen.
2. Bis **Workflow permissions** nach unten scrollen.
3. **Read and write permissions** auswählen.
4. **Save** anklicken.

Der monatliche Workflow aktualisiert nur `.github/keepalive.txt`. Dadurch bleibt der Zeitplan auch nach mehr als 60 Tagen ohne manuelle Änderungen aktiv.

## 5. Ersten Test manuell starten

1. Im Repository oben **Actions** öffnen.
2. Links **Midea und Knuffelwuff prüfen** auswählen.
3. Rechts **Run workflow → Run workflow** anklicken.
4. Den gestarteten Lauf öffnen und warten, bis alle Schritte grün sind.

Beim ersten Knuffelwuff-Lauf wird nur der aktuelle Bestand als Ausgangsbasis gespeichert. Dadurch werden nicht sofort E-Mails für alle vorhandenen Artikel verschickt.

## 6. Automatischer Betrieb

Der Workflow läuft automatisch zu den Minuten 07, 22, 37 und 52 jeder Stunde. Das entspricht vier Prüfungen pro Stunde und damit einem Intervall von ungefähr 15 Minuten.

GitHub kann geplante Läufe bei hoher Auslastung etwas verzögern. Der Bot ist daher dauerhaft aktiv, aber nicht sekundengenau.

## 7. Testmail aus GitHub senden

Der normale Workflow verschickt nur bei einer echten Verfügbarkeitsänderung eine E-Mail. Für einen reinen E-Mail-Test kann lokal weiterhin `2_email_testen_windows.bat` verwendet werden.

Alternativ kann im GitHub-Workflow vorübergehend der Befehl

```yaml
run: python bot.py --test-notification
```

eingetragen und der Workflow manuell gestartet werden. Danach wieder auf

```yaml
run: python bot.py --once
```

zurückstellen.

## 8. Kostenkontrolle

Für den 15-Minuten-Zeitplan ein öffentliches Repository verwenden. Bei öffentlichen Repositorys sind Standard-GitHub-Runner kostenlos. Keine Zahlungsdaten hinterlegen und keine größeren Runner auswählen.

## 9. Bot pausieren

Im Repository **Actions** öffnen, den jeweiligen Workflow auswählen, rechts das Menü mit den drei Punkten öffnen und **Disable workflow** anklicken.
