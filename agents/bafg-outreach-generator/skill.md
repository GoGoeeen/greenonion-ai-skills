---
name: bafg-outreach-generator
description: >
  Automatisierter Outreach-Generator für BaFG/Barrierefreiheits-Leads bei GreenOnion.
  Zieht täglich bis zu 25 qualifizierte Leads aus Supabase (Score unter 80, Branchen: Hotellerie,
  Ärzte, Optiker), bestimmt den richtigen Touch-Typ (Erstkontakt, Follow-up, letzter Touch),
  generiert kanal-spezifische Nachrichten (Email + LinkedIn), schreibt den Status zurück nach
  Supabase und gibt alle Nachrichten als Markdown-Datei aus zum manuellen Versand.
  Verwende diesen Skill IMMER bei Anfragen wie "starte den Outreach-Batch", "generiere die
  heutigen Nachrichten", "Outreach-Run starten", "BaFG Leads bearbeiten", "welche Leads
  bekommen heute eine Nachricht", oder wenn Martin den täglichen Outreach-Prozess anstoßen will.
---

# BaFG Outreach Generator

Täglicher Skill zur automatisierten Generierung von Outreach-Nachrichten für Leads aus dem
Barrierefreiheits-Scan-Pipeline. Läuft als separate Aufgabe nach dem "leadgen daily batch".

## Voraussetzungen

Vor dem ersten Lauf muss die SQL-Migration eingespielt sein:
`references/migration_outreach_columns.sql` → im Supabase SQL Editor ausführen.

Supabase-Instanz: `https://vghixeurkagwfohkvhzf.supabase.co`
Tabellen: `clients`, `accessibility_scans`
Kalender-Link (Konstante): `https://cal.com/greenonion`
Report-API: `https://flow.greenonion.services/api/internal/report-link`
INTERNAL_API_SECRET: lkjsadflkj4m789j65BBAkdLLLKKkfJofd3Kntg

---

## Workflow

### Schritt 1: Leads aus Supabase ziehen

Führe folgende Abfrage gegen Supabase aus (via MCP oder API):

```sql
SELECT
  c.id,
  c.company_name,
  c.salutation,
  c.contact_name,
  c.contact_email,
  c.contact_linkedin_url,
  c.company_url,
  c.nace_code,
  c.outreach_status,
  c.outreach_touch1_at,
  c.outreach_touch2_at,
  a.score,
  a.critical_count,
  a.serious_count
FROM clients c
JOIN accessibility_scans a ON a.client_id = c.id
WHERE
  a.score < 80
  AND (
    c.outreach_status IS NULL
    OR (c.outreach_status = 'touch1_sent'
        AND c.outreach_touch1_at < NOW() - INTERVAL '7 days')
    OR (c.outreach_status = 'touch2_sent'
        AND c.outreach_touch2_at < NOW() - INTERVAL '7 days')
  )
  AND (c.outreach_status IS NULL
       OR c.outreach_status NOT IN ('touch3_sent','responded','converted','excluded'))
  AND (
    -- Hotellerie (aktiv)
    c.nace_code LIKE '55.%'
    -- Ärzte: aktivieren sobald 86.xx-Leads in DB vorhanden
    -- OR c.nace_code LIKE '86.%'
    -- Augenoptiker: aktivieren sobald 47.78-Leads in DB vorhanden
    -- OR c.nace_code = '47.78'
  )
ORDER BY a.score ASC
LIMIT 25;
```

**Wenn keine Leads zurückkommen:** Ausgabe "Kein Outreach-Batch heute. Alle Leads bereits
kontaktiert oder Kriterien nicht erfüllt." und Skill beenden.

---

### Schritt 1b: Report-Links abrufen

Für jeden Lead den Report-Link via API abrufen:

```
GET https://flow.greenonion.services/api/internal/report-link?client_id={c.id}
Authorization: Bearer $INTERNAL_API_SECRET
```

Antwort: `{ "report_url": "https://flow.greenonion.services/report/accessibility/..." }`

**Fehlerbehandlung:**
- 404: Kein Token vorhanden. Lead für Touch 1 überspringen, `outreach_notes = 'Kein Report-Token'`
- 500: Entschlüsselung fehlgeschlagen. Lead überspringen, `outreach_notes = 'Token-Entschlüsselung fehlgeschlagen'`
- Netzwerkfehler: Gesamten Batch abbrechen, keine Supabase-Updates durchführen

---

