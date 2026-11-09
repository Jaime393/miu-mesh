# NODOS — lo que el workflow dice de los 3 nodos

Fuente: `.github/workflows/mesh-heartbeat.yml`, leído entero. El workflow nombra los nodos solo como A, B y C; no da el nombre del servicio de cada uno.

## Nodo C

- Ping simple (sin parámetro `reactivate`).
- El paso imprime "Ping nodo C verificado vivo".
- Su curl lleva `--max-time 15` y un `|| echo` de reserva: "C timeout cold start normal".

## Nodo A

- Curl con parámetro `reactivate=true`.
- Su curl lleva `--max-time 15` y un `|| echo` de reserva: "A cold start".

## Nodo B

- Curl con parámetro `reactivate=true`.
- Su curl lleva `--max-time 15` y un `|| echo` de reserva: "B cold start".

## Qué escribe el latido

- El paso 1 escribe un JSON `mesh_heartbeat_<epoch>.json` con los campos literales `vive: true`, `mesh: 3`, `nodo: "github"`, `phi: "177->180"`, `filas: 488` y `ts` con la fecha UTC del momento. Los valores de ese JSON son texto literal del archivo, no métricas verificadas por mí.
- El paso 2 sube los JSON como artefacto (`mesh-heartbeat-<número de corrida>`, retención 90 días). Ningún JSON de latido está en el repo.

Las 3 URLs de macros no se reproducen aquí; solo se anota su presencia, una por nodo.

Fuente: `~/workspace/mineria/miu-mesh/MINERIA.md` Parte 1 §1.4.
