# Referenzwerte · Interne Tools der Lutz Spring GmbH

Stand: 2026-10-25 · Nur erfundene Werte, keine Kundendaten (Repo ist öffentlich) [BESTÄTIGT, 2026-10-12].
Zweck: Nachweis, dass ein Umbau nichts an der Rechnung ändert (UMBAU-ABLAUF.md Schritt 3 und 5). Jeder Fall wird in der Altfassung gemessen (Spalte „alt“), dann in der Neufassung (Spalte „neu“). Jede Abweichung ist ein Fehler, bis das Gegenteil belegt ist. PDFs unter `tests/pdf/TOOLNAME_FALL_alt.pdf` und `…_neu.pdf`.

Die Spalte „erwartet (Formel)“ ist aus dem Rechenteil der Altfassung nachgerechnet (Python, gleiche Formeln und Rundung) und dient nur als Plausibilitätsprüfung; maßgeblich ist die Messung in der Altfassung.

Testfälle je Tool: A1–A6 `auslegung.html` (angelegt 2026-10-25; A5 nur neu, A6 Erweiterung Kaskade) · K1–K6 `kostenvergleich.html` · F1–F6 `foerderung.html` (angelegt 2026-10-26) · H1–H6 `heizkoerper.html` (angelegt 2026-11-08) · B1–B3 `bestandsaufnahme.html`. Die Fälle der noch nicht begonnenen Tools werden beim jeweiligen Umbau (Schritt 2) angelegt.

---

## kostenvergleich.html · K1–K6

Altfassung: `alt/kostenvergleich_2026-10-25.html` · Feldliste: `tests/feldliste-kostenvergleich.md`.
Messung „alt“ am 2026-10-25 in der unveränderten Altfassung (lokaler Server, Chromium; Eingaben per Skript gesetzt, Ergebnisse aus der Anzeige abgelesen). PDFs: `tests/pdf/kostenvergleich_K1_alt.pdf` … `K3_alt.pdf` (Druck aus Chrome, A4, ohne Kopf-/Fußzeile).
Rundung der Altfassung: Jahreskosten und Ersparnis auf ganze €, ct/kWh auf 2 Nachkommastellen, effektive JAZ auf 2 Nachkommastellen, Prozent auf ganze Zahl, PV-Mengen auf ganze kWh.

**Befund aus der Messung (K2, K3):** Die Altfassung übernimmt den SCOP nicht exakt. Das JAZ-Feld ist ein Schieberegler mit Schrittweite 0,1; beim Setzen von 3,79 bzw. 5,09 rundet der Browser auf 3,8 bzw. 5,1, und mit diesem gerundeten Wert wird gerechnet (Anzeige „JAZ 3,8“ neben dem Hinweis „SCOP 55 °C für WEB 9/14“). Die Spalte „erwartet (Formel)“ rechnet mit dem exakten Datenblattwert und weicht deshalb um wenige € ab. Entscheidung Fabian 2026-10-25 (06): Neufassung rechnet mit dem exakten SCOP; die Abweichungen bei K2/K3 sind gewollt und entsprechen der Spalte „erwartet (Formel)“. K1 (JAZ manuell) ist identisch.

Messung „neu“ am 2026-10-25 in `kostenvergleich.html` (design.css 2.1.1), gleiche Eingaben, Heizöl als 0,95 €/L (neue Einheit). Alle Werte stimmen mit „erwartet (Formel)“ überein. PDFs: `tests/pdf/kostenvergleich_K1_neu.pdf` … `K3_neu.pdf`.

### K1 · Gas, Standardwerte, JAZ manuell, ohne PV
Eingaben: Jahres-Wärmebedarf 18 000 kWh · Baugröße manuell · JAZ 4,0 · Heizstab-Anteil 0 % · Strompreis 27,0 ct/kWh · Bestandsanlage Gas · Gaspreis 11,0 ct/kWh · Nutzungsgrad 92 % · PV aus.

| Kennzahl | Einheit | erwartet (Formel) | alt (gemessen 2026-10-25) | neu (gemessen) | Status |
|---|---|---|---|---|---|
| Effektive JAZ | – | 4,00 | 4,00 | 4,00 | gleich |
| Wärmepumpe ct/kWh Wärme | ct/kWh | 6,75 | 6,75 | 6,75 | gleich |
| Gas ct/kWh Wärme | ct/kWh | 11,96 | 11,96 | 11,96 | gleich |
| Jahreskosten Wärmepumpe | €/a | 1 215 | 1 215 | 1 215 | gleich |
| Jahreskosten Gas | €/a | 2 152 | 2 152 | 2 152 | gleich |
| Ersparnis pro Jahr | €/a | 937 | 937 | 937 | gleich |
| Günstiger als Gas | % | 44 | 44 | 44 | gleich |

### K2 · Öl, WEB 9/14 bei W55 (SCOP 3,79), Heizstab 3 %, ohne PV
Eingaben: Jahres-Wärmebedarf 24 000 kWh · Baugröße WEB 9/14 · Heizflächen Heizkörper W55 (JAZ übernimmt SCOP, Feld gesperrt) · Heizstab-Anteil 3 % · Strompreis 30,0 ct/kWh · Bestandsanlage Öl · Ölpreis 95 ct/L · Nutzungsgrad 88 % · PV aus.

| Kennzahl | Einheit | erwartet (Formel, SCOP 3,79) | alt (gemessen 2026-10-25, JAZ-Feld 3,8) | neu (gemessen) | Status |
|---|---|---|---|---|---|
| JAZ (übernommen) | – | 3,79 | 3,8 (Reglerschritt 0,1) | 3,79 | gewollt (06: SCOP exakt) |
| Effektive JAZ | – | 3,50 | 3,51 | 3,50 | gewollt, = Formel |
| Wärmepumpe ct/kWh Wärme | ct/kWh | 8,58 | 8,56 | 8,58 | gewollt, = Formel |
| Öl ct/kWh Wärme | ct/kWh | 10,80 | 10,80 | 10,80 | gleich (Eingabe 0,95 €/L) |
| Jahreskosten Wärmepumpe | €/a | 2 059 | 2 054 | 2 059 | gewollt, = Formel |
| Jahreskosten Öl | €/a | 2 591 | 2 591 | 2 591 | gleich |
| Ersparnis pro Jahr | €/a | 532 | 537 | 532 | gewollt, = Formel |
| Günstiger als Öl | % | 21 | 21 | 21 | gleich |

Hinweis zu K2 (Stand 2026-10-25, nach Umbau der Heizflächen-Option): Die Option „Heizkörper · W55“ gibt es in der Neufassung nicht mehr (Auslegung auf 50 °C, Entscheidung Fabian, 06). K2 bleibt als Nachweis der Rechenlogik gültig und ist in der Neufassung über „Baugröße manuell, JAZ 3,79“ reproduzierbar (gemessen 2026-10-25: identische Werte wie in Spalte „neu“). Für die neue Option gilt K4.

### K4 · Öl, WEB 9/14 bei W50 (SCOP interpoliert), Heizstab 3 %, ohne PV
Erweiterung 2026-10-25 (Entscheidung Fabian: Heizkörper-Auslegung auf 50 °C). Der SCOP W50 ist kein Datenblattwert, sondern linear zwischen W35 (4,59) und W55 (3,79) interpoliert und auf 2 Stellen gerundet: 4,59 + (3,79 − 4,59) × 15 ÷ 20 = 3,99 [VORSCHLAG der Methode; offener Punkt 07: Wert bei Weishaupt erfragen]. Kein „alt“-Wert, weil die Altfassung nur W35/W55 kannte.
Eingaben wie K2, aber Heizflächen „Heizkörper · W50“: Jahres-Wärmebedarf 24 000 kWh · WEB 9/14 · Heizstab-Anteil 3 % · Strompreis 30,0 ct/kWh · Öl 0,95 €/L · Nutzungsgrad 88 % · PV aus.

| Kennzahl | Einheit | erwartet (Formel) | alt | neu (gemessen 2026-10-25) | Status |
|---|---|---|---|---|---|
| JAZ (übernommen, W50) | – | 3,99 | – | 3,99 | = Formel |
| Effektive JAZ | – | 3,66 | – | 3,66 | = Formel |
| Wärmepumpe ct/kWh Wärme | ct/kWh | 8,19 | – | 8,19 | = Formel |
| Öl ct/kWh Wärme | ct/kWh | 10,80 | – | 10,80 | = Formel |
| Jahreskosten Wärmepumpe | €/a | 1 966 | – | 1 966 | = Formel |
| Jahreskosten Öl | €/a | 2 591 | – | 2 591 | = Formel |
| Ersparnis pro Jahr | €/a | 625 | – | 625 | = Formel |
| Günstiger als Öl | % | 24 | – | 24 | = Formel |

PDF: `tests/pdf/kostenvergleich_K4_neu.pdf`.

### K5 · Gas, WWP LA 20/30-A R bei W35 (SCOP 5,20), ohne Heizstab, ohne PV
Erweiterung 2026-10-25 (Fabian: weitere Baureihen WAB, WGB, WWP LA). SCOP W35/W55 der WWP LA aus Weishaupt Fachinformation 83603701 (07/2026) [BELEGT]. Kein „alt“-Wert, weil die Altfassung nur WEB kannte. Für WAB und WGB ist nur der SCOP W35 belegt; der W55-Wert wird seit dem Nachtrag K6 aus ηs zurückgerechnet.
Eingaben: Jahres-Wärmebedarf 40 000 kWh · WWP LA 20/30-A R · Flächenheizung W35 · Heizstab-Anteil 0 % · Strompreis 30,0 ct/kWh · Gas 14,0 ct/kWh · Nutzungsgrad 92 % · PV aus.

| Kennzahl | Einheit | erwartet (Formel) | alt | neu (gemessen 2026-10-25) | Status |
|---|---|---|---|---|---|
| JAZ (übernommen, W35) | – | 5,20 | – | 5,20 | = Formel |
| Effektive JAZ | – | 5,20 | – | 5,20 | = Formel |
| Wärmepumpe ct/kWh Wärme | ct/kWh | 5,77 | – | 5,77 | = Formel |
| Gas ct/kWh Wärme | ct/kWh | 15,22 | – | 15,22 | = Formel |
| Jahreskosten Wärmepumpe | €/a | 2 308 | – | 2 308 | = Formel |
| Jahreskosten Gas | €/a | 6 087 | – | 6 087 | = Formel |
| Ersparnis pro Jahr | €/a | 3 779 | – | 3 779 | = Formel |
| Günstiger als Gas | % | 62 | – | 62 | = Formel |

PDF: `tests/pdf/kostenvergleich_K5_neu.pdf`.

### K6 · Gas, WAB 11-A bei W50 (W55 aus ηs zurückgerechnet), ohne Heizstab, ohne PV
Erweiterung 2026-10-25 (Fabian: für WAB und WGB wird ein sinnvoller SCOP gebraucht, notfalls errechnet). Weishaupt veröffentlicht für WAB/WGB keinen SCOP W55; er wird aus der ηs (W55) der Fachinformation zurückgerechnet: SCOP = (ηs + 3) × 2,5 ÷ 100 (Verordnung (EU) 813/2013, CC = 2,5, F = 3 %; an sechs belegten Weishaupt-Paaren geprüft, Abweichung ≤ 0,4 Prozentpunkte). WAB 11-A: ηs 138 % → W55 ≈ 3,53; W50 = 4,70 + (3,53 − 4,70) × 15 ÷ 20 = 3,82 [BERECHNET, Näherung; 07/21].
Eingaben: Jahres-Wärmebedarf 18 000 kWh · WAB 11-A · Heizkörper W50 · Heizstab-Anteil 0 % · Strompreis 30,0 ct/kWh · Gas 14,0 ct/kWh · Nutzungsgrad 92 % · PV aus.

| Kennzahl | Einheit | erwartet (Formel) | alt | neu (gemessen 2026-10-25) | Status |
|---|---|---|---|---|---|
| JAZ (übernommen, W50) | – | 3,82 | – | 3,82 | = Formel |
| Effektive JAZ | – | 3,82 | – | 3,82 | = Formel |
| Wärmepumpe ct/kWh Wärme | ct/kWh | 7,85 | – | 7,85 | = Formel |
| Gas ct/kWh Wärme | ct/kWh | 15,22 | – | 15,22 | = Formel |
| Jahreskosten Wärmepumpe | €/a | 1 414 | – | 1 414 | = Formel |
| Jahreskosten Gas | €/a | 2 739 | – | 2 739 | = Formel |
| Ersparnis pro Jahr | €/a | 1 326 | – | 1 326 | = Formel |
| Günstiger als Gas | % | 48 | – | 48 | = Formel |

PDF: `tests/pdf/kostenvergleich_K6_neu.pdf`.

### K3 · Gas, WEB 7/10 bei W35 (SCOP 5,09), Heizstab 1,5 %, mit PV
Eingaben: Jahres-Wärmebedarf 12 000 kWh · Baugröße WEB 7/10 · Heizflächen Flächenheizung W35 (JAZ übernimmt SCOP) · Heizstab-Anteil 1,5 % · Strompreis 28,0 ct/kWh · Bestandsanlage Gas · Gaspreis 12,4 ct/kWh · Nutzungsgrad 85 % · PV an: 9,5 kWp · 980 kWh/kWp · Deckungsanteil 30 %.

| Kennzahl | Einheit | erwartet (Formel, SCOP 5,09) | alt (gemessen 2026-10-25, JAZ-Feld 5,1) | neu (gemessen) | Status |
|---|---|---|---|---|---|
| JAZ (übernommen) | – | 5,09 | 5,1 (Reglerschritt 0,1) | 5,09 | gewollt (06: SCOP exakt) |
| Effektive JAZ | – | 4,80 | 4,80 | 4,80 | gleich |
| Wärmepumpe ct/kWh Wärme | ct/kWh | 5,84 | 5,83 | 5,84 | gewollt, = Formel |
| Gas ct/kWh Wärme | ct/kWh | 14,59 | 14,59 | 14,59 | gleich |
| Jahreskosten Wärmepumpe | €/a | 701 | 699 | 701 | gewollt, = Formel |
| Jahreskosten Gas | €/a | 1 751 | 1 751 | 1 751 | gleich |
| PV-Ertrag pro Jahr | kWh | 9 310 | 9 310 | 9 310 | gleich |
| Gedeckter WP-Stromanteil | kWh | 751 | 749 | 751 | gewollt, = Formel |
| Zusätzliche Ersparnis durch PV | €/a | 210 | 210 | 210 | gleich |
| Jahreskosten WP + PV | €/a | 490 | 490 | 490 | gleich |
| Ersparnis pro Jahr (WP + PV gegen Gas) | €/a | 1 260 | 1 261 | 1 260 | gewollt, = Formel |
| Günstiger als Gas | % | 72 | 72 | 72 | gleich |

