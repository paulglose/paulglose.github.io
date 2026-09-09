# Projekt-Kontext (Stand: für Claude Code)

Diese Datei fasst zusammen, woran zuletzt in einem anderen Claude-Chat
(claude.ai) gearbeitet wurde. Bitte diese Datei zuerst lesen, bevor
du loslegst - dann muss ich dir nicht alles neu erklären.

## Übergeordnetes Ziel

Paul (KI-Science Master-Student, 4 Jahre Unity/Game-Dev-Erfahrung,
Gründer von "Void Shifter") baut sich ein Portfolio auf, um bessere
Chancen auf KI-Engineering-Jobs (Werkstudent/Praktikum/Junior) zu haben.

## Projekt 1: Portfolio-Website (paulglose.github.io)

**Status:** Live, Grundstruktur fertig, muss noch mit echten Inhalten
gefüllt werden.

- Repo: `paulglose.github.io` (GitHub Pages, Branch `main`, Root)
- Eine Datei: `index.html` (komplett self-contained, CSS inline)
- Design: bewusst NICHT generisch (kein Cream+Terracotta, keine
  SaaS-Cards). Stattdessen: "Blueprint/Schaltplan"-Ästhetik.
  - Farben: Navy `#0F1B2E` (Hintergrund), Off-White `#F4F5F2` (falls
    Content-Flächen genutzt werden), Amber `#E8A33D` (einziger Akzent)
  - Typo: Space Grotesk (Headlines), IBM Plex Sans (Fließtext),
    IBM Plex Mono (nur kleine Tags/Status-Labels)
  - Struktur: Hero, About, Projects (als Log/Registry-Liste, nicht als
    Karten), Skills (Tag-Cluster), Contact
- **Noch offen / TODO:**
  - ~~Platzhalter-E-Mail~~ erledigt: `paulglose@icloud.com` eingetragen
  - ~~Platzhalter-GitHub-Links~~ erledigt: GitHub-Links (Hero + Contact) erstmal
    komplett entfernt, da noch kein GitHub-Handle feststeht — sobald eins
    feststeht, wieder einbauen
  - ~~"Game Dev Portfolio"-Link (paul-glosemeyer.de)~~ erledigt: entfernt,
    da noch keine echte Seite existiert
  - ~~LinkedIn-Link~~ erledigt: auf `https://www.linkedin.com/in/paul-glosemeyer-51a4a81ba/` aktualisiert
  - Projekt-Links (`href="#"`) waren als `<a href="#">` Platzhalter drin -
    jetzt zu nicht-klickbaren `<div>`s gemacht, bis echte Repo-URLs existieren
  - Erstes Projekt (LoL-Overlay-Tool, siehe unten) als eigenes Repo
    hochladen und verlinken
  - Sobald GitHub-Handle feststeht: GitHub-Link wieder in Hero + Contact ergänzen

## Projekt 2: LoL "Bin ich sichtbar?"-Overlay-Tool

**Status:** Funktioniert vollständig, lokal auf Pauls Windows-Rechner
getestet. Noch NICHT auf GitHub hochgeladen / nicht als Portfolio-Repo
aufbereitet.

**Was es macht:** Erkennt per Screen-Capture, ob ein bestimmter
League-of-Legends-Buff (Nightstalker-Tarnung von "Schattengleve"/
Duskblade) gerade aktiv ist, und zeigt dann ein kleines Overlay-Icon
(schwarzes durchgestrichenes Auge) mittig über dem Champion an - rein
visuell, click-through (keine Interaktion möglich).

**Technischer Weg dahin (wichtig für Doku/README später):**
- Erste Ansätze (Radial-Prozent-Messung des Cooldown-Rings per
  Polar-Unwrapping, Timer-basierte Logik, maskiertes Matching) wurden
  alle verworfen - siehe unten "Warum" für die Portfolio-Story.
- Kernproblem 1: Die Buff-Leiste ist teiltransparent, das
  Spielgeschehen scheint durch -> normales Template Matching lieferte
  zufällige Scores.
- Kernproblem 2: Das Icon kommt 2x vor - einmal OHNE Ring (das
  Ziel-Icon: "ich bin unsichtbar"), einmal MIT Ring (ein anderer
  Effekt, soll ignoriert werden). Beide sehen fast identisch aus.
