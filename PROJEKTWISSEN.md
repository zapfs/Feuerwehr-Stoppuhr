# Feuerwehr Leistungsprüfung – Vollständiges Projektwissen

Dieses Dokument fasst alle fachlichen Regeln und technischen Entscheidungen der
App „Leist-Prüf-Uhr" zusammen, damit ein neues Projekt ohne Informationsverlust
gestartet werden kann.

---

## 1. App-Übersicht

**Name:** Leist-Prüf-Uhr  
**Datei:** `Leist-Pruef-Uhr.html` (Single-File-App, kein Build-Tool, kein Framework)  
**Zweck:** Digitale Stoppuhr für Feuerwehr-Leistungsprüfungen Bayern  
**Hosting:** GitHub Pages – `https://zapfs.github.io/Feuerwehr-Stoppuhr/Leist-Pruef-Uhr.html`  
**PWA:** Ja – `manifest.json` + `sw.js` (Cache-first Service Worker)  

Unterstützte Prüfungen:
- **Löscheinsatz** – „Die Gruppe im Löscheinsatz" (Variante I / II / III)
- **Hilfeleistung** – „Die Gruppe im Hilfeleistungseinsatz" (Aufbau A / B)

---

## 2. Screen-Reihenfolge (Navigation)

```
Auswahl → Setup → [Testfragen] → [Knoten] → [Saugleitung] → [Trockensaug] → Einsatz → Ergebnis
```

Optionale Screens je Modus:

| Screen         | Löscheinsatz | Hilfeleistung |
|----------------|:---:|:---:|
| Testfragen     | wenn GF ≥ Stufe 2 oder Stufe-6-Mitglieder | gleich |
| Knoten & Stiche| ✓ | ✗ |
| Saugleitung    | ✓ (alle Varianten) | ✗ |
| Trockensaugprobe| ✓ (alle Varianten) | ✗ |
| Einsatzübung   | ✓ | ✓ |
| Schutzleiter-Hinweis | ✗ | ✓ (im Ergebnis-Screen) |

Navigation: History-Stack (`Nav.history[]`). Zurück = `pop()` ohne erneutes `activate()`.
Beim Verlassen der Einsatzübung mit laufender Uhr → Sicherheitsabfrage.

---

## 3. Löscheinsatz – Fachregeln

### 3.1 Varianten

| Variante | Beschreibung | Basis-Einsatzzeit |
|---|---|---|
| I | Außenangriff – Wasserentnahme aus Hydranten | 190 s |
| II | Außenangriff – Wasserentnahme mit Saugleitung | 240 s |
| III | Innenangriff – Wasserentnahme aus Hydranten | 300 s |

### 3.2 Zeitanpassungen – Einsatzübung

| Anpassung | Variante | Wert |
|---|---|---|
| Zusätzliche B-Leitungs-Längen (über 1 hinaus) | I, III | +10 s je Länge |
| Saugschläuche auf Fahrzeugdach* | II | +60 s |

*Nur wenn Saugschläuche während der Übung vom Dach entnommen werden.
Wenn sie vorher abgelegt wurden (LF 20 KATS etc.) entfällt die Zusatzzeit (FAQ).

Formel:
```
Var I:   max = 190 + 10 * extraBLaengen
Var II:  max = 240 + (dachverlastung ? 60 : 0)
Var III: max = 300 + 10 * extraBLaengen
```

### 3.3 Zeitanpassungen – Saugleitung kuppeln

Basis: 100 s

| Anpassung | Variante | Wert |
|---|---|---|
| Saugschläuche auf Fahrzeugdach | I, III | +60 s |
| B-Saugleitung KLF (Variante I/III) | I, III | −10 s |
| B-Saugleitung KLF (Variante II) | II | −10 s |
| Abweichende Saugschlauchanzahl (von 4) | II | ±10 s je Schlauch |
| Ohne Halte-/Ventilleine und Saugkorb | II | −20 s |