### Schritt 2: Touch-Typ bestimmen

Für jeden Lead:

| outreach_status   | Aktion        |
|-------------------|---------------|
| NULL              | Touch 1 generieren |
| touch1_sent       | Touch 2 generieren |
| touch2_sent       | Touch 3 generieren |

---

### Schritt 3: Nachrichten generieren

Lade die Templates aus `references/templates.md`.

Für jeden Lead zwei Nachrichten generieren:
- Email (mit Betreff)
- LinkedIn (kürzer, kein Betreff)

**Variablen befüllen:**

| Template-Variable | Quelle                                                         |
|-------------------|----------------------------------------------------------------|
| {{LIEBE_FORM}}    | salutation: "Herr" → "Lieber", "Frau" → "Liebe", unbekannt → entfällt (nur "Guten Tag!") |
| {{ANREDE}}        | salutation (Herr/Frau)                                         |
| {{NACHNAME}}      | contact_name (Hinweis: enthält vollen Namen, letztes Wort nehmen) |
| {{FIRMENNAME}}    | company_name                                                   |
| {{DOMAIN}}        | company_url                                                    |
| {{SCORE}}         | score aus accessibility_scans                                  |
| {{REPORT_LINK}}   | aus accessibility_scans.report_url oder generiert              |
| {{CAL_LINK}}      | https://cal.com/greenonion (Konstante)                         |

**Fallback-Regeln:**
- `contact_name` enthält den vollen Namen (z.B. "Maria Müller"). Für {{NACHNAME}} das letzte Wort verwenden. Falls nur ein Wort vorhanden: direkt verwenden.
- Kein `contact_name`: Anrede durch "Guten Tag!" ersetzen, in outreach_notes vermerken
- Kein Score: Lead überspringen, outreach_status auf 'excluded' setzen, Grund notieren
- Kein `company_url`: Domain aus `contact_email` ableiten (Teil nach @), in outreach_notes vermerken

---

### LinkedIn-Formatierung (STRICT)

LinkedIn-Nachrichten müssen als reiner Plain Text ausgegeben werden, der direkt in LinkedIn Messages
kopiert werden kann – ohne Formatierungsfehler, verschobene Zeilenumbrüche oder zusammengezogene
Absätze.

**Regeln:**

1. Kein Markdown – keine Syntax wie **fett**, *kursiv*, kein Code-Block, keine HTML-Tags
2. Absätze – genau eine Leerzeile zwischen Absätzen (2x Return). Kein einzelner Return mitten im Absatz.
3. Zeilenlänge – max. 70 Zeichen pro Zeile, dann Umbruch. Kein harter Break mitten in einem Satz.
4. Bullets – Format: "- " (Bindestrich + Leerzeichen). Jeder Bullet beginnt in neuer Zeile.
5. Leerzeilen stabilisieren – LinkedIn zieht Leerzeilen manchmal zusammen. Um das zu verhindern,
   Leerzeilen mit einem Zero-Width Space (U+200B) auffüllen:

   [Absatz 1]
   ​
   [Absatz 2]

   Das unsichtbare Zeichen zwischen den Absätzen hält die Leerzeile stabil beim Einfügen.
6. Keine Sonderzeichen – keine Tabs, keine mehrfachen Leerzeichen, keine Non-breaking Spaces
7. Mobile-optimiert – max. 3-5 Zeilen pro Absatz

**Gilt ausschliesslich für LINKEDIN-Nachrichten.** Email-Nachrichten werden normal formatiert.

---

### Schritt 4: Markdown-Output erstellen

Erstelle eine Datei `outreach_YYYY-MM-DD.md` mit folgendem Format:

```markdown
# Outreach Batch: [DATUM]
Gesamt: [N] Leads | Touch 1: [N] | Touch 2: [N] | Touch 3: [N]
Übersprungen: [N]

---

## 1. [FIRMENNAME] | Touch [N] | Score: [SCORE]
**Domain:** [DOMAIN]
**Kontakt:** [ANREDE] [NACHNAME] | [EMAIL]
**Status vorher:** [outreach_status]

### EMAIL
**Betreff:** [BETREFF]

[NACHRICHTENTEXT]

---

### LINKEDIN

[NACHRICHTENTEXT]

---

## 2. [nächster Lead]
...

---
## Übersprungene Leads
| Firma | Grund |
|-------|-------|
| ...   | ...   |
```

---

### Schritt 5: Supabase-Update

