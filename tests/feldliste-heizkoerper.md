# Feldliste · heizkoerper.html (Heizkörper-Auslegung)

Stand: 2026-11-08 · Erstellt von: Claude Code (Sitzung Umbau heizkoerper) · Geprüft von Fabian: ☑ 2026-11-08 („passt“); die Vorschläge zu den offenen Fragen 1–14 gelten damit als angenommen
Zweck: Vor dem Neubau nach DESIGNREGELN.md 6.1 jedes Feld der Altfassung befragen. Erst nach Freigabe bauen.
Alle Beispielwerte erfunden, keine Kundendaten. Altfassung: `alt/heizkoerper_2026-11-08.html` (byteidentisch mit dem Stand im Git, Commit f754073; SHA-256 `4c5e7894…047d7`).

## Fragen je Feld
1. Geht der Wert in die Rechnung ein? Wenn nein: Ergebnis oder weg.
2. Hat er einen Standardwert aus dem Firmengehirn? Dann vorbelegen.
3. Ändert ihn der Nutzer bei jedem Durchlauf? Wenn nein: „Erweitert“.
4. Folgt er aus einer anderen Eingabe? Dann berechnen und anzeigen.
5. Gehört er zu einer Gruppe? Dann gemeinsame Karte oder Zeile.

## Was die Altfassung rechnet (bleibt unverändert)
Rechenteil = Zeilen 509–1189 (Datentabellen, `DB()`, `dTlog`, `leistung`), 1353–1449 (`runFit`, Lagerabgleich mit Begründungen, Sortierung), 1836–1882 (Multi: `korbSummeLeistung`, `korbNoetigeVL`, `korbErreichbareRaumtemp`), 1899–1960 (`runTemp`, `requiredVL`), 2085–2139 (`erreichbareRaumtemp`, `runRoom`), 2434–2447 (`erzeugeUntertempHinweis`), 2449–2564 (`speichereRaum`: Raumobjekt mit QN, Konsole, nötiger VL, erreichbarer Raumtemperatur), 2583–2658 (Artikelstamm), 2673–2929 (GAEB X83 inkl. `konsoleAddieren`), 2989–3266 (Bericht mit Bestellübersicht), 3278–3313 (Projektdatei). Alles davon wird unverändert übernommen; nur die Verbindung zu den Feldern (ids) ändert sich.
- Leistung: Φ = Φ<sub>Norm</sub> × (ΔT<sub>log</sub> / 49,83 K)<sup>n</sup>, ΔT<sub>log</sub> aus VL/RL/t<sub>L</sub> (EN 442). Normwerte 75/65/20: Kermi therm-x2 Profil (`sl` W/m × BL), Plan (auf 50/40 zurückgerechnete Normwerte, Maße enden auf 5), Verteo-Profil/-Plan (Wattmatrix BH × BL), Arbonia/Zehnder (W je Glied × Glieder, 45 bzw. 46 mm).
- Tab ① „Passende Heizkörper“: alle Baugrößen mit BH ≤ max. BH + 20 mm und BL ≤ max. BL + 20 mm (`FLAECHEN_TOLERANZ`), die Heizlast × (1 + Reserve) allein decken; Deckung bezogen auf die reine Heizlast; „knapp“ unter 110 %; Sortierung Lagertreffer → Bestandstyp → Bautiefe nahe Bestand (20-mm-Stufen) → kleinste Fläche → knappste Deckung; Empfehlung = erste Zeile; max. 40 Zeilen. Multi-Modus: alle, die in die Fläche passen, stärkste zuerst.
- Lagerabgleich `imLager()`: Hersteller, Typ, BH, BL, Stück > 0, Bauform (Kermi), Ausführung K/V (Pflicht; fehlt `v`, kein Treffer), bei Ventil Anschlussseite. Jede nicht nutzbare Lagerposition nennt ihren Grund (Fläche, Bauform, Ausführung, Anschlussseite, Maß fehlt in Tabelle, Heizlast). Regeln aus TOOLS-WISSEN.md gelten unverändert.
- Tab ② „Nötige Systemtemperatur“: je Baugröße die VL, bei der Bedarf erreicht wird, Spreizung = VL − RL fest; Bisektion 60 Schritte; „WP-tauglich“ ≤ 55 °C, „hoch“ ≤ 75 °C; max. 120 Zeilen. Multi: gemeinsame VL für die Summe.
- Tab ③ „Erreichbare Raumtemperatur“: je Typ und Bauhöhe der größte HK mit BL ≤ max. BL (hier ohne 20-mm-Toleranz); Gleichgewicht aus HK-Abgabe und Heizlast, die linear mit (t<sub>L</sub> − t<sub>a</sub>) skaliert; Bisektion 80 Schritte; „Wunsch erreicht“ ≥ t<sub>L</sub> − 0,05 K, „knapp“ ≥ t<sub>L</sub> − 1 K. Beim Speichern in Tab ③ mit Unterschreitung > 0,5 K wird der Kundenhinweis (Sie-Form) in die Raumnotiz geschrieben.
- Raum speichern: Raumobjekt mit `hk` (oder `hkliste` im Multi-Modus), `konsole`, `noetigeVL`, `erreichbareTemp`, `ggLeistung`, Kennzeichen `istBestand`, `ausLager`, `anschluss` (nur Ventil). Gleicher Name und gleiche Etage überschreibt; Undo (Strg+Z, 30 Schritte).
- Konsolen (eine Stelle `konsoleAddieren`): therm-x2 Wand = Schnellmontageset nach Nenn-Bauhöhe (605 → 600), 3. Konsole ab BL 1800; Standkonsole ZB01380001 (BH 300–554) / ZB01380002 (BH 600–954), je Konsole eine Blende ZB00290001, Typ 11 Ventil zusätzlich ZB01450001; Verteo-Set nach Bauhöhe. Bestands-HK ohne Halterbedarf: `konsole = null`.
- Bestellnummern: FK0/FTV (Profil), PK0/PTV (Plan), FSN (Verteo-Profil), PSN (Verteo-Plan), Arbonia Modell + Glieder, Zehnder Charleston `3060-20`; dm mit `Math.floor`.
- GAEB X83: Pos. 1 Heizkörper je Raum, Pos. 2 Installationsmaterial summiert, Pos. 3 Arbeits- und Serviceleistungen (Arbeitszeit nur bei gesetzter Montagezeit); Langtext-Reihenfolge laut TOOLS-WISSEN.md; Artikelstamm aus `localStorage` (DATANORM, nie im Repo).
- Datenquellen: Tabellen im Skript (Kermi Fachinformation 2018, Kermi-Leistungsrechner 50/40 für Plan und Verteo, Arbonia 2017, Zehnder Charleston), `lager.json` per `fetch` (nur über echte Adresse), Artikeldaten per Datei.
- Dateien: Browser-Speicher `lutzspring_hk_auslegung_v1` (Kunde, Raumname, Etage, Hersteller, Bauform, Eingaben, Räume); Projektdatei `Heizkoerper_<Kunde>.json` (`version: 1`, `kunde`, `raeume`); GAEB `<Kunde>.X83`; Druck über `window.print()` mit `#printArea` (Bericht je Raum, Bestellübersicht: zu bestellen, aus Lager, Konsolen, Bestand). Dokumenttitel = PDF-Dateiname `Heizkoerper-Auslegung_<Kunde>`.
- Befund Schritt 1: `04_tool-konventionen.md` nennt Stand 2026-08-14 (Erkennungsmerkmal Verteo-Plan, Standkonsolen, „aus Lager“). Die Datei im Git ist die Fassung 09/2026 aus `Auslegung-WP` (zusätzlich GAEB-Export, Artikeldaten, leere Flächenfelder = unbegrenzt, Bestandskonsolen, Lagerabgleich mit K/V), wie TOOLS-WISSEN.md beschreibt. Grundlage des Umbaus ist der Git-Stand (CLAUDE.md); 04 wird in Schritt 8 nachgezogen.

