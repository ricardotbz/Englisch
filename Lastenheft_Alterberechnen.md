# Lastenheft – Alter berechnen
## M306 Softwareprojekt mit Qualitätssicherung

**Technische Berufsschule Zürich**  
**Abteilung Informations-Technik**  
**Datum:** 05. Januar 2026  
**Version:** 1.0  
**Autor:** Harald G. Mueller

---

## 1. Auftraggeber und Auftragnehmer

| Rolle | Name/Beschreibung |
|------|-------------------|
| **Auftraggeber (Kunde)** | Anonyme HR-Abteilung / Personalabteilung |
| **Auftragnehmer (Entwickler)** | IT-Projektteam M306 |
| **Projektleiter** | Harald G. Mueller |
| **Kontakt** | Technische Berufsschule Zürich |

---

## 2. Ausgangslage und Problemstellung

Der Kunde benötigt eine automatisierte Lösung zur Berechnung des Alters von Personen basierend auf ihrem Geburtsdatum. Die Lösung soll später für eine **Massenverarbeitung (Batch-Processing)** mehrerer Personen eingesetzt werden können.

**Anforderung:** Die Software darf **keine interaktiven Dialoge** enthalten, sondern muss vollautomatisiert über Kommandozeile aufrufbar sein.

---

## 3. Zielsetzung

Die Entwicklung eines **skriptgestützten Programms**, das:
- Ein Geburtsdatum als Eingabe akzeptiert
- Das Alter exakt berechnet (unter Berücksichtigung von Schaltjahren)
- Das Ergebnis in standardisiertem Format ausgibt
- Eingabefehler robust handhabt
- Batch-fähig ist (automatisiert auf Listen anwendbar)

---

## 4. Funktionale Anforderungen

### 4.1 Eingabe (Input)

| Anforderung | Beschreibung |
|-------------|-------------|
| **Eingabeformat** | Kommandozeilen-Argument im Format `dd.mm.yyyy` |
| **Beispiele** | `15.03.1990`, `29.02.2000`, `01.01.2010` |
| **Batch-Fähigkeit** | Skript kann in einer Schleife mehrfach aufgerufen werden (z.B. für CSV-Dateien mit mehreren Personen) |
| **Keine Dialoge** | Keine interaktiven Eingabeaufforderungen; nur Kommandozeilenargumente akzeptiert |

### 4.2 Validierung (Validation)

| Validierungsregel | Aktion bei Fehler |
|-------------------|------------------|
| **Format-Prüfung** | Eingabe muss Format `dd.mm.yyyy` haben (drei Punkte trennen Tag, Monat, Jahr) | Fehlermeldung: „Format ungültig. Verwende dd.mm.yyyy" + Abbruch |
| **Tag (dd)** | 01–31, abhängig von Monat und Schaltjahr | Fehlermeldung: „Tag ungültig (1–31)" + Abbruch |
| **Monat (mm)** | 01–12 | Fehlermeldung: „Monat ungültig (1–12)" + Abbruch |
| **Jahr (yyyy)** | ≥ 1900 und ≤ Aktuelles Jahr | Fehlermeldung: „Jahr ungültig (≥1900 und ≤ heute)" + Abbruch |
| **Schaltjahr-Prüfung** | 29.02 ist nur in Schaltjahren (teilbar durch 4, außer durch 100 außer durch 400) gültig | Fehlermeldung: „29.02 ungültig für dieses Jahr" + Abbruch |
| **Zukünftiges Datum** | Geburtsdatum darf nicht in der Zukunft liegen | Fehlermeldung: „Geburtsdatum liegt in der Zukunft" + Abbruch |

### 4.3 Verarbeitung (Process)

| Prozessschritt | Beschreibung |
|----------------|-------------|
| **Datums-Parsing** | Aufteilen der Eingabe in Tag, Monat, Jahr |
| **Konvertierung** | Umwandlung zu internem Datumsformat (z.B. Python `datetime`) |
| **Schaltjahr-Behandlung** | Automatische Berücksichtigung von Schaltjahren bei der Berechnung |
| **Differenzberechnung** | Berechnung der exakten Differenz zwischen Geburtsdatum und heute |
| **Aufschlüsselung** | Umwandlung der Gesamtdifferenz in Jahre, Monate, Tage und Gesamttage |

### 4.4 Ausgabe (Output)

| Feld | Beschreibung | Beispiel |
|------|-------------|---------|
| **Format** | Text-Output in fester Struktur | – |
| **Zeile 1** | `Das Alter ist A Jahre B Monate und C Tage, das sind D Tage.` | `Das Alter ist 35 Jahre 10 Monate und 21 Tage, das sind 13102 Tage.` |
| **A (Jahre)** | Volle Jahre seit Geburt | 35 |
| **B (Monate)** | Monate nach dem letzten Geburtstag | 10 |
| **C (Tage)** | Tage nach dem letzten vollständigen Monat | 21 |
| **D (Tage)** | Gesamtzahl der Tage seit Geburt | 13102 |
| **Ausgabeziel** | Standard-Output (stdout) | Kommandozeile / Datei (bei Umleitung) |

