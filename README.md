# Mitarbeiterprofil POC

Ein einfacher statischer Proof-of-Concept für ein Mitarbeiterprofil mit Radar-Chart, geeignet für GitHub Pages und Notion-Embedding.

## Überblick

Diese Seite zeigt ein Radar-Chart mit Bewertungen für verschiedene Kompetenzen eines Mitarbeiters. Die Daten werden über URL-Parameter übergeben. Es gibt zwei Modi:

- **Einzelprofil** – zeigt die Ausprägungen einer Person
- **Vergleichsmodus** – überlagert mehrere Profile im selben Chart, z. B. um eine Person mit einem Archetypen zu vergleichen und Entwicklungspotenziale sichtbar zu machen

## Technologie

- Reines HTML/CSS/JavaScript
- Chart.js per CDN
- Kein Build-Prozess erforderlich

## Verwendung

Öffne `index.html` in einem Browser oder deploye auf GitHub Pages.

### URL-Parameter

| Parameter    | Beschreibung |
|--------------|--------------|
| `name`       | Seitentitel / Name des Mitarbeiters |
| `dimensions` | Komma-separierte Achsenbeschriftungen |
| `values`     | Werte (0–5) pro Datensatz, komma-separiert. Mehrere Datensätze werden mit `\|` getrennt. |
| `labels`     | *(optional)* Komma-separierte Namen der Datensätze – wird in der Legende angezeigt, wenn mehr als ein Datensatz vorhanden ist |

### Modus 1: Einzelprofil

Für eine Person ohne `labels`-Parameter. Die Legende wird automatisch ausgeblendet.

```
index.html
  ?name=Anna%20Schmidt
  &dimensions=Kommunikation,Teamarbeit,Fachwissen,Eigenverantwortung,Problemlösung
  &values=4,5,3,4,2
```

### Modus 2: Profilvergleich

Mehrere Datensätze werden mit `|` in `values` getrennt und über `labels` benannt. Die Legende wird automatisch eingeblendet.

Ein typischer Anwendungsfall ist der Vergleich einer Person mit einem Ziel-Archetypen (z. B. „Senior Analyst"), um auf einen Blick zu sehen, in welchen Bereichen noch Entwicklungspotenzial besteht.

```
index.html
  ?name=Vergleich%20Anna%20vs.%20Senior%20Analyst
  &dimensions=Kommunikation,Teamarbeit,Fachwissen,Eigenverantwortung,Problemlösung
  &values=4,5,3,4,2|5,4,5,5,5
  &labels=Anna%20Schmidt,Senior%20Analyst
```

Jeder Datensatz erhält automatisch eine eigene Farbe. Es werden bis zu 4 Datensätze unterstützt.

Wenn Parameter fehlen oder ungültig sind, wird ein Default-Profil verwendet.