---

## auslegung.html · A1–A6

Altfassung: `alt/auslegung_2026-10-25.html` · Feldliste: `tests/feldliste-auslegung.md`.
Angelegt 2026-10-25 (Schritt 2). Spalte „erwartet (Formel)“ aus dem Rechenteil der Altfassung nachgerechnet (Python, gleiche Stützpunkte, Interpolation und Rundung: kW auf 1 Nachkommastelle, Prozent ganzzahlig). Spalte „alt“ wird in Schritt 3 in der unveränderten Altfassung gemessen, „neu“ in Schritt 5.

Messung „alt“ am 2026-10-25 in der unveränderten Altfassung (`auslegung.html` im Git, byteidentisch mit `alt/auslegung_2026-10-25.html`; die Kopie in `alt/` selbst lädt `theme.css` nicht, weil die Datei im Hauptordner liegt). Ablauf: lokaler Server, Eingaben als Arbeitsstand im Format der Altfassung gesetzt (localStorage, wie nach einem Neuladen), Ergebnisse aus der Anzeige abgelesen (headless Chrome, DOM), PDFs `tests/pdf/auslegung_A1_alt.pdf` … `A4_alt.pdf` (Druckzusammenfassung der Altfassung, A4, je 2 Seiten, ohne Kopf-/Fußzeile des Browsers). Alle Kennzahlen stimmen mit „erwartet (Formel)“ überein. Anzeigeformat der Altfassung: ganze kW ohne „,0“ („10 kW“, „≈ 13 kW“, „1,7–14 kW“); in der Tabelle steht der Wert, das Format in Klammern.

**Befunde aus der Messung (Altfassung):**
1. Kühlfunktion geht beim Neuladen verloren: `standAnwenden` prüft die Sichtbarkeit der Kühlung, bevor die Heizflächen wiederhergestellt sind; dadurch wird „Kühlung ja“ auf „nein“ gesetzt und der Kühlblock bleibt ausgeblendet, bis die Heizfläche erneut angetippt wird. Betrifft auf dem iPad jedes Neuladen der Homescreen-App. Für A2 wurden die Zusatzpositionen deshalb zusätzlich im Browser nach echtem Klick (Fußbodenheizung, „Ja, aktive Kühlung“) gemessen; das PDF `auslegung_A2_alt.pdf` zeigt den Zustand nach Neuladen (Kühloption statt der vier Kühlpositionen). Die Neufassung muss den Zustand vollständig wiederherstellen; das ist eine gewollte Abweichung (Fehlerbehebung), Kennzahlen unverändert.
2. Die Kundenfälle-Liste wird nie aus dem Speicher geladen (`kundenLaden` ist definiert, wird aber nicht aufgerufen); nach einem Neuladen ist die Liste leer, und „Speichern“ überschreibt die gespeicherte Liste. Es gibt daher keine alten Fälle, die zu übernehmen wären (Feldliste, Frage 1). Der Fallname erscheint deshalb auch nicht in den PDFs; der Seitentitel lautet „Wärmepumpenauslegungstool - Gerät - Datum“.
3. Die Kennzahlen „Gebäudeheizlast“, „WP-Leistung bei Norm-Außentemperatur“ und „Heizstab-Anteil“ erscheinen nur bei Luft/Wasser; bei Sole/Wasser (A4) steht stattdessen der Sole-Hinweis.

Messung „neu“ am 2026-10-25 in `auslegung.html` (design.css 2.1.2): gleiche Eingaben als Browser-Speicher der Neufassung gesetzt (Felder per id, Verteilerliste und Aeroblock-Zustand), Ergebnisse aus der Anzeige abgelesen (headless Chrome), PDFs `tests/pdf/auslegung_A1_neu.pdf` … `A5_neu.pdf`. **Alle Kennzahlen aller Fälle sind identisch mit „erwartet (Formel)“ und „alt“** (Vergleich per Skript, 0 Abweichungen). Unterschiede nur in der Darstellung: ganze kW mit „,0“ („10,0 kW“), Status als Badge „gedeckt / knapp / nicht gedeckt“ statt ✓ ◐ ⚠, Hinweise ohne Symbole, „Blossom-ic“ statt „Blossom-IC“, kein „R290“ im Gerätetext. A2: die vier Kühlpositionen erscheinen in der Neufassung auch nach Neuladen (Befund 1 behoben). Die Neufassung druckt Ergebnis und Bericht (Zusammenfassung, Heizlast, Ergebnis, Zusatzpositionen, Rechenweg); die PDFs sind länger als die der Altfassung, weil Karten im Druck nicht umbrechen (BACKLOG).
Die Altfassung kennt für Norm-Außentemperatur und Bivalenzpunkt nur feste Auswahlwerte; A1–A4 nutzen deshalb −14 / −12 / −16 °C. Für die neue Vorgabe −12,7 °C (07/5) kommt in Schritt 5 ein Fall A5 nur „neu“ (wie K4 beim Kostenvergleich), sobald Fabian die Feldliste freigibt.
Gemeinsame Vorgaben, soweit nicht anders genannt: Wärmequelle Luft/Wasser · Aufgabe Heizung + Warmwasser · Speicher getrennt WAS 300 l + WES 200 l · Abwasser ja · Zirkulation nein · Solarthermie nein · PV nein · strenge Auslegung aus · bisheriger Vorlauf unbekannt · Rohrsystem Zweirohr · RTL nein · Kühlung nein · Aeroblock-Alternative nicht aktiviert · Notizen leer.

### A1 · Öl, Pauschale, Heizkörper, Standardwerte
Eingaben: Bestand Öl, 3 000 L/a, Kesseltyp Niedertemperatur (85 %) · Wohnfläche 120 m² · Wohneinheiten 1 · Personen 2 · Gebäudehülle Pauschale „Altbau teilsaniert (90 W/m²)“ · Heizflächen Heizkörper, 8 Stück, Zweirohr · Norm-Außentemperatur −14 °C · Bivalenzpunkt −7 °C · Ziel-Vorlauf 50 °C (automatisch aus Heizkörper).
Rechenweg: Verbrauch 3 000 × 10 × 0,85 ÷ 1 800 = 14,17 kW · Gebäude 120 × 90 ÷ 1 000 = 10,80 kW · Abweichung 26,97 % → 50/50 → 12,48 kW · + 2 × 0,2 kW = 12,88 kW · Lastanteil (15 + 7) ÷ (15 + 14) = 0,7586 → 9,77 kW · WEB 7/10 bei −7/W50: 7,00 × 1,15 = 8,05 < 9,77 → WEB 9/14: (9,4 + 9,3) ÷ 2 = 9,35 × 1,15 = 10,75 ≥ 9,77 · bei −14 °C: 8,55 + (1 ÷ 8) × (9,35 − 8,55) = 8,65 kW.

| Kennzahl | Einheit | erwartet (Formel) | alt (gemessen) | neu (gemessen) | Status |
|---|---|---|---|---|---|
| Ölverbrauchsansatz | kW | 14,2 | 14,2 | 14,2 kW | gleich |
| Gebäudeansatz (Pauschale 90 W/m²) | kW | 10,8 | 10,8 | 10,8 kW | gleich |
| Plausibilität | – | „plausibel (27 % Abweichung)“, Mittel beider Wege | „plausibel (27 % Abweichung)“, Mittel beider Wege | Verbrauch und Gebäudehülle plausibel (27 % Abweichung). Auslegungswert = Mittel beider Wege, plus 0,4 kW Warmwasser für 2 Personen. | gleich |
| Auslegungswert Heizlast (inkl. 0,4 kW Warmwasser) | kW | 12,9 | 12,9 | 12,9 kW | gleich |
| Empfohlenes Gerät | – | Weishaupt Evoblock WEB 9/14 · Modulation 1,7–14,0 kW · 230 V | Weishaupt Evoblock WEB 9/14 · Modulation 1,7–14 kW · 230 V (Anzeige „14“) | Weishaupt Evoblock WEB 9/14 · Modulation 1,7–14,0 kW · 230 V · Luft/Wasser · Badge „Vorschlag“ | gleich |
| Gebäudelast bei −7 °C | kW | 9,8 | 9,8 | 9,8 kW | gleich |
| WP-Leistung A−7/W50 | kW | ≈ 9,4 | ≈ 9,4 | ≈ 9,4 kW | gleich |
| Deckung am Bivalenzpunkt | % | 96 | 96 | 96 % | gleich |
| Status Bivalenzpunkt | – | knapp (geringe Heizstab-Unterstützung) | „◐ Am Bivalenzpunkt −7 °C bereits geringe Heizstab-Unterstützung nötig“ (warn) | „knapp“ · Am Bivalenzpunkt −7 °C bereits geringe Heizstab-Unterstützung nötig. | gleich |
| Nennwärmeleistung A−7/W35 | kW | 9,5 | 9,5 | 9,5 kW | gleich |
| Gebäudeheizlast bei −14 °C | kW | 12,9 | 12,9 | 12,9 kW | gleich |
| WP-Leistung A−14/W50 | kW | ≈ 8,7 | ≈ 8,7 | ≈ 8,7 kW | gleich |
| Heizstab-Anteil bei −14 °C | kW | ≈ 4,2 (Stufe 2) | ≈ 4,2; Hinweis „rechnerisch Stufe 2“ | ≈ 4,2 kW (Stufe 2) | gleich |
| Taktungsrisiko | – | nein | nein | nein | gleich |
| Alternative Aeroblock | – | WAB 14, ≈ 10,9 kW, 100 % Deckung | Weishaupt Aeroblock WAB 14, „Leistung ≈ 10,9 kW, 100 % Deckung“, Button „Aeroblock stattdessen auslegen“ | Alternative: Weishaupt Aeroblock WAB 14 · Würde die Anforderung ebenfalls erfüllen (Leistung ≈ 10,9 kW, 100 % Deckung am Bivalenzpunkt), ist aber teurer. Nur akti… | gleich |
| Zusatzpositionen | – | Öltankentsorgung · WAS 300 l · WES 200 l · Schallgutachten (46 dB(A), optional) | Öltankentsorgung · WAS 300 l · WES 200 l (erforderlich) · Schallgutachten (optional) | Öltankentsorgung inkl. Restölentsorgung [erforderlich] · WAS 300 l · Trinkwasserspeicher [erforderlich] · WES 200 l · Energiespeicher [erforderlich] · Schallgutachten / Schallschutzmaßnahmen [optional] | gleich |

### A2 · Gas in kWh, Bauteil-Konfigurator, Fußbodenheizung, Kühlung, WMS, Aeroblock aktiviert
Eingaben: Bestand Gas, 18 000 kWh/a (Einheit kWh), Kesseltyp Brennwert (96 %) · Wohnfläche 150 m² · Wohneinheiten 1 · Personen 4 · Gebäudehülle Bauteile: Baujahr 1995–2001, Fenster 2-fach modern, Wand massiv / 24–36 cm / Teildämmung, Dach gut gedämmt · Heizflächen Fußbodenheizung, 2 Verteiler (Erdgeschoss 6 Kreise, 1. Obergeschoss 5 Kreise) · Kühlung ja, 3 Räume · Norm-Außentemperatur −12 °C · Bivalenzpunkt −7 °C · Ziel-Vorlauf 35 °C (automatisch) · Speicher WMS 300/120 · Abwasser nein · Zirkulation ja · Solarthermie ja, demontieren · PV geplant · im Ergebnis „Aeroblock stattdessen auslegen“ angetippt.
Rechenweg: Verbrauch 18 000 × 0,96 ÷ 1 800 = 9,60 kW · spezifisch max(15; 45 + 5 + (14 + 0 − 6) + (−3)) = 55 W/m² → 8,25 kW · Abweichung 15,13 % → 50/50 → 8,925 kW · + 4 × 0,2 = 9,725 kW · Lastanteil 22 ÷ 27 = 0,8148 → 7,92 kW · Evoblock: WEB 7/10 bei −7/W35 7,09 × 1,15 = 8,15 ≥ 7,92 · Aeroblock: WAB 8 6,2 × 1,15 = 7,13 < 7,92; WAB 11 6,9 × 1,15 = 7,94 ≥ 7,92 → aktiviert · bei −12 °C: 6,2 + (3 ÷ 8) × 0,7 = 6,46 kW.

| Kennzahl | Einheit | erwartet (Formel) | alt (gemessen) | neu (gemessen) | Status |
|---|---|---|---|---|---|
| Gasverbrauchsansatz | kW | 9,6 | 9,6 | 9,6 kW | gleich |
| Bauteilansatz (Zeile) | – | „≈ 55 W/m² (Basis 45 + Fenster 5 + Wand 8 + Dach −3)“ | „≈ 55 W/m² (Basis 45 + Fenster 5 + Wand 8 + Dach −3)“ | im Rechenweg: Bauteilansatz ≈ 55 W/m² (Basis 45 + Fenster 5 + Wand 8 + Dach −3; mindestens 15) × 150 m² ÷ 1 000 = 8,25 kW | gleich |
| Gebäudeansatz | kW | 8,3 | 8,3 | 8,3 kW | gleich |
| Plausibilität | – | „plausibel (15 % Abweichung)“ | „plausibel (15 % Abweichung)“ | Verbrauch und Gebäudehülle plausibel (15 % Abweichung). Auslegungswert = Mittel beider Wege, plus 0,8 kW Warmwasser für 4 Personen. | gleich |
| Auslegungswert Heizlast (inkl. 0,8 kW Warmwasser) | kW | 9,7 | 9,7 | 9,7 kW | gleich |
| Standardempfehlung (vor Umschalten) | – | Weishaupt Evoblock WEB 7/10 | Weishaupt Evoblock WEB 7/10 („Standardempfehlung wäre …“, Button „Zurück zur Evoblock (WEB 7/10)“) | Weishaupt Evoblock WEB 7/10 (Text in der Alternativ-Karte, Button „Zurück zur Evoblock“) | gleich |
| Ausgelegtes Gerät (Ausweichlösung aktiv) | – | Weishaupt Aeroblock WAB 11 · Modulation 1,6–11,0 kW · 230 V | Weishaupt Aeroblock WAB 11 · Modulation 1,6–11 kW · 230 V, Badge „Ausweichlösung aktiv“ | Weishaupt Aeroblock WAB 11 · Modulation 1,6–11,0 kW · 230 V · Luft/Wasser · Badge „Ausweichlösung“ | gleich |
| Gebäudelast bei −7 °C | kW | 7,9 | 7,9 | 7,9 kW | gleich |
| WP-Leistung A−7/W35 | kW | ≈ 6,9 | ≈ 6,9 | ≈ 6,9 kW | gleich |
| Deckung am Bivalenzpunkt | % | 87 | 87 | 87 % | gleich |
| Status Bivalenzpunkt | – | knapp (geringe Heizstab-Unterstützung) | „◐ … geringe Heizstab-Unterstützung nötig“ (warn) | „knapp“ · Am Bivalenzpunkt −7 °C bereits geringe Heizstab-Unterstützung nötig. | gleich |
| Nennwärmeleistung A−7/W35 | kW | 6,9 | 6,9 | 6,9 kW | gleich |
| Gebäudeheizlast bei −12 °C | kW | 9,7 | 9,7 | 9,7 kW | gleich |
| WP-Leistung A−12/W35 | kW | ≈ 6,5 | ≈ 6,5 | ≈ 6,5 kW | gleich |
| Heizstab-Anteil bei −12 °C | kW | ≈ 3,3 (Stufe 2) | ≈ 3,3; Hinweis „rechnerisch Stufe 2“ | ≈ 3,3 kW (Stufe 2) | gleich |
| Taktungsrisiko | – | nein | nein | nein | gleich |
| Zusatzpositionen | – | Kanister Solarflüssigkeit · WMS 300/120 · Hebeanlage · Zirkulationspumpe · zusätzliche Heizkreisverteiler (1 über Standard, 2 Verteiler / 11 Kreise) · Taupunktfühler · diffusionsdichte Dämmung · Blossom-ic Raumregler (3 Räume) · Umschaltung Heizen/Kühlen · Schallgutachten (49 dB(A)) | nach Klick: Kanister · WMS 300/120 · Hebeanlage · Zirkulationspumpe · zusätzliche Heizkreisverteiler (1 über Standard) · Taupunktfühler · diffusionsdichte Dämmung · Blossom-IC Raumregler (3 Räume) · Umschaltung Heizen/Kühlen (alle erforderlich) · Schallgutachten (optional). Nach Neuladen fehlen die vier Kühlpositionen (Befund 1) | Leerer Kanister für Solarflüssigkeit [erforderlich] · WMS 300/120 · Modularspeicher [erforderlich] · Hebeanlage für Ablauf Sicherheitsgruppe [erforderlich] · Zirkulationspumpe mit Zeit-/Temperatursteuerung [erforderlich] · Zusätzliche Heizkreisverteiler (1 über Standard) [erforderlich] · Taupunktfühler / Kondensatüberwachung [erforderlich] · Diffusionsdichte Dämmung der kälteführenden Leitungen [erforderlich] · Blossom-ic Raumregler (3 Räume) [erforderlich] · Umschaltung Heizen/Kühlen + Kühlstation [erforderlich] · Schallgutachten / Schallschutzmaßnahmen [optional] | gleich, auch nach Neuladen (Befund 1 behoben) |

