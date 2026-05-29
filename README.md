# Knowly v2 KG-Viz — AI Hour Storytelling

3D-Visualisierung des Knowly v2 Knowledge Graph mit 6 narrated scenes fuer die AI Hour.

## Use

### Lokal starten

```bash
cd "missions/M-037-knowly-v2-deploy/ai-hour-viz"
python3 -m http.server 8765
# Browser: http://localhost:8765/
```

Voraussetzung: LightRAG v2 muss auf Port 9623 laufen (laeuft bereits in Docker).

**ABER:** Sobald die `kg-data.json` einmal generiert ist, ist die HTML standalone — also auch ohne laufenden LightRAG-Container nutzbar. Solange du nicht neu extrahierst.

### Daten neu generieren

Wenn der KG-Container Updates hat oder andere Anker-Themen interessanter waeren:

```bash
python3 extract_subgraph.py
```

Die Anker-Liste steht in `extract_subgraph.py` → `ANCHORS` (aktuell 13 Hubs: Pets Deli, Hund, Belly+ Huhn, TruPet™ Postbiotika, Verdauung, BARF, Hypoallergen, Strafbewehrte Unterlassungserklaerung VSW, etc.). Edit + re-run = neue Daten.

## Controls

- **Maus-Drag** zum Drehen
- **Scroll** zum Zoomen
- **Klick auf Knoten** zum Fokussieren mit Kamera-Anflug
- **Hover** zeigt Tooltip mit Beschreibung + Lanes
- **Pfeiltasten ← →** durchs Skript navigieren
- **Leertaste** = Auto-Play An/Aus
- **Legend-Klick** = Knoten-Typ oder Lane ausblenden (toggle)

## Story (6 Scenes)

1. **Knowly v2 — der Knowledge Graph** — wide intro mit den Eckdaten
2. **Sechs Wissens-Lanes** — Lane-Cycling, jede Farbe = eine Lane (lila Studien, gruen Reviews, rot Compliance, blau Produkte, gelb DUX, orange Akzeptanz)
3. **Belly+ Huhn — ein Produkt im Zentrum** — Spotlight auf 'Trockenfutter Belly+ Huhn fuer Hunde' + alle 1-Hop-Verbindungen
4. **412 Studien als eigene Entitaeten** — Produkt+Study-Subgraph (lila→blau Wissenschafts-Belege)
5. **Compliance — was wir nicht sagen duerfen** — nur die roten Compliance-Kanten + 41-Live-Treffer Stat
6. **Frei erkunden** — voller Graph zurueck, klick-explore-Mode

## Auto-Play

Klick "▶ Auto-Play" → 14 sec pro Scene → stoppt automatisch nach Scene 6. Jan kann waehrend des Talks reden, ohne Buttons anzufassen.

## Stack

- **3d-force-graph** (Vasco Asturiano, MIT) via CDN
- **three.js 0.149.0** (WebGL renderer)
- **LightRAG /graphs API** (`http://localhost:9623/graphs?label=X&max_depth=Y&max_nodes=Z`)
- Single-file `index.html` + extracted `kg-data.json` (2.5 MB)

## Files

- `index.html` — 23 KB, alles drin (CSS + JS + Story-Config)
- `kg-data.json` — 2.5 MB, 1747 Knoten + 8271 Kanten
- `extract_subgraph.py` — Python-Script zur Daten-Extraktion
- `README.md` — diese Datei

## Anpassen

**Anker-Themen tauschen:** `extract_subgraph.py` → `ANCHORS`-Liste.

**Story-Text:** `index.html` → `SCENES`-Array oben im Script-Block. Body-HTML kann Inline-Styles haben (`<span class='highlight'>...</span>`).

**Farben:** `kg-data.json` → `lanes` und `entity_groups`. Oder aendere `LANES` und `ENTITY_GROUPS` in `extract_subgraph.py` und re-run.

**Auto-Play-Tempo:** `index.html` → `AUTOPLAY_INTERVAL_MS` (default 14000ms).

## Hosting

Aktuell rein lokal. Falls Public-Deploy gewuenscht:

```bash
# GitHub Pages Variante (analog zu team-update)
cd /tmp/knowly-v2-kg-viz-pages
git init
cp /Users/janbohlmann/.../ai-hour-viz/index.html .
cp /Users/janbohlmann/.../ai-hour-viz/kg-data.json .
git add -A && git commit -m "Initial"
git remote add origin git@github.com:janbohlmann-lab/knowly-v2-kg-viz.git
git push -u origin main
# Dann in Repo-Settings → Pages → Branch=main, Folder=root
```

Aber: kg-data.json ist 2.5 MB, GitHub Pages serviert das problemlos. Achtung — die Daten sind public dann.

## Status

- Build: 2026-05-29, ~2h
- Tested: alle 6 Scenes durchgegangen, Auto-Play OK, Camera-Centering OK
- Ready fuer AI Hour heute Abend
