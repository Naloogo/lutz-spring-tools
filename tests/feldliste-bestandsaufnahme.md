# Feldliste · bestandsaufnahme.html (Bestandsaufnahme Lager)

Stand: 2026-11-10 · Erstellt von: Claude Code (Sitzung Umbau bestandsaufnahme) · Geprüft von Fabian: ☑ 2026-11-10 („passt“); die Vorschläge zu den offenen Fragen 1–10 gelten damit als angenommen (06)
Zweck: Vor dem Neubau nach DESIGNREGELN.md 6.1 jedes Feld der Altfassung befragen. Erst nach Freigabe bauen.
Alle Beispielwerte erfunden, keine Kundendaten. Altfassung: `alt/bestandsaufnahme_2026-11-10.html` (byteidentisch mit dem Stand im Git, Commit f754073; SHA-256 `206da856…02b8dd`).

## Fragen je Feld
1. Geht der Wert in die Rechnung ein? Wenn nein: Ergebnis oder weg.
2. Hat er einen Standardwert aus dem Firmengehirn? Dann vorbelegen.
3. Ändert ihn der Nutzer bei jedem Durchlauf? Wenn nein: „Erweitert“.
4. Folgt er aus einer anderen Eingabe? Dann berechnen und anzeigen.
5. Gehört er zu einer Gruppe? Dann gemeinsame Karte oder Zeile.

## Was die Altfassung tut (bleibt unverändert)
Das Tool rechnet nichts; sein „Rechenteil“ ist das **Dateiformat `lager.json`** und die Regeln, nach denen Positionen entstehen. Beides ist die Schnittstelle zu `heizkoerper.html` (Lagerabgleich `imLager()`, TOOLS-WISSEN.md) und wird unverändert übernommen:
- **Baureihen und Typen** (Zeilen 195–200, `HERSTELLER_LISTE`): `kermi` Kermi therm-x2 Typ 11/22/33 (mit Ausführung K/V und Bauform) · `verteo` Kermi Verteo-Profil Typ 10/20/21/22 · `arbonia` Arbonia Röhren 2- bis 6-Säuler · `zehnder` Zehnder Charleston 2- bis 6-Säuler. Identisch zum Auslegungstool.
- **Positionsobjekt** (Zeilen 236–257, `leseFormular`): `{ hersteller, typ, bh, bl, stueck }`, dazu nur bei Kermi therm-x2 `v` („K“/„V“, **Pflicht**, sonst Fehlertext) und `bauform` („profil“/„plan“), nur bei Ventil `anschluss` („rechts“/„links“), `notiz` nur wenn ausgefüllt. Ganzzahlen (`parseInt`). Fehlertexte: Bauhöhe und Baulänge fehlen · Stückzahl < 1 · Ausführung fehlt.
- **Zusammenführen** (Zeilen 258–270): Neue Position mit gleichem Hersteller, Typ, BH, BL, Ausführung, Bauform und Anschlussseite → Stück wird auf die vorhandene Position addiert, eine neue Notiz ersetzt die alte. Im Bearbeiten-Modus wird die Position überschrieben (ohne Zusammenführen).
- **Laden** (Zeilen 339–359): akzeptiert `{ bestand: [...] }` oder ein nacktes Array; Einträge ohne `hersteller`, `typ`, `bh`, `bl` werden verworfen; Zahlen werden gewandelt, `stueck` ohne Wert = 1; `v`, `bauform`, `anschluss`, `notiz` nur, wenn vorhanden; `stand` aus der Datei setzt das Datumsfeld. Fehler: „Datei konnte nicht gelesen werden: …“.
- **Datei** (Zeilen 361–378): `{ _hinweis, stand, bestand }`, `JSON.stringify(…, null, 2)`, Dateiname fest **`lager.json`** (so lädt `heizkoerper.html` die Datei über die Adresse), `stand` = Datumsfeld oder heute. Der `_hinweis`-Text bleibt wörtlich (Dokumentation des Formats in der Datei selbst). Leere Liste: Hinweis, keine Datei.
- **Zähler** (Zeile 308): „n Positionen · m Stück“.
- Datenquellen: keine eingebetteten Tabellen, kein `fetch`; die Datei kommt nur über die Dateiauswahl. Kein Browser-Speicher (ein Neuladen der Seite verwirft die Liste). Keine Druckausgabe. Nicht in `04_tool-konventionen.md` als Erkennungsmerkmal beschrieben (dort nur Zweck, Ablauf, 5er-Raster, Stand 2026-08-14).
- Befund Schritt 1 gegen 04: 04 nennt „10 Positionen, Lagerstand 2026-08-12“; die `lager.json` im Repo hat 14 Positionen (14 Stück), Stand 2026-08-12. Grundlage ist der Git-Stand; 04 wird in Schritt 8 nachgezogen.
- Befund gegen `heizkoerper.html`: Der `_hinweis` in der Datei sagt „Pflicht bei Kermi/Verteo: v“. `heizkoerper.html` vergleicht `v` nur, wenn es selbst eine Ausführung sucht (`ausfVarianten`, nur bei Baureihen mit `hatAusf`, also Kermi therm-x2); bei Verteo ist `v` leer und wird nicht geprüft. Die Altfassung fragt die Ausführung deshalb zu Recht nur bei Kermi therm-x2 ab. Der Hinweistext ist ungenau, bleibt aber im Umbau wörtlich (offene Frage 9).

