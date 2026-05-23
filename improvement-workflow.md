# Skill Improvement Workflow

## Überblick

Wöchentlicher n8n-Workflow der schlechte Outputs sammelt, einen Verbesserungsvorschlag
generiert und automatisch einen Pull Request auf GitHub öffnet.

---

## Voraussetzung: Output_Quality Feld in Airtable

In der Tasks-Tabelle (`tblVf20K7VjxP0NEG`) ein neues Feld hinzufügen:

| Feld | Typ | Werte |
|---|---|---|
| `Output_Quality` | Single Select | `Good`, `Acceptable`, `Bad` |

Wer bewertet:
- LinkedIn-Posts: Viktor setzt den Wert bevor Posts rausgehen
- Strategische Outputs (Angebote, Research): Martin nach Verwendung
- Default: leer lassen (kein Feld = nicht bewertet)

---

## Workflow: Skill Improvement Weekly

### Trigger
Schedule: jeden Montag 08:00

### Schritt 1: Airtable - Bad Tasks holen
HTTP Request an Airtable API:
```
GET https://api.airtable.com/v0/apppSRjV2Tn9x6nYX/tblVf20K7VjxP0NEG
  ?filterByFormula=AND(
    {Output_Quality}="Bad",
    IS_AFTER({Created}, DATEADD(TODAY(), -7, "days"))
  )
  &fields[]=Assigned Agent
  &fields[]=Input
  &fields[]=Output
  &fields[]=Output_Quality
  &fields[]=Task_Category
```

### Schritt 2: IF - Bad Tasks vorhanden?
- Wenn 0 Ergebnisse: Workflow beenden (kein PR nötig)
- Wenn Ergebnisse: weitermachen

### Schritt 3: Tasks nach Agent gruppieren
Code Node:
```javascript
const tasks = $input.all().map(item => item.json);
const grouped = {};
for (const task of tasks) {
  const agentId = task.fields?.['Assigned Agent']?.[0] || 'unknown';
  if (!grouped[agentId]) grouped[agentId] = [];
  grouped[agentId].push({
    input: task.fields?.Input || '',
    output: task.fields?.Output || '',
  });
}
return Object.entries(grouped).map(([agentId, tasks]) => ({
  json: { agentId, tasks }
}));
```

### Schritt 4: Pro Agent - aktuellen Skill aus GitHub laden
HTTP Request:
```
GET https://raw.githubusercontent.com/GoGoeeen/greenonion-ai-skills/main/agents/
    {agent_path}/skill.md
```

Agent_path über Airtable Agent-Record holen (GitHub_Skill_Path Feld).

### Schritt 5: Verbesserungsvorschlag generieren
Claude API Call (claude-sonnet-4-20250514):

System Prompt:
```
Du bist ein Skill-Optimierer. Du analysierst fehlerhafte Agent-Outputs und verbesserst
den zugehörigen Skill so, dass die Fehler nicht mehr auftreten.

Regeln:
- Ändere nur was nötig ist. Kein Umschreiben funktionierender Teile.
- Begründe jede Änderung in einem separaten Abschnitt.
- Gib den vollständigen neuen skill.md Inhalt zurück.
- Gib danach einen Abschnitt "## Änderungen" mit maximal 5 Bullet Points was geändert wurde.
```

User Prompt:
```
Aktueller Skill:
{skill_content}

Fehlerhafte Outputs (letzte 7 Tage):
{bad_tasks_formatted}

Erstelle eine verbesserte Version des Skills.
```

### Schritt 6: GitHub Pull Request öffnen

#### 6a: Branch erstellen
```
POST https://api.github.com/repos/GoGoeeen/greenonion-ai-skills/git/refs
Authorization: token {GITHUB_TOKEN}

{
  "ref": "refs/heads/improve/{agent-name}-{YYYY-MM-DD}",
  "sha": "{main branch SHA}"
}
```

SHA von main holen:
```
GET https://api.github.com/repos/GoGoeeen/greenonion-ai-skills/git/ref/heads/main
```

#### 6b: Datei im Branch aktualisieren
```
PUT https://api.github.com/repos/GoGoeeen/greenonion-ai-skills/contents/agents/{agent-name}/skill.md
Authorization: token {GITHUB_TOKEN}

{
  "message": "auto: improve {agent-name} skill based on {N} bad outputs",
  "content": "{base64 encoded new skill content}",
  "branch": "improve/{agent-name}-{YYYY-MM-DD}",
  "sha": "{current file SHA}"
}
```

File SHA holen:
```
GET https://api.github.com/repos/GoGoeeen/greenonion-ai-skills/contents/agents/{agent-name}/skill.md
```

#### 6c: Pull Request erstellen
```
POST https://api.github.com/repos/GoGoeeen/greenonion-ai-skills/pulls
Authorization: token {GITHUB_TOKEN}

{
  "title": "Skill-Verbesserung: {agent-name} ({N} schlechte Outputs)",
  "body": "## Automatisch generiert\n\n{changes_section_from_claude}\n\n## Fehlerhafte Outputs\n{bad_tasks_summary}",
  "head": "improve/{agent-name}-{YYYY-MM-DD}",
  "base": "main"
}
```

### Schritt 7: Slack-Benachrichtigung
Post in #ai-tasks:
```
🔧 Skill-Review bereit: {agent-name}
Basis: {N} schlechte Outputs der letzten Woche
PR: {pr_url}
Bitte reviewen und mergen oder ablehnen.
```

---

## GitHub Token einrichten

1. GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Berechtigungen: `repo` (vollständig)
3. Token in n8n als Credential hinterlegen: Typ "Header Auth", Name "Authorization",
   Value "token {DEIN_TOKEN}"

---

## Was passiert nach dem Merge

n8n lädt skills beim nächsten Task-Run frisch von GitHub. Kein Deployment, kein Neustart.
Der neue Skill ist sofort aktiv.

---

## Eskalations-Regel

Wenn in einer Woche mehr als 5 Bad-Outputs für denselben Agent: kein automatischer PR.
Stattdessen Slack-Nachricht: "Kritische Anzahl schlechter Outputs bei {agent-name}.
Manuelle Überprüfung empfohlen vor automatischem Improvement."
Schwellenwert kann im Code Node angepasst werden.
