# Artist Operating System

Ein persönliches Karriere-Dashboard für unabhängige Musiker:innen — gebaut für realistische Lebensbedingungen (Familie, Dayjob, 30 min/Tag).

Läuft als statische HTML-App direkt im Browser. Kein Server, kein Framework, keine Abhängigkeiten.

---

## Demo

Lokal öffnen: `index.html` einfach im Browser aufrufen.

Online: Nach dem Deployen auf GitHub Pages erreichbar unter  
`https://DEINNAME.github.io/REPONAME`

---

## Inhalt

| Tab | Beschreibung |
|-----|-------------|
| Prioritäten | Wöchentliche Checkliste + offene Entscheidungen |
| Release-Plan | EP-Sequenz, Maßnahmen-Zeitplan, Pressetext-Template |
| Wochenplan | 30-min/Tag-Modell, Automatisieren vs. Weglassen, Review-Zyklen |
| Budget | Budgetverteilung 500 €/Song, Ausgaben-Tracker, Hebelkraft-Vergleich |
| MVAS | Minimum Viable Artist System — die 3 größten Hebel, 80/20-Regeln |
| Video | Batch-Produktions-Strategie, Drehtag-Checkliste, Referenz-Ästhetik |
| KPIs | Manuelle Tracking-Felder für Spotify, Newsletter, Follower etc. |
| Risiken | Die 5 größten Risiken + Gegenmaßnahmen |
| Ideen | Ideen-Parkplatz + Ressourcen-Datenbank (Netzwerk, Locations) |

---

## Deployment auf GitHub Pages

**Schritt 1 — Repository anlegen**

```bash
git init
git add index.html README.md
git commit -m "Initial commit: Artist OS"
git branch -M main
git remote add origin https://github.com/DEINNAME/REPONAME.git
git push -u origin main
```

**Schritt 2 — GitHub Pages aktivieren**

1. Repository → **Settings** → **Pages**
2. Source: `Deploy from a branch`
3. Branch: `main` / Ordner: `/ (root)`
4. Speichern

Nach ~1 Minute ist die App live unter:  
`https://DEINNAME.github.io/REPONAME`

---

## Datenspeicherung

Alle Eingaben (Checklisten, Ausgaben, KPIs, Ideen, Ressourcen) werden im **localStorage** des Browsers gespeichert.

- ✅ Daten bleiben nach dem Schließen des Browsers erhalten
- ✅ Keine Anmeldung, kein Server, kein Datenschutzproblem
- ⚠️ Daten sind gerätespezifisch — kein automatischer Sync zwischen Geräten
- ⚠️ Daten gehen verloren, wenn der Browser-Cache geleert wird

**Tipp:** Wichtige Einträge (z.B. Ressourcen-Datenbank) gelegentlich als Screenshot oder Textdatei sichern.

---

## Anpassen

Die gesamte App besteht aus einer einzigen Datei (`index.html`) mit drei Abschnitten:

```
index.html
├── <style>      — Alle CSS-Variablen und Styles
├── <body>       — HTML-Inhalt der 9 Panels
└── <script>     — Interaktionslogik und localStorage
```

### Häufige Anpassungen

**Eigene Checklisten-Einträge als Standard setzen**  
In der `<script>`-Sektion die Variable `defaultChecks` bearbeiten:

```js
var defaultChecks = [
  { label: 'Meine eigene Aufgabe', checked: false },
  // ...
];
```

**Eigene KPI-Felder definieren**  
Variable `defaultKpis` anpassen:

```js
var defaultKpis = [
  { label: 'Spotify Hörer:innen / Monat', value: '—' },
  { label: 'Mein eigener KPI', value: '—' },
  // ...
];
```

**Budget-Posten ändern**  
Variable `defaultBudget` anpassen:

```js
var defaultBudget = [
  { label: 'Musikvideo', amount: 150 },
  // ...
];
```

**Farben ändern**  
Alle Farben sind als CSS-Variablen in `:root` definiert (Light Mode) und `@media (prefers-color-scheme: dark)` (Dark Mode). Einfach die Hex-Werte ersetzen.

---

## Technische Details

- Reines HTML/CSS/JavaScript — kein Build-Prozess, kein npm
- Keine externen Abhängigkeiten oder CDN-Aufrufe
- Dark Mode über `prefers-color-scheme` Media Query
- Responsive ab ~320px Breite
- Getestet in Chrome, Firefox, Safari, Edge

---

## Erweiterungsideen

- **Export-Funktion** — Daten als JSON herunterladen (Backup)
- **Import-Funktion** — JSON-Backup wieder einlesen
- **Song-Datenbank** — Eigener Tab mit allen Songs, Status und Assets
- **Kalender-Ansicht** — Release-Daten visuell darstellen
- **Cloud-Sync** — Mit Supabase (kostenlos) geräteübergreifend speichern
- **PWA** — Als installierbare App auf dem Homescreen

---

## Lizenz

Persönliches Projekt — frei verwendbar, anpassbar und erweiterbar.

---

## Cloud-Sync mit Supabase einrichten

Alle Daten werden standardmäßig im Browser-localStorage gespeichert. Mit Supabase werden sie zusätzlich in der Cloud gesichert und sind auf jedem Gerät verfügbar.

### Schritt 1 — Supabase-Projekt anlegen

1. Gehe zu [supabase.com](https://supabase.com) und erstelle einen kostenlosen Account
2. Klicke auf **New project** — Name z.B. `artist-os`, Region `EU West`
3. Warte ~1 Minute bis das Projekt bereit ist

### Schritt 2 — Datenbank-Tabelle anlegen

Im Supabase-Dashboard → **SQL Editor** → **New query** — diesen Code ausführen:

```sql
create table artist_os (
  key   text primary key,
  value jsonb not null,
  updated_at timestamp with time zone default now()
);

-- Nur Zugriff mit dem API-Key erlauben (Row Level Security)
alter table artist_os enable row level security;

create policy "anon can read and write"
  on artist_os
  for all
  to anon
  using (true)
  with check (true);
```

### Schritt 3 — API-Key in die App eintragen

Im Supabase-Dashboard → **Project Settings** → **API**:

- **Project URL** kopieren → in `index.html` bei `SUPABASE_URL` eintragen
- **anon / public Key** kopieren → bei `SUPABASE_KEY` eintragen

```js
var SUPABASE_URL = 'https://DEINPROJEKT.supabase.co';
var SUPABASE_KEY = 'eyJhbGciOi...langer Key...';
```

### Schritt 4 — Repo auf privat stellen

Da der API-Key in der `index.html` steht:

GitHub → Repository → **Settings** → **General** → **Danger Zone** → **Change repository visibility** → **Private**

GitHub Pages funktioniert auch mit privaten Repos.

### Fertig

Nach dem nächsten Push erscheint in der App-Kopfzeile **"Cloud-Sync aktiv"** in grün.  
Alle Änderungen werden ab sofort automatisch in Supabase gespeichert — und beim nächsten Öffnen auf jedem Gerät geladen.