## Felder

| Nr. | Feld (alt) | id (alt) | Einheit | Standard (alt) | Bereich (alt) | Pflicht | Herkunft des Werts | Gruppe (neu) | Standard / Erweitert / Ergebnis / entfällt | Feld (neu) | id (neu) | Begründung |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | lager.json laden | ladeFile (`type="file"`) | – | – | `.json` | nein | vorhandene `lager.json` (aus dem Repo heruntergeladen oder zuletzt gesichert) | Lagerbestand (Karte 1) | Standard | Lagerbestand laden (Datei) | lagerDatei | Sekundärknopf als `<label for>` wie in `heizkoerper.html`; `LS.datei.laden` mit demselben Normalisieren wie alt; Rückmeldung `LS.toast('… Positionen geladen', 'ok')`. Ersetzt eine vorhandene Liste wie alt (offene Frage 2: Rückfrage, wenn die Liste nicht leer ist) |
| 2 | Stand (Datum) | stand (`type="date"`) | Datum | heute | – | nein | Tag der Aufnahme; beim Laden aus der Datei | Lagerbestand (Karte 1) | Standard | Stand der Aufnahme | stand | bleibt Datumsfeld; wird in die Datei geschrieben (`stand`) und in `heizkoerper.html` in der Statuszeile und im Bericht angezeigt. Hinweis „Steht in der Datei und im Auslegungstool (Lagerstand)“. Leer → heute (wie alt) |
| 3 | Hersteller / Baureihe | hersteller (`<select>`, 4 Einträge) | – | kermi | kermi / verteo / arbonia / zehnder | ja | Betrieb (Kermi bevorzugt, 01) | Position (Karte 2) | Standard | Hersteller / Baureihe | hersteller | `.ls-seg` mit vier Einträgen „Kermi therm-x2 / Kermi Verteo / Arbonia / Zehnder“ wie in `heizkoerper.html` (gleiche Beschriftung über alle Tools). Wechsel baut die Typen neu auf und blendet Ausführung/Bauform/Anschlussseite ein oder aus (wie alt) |
| 4 | Typ | typ (`<select>`, je Hersteller) | – | erster Eintrag (Kermi: Typ 11) | Typen des Herstellers (3–5) | ja | Typenschild des Heizkörpers | Position | Standard | Typ | typ | Vorschlag `.ls-seg` mit kurzen Beschriftungen („Typ 11 / 22 / 33“, „Verteo 10 / 20 / 21 / 22“, „2 / 3 / 4 / 5 / 6-Säuler“); Alternative `.ls-select` (offene Frage 3). Werte intern unverändert (`'11'`, `'2'` …) |
| 5 | Ausführung | ausf (`<select>` – / K / V) | – | **leer** („–“) | K / V | **ja** (nur Kermi therm-x2) | Anschlusstechnik (TOOLS-WISSEN: K und V sind verschiedene Artikel; fehlt `v`, findet `heizkoerper.html` die Position nicht) | Position | Standard (nur Kermi therm-x2) | Ausführung | ausfuehrung | `.ls-seg` „Kompakt (K) / Ventil (V)“ **ohne Vorauswahl**; ohne Wahl Fehlertext „Ausführung wählen: K (seitlich) oder V (Ventil, Mittelanschluss)“ mit Fokus. Bewusst keine Vorbelegung, damit niemand aus Versehen K speichert (offene Frage 4). Werte `K`/`V` wie alt |
| 6 | Bauform | bauform (`<select>` Profil / Plan) | – | profil | profil / plan | ja (nur Kermi therm-x2) | Typenschild / Sichtprüfung | Position | Standard (nur Kermi therm-x2) | Bauform | bauform | `.ls-seg` „Profil / Plan“; Hinweis bei Plan: „Plan-Maße enden auf 5 (605 × 1005)“ (06, 2026-08-14). Werte wie alt |
| 7 | Ventil-Anschluss | anschluss (`<select>` Rechts / Links) | – | rechts | rechts / links | ja (nur Kermi therm-x2 mit V) | Anschlusslage am Heizkörper | Position | Standard (nur bei Ventil) | Anschlussseite | anschluss | `.ls-seg` „rechts / links“, Beschriftung wie in `heizkoerper.html`; sichtbar nur bei Ventil (wie alt). Nur bei V in die Datei geschrieben (TOOLS-WISSEN: Seite gibt es nur beim Ventil) |
| 8 | Bauhöhe (mm) | bh (`type="number"`, step 10) | mm | leer | > 0 | ja | Maß am Heizkörper (Nennmaß, nicht Einbaumaß) | Position, Zeile „Bauhöhe × Baulänge“ | Standard | Bauhöhe | bh | `type="text" inputmode="decimal"`, Einheit mm als Suffix; Prüfung wie alt (fehlt oder 0 → Fehlertext „Bauhöhe in mm eintragen, z. B. 600“), Wert als Ganzzahl. Zusätzlich **Plan-Raster**: bei Bauform Plan Fehlertext, wenn BH oder BL nicht auf 5 endet (06, 2026-08-14 „Plan-Lagerbestand im 5er-Raster, sonst greift der Lagerabgleich nicht“; UMBAU-ABLAUF Besonderheit). Neue Prüfung, offene Frage 5 |
| 9 | Baulänge (mm) | bl (`type="number"`, step 10) | mm | leer | > 0 | ja | wie 8 | Position, Zeile „Bauhöhe × Baulänge“ | Standard | Baulänge | bl | wie 8; Fehlertext „Baulänge in mm eintragen, z. B. 1000“ |
| 10 | Stückzahl | stueck (`type="number"`, min 1) | Stück | 1 | ≥ 1 | ja | Zählung im Lager | Position | Standard | Stück | stueck | `.ls-stepper` (Regel 7: Stückzahlen nie als freies Textfeld), min 1, Standard 1; Eingabe < 1 → Fehlertext wie alt |
| 11 | Notiz (optional, z. B. Lagerplatz) | notiz (`type="text"`) | Text | leer | – | nein | Lagerplatz, Zustand | Position | Standard | Notiz | notiz | Platzhalter „z. B. Regal A3“ bleibt (kein Personenbezug); Hinweis „Lagerplatz oder Zustand, keine Kunden- oder Bauvorhabennamen“ (Repo öffentlich). Wird in `heizkoerper.html` als „Lagerplatz“ im Bericht gezeigt |
| 12 | + Hinzufügen / ✓ Speichern | addBtn | – | – | – | – | – | Aktionen der Karte Position | Standard | + Position speichern | btnSpeichern | Primärknopf (Vokabular Abschnitt 8). Im Bearbeiten-Modus überschreibt er die markierte Position (wie „+ Raum speichern“ in `heizkoerper.html`); Beschriftung bleibt gleich, die Hinweiszeile sagt, welche Position bearbeitet wird. Rückmeldungen `LS.toast`: „Gespeichert“, „Vorhandene Position ergänzt (Stück addiert)“, „Position aktualisiert“ |
| 13 | Bearbeitung abbrechen | cancelEdit | – | – | – | – | – | Aktionen der Karte Position | Standard (nur im Bearbeiten) | Abbrechen | btnAbbrechen | Sekundärknopf, nur sichtbar im Bearbeiten-Modus (wie alt) |
| 14 | Hinweis „Du bearbeitest eine vorhandene Position“ | editnote | – | – | – | – | – | Karte Position | Ergebnis (Hinweis) | `.ls-note--info` „Du bearbeitest Position n (Kermi therm-x2 Typ 22 K 600 × 1000). ‚+ Position speichern‘ übernimmt die Änderung.“ | hinweisBearbeiten | wie alt, nur Baustein des Designsystems; die bearbeitete Position ist in der Liste markiert (`.ls-item.is-selected`) |
| 15 | Zähler „n Positionen · m Stück“ | count | – | – | – | – | berechnet aus der Liste | Lagerbestand (Karte 3) | Ergebnis | Kopfzeile der Karte „Lagerbestand“: „14 Positionen · 14 Stück“ und `.ls-stand` „Stand 2026-08-12“ | zaehler | gleicher Text wie alt; Stand sichtbar, weil er in die Datei geht |
| 16 | Tabelle (Hersteller, Typ, Ausf., BH, BL, Stück, Notiz, Ändern/Löschen) | tbody | – | – | – | – | Liste | Lagerbestand (Karte 3) | Standard | Positionen als `.ls-items` | positionenListe | Je Position eine Zeile: Badge Hersteller (neutral, kein eigener Farbton je Hersteller; Regel 2: Zustandsfarben sind keine Herstellerfarben), Typ und Bauform, Ausführung K/V mit Seite nur bei V, „600 × 1000 mm“, **Stepper für Stück direkt in der Zeile** (UMBAU-ABLAUF: „Stepper je Artikel“; ändert `stueck` der Position sofort, min 1), Notiz, Aktionen „Bearbeiten“, „Löschen“. Auf dem Handy einspaltig. Reihenfolge = Reihenfolge der Datei (wie alt; `heizkoerper.html` nennt Lager-Gründe in dieser Reihenfolge) |
| 17 | Ändern (je Zeile) | `[data-edit]` | – | – | – | – | – | Liste | Standard | Bearbeiten | – | Vokabular Abschnitt 8; lädt die Position ins Formular, scrollt zur Karte Position (wie alt) |
| 18 | Löschen (je Zeile) | `[data-del]` | – | – | – | – | – | Liste | Standard | Löschen | – | alt löscht **ohne Rückfrage** → neu `LS.rueckfrage` inline (Regel 7); Rückmeldung „Position gelöscht“ |
| 19 | Alle löschen | clearBtn (`confirm()`) | – | – | – | – | – | Toolbar der Karte Lagerbestand | Standard | Alle löschen | btnAlleLoeschen | Sekundärknopf mit `LS.rueckfrage` („Alle n Positionen löschen?“) statt `confirm()`; leert Liste und Browser-Speicher, Stand bleibt. Kein Markenknopf: „Neues Projekt“ gibt es hier nicht (offene Frage 6) |
| 20 | ⤓ lager.json herunterladen | downloadBtn | – | – | – | – | – | Toolbar der Karte Lagerbestand | Standard | lager.json sichern (Datei) | btnSichern | **Primärknopf** des Tools (das Ergebnis ist die Datei). `LS.datei.sichern('lager.json', daten)` mit **unverändertem Inhalt** (`_hinweis`, `stand`, `bestand`, Einrückung 2). Leere Liste → Hinweis, keine Datei (wie alt). Rückmeldung „lager.json erstellt“ |
| 21 | Infobox („Mit diesem Tool pflegst du …“) | .infobox | – | – | – | – | – | Kopf | → „So geht’s“ | `.ls-help` mit Ablauf: 1. Lagerbestand laden (Datei) oder leer beginnen · 2. Positionen erfassen · 3. „lager.json sichern (Datei)“ · 4. Datei im Repo neben `heizkoerper.html` ersetzen (GitHub) · iPad-Hinweis (Teilen-Blatt, Safari statt Vorschau) | – | Regel 6: Tools mit Dateifunktionen zeigen den iPad-Hinweis; Text ohne Emoji |
| 22 | Toast | #toast | – | – | – | – | – | – | → `LS.toast` | – | Baustein des Designsystems |
| 23 | Kopf (Base64-Logo, „Bestandsaufnahme“) | header | – | – | – | – | – | Kopf | → Vorlage | Logo B, Toolname „Bestandsaufnahme“, Theme-Schalter | – | Vorlage; Kunde/Projekt und Datum der Vorlage entfallen (kein Projekt, offene Frage 7); im Druck Kopfzeile „Lagerbestand · Stand …“ erst mit Erweiterung 1 |
| 24 | Fuß („Bestandsaufnahme · Lutz Spring GmbH · erzeugt lager.json …“) | .footer | – | – | – | – | – | Fuß | → Vorlage | `.ls-footer`: „Lutz Spring GmbH · Heizung · Sanitär · Wärmepumpen · Bestandsaufnahme erzeugt `lager.json` für die Heizkörper-Auslegung · Lagerstand ist ein Vorschlag, kein Nachweis · Stand JJJJ-MM-TT“ | – | Slogan laut CLAUDE.md |
| 25 | Haus-Icon zur Startseite | .zurueck | – | – | – | – | – | Fuß | → Vorlage | `.ls-home` | – | Vorlage |
| 26 | – (nicht vorhanden) | – | – | – | – | – | – | Browser-Speicher | **neu (Regel 6)** | Liste und Stand bleiben im Browser (`LS.speicher`, Schlüssel `bestandsaufnahme-lager`) | – | Regel 6 [BESTÄTIGT: bisherige Praxis]: Eingaben überleben das Verlassen der Seite. In der Altfassung geht die Liste beim Neuladen verloren, auf dem iPad ein echtes Risiko. Ohne Rechenwirkung; „Alle löschen“ leert den Speicher. Offene Frage 1 |

