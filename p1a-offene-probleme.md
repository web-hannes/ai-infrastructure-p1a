# P1a – offene Probleme (Stand 17.11.2026)

Bezieht sich auf `ai-infrastructure-p1a/inD_project_notebook_phase_structure.ipynb`. Prio: **H** = verfälscht Ergebnisse, **M** = schwächt Aussagekraft, **N** = Kosmetik.

| # | Prio | Problem | Folge | Fix |
|---|---|---|---|---|
| 1 | H | Schnelle Zweiräder mit Boxgröße 0 kommen im Training (Kreuzung 1+2) kaum vor | Modell bricht sie in jedem Frame ab: 384 echte → 1.657 vorhergesagte Tracks (Aufnahme 0) | Größe für Fahrrad/Fußgänger nicht als Feature nutzen; Split mit schnellem Verkehr im Training |
| 2 | H | `TRACK_THRESHOLD = 0.5` in 7.1 fest statt Val-Schwelle (0,15) | Unnötig viele neue IDs | `None` oder niedriger (0,01 → 478 IDs) |
| 3 | H | Modellwahl nach Val-F1: alle Modelle ≈ 0,9999 | Zufallswahl; Gewinner hat auf Test Recall 0,73 | Nach Val-Loss oder Tracking-Metrik wählen; schwierigeres Val-Set |
| 4 | H | Nur Paar-F1, keine Tracking-Metrik | Zerstückelung unsichtbar (F1 0,998 trotz 4× zu vieler IDs) | IDF1 + ID-Fehler (`id_scores` aus alter Version) auf allen Test-Aufnahmen |
| 5 | M | `without_fragmentation` zählt echte Tracks, nicht Bruchstücke | Wirkt gut (0,977), verdeckt Problem 1 | Durch ID-Fehler pro 100 Tracks ersetzen |
| 6 | M | Lücken-Fallback teilt dx/dy durch Lückenlänge | Abgebrochene Tracks werden nie wieder verbunden | Position mit Geschwindigkeit vorhersagen (Kalman-artig) |
| 7 | M | Keine Geschwindigkeit als Feature (nur Paare t → t+1) | Größter Gewinn laut alter Ablation fehlt | Geschwindigkeit aus Track-Verlauf als Feature |
| 8 | M | 7.1 nutzt festes `TRACK_MODEL` und nur 1 Aufnahme | Tracking-Ergebnis zeigt nicht das gewählte Modell, kleine Stichprobe | Gewinner bzw. alle Modelle; `MAX_RECORDINGS = None` |
| 9 | M | Synthetische Fahrrad-/Fußgängergrößen aus 5.1 sind pro Track konstant | Bei Nutzung: Modell liest ID an der Größe ab (Leakage) | Pro Frame neu ziehen oder weglassen |
| 10 | M | Daten sind geglättete Referenzboxen ohne Rauschen und Lücken | Alle Modelle ≥ 0,98, Unterschiede kaum aussagekräftig | Robustheitstest: Positionsrauschen, fehlende Detections |
| 11 | M | Zwei Stillstandsregeln (`STATIC_MAX_DIST_M`, `PARK_*`) parallel | Uneinheitliche Bereinigung | Auf eine Regel vereinheitlichen |
| 12 | N | Text in 5.3 nennt 200 Frame-Paare, Code nutzt 300 | Doku falsch | Text anpassen |
| 13 | N | `torch` 3× importiert; Kapitel 5.3–7 deutsch, Rest englisch | Unübersichtlich | Import einmal; eine Sprache |
