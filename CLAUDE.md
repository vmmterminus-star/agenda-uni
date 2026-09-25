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
