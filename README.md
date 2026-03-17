# Mitarbeiterprofil POC

Ein einfacher statischer Proof-of-Concept für ein Mitarbeiterprofil mit Radar-Chart, geeignet für GitHub Pages und Notion-Embedding.

## Überblick

Diese Seite zeigt ein Radar-Chart mit Bewertungen für verschiedene Kompetenzen eines Mitarbeiters. Die Daten können optional über URL-Parameter übergeben werden, um individuelle Profile zu generieren.

## Technologie

- Reines HTML/CSS/JavaScript
- Chart.js per CDN
- Kein Build-Prozess erforderlich

## Verwendung

Öffne `index.html` in einem Browser oder deploye auf GitHub Pages.

### URL-Parameter

Die Seite unterstützt URL-Parameter für dynamische Profile:

- `name`: Name des Mitarbeiters (z. B. `Max%20Mustermann`)
- `dimensions`: Komma-separierte Liste der Dimensionen (z. B. `Kommunikation,Teamarbeit,Fachwissen`)
- `values`: Komma-separierte Liste der Werte (0-5, z. B. `4,5,3,4,4,2`)

Beispiel-URLs:
- `index.html?name=Max%20Mustermann&dimensions=Kommunikation,Teamarbeit,Fachwissen,Eigenverantwortung,Problemlösung,Führung&values=4,5,3,4,4,2`
- `index.html?name=Anna%20Schmidt&dimensions=Kreativität,Analytik,Leadership&values=5,4,3`

Wenn Parameter fehlen oder ungültig sind, wird ein Default-Profil verwendet.

## Notion-Anbindung

Der Code ist vorbereitet für eine spätere Integration mit Notion (z. B. via API oder JSON). Die Datenstruktur ist klar getrennt, und Kommentare zeigen, wo externe Daten eingespeist werden können.