## Reihenfolge der Gruppen (neu)
1. **Kopf** (Vorlage) und **So geht’s** (`.ls-help`, zu).
2. **Karte „Lagerbestand“ (Datei)**: „Lagerbestand laden (Datei)“ · „Stand der Aufnahme“ (Datum). Hinweis: Die Datei lädt nur über die Dateiauswahl, nicht automatisch (Erweiterung 2).
3. **Karte „Position“**: Hersteller / Baureihe (`.ls-seg`) · Typ · Ausführung (nur therm-x2) · Bauform (nur therm-x2) · Anschlussseite (nur Ventil) · Zeile „Bauhöhe × Baulänge“ (mm) · Stück (`.ls-stepper`) · Notiz · Primärknopf „+ Position speichern“, „Abbrechen“ im Bearbeiten-Modus · Hinweiszeile beim Bearbeiten.
4. **Karte „Lagerbestand“ (Liste)**: Kopfzeile „n Positionen · m Stück“ mit `.ls-stand` Stand · Positionen als `.ls-items` mit Stepper je Artikel, „Bearbeiten“, „Löschen“ (Rückfrage) · Leerzustand `.ls-empty` („Noch keine Heizkörper erfasst. Lagerbestand laden (Datei) oder oben eine Position anlegen.“) · Toolbar: **„lager.json sichern (Datei)“ (primär)**, „Alle löschen“ (Rückfrage).
5. **Fuß** (Vorlage).
Kein Bereich „Erweitert“: alle Felder werden je Position gebraucht; selten geänderte Felder (Bauform, Anschlussseite) sind ohnehin nur sichtbar, wenn die Baureihe sie hat.