Formel:
```
sauglMax = max(10,
  100
  + (dachverlastung ? 60 : 0)      // Var I/III
  + (bSaugleitungI  ? -10 : 0)     // Var I/III
  + (bSaugleitungII ? -10 : 0)     // Var II
  + saugschlauchCount * saugschlauchSign * 10  // Var II, Wert −10..+10
  + (ohneHalteleine ? -20 : 0)     // Var II
)
```

Hinweis: Saugschlauchanzahl-Eingabe ist ein einzelnes Zahlenfeld (−10 bis +10),
Vorzeichen bedeutet Abweichung von 4 (negativ = weniger, positiv = mehr).

### 3.4 Trockensaugprobe

- Haltezeit: **120 s** (2 Minuten Unterdruck halten)
- 1. Versuch bestanden: **0 Fehlerpunkte**
- 2. Versuch nötig: **+5 Fehlerpunkte**
- Nicht bestanden: Prüfung gilt als **unvollständig**
- Laut FAQ: Immer komplette Saugleitung mit 4 Saugschläuchen, Saugkorb, Halte- und Ventilleine (auch bei Var II mit weniger Schläuchen!)

### 3.5 Knoten und Stiche

8 Einzeltimer mit Zeitlimit je Funktion:

| Funktion | Zeitlimit |
|---|---|
| Maschinist – Zimmermannschlag | 15 s |
| Melder – Mastwurf gestochen | 15 s |
| Angriffstruppführer (ATF) – Brustbund mit Spierenstich | 40 s |
| Angriffstruppmann (ATM) – Brustbund mit Spierenstich | 40 s |
| Wassertruppführer (WTF) – Mastwurf gelegt mit Halbschlag | 15 s |
| Wassertruppmann (WTM) – Mastwurf gelegt mit Halbschlag | 15 s |
| Schlauchtruppführer (STF) – Mastwurf gelegt mit Halbschlag | 15 s |
| Schlauchtruppmann (STM) – Mastwurf gelegt mit Halbschlag | 15 s |

**Fehlerpunkte bei Zeitüberschreitung: 2 FP pro Knoten/Stich** (nicht 5!)
Quelle: Richtlinie. Eine Überschreitung führt NICHT zum Nicht-Bestehen.

### 3.6 Zeitansagen Einsatzübung

Nur Minutenmarken anzeigen/ansagen, die ≤ Höchstzeit sind:

```javascript
annTimes = [60, 120, 180, 240, 300].filter(t => t <= einsatzMax)
```

Beispiele:
- Var I (190 s): 60, 120, 180
- Var II (240 s): 60, 120, 180, 240
- Var III (300 s): 60, 120, 180, 240, 300

### 3.7 Testfragen Löscheinsatz

| Teilnehmer | Bedingung | Zeit |
|---|---|---|
| Gruppenführer | Ab Stufe 2 (unabhängig von Gruppe) | 10 Minuten Countdown |
| Mannschaft mit Stufe 6 | Je Teilnehmer | 5 Minuten Countdown |

GF-Stufe entscheidet allein über Testfragen (FAQ). Testfragen laufen parallel zur Navigation.
Zeitansagen: 60 s, 30 s, 10 s Restzeit.

---

## 4. Hilfeleistung – Fachregeln

### 4.1 Aufbau-Varianten

| Aufbau | Beschreibung | Höchstzeit | Zeitansagen |
|---|---|---|---|
| A | Geräte außerhalb des Fahrzeugs | 300 s | 60, 120, 180, 240, 300 s |
| B | Geräte im/am Fahrzeug | 240 s | 60, 120, 180, 240 s |

**Keine Zeitanpassungen** (FAQ bestätigt: keine Unterscheidung nach Gerätealter).

### 4.2 Zeitmessung Einsatzübung

- **Start:** Kommando „Absitzen!"
- **Ende:** Rückmeldung „Person befreit, an Rettungsdienst übergeben"

### 4.3 Schutzleiterprüfung

