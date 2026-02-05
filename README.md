# PHWTEX
**Inoffizielle** $\LaTeX$-Vorlage für Ausarbeitungen an der **PHWT**.

## Installation

### Mit Docker
1. Installiere [Visual Studio Code](https://code.visualstudio.com/)
2. Installiere [Docker](https://www.docker.com/get-started/)
3. Führe `docker pull texlive/texlive:latest` in der Kommandozeile aus, um das LaTeX Docker-Image herunterzuladen (das kann etwas dauern)
4. Klone dieses Repository oder lade es als ZIP-Datei herunter und entpacke es
5. Öffne den Ordner in Visual Studio Code
6. Installiere die [LaTeX Workshop Extension](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop)
7. Nehme eine Anpassung in `chapters/01_Einleitung.tex` vor, speichere die Datei und klicke auf den grünen Pfeil in der oberen Leiste (*Build LaTeX project*)
8. Öffne `build/main.pdf` und überprüfe das Ergebnis

### Ohne Docker

Firmenrechner am I right (～￣▽￣)～

1. Installiere dir [TeX Live](https://www.tug.org/texlive/) (wird ewig dauern, nimm dir Zeit)
2. Stelle sicher, dass `C:\Users\<User>\texlive\20XX\bin\windows\` in deiner PATH-Umgebungsvariable ist (ersetze `<User>` und `20XX` entsprechend)
3. Installiere [Visual Studio Code](https://code.visualstudio.com/)
4. Klone dieses Repository oder lade es als ZIP-Datei herunter und entpacke es
5. Öffne den Ordner in Visual Studio Code
6. Installiere die [LaTeX Workshop Extension](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop)
7. Passe `.vscode/settings.json` folgendermaßen an

```diff
-   "latex-workshop.docker.enabled": true,
-   "latex-workshop.docker.image.latex": "texlive/texlive:latest",
```

8. Nehme eine Anpassung in `chapters/01_Einleitung.tex` vor, speichere die Datei und klicke auf den grünen Pfeil in der oberen Leiste (*Build LaTeX project*)
9. Öffne `build/main.pdf` und überprüfe das Ergebnis

---

## Build-Prozess

Der Build-Prozess wird durch die LaTeX Workshop Extension in Visual Studio Code gesteuert.
Standardmäßig musst du den Build-Prozess manuell starten, indem du auf den grünen Pfeil in der oberen Leiste klickst (*Build LaTeX project*). Die PDF wird dann im `build/`-Verzeichnis abgelegt. Alternativ kannst du in den `.vscode/settings.json` folgende Änderung vornehmen, um den Build-Prozess bei jedem Speichern automatisch zu starten:

```diff
-   "latex-workshop.latex.autoBuild.run": "never",
+   "latex-workshop.latex.autoBuild.run": "onSave",
```
Dadurch wird der Build-Prozess jedes Mal gestartet, wenn du eine Datei speicherst. Die Extension sieht standardmäßig den `onFileChange`-Trigger vor. Ich habe die Angewohnheit viel zu speichern und oft Dateien zu wechseln, weshalb ich einen manuellen Build bevorzuge, um nicht alle 10 Sekunden einen Build zu starten, der vielleicht noch Error wirft.

---

## Formatierung

Beim Speichern einer `.tex`-Datei wird diese mit `latexindent` automatisch formatiert. Da `latexindent` ein pain in the ass ist aufzusetzen, habe ich mich nach viel Herumprobieren dazu entschieden, einfach die Default-Config zu verwenden. Das heißt leider, dass die single-sentence per line Regel nicht umgesetzt wird. Dafür werden Tabellen aber schön getabbt (✿◠‿◠).

Wenn sich irgendwer die Mühe machen will das aufzusetzen, immer her damit.

---

## Arbeiten mit der Vorlage

An sich ist der Aufbau der Vorlage recht simpel. Schau' dir am besten einfach mal die einzelnen Dateien der Vorlage an.

Text schreiben wirst du hauptsächlich in den Kapiteln im `chapters/`-Verzeichnis. Wenn du ein Kapitel hinzufügst oder entfernst, musst du auch `content.tex` anpassen, da dort die Kapitel eingebunden werden.

### Dateien und Verzeichnisse

| Datei/Verzeichnis | Bedeutung           | Für dich relevant                                         |
| ----------------- | ------------------- | --------------------------------------------------------- |
| acronyms.tex      | Abkürzungen         | Hier kannst du deine Abkürzungen definieren               |
| appendix.tex      | Anhang              | Hier kannst du Anhänge hinzufügen                         |
| commands.tex      | Eigene Befehle      | Hier kannst du eigene Befehle definieren                  |
| content.tex       | Hauptinhalt         | Hier kannst du deine Kapitel einfügen                     |
| main.tex          | Einstiegspunkt      | Hier musst du seltenst etwas anpassen                     |
| meta.tex          | Metadaten           | Hier kannst du Metadaten wie Titel, Autor, etc. anpassen  |
| packages.tex      | Eingebundene Pakete | Hier kannst du weitere Pakete einbinden                   |
| assets/           | Assets              | Hier legst du weitere Assets ab (Bilder, Quellcode, etc.) |
| chapters/         | Kapitel             | Hier legst du deine Kapitel an                            |
| figures/          | Abbildungen         | Hier legst du deine Abbildungen ab                        |
| tables/           | Tabellen            | Hier legst du deine Tabellen an                           |

### Abkürzungen

Bei Abkürzungen ist zwischen Akronymen und Abkürzungen zu unterscheiden. Ein Akronym ist eine Abkürzung, die aus den Anfangsbuchstaben mehrerer Wörter gebildet wird (z.B. "API"), während eine Abkürzung eine verkürzte Form eines Wortes oder einer Phrase ist (z.B. "z.B."). Ob das die korrekte Definition ist, sei mal dahingestellt, aber so ist das hier gemeint.

In `acronyms.tex` sollten lediglich Akronyme definiert werden, also Abkürzungen, die aus den Anfangsbuchstaben mehrerer Wörter gebildet werden. Alle anderen Abkürzungen sind in `commands.tex` zu definieren.

> [!NOTE]
> **LaTeX**: Ein \gls{API} kann \zB als Schnittstelle implementiert werden. \
> **PDF**: Ein Application Programming Interface (API) kann z.B. als Schnittstelle implementiert werden.

Ferner werden Akronyme automatisch beim ersten Vorkommen ausgeschrieben und mit der Abkürzung in Klammern versehen. Alle weiteren Vorkommen werden dann nur noch mit der Abkürzung dargestellt. Akronyme werden auch mit einem Eintrag im Abkürzungsverzeichnis aufgeführt, während andere Abkürzungen nicht im Abkürzungsverzeichnis auftauchen. Klingt komisch, ist aber so.

### Quellenangaben

#### Literaturverzeichnis

Das Literaturverzeichnis wird mit `biblatex` erstellt. Alle Quellenangaben werden in der `references.bib`-Datei im BibTeX-Format gespeichert. In `main.tex` kannst du den Zitierstil anpassen (z.B. `numeric`, `authoryear`, etc.). Die Vorlage verwendet standardmäßig den `ieee`-Stil. Eine Liste weiterer Zitierstile findest du bei [Overleaf](https://www.overleaf.com/learn/latex/Biblatex_bibliography_styles).

#### Zitieren

Es gibt verschiedene Möglichkeiten, Quellen zu zitieren. LaTeX bietet hier eine Vielzahl von Befehlen, die du je nach Bedarf verwenden kannst.

Hier sind einige Beispiele:

| Befehl            | Ausgabe                    | Beispiel             |
| ----------------- | -------------------------- | -------------------- |
| `\cite{key}`      | [1]                        | \cite{doe:2020}      |
| `\parencite{key}` | [1]                        | \parencite{doe:2020} |
| `\textcite{key}`  | Doe [1]                    | \textcite{doe:2020}  |
| `\footcite{key}`  | $^1$ <Vermerk in Fußzeile> | \footcite{doe:2020}  |

>[!NOTE]
> Der Zitierstil hat ebenfalls Einfluss darauf, wie die Ausgabe der Zitate aussieht. Bei anderen Stilen sieht `\cite{key}` z.B. anders aus als `\parencite{key}`. 

>[!NOTE]
> Du kannst auch Seitenangaben zu deinen Zitaten hinzufügen, indem du optionale Argumente verwendest. Zum Beispiel: `\cite[42]{doe:2020}` oder `\cite[40--42]{doe:2020}`. Die "S. " Präfix wird automatisch hinzugefügt. Schreibst du dort aber etwas anderes als Zahlen oder Zahlenbereiche hin, wird kein Präfix hinzugefügt.

### Tabellen, Abbildungen und Listings

Tabellen, Abbildungen und Quellcode-Listings sollten in den entsprechenden Verzeichnissen `tables/`, `figures/` und `listings/` abgelegt werden. In den Kapiteln kannst du sie dann mit den zugehörigen Befehlen einbinden:

| Element   | Befehl                  | Beispiel                                                                                       |
| --------- | ----------------------- | ---------------------------------------------------------------------------------------------- |
| Tabelle   | `\tabelle{dateiname}`   | `\tabelle{meine_tabelle}` bei `tables/meine_tabelle.tex`                                       |
| Abbildung | `\abbildung{dateiname}` | `\abbildung{mein_bild}` bei `figures/mein_bild.tex` die Bilddatei wird unter `assets` abgelegt |
| Listing   | `\code{dateiname}`      | `\code{mein_code}` bei `listings/mein_code.tex`                                                |

Für jedes dieser Elemente habe ich in `.vscode/phwtex.code-snippets` auch einen Code-Snippet hinterlegt, den du ganz einfach ein neues Element einfügen kannst.

Bei Tabellen wird bei den Snippets unterschieden zwischen "Standard" (nur so breit wie nötig), "Seiten-Tabelle" (über die gesamte Textbreite) und "Mehrspaltige Tabelle" (für Tabellen, die mehr Struktur benötigen).

Für Listings gibt es ein Snippet bei dem der Code in die `.tex`-Datei geschrieben wird und ein Snippet, bei dem der Code in einer separaten Datei im `assets/`-Verzeichnis abgelegt wird.

Für Abbildungen gibt es nur ein Snippet, da Abbildungen immer in einer separaten Datei im `assets/`-Verzeichnis abgelegt werden sollten.

### Referenzieren

Wie du referenzierst hängt davon ab, was du referenzieren möchtest. Grundsätzlich verwendest du immer `\cref{label}` aus dem `cleveref`-Paket, da dieses automatisch den richtigen Namen (z.B. "Abbildung", "Tabelle", "Kapitel", etc.) vor die Referenz setzt.

Mit `\cref` kannst du auch mehrere Elemente gleichzeitig referenzieren:

`Wie \cref{fig:beispiel-1,fig:beispiel-2,tab:tabstd} zeigen,`

wird zu

`Wie Abbildungen 1 und 2 und Tabelle 1 zeigen,`

Wenn du etwas aus dem Anhang referenzieren möchtest, solltest du den `\anhang` Befehl verwenden:

`\anhang{app:mein_anhang}{fig:mein_bild}`

wird zu

`Abbildung 1 (Anhang A.1 auf Seite vi)`

#### Label-Naming Konventionen

| Element             | Präfix im Label | Beispiel                     |
| ------------------- | --------------- | ---------------------------- |
| Abschnitt           | `sec:`          | `\label{sec:einleitung}`     |
| Unterabschnitt      | `subsec:`       | `\label{subsec:motivation}`  |
| Unterunterabschnitt | `subsubsec:`    | `\label{subsubsec:beispiel}` |
| Abbildung           | `fig:`          | `\label{fig:mein_bild}`      |
| Tabelle             | `tab:`          | `\label{tab:meine_tabelle`   |
| Listing             | `lst:`          | `\label{lst:mein_code}`      |
| Gleichung           | `eq:`           | `\label{eq:meine_gleichung}` |
| Anhang              | `app:`          | `\label{app:mein_anhang}`    |

### Metadaten

Die Metadaten deiner Arbeit (Titel, Autor, Datum, etc.) kannst du in der `meta.tex`-Datei anpassen.

### Unterschrift

Für die Eidesstattliche Erklärung benötigst du eine Unterschrift. Die Vorlage enthält einen Platzhalter, diesen musst du durch ein Bild deiner Unterschrift ersetzen. Erstelle dir eine Unterschrift (im Idealfall 500x200px) und speichere sie im `assets/`-Verzeichnis als `Unterschrift.png` ab. Bei anderem Namen oder anderer Dateiendung musst du diese in `statutory-declaration.tex` anpassen.

### Todos

Du kannst den `\todo`-Befehl verwenden, um Anmerkungen in deinem LaTeX-Dokument zu hinterlassen. Diese Anmerkungen werden im PDF als farbige Boxen angezeigt und helfen dir, wichtige Punkte oder Aufgaben zu markieren, die du später bearbeiten möchtest.

Beispiel:

```latex
Wie in Abbildung \cref{fig:mein_bild} zu sehen ist, \todo{Hier noch genauer erklären, warum das wichtig ist.} ist dies ein wichtiger Aspekt.
```

Dasselbe Paket, das den `\todo`-Befehl bereitstellt, stellt auch einen `\missingfigure`-Befehl zur Verfügung, mit dem du Platzhalter für Abbildungen einfügen kannst, die du später hinzufügen möchtest.

Für alle Todos und fehlenden Abbildungen wird automatisch ein Inhaltsverzeichnis erstellt, das am Anfang des Dokuments mit dem Befehl `\listoftodos` eingefügt ist.

> [!WARNING]
> Vergiss nicht, alle Todos und fehlenden Abbildungen zu entfernen, bevor du deine Arbeit einreichst! Außerdem musst du die `\listoftodos`-Anweisung in `main.tex` auskommentieren oder entfernen, da sonst das Inhaltsverzeichnis der Todos mit in die Arbeit aufgenommen wird - auch wenn es leer ist.

```diff
% ------------------ ToDo Notes --------------------
% Auskommentieren vor der Abgabe!
- \listoftodos
- \clearpage
+ % \listoftodos
+ % \clearpage
```

### Listings

Wie du Listings einfügst habe ich bereits weiter oben erklärt. Hier geht es darum, wie du weitere Sprachen hinzufügen kannst, die von `listings` nicht standardmäßig unterstützt werden.
Eine Liste standardmäßig unterstützter Sprachen findest du in der [listings Dokumentation](https://ctan.project-creative.net/macros/latex/contrib/listings/listings.pdf).

Wenn du eine Sprache einfügen möchtest, die nicht standardmäßig unterstützt wird, musst du diese selbst definieren. Das machst du in `packages.tex` mit dem Befehl `\lstdefinelanguage`.
Am einfachsten ist es, wenn du einer KI des Vertrauens den Abschnitt `Code Listings` aus `packages.tex` als Kontext gibst und sie bittest, dir die Definition für die gewünschte Sprache zu erstellen.