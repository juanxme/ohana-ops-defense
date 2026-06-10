# Ohana Ops & Defense · AI Adoption Diagnostic

AI adoption diagnostic quiz for the **Ops & Defense Colombia** team (Ohana program).

## What it measures

- **User type**: role within Ops & Defense (question 1)
- **Actual usage level**: installed stack, frequency, prompting depth (questions 2–4)
- **Impact**: what the person has built and concrete results achieved (questions 5–6)
- **Multiplication**: whether learnings are shared with the team (question 7)
- **Barriers and sentiment**: qualitative context for the program (questions 8–9)
- **Who responds**: Nubank email (the diagnostic is not anonymous)

Respondents don't see their result. At the end, they receive a thank-you message
from the Ohana team. The score, habits, and the calculated track go directly to
the spreadsheet so the team can build each person's support plan.

Each response is assigned one of three tracks:

| Track | Profile | Adoption score |
|---|---|---|
| Track 1 | Explorer | 0–40% |
| Track 2 | Practitioner | 41–70% |
| Track 3 | Multiplier | 71–100% |

## Response capture (Google Sheets)

The diagnostic is **not anonymous**: it asks for the Nubank email up front and
sends each submission to a Google Apps Script when the questionnaire is completed.
To set it up:

1. Create a Google Sheet and open **Extensions > Apps Script**.
2. Paste a `doPost(e)` script that calls `appendRow` with the JSON payload fields:
   `fecha`, `equipo`, `correo`, `rol`, `stack`, `frecuencia`, `profundidad`,
   `construido`, `impacto`, `multiplicar`, `barrera`, `emocion`, `score`, `habitos`, `track`.
3. Deploy it as a **Web App** (execute as you; access: anyone).
4. Copy the Web App URL and paste it into the `index.html` file, in the
   `APPS_SCRIPT_URL` constant.

Apps Script example:

```javascript
function doPost(e) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  const d = JSON.parse(e.postData.contents);
  sheet.appendRow([
    d.fecha, d.correo, d.rol, d.stack, d.frecuencia, d.profundidad,
    d.construido, d.impacto, d.multiplicar, d.barrera, d.emocion,
    d.score, d.habitos, d.track
  ]);
  return ContentService.createTextOutput('ok');
}
```

While `APPS_SCRIPT_URL` is set to `'TU_URL_AQUI'`, nothing is sent (test mode).

## Local development

The project is a single static `index.html` file, with no build step:

```bash
open index.html
# or
python3 -m http.server 8000
```

## Deployment

Published with GitHub Pages from the `main` branch (repository root).