## Felder

| Nr. | Feld (alt) | id (alt) | Einheit | Standard (alt) | Bereich (alt) | Pflicht | Herkunft des Werts | Gruppe (neu) | Standard / Erweitert / Ergebnis / entfällt | Feld (neu) | id (neu) | Begründung |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Kunde / Projekt | kunde | Text | leer | – | nein | Nutzer; ohne Personendaten („BV Muster“, Ort + Gewerk) | Projekt | Standard | Kunde / Projekt | projekt | Platzhalter „z. B. Fam. Müller, Musterstr. 1“ entfällt (Datenschutz) → „BV Muster“; steht im Kopf, Bericht, Datei- und GAEB-Namen |
| 2 | Aktueller Raum | raumname | Text | leer | – | ja (zum Speichern) | Nutzer | Raum | Standard | Aktueller Raum | raumName | Pflicht erst beim Speichern; Fehlertext statt `alert()` |
| 3 | Etage (optional) | Buttons `.etage-btn` KG/EG/1. OG/2. OG/3. OG/DG, Toggle | – | keine | 6 Werte oder keine | nein | Nutzer | Raum | Standard | Etage | etage | Sechs Optionen plus „keine“ → `.ls-select` (Regel 6: 2–4 Optionen als `.ls-seg`), offene Frage 9. Gruppiert den Bericht (feste Reihenfolge KG … DG, „Ohne Etagenangabe“ zuletzt); Name + Etage ist der Schlüssel beim Überschreiben |
| 4 | Hersteller / Baureihe | `.mfr-tab` kermi / verteo / arbonia / zehnder | – | kermi | 4 Werte | ja | Betrieb (Kermi bevorzugt, 01) | Heizkörper | Standard | Hersteller / Baureihe | hersteller | `.ls-seg` mit vier kurzen Einträgen („Kermi therm-x2“, „Kermi Verteo“, „Arbonia“, „Zehnder“). **Verhalten bleibt:** Wechsel auf Verteo hebt max. BH auf ≥ 2400 und max. BL auf ≥ 800, Wechsel auf Arbonia/Zehnder hebt max. BH < 900 auf 1800 (Zeilen 1338–1346); baut Typenliste, Bestand-Auswahl und Methodenhinweis neu |
| 5 | Heizlast des Raumes Φ<sub>HL</sub> | heizlast | W | 1200 | ab 50, Schritt 10 | ja | Heizlastberechnung (Heizreport, Raumwert) | Bedarf | Standard | Heizlast Φ<sub>HL</sub> | heizlast | `type="text" inputmode="decimal"`, Prüfbereich 50–50 000 mit Fehlertext; Hinweis „Raumwert aus dem Heizlastbericht“ |
| 6 | Sicherheitsreserve | reserve | % | **15** | 0–50 | nein | Einzelfall; **kein Standard** (06, 2026-10-18) | Erweitert | Erweitert | Sicherheitsreserve | reserve | Standard **0 %** statt 15 % (offene Frage 1). Kurzzeile „Reserve 0 %“ solange Standard. Rechnung identisch, nur der Vorbelegungswert ändert sich; Referenzfälle setzen die Reserve ausdrücklich |
| 7 | Raum-/Wunschtemperatur t<sub>L</sub> | raumtemp | °C | 20 | 10–30, Schritt 0,5 | ja | Kundenwunsch / Norm-Innentemperatur | Bedarf | Standard | Raumtemperatur t<sub>L</sub> | raumTemp | Vokabular Abschnitt 8 |
| 8 | Schnellwahl Raumtemperatur | `#rtpresets .preset` 16/18/20/21/22/24 | °C | – | 6 Werte | nein | Komfort | Bedarf | → Feld 7 | – | – | Doppelung (6.1 „Ein Feld, ein Zweck“); die Zahl wird direkt getippt. Offene Frage 4 |
| 9 | Norm-Außentemperatur | taussen | °C | **−14** | −20 … 5, Schritt 1 | ja (nur Tab ③ und Hinweistext) | Region Oranienburg **−12,7 °C** (06, 2026-10-18); sonst DIN/TS 12831-1 | Temperaturen | Standard | Norm-Außentemperatur | normAT | Standard −12,7 °C statt −14 °C (offene Frage 2); Schritt 0,1. Geht nur in Tab ③ (erreichbare Raumtemperatur) und in den Kundenhinweis ein. Hinweis „Region Oranienburg −12,7 °C“ |
| 10 | Vorlauf | tv | °C | 50 | frei, Schritt 1 | ja | Auslegung 50/40 °C (06) | Temperaturen | Standard | Systemtemperatur VL | vl | Zeile „Systemtemperatur VL / RL“ mit zwei Feldern; Prüfung VL > RL > t<sub>L</sub> mit Fehlertext (die Altfassung rechnet sonst ΔT<sub>log</sub> = 0) |
| 11 | Rücklauf | tr | °C | 40 | frei, Schritt 1 | ja | wie 10 | Temperaturen | Standard | Systemtemperatur RL | rl | wie 10 |
| 12 | Schnellwahl System | `#presets .preset` 35/28 · 40/30 · 45/35 · 50/40 · 55/45 · 70/55 · 75/65 | °C | 50/40 aktiv | 7 Paare | nein | Praxiswerte | Temperaturen | Standard (verkürzt) oder → Felder 10/11 | Schnellwahl | vlrlSchnellwahl | Vorschlag: `.ls-seg` mit vier Paaren 35/28 · 45/35 · 50/40 · 55/45 (WP-Bereich), höhere Werte frei tippen; oder ganz entfallen (offene Frage 4). Keine Rechenwirkung, setzt nur Felder 10/11 |
| 13 | ΔT<sub>log</sub> | dtlog (disabled) | K | – | – | – | berechnet aus 7, 10, 11 | Ergebnis | Ergebnis | ΔT<sub>log</sub> | – | Zwischenrechnung → Kennzahl in der Auslegungsbasis (6.1 „Doppelungen und Zwischenrechnungen werden Ergebnis“) |
| 14 | Vorhandener Heizkörper (Bestand-Typ) | bestand (select, je Hersteller) | Typ | keiner | Typen des Herstellers | nein | Bestand vor Ort | Erweitert | Erweitert | Vorhandener Heizkörpertyp | bestandTyp | `.ls-select`, Einträge je Hersteller wie alt („Typ 22 (BT 100 mm)“); wirkt nur auf Sortierung und Hinweis. Selten genutzt → Erweitert (offene Frage 6) |
| 15 | Vorhandene Bautiefe (Guss, Auswahl) | gusstiefe (select 70/110/160/220/250) | mm | keine | 5 Werte | nein | Gussradiator DIN 4703 | Erweitert | → Feld 16 | – | – | Doppelung mit Feld 16: freier Wert hat Vorrang (Zeile 1279). Werte werden Hinweistext |
| 16 | Vorhandene Bautiefe (frei) | gusstiefeFrei | mm | leer | 40–300, Schritt 5 | nein | Messung vor Ort | Erweitert | Erweitert | Vorhandene Bautiefe | bautiefeBestand | Ein Zahlenfeld; Hinweis „Guss 2-Säuler 70 · 3-Säuler 110 · 4-Säuler 160 · 5-Säuler 220 · 6-Säuler 250 mm (DIN 4703)“. Wirkt nur auf Sortierung (20-mm-Stufen) und BT-Spalte (offene Frage 5) |
| 17 | Heizkörper pro Raum | `.hkmode-tab` single / multi | – | single | 2 Werte | ja | Arbeitsweise | Heizkörper | Standard | Heizkörper je Raum | hkModus | `.ls-seg` „Ein Heizkörper / Mehrere Heizkörper“; Multi blendet den Korb (`.ls-items` mit `.ls-stepper` für die Anzahl) ein und ändert Trefferregel und Sortierung wie alt |
| 18 | Max. Bauhöhe | maxH | mm | 600 | leer = unbegrenzt; sonst > 0 | nein | Brüstungshöhe aus dem Plan (05) | Einbau | Standard | Max. Bauhöhe | maxBH | Leer bleibt „unbegrenzt“ (Hinweis im Ergebnis wie alt). Standard 600 beibehalten (offene Frage 3); Toleranz + 20 mm bleibt und wird im Hinweis genannt |
| 19 | Max. Baulänge | maxL | mm | 1400 | leer = unbegrenzt | nein | Installationsfläche vor Ort | Einbau | Standard | Max. Baulänge | maxBL | wie 18 |
| 20 | Bauart / Typen | `#typefilter` Checkboxen (je Hersteller) | – | alle an | Typen des Herstellers | ja (mind. einer) | Nutzer | Heizkörper | Standard | Typen | typen | `.ls-check` in einer Zeile, Beschriftung wie alt („Typ 11/22/33“, „Verteo Typ 10/20/21/22“, „2- bis 6-Säuler“); keiner gewählt → Fehlertext |
| 21 | Ausführung | `input[name=ausf]` both / FK0 / FTV | – | both | 3 Werte | ja (nur Kermi therm-x2) | Anschlusstechnik (TOOLS-WISSEN: K und V sind verschiedene Artikel) | Heizkörper | Standard (nur therm-x2) | Ausführung | ausfuehrung | `.ls-seg` „K & V / Kompakt / Ventil“; Werte `both`/`FK0`/`FTV` intern wie alt (gehen in `runFit`, Lagerabgleich, Bestellschlüssel ein) |
| 22 | Bauform | `input[name=bauform]` profil / plan | – | profil | 2 Werte | ja (Kermi therm-x2 und Verteo) | Kundenwunsch | Heizkörper | Standard (nur Kermi) | Bauform | bauform | `.ls-seg` „Profil / Plan“; schaltet Datentabelle, Baulängenraster (…5) und Bestellschlüssel (F/P) |
| 23 | Ventil-Anschluss | `input[name=anschluss]` rechts / links | – | rechts | 2 Werte | ja (nur wenn Ventil möglich) | Anschlusslage vor Ort | Heizkörper | Standard (nur Kermi mit Ventil) | Anschlussseite | anschluss | `.ls-seg` „rechts / links“, sichtbar nur bei Kermi therm-x2 mit K & V oder Ventil (wie alt). Geht in Bestellschlüssel, Lagerabgleich, Artikelstamm ein |
| 24 | Passende Heizkörper suchen | #calc | – | – | – | – | – | Aktionen | Standard | Passende Heizkörper suchen | – | einziger Primärbutton; danach `LS.zumErgebnis` |
| 25 | Zurücksetzen | #clear | – | – | – | – | – | Aktionen | Standard | Zurücksetzen | – | stellt Standardwerte her (neu: Reserve 0, Norm-AT −12,7), Räume bleiben |
| 26 | Ergebnis-Tab | `.mode-tab` fit / temp / room | – | fit | 3 Werte | ja | Arbeitsweise | Ergebnis | Standard | Ansicht | ansicht | `.ls-seg` über dem Ergebnis: „Passende Heizkörper / Nötige Vorlauftemperatur / Erreichbare Raumtemperatur“. **Rechenwirkung:** in Tab ③ gespeicherte Räume erhalten den Kundenhinweis (Zeile 2435); bleibt so |
| 27 | Zeile anklicken (Auswahl) | `tr.clickrow` → `zeigeBezeichnung` | – | Empfehlung | Trefferliste | ja (zum Speichern) | Nutzer | Ergebnis | Standard | Auswahl in `.ls-table--cards` (`tr.is-clickable`, `is-selected`) | – | Auswahlkarte zeigt Modell, Klartext, Bestellnummer als `<code>`, Detail, Leistung |
| 28 | Konsolenart | `.kons-btn` wand / stand | – | wand | 2 Werte (nur Kermi therm-x2) | nein | Montageart vor Ort | Ergebnis (Auswahlkarte) | Standard | Konsole | konsolenArt | `.ls-seg` „Wandkonsole / Standkonsole“; Verteo nur Wand-Set, Arbonia/Zehnder keine Konsolenlogik (wie alt) |
| 29 | aus dem Lager entnehmen | #ausLagerCheck | – | an, wenn Lagertreffer | an/aus | nein | Lagerstand ist Vorschlag (TOOLS-WISSEN) | Ergebnis (Auswahlkarte) | Standard (nur bei Lagertreffer) | aus Lager entnehmen | ausLager | `.ls-check`; aus → Position wandert im Bericht zu „zu bestellen“ |
| 30 | Bestandsheizkörper – bleibt hängen | #bestandCheck | – | aus | an/aus | nein | Kundenentscheidung | Ergebnis (Auswahlkarte) | Standard | Bestandsheizkörper (wird nicht getauscht) | istBestand | `.ls-check`; Badge „Bestand“ in Treffer, Raumliste, Bericht, GAEB (nicht geliefert) |
| 31 | Halter trotzdem nötig | #bestandKonsCheck | – | aus | an/aus (nur bei 30) | nein | Vor Ort | Ergebnis (Auswahlkarte) | Standard (nur bei Bestand) | Konsolen trotzdem einplanen | bestandKonsole | `.ls-check`; steuert `konsole` am Bestands-HK |
| 32 | + zum Raum hinzufügen (Multi) · Menge ± · ✕ | `hkKorbAdd`, `hkKorbMenge`, `hkKorbRemove` | Stück | 1 | ≥ 1 | ja (Multi) | Nutzer | Ergebnis (Korb) | Standard (nur Multi) | Heizkörper in diesem Raum | korb | `.ls-items` mit `.ls-stepper` (Regel 7: Stückzahlen nie als Textfeld), Deckungsbalken → Badge gedeckt/knapp/nicht gedeckt mit Text (Schwellen 100 % / 90 % wie alt) |
| 33 | + Raum speichern | #raumSpeichern | – | – | – | – | – | Ergebnis | Standard | + Raum speichern | – | Vokabular; Rückmeldung `LS.toast('Gespeichert')`; Fehlertexte statt `alert()` |
| 34 | Raumliste: Notiz je Raum | `input.rnotiz` | Text | leer (oder Kundenhinweis aus Tab ③) | – | nein | Nutzer | Gespeicherte Räume | Standard | Notiz | notiz | `.ls-notiz`; erscheint im Bericht und im GAEB-Langtext |
| 35 | Raumliste: Bestand (ganzer Raum) | `input[data-bestand]` | – | aus | an/aus | nein | Kundenentscheidung | Gespeicherte Räume | Standard | Bestand (Raum bleibt) | raumBestand | Badge „Bestand“; Raum wird weder bestellt noch bekommt er Konsolen |
| 36 | Raumliste: Löschen | `button[data-del]` | – | – | – | – | – | Gespeicherte Räume | Standard | Löschen | – | `LS.rueckfrage` statt sofortigem Löschen; Undo bleibt (offene Frage 11) |
| 37 | Neues Projekt | #neuesProjekt | – | – | – | – | – | Projekt | Standard | Neues Projekt | – | `LS.rueckfrage` statt `confirm()`; leert Räume, Kunde, Speicher, Standardwerte |
| 38 | Bericht drucken | #berichtDrucken | – | – | – | – | – | Projekt (Toolbar) | Standard | Bericht drucken / PDF | – | Vokabular; Druck nach `bericht-vorlage.html`, nur Kopf, Bericht, Fuß |
| 39 | Als PDF speichern | #berichtPdf | – | – | – | – | – | Projekt | entfällt | – | – | Ruft dieselbe Druckfunktion auf, nur mit vorgeschaltetem `alert()`; der Hinweis zum Teilen-Blatt steht künftig im „So geht’s“ (offene Frage 8) |
| 40 | Projekt sichern (Datei) | #projektExport | – | – | – | – | – | Projekt (Toolbar) | Standard | Projekt sichern (Datei) | – | `LS.datei` mit `version`, `tool`, `exportiert`, `projekt`, `raeume`; Dateiname `heizkoerper_BV-Muster_JJJJ-MM-TT.json` (`LS.dateiname`) |
| 41 | Projekt laden (Datei) | #projektImportFile | – | – | – | – | – | Projekt (Toolbar) | Standard | Projekt laden (Datei) | – | lädt alte Dateien (`version: 1`, `kunde`, `raeume`) über `FELD_MAPPING`; `LS.rueckfrage` vor dem Ersetzen |
| 42 | GAEB (.X83) für Hero | #gaebExport | – | – | – | – | – | Projekt (Toolbar) | Standard | GAEB (.X83) für HERO | – | Generator unverändert (TOOLS-WISSEN) |
| 43 | Montagezeit je HK | gaebMinuten | min | leer | ≥ 0, Schritt 5 | nein | Betrieb (keine Zeitnorm erfinden) | Erweitert | Erweitert | Montagezeit je Heizkörper (GAEB) | montageMinuten | Leer → keine Position „Arbeitszeit Monteur“ (wie alt). Gehört zum Export, nicht in die Kopfzeile (offene Frage 7); wird im Browser-Speicher mitgeführt (alt nicht) |
| 44 | Artikeldaten laden | #artikelDatei / #artikelLabel | Datei | – | JSON aus DATANORM-Konverter | nein | Großhandel (nie im Repo) | Projekt (Toolbar) | Standard | Artikeldaten laden | – | Status „Artikeldaten: n Artikel“ als `.ls-status`-Zeile oder Kurzzeile; `localStorage`-Schlüssel `lutzspring-artikelstamm` **unverändert**, damit geladene Daten erhalten bleiben |
| 45 | Lagerstatus | #lagerStatus | – | – | aus `lager.json` | – | `lager.json` | Statuszeile | Ergebnis | „Lagerbestand geladen: n Positionen (m Stück) · Stand …“ | – | `.ls-status` (Bauplan 1); ohne Datei ausgeblendet, Tool rechnet weiter |
| 46 | iOS-Vorschau-Warnung | #ioswarn | – | – | – | – | Quick-Look-Erkennung | So geht’s | Standard | iPad-Hinweis in `.ls-help` | – | Text aus Regel 6 (Safari statt Vorschau, Teilen-Blatt → Dateien); Erkennung `checkVorschau` bleibt als `.ls-note--warn` |
| 47 | Methodenhinweis | #methodnote | – | – | je Hersteller | – | Norm/Quelle | Fuß / Rechenweg | Ergebnis | Rechenweg | – | Formel, Eingangswerte, Zwischenergebnisse im Ergebnis (aufklappbar, im Druck sichtbar); Quelle je Hersteller im Fuß |
| 48 | Summary „Auslegungsbasis“ | `.summary` | – | – | – | – | berechnet | Ergebnis | Ergebnis | Kennzahlen `.ls-kpis` | – | Heizlast · Bedarf · Raumtemperatur · System · ΔT<sub>log</sub> · max. Fläche; Hinweis „ohne Begrenzung“ als `.ls-note--warn` vor dem Ergebnis |