### A3 · Vorhandene Wärmepumpe, U-Werte direkt, Heizkörper + FBH, nur Heizung, streng, bisheriger Vorlauf bekannt
Eingaben: Bestand Wärmepumpe, 6 000 kWh/a Strom, JAZ 3,0 · Wohnfläche 200 m² · Wohneinheiten 2 · Personen 3 (ohne Wirkung, WW extern) · Gebäudehülle U-Werte: Außenwand 180 m² / 1,2 · Dach 120 m² / 0,6 · Fenster 30 m² / 2,8 · Boden 120 m² / 0,8 · Heizflächen Beides: 6 Heizkörper, Einrohr, RTL ja (2 Stück), Vorlauf Heizkörper-Kreis 50 °C, FBH-Kreis 40 °C, 1 Verteiler (Erdgeschoss 4 Kreise) · bisheriger Vorlauf bekannt: 70 °C, „knapp“ · Aufgabe nur Heizung · WES 500 l · Norm-Außentemperatur −16 °C · Bivalenzpunkt −7 °C · Ziel-Vorlauf 50 °C (aus Heizkörper-Kreis) · strenge Auslegung an.
Rechenweg: Verbrauch 6 000 × 3,0 ÷ 1 800 = 10,00 kW · ΔT = 36 K · Σ(A × U) = 216 + 72 + 84 + 96 = 468 W/K → Transmission 16,85 kW · Lüftung 0,5 × 200 × 2,5 × 0,34 × 36 ÷ 1 000 = 3,06 kW → 19,91 kW · Abweichung 66,26 % > 35 → 0,6 × 19,91 + 0,4 × 10,00 = 15,94 kW · kein Warmwasser · Lastanteil 22 ÷ 31 = 0,7097 → 11,32 kW · streng (× 1,0): WEB 7/10 7,00 · 9/14 9,35 · 10/15 9,95 < 11,32; WEB 13/20 (13,0 + 12,9) ÷ 2 = 12,95 ≥ 11,32 · Aeroblock: WAB 14 10,9 < 11,32 → keine Alternative · bei −16 °C (geklemmt auf −15): 12,35 kW.

| Kennzahl | Einheit | erwartet (Formel) | alt (gemessen) | neu (gemessen) | Status |
|---|---|---|---|---|---|
| WP-Verbrauchsansatz | kW | 10,0 | 10 (Anzeige „10 kW“) | 10,0 kW | gleich |
| U-Wert-Zeile | – | „Transmission ≈ 16,8 kW + Lüftung ≈ 3,1 kW bei ΔT=36 K“ | „Transmission ≈ 16,8 kW + Lüftung ≈ 3,1 kW bei ΔT=36 K“ | im Rechenweg: Transmission Σ(A × U) × ΔT = 468,0 W/K × 36,0 K ÷ 1 000 ≈ 16,8 kW + Lüftung 0,5 1/h × 200 m² × 2,5 m × 0,34 Wh/(m³·K) × 36,0 K ÷ 1 000 ≈ 3,1 kW (Norm-Innentemperatur 20 °C) = 19,91 kW | gleich |
| Gebäudeansatz | kW | 19,9 | 19,9 | 19,9 kW | gleich |
| Plausibilität | – | „Große Abweichung (66 %) … Gebäudeansatz 60 %“ | „Große Abweichung (66 %) … Gebäudeansatz 60 %“ | Große Abweichung zwischen Verbrauch und Gebäudehülle (66 %). Altanlage evtl. über- oder unterdimensioniert oder untypisches Nutzerverhalten; der Gebäudeansatz wird mit 60 % stärker gewichtet. Vor dem Angebot prüfen. | gleich |
| Auslegungswert Heizlast (ohne Warmwasser) | kW | 15,9 | 15,9 | 15,9 kW | gleich |
| Empfohlenes Gerät | – | Weishaupt Evoblock WEB 13/20 · Modulation 3,2–20,0 kW · 400 V | Weishaupt Evoblock WEB 13/20 · Modulation 3,2–20 kW · 400 V | Weishaupt Evoblock WEB 13/20 · Modulation 3,2–20,0 kW · 400 V · Luft/Wasser · Badge „Vorschlag“ | gleich |
| Gebäudelast bei −7 °C | kW | 11,3 | 11,3 | 11,3 kW | gleich |
| WP-Leistung A−7/W50 | kW | ≈ 13,0 | ≈ 13 (Anzeige „≈ 13 kW“) | ≈ 13,0 kW | gleich |
| Deckung am Bivalenzpunkt | % | 100 | 100 | 100 % | gleich |
| Status Bivalenzpunkt | – | gedeckt (ohne Heizstab) | „✓ Die Wärmepumpe deckt den Bivalenzpunkt −7 °C vollständig ohne Heizstab“ (success) | „gedeckt“ · Die Wärmepumpe deckt den Bivalenzpunkt −7 °C vollständig ohne Heizstab. | gleich |
| Nennwärmeleistung A−7/W35 | kW | 13,2 | 13,2 | 13,2 kW | gleich |
| Gebäudeheizlast bei −16 °C | kW | 15,9 | 15,9 | 15,9 kW | gleich |
| WP-Leistung A−16/W50 | kW | ≈ 12,4 | ≈ 12,4 | ≈ 12,4 kW | gleich |
| Heizstab-Anteil bei −16 °C | kW | ≈ 3,6 (Stufe 2) | ≈ 3,6; Hinweis „rechnerisch Stufe 2“ | ≈ 3,6 kW (Stufe 2) | gleich |
| Taktungsrisiko | – | nein | nein | nein | gleich |
| Alternative Aeroblock | – | keine | keine | keine | gleich |
| Heizflächen-Bewertung | – | „Absenkung prüfen“ (70 → 50 °C = 20 K, warn) | „Von 70 °C auf 50 °C sind 20 K Absenkung — das ist deutlich …“ (warn); Zusatzposition „Absenkversuch vor Angebotslegung“ (objektabhängig) | „Heizflächen: Absenkung prüfen. Von 70 °C auf 50 °C sind 20 K Absenkung, das ist deutlich. Vor dem Angebot einen Absenkversuch durchführen oder die Heizflächen r…“ (warn) | gleich |
| Zusatzpositionen | – | WES 500 l · Erweiterungsmodul 2. Heizkreis · Absenkversuch vor Angebotslegung (objektabhängig) · Kühlfunktion optional · Schallgutachten (45 dB(A)) | WES 500 l · Erweiterungsmodul 2. Heizkreis (erforderlich) · Absenkversuch (objektabhängig) · Kühlfunktion optional · Schallgutachten (optional) | WES 500 l · Energiespeicher [erforderlich] · Erweiterungsmodul 2. Heizkreis + gemischte Pumpengruppe [erforderlich] · Absenkversuch vor Angebotslegung [objektabhängig] · Kühlfunktion über Flächenheizung (optional) [optional] · Schallgutachten / Schallschutzmaßnahmen [optional] | gleich |

### A4 · Sole/Wasser, kleiner Neubau ohne Verbrauch, Taktungsrisiko
Eingaben: Bestand Gas mit Verbrauch 0 (Feld geleert), Einheit kWh, Kesseltyp Niedertemperatur (90 %) · Wohnfläche 60 m² · Wohneinheiten 1 · Personen 2 · Gebäudehülle Pauschale „Neubau GEG (30 W/m²)“ · Heizflächen Fußbodenheizung, 1 Verteiler (Erdgeschoss 6 Kreise) · Wärmequelle Sole/Wasser · Speicher getrennt WAS 200 l + WES 70 l · Norm-Außentemperatur −14 °C · Bivalenzpunkt −7 °C · Ziel-Vorlauf 35 °C (automatisch).
Rechenweg: kein Verbrauchsansatz · Gebäude 60 × 30 ÷ 1 000 = 1,80 kW · + 0,4 kW = 2,20 kW · Lastanteil 0,7586 → 1,67 kW · WGB 8 bei −7/W35 8,6 × 1,15 = 9,89 ≥ 1,67 · Taktung: 1,67 < 1,7 × 1,3 = 2,21 → ja; Reihenpuffer: 2,20 < 2,21 → ja.

| Kennzahl | Einheit | erwartet (Formel) | alt (gemessen) | neu (gemessen) | Status |
|---|---|---|---|---|---|
| Gasverbrauchsansatz | – | „—“ (kein Verbrauch) | „—“ (kein Verbrauch) | – | gleich |
| Gebäudeansatz (Pauschale 30 W/m²) | kW | 1,8 | 1,8 | 1,8 kW | gleich |
| Plausibilität | – | „Nur Gebäudeansatz genutzt“ | „Nur Gebäudeansatz genutzt“ | Nur Gebäudeansatz genutzt (kein Verbrauch angegeben), plus 0,4 kW Warmwasser für 2 Personen. | gleich |
| Auslegungswert Heizlast (inkl. 0,4 kW Warmwasser) | kW | 2,2 | 2,2 | 2,2 kW | gleich |
| Empfohlenes Gerät | – | Weishaupt Geoblock WGB 8 · Modulation 1,7–8,6 kW · 400 V | Weishaupt Geoblock WGB 8 · Modulation 1,7–8,6 kW · 400 V | Weishaupt Geoblock WGB 8 · Modulation 1,7–8,6 kW · 400 V · Sole/Wasser · Badge „Vorschlag“ | gleich |
| Gebäudelast bei −7 °C | kW | 1,7 | 1,7 | 1,7 kW | gleich |
| WP-Leistung A−7/W35 | kW | ≈ 8,6 | ≈ 8,6 | ≈ 8,6 kW | gleich |
| Deckung am Bivalenzpunkt | % | 100 | 100 | 100 % | gleich |
| Status Bivalenzpunkt | – | gedeckt (ohne Heizstab) | „✓ … vollständig ohne Heizstab“ (success) | „gedeckt“ · Die Wärmepumpe deckt den Bivalenzpunkt −7 °C vollständig ohne Heizstab. | gleich |
| Sole-Hinweis | – | monovalenter Betrieb, Heizlast 2,2 kW gedeckt | „Monovalenter Betrieb — die Sole/Wasser-Wärmepumpe deckt die volle Heizlast von 2,2 kW …“ (success) | „Monovalenter Betrieb. Die Sole/Wasser-Wärmepumpe deckt die volle Heizlast von 2,2 kW auch an den kältesten Tagen, da die…“ (ok) | gleich |
| Taktungsrisiko | – | ja (1,7 kW nahe 1,7 kW Modulationsminimum) | ja: „↻ Taktungsrisiko im Teillastbetrieb: Last am Bivalenzpunkt (1,7 kW) nahe der unteren Modulationsgrenze (1,7 kW)“ | ja: „Taktungsrisiko im Teillastbetrieb. Last am Bivalenzpunkt (1,7 kW) nahe der unteren Modulationsgrenze (1,7 kW). Ausreichendes Anlagenvolumen bzw. Pufferspeicher vorsehen.“ | gleich |
| Zusatzpositionen | – | Erdwärmesonde · Soleverteiler · WAS 200 l · WES 70 l · zusätzliches Anlagenvolumen (Reihenpuffer) · Kühlfunktion optional · Schallgutachten (39 dB(A)) | Erdwärmesonde · Soleverteiler · WAS 200 l · WES 70 l · zusätzliches Anlagenvolumen (erforderlich) · Kühlfunktion optional · Schallgutachten (optional) | Erdwärmesonde inkl. Bohrung [erforderlich] · Soleverteiler + Solekreis-Zubehör [erforderlich] · WAS 200 l · Trinkwasserspeicher [erforderlich] · WES 70 l · Energiespeicher [erforderlich] · Zusätzliches Anlagenvolumen (Reihenpuffer) [erforderlich] · Kühlfunktion über Flächenheizung (optional) [optional] · Schallgutachten / Schallschutzmaßnahmen [optional] | gleich |

### A5 · wie A1, Norm-Außentemperatur −12,7 °C (nur Neufassung)
Eingaben wie A1, Norm-Außentemperatur −12,7 °C (Vorgabe der Neufassung, Region Oranienburg; in der Altfassung nicht wählbar, Entscheidung Fabian 2026-10-25, 06). Angelegt 2026-10-25 in Schritt 5.
Rechenweg: Heizlast wie A1 = 12,88 kW · Lastanteil (15 + 7) ÷ (15 + 12,7) = 0,7942 → 10,23 kW · WEB 7/10: 8,05 < 10,23 → WEB 9/14: 10,75 ≥ 10,23 · bei −12,7 °C: 8,55 + (2,3 ÷ 8) × 0,8 = 8,78 kW → Heizstab 12,88 − 8,78 = 4,10 kW.

