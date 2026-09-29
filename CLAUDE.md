# Semestre 2026B (agenda universitaria)

Agenda de la universidad de Valen (CUAAD, arquitectura): materias, tareas, calificaciones, calendario, faltas y cancelaciones. Publicada en https://vmmterminus-star.github.io/agenda-uni/

## Cómo está hecha
- Todo vive en un solo `index.html` (HTML + CSS + JS juntos, JS estilo `var`/ES5). No hay que instalar ni compilar nada.
- Fuente: Josefin Sans. Color de tema: `#F7F4EF`.
- El ícono de iPhone va en base64 y el favicon es un SVG en línea, ambos en el `<head>`.

## Datos (¡cuidado!)
- Se guardan en `localStorage` con el prefijo `p9_`: `p9_mat`, `p9_tar`, `p9_cal`, `p9_fin`, `p9_gen`, `p9_calts`, `p9_pap`, `p9_matpap`, `p9_canc`, `p9_falt`, `p9_seed`, `p9_seedv`.
- La app trae datos iniciales ("seed") versionados con `SEEDV`. Si agregas datos iniciales, sube `SEEDV` y no borres lo que ella ya capturó.
- Nunca cambies nombres de claves ni la forma de los datos sin migrar lo que ya existe.

## Cómo trabajar con Valen
- Valen no programa. Explícale todo en español sencillo, sin tecnicismos.
- Antes de subir cualquier cambio: abre la app en el navegador integrado, prueba el cambio (también en tamaño celular) y revisa la consola.
- Enséñale el resultado. Haz commit y push a `main` solo cuando ella diga que sí. En ~1 minuto queda en línea.
- El repo es público: nada de contraseñas ni datos personales aquí.

## Widget de iPhone y avisos
- Al sincronizar, `widgetResumen()` sube una segunda fila `<código>__widget` a `escuela_sync` (solo si cambió). La lectura normal filtra por código exacto, así que nunca se mezcla con los datos.
- `widget.txt` es el script de Scriptable (tamaños chico, mediano y grande). Va sin llaves: Valen las escribe en el iPhone.
- Los avisos de Pushover viven en Supabase (esquema `avisos`, revisión cada 15 min con pg_cron). Los SQL están en su carpeta ESCUELA, no en este repo.
- Si cambias la forma de `widgetResumen()`, cambia también el script y el SQL.

## Conexión con la agenda personal (acuerdo entre las dos apps)
La agenda personal (`vmmterminus-star/agenda-personal`) muestra las tareas escolares y puede palomearlas. Las dos usan la tabla `escuela_sync` de Supabase. Reglas:
- **Leer:** la personal lee solo `<código escolar>__widget` → `data.tar` = tareas pendientes (sin entregadas ni "no se hizo"), cada una `{i, t, m, c, f, p?, h?, k?, e?, x?}` (`i` id, `t` nombre, `m` materia, `f` fecha límite `AAAA-MM-DD`, `p` fecha preferida, `h` hora `HH:MM`, `k` tipo, `x:1` si es de tesis).
- **Palomear:** la personal hace upsert de una fila por tarea: `code = <código escolar>__hecha__<i>`, `data = {v:1, i, st:2, ts:Date.now()}`. Para deshacer, la misma fila con `st:0` y `ts` nuevo.
- **Nunca** escribe en `<código escolar>` ni en `<código escolar>__widget`.
- Esta app, al sincronizar, lee `like.<código>__hecha__*`, aplica cada fila solo si su `ts` es más nuevo que el de la tarea (las "no se hizo" no se tocan), sube sus datos y luego borra esas filas (`code=eq…&data->>ts=eq…`, así no borra una que se volvió a mandar).
- El widget escolar y los avisos (`avisos.revisar`) ya esconden las tareas con fila `__hecha__` en `st:2`, aunque esta app no se haya abierto.
- Los avisos de Pushover de tareas escolares salen solo de esta app: la personal no debe mandarlos.