Wird **nach** der Zeitmessung durchgeführt (nicht gestoppt).
→ Hinweistext im Ergebnis-Screen: „Schutzleiterprüfung an verwendeten Geräten (Stromerzeuger, Leitungen, elektrisch betriebene Geräte) durchführen."

### 4.4 Testfragen Hilfeleistung

Identisch zum Löscheinsatz: GF ab Stufe 2, Stufe-6-Mitglieder je 5 min.
Stufe-6-Frage gilt auch für Hilfeleistung (FAQ: Gefahrenmatrix bei Stufe 6).

### 4.5 Fehlerpunkte

Keine eigenen Knoten-/Saugleitungs-Fehlerpunkte.
Fehlerpunkte nur durch Testfragen (bewertet der Schiedsrichter separat).

---

## 5. Fehlerpunkte – Gesamtübersicht

| Ereignis | FP |
|---|---|
| Knoten/Stich Zeitüberschreitung | 2 FP je Knoten |
| Trockensaugprobe 2. Versuch | 5 FP |
| Trockensaugprobe nicht bestanden | Prüfung unvollständig |

**25 FP-Gesamtlimit (Stufe 1):** Bewertung durch Schiedsrichter – nicht in der App implementiert (Schiedsrichter-Aufgabe).

Die App zeigt:
- Fehlerpunkte-Summe im Ergebnis-Screen
- Gesamtstatus: „Alle Zeiten aufgezeichnet" oder „Fehlende Zeiten: [Liste]"
- Kein automatisches „Bestanden/Nicht bestanden" (zu viele externe Kriterien)

---

## 6. Technische Architektur

### 6.1 Stack

- **Vanilla HTML/CSS/JS** – kein Framework, kein Build-Tool
- **Single File** – alles in `Leist-Pruef-Uhr.html` (~2800 Zeilen)
- **Web Audio API** – Beep-Töne für Zeitansagen
- **SpeechSynthesis** – Sprach-Zeitansagen (de-DE)
- **requestAnimationFrame** – Timer-Ticks

### 6.2 Layout-Struktur (HTML)

```
body (display:flex; flex-direction:column; height:100%; overflow:hidden)
  ├── #tf-status-bar        (flex-shrink:0; in Dokumentfluss, nicht position:fixed)
  └── #app                  (flex:1; min-height:0; display:flex; flex-direction:column)
        └── .screen.active  (flex:1; min-height:0; display:flex; flex-direction:column)
              ├── .screen-header
              ├── .screen-body   (flex:1; min-height:0; overflow-y:auto)
              └── .screen-footer
```

**Wichtig:** `#tf-status-bar` ist im normalen Dokumentfluss (kein `position:fixed`).
Er schiebt `#app` automatisch nach unten, egal wie viele Zeilen er hat.

**Safari-Kompatibilität:**
- Kein `viewport-fit=cover` (würde Content unter Safari-Navigationsleiste schieben)
- `overscroll-behavior: none` auf html/body → verhindert Pull-to-Refresh
- `overscroll-behavior-y: contain` auf .screen-body → internes Scrollen OK

### 6.3 TimerEngine-Klasse

```javascript
class TimerEngine {
  constructor({ maxSeconds, onTick, onAnnounce, announceTimes, countDown })
  start()   // speichert performance.now() Offset, startet rAF-Loop
  stop()    // pausiert, erhält elapsed
  reset()   // zurück auf 0
  get elapsed()  // Sekunden seit Start (Float)
  get state()    // 'idle' | 'running' | 'stopped'
}
```

- Läuft weiter wenn Screen gewechselt wird (kein Stop bei Navigation)
- Bei `activate()` wird laufender Timer wiederhergestellt: Buttons auf Stop-Zustand setzen

### 6.4 SESSION-Objekt