Konstanten, die keine Felder werden (bleiben im Code, werden im Rechenweg genannt): Flächentoleranz 20 mm · „knapp“ < 110 % · WP-tauglich ≤ 55 °C, hoch ≤ 75 °C · Wunsch erreicht ≥ t<sub>L</sub> − 0,05 K, knapp ≥ t<sub>L</sub> − 1 K · Kundenhinweis ab 0,5 K Unterschreitung · 3. Konsole ab BL 1800 · Bautiefen-Sortierung in 20-mm-Stufen · Anzeige max. 40 (Tab ①/③) bzw. 120 Zeilen (Tab ②) · Undo 30 Schritte · Etagenreihenfolge KG, EG, 1. OG, 2. OG, 3. OG, DG.

## Reihenfolge der Gruppen (neu)
1. Statuszeile Lagerbestand (nur wenn `lager.json` geladen) · „So geht’s“ (eingeklappt, mit iPad-Hinweis)
2. Kopf (Logo B, Toolname, Kunde/Projekt, Datum, Theme-Schalter)
3. Projekt: Kunde / Projekt · Toolbar „Projekt sichern (Datei)“, „Projekt laden (Datei)“, „Bericht drucken / PDF“, „GAEB (.X83) für HERO“, „Artikeldaten laden“, „Neues Projekt“
4. Gespeicherte Räume (`.ls-items`): Nummer, Etage + Name, Kurzzeile (Heizkörper, Leistung bei VL/RL, Heizlast, ✓/⚠), Badges „Bestand“, „aus Lager“, Notiz, Haken „Bestand (Raum bleibt)“, Löschen mit Rückfrage
5. Raum: Aktueller Raum · Etage · Heizlast Φ<sub>HL</sub> · Raumtemperatur t<sub>L</sub> · Norm-Außentemperatur · Systemtemperatur VL / RL (mit Schnellwahl, offene Frage 4)
6. Einbau und Heizkörper: Max. Bauhöhe · Max. Baulänge · Hersteller / Baureihe · Bauform · Typen · Ausführung · Anschlussseite · Heizkörper je Raum
7. Erweitert (`<details>`, Kurzzeile „Reserve 0 % · kein Bestandstyp · keine Bautiefe · Montagezeit leer“): Sicherheitsreserve · Vorhandener Heizkörpertyp · Vorhandene Bautiefe · Montagezeit je Heizkörper (GAEB)
8. Aktionen: „Passende Heizkörper suchen“ (primär), „Zurücksetzen“
9. Ergebnis: Ansicht (`.ls-seg` ①/②/③) · Kennzahlen · Hinweise, die eine Handlung verlangen (ohne Begrenzung, Bestandstyp nicht gedeckt) · Lager-Hinweise (Treffer `.ls-note--info`, nicht nutzbar mit Gründen `.ls-note--warn`) · Trefferliste `.ls-table--cards` mit Badges „Empfohlen“, „aus Lager“, „Bestand“, „gedeckt/knapp“ · Auswahlkarte (Bezeichnung, Bestellnummer `<code>`, Konsole Wand/Stand, Haken aus Lager / Bestand / Halter) · Multi: Korb `.ls-items` mit Stepper und Deckungs-Badge · „+ Raum speichern“ · Rechenweg (aufklappbar)
10. Bericht (nur Druck, nach `bericht-vorlage.html`): Deckkopf (Kunde/Projekt, Datum, Anzahl Räume) · je Etage/Raum eine Karte mit Label/Wert-Liste (Heizkörper, Maße, Bestellnummer, Konsolen, Leistung/Deckung, nötige VL, erreichbare Raumtemperatur, Notiz) · Bestelltabellen „Zu bestellende Heizkörper“, „Aus dem Lager entnehmen“, „Zu bestellende Konsolen“, „Bestandsheizkörper“ mit Summenzeilen · Fuß (EN 442, „Angaben ohne Gewähr“, Toolname, Stand)
11. Fuß · Haus-Icon

