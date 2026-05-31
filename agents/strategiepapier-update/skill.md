---
name: strategiepapier-update
description: Wöchentlicher Update-Skill für das GreenOnion Strategiepapier. Führt Martin durch einen strukturierten 15-Minuten-Check, stellt die sieben Pflichtfragen zu Cash, Pipeline, Projekten, People und Prioritäten, und aktualisiert dann das Strategiepapier als neue MD-Datei mit aktuellem Datum. Verwende diesen Skill IMMER wenn Martin sagt "Strategiepapier aktualisieren", "wöchentliches Update", "Freitagscheck", "Montagscheck", "Pipeline aktualisieren", "Strategiepapier pflegen" oder ähnliches. Auch triggern wenn Martin das Strategiepapier hochlädt und fragt "was müssen wir updaten" oder "lass uns das durchgehen".
---

# Strategiepapier-Update: Wöchentlicher Pflegeworkflow

## Zweck

15-Minuten-Slot, einmal pro Woche (Freitag oder Montag früh). Martin beantwortet sieben Pflichtfragen. Danach wird das Strategiepapier als neue MD-Datei mit aktuellem Datum ausgegeben. Kein Blabla, keine Wiederholungen, kein Aufwärmen.

## Voraussetzung

Martin lädt das aktuelle Strategiepapier (MD-Datei) per Drag-and-Drop in den Chat. Ohne diese Datei kein Update, weil sonst aus dem Memory rekonstruiert wird und Fehler entstehen.

Falls keine Datei hochgeladen wurde: Direkt ansprechen. "Bitte das aktuelle Strategiepapier hochladen, dann starten wir."

## Ablauf

### Schritt 1: Sieben Pflichtfragen stellen

Alle sieben Fragen in einer einzigen Nachricht stellen, nummeriert, knapp. Nicht aufteilen, nicht dramatisieren.

```
Wöchentlicher Check. Bitte kurz antworten, dann aktualisiere ich das Papier.

1. Cash-Stand heute (EUR) und nächster erwarteter Eingang mit Datum?
2. Pipeline-Änderungen: neue Deals, verlorene Deals, geänderte Closing-Wahrscheinlichkeiten oder Beträge?
3. Abgeschlossene Projekte oder neue Aufträge diese Woche?
4. People/Channel: Änderungen bei Team, Partnern, Referrals?
5. Friedhof: Irgendetwas, das raus oder rein soll?
6. Strategische Änderungen: Produkt, Positionierung, Preise?
7. Top-3-Prioritäten für die nächste Woche?
```

### Schritt 2: Antworten verarbeiten

Antworten durchgehen. Wenn etwas unklar oder widersprüchlich ist, eine gezielte Rückfrage stellen. Nicht mehrere auf einmal. Dann direkt zum Update.

### Schritt 3: Strategiepapier aktualisieren

Vollständige MD-Datei ausgeben mit:
- Datum im Header auf heute aktualisiert
- Alle geänderten Abschnitte eingearbeitet
- Status-Header (Abschnitt 1) immer neu schreiben, weil Cash und Pipeline sich jede Woche ändern
- Dateiname: GreenOnion_Strategiepapier_YYYY-MM-DD.md

## Inhaltliche Regeln beim Schreiben

Diese Regeln gelten für jede Version des Strategiepapiers:

- Kein Nachhaltigkeitsjargon: kein "holistisch", "ganzheitlich", "Synergien", kein "ESG" als Buzzword
- Keine Em-Dashes (— oder –), stattdessen Komma, Punkt, Doppelpunkt oder neuer Satz
- Zahlen immer in EUR, keine Abstraktionen
- Risikohinweise klar benennen, nicht wegoptimieren
- Friedhof ist kein Mülleimer: nur rein was wirklich tot oder geparkt ist, mit kurzem Grund
- Twin Transition ist internes Kompasswort, taucht nach außen nie auf

## Struktur des Strategiepapiers (sieben Abschnitte, immer gleich)

1. Status-Header (Cash, Pipeline-Klammer, Betriebsmodus)
2. Strategische Klammer (Positionierung, Zielgruppe, Engpass-Thema)
3. Produktportfolio (Hauptprodukt, zweite Schiene, Akquise-Motor, Eingestampft)
4. Pipeline-Aggregation (gewichtet, drei Kategorien, Risikohinweise)
5. People und Channel (Intern, Vertrieb extern, Partner aktiv, Partner Wartestellung, Investoren, Referral)
6. Aktive Projekte (je ein Absatz pro Projekt, Status und nächste Aktion)
7. Friedhof

## Wichtig

Das Papier ist maximal drei A4-Seiten. Wenn es länger wird, kürzen, nicht dehnen. Detail gehört in Notion, nicht ins Strategiepapier.
