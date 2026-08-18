# Blocksatz

**Warum ein guter Zeilenumbruch ein Optimierungsproblem ist.** Jeder Browser bricht Zeilen gierig.
Knuth und Plass haben 1981 gezeigt, dass man den ganzen Absatz auf einmal betrachten muss — hier
stehen beide Verfahren übereinander, mit der Güte jeder einzelnen Zeile daneben.

→ **[Blatt öffnen](https://ssims437.github.io/blocksatz/)**

- **Zwei Absätze übereinander**: gierig (wie jeder Browser) und optimal (der ganze Absatz auf einmal)
- **Güte je Zeile** als Balken und Zahl: wie weit die Zwischenräume gedehnt oder gestaucht werden
  mussten
- **Silbentrennung** zuschaltbar, nach Sprechsilbenregeln — mit der Trefferquote gegen ein Wörterbuch
- **Eigener Text**, Spaltenbreite und Schriftgrad am Regler; der Unterschied wächst mit schmaler Spalte
- **Prüflauf** — sieben Zeilen, darunter der Beweis gegen **alle** möglichen Aufteilungen

## Was „optimal" hier heißt

Jede Zeile bekommt eine Strafe `100·r³`, wobei `r` misst, wie weit die Wortzwischenräume vom Sollmaß
abweichen mussten. Die dritte Potenz ist Absicht: eine doppelt so stark gedehnte Zeile ist
**achtmal** so schlimm. Deshalb verteilt das Verfahren den Schaden, statt ihn irgendwo abzuladen —
und macht dafür bereitwillig eine frühe Zeile enger, damit vier spätere ruhig bleiben.

Bei 300 px Spaltenbreite und eingeschalteter Trennung: **gierig 13 Zeilen mit Gesamtstrafe 21 908**,
davon zwei Zeilen am Anschlag (10 000). **Optimal 12 Zeilen mit 905** — schlechteste Zeile 491.

## Was der Prüflauf zeigt

| Behauptung | Ergebnis |
|---|---|
| **das Verfahren trifft wirklich das Minimum** | 144 Absätze · **103 264 Aufteilungen einzeln durchprobiert** · Abweichung **0** |
| optimal ist nie schlechter als gierig | 600 Absätze: **379 besser, 221 gleich, 0 schlechter** · 42 % weniger Gesamtstrafe |
| keine Zeile ragt über die Spalte | 1570 Zeilen, 0 Überschreitungen |
| **gierig ist monoton — optimal nicht** | 2400 Spaltenbreiten: gierig 0 Ausreißer, **optimal 27** — und das ist kein Fehler |
| doppelt gedehnt heißt achtmal so schlimm | `100·r³` an vier Stellen, Faktor 8,0 |
| Silbentrennung gegen das Wörterbuch | **38 von 42** richtig (90 %) — daneben genau die zusammengesetzten Wörter |
| Silbentrennung senkt die Strafe | 17 mal besser, 0 mal schlechter · **73 % weniger** |

## Was mich das gekostet hat

**„Die letzte Zeile ist straffrei" — und schon setzt die Optimierung den ganzen Absatz in eine
einzige Zeile.** Im Blocksatz wird die letzte Zeile eines Absatzes nicht gedehnt; sie darf kurz
bleiben. Das hatte ich als „kostet nichts" programmiert — woraufhin das Verfahren die billigste
aller Lösungen fand: **alles in eine Zeile**, das ist dann die letzte, Strafe null. Der Prüflauf
meldete pflichtschuldig „Minimum getroffen", weil die erschöpfende Aufzählung denselben Fehler
hatte. Zwei falsche Zeugen, die sich gegenseitig bestätigen — die unangenehmste Sorte grün.

**Denselben Fehler dann gleich noch einmal.** Nach der Korrektur brauchte das Verfahren einen
zweiten Durchgang für den Fall, dass es *gar keinen* zulässigen Umbruch gibt (schmale Spalte, keine
Trennung). In diesem Notfall-Durchgang hatte ich die Prüfung wieder ausgenommen — und prompt stand
dort wieder der Ein-Zeilen-Absatz.

**Der eigentliche Denkfehler dahinter:** Ich hatte zwei völlig verschiedene Dinge gleich bestraft.

| | was es ist | wie damit umzugehen ist |
|---|---|---|
| **zu locker** | die Zwischenräume müssten weiter gedehnt werden als erlaubt | hässlich, im Notfall hinnehmbar |
| **passt nicht** | der Text ist selbst maximal gestaucht breiter als die Spalte | keine schlechte Zeile — gar keine |

Solange beides 10 000 kostet, ist **eine** unmögliche Zeile billiger als **fünf** hässliche. Erst
als „passt nicht" zum harten Ausschluss wurde und „zu locker" zur teuren, aber erlaubten Wahl,
kam heraus, was herauskommen soll.

**Meine Monotonie-Behauptung war falsch.** „Breitere Spalte braucht nie mehr Zeilen" klingt
zwingend und stimmt für den gierigen Umbruch (0 Ausreißer in 2400 Fällen). Für den optimalen ist es
falsch — in **27 Fällen** nimmt er bei breiterer Spalte eine Zeile **mehr**, weil ihm das billiger
kommt, als eine hässliche Zeile hinzunehmen. Statt die Prüfung zu streichen, misst sie jetzt beides
und benennt den Unterschied: das ist keine Panne, sondern der Charakter des Verfahrens.

**„ch" hat „sch" zerrissen.** Die Trennregel lässt Digrafen zusammen — nur lief meine Schleife die
Liste der Reihe nach durch, und der spätere Treffer überschrieb den früheren. Ergebnis:
`Flas-che` statt `Fla-sche`. Längster Digraf zuerst, beim ersten Treffer abbrechen. Zusammen mit
der zweiten Korrektur — im Deutschen dürfen **zwei** Buchstaben auf die nächste Zeile, nicht erst
drei, sonst gibt es kein `Kis-te` — stieg die Trefferquote von **67 % auf 90 %**.

**Die restlichen vier Wörter sind kein Fehler, sondern die Grenze der Methode:** `Buchs-ta-be`
statt `Buch-sta-be`, `Zeitsch-rift` statt `Zeit-schrift`, `Vers-tänd-nis` statt `Ver-ständ-nis`.
Sprechsilbenregeln kennen die **Fugen zusammengesetzter Wörter** nicht. Echte Trennprogramme
benutzen Liangs Mustertabellen und ein Wörterbuch — genau das misst die Prüfzeile, statt es zu
verschweigen.

**Der Prüflauf misst keine Schrift.** Für die Prüfungen werden die Wortbreiten **gewürfelt** statt
gemessen. Sonst hinge das Ergebnis davon ab, welche Schriften auf dem Rechner liegen — und in der
CI ist das eine andere Menge als hier. Das Blatt selbst misst natürlich echt, mit `measureText`.

**Was das Blatt nicht kann:** keine echten TeX-Strafen (Klubben, Schusterjungen, Hurenkinder,
`\penalty`), keine optischen Randausgleiche, keine Kerning-Korrektur, keine Absatzübergreifende
Seitenaufteilung, keine Liang-Muster. Und die Güte misst nur Zwischenräume — die *Flüsse*, also die
senkrechten weißen Bahnen, die durch zufällig übereinanderliegende Lücken entstehen, sieht es nicht.

## Technik

Eine einzelne HTML-Datei. Kein Build, keine Bibliothek, nichts verlässt den Browser.
Kürzester Weg durch alle Umbruchstellen (dynamische Programmierung), Canvas 2D, hell und dunkel.

## Die ganze Sammlung

Alle Blätter nach Feld geordnet, jedes mit eigenem Repo:
**[ssims437.github.io](https://ssims437.github.io/)**

## Lizenz

MIT
