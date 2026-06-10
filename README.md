# Ohana Ops & Defense · ¿Cuál es tu track de AI?

Quiz de diagnóstico de adopción de AI para el equipo de **Ops & Defense Colombia** (programa Ohana).
Basado en [ohana-growth](https://github.com/marcial-moreno/ohana-growth) y [ohana-quiz](https://github.com/danielasoulier-afk/ohana-quiz).

## Qué mide

- **Tipo de usuario**: rol dentro de Ops & Defense (pregunta 1)
- **Nivel de uso real**: stack instalado, frecuencia, profundidad de prompting (preguntas 2–4)
- **Impacto**: qué ha construido y qué resultados concretos ha logrado (preguntas 5–6)
- **Multiplicación**: si comparte lo aprendido con el equipo (pregunta 7)
- **Barreras y sentimiento**: contexto cualitativo para el programa (preguntas 8–9)
- **Quién responde**: nombre y correo Nubank (el diagnóstico no es anónimo)

El resultado asigna uno de tres tracks:

| Track | Perfil | Score de adopción |
|---|---|---|
| Track 1 | Explorador | 0–40% |
| Track 2 | Practicante | 41–70% |
| Track 3 | Multiplicador | 71–100% |

## Captura de respuestas (Google Sheets)

El diagnóstico **no es anónimo**: pide nombre completo y correo Nubank, y envía cada
respuesta a un Google Apps Script al hacer submit. Para activarlo:

1. Crea un Google Sheet y abre **Extensiones → Apps Script**.
2. Pega un script `doPost(e)` que haga `appendRow` con los campos del payload JSON:
   `fecha`, `equipo`, `nombre`, `correo`, `rol`, `stack`, `frecuencia`, `profundidad`,
   `construido`, `impacto`, `multiplicar`, `barrera`, `emocion`, `score`, `habitos`, `track`.
3. Publica como **Web App** (ejecutar como tú, acceso: cualquier persona).
4. Copia la URL del Web App y pégala en `index.html`, en la constante `APPS_SCRIPT_URL`.

Ejemplo de Apps Script:

```javascript
function doPost(e) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  const d = JSON.parse(e.postData.contents);
  sheet.appendRow([
    d.fecha, d.nombre, d.correo, d.rol, d.stack, d.frecuencia, d.profundidad,
    d.construido, d.impacto, d.multiplicar, d.barrera, d.emocion,
    d.score, d.habitos, d.track
  ]);
  return ContentService.createTextOutput('ok');
}
```

Mientras `APPS_SCRIPT_URL` sea `'TU_URL_AQUI'`, no se envía nada (modo de prueba).

## Desarrollo local

Es un solo `index.html` estático, sin build:

```bash
open index.html
# o
python3 -m http.server 8000
```

## Despliegue

Publicado con GitHub Pages desde la rama `main` (raíz del repo).