| Kennzahl | Einheit | erwartet (Formel) | alt (gemessen) | neu (gemessen 2026-10-25) | Status |
|---|---|---|---|---|---|
| Ölverbrauchsansatz | kW | 14,2 | – | 14,2 kW | gleich |
| Gebäudeansatz (Pauschale 90 W/m²) | kW | 10,8 | – | 10,8 kW | gleich |
| Auslegungswert Heizlast (inkl. 0,4 kW Warmwasser) | kW | 12,9 | – | 12,9 kW | gleich |
| Empfohlenes Gerät | – | Weishaupt Evoblock WEB 9/14 · Modulation 1,7–14,0 kW · 230 V | – | Weishaupt Evoblock WEB 9/14 · Modulation 1,7–14,0 kW · 230 V · Luft/Wasser | gleich |
| Gebäudelast bei −7 °C | kW | 10,2 | – | 10,2 kW | gleich |
| WP-Leistung A−7/W50 | kW | ≈ 9,4 | – | ≈ 9,4 kW | gleich |
| Deckung am Bivalenzpunkt | % | 91 | – | 91 % | gleich |
| Status Bivalenzpunkt | – | knapp (Heizstab 0,88 kW ≤ 15 % von 14 kW) | – | „knapp“ | gleich |
| Nennwärmeleistung A−7/W35 | kW | 9,5 | – | 9,5 kW | gleich |
| Gebäudeheizlast bei −12,7 °C | kW | 12,9 | – | 12,9 kW | gleich |
| WP-Leistung A−12,7/W50 | kW | ≈ 8,8 | – | ≈ 8,8 kW | gleich |
| Heizstab-Anteil bei −12,7 °C | kW | ≈ 4,1 (Stufe 2) | – | ≈ 4,1 kW (Stufe 2) | gleich |
| Taktungsrisiko | – | nein | – | nein | gleich |
| Alternative Aeroblock | – | WAB 14, ≈ 10,9 kW, 100 % Deckung | – | Alternative: Weishaupt Aeroblock WAB 14 | gleich |
| Zusatzpositionen | – | Öltankentsorgung · WAS 300 l · WES 200 l · Schallgutachten (optional) | – | Öltankentsorgung inkl. Restölentsorgung [erforderlich] · WAS 300 l · Trinkwasserspeicher [erforderlich] · WES 200 l · Energiespeicher [erforderlich] · Schallgutachten / Schallschutzmaßnahmen [optional] | gleich |

### A6 · Mehrfamilienhaus, Kaskade (Erweiterung Schritt 10, nur Neufassung)
Erweiterung „Kaskaden-Planung“ [Entscheidung Fabian 2026-10-25, 06]: Aufstellung „Kaskade“, n baugleiche Geräte; Anzahl automatisch = kleinste Zahl ≥ 2, deren Leistung am Bivalenzpunkt × Faktor (1,15 bzw. 1,0 streng) die Gebäudelast deckt, oder von Hand (2–8). Gesamtleistung = n × Einzelleistung an beiden Betriebspunkten; Status mit n × maximaler Leistung; Taktung gegen das Modulationsminimum eines Geräts; Zusatzposition „Kaskadenregelung und hydraulische Einbindung“. Methode ist ein [VORSCHLAG]; Kaskadierbarkeit, Höchstzahl und Regelung je Baureihe laut Weishaupt-Planungsunterlagen prüfen (07/25). Einzelgerät-Pfad und A1–A5 unverändert.
Eingaben: Bestand Gas 60 000 kWh/a (kWh), Kesseltyp Niedertemperatur (90 %) · Wohnfläche 600 m² · Wohneinheiten 8 · Personen 16 · Pauschale „Teilsaniert / 90er Jahre (70 W/m²)“ · Heizkörper, 40 Stück, Zweirohr · Norm-Außentemperatur −12,7 °C · Bivalenzpunkt −7 °C · Vorlauf 50 °C (automatisch) · WAS 600 l + WES 910 l · Aufstellung Kaskade, Gerät WWP LA 20/30-A R, Anzahl automatisch.
Rechenweg: Verbrauch 60 000 × 0,90 ÷ 1 800 = 30,00 kW · Gebäude 600 × 70 ÷ 1 000 = 42,00 kW · Abweichung 33,3 % → 50/50 → 36,00 kW · + 16 × 0,2 = 39,20 kW · Lastanteil 22 ÷ 27,7 = 0,7942 → 31,13 kW · je Gerät bei −7/W50: (20,0 + 19,5) ÷ 2 = 19,75 kW · n = max(2; aufgerundet(31,13 ÷ (19,75 × 1,15) = 1,37)) = 2 · gesamt 39,50 kW · bei −12,7 °C je Gerät 16,75 + (2,3 ÷ 8) × 3,0 = 17,61 kW, gesamt 35,23 kW → Heizstab 39,20 − 35,23 = 3,98 kW · Taktung 31,13 ≥ 1,3 × 6,8 → nein.

| Kennzahl | Einheit | erwartet (Formel) | alt (gemessen) | neu (gemessen) | Status |
|---|---|---|---|---|---|
| Gasverbrauchsansatz | kW | 30,0 | – | 30,0 kW | gleich |
| Gebäudeansatz (Pauschale 70 W/m²) | kW | 42,0 | – | 42,0 kW | gleich |
| Plausibilität | – | „plausibel (33 % Abweichung)“, Mittel beider Wege | – | Verbrauch und Gebäudehülle plausibel (33 % Abweichung). Auslegungswert = Mittel beider Wege, plus 3,2 kW Warmwasser für 16 Personen. | gleich |
| Auslegungswert Heizlast (inkl. 3,2 kW Warmwasser) | kW | 39,2 | – | 39,2 kW | gleich |
| Empfohlene Baugröße | – | 2 × Weishaupt WWP LA 20/30-A R · Modulation 6,8–61,2 kW gesamt · 2 × 400 V | – | 2 × Weishaupt WWP LA 20/30-A R · Kaskade aus 2 baugleichen Geräten (Anzahl automatisch) · Modulation 6,8–61,2 kW gesamt · 2 × 400 V · Luft/Wasser | gleich |
| Gebäudelast bei −7 °C | kW | 31,1 | – | 31,1 kW | gleich |
| WP-Leistung A−7/W50 (gesamt) | kW | ≈ 39,5 | – | ≈ 39,5 kW | gleich |
| Deckung am Bivalenzpunkt | % | 100 | – | 100 % | gleich |
| Status Bivalenzpunkt | – | gedeckt | – | „gedeckt“ · Die Wärmepumpen-Kaskade deckt den Bivalenzpunkt −7 °C vollständig ohne Heizstab. | gleich |
| Nennwärmeleistung A−7/W35 | kW | 2 × 20,0 | – | 2 × 20,0 kW | gleich |
| Gebäudeheizlast bei −12,7 °C | kW | 39,2 | – | 39,2 kW | gleich |
| WP-Leistung A−12,7/W50 (gesamt) | kW | ≈ 35,2 | – | ≈ 35,2 kW | gleich |
| Heizstab-Anteil bei −12,7 °C | kW | ≈ 4,0 | – | ≈ 4,0 kW | gleich |
| Taktungsrisiko | – | nein | – | nein | gleich |
| Alternative Aeroblock | – | keine (bei Kaskade) | – | keine | gleich |
| Zusatzpositionen | – | Kaskadenregelung und hydraulische Einbindung (2 Geräte) · WAS 600 l · WES 910 l · Schallgutachten (54 dB(A), optional) | – | Kaskadenregelung und hydraulische Einbindung (2 Geräte) [erforderlich] · WAS 600 l · Trinkwasserspeicher [erforderlich] · WES 910 l · Energiespeicher [erforderlich] · Schallgutachten / Schallschutzmaßnahmen [optional] | gleich |

## foerderung.html · F1–F6

Altfassung: `alt/foerderung_2026-10-26.html` · Feldliste: `tests/feldliste-foerderung.md`.
Angelegt 2026-10-26 (Schritt 2). Spalte „erwartet (Formel)“ aus dem Rechenkern der Altfassung nachgerechnet (Python, gleiche Stufenlogik und Rundung: Euro auf ganze Zahl, Prozent ganzzahlig; Anzeige mit Tausenderpunkt „15.680 €“). Spalte „alt“ wird in Schritt 3 in der unveränderten Altfassung gemessen, „neu“ in Schritt 5.
Der Rechenkern hängt vom Tag der Antragstellung ab (Standard: heute); jeder Fall setzt das Datum deshalb ausdrücklich. Kontrolle gegen die IKZ-Tabelle aus dem Code-Kommentar: EFH 2026 → 22 400 / 19 600 / 15 680 / 12 880 €, 3 WE bis 30 T€ → 27 067 €, 8 WE ohne Selbstnutzung → 35 700 € (alle mit dem Python-Nachbau bestätigt).
Stufentabelle (bei allen Fällen gleich, Antrag ab): 21.07.2026 16 % / 28 000 € · 01.02.2027 12 % / 27 250 € · 01.08.2027 8 % / 26 500 € · 01.02.2028 4 % / 25 750 € · 01.08.2028 entfällt / 25 000 € · 01.02.2029 / 24 250 € · 01.08.2029 / 23 500 € · 01.02.2030 / 22 750 € · 01.08.2030 / 22 000 €.

Messung „alt“ am 2026-10-27 in der unveränderten Altfassung (`foerderung.html` im Git, byteidentisch mit `alt/foerderung_2026-10-26.html`; die Kopie in `alt/` lädt `theme.css` und `logo.png` nicht, weil beide im Hauptordner liegen). Ablauf: lokaler Server (Port 8731), headless Chrome über das Debug-Protokoll, Eingaben per Skript in die Felder gesetzt (Zahlenfelder mit `input`, Datum und Haken mit `change`, Auswahl per Klick auf die Buttons), Ergebnisse aus der Anzeige abgelesen, PDFs `tests/pdf/foerderung_F1_alt.pdf` … `F6_alt.pdf` (A4, 12 mm Rand, ohne Kopf-/Fußzeile des Browsers; die Altfassung hat keine Druckansicht, gedruckt wird die ganze Seite mit Formular, 3–4 Seiten). **Alle Zuschüsse und Bausteine stimmen mit „erwartet (Formel)“ überein.** Die Stufentabelle erscheint in allen Fällen mit den neun Zeilen von oben; die Zeile des Antragsdatums trägt „← jetzt“.

**Befund aus der Messung (Altfassung):**
1. F5 zeigt „noch 140 Tage“ statt der kalendarischen 139: Die Altfassung teilt die Millisekunden-Differenz zweier lokaler Mitternachten durch 86 400 000 und rundet auf; zwischen 15.09.2027 und 01.02.2028 liegt die Umstellung von Sommer- auf Winterzeit (eine Stunde mehr), also 139,04 → 140. Fälle ohne Zeitumstellung dazwischen (F1–F4, F6: 26.10.2026 liegt nach der Umstellung am 25.10.2026) sind nicht betroffen. Keine Wirkung auf den Zuschuss. Vorschlag für die Neufassung: Tage kalendarisch zählen (UTC-Mitternacht), dann 139; das wäre eine begründete Abweichung in Schritt 5, Entscheidung Fabian.

### F1 · Einfamilienhaus, Öl, mittleres Einkommen (IKZ-Spalte 15 680 €)
Eingaben: Antrag 26.10.2026 · 1 WE · Kosten 30 000 € · selbst bewohnt · Ölheizung Baujahr 2005 · Einkommen 45 000 €, kein Kind.
Rechenweg: Stufen 0 · Höchstbetrag 28 000 € · förderfähig min(30 000; 28 000) = 28 000 € · Grund 30 % + Klima 16 % (Öl) + Einkommen 10 % (45 000 ≤ 50 000) = 56 % < 70 % · 28 000 × 56 % = 15 680 €.

| Kennzahl | Einheit | erwartet (Formel) | alt (gemessen) | neu (gemessen) | Status |
|---|---|---|---|---|---|
| Voraussichtlicher Zuschuss | € | 15 680 | 15.680 € („von 28.000 € förderfähigen Kosten“) | | |
| Förderhöchstbetrag | € | 28 000 | 28.000 € | | |
| Förderfähige Kosten | € | 28 000 | 28.000 € | | |
| Grundförderung | % | 30 | 30 % | | |
| Klimageschwindigkeitsbonus | % | 16 | 16 % | | |
| Einkommensbonus | % | 10 | 10 % | | |
| Fördersatz | % | 56 (nicht gedeckelt, Grenze 70) | 56 % (Zeile „Fördersatz“ ohne Deckel-Zusatz; Erklärung: „Obergrenze von 70 % ist damit nicht erreicht“) | | |
| Hinweis Kosten | – | „2.000 € bleiben unberücksichtigt“ | „Ihre Kosten liegen über dem Förderhöchstbetrag. 2.000 € bleiben unberücksichtigt.“ | | |
| Frist | – | noch 98 Tage bis 31.01.2027; ab 01.02.2027 Klimabonus 12 %, Höchstbetrag 27 250 € | „noch 98 Tage, bis zum 31.01.2027. Ab dem 01.02.2027 sinken der Klimageschwindigkeitsbonus auf 12 % und der Höchstbetrag der ersten Wohneinheit auf 27.250 €.“ · Tabelle: 21.07.2026 ← jetzt | | |
| Alter der Anlage | Jahre | rund 21 | „· rund 21 Jahre alt“ | | |

### F2 · Einfamilienhaus, Gas 16 Jahre (kein Klimabonus), Kind, unterste Einkommensstufe (IKZ-Spalte 19 600 €)
Eingaben: Antrag 26.10.2026 · 1 WE · Kosten 40 000 € · selbst bewohnt · Gasheizung Baujahr 2010 · Einkommen 38 000 €, Kind ja.
Rechenweg: Alter 16 < 20 → kein Klimabonus · anzusetzend 38 000 − 10 000 = 28 000 ≤ 30 000 → 40 %, Obergrenze 80 % · 30 + 0 + 40 = 70 % < 80 % · förderfähig 28 000 € · 28 000 × 70 % = 19 600 €.