```javascript
SESSION = {
  mode: 'loscheinsatz' | 'hilfeleistung',
  variante: 'I'|'II'|'III'|'A'|'B',
  einsatzMax: Number,        // berechnete Höchstzeit Einsatz
  saugleitungMax: Number,    // berechnete Höchstzeit Saugleitung
  gfTestfragen: Boolean,
  stufe6Count: Number,
  adj: { extraBLaengen, saugschlauchCount, saugschlauchSign,
         ohneHalteleine, bSaugleitungII, bSaugleitungI, dachverlastung },
  results: {
    knoten: { maschinist, melder, atf, atm, wtf, wtm, stf, stm },
    saugleitung: { elapsed, versuche },
    trockensaug: { mainElapsed, versuche, halteOk },
    einsatz: Number,
    testfragenGF: Number,
    testfragenMembers: Number[],
  }
}
```

### 6.5 TF-Manager (Testfragen, parallel)

```javascript
TF = {
  engines: { gf: TimerEngine, members: TimerEngine[] },
  updateStatusBar(),   // zeigt/versteckt gelben Balken
  resetAll(),
}
```

Gelber Status-Balken oben erscheint wenn Testfragen-Timer läuft und man nicht auf dem Testfragen-Screen ist. Er zeigt GF-Restzeit und Mannschafts-Restzeit.

### 6.6 Navigation

```javascript
Nav = {
  current: String,
  history: String[],
  show(screenName, { pushHistory }),
  next(),
  back(),
  _getOrder(),   // liefert Screen-Reihenfolge je nach SESSION.mode + variante
}
```

`_getOrder()` filtert Screens modusabhängig:
```javascript
list = ['auswahl', 'setup'];
if (gfTestfragen || stufe6Count > 0) list.push('testfragen');
if (mode === 'loscheinsatz') {
  list.push('knoten', 'saugleitung', 'trockensaug');
}
list.push('einsatz', 'ergebnis');
```

### 6.7 Custom Confirm Modal

`confirmReset(message)` → Promise<boolean>
Ersetzt `window.confirm` (funktioniert in PWA-Modus).

### 6.8 Stepper-Buttons

Delegierter Event-Handler auf `document`:
```javascript
document.addEventListener('click', e => {
  const btn = e.target.closest('.stepper-btn');
  const input = document.getElementById(btn.dataset.for);
  const delta = Number(btn.dataset.delta);
  input.value = Math.min(max, Math.max(min, (parseInt(input.value)||0) + delta));
  input.dispatchEvent(new Event('input', { bubbles: true }));
  input.dispatchEvent(new Event('change', { bubbles: true }));
});
```

### 6.9 In-App Hilfe

Vollbild-Overlay mit `<details>/<summary>` Accordion-Abschnitten:
- Installation iOS / Android
- Offline-Nutzung
- Löscheinsatz-Ablauf
- Hilfeleistung-Ablauf
- Fehlerpunkte-Übersicht

Geöffnet via „ℹ️ Hilfe & Bedienung"-Button auf dem Auswahl-Screen.

---

## 7. PWA-Konfiguration

### manifest.json
```json
{
  "name": "Leist-Prüf-Uhr",
  "short_name": "Leist-Prüf-Uhr",
  "start_url": "./Leist-Pruef-Uhr.html",
  "display": "standalone",
  "background_color": "#1a1a1a",
  "theme_color": "#1a1a1a",
  "icons": [
    { "src": "icons/icon-192.png", "sizes": "192x192", "type": "image/png" },
    { "src": "icons/icon-512.png", "sizes": "512x512", "type": "image/png" }
  ]
}
```

### sw.js (Cache-first)
```javascript
const CACHE = 'leist-pruef-uhr-v6';
const ASSETS = ['./Leist-Pruef-Uhr.html'];
// install: cache assets + skipWaiting
// activate: delete old caches + clients.claim
// fetch: cache first, then network
```

**Cache-Version bei jeder Änderung erhöhen!**

---

## 8. Farbpalette (CSS Custom Properties)