## Ergebnis (Abschnitt 7 DESIGNREGELN)
- Kennzahlen: Heizlast · Bedarf (mit Reserve) · Raumtemperatur · System VL/RL · ΔT<sub>log</sub> · Max. Fläche.
- Tab ①: Tabelle Heizkörper (Typ, Ausführung, Badges) · BH · BL · BT (mit Abweichung zur Bestandstiefe, Zustand als Text und Farbe) · Leistung · Deckung · Status-Badge „gedeckt“ (≥ 110 %) / „knapp“ (100–110 %); Überhang über die Fläche als Text „+20 mm“.
- Tab ②: nötige VL · RL · Badge „WP-tauglich“ / „hoch“ / „> 75 °C“; „≤“-Werte als „überdimensioniert“ gekennzeichnet.
- Tab ③: erreichbare t<sub>L</sub> · Δ zum Wunsch · Leistung im Gleichgewicht · Badge „Wunsch erreicht“ / „knapp darunter“ / Wert in Rot mit Text.
- Leerzustand (kein Treffer): „Kein Heizkörper deckt bei VL/RL die Heizlast in der Fläche. Nötige Vorlauftemperatur in Ansicht ② prüfen oder Fläche vergrößern.“ plus Lagerliste mit Gründen.
- Alle Zahlen über `LS.fmt` (Komma, schmales Leerzeichen, echtes Minus); die Altfassung zeigt Punkt-Dezimale („24.7 K“, „-12.7 °C“). Das ist eine Darstellungs-, keine Rechenabweichung und wird in Schritt 5 so vermerkt.