Für jeden erfolgreich generierten Lead Status zurückschreiben:

**Touch 1 generiert:**
```sql
UPDATE clients SET
  outreach_status = 'touch1_sent',
  outreach_touch1_at = NOW(),
  outreach_date = NOW(),
  outreach_channel = 'email'  -- default, Martin passt manuell an wenn LinkedIn
WHERE id = '[CLIENT_ID]';
```

**Touch 2 generiert:**
```sql
UPDATE clients SET
  outreach_status = 'touch2_sent',
  outreach_touch2_at = NOW(),
  outreach_date = NOW()
WHERE id = '[CLIENT_ID]';
```

**Touch 3 generiert:**
```sql
UPDATE clients SET
  outreach_status = 'touch3_sent',
  outreach_date = NOW()
WHERE id = '[CLIENT_ID]';
```

**Lead übersprungen (kein Score, kein Report):**
```sql
UPDATE clients SET
  outreach_status = 'excluded',
  outreach_notes = '[GRUND]'
WHERE id = '[CLIENT_ID]';
```

**Wichtig:** Supabase-Updates erst ausführen NACHDEM die Markdown-Datei erfolgreich
ausgegeben wurde. Nie Updates ohne erfolgreiche Ausgabe.

---

### Schritt 6: Zusammenfassung ausgeben

Nach Abschluss kurze Zusammenfassung im Chat:

```
Outreach-Batch [DATUM] abgeschlossen.
- [N] Nachrichten generiert (Touch 1: N, Touch 2: N, Touch 3: N)
- [N] Leads übersprungen
- Supabase aktualisiert
- Datei: outreach_[DATUM].md
```

---

## Fehlerbehandlung

| Fehler                        | Verhalten                                              |
|-------------------------------|--------------------------------------------------------|
| Supabase nicht erreichbar     | Abbruch, Fehlermeldung, keine Updates                  |
| accessibility_scans leer      | Lead überspringen, als 'excluded' markieren            |
| contact_name fehlt            | Fallback-Anrede "Guten Tag!", weiterverarbeiten        |
| report_url fehlt (Touch 1)    | Touch 1 überspringen, in notes vermerken               |
| Supabase-Update schlägt fehl  | Markdown trotzdem ausgeben, Fehler in Zusammenfassung  |

---

## Wichtige Hinweise

- **Nie automatisch versenden.** Nur generieren und ausgeben. Martin versendet manuell.

- **⚠️ ERINNERUNG NACH VERSAND:** Am Ende jedes Outreach-Batch diese Zeile prominent im
  Chat und in der Markdown-Datei ausgeben:
  > "Nicht vergessen: Nach dem Versand in Supabase für jeden Lead den `outreach_channel`
  > auf 'linkedin' oder 'email' korrigieren (Default ist 'email').
  > Tabelle: clients | Feld: outreach_channel"

- **outreach_channel** wird initial auf 'email' gesetzt. Martin passt auf 'linkedin' an,
  wenn er die Nachricht über LinkedIn versendet hat.
- **contact_linkedin_url** ist vorhanden: Bei LinkedIn-Nachrichten diese URL in der
  Zusammenfassung mit ausgeben, damit Martin den richtigen Kontakt direkt öffnen kann.
- **report_url**: Wird via `GET https://flow.greenonion.services/api/internal/report-link?client_id={id}`
  abgerufen. Auth: `Authorization: Bearer $INTERNAL_API_SECRET`. Secret in Cowork unter
  Settings → Secrets hinterlegen, nie im Skill hardcoden.
- **NACE-Erweiterung**: Sobald Ärzte (86.xx) oder Optiker (47.78) in der DB erscheinen,
  die auskommentierten Zeilen im SQL-Filter aktivieren. Kein weiterer Umbau nötig.
- Die Batch-Größe von 25 ist ein hartes Limit, auch wenn mehr Leads qualifiziert wären.
- Score-Schwellenwert: < 80. Leads mit Score >= 80 werden nicht kontaktiert.

---

## Referenzdateien

- `references/templates.md` — Alle Nachrichten-Templates (Email + LinkedIn, 3 Touches).
  **Immer laden bevor Nachrichten generiert werden.**
  Enthält: Kommunikationsregeln, Touch 1 (Benchmarking), Touch 2 (ARIA + Förderung),
  Touch 3 (SEO/GEO-Frame, minimaler CTA "Ja reicht")
- `references/migration_outreach_columns.sql` — SQL-Migration für Supabase