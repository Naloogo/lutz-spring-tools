# Feldliste · ausdehnungsgefaess.html (Ausdehnungsgefäß Heizung)

Stand: 2026-11-10 · Erstellt von: Claude Code (Sitzung Neubau ausdehnungsgefaess) · Geprüft von Fabian: ☐ (Datum)
Zweck: Neubau nach UMBAU-ABLAUF.md (wie bad-angebot: Schritte 1–3 entfallen). Es gibt keine Altfassung; die Feldliste legt vor dem Bau fest, was das Tool abfragt, woher die Werte kommen und wie gerechnet wird. Erst nach Freigabe bauen.
Alle Beispielwerte erfunden, keine Kundendaten. Referenzfälle M1–M3 in `tests/referenzwerte.md`.

Entscheidungen von Fabian (2026-11-10, Chat): eigenes Tool mit Kachel; nur Heizung (geschlossene Anlage, Wasser ohne Frostschutz, bis 100 °C); max. Vorlauftemperatur bei Wärmepumpe 50 °C; Wasserinhalt wird nur über Rohrnetz, Heizschlangen (Fußbodenheizung) und Heizkörper geschätzt, Erzeuger und Puffer kommen aus dem Datenblatt [BESTÄTIGT, Fabian 2026-11-10; Eintrag in 06 nach Freigabe].

## Fragen je Feld
1. Geht der Wert in die Rechnung ein? Wenn nein: Ergebnis oder weg.
2. Hat er einen Standardwert aus dem Firmengehirn oder einer belegten Quelle? Dann vorbelegen.
3. Ändert ihn der Nutzer bei jedem Durchlauf? Wenn nein: „Erweitert“.
4. Folgt er aus einer anderen Eingabe? Dann berechnen und anzeigen.
5. Gehört er zu einer Gruppe? Dann gemeinsame Karte oder Zeile.

## Rechengrundlage [VORSCHLAG, Quelle belegt]
Reflex Winkelmann GmbH, „Fachgerecht planen, berechnen und ausrüsten“, 1. Auflage 2019, Druck-Nr. 9128882 / RE1953de 11-19 (Berechnung nach DIN EN 12828 und VDI 4708 Blatt 1), Seiten 7, 10 und 12 [BELEGT: PDF gelesen 2026-11-10]. Alle Formeln stehen im Code als Kommentar mit Seitenangabe.