## Ergebnis (Abschnitt 7 DESIGNREGELN)
Das Ergebnis ist die Liste selbst und die Datei. Keine Kennzahlen außer dem Zähler, keine Empfehlungskarte, kein Rechenweg (nichts wird gerechnet). Badges: Hersteller neutral; Ausführung als Text „K“/„V rechts“. Kein Druck im Umbau (Erweiterung 1).

## Zuordnung alt → neu für Projektdateien (FELD_MAPPING)
Entfällt. Die einzige Datei ist `lager.json`; ihr Format bleibt unverändert (`_hinweis`, `stand`, `bestand` mit `hersteller`, `typ`, `bh`, `bl`, `stueck`, optional `v`, `bauform`, `anschluss`, `notiz`). Alte und neue Fassung lesen und schreiben dieselbe Datei; `heizkoerper.html` liest sie unverändert.
```js
var FELD_MAPPING = {}; /* lager.json: Format unverändert, keine Zuordnung nötig */
```

## Entfallene oder zu Ergebnissen gewordene Felder
| Feld (alt) | Was stattdessen | Begründung |
|---|---|---|
| Infobox | „So geht’s“ (`.ls-help`) | Baustein der Vorlage |
| Hersteller-Farbbadges (`.mfr-kermi` …) | neutraler Badge | Regel 2: keine Farben außerhalb des Designsystems; die Farbe trug keine Information, die der Text nicht hat |
| `confirm()` bei „Alle löschen“, Löschen ohne Rückfrage | `LS.rueckfrage` inline | Regel 7 |
| „✓ Speichern“ als zweite Beschriftung des Hinzufügen-Knopfs | ein Knopf „+ Position speichern“, Hinweiszeile nennt die bearbeitete Position | ein Knopf, ein Zweck; wie in `heizkoerper.html` |
| Kunde / Projekt (Vorlage) | entfällt | das Tool hat kein Projekt; die Datei heißt immer `lager.json` |