```css
--c-ok:        #2e7d32   /* Grün – bestanden, laufend */
--c-ok-light:  #43a047
--c-warn:      #f57f17   /* Orange – Warnung, TF-Statusbalken */
--c-warn-light:#fbc02d
--c-danger:    #b71c1c   /* Rot – nicht bestanden, Fehler */
--c-accent:    #e53935   /* Akzentrot – Buttons, Badges */
--c-bg:        #1a1a1a   /* Hintergrund */
--c-surface:   #242424   /* Karten, Header */
--c-surface2:  #2e2e2e   /* Sekundäre Flächen */
--c-border:    #3a3a3a
--c-text:      #f0f0f0
--c-muted:     #999
```

---

## 9. Bekannte Randfälle & FAQ-Erkenntnisse

| Thema | Regel |
|---|---|
| Dachverlastung +60 s | Nur wenn Saugschläuche WÄHREND der Übung vom Dach entnommen werden. Bei Vorab-Ablage: kein Zuschlag. |
| GF Testfragen | Hängt nur von GF-Stufe ab (ab Stufe 2), nicht von Gruppenstufe. |
| Trockensaugprobe Var II | Immer komplette Saugleitung (4 Schläuche, Saugkorb, Leine) – auch wenn weniger Schläuche normal genutzt werden. |
| TSF-W TS im Fahrzeug | Keine Zeitanpassung ob TS entnommen oder im Fahrzeug bleibt. |
| Systemtrenner/Rückfluss | Keine Zeitanpassung erforderlich. |
| Knoten FP | 2 FP je überschrittenem Knoten (nicht 5, nicht Ausschlusskriterium). |
| Trockensaug 2. Versuch | +5 FP. Nicht bestanden = Prüfung unvollständig (nicht gesondert „nicht bestanden"). |
| Hilfeleistung Hydraulikgeräte | Keine Zeitunterschiede zwischen alt/neu. Akkugeräte jetzt erlaubt. |
| Stufe 1 Gruppe 25 FP-Limit | Gilt auch wenn GF/Maschinist höhere Stufe ablegen. Schiedsrichter-Bewertung. |
| Testfrage GF Helm (Frage 53/C) | Wird nicht mehr gewertet. Kein Einfluss auf unsere App. |

---

## 10. GitHub-Repository

**Repo:** `zapfs/Feuerwehr-Stoppuhr`  
**Haupt-Branch:** `main` (wird von GitHub Pages bedient)  
**Feature-Branch:** `claude/analyze-code-explanation-op0z8`  

**Release-Workflow** (`.github/workflows/release.yml`):
- Trigger: Push von Tag `v*`
- Packt `Leist-Pruef-Uhr.zip` und hängt sie ans GitHub Release
- Benötigt: `permissions: contents: write`

**Release erstellen:**
```bash
git tag v2.x && git push origin v2.x
```

---

## 11. Installations-Anleitung für Vereinsmitglieder

### iOS (Safari)
1. Link in **Safari** öffnen (nicht Chrome/Firefox!)
2. Teilen-Symbol (↑) antippen
3. „Zum Home-Bildschirm" wählen
4. „Hinzufügen" bestätigen

### Android (Chrome)
1. Link in **Chrome** öffnen
2. Banner „App installieren" antippen (oder Menü ⋮ → „App installieren")
3. „Installieren" bestätigen

**Offline:** Nach erstem Laden mit Internet vollständig offline nutzbar.
**Updates:** Automatisch beim nächsten Öffnen mit Internet.

---

## 12. Nicht implementierte Regelungen (bewusst weggelassen)

- **25 FP-Gesamtgrenze** – Schiedsrichter-Aufgabe, zu viele externe Faktoren
- **Bestanden/Nicht bestanden gesamt** – weitere Kriterien (Ausrüstung, Verhalten) außerhalb der App
- **Spezifische Testfragen-Inhalte** – nur Zeiterfassung, kein Fragebogen-Inhalt
- **Sonderfall RW/GW (Hilfeleistung)** – andere Funktionszuordnungen, aber gleiche Höchstzeiten
- **Gefahrenmatrix Stufe 6** – Bewertung durch Schiedsrichter