| Schritt | Formel | Quelle |
|---|---|---|
| Statischer Druck | p<sub>st</sub> = h<sub>st</sub> ÷ 10 m/bar | Reflex S. 92 |
| Vordruck (Mindestbetriebsdruck) | p<sub>0</sub> = p<sub>st</sub> + p<sub>D</sub> + 0,2 bar, mindestens 1,0 bar; p<sub>D</sub> = 0 unter 100 °C | Reflex S. 10, 12 (Reflex-Empfehlung) |
| Sicherheitsventil, Mindestabstand | p<sub>SV</sub> ≥ p<sub>0</sub> + 1,5 bar (p<sub>SV</sub> ≤ 5 bar) bzw. + 2,0 bar (p<sub>SV</sub> > 5 bar) – nur Prüfung mit Hinweis | Reflex S. 12 |
| Enddruck | p<sub>e</sub> = p<sub>SV</sub> − 0,5 bar (p<sub>SV</sub> ≤ 5 bar) bzw. p<sub>SV</sub> − 0,1 · p<sub>SV</sub> (p<sub>SV</sub> > 5 bar) | Reflex S. 12 (Schließdruckdifferenz nach ISO 4126-1) |
| Ausdehnung | n [%] aus Tabelle „+10 °C auf t“ für die max. Vorlauftemperatur, zwischen den 10-K-Stufen linear: 10 °C 0 · 20 0,13 · 30 0,37 · 40 0,72 · 50 1,15 · 60 1,66 · 70 2,24 · 80 2,88 · 90 3,58 · 100 4,34; n<sub>R</sub> ebenso für die Rücklauftemperatur | Reflex S. 7 (reines Wasser, Fülltemperatur 10 °C) |
| Ausdehnungsvolumen | V<sub>e</sub> = n ÷ 100 × V<sub>A</sub> | Reflex S. 12 |
| Wasservorlage | V<sub>V</sub> = 0,005 × V<sub>A</sub>, mindestens 3 l (für V<sub>n</sub> > 15 l); V<sub>V</sub> ≥ 0,2 × V<sub>n</sub> (für V<sub>n</sub> ≤ 15 l) | Reflex S. 12 |
| Nennvolumen | V<sub>n</sub> = (V<sub>e</sub> + V<sub>V</sub>) × (p<sub>e</sub> + 1) ÷ (p<sub>e</sub> − p<sub>0</sub>); bei V<sub>n</sub> ≤ 15 l mit V<sub>V</sub> = 0,2 V<sub>n</sub> aufgelöst: V<sub>n</sub> = V<sub>e</sub> × k ÷ (1 − 0,2 k), k = (p<sub>e</sub> + 1) ÷ (p<sub>e</sub> − p<sub>0</sub>) | Reflex S. 10, 12; Auflösung [VORSCHLAG] |
| Gefäßwahl | nächstgrößeres Nennvolumen der Baureihe Reflex N: 8 · 12 · 18 · 25 · 35 · 50 · 80 · 100 · 140 · 200 · 250 · 300 · 400 · 500 · 600 · 800 · 1000 l, Werksvordruck 1,5 bar | Reflex „Membran-Druckausdehnungsgefäße Reflex Refix“, S. 11 [BELEGT]; Baureihe [VORSCHLAG, Frage 3] |
| Anfangsdruck (Kontrolle) | p<sub>a</sub> = (p<sub>e</sub> + 1) ÷ [1 + V<sub>e</sub> (p<sub>e</sub> + 1)(n + n<sub>R</sub>) ÷ (V<sub>n,gewählt</sub> (p<sub>0</sub> + 1) · 2n)] − 1 bar; Bedingung p<sub>a</sub> ≥ p<sub>0</sub> + 0,25 bar, sonst nächste Größe | Reflex S. 12 („0,25…0,3 bar“; 0,25 bar reproduziert die Schnellauswahltabelle S. 92 bei p<sub>0</sub> = 0,5 und 1,0 bar, s. Referenzwerte) |
| Fülldruck | p<sub>F</sub> = p<sub>a</sub> (Fülltemperatur 10 °C = tiefste Systemtemperatur) | Reflex S. 10, 11 |

Gegenprobe an der Reflex-Schnellauswahltabelle (S. 92, Heizung 90/70 °C): p<sub>SV</sub> 3 bar, p<sub>0</sub> 1,0 bar, V<sub>A</sub> 2 100 l → V<sub>n</sub> = 199,9 l, Reflex N 200 (Tabelle: N 200 bis 2 100 l) [BELEGT, Skript 2026-11-10]. In der Spalte p<sub>0</sub> 1,5 bar liegt die Tabelle enger (N 200 nur bis 1 090 l); das Tool rechnet nach der Formel und zeigt den Hinweis „maßgeblich ist die Herstellerauslegung“.

## Wasserinhalt V<sub>A</sub> (Schätzung nach Fabians Vorgabe)
V<sub>A</sub> = Rohrnetz + Fußbodenheizung + Heizkörper + Pufferspeicher + Wärmeerzeuger + Sonstiges. Alternativ „Anlageninhalt bekannt“ (direkte Eingabe, überschreibt die Schätzung).

Rohrinhalt je Meter (Volumen = π/4 × d<sub>i</sub>², d<sub>i</sub> = d − 2 s):