---

## 5. Nicht-funktionale Anforderungen

| Anforderung | Beschreibung |
|------------|-------------|
| **Programmiersprache** | Python 3, PowerShell, Linux-Bash oder PHP (Kunde akzeptiert alle) |
| **Plattformunabhängigkeit** | Skript sollte auf Windows, macOS und Linux lauffähig sein (Python empfohlen) |
| **Fehlerrobustheit** | Keine unkontrollierten Abstürze; alle Fehler mit aussagekräftigen Meldungen handhabt |
| **Codequalität** | Gut kommentiert, lesbar, Einhaltung von Coderichtlinien (z.B. PEP8 für Python) |
| **Testbarkeit** | Codestruktur ermöglicht vollständige Unit-Tests aller Funktionen |
| **Performance** | Ausführung in < 1 Sekunde pro Aufruf |
| **Kein UI** | Keine grafische Oberfläche; reine Kommandozeilen-Anwendung |

---

## 6. Annahmen und Rahmenbedingungen

| Annahme | Beschreibung |
|--------|-------------|
| **Systemvoraussetzungen** | Python 3.6+ oder Bash 4.0+ oder PowerShell 5.0+ bereits installiert |
| **Datum-Standard** | Gregorianischer Kalender; Schaltjahre nach ISO-8601 |
| **Referenz-Zeit** | Alter wird relativ zu „heute" (aktuelles Systemdatum) berechnet |
| **Zeitzone** | Irrelevant; nur Datum ohne Uhrzeit |
| **Encoding** | UTF-8 für Ein-/Ausgabe |
| **Abhängigkeiten** | Nur Standard-Bibliotheken (keine externen Pakete erforderlich) |

---

## 7. Abgrenzung (Out of Scope)

Folgende Funktionen sind **nicht** Teil dieser Anforderung:

- Benutzer-Dialoge oder interaktive Eingabeaufforderungen
- Speicherung von Daten in Datenbanken
- Graphische Benutzeroberfläche (GUI)
- Mehrsprachigkeit (nur Deutsch)
- Unterstützung für historische Kalender (z.B. Julianischer Kalender)
- Zeitzonenverwaltung
- Integration mit anderen Systemen (API)

---

## 8. Lieferumfang

| Komponente | Format | Beschreibung |
|-----------|--------|-------------|
| **Quellcode** | `.py` / `.ps1` / `.sh` | Vollständiges, kommentiertes Skript |
| **Dokumentation** | `.txt` / `.md` | Anleitung zur Installation und Verwendung |
| **Tests** | `.txt` / Testbericht | Mindestens 6 Testszenarien inkl. Schaltjahre |
| **Projektplan** | Gantt-Diagramm | Zeitliche Planung der Arbeitspakete |
| **Design-Dokument** | Activity Diagram | Ablaufdiagramm des Skripts |
| **Alle Dokumente** | 1 `.pdf` | Zusammengefasste Abgabe nach Vorgabe |

---

## 9. Akzeptanzkriterien

Das Projekt wird als **erfolgreich abgeschlossen** akzeptiert, wenn:

1. ✅ Das Skript alle Testszenarien erfolgreich besteht (inklusive Schaltjahre-Tests)
2. ✅ Die Eingabevalidierung alle fehlerhaften Eingaben robust handhabt
3. ✅ Das Output-Format exakt der Spezifikation entspricht
4. ✅ Der Code kommentiert und nach Richtlinien geschrieben ist
5. ✅ Ein detaillierter Testbericht vorliegt
6. ✅ Ein Review-Bericht über das „Konkurrenzprodukt" einer anderen Gruppe vorliegt
7. ✅ Alle Dokumentationen (PAP, Pflichtenheft, Design, Code, Tests, Reflexion) in einem PDF vorliegen
8. ✅ Die Abgabefrist (siehe Aufgabenblatt) eingehalten ist

---

## 10. Zeitrahmen und Ressourcen

| Ressource | Umfang |
|----------|--------|
| **Gesamtzeit Erstellung** | 3 Unterrichtslektionen (à 45 min) |
| **Zeit für Review** | 1 Unterrichtslektion |
| **Team-Größe** | 3–5 Personen (ideal: 4) |
| **Rollen** | Projektleiter, Business-Analyst, Testmanager, Entwickler |
| **Hilfsmittel** | Alle erlaubt (Internet-Foren, Dokumentation, StackOverflow etc.) |

---

## 11. Kontakt und Fragen

Bei Fragen oder Unklarheiten: Lehrperson (M306 Dozent)

---

**Freigegeben:** 05. Januar 2026  
**Unterschrift Auftraggeber:** _________________________  
**Unterschrift Auftragnehmer (Projektleiter):** _________________________