| Kennzahl | Einheit | erwartet (Formel) | alt (gemessen) | neu (gemessen) | Status |
|---|---|---|---|---|---|
| Voraussichtlicher Zuschuss | € | 19 600 | 19.600 € (von 28.000 €) | | |
| Förderhöchstbetrag | € | 28 000 | 28.000 € | | |
| Förderfähige Kosten | € | 28 000 | 28.000 € | | |
| Klimageschwindigkeitsbonus | % | 0 (Gas 16 Jahre; Text „ab Baujahr 2006 oder älter wäre er möglich“) | keine Zeile im Ergebnis; Erklärung: „rund 16 Jahre alt … mindestens 20 Jahre — er greift hier nicht. Ab Baujahr 2006 oder älter wäre er möglich.“ | | |
| Einkommensbonus | % | 40 (anzusetzend 28 000 €) | 40 % (Erklärung: „Aus 38.000 € werden damit 28.000 €“) | | |
| Fördersatz | % | 70 (Grenze 80, nicht gedeckelt) | 70 % (ohne Deckel-Zusatz) | | |
| Hinweis Kosten | – | „12.000 € bleiben unberücksichtigt“ | „12.000 € bleiben unberücksichtigt.“ | | |
| Frist | – | noch 98 Tage bis 31.01.2027; ab 01.02.2027 sinkt nur der Höchstbetrag auf 27 250 € | „noch 98 Tage, bis zum 31.01.2027. Ab dem 01.02.2027 sinkt der Höchstbetrag der ersten Wohneinheit auf 27.250 €.“ | | |

### F3 · Drei Wohneinheiten, Kohle, niedriges Einkommen, Deckel 80 % (IKZ 3 WE → 27 067 €)
Eingaben: Antrag 26.10.2026 · 3 WE · Kosten 70 000 € · selbst bewohnt · Kohle/Holz Baujahr 1995 · Einkommen 25 000 €, kein Kind.
Rechenweg: Höchstbetrag 28 000 + 2 × 15 000 = 58 000 € · förderfähig 58 000 € · Anteil je WE 19 333,33 € · 30 + 16 + 40 = 86 % > 80 % → 80 % · selbst 19 333,33 × 80 % = 15 466,67 € · übrige 2 × 19 333,33 × 30 % = 11 600 € · Summe 27 066,67 → 27 067 €.

| Kennzahl | Einheit | erwartet (Formel) | alt (gemessen) | neu (gemessen) | Status |
|---|---|---|---|---|---|
| Voraussichtlicher Zuschuss | € | 27 067 | 27.067 € (von 58.000 €) | | |
| Förderhöchstbetrag | € | 58 000 | 58.000 € | | |
| Förderfähige Kosten | € | 58 000 | 58.000 € | | |
| Anteil je Wohneinheit | € | 19 333 | 19.333 € (Erklärung und Rechnung) | | |
| Fördersatz | % | 80 (gedeckelt auf 80 %, Summe 86) | „Fördersatz (gedeckelt auf 80 %)“ 80 %; Rechnung: Summe 86 %, „Obergrenze 80 % ← gilt“ | | |
| Zuschuss selbst bewohnte WE | € | 15 467 | 15.467 € (Rechnung „19.333 € × 80 %“) | | |
| Zuschuss übrige 2 WE | € | 11 600 | 11.600 € (Rechnung „19.333 € × 30 %“) | | |
| Hinweis Kosten | – | „12.000 € bleiben unberücksichtigt“ | „12.000 € bleiben unberücksichtigt.“ | | |

### F4 · Acht Wohneinheiten, vermietet (IKZ 8 WE ohne Selbstnutzung → 35 700 €)
Eingaben: Antrag 26.10.2026 · 8 WE · Kosten 150 000 € · vermietet · Gasheizung Baujahr 1998 · (Einkommen ohne Wirkung).
Rechenweg: Höchstbetrag 28 000 + 5 × 15 000 + 2 × 8 000 = 119 000 € · förderfähig 119 000 € · Anteil 14 875 € · nur Grundförderung 30 % · 8 × 14 875 × 30 % = 35 700 €.

| Kennzahl | Einheit | erwartet (Formel) | alt (gemessen) | neu (gemessen) | Status |
|---|---|---|---|---|---|
| Voraussichtlicher Zuschuss | € | 35 700 | 35.700 € (von 119.000 €) | | |
| Förderhöchstbetrag | € | 119 000 | 119.000 € | | |
| Förderfähige Kosten | € | 119 000 | 119.000 € | | |
| Fördersatz (ohne Selbstnutzung) | % | 30 | „Fördersatz (ohne Selbstnutzung)“ 30 % | | |
| Karte Einkommen | – | ausgeblendet | ausgeblendet (Schritt 3 `display:none`) | | |
| Hinweis Kosten | – | „31.000 € bleiben unberücksichtigt“ | „31.000 € bleiben unberücksichtigt.“ | | |
| Frist | – | noch 98 Tage; ab 01.02.2027 sinkt nur der Höchstbetrag auf 27 250 € (kein Klimabonus) | „noch 98 Tage … sinkt der Höchstbetrag der ersten Wohneinheit auf 27.250 €.“ | | |

### F5 · Antrag 2027 nach zwei Absenkungen, zwei WE, Gas 21 Jahre, EU-Haken sichtbar und nicht gesetzt
Eingaben: Antrag 15.09.2027 · 2 WE · Kosten 50 000 € · selbst bewohnt · Gasheizung Baujahr 2006 · Einkommen 45 000 €, kein Kind · „made in EU“ sichtbar, nicht angehakt.
Rechenweg: Stufen 2 (01.02.2027, 01.08.2027) · Klima 16 − 8 = 8 % · Höchstbetrag 1. WE 28 000 − 1 500 = 26 500 € · gesamt 41 500 € · förderfähig 41 500 € · Anteil 20 750 € · Grund 15 % (ab 2027 ohne EU) + 8 + 10 = 33 % · selbst 20 750 × 33 % = 6 847,5 € · übrige 20 750 × 15 % = 3 112,5 € · Summe 9 960 €.
Kontrollwert nach Anhaken „made in EU“: Grund 30 % → 48 % → 9 960 + 6 225 = 16 185 €.

| Kennzahl | Einheit | erwartet (Formel) | alt (gemessen) | neu (gemessen) | Status |
|---|---|---|---|---|---|
| Voraussichtlicher Zuschuss | € | 9 960 | 9.960 € (von 41.500 €) | | |
| Förderhöchstbetrag | € | 41 500 | 41.500 € | | |
| Grundförderung | % | 15 | 15 % (EU-Haken sichtbar, nicht gesetzt) | | |
| Klimageschwindigkeitsbonus | % | 8 (Gas 21 Jahre; „bereits 2-mal abgesenkt“) | 8 % (Erklärung: „rund 21 Jahre alt … greift mit 8 %. Der Bonus ist bereits 2-mal abgesenkt worden“) | | |
| Einkommensbonus | % | 10 | 10 % | | |
| Fördersatz | % | 33 | 33 % | | |
| Zuschuss selbst bewohnte WE / übrige WE | € | 6 848 / 3 113 (Anzeige gerundet) | 6.848 € / 3.113 € (Rechnung „20.750 € × 33 %“ / „× 15 %“) | | |
| Zuschuss mit EU-Haken (Kontrollwert) | € | 16 185 | 16.185 € (Grund 30 %, Satz 48 %) | | |
| Frist | – | noch 139 Tage bis 31.01.2028; ab 01.02.2028 Klimabonus 4 %, Höchstbetrag 25 750 € | **„noch 140 Tage“** (Befund 1: Zeitumstellung), bis zum 31.01.2028; „ab dem 01.02.2028 sinken der Klimageschwindigkeitsbonus auf 4 % und der Höchstbetrag der ersten Wohneinheit auf 25.750 €“ | | |
| Stufentabelle, markierte Zeile | – | 01.08.2027 · 8 % · 26 500 € („jetzt“) | „01.08.2027 ← jetzt · 8 % · 26.500 €“ | | |

### F6 · Erneuerbare Bestandsanlage nach 2008, Steuerjahre einzeln, Kosten unter dem Höchstbetrag
Eingaben: Antrag 26.10.2026 · 1 WE · Kosten 20 000 € · selbst bewohnt · Erneuerbare Heizung Baujahr 2012 · Steuerjahre einzeln 44 000 € (2023) und 46 000 € (2024) → Mittel 45 000 € · kein Kind.
Rechenweg: kein Klimabonus (EE) · Einkommen 45 000 → 10 % · 30 + 0 + 10 = 40 % · förderfähig min(20 000; 28 000) = 20 000 € · 20 000 × 40 % = 8 000 €. EE-Hinweis sichtbar: Anlage nach 01.01.2008, Förderung soll ab 2027 entfallen.

| Kennzahl | Einheit | erwartet (Formel) | alt (gemessen) | neu (gemessen) | Status |
|---|---|---|---|---|---|
| Voraussichtlicher Zuschuss | € | 8 000 | 8.000 € (von 20.000 €) | | |
| Förderfähige Kosten | € | 20 000 (voller Betrag) | 20.000 € (Erklärung: „liegen darunter … vollen Betrag“) | | |
| Einkommen (Mittel aus beiden Jahren) | € | 45 000 | Hauptfeld 45000 nach Eingabe 44000 / 46000 | | |
| Fördersatz | % | 40 | 40 % (Grund 30 % + Einkommensbonus 10 %, keine Klima-Zeile) | | |
| Hinweis Kosten | – | keiner | keiner (ausgeblendet) | | |
| EE-Hinweis | – | sichtbar: „nach dem 1. Januar 2008 … ab 2027 entfallen“ | sichtbar: „… wurde 2012 eingebaut. Für solche Anlagen soll die Förderung beim Austausch ab 2027 entfallen — betroffen sind alle, die nach dem 1. Januar 2008 eingebaut wurden. Heute wird Ihr Tausch noch gefördert …“ | | |

## heizkoerper.html · H1–H6

Altfassung: `alt/heizkoerper_2026-11-08.html` (byteidentisch mit dem Git-Stand f754073) · Feldliste: `tests/feldliste-heizkoerper.md`.
Angelegt 2026-11-08 (Schritt 2). Spalte „erwartet (Formel)“: der Rechenkern der Altfassung (Datentabellen, `DB()`, `dTlog`, `leistung`, `requiredVL`, `erreichbareRaumtemp`, Korb-Funktionen, Konsolen, `gaebBezeichnung`) wurde unverändert aus der HTML-Datei ausgeschnitten und in Node 24 ausgeführt; `runFit` (Filter, Lagerabgleich, Sortierung) und die Raumspeicherung sind dort Zeile für Zeile nachgebaut (Skript `erwartet.mjs`, Scratchpad der Sitzung, nicht im Repo). Spalte „alt“ wird in Schritt 3 in der unveränderten Altfassung gemessen, „neu“ in Schritt 5.
Gemeinsame Vorgaben: `lager.json` geladen (Stand 2026-08-12, 14 Positionen, 14 Stück; lokaler Server, weil `fetch` lokal nicht geht) · Norm-Außentemperatur −12,7 °C in jedem Fall ausdrücklich gesetzt (Altfassung-Standard −14) · Reserve ausdrücklich gesetzt (Altfassung-Standard 15 %) · Kunde/Projekt „BV Muster“ · Werte wie die Altfassung sie anzeigt (Punkt-Dezimale „24.7“, „%“ ohne Leerzeichen); die Neufassung formatiert mit Komma, das gilt als Darstellungs-, nicht als Rechenabweichung.
Ablauf je Fall: Eingaben setzen → Ansicht wählen → „Passende Heizkörper suchen“ → Zeile anklicken (Auswahlkarte) → ggf. Konsole/Haken → „+ Raum speichern“ → Bericht drucken als `tests/pdf/heizkoerper_Hn_alt.pdf` (Projekt mit nur diesem Raum). Zusätzlich H-Gesamt: alle sechs Räume in einem Projekt, Bestellübersicht im Bericht und GAEB-Datei (`tests/pdf/heizkoerper_HG_alt.pdf`, X83 nur geprüft, nicht abgelegt).
ΔT<sub>log</sub> 50/40/20 = 24,7 K (Faktor zu 75/65/20: (24,66/49,83)<sup>n</sup> ≈ 0,40 bei n = 1,30, Firmengehirn 05).

Messung „alt“ am 2026-11-08 in der unveränderten Altfassung (`heizkoerper.html` im Git, byteidentisch mit `alt/heizkoerper_2026-11-08.html`; die Kopie in `alt/` lädt `theme.css` nicht, weil sie im Hauptordner liegt). Ablauf: lokaler Server (Port 8731, dadurch lädt `lager.json`: 14 Positionen, Stand 2026-08-12), headless Chrome über das Debug-Protokoll, je Fall frischer Browser-Speicher; Hersteller-Tab und Ansicht per Klick, Zahlenfelder mit `input`/`change`, Ausführung/Bauform/Anschluss als Radio mit `change`, Auswahl über `zeigeBezeichnung`, Korb über `hkKorbAdd`/`hkKorbMenge`, Standkonsole über `setKonsolenArt`, dann „+ Raum speichern“; Ergebnisse aus der Anzeige und aus dem gespeicherten Raumobjekt abgelesen, Bericht über `druckeBericht()` erzeugt und als PDF gedruckt (A4, 12 mm Rand, ohne Kopf-/Fußzeile des Browsers): `tests/pdf/heizkoerper_H1_alt.pdf` … `H6_alt.pdf` (je 1 Seite, H5 2 Seiten; Kunde/Projekt im Kopf „–“, weil je Fall nur der Raum gesetzt wurde) und `heizkoerper_HG_alt.pdf` (alle sechs Räume, „BV Muster“, 4 Seiten). Skript `messen_alt.mjs` im Scratchpad der Sitzung, nicht im Repo. **Alle Treffer, Reihenfolgen, Leistungen, Deckungen, Bestellnummern, Konsolen, Lager-Gründe, nötigen Vorlauftemperaturen, erreichbaren Raumtemperaturen, Notizen, Bestellübersichten und GAEB-Positionen stimmen mit „erwartet (Formel)“ überein.**

Messung „neu“ am 2026-11-09 (Schritt 5) in der Neufassung (`heizkoerper.html`, design.css 2.1.3): gleiche Methode wie „alt“ (lokaler Server mit `lager.json`, headless Chrome, je Fall frischer Browser-Speicher, Eingaben per Skript in die neuen Felder, Auswahl/Korb/Konsole über die Funktionen der Seite, „+ Raum speichern“), PDFs `tests/pdf/heizkoerper_H1_neu.pdf` … `H6_neu.pdf` und `heizkoerper_HG_neu.pdf` mit erzwungenem Hell-Schema (A4, 12 mm Rand). **Die gespeicherten Raumobjekte aller sechs Fälle sind identisch mit der Altfassung** (JSON-Vergleich ohne Zeitstempel; H6 nur Zahlenformat der Notiz laut 06); Treffer, Reihenfolgen, Lager-Gründe, Bestellnummern, Konsolen, Ansicht ②/③, Bestellübersicht und GAEB-Struktur samt Kurztexten stimmen überein; keine Konsolenfehler. Darstellungsunterschiede (keine Rechenabweichung): Komma-Dezimale und schmales Leerzeichen als Tausender („1 106 W“, „24,7 K“, „−12,7 °C“), Maße „600 × 1400 mm“, Status als Badges („101 % · knapp“, „115 % · gedeckt“, „Empfohlen“, „aus Lager · 1 St. (rechts)“), Hinweistexte ohne Emoji und ohne „Tab ②“. PDFs: Einzelberichte 2 Seiten (alt 1), weil Rahmendaten, Raumkarte und Bestelltabellen nach der Berichtsvorlage eigene Karten sind, die nicht umbrechen; H-Gesamt 4 Seiten (wie alt). Gewollte Abweichung (Fabian, 2026-11-09, 06): Im Multi-Modus zeigen Ansicht ② und ③ die Summe aus den gerundeten Einzelwerten des Korbs (2 481 W, wie Korb, Raumliste und Bericht; alt 2 480 W ungerundet) und die Stückzahl im Text („3 Heizkörper“, alt „2“ = Korbeinträge). Die Rechnung dahinter (nötige gemeinsame VL, Gleichgewicht) ist unverändert.

