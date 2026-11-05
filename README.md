# Seriennummern-Generator für Cricut Print Then Cut

Erzeugt Bögen mit Seriennummern-Aufklebern für **Cricut Print Then Cut** auf A4 – als zwei Dateien in exakt gleicher Größe:

1. **SVG** – das Schnittraster (nur innere horizontale und vertikale Linien)
2. **PNG** – das Druckbild mit allen Seriennummern (1200 dpi) und optionalem Data-Matrix-Code

**➜ Online nutzen:** https://webfraggle.github.io/cricut-serialnumber-sticker-genrator/

Oder `seriennummern-generator.html` herunterladen und per Doppelklick im Browser öffnen – keine Installation, keine Internetverbindung nötig.

![Beispiel-Aufkleber](docs/beispiel-aufkleber.png)

## Funktionen

- Eingabe von **Präfix** (z. B. `MoBaDi-ZZA-`) und **Startnummer**
- Standard: Aufkleber **40 × 5 mm**, **4 × 47 = 188 Stück** pro Blatt, Gesamtmaß **160 × 235 mm**
- Nummerierung **spaltenweise** (Spalte 1 von oben nach unten, dann Spalte 2 …)
- **Data-Matrix** (ECC200) rechts neben dem Text, enthält nur die Nummer
- Schriftgröße wird automatisch maximal an die längste Nummer des Blatts angepasst
- Live-**Vorschau** mit eingeblendetem Schnittraster
- Merkt sich alle Einstellungen und schlägt nach dem Export die **nächste Startnummer** vor
- PNG enthält die dpi-Angabe (pHYs), SVG ist in mm bemaßt
- Erweiterte Einstellungen: Aufklebergröße, Nutzfläche, Innenabstand, dpi, Schriftart

## Verwendung in Cricut Design Space

1. In der App Präfix und Startnummer eingeben, **SVG** und **PNG** herunterladen.
2. Beide Dateien in Design Space hochladen (PNG als Druckbild für Print Then Cut, SVG als Schnitt).
3. Beide auf das in der App angezeigte Maß setzen (Standard **160 × 235 mm**) und exakt übereinanderlegen.
4. Beide Ebenen markieren und **Anfügen (Attach)** – nicht *Flatten*, sonst wird nur die Außenkontur geschnitten.
5. Drucken, Material (Kiss-Cut) einstellen, schneiden.

Die Außenkante steckt bewusst nicht im SVG: Cricut schneidet beim Print Then Cut ohnehin die Kontur des PNG.

## Hinweise

- Die nutzbare Print-Then-Cut-Fläche ist mit 171 × 235 mm (6,75" × 9,25") vorbelegt und kann angepasst werden.
- Das 1200-dpi-PNG ist groß (ca. 7559 × 11102 px). **Chrome oder Firefox** empfohlen; Safari kann an Größenlimits scheitern.
- Einstellungen werden im Browser (localStorage) gespeichert – lokal geöffnete Datei und GitHub-Pages-Version haben getrennte Speicher.

## Technik

Eine einzige HTML-Datei ohne Abhängigkeiten. Der Data-Matrix-Encoder ist selbst implementiert (ISO/IEC 16022, ASCII-Modus) und gegen den Norm-Testvektor sowie den zxing-Decoder geprüft. Details zum Konzept: [`docs/konzept.md`](docs/konzept.md).

## Lizenz

MIT – siehe [LICENSE](LICENSE).
