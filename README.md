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
- **Löscheinsatz** – Die Gruppe im Löscheinsatz
- **Technische Hilfeleistung** – Die Gruppe im Hilfeleistungseinsatz

---

### Setup-Bildschirm

Hier werden alle Parameter für die Prüfung eingestellt.

#### Löscheinsatz
- **Variante** wählen: Variante I (Saugbetrieb), Variante II (Überdruckbetrieb), Variante III (Saugbetrieb mit Steigrohr)
- **Stufe** wählen (1–6)
- **Zeitanpassungen** (optionale Zu-/Abschläge): B-Länge Saugleitung, Dachverlastung, weitere
- **Prüflinge** für Testfragen eintragen (Gruppenführer + ggf. Mannschaft Stufe 6)

#### Hilfeleistung
- **Aufbau** wählen: Aufbau A (Höchstzeit 300 s) oder Aufbau B (Höchstzeit 240 s)
- **Stufe** und **Prüflinge** wie beim Löscheinsatz

Alle Zahlenfelder haben **− / +**-Tasten zum schnellen Anpassen ohne Tastatur.

---

### Testfragen (Stufen 1–6)

- **Gruppenführer:** 10 Minuten Countdown
- **Mannschaft Stufe 6:** je 5 Minuten pro Prüfling (laufen parallel)
- Akustische Zeitansagen bei 60 s, 30 s, 10 s Restzeit
- Start/Stopp und Neustart für jeden Timer separat
- **Bestanden / Nicht bestanden** je Timer manuell setzen

---

### Knoten und Stiche (nur Löscheinsatz)

- Acht Einzeltimer (einer pro Knoten/Stich) mit individuellem Zeitlimit
- Je Timer: Starten, Stoppen, Bestanden/Nicht-bestanden markieren
- Überschreitung des Zeitlimits → **2 Fehlerpunkte** pro Knoten

---

### Kuppeln der Saugleitung (nur Löscheinsatz)

- Stoppuhr für den Saugleitungsaufbau
- Zeitlimit abhängig von Variante und Zeitanpassungen
- Zweiter Versuch möglich (Mannschaft hilft → Zeitlimit entfällt)

---

### Trockensaugprobe (nur Löscheinsatz)

- 2-Minuten-Countdown (Haltezeitprüfung)
- **2 Minuten halten**: kein Fehlerpunkt
- **2. Versuch notwendig**: 5 Fehlerpunkte
- **Nicht bestanden**: Prüfung nicht abgeschlossen

---

### Einsatzübung (beide Prüfungen)

- Zentrale Stoppuhr mit Höchstzeit-Anzeige
- Akustische Zeitansagen (Minuten-Ansagen, Sprache Deutsch)
- **Läuft im Hintergrund weiter**, auch wenn zur vorherigen Seite navigiert wird
- Beim Verlassen der Seite während laufender Uhr: Sicherheitsabfrage

#### Hilfeleistung-Zusatz
- Hinweis: **Schutzleiterprüfung** nach der Zeitmessung durchführen (nicht gestoppt)

---

### Ergebnis / Bericht

Zusammenfassung der gesamten Prüfung:

- Zeiten aller Abschnitte auf einen Blick
- **Fehlerpunkte** (Knoten, Trockensaugprobe)
- **Gesamtstatus:** „Alle Zeiten aufgezeichnet" oder Hinweis auf fehlende Abschnitte
- „**Neue Prüfung**"-Button (mit Sicherheitsabfrage) → zurück zum Startbildschirm
- „**← Zurück**"-Button zum Korrigieren

---

## Verteilen im Verein

Den Link einfach per **WhatsApp, E-Mail oder Aushang** weitergeben:

```
https://zapfs.github.io/Feuerwehr-Stoppuhr/Leist-Pruef-Uhr.html
```

Jeder Schiedsrichter öffnet den Link einmal mit Internetverbindung und installiert die App auf seinem Smartphone. Danach ist sie offline verfügbar.

---

## Technische Hinweise

- Keine Installation auf einem Server notwendig
- Läuft komplett im Browser – keine Daten werden übertragen
- Getestet mit Safari (iOS) und Chrome (Android)
- Sprache: Deutsch (Zeitansagen per Sprachausgabe)