## Offene Fragen an Fabian
1. **Browser-Speicher** (Feld 26): Liste und Stand im Browser merken, bis „Alle löschen“? Vorschlag: ja (Regel 6; verhindert Datenverlust beim Neuladen auf dem iPad). Beim Laden einer Datei ersetzt die Datei den gemerkten Stand.
2. **Laden über eine volle Liste** (Feld 1): Die Altfassung ersetzt die Liste ohne Rückfrage. Vorschlag: `LS.rueckfrage` „n vorhandene Positionen durch die Datei ersetzen?“, wenn die Liste nicht leer ist.
3. **Typ als Segment oder Aufklappliste** (Feld 4): Vorschlag `.ls-seg` (3–5 kurze Einträge, ein Tipp statt zwei); bei Arbonia/Zehnder fünf Einträge „2 / 3 / 4 / 5 / 6-Säuler“. Falls das auf dem Handy zu eng wird (BACKLOG-Thema `.ls-seg--full`), `.ls-select`.
4. **Ausführung ohne Vorauswahl** (Feld 5): Vorschlag: kein Standard, Fehlertext bei fehlender Wahl (wie alt). Alternative: „K“ vorbelegen (schneller, aber K/V-Fehler aus TOOLS-WISSEN wahrscheinlicher).
5. **Plan-Raster prüfen** (Felder 8/9, neu): Bei Bauform Plan Fehlertext, wenn Bauhöhe oder Baulänge nicht auf 5 endet („Plan-Maße enden auf 5, z. B. 605 × 1005; sonst findet die Heizkörper-Auslegung die Position nicht“)? Vorschlag: ja, blockierend. Alternative: nur Hinweis, Speichern trotzdem möglich. Ohne Prüfung bleibt es wie alt.
6. **„Alle löschen“** (Feld 19): Sekundärknopf mit Rückfrage, kein Markenknopf (2.4: Markenknopf nur für „Neues Projekt“)? Vorschlag: ja.
7. **Kunde / Projekt der Vorlage** (Feld 23): entfällt, weil die Datei immer `lager.json` heißt? Vorschlag: ja.
8. **Stepper in der Liste** (Feld 16): Ändert die Stückzahl der Position sofort (ohne Bearbeiten-Modus), min 1; Position auf 0 → stattdessen „Löschen“? Vorschlag: ja, min 1. Alternative: Stepper bis 0 erlaubt und 0 löscht die Position nach Rückfrage.
9. **`_hinweis`-Text in der Datei**: bleibt wörtlich (sagt „Pflicht bei Kermi/Verteo: v“, tatsächlich nur bei Kermi therm-x2). Vorschlag: im Umbau unverändert lassen (Teil des Dateiformats, byteidentischer Nachweis B3); Korrektur später als eigener Punkt, dann auch `lager.json` im Repo neu erzeugen.
10. **Bereichsprüfung Maße** (Felder 8/9): Die Altfassung prüft nur „> 0“. Eine Plausibilitätsgrenze (z. B. 100–3 000 mm) wäre neu und ohne Quelle; Vorschlag: wie alt nur „> 0“ prüfen, keine erfundenen Grenzen.