Vorabmessung „neu“ am 2026-11-09 (Schritt 4, ohne PDFs): gleiche Eingaben per Skript in die Neufassung (ids laut Feldliste), gespeicherte Raumobjekte aller sechs Fälle identisch mit der Altfassung (JSON-Vergleich ohne Zeitstempel; H6 nur Zahlenformat der Notiz laut 06), GAEB-Struktur und Kurztexte gleich, keine Konsolenfehler. Spalte „neu“ wird in Schritt 5 förmlich gefüllt.

**Befunde aus der Messung (Altfassung, keine Rechenfehler, aber für die Neufassung zu klären):**
1. Multi-Modus, Ansicht ② und ③: Die Ergebnisbox sagt „Summe der 2 Heizkörper“, obwohl drei Stück im Raum sind (sie zählt Korbeinträge, nicht Stückzahlen). Vorschlag Neufassung: Stückzahl nennen („3 Heizkörper“).
2. Multi-Modus: Die Tabelle in Ansicht ②/③ zeigt „Summe aktuell 2480 W“, Korb, Raumliste, gespeicherter Raum und Bericht „2481 W“. Die Tabelle summiert ungerundete Leistungen (905,4 × 2 + 670,9 = 2 480,x), der Korb gerundete (905 × 2 + 671). Vorschlag Neufassung: einheitlich die gerundeten Einzelwerte summieren, wie Korb und Bericht; das wäre eine begründete Abweichung von 1 W in dieser einen Tabellenzeile, Entscheidung Fabian in Schritt 5.
3. Bericht: Überschrift „Zu bestellende Wandkonsolen“ enthält auch Standkonsolen und Blenden. Vorschlag Neufassung: „Konsolen und Zubehör“.
4. Bericht-Kopf trägt den alten Text-Slogan „Sanitär · Heizung · Klimatechnik“ und bei leerem Feld „Kunde/Projekt: –“; in der Neufassung Logo B mit Slogan im Logo (Berichtsvorlage).
5. Multi-Modus, Ansicht ①: Die Spalten „Deckung“ und „Passung“ bewerten jeden Heizkörper einzeln (19 %, „knapp“), obwohl kombiniert wird; alle Zeilen heißen „knapp“. Vorschlag Neufassung: im Multi-Modus Deckung ohne Badge oder Badge „Baustein“.
6. Leerzustand Ansicht ① verweist auf „Tab ②“; Neufassung: „Ansicht ‚Nötige Vorlauftemperatur‘“.
7. Spalte „Lagerplatz“ in „Aus dem Lager entnehmen“ zeigt „–“, weil `lager.json` kein `notiz`-Feld führt; bleibt so (Datenfrage, nicht Tool).

### H1 · Kermi therm-x2 Profil, K & V, kein Lagertreffer (Raum „Wohnzimmer“, EG)
Eingaben: Kermi therm-x2 · Bauform Profil · Heizlast 1 100 W · Reserve 0 % · Raum 20 °C · Norm-AT −12,7 °C · 50/40 °C · max. 600 × 1400 mm · Typen 11/22/33 · K & V · Anschluss rechts · ein Heizkörper · Ansicht ① · Empfehlung (erste Zeile) anklicken · Wandkonsole · speichern.
Rechenweg: Bedarf 1 100 W · ΔT<sub>log</sub> 24,7 K · Typ 33 BH 500: 1 944 W/m × 1,4 m = 2 722 W × (24,66/49,83)<sup>1,2801</sup> = 1 106 W ≥ 1 100 W.

| Kennzahl | Einheit | erwartet (Formel) | alt (gemessen) | neu (gemessen) | Status |
|---|---|---|---|---|---|
| Auslegungsbasis | – | 1100 W · +0% → 1100 W · 20 °C · 50/40 °C · ΔT log 24.7 K · 600×1400 mm | Heizlast 1100 W · + 0% Reserve 1100 W · Raumtemp. 20 °C · System 50/40 °C · ΔT log 24.7 K · Max. Fläche 600×1400 mm (Feld ΔT<sub>log</sub> 24.7) | Kennzahlen: Heizlast 1 100 W · Bedarf 1 100 W (inkl. 0 % Reserve) · Raumtemperatur 20 °C · System VL/RL 50/40 °C (Spreizung 10 K) · ΔT<sub>log</sub> 24,7 K · Max. Fläche 600 × 1 400 mm | gleich (Darstellung) |
| Treffer | Anzahl | 6 (Typ 33: 500×1400 K/V, 600×1300 K/V, 600×1400 K/V) | „6 Treffer · Ausführung K/V“ | „6 Treffer · Ausführung K/V“ | gleich |
| Reihenfolge | – | 1. Typ 33 K 500×1400 · 2. Typ 33 V 500×1400 · 3. K 600×1300 · 4. V 600×1300 · 5. K 600×1400 · 6. V 600×1400 | wie erwartet: K 500×1400 (Empfehlung), V 500×1400, K 600×1300, V 600×1300, K 600×1400, V 600×1400 | wie alt: K 500×1400 (Empfohlen), V 500×1400, K 600×1300, V 600×1300, K 600×1400, V 600×1400 | gleich |
| Empfehlung | – | Typ 33 K 500×1400, BT 155 · 1106 W · 101% · knapp | Typ 33 K „Empfehlung“ · 500 · 1400 · 155 · 1106 · 101% · knapp | Typ 33 K „Empfohlen“ · 500 · 1400 · 155 · 1 106 W · „101 % · knapp“ | gleich |
| Weitere Zeilen | W / % | 600×1300: 1176 W · 107% · knapp · 600×1400: 1266 W · 115% · ok | 600×1300: 1176 · 107% · knapp · 600×1400: 1266 · 115% · ok | 600×1300: 1 176 W · 107 % · knapp · 600×1400: 1 266 W · 115 % · gedeckt | gleich |
| Auswahlkarte | – | therm-x2 Profil-K Typ 33 · Kermi therm-x2 Profil-K (FK0) Typ 33, 500 × 1400 mm, BT 155 mm · Nabenabstand 446 mm | therm-x2 Profil-K Typ 33 · Kermi therm-x2 Profil-K (FK0) Typ 33, 500 × 1400 mm, BT 155 mm · Nabenabstand 446 mm · Leistung 1106 W | therm-x2 Profil-K Typ 33 · Kermi therm-x2 Profil-K (FK0) Typ 33, 500 × 1400 mm, BT 155 mm · 1 106 W bei aktuellen Bedingungen · 101 % · knapp · Nabenabstand 446 mm | gleich |
| Bestellnummer | – | FK0330514 | FK0330514 | FK0330514 | gleich |
| Konsole (Wand) | – | Schnellmontageset Art. 1815977 (2 Konsolen) | „Art. 1815977 – Schnellmontageset WA 30 mm weiß (2 Konsolen)“ | „Art. 1815977 – Schnellmontageset WA 30 mm weiß (2 Konsolen)“ (Segment Wandkonsole/Standkonsole) | gleich |
| Lagertreffer | Anzahl | 0; Hinweis „Weitere Lagerware vorhanden, aber hier nicht einsetzbar“ mit 14 Positionen | 0 markierte Zeilen; Hinweis „Weitere Lagerware vorhanden, aber hier nicht einsetzbar“ mit 14 Positionen | 0 markierte Zeilen; Hinweis „Weitere Lagerware vorhanden, aber hier nicht einsetzbar“ mit 14 Positionen | gleich |
| Lager-Gründe (Auszug) | – | Typ 22 V 600×600: andere Anschlussseite (Lager links, gebraucht rechts) · Typ 33 V 600×1000: deckt die Heizlast nicht (905 W < 1100 W) · Typ 33 V 605×1005: andere Bauform (Lager plan, ausgelegt profil) · Typ 22 V 300×1600: passt nicht in die Fläche (max. 600×1400 mm) · Typ 33 V 600×1200: deckt nicht (1086 W < 1100 W) · Typ 22 K 500×700: deckt nicht (408 W) | alle 14 Gründe wortgleich (Reihenfolge der Lagerdatei) | alle 14 Gründe wortgleich; Maße mit Leerzeichen („600 × 1400 mm“ statt „600×1400 mm“) | gleich (Darstellung) |
| Gespeicherter Raum | – | Leistung 1106 W · Deckung 101% · nötige VL 49.9 °C (Spreizung 10 K) · erreichbare Raumtemp. 20.1 °C bei 1102 W · QN 2722 W | leistung 1106 · deckung 101 · noetigeVL „49.9“ · erreichbareTemp „20.1“ · ggLeistung 1102 · qn 2722 · konsole setNr 1815977, konsolen 2 | Raumobjekt identisch mit alt (JSON-Vergleich ohne Zeitstempel): leistung 1106 · deckung 101 · noetigeVL „49.9“ · erreichbareTemp „20.1“ · ggLeistung 1102 · qn 2722 · konsole 1815977 | gleich |
| GAEB-Kurztext (ohne Artikelstamm) | – | KERMI X2 Profil-K Typ33 BH500x155x1400mm QN2722, weiß, 10 bar, m. Abdeckung | KERMI X2 Profil-K Typ33 BH500x155x1400mm QN2722, weiß, 10 bar, m. Abdeckung (Kurztext in H-Gesamt) | KERMI X2 Profil-K Typ33 BH500x155x1400mm QN2722, weiß, 10 bar, m. Abdeckung (H-Gesamt) | gleich |

### H2 · Kermi Profil, Lagertreffer bevorzugt, „aus Lager“ (Raum „Küche“, ohne Etage)
Eingaben: wie H1, aber Heizlast 900 W · Reserve 0 % · Ansicht ① · Empfehlung anklicken (Lagertreffer, Haken „aus dem Lager entnehmen“ bleibt) · Wandkonsole · speichern.
Rechenweg: Typ 33 BH 600 BL 1000: 2 236 W × (24,66/49,83)<sup>1,2866</sup> = 905 W ≥ 900 W; Lagerposition Typ 33 V 600×1000 rechts, 1 Stück.

| Kennzahl | Einheit | erwartet (Formel) | alt (gemessen) | neu (gemessen) | Status |
|---|---|---|---|---|---|
| Treffer | Anzahl | 20, davon 4 Lagertreffer (oben, markiert) | „20 Treffer · Ausführung K/V“, 4 Zeilen als Lagertreffer markiert | „20 Treffer · Ausführung K/V“, 4 Lagerzeilen oben | gleich |
| Reihenfolge (erste 6) | – | 1. Typ 33 V 600×1000 📦 (rechts) · 2. Typ 33 K 500×1200 📦 · 3. Typ 33 V 500×1200 📦 (rechts) · 4. Typ 33 V 600×1200 📦 (rechts) · 5. Typ 33 K 400×1400 · 6. Typ 33 V 400×1400 | wie erwartet: V 600×1000 📦 (rechts) Empfehlung · K 500×1200 📦 · V 500×1200 📦 (rechts) · V 600×1200 📦 (rechts) · K 400×1400 · V 400×1400; danach K 600×1000, K/V 500×1300, K 600×1100 | wie alt: V 600×1000 „aus Lager · 1 St. (rechts)“ Empfohlen · K 500×1200 aus Lager · V 500×1200 aus Lager (rechts) · V 600×1200 aus Lager (rechts) · K 400×1400 · V 400×1400 | gleich |
| Empfehlung | – | Typ 33 V 600×1000, BT 155 · 905 W · 101% · knapp · Lager 1 St. (rechts) | Typ 33 V „Empfehlung“ „📦 Lager: 1 St. (rechts)“ · 600 · 1000 · 155 · 905 · 101% · knapp | Typ 33 V „Empfohlen“ „aus Lager · 1 St. (rechts)“ · 600 · 1000 · 155 · 905 W · 101 % · knapp | gleich |
| Lager-Hinweis | – | „4 passende Heizkörper liegen auf Lager und werden bevorzugt vorgeschlagen“ | „📦 4 passende Heizkörper liegen auf Lager und werden bevorzugt vorgeschlagen (oben markiert). Kein Neukauf nötig, wenn die Deckung passt.“ | „4 passende Heizkörper liegen auf Lager und werden bevorzugt vorgeschlagen (oben markiert). Kein Neukauf nötig, wenn die Deckung passt. Lagerstand ist ein Vorschlag, kein Nachweis.“ | gleich (Darstellung) |
| Lager nicht nutzbar | Anzahl | 10 (Anschlussseite 1 · Bauform plan 2 · Fläche 4 · deckt nicht 3: 500×1000 790 W, 22 K 500×700 408 W, 22 K 500×800 466 W) | 10 Positionen mit den erwarteten Gründen | 10 Positionen, Gründe wie alt | gleich |
| Auswahlkarte | – | therm-x2 Profil-V Typ 33 (R) · Kermi therm-x2 Profil-V (FTV) Typ 33, 600 × 1000 mm, BT 155 mm, Anschluss rechts · Nabenabstand 546 mm · „Liegt im Lager – aus dem Lager entnehmen“ | therm-x2 Profil-V Typ 33 (R) · Kermi therm-x2 Profil-V (FTV) Typ 33, 600 × 1000 mm, BT 155 mm, Anschluss rechts · Nabenabstand 546 mm · „📦 Liegt im Lager – aus dem Lager entnehmen. Haken weg, wenn er trotzdem bestellt werden soll.“ | therm-x2 Profil-V Typ 33 (R) · … Anschluss rechts · 905 W · 101 % · knapp · Nabenabstand 546 mm · Haken „aus dem Lager entnehmen“ gesetzt | gleich |
| Bestellnummer | – | FTV330601001R1K | FTV330601001R1K | FTV330601001R1K | gleich |
| Konsole (Wand) | – | Schnellmontageset Art. 1815979 (2 Konsolen) | „Art. 1815979 – Schnellmontageset WA 30 mm weiß (2 Konsolen)“ | „Art. 1815979 – Schnellmontageset WA 30 mm weiß (2 Konsolen)“ | gleich |
| Gespeicherter Raum | – | 905 W · 101% · nötige VL 49.9 °C · erreichbare Raumtemp. 20.1 °C bei 902 W · QN 2236 W · Kennzeichen „aus Lager“, Anschluss rechts | leistung 905 · deckung 101 · noetigeVL „49.9“ · erreichbareTemp „20.1“ · ggLeistung 902 · qn 2236 · ausLager true · anschluss „rechts“ | Raumobjekt identisch mit alt: 905 · 101 · „49.9“ · „20.1“ · 902 · qn 2236 · ausLager true · anschluss „rechts“ | gleich |
| Bericht | – | Raum unter „Aus dem Lager entnehmen“ (1 Stück), nicht unter „Zu bestellende Heizkörper“; Konsolen 1× 1815979 | Raumkarte mit „📦 aus Lager“; in H-Gesamt unter „Aus dem Lager entnehmen“ (1×, Lagerplatz „–“), nicht unter „Zu bestellen“; Konsolen 1815979 mitgezählt | Raumkarte mit Badge „aus Lager“; in H-Gesamt unter „Aus dem Lager entnehmen“ (1 ×, Lagerplatz „–“); Konsolen 1815979 mitgezählt | gleich |