| Rohr | d × s | l/m | Quelle |
|---|---|---|---|
| Kupfer (Viega Sanpress) | 15 × 1,0 | 0,133 | Maße DIN EN 1057 [BELEGT: Viega-Unterlage Löschwasser nennt 15/18/22 × 1,0]; Volumen gerechnet |
| Kupfer | 18 × 1,0 | 0,201 | wie oben |
| Kupfer | 22 × 1,0 | 0,314 | wie oben |
| Temponox 1.4520 | 28 × 1,2 | 0,51 | Viega Gebrauchsanleitung Temponox 10/2021, S. 8 [BELEGT] |
| Temponox | 35 × 1,5 | 0,80 | wie oben |
| Temponox | 42 × 1,5 | 1,19 | wie oben |
| Temponox | 54 × 1,5 | 2,04 | wie oben |
| Alpex (MSV) | 16 × 2,0 · 20 × 2,0 · 26 × 3,0 | 0,113 · 0,201 · 0,314 | Wandstärken [UNGEPRÜFT: Fränkische Alpex-Datenblatt], Volumen gerechnet |
| Fußbodenheizung | 16 × 2,0 (Standard) oder 17 × 2,0 | 0,113 bzw. 0,133 | Wandstärke [UNGEPRÜFT, Frage 4], Volumen gerechnet; Rohrlänge = Fläche ÷ Verlegeabstand |

Heizkörper (Kermi Flachheizkörper): Wasserinhalt je Meter Baulänge und je wasserführender Platte, aus der Tabelle „Gewicht, Wasserinhalt“ therm-x2 Profil Hygiene (Kermi Flachheizkörper Technik, S. 28–29; Typ 20 = 2 × Typ 10, Typ 30 = 3 × Typ 10, an allen Bauhöhen nachgerechnet) [BELEGT: Kermi-PDF 2026-11-10]:

| Bauhöhe mm | 300 | 400 | 500 | 600 | 750 | 900 |
|---|---|---|---|---|---|---|
| l/m je Platte | 1,74 | 2,18 | 2,61 | 3,05 | 3,70 | 4,36 |

Zuordnung der Typen [VORSCHLAG, Frage 5]: Typ 10/11/12 = 1 Platte, Typ 20/21/22 = 2 Platten, Typ 30/33 = 3 Platten (Konvektorbleche führen kein Wasser). Zu prüfen am Kermi-Technikkatalog der Standardreihe (Portal am 2026-11-10 nicht erreichbar, 503).

Rückfallwert ohne Einzelaufnahme (nur Hinweis, keine Eingabe, Frage 6): Weishaupt nennt überschlägig 15 l/kW für Plattenheizkörper bei 55/45 °C und 25 l/kW für Fußbodenheizung bei 35/28 °C, bezogen auf den Heizwärmebedarf, Pufferinhalt zusätzlich [BELEGT: Montage- und Betriebsanleitung WAB 8/11, 83330301 1/2026-09, Kap. 5.1.1 S. 39].

## Felder