## Erweiterungen nach der Freigabe (Schritt 10, Vorschläge, nicht Teil des Umbaus)
1. **Bericht „Lagerbestand“ drucken / PDF**: Tabelle nach Berichtsvorlage (Hersteller, Typ, Bauform, Ausführung, Seite, BH × BL, Stück, Notiz), Summenzeile, Stand im Kopf; Knopf „Bericht drucken / PDF“ als zweiter Primärknopf oder statt „lager.json sichern“ als Primär (dann Datei sekundär). Ohne Rechenwirkung.
2. **Aktuellen Lagerbestand beim Start laden**: `LS.datei.extern('lager.json')` über die echte Adresse wie in `heizkoerper.html`, mit Statuszeile „Lagerbestand geladen: 14 Positionen · Stand 2026-08-12“; Vorrang vor dem Browser-Speicher nur nach Rückfrage, wenn dort ungesicherte Änderungen liegen. Spart den Umweg über den Download aus GitHub.
3. **Duplizieren** je Position (wie in `heizkoerper.html`), z. B. gleicher Heizkörper mit anderer Anschlussseite.
4. **Dateiname mit Datum** (`lager_2026-11-10.json`) als zweite Sicherung neben `lager.json`? Nur sinnvoll, wenn der Ablauf „ins Repo hochladen“ den festen Namen behält.