### H3 · Kermi Plan, nur Ventil links, Bestandstyp 22 ohne Treffer, Reserve 10 %, Standkonsole, Lagertreffer links (Raum „Bad“, 1. OG)
Eingaben: Kermi therm-x2 · Bauform Plan · Heizlast 900 W · Reserve 10 % · Raum 20 °C · Norm-AT −12,7 °C · 50/40 °C · max. 605 × 1205 mm · Typen 11/22/33 · nur Ventil (FTV) · Anschluss links · Bestand-Typ 22 · Ansicht ① · Empfehlung anklicken · Standkonsole wählen · speichern.
Rechenweg: Bedarf 990 W · Typ 33 Plan BH 605 BL 1105: 2 212,55 W/m × 1,105 m = 2 445 W × (24,66/49,83)<sup>1,28</sup> = 994 W ≥ 990 W · Typ 22 Plan 605×1205: 1 829 W × 0,4036 = 738 W < 990 W → kein Bestandstyp-Treffer.

| Kennzahl | Einheit | erwartet (Formel) | alt (gemessen) | neu (gemessen) | Status |
|---|---|---|---|---|---|
| Auslegungsbasis | – | 900 W · +10% → 990 W · ΔT log 24.7 K · 605×1205 mm | 900 W · + 10% Reserve 990 W · ΔT log 24.7 K · 605×1205 mm | Heizlast 900 W · Bedarf 990 W (inkl. 10 % Reserve) · ΔT<sub>log</sub> 24,7 K · 605 × 1 205 mm | gleich (Darstellung) |
| Treffer | Anzahl | 2: 1. Typ 33 V 605×1105 📦 (links) 994 W · 110% · ok · 2. Typ 33 V 605×1205 1084 W · 120% · ok | „2 Treffer · Ausführung V (Ventil)“: Typ 33 V „Empfehlung“ „📦 Lager: 1 St. (links)“ 605 · 1105 · 155 · 994 · 110% · ok · Typ 33 V 605 · 1205 · 155 · 1084 · 120% · ok | „2 Treffer · Ausführung V (Ventil)“: Typ 33 V Empfohlen „aus Lager · 1 St. (links)“ 605 · 1105 · 155 · 994 W · 110 % · gedeckt · Typ 33 V 605 · 1205 · 155 · 1 084 W · 120 % · gedeckt | gleich |
| Bestand-Hinweis | – | „Bestand Typ 22 gewählt, aber kein Typ 22 deckt die Heizlast in der Fläche“ (Ausweich-Hinweis) | „Bestand Typ 22 gewählt, aber kein Typ 22 deckt die Heizlast in der Fläche. Empfehlung weicht daher auf einen anderen Typ aus – hier sind Umbaumaßnahmen (Konsolen/Bautiefe) zu erwarten.“ | „Bestand Typ 22 gewählt, aber kein Typ 22 deckt die Heizlast in der Fläche. Die Empfehlung weicht auf einen anderen Typ aus; Umbaumaßnahmen (Konsolen, Bautiefe) sind zu erwarten.“ | gleich (Darstellung) |
| Lager nicht nutzbar (Auszug) | – | Typ 33 V 605×1005: andere Anschlussseite (Lager rechts, gebraucht links) · Typ 33 V 505×1805: passt nicht in die Fläche (max. 605×1205 mm) · alle Profil-Positionen: andere Bauform (Lager profil, ausgelegt plan) – 13 Positionen | 13 Positionen, Gründe wie erwartet (605×1005: andere Anschlussseite (Lager rechts, gebraucht links); 505×1805: Fläche; Profil-Positionen: andere Bauform (Lager profil, ausgelegt plan)) | 13 Positionen, Gründe wie alt | gleich |
| Auswahlkarte | – | therm-x2 Plan-V Typ 33 (L) · Kermi therm-x2 Plan-V (PTV) Typ 33, 605 × 1105 mm, BT 155 mm, Anschluss links · Nabenabstand 551 mm | therm-x2 Plan-V Typ 33 (L) · Kermi therm-x2 Plan-V (PTV) Typ 33, 605 × 1105 mm, BT 155 mm, Anschluss links · Nabenabstand 551 mm · Leistung 994 W | therm-x2 Plan-V Typ 33 (L) · … Anschluss links · 994 W · 110 % · gedeckt · Nabenabstand 551 mm | gleich |
| Bestellnummer | – | PTV330601101L1K | PTV330601101L1K | PTV330601101L1K | gleich |
| Konsole (Stand) | – | Standkonsole innenliegend Art. ZB01380002 (2 Stück) + Fertigboden-Blende Art. ZB00290001 (2 Stück) | „Art. ZB01380002 – Standkonsole innenliegend (2 Stück) + Art. ZB00290001 – Fertigboden-Blende (2 Stück)“ | Segment auf „Standkonsole“: „Art. ZB01380002 – Standkonsole innenliegend (2 Stück) + Art. ZB00290001 – Fertigboden-Blende (2 Stück)“ | gleich |
| Gespeicherter Raum | – | 994 W · Deckung 110% · nötige VL 49.9 °C · erreichbare Raumtemp. 21.2 °C bei 932 W · QN 2445 W · aus Lager, Anschluss links | leistung 994 · deckung 110 · noetigeVL „49.9“ · erreichbareTemp „21.2“ · ggLeistung 932 · qn 2445 · ausLager true · anschluss „links“ · konsole art stand, ZB01380002 ×2, Blende ZB00290001 ×2 | Raumobjekt identisch mit alt: 994 · 110 · „49.9“ · „21.2“ · 932 · qn 2445 · ausLager true · anschluss „links“ · konsole stand ZB01380002 ×2, Blende ×2 | gleich |
| GAEB-Kurztext | – | KERMI X2 Plan-V Typ33 BH605x155x1105mm QN2445, weiß, 10 bar, m. Abdeckung | KERMI X2 Plan-V Typ33 BH605x155x1105mm QN2445, weiß, 10 bar, m. Abdeckung | KERMI X2 Plan-V Typ33 BH605x155x1105mm QN2445, weiß, 10 bar, m. Abdeckung | gleich |

### H4 · Kermi Verteo-Profil, Typ 20/21/22, 45/35 °C, Raum 22 °C, Ansicht ② (Raum „Flur“, EG)
Eingaben: Hersteller Kermi Verteo (hebt max. BH/BL automatisch auf 2400/800) · danach max. 2000 × 800 mm setzen · Bauform Profil · Typ 10 abwählen · Heizlast 700 W · Reserve 0 % · Raum 22 °C · Norm-AT −12,7 °C · 45/35 °C · Ansicht ① Empfehlung anklicken · Ansicht ② ablesen · speichern.
Rechenweg: ΔT<sub>log</sub>(45/35/22) = 17,5 K · Verteo Typ 22 BH 1800 BL 800: 2 957 W × (17,53/49,83)<sup>1,34</sup> = 729 W ≥ 700 W · Typ 22 2000×800: 3 172 W → 782 W.

| Kennzahl | Einheit | erwartet (Formel) | alt (gemessen) | neu (gemessen) | Status |
|---|---|---|---|---|---|
| Auslegungsbasis | – | 700 W · 22 °C · 45/35 °C · ΔT log 17.5 K · 2000×800 mm | 700 W · 22 °C · 45/35 °C · ΔT log 17.5 K · 2000×800 mm (nach Herstellerwechsel stand 2400×1400 im Feld, danach gesetzt) | 700 W · 22 °C · 45/35 °C · ΔT<sub>log</sub> 17,5 K · 2 000 × 800 mm (nach Herstellerwechsel stand 2 400 × 1 400 im Feld, danach gesetzt – Verhalten wie alt) | gleich (Darstellung) |
| Treffer ① | Anzahl | 2: 1. Verteo Typ 22 1800×800 BT 100 · 729 W · 104% · knapp · 2. Verteo Typ 22 2000×800 · 782 W · 112% · ok | „2 Treffer“: Verteo Typ 22 „Empfehlung“ 1800 · 800 · 100 · 729 · 104% · knapp · Verteo Typ 22 2000 · 800 · 100 · 782 · 112% · ok | „2 Treffer“: Verteo Typ 22 Empfohlen 1800 · 800 · 100 · 729 W · 104 % · knapp · Verteo Typ 22 2000 · 800 · 100 · 782 W · 112 % · gedeckt | gleich |
| Ansicht ② erste Zeilen | °C | Typ 22 2000×800 → 43.6 · Typ 22 1800×800 → 44.5 · Typ 22 2000×700 → 45.2 · Typ 22 1600×800 → 46.0 (alle „WP-tauglich“) | 2000×800 → 43.6 / 33.6 · 1800×800 → 44.5 / 34.5 · 2000×700 → 45.2 / 35.2 · 1600×800 → 46.0 / 36.0 · 1800×700 → 46.2 · Typ 21 2000×800 → 46.9; alle „WP-tauglich“ | 2000×800 → 43,6 / 33,6 °C · 1800×800 → 44,5 / 34,5 · 2000×700 → 45,2 / 35,2 · 1600×800 → 46,0 / 36,0 · 1800×700 → 46,2; alle „WP-tauglich“ | gleich |
| Auswahlkarte | – | Verteo-Profil Typ 22 · Kermi Verteo-Profil (FSN) Typ 22, 1800 × 800 mm, BT 100 mm, weiß RAL 9016 | Verteo-Profil Typ 22 · Kermi Verteo-Profil (FSN) Typ 22, 1800 × 800 mm, BT 100 mm, weiß RAL 9016 · vertikaler Flachheizkörper · Leistung 729 W | Verteo-Profil Typ 22 · Kermi Verteo-Profil (FSN) Typ 22, 1800 × 800 mm, BT 100 mm, weiß RAL 9016 · 729 W · 104 % · knapp | gleich |
| Bestellnummer | – | FSN221808 | FSN221808 | FSN221808 | gleich |
| Konsole | – | Verteo Wandkonsolen-Set lang Art. 1816042 (1 Set) | „🔩 Konsolen: Art. 1816042 – Verteo Wandkonsolen-Set lang, weiß (1 Set)“ | „Art. 1816042 – Verteo Wandkonsolen-Set lang, weiß (1 Set)“ | gleich |
| Gespeicherter Raum | – | 729 W · 104% · nötige VL 44.5 °C (Spreizung 10 K) · erreichbare Raumtemp. 22.4 °C bei 708 W · QN 2957 W | leistung 729 · deckung 104 · noetigeVL „44.5“ · erreichbareTemp „22.4“ · ggLeistung 708 · qn 2957 · konsole art verteo 1816042 | Raumobjekt identisch mit alt: 729 · 104 · „44.5“ · „22.4“ · 708 · qn 2957 · konsole verteo 1816042 | gleich |
| GAEB-Kurztext | – | Klartext (kein therm-x2-Format): Kermi Verteo-Profil (FSN) Typ 22, 1800 × 800 mm, BT 100 mm, weiß RAL 9016 | Kermi Verteo-Profil (FSN) Typ 22, 1800 × 800 mm, BT 100 mm, weiß RAL 9016 | Kermi Verteo-Profil (FSN) Typ 22, 1800 × 800 mm, BT 100 mm, weiß RAL 9016 | gleich |

### H5 · Mehrere Heizkörper je Raum, Kermi Profil nur Kompakt (Raum „Wohnküche“, EG)
Eingaben: Kermi therm-x2 Profil · Heizlast 2 400 W · Reserve 0 % · Raum 20 °C · Norm-AT −12,7 °C · 50/40 °C · max. 600 × 1000 mm · nur Kompakt (FK0) · „Mehrere Heizkörper“ · Ansicht ① · Typ 33 K 600×1000 anklicken → „+ zum Raum hinzufügen“, Menge auf 2 · Typ 22 K 600×1000 anklicken → hinzufügen · Ansicht ② und ③ ablesen · speichern.
Rechenweg: Typ 33 600×1000 = 905 W, Typ 22 600×1000 = 1 666 W × 0,4027 = 671 W · Summe 2 × 905 + 671 = 2 481 W ≥ 2 400 W · Deckung 2 481 / 2 400 = 103 %.