## Zuordnung alt → neu für Projektdateien (FELD_MAPPING)
Die Projektdatei der Altfassung enthält keine Formularfelder, sondern `kunde` und die fertigen Raumobjekte (`raeume`). Die Raumobjekte (`hk`, `hkliste`, `konsole`, `noetigeVL`, `erreichbareTemp`, `ggLeistung`, `istBestand`, `ausLager`, `anschluss`, `etage`, `notiz`, `bestand`) werden **unverändert** weiterverwendet, weil Bericht, Bestellübersicht und GAEB sie lesen. Nur der Kopf wird abgebildet:
```js
var FELD_MAPPING = { kunde: 'projekt' };            // Datei v1 → v2; raeume bleiben wie sie sind
// Browser-Speicher (nur beim ersten Start nach dem Umbau, Schlüssel lutzspring_hk_auslegung_v1):
// raumname → raumName · raumtemp → raumTemp · taussen → normAT · tv → vl · tr → rl ·
// maxH → maxBH · maxL → maxBL · bestand → bestandTyp · gusstiefeFrei (sonst gusstiefe) → bautiefeBestand
```
Wichtig vor der Umstellung der iPads: Räume, die nur im Browser-Speicher der alten Adresse liegen, vorher mit „Projekt sichern (Datei)“ sichern (07, Punkt 2.4). Der Artikelstamm-Schlüssel `lutzspring-artikelstamm` bleibt gleich.

