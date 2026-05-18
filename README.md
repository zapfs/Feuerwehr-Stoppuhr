# Leist-Prüf-Uhr

Digitale Stoppuhr-App für Feuerwehr-Leistungsprüfungen in Bayern.

Unterstützt beide Prüfungsarten:
- **Die Gruppe im Löscheinsatz** (Variante I, II, III)
- **Die Gruppe im Hilfeleistungseinsatz** (Aufbau A, B)

---

## App öffnen

**Link:**
```
https://zapfs.github.io/Feuerwehr-Stoppuhr/Leist-Pruef-Uhr.html
```

Diesen Link einfach im Browser auf dem Smartphone öffnen – kein App Store, keine Installation erforderlich. Die App kann danach auch **offline** genutzt werden.

---

## Installation als App (empfohlen)

Die App ist eine **Progressive Web App (PWA)**: Sie kann wie eine echte App auf dem Home-Bildschirm installiert werden und läuft dann im Vollbild ohne Browser-Leiste. Nach dem ersten Laden funktioniert sie auch ohne Internetverbindung.

---

### Installation auf dem iPhone / iPad (iOS – Safari)

> **Wichtig:** Die Installation funktioniert **nur mit Safari**. Andere Browser (Chrome, Firefox) auf iOS unterstützen die Installation nicht.

1. Den Link oben in **Safari** öffnen.
2. Unten in der Mitte auf das **Teilen-Symbol** tippen (Rechteck mit Pfeil nach oben ↑).
3. Im Menü nach unten scrollen und **„Zum Home-Bildschirm"** antippen.
4. Den Namen bestätigen und **„Hinzufügen"** tippen.

Die App erscheint jetzt als Symbol auf dem Home-Bildschirm und startet im Vollbild – genau wie eine native App.

---

### Installation auf Android (Chrome)

1. Den Link oben in **Chrome** öffnen.
2. Es erscheint automatisch ein Banner am unteren Bildschirmrand: **„App installieren"** – diesen antippen.  
   Falls das Banner nicht erscheint: Menü (drei Punkte oben rechts) → **„App installieren"** oder **„Zum Startbildschirm hinzufügen"**.
3. Im Dialog **„Installieren"** bestätigen.

Die App erscheint jetzt als Symbol in der App-Übersicht und läuft im Vollbild ohne Browser-Leiste.

---

### Offline-Nutzung

Nach der ersten Installation (mit Internetverbindung) ist die App **vollständig offline nutzbar**. Updates werden automatisch geladen, sobald das Gerät wieder online ist und die App geöffnet wird.

---

## Funktionsübersicht

### Startbildschirm – Prüfungsart wählen

Beim Start wird gefragt, welche Prüfung durchgeführt wird:
- **🔴 Löscheinsatz** – Die Gruppe im Löscheinsatz
- **🟢 Technische Hilfeleistung** – Die Gruppe im Hilfeleistungseinsatz

---

### Setup-Bildschirm

Hier werden alle Parameter für die Prüfung eingestellt.

#### Löscheinsatz
- **Variante** wählen: I (Saugbetrieb), II (Überdruckbetrieb), III (Saugbetrieb mit Steigrohr)
- **Testfragen** aktivieren: Gruppenführer (10 min) und/oder Anzahl Stufe-6-Prüflinge (je 5 min)
- **Zeitanpassungen** (optionale Zu-/Abschläge je nach Variante): B-Länge Saugleitung, abweichende Schlauchlänge, Halteleine, Dachverlastung u. a.
- Berechnete Höchstzeiten werden sofort angezeigt

#### Hilfeleistung
- **Aufbau** wählen: A (Höchstzeit 300 s) oder B (Höchstzeit 240 s)
- **Testfragen** wie beim Löscheinsatz
- Keine Zeitanpassungen

---

### Testfragen-Statusleiste

Während die Testfragen-Timer laufen, zeigt eine **Statusleiste oben** auf jeder Seite die verbleibende Zeit an:
- **Orange blinkend**: 30 Sekunden vor Ablauf der Höchstzeit
- **Rot blinkend**: Höchstzeit überschritten
- Tippen auf **„→ Öffnen"** springt direkt zum Testfragen-Screen

---