| Kennzahl | Einheit | erwartet (Formel) | alt (gemessen) | neu (gemessen) | Status |
|---|---|---|---|---|---|
| Treffer ① (Multi) | Anzahl | 98 (alle, die in die Fläche passen); erste Zeilen Lager: Typ 22 K 500×800 📦 466 W · Typ 22 K 500×700 📦 408 W · dann Typ 33 K 600×1000 905 W · 600×900 814 W · 500×1000 790 W | „98 Treffer · Ausführung K (Kompakt)“, 40 Zeilen angezeigt, 2 Lagerzeilen oben: Typ 22 K „Empfehlung“ 📦 500×800 466 · 19% · knapp · Typ 22 K 📦 500×700 408 · 17% · knapp · dann Typ 33 K 600×1000 905 · 38% · 600×900 814 · 500×1000 790; Hinweis „📦 2 Lager-Heizkörper passen in die Fläche …“ | „98 Treffer · Ausführung K (Kompakt) · Kombination“, 40 Zeilen, 2 Lagerzeilen oben (Typ 22 K 500×800 466 W · 19 % · knapp; 500×700 408 W), dann 33 K 600×1000 905 W · 600×900 814 W · 500×1000 790 W; Hinweis „2 Lager-Heizkörper passen in die Fläche …“ | gleich |
| Korb | – | 2× therm-x2 Profil-K Typ 33 600×1000 (FK0330610, 2×905 = 1810 W) + 1× Profil-K Typ 22 600×1000 (FK0220610, 671 W) | „Heizkörper in diesem Raum (3)“: 2× therm-x2 Profil-K Typ 33 · 600×1000 mm · FK0330610 · 2×905 = 1810 W · 1× Profil-K Typ 22 · 600×1000 mm · FK0220610 · 671 W | „Heizkörper in diesem Raum 3“: 2 × therm-x2 Profil-K Typ 33 · 600 × 1000 mm · FK0330610 · 2 × 905 = 1 810 W · 1 × Profil-K Typ 22 · FK0220610 · 671 W (Stepper) | gleich |
| Deckung Korb | W / % | 2481 W von 2400 W · 103% · „Heizlast gedeckt“ | „✓ Heizlast gedeckt (2481 W von 2400 W nötig)“ | Badge „gedeckt · 103 %“ · „Heizlast gedeckt (2 481 W von 2 400 W nötig)“ · Balken | gleich |
| Ansicht ② (Summe) | °C | nötige gemeinsame VL 49.4 (RL 39.4, Spreizung 10 K) · „WP-tauglich“ | „… gemeinsame Vorlauftemperatur von 49.4 °C nötig (Rücklauf 39.4 °C, Spreizung 10 K). ✓ Das ist WP-tauglich (≤ 55 °C).“ Tabelle: Summe aktuell (50/40 °C) **2480 W**, Bedarf 2400 W (Befund 2) | „… gemeinsame Vorlauftemperatur von 49,4 °C nötig (Rücklauf 39,4 °C, Spreizung 10 K). Das ist WP-tauglich (≤ 55 °C).“ Tabelle: Summe aktuell (50/40 °C) **2 481 W**, Bedarf 2 400 W; Text „Summe der 3 Heizkörper“ | gewollte Abweichung (06, 2026-11-09): Anzeige-Summe aus gerundeten Einzelwerten wie Korb und Bericht, Stückzahl statt Korbeinträge |
| Ansicht ③ (Summe) | °C | erreichbare Raumtemperatur 20.4 (+0.4 K) bei 2429 W Summen-Abgabe · „Wunschtemperatur wird gehalten“ | „… Raumtemperatur von 20.4 °C ein (+0.4 K zum Wunsch 20 °C). ✓ Die Wunschtemperatur wird gehalten.“ Tabelle: Summe bei Wunschtemp. 2480 W · Summe im Gleichgewicht (20.4 °C) 2429 W · Heizlast 2400 W | „… Raumtemperatur von 20,4 °C ein (+0,4 K zum Wunsch 20 °C). Die Wunschtemperatur wird gehalten.“ Text „Mit den 3 Heizkörpern zusammen …“; Tabelle: Summe bei Wunschtemperatur 2 481 W · Summe im Gleichgewicht (20,4 °C) 2 429 W · Heizlast 2 400 W | gewollte Abweichung (06, 2026-11-09), Gleichgewichtswerte gleich |
| Gespeicherter Raum | – | 3 HK · Σ 2481 W · Deckung 103% ✓ · nötige VL 49.4 · erreichbare Temp. 20.4 · QN 2236 / 1666 W | mehrfach · gesamtleistung 2481 · deckung 103 · gedeckt true · noetigeVL „49.4“ · erreichbareTemp „20.4“ · ggLeistung 2429 · hkliste 2× FK0330610 (905 W, qn 2236), 1× FK0220610 (671 W, qn 1666) · keine Notiz | Raumobjekt identisch mit alt: mehrfach · gesamtleistung 2481 · deckung 103 · gedeckt true · „49.4“ · „20.4“ · 2429 · hkliste 2× FK0330610 (905 W, qn 2236), 1× FK0220610 (671 W, qn 1666) | gleich |
| Konsolen | – | je HK Schnellmontageset 1815979; Bericht/GAEB summiert 3× 1815979 | je Eintrag setNr 1815979, konsolen 2; Raumliste „3 HK: 2× … Typ 33 600×1000 + … Typ 22 600×1000 · Σ 2481 W“ | je Eintrag 1815979, konsolen 2; Raumliste „3 HK: 2 × … Typ 33 600 × 1000 + … Typ 22 600 × 1000 · Σ 2 481 W … · gedeckt“ | gleich |
| Bericht | – | Zu bestellen: 2× FK0330610, 1× FK0220610 · Konsolen 3× 1815979 | PDF 2 Seiten: Raumkarte mit drei Heizkörper-Blöcken („Heizkörper 1 (2 Stück)“ …); in H-Gesamt: 2× FK0330610, 1× FK0220610, Konsolen 1815979 anteilig 3× | PDF 2 Seiten: Raumkarte mit Gesamtleistung-Badge und drei Heizkörper-Einträgen; H-Gesamt: 2 × FK0330610, 1 × FK0220610, Konsolen 1815979 anteilig 3 × | gleich |

### H6 · Zehnder Charleston, kein Treffer in ①, Ansicht ③ mit Kundenhinweis (Raum „Schlafzimmer“, DG)
Eingaben: Hersteller Zehnder (hebt max. BH auf 1800) · danach max. 600 × 1000 mm setzen · Heizlast 1 600 W · Reserve 0 % · Raum 21 °C · Norm-AT −12,7 °C · 50/40 °C · alle Säulenzahlen · Ansicht ① (leer) · Ansicht ③ · erste Zeile anklicken · speichern (Notiz wird automatisch gefüllt).
Rechenweg: ΔT<sub>log</sub>(50/40/21) = 23,6 K · 6-Säuler BH 600, 21 Glieder (966 mm ≤ 1000): 117 W × 21 = 2 457 W × (23,65/49,83)<sup>1,30</sup> = 932 W (58 % bei 21 °C) · Gleichgewicht: Heizlast(t<sub>L</sub>) = 1 600 × (t<sub>L</sub> + 12,7)/(33,7) = Abgabe(t<sub>L</sub>) → t<sub>L</sub> = 14,4 °C bei 1 287 W.

| Kennzahl | Einheit | erwartet (Formel) | alt (gemessen) | neu (gemessen) | Status |
|---|---|---|---|---|---|
| Ansicht ① | – | Leerzustand „Kein Heizkörper deckt bei 50/40 °C die Heizlast innerhalb der Fläche 600×1000 mm“; Lagerliste leer (kein Zehnder im Lager) | „Kein Heizkörper deckt bei 50/40 °C die Heizlast innerhalb der Fläche 600×1000 mm. Wechsle zu Tab ② …“; keine Lagerliste (Lager hat keine Zehnder-Position). Nach Herstellerwechsel stand 1800×1400 im Feld, danach 600×1000 gesetzt | Leerzustand „Kein Heizkörper deckt bei 50/40 °C die Heizlast innerhalb der Fläche 600 × 1 000 mm. Ansicht ‚Nötige Vorlauftemperatur‘ prüfen oder die Installationsfläche vergrößern.“; keine Lagerliste. Nach Herstellerwechsel stand 1 800 × 1 400 im Feld, danach 600 × 1 000 gesetzt | gleich (Darstellung) |
| Ansicht ③ erste Zeilen | °C | 6-Säuler 600×966 → 14.4 (−6.6 K, GG 1287 W) · 6-Säuler 550×966 → 13.4 (GG 1240 W) · 5-Säuler 600×966 → 12.3 (GG 1188 W) · 6-Säuler 500×966 → 12.3 (GG 1188 W) · 5-Säuler 550×966 → 11.3 | 6-Säuler 600 · 966 · 14.4 · −6.6 K · 1287 · 6-Säuler 550 · 966 · 13.4 · −7.6 K · 1240 · 5-Säuler 600 · 966 · 12.3 · −8.7 K · 1188 · 6-Säuler 500 · 966 · 12.3 · 1188 · 5-Säuler 550 · 11.3 · 1139 · 6-Säuler 450 · 11.1 · 1132 | 6-Säuler 600 · 966 · 14,4 °C · −6,6 K · 1 287 W · 6-Säuler 550 · 13,4 · −7,6 · 1 240 · 5-Säuler 600 · 12,3 · −8,7 · 1 188 · 6-Säuler 500 · 12,3 · 1 188 · 5-Säuler 550 · 11,3 · 1 139 | gleich |
| Bewertung | – | alle Zeilen rot mit Wert („14.4 °C“ usw.), keine „Wunsch erreicht“ | Pille je Zeile mit dem Wert („14.4 °C“ usw.), keine „Wunsch erreicht“ | Badge je Zeile „14,4 °C · nicht erreicht“ usw., keine „Wunsch erreicht“ | gleich (Darstellung) |
| Auswahlkarte | – | Charleston 6060-21 · Zehnder Charleston 6-Säuler, BH 600 mm, 21 Glieder (BL 966 mm), BT 210 mm, weiß RAL 9016 · 21 Glieder × 46 mm | Charleston 6060-21 · Zehnder Charleston 6-Säuler, BH 600 mm, 21 Glieder (BL 966 mm), BT 210 mm, weiß RAL 9016 · 21 Glieder × 46 mm · Leistung 932 W | Charleston 6060-21 · Zehnder Charleston 6-Säuler, BH 600 mm, 21 Glieder (BL 966 mm), BT 210 mm, weiß RAL 9016 · 932 W · 58 % · knapp · 21 Glieder × 46 mm | gleich |
| Bestellnummer | – | 6060-21 · weiß | „6060-21 · weiß“ | „6060-21 · weiß“ | gleich |
| Konsole | – | keine (Zehnder ohne Konsolenlogik) | keine Konsolenzeile in Auswahlkarte und Bericht | „keine Konsolenlogik für diese Baureihe“; im Bericht keine Konsolenzeile | gleich |
| Gespeicherter Raum | – | Leistung 932 W · Deckung 58% · nötige VL 62.1 °C · erreichbare Raumtemp. 14.4 °C bei 1287 W · QN 2457 W | leistung 932 · deckung 58 · noetigeVL „62.1“ · erreichbareTemp „14.4“ · ggLeistung 1287 · qn 2457 · konsole null | Raumobjekt bis auf die Notiz identisch mit alt: 932 · 58 · „62.1“ · „14.4“ · 1287 · qn 2457 · konsole null | gleich |
| Notiz (automatisch) | – | „Bitte beachten Sie: Die in diesem Raum maximal möglichen Installationsflächen reichen leider nicht aus, um Ihre Wunschtemperatur von 21 °C vollständig zu erreichen. Der gewählte Heizkörper deckt rund 80 % der benötigten 1600 Watt ab. Bei einer Norm-Außentemperatur von -12.7 °C stellt sich damit eine Raumtemperatur von etwa 14.4 °C ein. …“ (80 % = 1287 / 1600) | wortgleich; „rund 80 %“, „-12.7 °C“, „etwa 14.4 °C“; steht im Bericht als Notiz und in der Raumliste | Text wortgleich, Zahlen formatiert: „rund 80 %“, „1 600 Watt“, „−12,7 °C“, „etwa 14,4 °C“ (06, 2026-11-08) | gewollte Abweichung (Format) |

### H-Gesamt · alle sechs Räume in einem Projekt „BV Muster“ (Bericht und GAEB)
Erwartet: Bericht gruppiert nach Etagen EG (Wohnzimmer, Flur, Wohnküche), 1. OG (Bad), DG (Schlafzimmer), „Ohne Etagenangabe“ (Küche). Bestellübersicht: Zu bestellende Heizkörper 1× FK0330514 · 1× FSN221808 · 2× FK0330610 · 1× FK0220610 · 1× „6060-21 · weiß“ = 6 Stück · Aus dem Lager entnehmen 1× FTV330601001R1K (Küche), 1× PTV330601101L1K (Bad) = 2 Stück · Konsolen: 1× 1815977, 1× 1815979 (H2) + 3× 1815979 (H5) = 4× 1815979, 1× 1816042, 2× ZB01380002, 2× ZB00290001 · Bestandsheizkörper: keine. GAEB: Pos. 1 mit sechs Raum-Untergruppen, Pos. 2 Installationsmaterial (5 Artikelzeilen), Pos. 3 ohne „Arbeitszeit Monteur“ (Montagezeit leer); XML wohlgeformt.

| Kennzahl | Einheit | erwartet (Formel) | alt (gemessen) | neu (gemessen) | Status |
|---|---|---|---|---|---|
| Zu bestellende Heizkörper | Stück | 6 | „6 Stück“: 1× FK0330514 (EG · Wohnzimmer) · 1× FSN221808 (EG · Flur) · 2× FK0330610 · 1× FK0220610 (EG · Wohnküche) · 1× 6060-21 · weiß (DG · Schlafzimmer) | „6 Stück“: 1 × FK0330514 (EG · Wohnzimmer) · 1 × FSN221808 (EG · Flur) · 2 × FK0330610 · 1 × FK0220610 (EG · Wohnküche) · 1 × 6060-21 · weiß (DG · Schlafzimmer); Summenzeile „6 × Heizkörper gesamt“ | gleich |
| Aus dem Lager entnehmen | Stück | 2 | „2 Stück“: FTV … rechts (Küche), PTV … links (1. OG · Bad); Lagerplatz „–“ | „2 Stück“: FTV … rechts (Küche), PTV … links (1. OG · Bad); Lagerplatz „–“ | gleich |
| Konsolen | Zeilen / Stück | 5 Zeilen · 1 + 4 + 1 + 2 + 2 = 10 Stück | Überschrift „Zu bestellende Wandkonsolen 10 Stück“: 1× 1815977 · 4× 1815979 · 1× 1816042 · 2× ZB00290001 · 2× ZB01380002 (Befund 3) | Überschrift „Konsolen und Zubehör 10 Stück“: 1 × 1815977 · 4 × 1815979 · 1 × 1816042 · 2 × ZB00290001 · 2 × ZB01380002; Summenzeile 10 × | gleich |
| GAEB-Positionen (bis 2026-11-09; danach entfernt, 06) | – | Pos. 1: 6 Räume, 7 Items · Pos. 2: 5 Items · Pos. 3: 2 Items | wohlgeformt · Pos. 1 „Heizkörper“ mit Untergruppen EG Wohnzimmer, Küche, 1. OG Bad, EG Flur, EG Wohnküche, DG Schlafzimmer, 7 Items · Pos. 2 „Installationsmaterial“ 5 Items · Pos. 3 „Arbeits- und Serviceleistungen“ 2 Items (Entsorgungspauschale, Auftragspauschale); 14 Kurztexte wie erwartet | wohlgeformt · Pos. 1 „Heizkörper“ mit sechs Raum-Untergruppen, 7 Items · Pos. 2 „Installationsmaterial“ 5 Items · Pos. 3 2 Items; 14 Kurztexte identisch mit alt | gleich |

## bestandsaufnahme.html · B1–B3
Noch anzulegen beim Umbau des Tools (Schritt 2). Je Fall: erfasste Positionen und erzeugte `lager.json` (nur Artikel und Stückzahlen).
