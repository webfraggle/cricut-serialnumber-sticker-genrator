# Seriennummern-Generator für Cricut Print Then Cut – Konzept

Stand: 2026-11-05

## Zweck

Bogen mit Seriennummern-Aufklebern (z. B. `MoBaDi-ZZA-888`) für Cricut **Print Then Cut** auf A4 erzeugen.
Die App liefert zwei Dateien in exakt gleicher Größe, die in Design Space übereinandergelegt und per *Attach* kombiniert werden:

1. **SVG** – nur das Schnittraster (innere horizontale und vertikale Linien).
2. **PNG** – Druckbild mit allen Seriennummern, 1200 dpi.

## Rahmenbedingungen

- Eine einzige HTML-Datei, per Doppelklick von der Festplatte startbar, ohne Internet, ohne externe Bibliotheken.
- Papier A4, nutzbare Print-Then-Cut-Fläche vorbelegt mit 171 × 235 mm (6,75" × 9,25"), im Formular änderbar.
- Aufkleber 40 × 5 mm, ohne Abstand aneinanderstoßend.
- Immer ein volles Blatt.

## Eingaben

- Präfix (frei, enthält ggf. Trennzeichen, z. B. `MoBaDi-ZZA-`)
- Startnummer (ganzzahlig, ohne führende Nullen)
- Erweitert (mit sinnvollen Vorgaben): Aufklebergröße, nutzbare Fläche, Innenabstand (0,5 mm), Schriftart, fett, Data-Matrix an/aus.

## Raster

- Spalten = ⌊171 / 40⌋ = 4, Reihen = ⌊235 / 5⌋ = 47 → **188 Aufkleber**, Gesamtmaß **160 × 235 mm**.
- Die App zeigt das exakte Gesamtmaß in mm an (zur Eingabe in Design Space).
- Nummerierung **spaltenweise**: Spalte 1 von oben nach unten, dann Spalte 2 usw.

## SVG (Schnitt)

- `width/height` in mm, `viewBox` in mm.
- Nur innere Linien (3 vertikal, 46 horizontal), **keine Außenkante** – die Außenkontur schneidet Cricut entlang des PNG.
- Alle Linien in einem einzigen `<path>` (eine Ebene in Design Space).

## PNG (Druck)

- 1200 dpi, weißer, deckender Hintergrund (damit die Außenkontur das volle Rechteck ist), schwarze Schrift.
- Auflösung im PNG hinterlegt (pHYs-Chunk), damit Programme 1200 dpi erkennen.
- Pro Aufkleber: 0,5 mm Innenabstand rundum → 4 mm nutzbare Höhe.
  - **Links:** Text = Präfix + Nummer, linksbündig, vertikal zentriert.
  - **Rechts:** Data-Matrix (ECC200) 4 × 4 mm, Inhalt **nur die Nummer**. Bis 6 Stellen → 10 × 10 Module (0,4 mm/Modul).
- Schriftgröße einheitlich pro Blatt, automatisch maximal so groß, dass die längste Nummer des Blatts passt.

## Bedienung

- Live-Vorschau des Blatts mit eingeblendetem Schnittraster (abschaltbar), Zoom.
- Download-Buttons für SVG und PNG. Dateinamen enthalten Nummernbereich.
- Die App merkt sich (localStorage) Einstellungen und schlägt nach dem Export als nächste Startnummer `letzte Nummer + 1` vor.

## Nicht-Ziele

- Keine Hintergrundfarbe, keine Beschnittzugabe, keine Außenkante im SVG.
- Keine Teilbögen, keine führenden Nullen.
- Schnittart (Kiss-Cut) wird am Cricut eingestellt, nicht in der App.

## Eigene Entscheidungen (zur Prüfung)

- Nutzbare Fläche und Aufklebermaße sind im Formular änderbar (Vorgabe 171 × 235 / 40 × 5 mm).
- Abstand zwischen Text und Data-Matrix: 1 Modul (0,4 mm).
- Vorgabe-Schrift: Arial fett.
- Nächste Startnummer wird beim Download des PNG gespeichert.
- Hinweis: Ein 1200-dpi-Bild dieser Größe (~7559 × 11102 px) ist sehr groß; Chrome/Firefox empfohlen.