- Finale Lösung (`simple_detect.py`):
  1. `select_region.py`: Suchbereich (grob) + Icon-Template (eng, per
     Mausauswahl) festlegen
  2. Im Suchbereich werden ALLE Positionen gefunden, die zum Template
     passen (`find_all_matches`, `cv2.matchTemplate` + `TM_CCOEFF_NORMED`)
  3. Pro Treffer zwei Prüfungen: (a) Durchschnittshelligkeit nahe an
     der Referenzhelligkeit des Templates, (b) eine lokal sehr
     empfindliche Prüfung in der "Ring-Zone" (oberer Icon-Bereich, wo
     der Ring beginnt) - fängt auch einen gerade erst gestarteten,
     winzigen Ring-Strich sofort ab (reine Durchschnittshelligkeit war
     dafür zu träge, ca. 2 Sekunden Verzögerung)
  4. Overlay: `hidden.png` (transparentes Icon), echte Transparenz via
     Windows `-transparentcolor`-Trick (Magenta als Farbschlüssel,
     Alpha hart auf 0/255 geschwellt statt weicher Kanten, sonst lila
     Ränder), Position frei wählbar über `pick_overlay_position.py`
     (Mausklick, speichert in `overlay_position.json`)
  5. Click-through per Windows-API (`ctypes`, `WS_EX_TRANSPARENT`)
  6. `start_detector.bat` zum bequemen Doppelklick-Start

**Aktuell benötigte Dateien (Rest wurde als Sackgasse identifiziert
und kann/soll fürs Portfolio-Repo weggelassen oder in einem
"exploration"-Ordner archiviert werden):**
- `simple_detect.py` (Hauptskript)
- `select_region.py`
- `pick_overlay_position.py`
- `region.json`, `template.png`, `overlay_position.json` (User-generierte Configs)
- `hidden.png` (Overlay-Icon)
- `start_detector.bat`

**TODO für dieses Projekt:**
- Sauberes GitHub-Repo aufsetzen (eigener Name, z.B. `lol-visibility-overlay`)
- Gutes README schreiben: was es macht, warum (Motivation), wie der
  technische Weg dahin aussah (die Sackgassen sind Teil der Story -
  zeigt Debugging-Prozess, nicht nur Endergebnis), Screenshots/GIF,
  Setup-Anleitung
- Auf `paulglose.github.io` als erstes Projekt verlinken (Status
  bereits auf "SHIPPED" gesetzt in der index.html, Link muss nur noch
  eingetragen werden)

## Geplante nächste Projekte (aus der KI-Engineering-Roadmap)

1. **RAG-Dokumentenassistent** (AKTUELL GESTARTET, Stand 2026-09-08) -
   Chatbot mit Retrieval über eigene Dokumente. Stack: Python, FastAPI,
   Chroma (Vector DB), OpenAI/Anthropic API, Streamlit-Frontend für
   den Anfang.
   - Konkretes Nutzungsbeispiel: eigene Uni-Skripte (KI-Science) als
     Wissensbasis, Fragen wie "Was ist der Unterschied zwischen
     Backpropagation und Gradient Descent laut Skript 3?" werden anhand
     der Dokumente beantwortet statt aus allgemeinem Trainingswissen.
   - Pipeline: Dokumente einlesen -> in Chunks zerlegen -> Embeddings
     erzeugen -> in Chroma speichern -> bei Nutzerfrage Frage embedden,
     Top 3-5 ähnliche Chunks abrufen -> Frage + Chunks ans LLM ->
     Antwort zurückgeben.
   - Ziel-Deployment: Live-Demo z.B. via Hugging Face Spaces.
   - Portfolio-Anspruch (gilt für jedes Projekt-Repo): funktionierender
     Code zum Klonen/Starten, README mit Screenshot/Demo-GIF,
     Setup-Anleitung, kurzer Architektur-Erklärung, idealerweise
     Live-Demo-Link.
2. Agentisches Tool-System (LangChain o.ä.)
3. KI-Agent in Unity (LLM-NPC oder RL-Agent) - Alleinstellungsmerkmal
   durch Unity-Hintergrund

## Was Claude Code jetzt konkret helfen könnte

- Das LoL-Overlay-Projekt in ein sauberes Portfolio-Repo mit README verwandeln
- Beim RAG-Projekt von Null starten
- index.html Platzhalter durch echte Daten ersetzen