### Testfragen (Stufen 1–6)

- **Gruppenführer:** 10-Minuten-Countdown
- **Mannschaft Stufe 6:** je 5 Minuten pro Prüfling (laufen parallel zueinander und zur übrigen Prüfung)
- Akustische Zeitansagen bei 60 s, 30 s, 10 s Restzeit
- Start / Stopp / Zurücksetzen für jeden Timer separat
- Ergebnis wird inline angezeigt; bei Überschreitung erscheint ein **✓ OK**-Button zur nachträglichen Korrektur (falls der Schiedsrichter sich verstoppt hat)

---

### Knoten und Stiche *(nur Löscheinsatz)*

- 8 Einzeltimer (einer pro Knoten/Stich) mit individuellem Zeitlimit
- Ergebnis erscheint direkt nach dem Stoppen: Zeit in Sekunden, bei Überschreitung **+2 Fehlerpunkte**
- Bei Überschreitung: **✓ OK**-Button für nachträgliche Korrektur

---

### Kuppeln der Saugleitung *(nur Löscheinsatz)*

- Stoppuhr für den Saugleitungsaufbau mit variantenabhängigem Zeitlimit
- Zweiter Versuch mit Mannschaftshilfe möglich (Zeitlimit entfällt dann)

---

### Trockensaugprobe *(nur Löscheinsatz)*

- Hauptstoppuhr + 2-Minuten-Haltezeitkontrolle
- 2. Versuch nötig → **5 Fehlerpunkte**
- Bei Überschreitung der Hauptstoppuhr: **✓ OK**-Button für nachträgliche Korrektur

---

### Einsatzübung *(beide Prüfungsarten)*

- Zentrale Stoppuhr mit prominenter Höchstzeit-Anzeige
- Akustische Zeitansagen in Minutenschritten (Sprachausgabe Deutsch)
- Läuft **im Hintergrund weiter**, auch wenn zur vorherigen Seite navigiert wird; beim Verlassen während laufender Uhr erscheint eine Sicherheitsabfrage
- **Warnung** beim Betreten des Screens, wenn noch Testfragen-Timer laufen (alle Sonderaufgaben müssen vor der Einsatzübung abgeschlossen sein)
- Bei Überschreitung der Höchstzeit: **✓ OK**-Button für nachträgliche Korrektur

#### Hilfeleistung-Zusatz
- Hinweis: **Schutzleiterprüfung** nach der Zeitmessung durchführen (wird nicht gestoppt)

---

### Ergebnis / Bericht

Abschlusszusammenfassung der Prüfung:

- **Checkliste** aller Abschnitte: ✅ Zeit aufgezeichnet / ⬜ Zeit fehlt
- **Fehlerpunkte** werden inline bei jedem betroffenen Timer angezeigt (nicht kumuliert)
- **„← Zurück"**-Button zum Korrigieren einzelner Zeiten
- **„Neue Prüfung"**-Button (mit Sicherheitsabfrage) → setzt alle Zeiten **und** alle Setup-Einstellungen zurück, zurück zum Startbildschirm

---

## ✓ OK – Korrektur-Button

Jeder Timer besitzt einen kleinen **✓ OK**-Button, der ausschließlich erscheint, wenn die Höchstzeit **überschritten** wurde. Er ermöglicht dem Schiedsrichter, die Zeit nachträglich auf Höchstzeit − 1 s zu setzen, wenn er sich beim Stoppen vertippt hat. Nach der Korrektur zeigt der Timer das normale Ergebnis – ohne sichtbaren Hinweis auf die Korrektur.

---

## Verteilen im Verein

Den Link einfach per **WhatsApp, E-Mail oder Aushang** weitergeben:

```
https://zapfs.github.io/Feuerwehr-Stoppuhr/Leist-Pruef-Uhr.html
```

Jeder Schiedsrichter öffnet den Link einmal mit Internetverbindung und installiert die App auf seinem Smartphone. Danach ist sie offline verfügbar.

---

## Technische Hinweise

- Keine Server-Installation notwendig – läuft komplett im Browser
- Keine Daten werden übertragen oder gespeichert
- Getestet mit Safari (iOS) und Chrome (Android)
- Sprache: Deutsch (Zeitansagen per Web Speech API)