| Nr. | Feld | id | Einheit | Standard | Bereich | Pflicht | Herkunft des Werts | Gruppe | Standard / Erweitert / Ergebnis | Begründung |
|---|---|---|---|---|---|---|---|---|---|---|
| 0 | Kunde / Projekt | projekt | – | leer | – | nein | Nutzer; keine Personendaten („BV Muster“, Ort + Gewerk) | Projekt | Standard | Kopf und Druck; keine Rechenwirkung |
| 1 | Wärmeerzeuger | erzeuger | – | Wärmepumpe | Wärmepumpe / Gas- oder Ölkessel | ja | Anlage | Anlage | Standard (`.ls-seg`) | setzt nur Vorbelegungen für 2, 3 und 4; keine eigene Rechenwirkung |
| 2 | Max. Vorlauftemperatur t<sub>TR</sub> | vlMax | °C | WP 50 · Kessel 75 | 20–100 | ja | Höchste Sollwerteinstellung des Reglers; WP 50 °C [BESTÄTIGT, Fabian 2026-11-10]; Kessel 75 °C [VORSCHLAG, Frage 2] | Anlage | Standard | bestimmt n; über 100 °C nicht abgedeckt (Fehlertext) |
| 3 | Rücklauftemperatur t<sub>R</sub> | rlMax | °C | WP 40 · Kessel 65 | 10–vlMax | ja | Systemtemperatur VL/RL; 50/40 Firmenstandard | Anlage | Standard (Zeile mit 2) | bestimmt n<sub>R</sub> in der Anfangsdruck-Kontrolle |
| 4 | Ansprechdruck Sicherheitsventil p<sub>SV</sub> | pSV | bar | 3,0 | 1,5–10 | ja | Anlage; Evoblock WEB 3 bar, Aeroblock WAB 2,5 bar, WWP LA 20/30 5,4 bar [BELEGT: Weishaupt 83603801 04/2026 S. 7, 83330301 S. 53, 83335201 Kap. 9.4] | Anlage | Standard, Hinweis mit den drei Gerätewerten | bestimmt p<sub>e</sub> |
| 5 | Statische Höhe h<sub>st</sub> | hStat | m | leer | 0–50 | ja | Höhe vom Gefäß bis zum höchsten Punkt der Anlage, vor Ort gemessen | Anlage | Standard | bestimmt p<sub>st</sub>; kein Standardwert, weil objektabhängig |
| 6 | Wasserinhalt bekannt? | vaModus | – | schätzen | schätzen / bekannt | ja | – | Wasserinhalt | Standard (`.ls-seg`) | „bekannt“ blendet 7 ein und 8–15 aus |
| 7 | Anlagenwasserinhalt V<sub>A</sub> | vaBekannt | l | leer | 10–50 000 | ja (bei „bekannt“) | Inbetriebnahmeprotokoll, Wasserzähler beim Füllen | Wasserinhalt | Standard (nur bei „bekannt“) | direkte Eingabe |
| 8 | Pufferspeicher | vPuffer | l | 0 | 0–5 000 | nein | Datenblatt; Evoblock 120 l, Aeroblock 145 l integriert [BELEGT: 83603801 S. 11, 83601801] | Wasserinhalt | Standard, Hinweis | größter Einzelposten; Weishaupt: Pufferinhalt mitberücksichtigen |
| 9 | Wärmeerzeuger / Inneneinheit | vErzeuger | l | 0 | 0–2 000 | nein | Datenblatt (Wasserinhalt des Geräts) | Wasserinhalt | Erweitert | selten bekannt, Datenblattwert |
| 10 | Rohrnetz, Meter je Dimension | rohr_cu15, rohr_cu18, rohr_cu22, rohr_tx28, rohr_tx35, rohr_tx42, rohr_al16, rohr_al20, rohr_al26 | m | 0 | 0–2 000 | nein | Aufmaß vor Ort, Rohrtabelle oben | Wasserinhalt · Rohrnetz | Standard (Kupfer 15/18/22, Temponox 28/35), Erweitert (Temponox 42, Alpex) | Firmenstandard Kupfer 15/18/22, Temponox ab 28 (06); Inhalt je Zeile als Ergebnis daneben |
| 11 | Fußbodenheizung Fläche | fbhFlaeche | m² | 0 | 0–2 000 | nein | Grundriss / Aufmaß | Wasserinhalt · Heizschlangen | Standard | Rohrlänge = Fläche ÷ Verlegeabstand |
| 12 | Verlegeabstand | fbhVa | cm | 15 | 10 / 15 / 20 / 25 / 30 | nein | Verlegeplan; 15 cm üblich [VORSCHLAG] | Wasserinhalt · Heizschlangen | Standard (`.ls-seg`) | – |
| 13 | FBH-Rohr | fbhRohr | – | 16 × 2,0 | 16 × 2,0 / 17 × 2,0 | nein | Frage 4 | Wasserinhalt · Heizschlangen | Erweitert | selten anders |
| 14 | Heizkörper (Liste) | hk[] mit hkTyp, hkBh, hkAnzahl, hkBl | – / mm / Stk / mm | Typ 22 · BH 600 · 1 · 1 000 | Typ 10/11/12/20/21/22/30/33; BH 300–900; 1–50; 400–3 000 | nein | Heizkörperliste oder Projektdatei aus heizkoerper.html (Frage 7) | Wasserinhalt · Heizkörper | Standard (`.ls-items`: Zeile = Typ, Bauhöhe, Anzahl, Baulänge; Inhalt je Zeile als Ergebnis) | Fabians Vorgabe: Heizkörper einzeln schätzen |
| 15 | Sonstiges | vSonst | l | 0 | 0–5 000 | nein | z. B. Heizregister, Handtuchheizkörper nach Datenblatt | Wasserinhalt | Erweitert | Sammelposten |
| 16 | Sicherheitszuschlag Vordruck | zuschlagP0 | bar | 0,2 | 0–0,5 | nein | Reflex S. 10 (Empfehlung) | Erweitert · Rechenregeln | Erweitert | nur bei engsten Druckverhältnissen anders |
| 17 | Mindest-Vordruck | p0Min | bar | 1,0 | 0,5–2 | nein | Reflex S. 10 (Zulaufdruck der Pumpe) | Erweitert · Rechenregeln | Erweitert | Flachbau/Dachzentrale |
| 18 | Mindestabstand Anfangsdruck zu Vordruck | dPa | bar | 0,25 | 0,25–0,3 | nein | Reflex S. 12 („0,25…0,3 bar“) | Erweitert · Rechenregeln | Erweitert | Kontrolle der Gefäßwahl |
| 19 | Zulässiger Betriebsüberdruck Wärmeerzeuger | pMaxGeraet | bar | leer | 1–10 | nein | Datenblatt; WWP LA 20/30: 1,0–4,5 bar [BELEGT: 83335201 Kap. 9.3] | Erweitert · Rechenregeln | Erweitert | Hinweis (warn), wenn p<sub>e</sub> darüber liegt (Referenzfall M3) |
| 20 | Eingebautes Ausdehnungsgefäß | magEingebaut | l | WP: 18 (Evoblock) · sonst 0 | 0–100 | nein | Evoblock WEB: 18 l [BELEGT: 83603801 04/2026 S. 7]; andere Geräte: Datenblatt | Erweitert | Erweitert, Kurzzeile sichtbar | Ergebnis „reicht / reicht nicht“ als Badge; Vordruck des eingebauten Gefäßes ist auf p<sub>0</sub> einzustellen |
| 21 | Notiz | notiz | – | leer | – | nein | Nutzer | Bericht | Standard (`.ls-notiz`) | erscheint im Druck |

