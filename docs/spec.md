# Kaze 風 — Fachliche Spezifikation

Stand: 2026-07-16. Diese Spezifikation ist die Programmier-Grundlage; sie beschreibt den Ist-Stand des Konzepts (keine Änderungshistorie). Der klickbare HTML-Prototyp (`mockup/`) zeigt jedes beschriebene Verhalten zum Anfassen.

> **Diagramme:** Drei Mermaid-Diagramme — Sperr-Lebenszyklus, Screen-Flow und Notfallcode-Sequenz — visualisieren diese Spezifikation und machen eine offene Architekturentscheidung sichtbar: [diagramme.md](diagramme.md).

## 1. Produktkern

Kaze (風, der Wind): eine bewusst einfache iPhone-App — definierte Apps für eine definierte Zahl Stunden sperren, damit man rausgeht und den Wind sucht. Der „Notfallcode" ;-) zum vorzeitigen Entblocken kommt per E-Mail und existiert nirgendwo auf dem Gerät. Kostenlos und Open Source (MIT) — kein Kauf, kein Abo, kein Login, keine Statistiken.

## 2. Modell: Die Sperre

- Eine **Sperre** = eine Menge Apps + eine Laufzeit. Jedes „Anwenden" erzeugt eine neue Sperre; beliebig viele laufen parallel, jede mit eigener Uhr.
- Eine App kann immer nur in **einer** aktiven Sperre stecken.
- **Verlängern:** jederzeit, sofort, ohne Hürde — nur mehr Sperre braucht keine Hürde.
- **Verkürzen / vorzeitig beenden:** nur mit dem Notfallcode. Pro Sperre ein eigener 6-stelliger Code in einer eigenen E-Mail; das Beenden einer Sperre lässt alle anderen weiterlaufen.
- **Ablauf:** Endet die Laufzeit, werden die Apps automatisch frei (Meldung „風 — … wieder frei").
- Restzeiten überall in **h:mm**, minutengenau aufgerundet — keine Sekunden. Einzige Ausnahme: der 15-Sekunden-Stopper, dort sind Sekunden der Inhalt.

## 3. Screens

### 3.1 Splash (erster Start des Tages)

- Erscheint **nur beim ersten App-Start des Tages** (gemerkt per Datum), sonst startet die App direkt im Einrichten.
- Kurz: **ca. 2,4 Sekunden**, dann automatischer Übergang; Antippen überspringt sofort.
- Inhalt: nur **ein** Zeichen — das rote 風-Siegel mit Schriftzug KAZE und der deutschen Bedeutung „kaze — der Wind".
- Fußzeile: neckarshore.ai-Logo (20 px) mit „von neckarshore.ai", MIT-Lizenz und Link aufs GitHub-Repo.

### 3.2 Einrichten (Startbildschirm)

- Schriftzug **風 KAZE**; hauchzartes Wasserzeichen **静** („Stille") hinter der Überschrift.
- Überschrift: **„Wer soll erst mal Ruhe geben?"**
- **App-Raster** (3 Spalten): Antippen wählt an/ab. Ausgewählt = farbiges Icon mit Zinnober-Ring und Haken-Badge; abgewählt = ausgegraut. Apps in laufender Sperre: nicht wählbar, statt Name steht „bis HH:MM", Antippen zeigt nur einen Hinweis.
- **„+"-Kachel** öffnet die App-Auswahl. In der echten App zwingend Apples System-Picker (FamilyControls) — jeder eigene Dialog ist nur Platzhalter.
- **Dauer-Slider** 1–24 Std: große Stundenzahl, daneben live „frei ab HH:MM".
- **Slide-to-Lock „Anwenden":** Die Wisch-Geste ist die Bestätigung, kein Extra-Dialog. Ohne App-Auswahl deaktiviert („Erst App wählen"). Nach dem Zuschieben wechselt die App zur Übersicht.
- Fußzeile: **Der „Notfallcode" ;-) zum Entblocken kommt per E-Mail.**

### 3.3 Verriegelt (Übersicht)

- Reine **Liste der aktiven Sperren**: App-Icons, Name(n), „frei ab HH:MM", Restzeit in Zinnober, dünner passiver Fortschrittsbalken (Restanteil der Laufzeit).
- Antippen einer Sperre öffnet ihre Detail-Seite.
- Outline-Button **„+ Weitere Apps sperren"** führt zum Einrichten.
- Läuft keine Sperre mehr, wechselt die App automatisch zum Einrichten.
- Wasserzeichen **静寂** (seijaku, tiefe Stille), vertikal gesetzt, beginnend unterhalb der Dynamic Island.

### 3.4 Sperre (Detail, EasyPark-Vorbild)

- Zurück-Pfeil **„‹ Übersicht"** oben links (iOS-Konvention).
- **Zifferblatt:** Restzeit groß (h:mm) mit „Endet HH:MM", dargestellt auf einer festen **12-Stunden-Skala** mit Stunden-Ticks; über 11 Std Rest schaltet die Skala auf 24 bzw. 48 Std (Angabe unter dem Ring). So bleibt immer viel freie Strecke zum Verlängern.
- **Verlängern durch Drehen:** Griffpunkt am Bogenende, Ziehen im Uhrzeigersinn. Winkel = Zeit auf dem Blatt, Live-Anzeige „+X Min — Endet HH:MM", beim Loslassen auf 5 Minuten gerundet. Gegen den Uhrzeigersinn bewegt sich nichts — Verkürzen kennt der Regler physisch nicht.
- **„Entriegeln"** (gefüllter Zinnober-Button) startet die **15-Sekunden-Nachdenkpause**: „Willst du noch einmal drüber nachdenken?" mit Sekunden-Countdown; größte Fläche ist der grüne Schild-Button **„Sperre weiterlaufen lassen"**. Läuft der Countdown ab, öffnet sich die Code-Seite.
- Wasserzeichen **時** (toki, Zeit).

### 3.5 Code-Eingabe (Entriegeln)

- Zeigt die Apps der betroffenen Sperre, ihr reguläres Ende und wohin der Code ging (maskierte E-Mail-Adresse).
- Wasserzeichen **鍵** (kagi, Schlüssel).
- **6 Ziffernfelder** mit Auto-Weiterschaltung; falscher Code: Schütteln, Felder leeren, Hinweis.
- Richtiger Code beendet **nur diese eine Sperre** — alle anderen laufen weiter.
- Grüner Schild-Button **„Doch nicht — Sperre läuft weiter"** als gleichwertiger Ausweg.

### 3.6 Einstellungen

Sechs Abschnitte, mehr nicht. Wasserzeichen: **心** (kokoro, Herz und Geist).

1. **Notfallcode:** E-Mail-Adresse (die einzige „Kontoinformation" der App), Strikter Modus (Code-Mail erst nach 10 Min Wartezeit), Partner-Modus (Code an fremde Adresse).
2. **Blockierung:** Safari-Websites mitsperren (instagram.com und Co.); Zeitpläne (werktags 9–17 Uhr) vorgesehen, noch nicht spezifiziert.
3. **Erscheinungsbild:** Sperr-Screens dunkel (Sumi-Tusche, Standard) oder hell (Washi).
4. **Benachrichtigungen:** „Wenn eine Sperre endet" — **Standard aus**, Hinweis: „Benachrichtigungen stören nur die Ruhe ;-)". Einschalten erst nach **15 Sekunden Bedenkzeit** („Willst du wirklich benachrichtigt werden?"); Ausschalten sofort.
5. **Die Zeichen:** Menüpunkt **„Woher der Wind weht"** — öffnet die Glossar-Unterseite.
6. **Über:** GitHub-Repo-Link, MIT-Lizenz, Preis-Zeile („Kostenlos. Kein Kauf, kein Abo, kein Login."), Feedback-Button (feedback@neckarshore.ai); Fußzeile mit neckarshore.ai-Logo (24 px) und Schriftzug.

### 3.7 Woher der Wind weht (Glossar-Unterseite)

- Erreichbar über Einstellungen → „Die Zeichen"; Zurück-Pfeil „‹ Einstellungen" oben links.
- Liste aller Zeichen der App: großes Zeichen links, rechts Lesung, deutsche Bedeutung und eine kurze Geschichte (2–3 Sätze) — z. B. bei 間: die Sonne 日 im Torbogen 門.
- Beantwortet das „What does it mean?", ohne die Screens selbst mit Text zu belasten.

## 4. Name und Zeichen: Kaze 風

**Kaze** (風, der Wind) — kurz, international aussprechbar, App-Store-tauglich, und das Zeichen erzählt die Geschichte der App: Sperre zu, geh raus, such den Wind. 風 ist zugleich die Marke: Hanko-Siegel, Schriftzug 風 KAZE, Splash und Freigabe-Toast.

Die weiteren Zeichen folgen dem **Wasserzeichen-System**: Jeder Screen trägt sein festes Zeichen als großes, dezentes Wasserzeichen rechts oben — ohne deutschen Text daneben. Es bleibt beim Scrollen stehen (unterhalb der Dynamic Island), der Inhalt läuft darunter durch. Die Bedeutungen stehen gesammelt im Glossar „Woher der Wind weht" (Einstellungen → Die Zeichen).

| Zeichen | Lesung | Bedeutung | Ort und Einsatz |
|---|---|---|---|
| 静 | shizuka | Stille, Ruhe | Einrichten — Wasserzeichen hinter der Überschrift (umgesetzt) |
| 静寂 | seijaku | tiefe, wohltuende Stille | Verriegelt — Wasserzeichen, vertikal (umgesetzt) |
| 時 | toki | Zeit | Sperre (Detail) — Wasserzeichen (umgesetzt) |
| 鍵 | kagi | Schlüssel | Code-Eingabe — Wasserzeichen (umgesetzt) |
| 心 | kokoro | Herz, Geist | Einstellungen — Wasserzeichen (umgesetzt) |
| 一息 | hitoiki | ein Atemzug, kurz innehalten | Kandidat: Nachdenkpause |
| 我慢 | gaman | geduldiges Durchhalten | Kandidat: grüner Schild-Button / Toast „Gute Entscheidung." |
| 間 | ma | bewusster Zwischenraum, Pause | Kandidat: künftiger Zeitplan-Screen |

## 5. Design-System: Japanese Minimal

- **Farben:** Washi-Papier (#F5F2EA) als Grund, Sumi-Tusche (#211E19) als Schrift, Zinnober (#B5442C) als einziger Akzent; Sperr-Screens wahlweise dunkel (Tusche, Standard) oder hell (Washi) — beide aus denselben Token. Grün nur für die motivierende „Weiterlaufen"-Wahl.
- **Typografie:** Mincho-Serifen für Überschriften (Hiragino Mincho / Yu Mincho) — auch auf den Sperr-Screens; System-Sans für UI-Text; Ziffern mit Tabellensatz.
- **Form:** Hairlines statt Karten, geringe Rundungen, keine Schatten-Effekte, viel Leerraum (Ma). Icons im SF-Symbols-Stil (dünne Linie). Das Hanko-Siegel als Markenelement.
- Referenz-Design „Japanese Minimal" von rauhut.com; exakte Angleichung an das Original-CSS steht aus.

## 6. Prinzip: Reibung statt Unmöglichkeit

Es geht nicht um Unmöglichkeit, sondern um Reibung. Der Impuls „nur kurz Instagram" überlebt selten die 60 Sekunden, die der Umweg übers Postfach kostet.

- **Notausgang bleibt offen:** Echte Notfälle gehen immer — das senkt die Hemmung, die Sperre überhaupt zu aktivieren.
- **Strikter Modus:** Code-Mail kommt erst nach 10 Minuten Wartezeit.
- **Partner-Modus** als Ausbaustufe: Code geht an eine fremde Adresse.
- Dieselbe Mechanik in klein: 15-Sekunden-Bedenkzeit vor dem Entriegeln und vor dem Einschalten von Benachrichtigungen.

## 7. Technische Umsetzung (iOS)

- Echtes App-Blockieren läuft über Apples **FamilyControls / ManagedSettings / DeviceActivity**.
- Das **Family-Controls-Entitlement** muss bei Apple beantragt werden (dauert erfahrungsgemäß Wochen — früh stellen).
- Die App sieht die gewählten Apps nur als **opake Token**, nicht mit Klarnamen — fürs Konzept unkritisch.
- Mehrere parallele Sperren: pro Sperre ein eigenes DeviceActivity-Schedule.
- E-Mail-Versand braucht ein Mini-Backend, oder: Code wird clientseitig erzeugt, verschlüsselt gespeichert, nur die Mail kennt den Klartext.

## 8. Offene Punkte

- Zeitplan-Modus (werktags 9–17 Uhr automatisch): MVP oder Version 2?
- Strikter Modus (10-Min-Wartezeit auf die Code-Mail): Standard an oder aus?
- Zifferblatt am Gerät testen: Fühlt sich die 12-Std-Skala beim Drehen richtig an?
- Edge Cases für die Umsetzung: Sperren müssen Neustart und Flugmodus überleben (lokal terminiert, nicht server-abhängig); Verhalten bei Zeitzonenwechsel definieren.

---

## Anhang A: Bewusst weggelassen (Anti-Features) — OUT OF SCOPE, NICHT UMSETZEN

**Ausdrücklich kein Bestandteil der Spezifikation — Negativliste, nichts hiervon bauen.** Die 15-Euro-Abo-Konkurrenz (Opal, Jomo und Co.) verkauft Statistiken, Gamification und „Focus Scores". Genau das nicht bauen:

- Keine Nutzungsstatistiken, keine Wochenreports
- Keine Streaks, Bäume oder Belohnungs-Tamagotchis
- Kein Account, kein Login — nur eine E-Mail-Adresse als Code-Empfänger
- **Kein Geldmodell:** kostenlos, Open Source (MIT) — kein Abo, kein Einmalkauf

## Anhang B: Verworfene Namens-Alternativen — OUT OF SCOPE, NICHT UMSETZEN

**Ausdrücklich kein Bestandteil der Spezifikation.** Reines Archiv der Namensfindung — der Name ist entschieden: Kaze (風). Nichts aus dieser Liste implementieren, benennen oder referenzieren.

| Verworfener Name | Japanisch | Lesung | Bedeutung |
|---|---|---|---|
| Sperrstunde | 門限 | mongen | „Torschluss-Zeit" — Sperrstunde/Heimkehrzeit |
| Funkstille | 沈黙 | chinmoku | Schweigen, Stille (fachlich: 無線封止 musen fūshi) |
| Sendepause | 放送休止 | hōsō kyūshi | Sendeunterbrechung im japanischen TV |
| Funkloch | 圏外 | kengai | „außerhalb des Bereichs" — kein Empfang |
| Stubenarrest | 外出禁止 | gaishutsu kinshi | Ausgehverbot (kürzer: 謹慎 kinshin) |
| Erstmal Ruhe | まず静かに | mazu shizuka ni | „erst einmal still" |
| Riegel | 閂 | kannuki | der hölzerne Torriegel (früherer Arbeitstitel) |