## Entfallene oder zu Ergebnissen gewordene Felder
| Feld (alt) | Was stattdessen | Begründung |
|---|---|---|
| Schnellwahl Raumtemperatur (16–24) | Zahlenfeld mit Standard 20 °C | Doppelung; offene Frage 4 |
| Schnellwahl System (7 Paare) | `.ls-seg` mit 4 Paaren oder Zahlenfelder | Doppelung; offene Frage 4 |
| ΔT<sub>log</sub> (graues Feld) | Kennzahl im Ergebnis | Zwischenrechnung |
| Bautiefe Guss (Auswahl) | Hinweis unter dem Zahlenfeld Bautiefe | Doppelung mit dem Freifeld |
| „Als PDF speichern“ | „Bericht drucken / PDF“ + Hinweis im „So geht’s“ | gleiche Funktion |
| Text-Slogan im Druckkopf „Sanitär · Heizung · Klimatechnik“ | Logo B mit Slogan „Heizung · Sanitär · Wärmepumpen“ | Designregel 2.6; alter Slogan ist überholt |
| Konsolenanzahl, Nabenabstand, QN | bleiben berechnete Angaben (Auswahlkarte, Bericht, GAEB) | ableitbar, wie alt |

## Offene Fragen an Fabian
1. Sicherheitsreserve: Standard 0 % statt 15 % und unter „Erweitert“ (06, 2026-10-18: 15 % ist kein Standard)? Vorschlag: ja.
2. Norm-Außentemperatur: Standard −12,7 °C statt −14 °C (06). Soll zusätzlich das PLZ-Feld mit `normaussentemperatur.json` wie in `auslegung.html` übernommen werden? Vorschlag: im Umbau nur der Standardwert; PLZ-Feld als Erweiterung Schritt 10.
3. Max. Bauhöhe / Baulänge: Standard 600 × 1400 mm beibehalten (Altfassung) oder leer (= unbegrenzt)? Vorschlag: 600 × 1400 beibehalten, leer bleibt möglich.
4. Schnellwahl: Systemtemperatur als `.ls-seg` 35/28 · 45/35 · 50/40 · 55/45 (höhere Werte frei tippen) und Raumtemperatur ohne Chips? Vorschlag: ja.
5. Vorhandene Bautiefe: ein Zahlenfeld (mm) mit Guss-Werten als Hinweis statt Auswahl plus Freifeld, unter „Erweitert“? Vorschlag: ja.
6. Vorhandener Heizkörpertyp (Bestand-Typ) nach „Erweitert“? Vorschlag: ja (selten genutzt, wirkt nur auf Sortierung).
7. Montagezeit je Heizkörper (GAEB) nach „Erweitert“ statt in die Kopfzeile; leer bleibt leer? Vorschlag: ja.
8. „Als PDF speichern“ entfällt zugunsten „Bericht drucken / PDF“? Vorschlag: ja.
9. Etage als `.ls-select` (sieben Einträge inkl. „keine“) statt sechs Buttons? Vorschlag: ja; Alternative `.ls-seg`, die in schmalen Spalten seitlich scrollt (BACKLOG 2026-10-25).
10. Gespeicherte Räume: Die Altfassung kann einen Raum nicht zurück ins Formular laden („Bearbeiten“), nur überschreiben (gleicher Name + Etage). Im Umbau so lassen; „Bearbeiten“ und „Duplizieren“ als Erweiterung Schritt 10 vormerken? Vorschlag: ja.
11. Undo (Strg/Cmd+Z für Räume) behalten, Rückmeldung über `LS.toast`? Vorschlag: ja.
12. Kundenhinweis bei Untertemperatur (Sie-Form, wird in die Notiz geschrieben): Text unverändert übernehmen, nur Zahlen mit Komma und echtem Minus formatieren? Vorschlag: ja (Darstellungsabweichung, in Schritt 5 vermerkt).
13. Messung der Referenzwerte mit geladener `lager.json` (Stand 2026-08-12, 14 Positionen) über den lokalen Server; H1–H6 als Räume eines Projekts „BV Muster“, zusätzlich H-Gesamt (Bestellübersicht, GAEB) – einverstanden?
14. `04_tool-konventionen.md`: Stand des Tools auf die Fassung 09/2026 (GAEB, Artikeldaten, Bestandskonsolen) korrigieren – in Schritt 8, oder schon jetzt?