Konstanten im Code (mit Quelle als Kommentar, nicht veränderlich im Sinn von Fördersätzen): n-Tabelle, Rohrinhalte, Kermi-Plattenwerte, Baureihe Reflex N, Fülltemperatur 10 °C, Schließdruckdifferenz 0,5 bar / 10 %.

## Reihenfolge der Gruppen
1. Projekt (Kunde / Projekt)
2. Anlage: Wärmeerzeuger (Segment) · Max. Vorlauf / Rücklauf · Ansprechdruck Sicherheitsventil · Statische Höhe
3. Wasserinhalt: Segment „schätzen / bekannt“ · Pufferspeicher · Rohrnetz (Meter je Dimension, Summe daneben) · Heizschlangen (Fläche, Verlegeabstand) · Heizkörper (Liste) · Erweitert: Erzeuger, Sonstiges, FBH-Rohr, Temponox 42, Alpex
4. Erweitert · Rechenregeln (Zuschlag, Mindest-Vordruck, Abstand Anfangsdruck, zulässiger Betriebsdruck, eingebautes MAG) – zu, solange Standard
5. Aktionen: Primärbutton „Berechnen“, „Zurücksetzen“; Projektkarte mit Markenknopf „Neues Projekt“, Sichern/Laden (Datei), Druckknopf bei Ergebnis
6. Ergebnis, Bericht, Fuß

