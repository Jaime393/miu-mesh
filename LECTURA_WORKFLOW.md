# LECTURA_WORKFLOW — mesh-heartbeat.yml

Archivo: `.github/workflows/mesh-heartbeat.yml` (1.362 bytes, 29 líneas). Descripción del texto leído; sus pasos no se ejecutaron en esta minería.

## Identidad

- Nombre del workflow: `MIU Mesh Heartbeat`.
- Disparadores: horario (`cron: '0 */6 * * *'`, cada 6 horas) y `workflow_dispatch`.
- Único job: `heartbeat`, en `ubuntu-latest`. Declara `permissions: contents: write`.
- El job hace checkout (`actions/checkout@v4`) y tiene 2 pasos.

## Paso 1 — "Ping mesh 3 nodos ciclo cerrado"

- Imprime "Ping nodo C verificado vivo".
- Hace 3 curls con `--max-time 15` a 3 URLs de macros de Google Apps Script (script.google.com/macros/s/…), una por nodo: C (ping simple), A y B (con parámetro `reactivate=true`). Las URLs no se reproducen aquí.
- Cada curl lleva un `|| echo` de reserva: "C timeout cold start normal", "A cold start", "B cold start".
- Escribe un JSON `mesh_heartbeat_<epoch>.json` con los campos literales `vive: true`, `mesh: 3`, `nodo: "github"`, `phi: "177->180"`, `filas: 488` y `ts` con la fecha UTC del momento. Los valores de ese JSON son texto literal del archivo, no métricas verificadas por mí.
- Luego lista los JSON con `ls -la *.json`.

## Paso 2 — "Subir huella que persiste"

- Sube los JSON como artefacto (`actions/upload-artifact@v4`), nombre `mesh-heartbeat-<número de corrida>`, ruta `mesh_heartbeat_*.json`, retención 90 días.

## Persistencia

- Ningún JSON de latido está en el repo: el único artefacto del workflow se sube, no se commitea.

Fuente: `~/workspace/mineria/miu-mesh/MINERIA.md` Parte 1 §1.4 (lectura de texto con `get_file_contents`).
