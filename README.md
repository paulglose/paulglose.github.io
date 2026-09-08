# Portfolio-Website auf GitHub Pages veröffentlichen

## Schritt 1: Repository erstellen

1. Auf GitHub einloggen, neues Repository erstellen
2. **Wichtig für die einfachste Variante:** Repo-Name muss exakt
   `deinusername.github.io` sein (dein GitHub-Benutzername statt
   "deinusername"). Damit ist die Seite automatisch unter
   `https://deinusername.github.io` erreichbar, ganz ohne weitere Konfiguration.
3. Repository auf "Public" stellen (Pflicht für kostenlose GitHub Pages)

## Schritt 2: Datei hochladen

**Einfachste Variante (ohne Git-Kommandozeile):**
1. Im neuen Repo auf "Add file" → "Upload files" klicken
2. `index.html` hochladen
3. Commit-Message eingeben, "Commit changes" klicken

**Mit Git (falls du das eh schon nutzt):**
```bash
git clone https://github.com/deinusername/deinusername.github.io.git
cd deinusername.github.io
# index.html reinkopieren
git add index.html
git commit -m "Portfolio-Seite hinzufuegen"
git push
```

## Schritt 3: GitHub Pages aktivieren

1. Im Repo auf "Settings" → "Pages" (linkes Menü)
2. Unter "Source": "Deploy from a branch" auswählen
3. Branch: "main", Ordner: "/ (root)" → Speichern
4. Nach 1-2 Minuten ist die Seite live unter `https://deinusername.github.io`

## Vor dem Veröffentlichen noch anpassen

In `index.html` suchen und ersetzen:
- `deine@email.de` → deine echte E-Mail-Adresse
- `https://github.com/` (zwei Stellen) → dein echtes GitHub-Profil, z.B. `https://github.com/deinusername`
- Die vier Projekt-Links (`href="#"`) → echte Links zu deinen GitHub-Repos, sobald sie existieren
- Den "SHIPPED"-Status beim ersten Projekt ggf. anpassen, falls du es noch nicht hochgeladen hast

## Neue Projekte später ergänzen

Jede Projekt-Zeile in der Projects-Sektion ist ein Block wie dieser:

```html
<a class="project-row" href="LINK_ZUM_REPO" target="_blank" rel="noopener">
  <div class="project-main">
    <div class="project-name">Projektname</div>
    <div class="project-desc">Kurze Beschreibung, was es macht.</div>
  </div>
  <div class="project-stack">Tech · Stack<br>Zeile 2</div>
  <div class="status live">SHIPPED</div>
</a>
```

Einfach diesen Block kopieren, Inhalte anpassen, an gewünschter Stelle einfügen.
Status-Optionen: `live` (Amber), `progress` (grau), `planned` (gedimmt).