## Ergebnis (DESIGNREGELN 7)
- Hinweise vor dem Ergebnis (`.ls-note--warn`): p<sub>SV</sub> unter p<sub>0</sub> + 1,5 bzw. 2,0 bar („Sicherheitsventil höher wählen oder Höhe prüfen“); p<sub>e</sub> über zulässigem Betriebsüberdruck des Geräts; Vorlauf über 100 °C (Fehler, keine Rechnung).
- Kennzahlen (`.ls-kpis`): **Nennvolumen erforderlich** V<sub>n</sub> (l) mit Unterzeile „gewählt: Reflex N … l“ · **Vordruck** p<sub>0</sub> (bar) „am Gefäß einstellen, auf Typenschild eintragen“ · **Fülldruck** p<sub>F</sub> (bar) „kalt, 10 °C, entlüftet“ · **Enddruck** p<sub>e</sub> (bar) „max. Betriebsdruck vor Ansprechen SV“.
- Empfehlungskarte (`.ls-card--navy`): gewähltes Gefäß mit Badge „Vorschlag“; bei eingebautem Gefäß Badge „eingebautes Gefäß reicht“ (ok) oder „eingebautes Gefäß reicht nicht – Zusatzgefäß … l“ (warn).
- Label/Wert-Liste (`.ls-dl`): Anlagenwasserinhalt mit Aufteilung (Rohrnetz, Heizschlangen, Heizkörper, Puffer, Erzeuger, Sonstiges), Ausdehnung n und n<sub>R</sub>, V<sub>e</sub>, V<sub>V</sub>, p<sub>st</sub>, Anfangsdruck-Kontrolle.
- Rechenweg (aufklappbar, im Druck sichtbar): jede Formel mit Eingangswerten, Zwischenergebnis und Einheit, Richtwerte mit ≈.
- Bericht nach `vorlagen/bericht-vorlage.html`: Kopf, Rahmendaten, Kennzahlen, Wasserinhalt-Tabelle, Rechenweg, Notiz, Fuß „Berechnung nach DIN EN 12828 mit den Planungsregeln von Reflex (2019) · Angaben ohne Gewähr – maßgeblich sind die Herstellerunterlagen“.

## Projektdatei
`version`, `tool: "ausdehnungsgefaess"`, `exportiert`, `projekt`, alle Felder oben, Heizkörperliste als Array. Keine Altfassung, daher `FELD_MAPPING = {}`.

## Offene Fragen an Fabian
1. Feldliste und Rechenweg so freigeben („passt“)? Rechengrundlage Reflex 2019 nach DIN EN 12828 als Betriebsstandard in 06 eintragen?
2. Kessel-Vorbelegung 75/65 °C (Max. Sollwert bei Gas/Öl im Bestand) – oder ein anderer Wert?
3. Baureihe Reflex N als Vorschlagsreihe – oder ein anderer Hersteller (Flamco Flexcon, Pneumatex)? Dann Liste mit Quelle nachliefern.
4. Fußbodenheizung: welches Rohr ist bei uns Standard (16 × 2,0 oder 17 × 2,0) und welcher Verlegeabstand als Vorbelegung?
5. Kermi-Zuordnung Typ 11/12 → 1 Platte, 21/22 → 2, 33 → 3 Platten: hast du den Kermi-Technikkatalog (Standardreihe) als PDF, um die Werte zu prüfen? Bis dahin [VORSCHLAG] mit Hinweis im Tool.
6. Soll zusätzlich der Weishaupt-Überschlag (15 l/kW Heizkörper, 25 l/kW Fußbodenheizung) als Schnellschätzung aus der Heizlast angeboten werden? Du wolltest die Einzelaufnahme; ich würde ihn nur als Hinweis zeigen.
7. Erweiterung später: Heizkörperliste aus einer heizkoerper.html-Projektdatei übernehmen (Typ, BH, BL je Raum)? Nicht im ersten Stand.
