# diseno-4housing — Contexto del proyecto

App de **Diseño** de 4housing: proyectos, tareas/hoja de ruta, minutas, revisiones,
asesorías, auditorías y desvíos/NC. Parte del portal unificado.

## Stack

- **Frontend:** `index.html` + `js/app.js` (el archivo real que carga la app — NO el
  `app.js` de la raíz, que está viejo) + `js/plantilla.js`, `js/campos_input.js`,
  `js/auditoria.js`. Vanilla JS. Acceso con supabase-js (`sb.from('diseno_...')`).
- **Hosting:** GitHub Pages, org `4housing`, repo `diseno-4housing`. URL: https://4housing.github.io/diseno-4housing/
- **Backend:** Supabase unificado → proyecto `wcpkpwxhqdcdljfwzcmy` (wcpk). Login Microsoft (Azure).
- **Edge function:** `auditoria-ia` (deployada en wcpk).

## Datos y acceso

- Tablas con prefijo **`diseno_`** (`diseno_proyectos`, `diseno_tareas`, `diseno_minutas`,
  `diseno_desvios_nc`, …) + vista `v_diseno_proyectos` (security_invoker). RLS por sector `diseno`.
- Roles de app: **coordinador / disenio / lectura** (`esCoord()`, `soyResponsable()`,
  `puedeTildar()`). `PERFIL.nombre` = nombre canónico "Nombre Apellido".
- Roles de proyecto como columnas en `diseno_proyectos`: `resp_diseno`, `resp_documentacion`,
  `resp_tecnico`, `coord_produccion`.

## Reglas de trabajo — NO NEGOCIABLES

> **Criterio, no candado.** Estas reglas son el default. Se pueden saltar si el dueño
> de la decisión (Pablo) lo resuelve explícitamente — pero Claude debe **advertir ANTES**,
> con claridad, que la acción incumple tal regla y qué riesgo tiene, y esperar el OK.
> Claude nunca rompe una regla por su cuenta ni en silencio.

1. **No romper lo que ya funciona.** Preferí agregar antes que modificar; `grep` de los
   usos antes de tocar código compartido; probá lo que tocaste, no solo lo que agregaste.
2. **SQL nunca se ejecuta solo.** Se entrega como `.sql` y lo corre una persona a mano en Supabase.
3. **Orden de deploy:** primero el SQL (si agrega tablas/columnas), después el HTML/JS.
4. **RLS siempre `authenticated`, nunca `anon`** + compuerta de sector (`tiene_sector('...')`).
5. **Secretos nunca en el código** (es público). anon/publishable es pública; tokens/service keys no.
6. **Validá el JS con `node --check`** antes de terminar (incluido `js/app.js`).
7. **Cambios incrementales y aditivos:** una feature por PR, chico y reversible.
8. **Decisiones estructurales se cierran antes de codear.**
9. **Git:** `git pull` antes; ramas por feature + PR o coordinar; commits chicos, en español;
   tras `stash pop`/merge chequeá marcadores de conflicto (`<<<<<<<`) antes de commitear.
10. **Prefijos de tabla por sector:** `diseno_` acá; resto `fhcomercial_`, `labocomercial_`,
    `planificacion_`, `compras_`, `eerr_`, `logistica_`. `core_` reservado.
11. **Datos de negocio nunca al repo público** (dumps `.sql` gitignoreados).
12. **Diagnosticar con evidencia** (`grep`/`diff`), no adivinar. Reportar con fidelidad.

## Cómo entregar
- HTML/JS: archivo completo, validado con `node --check`. SQL: archivo `.sql` aparte, corrido a mano.
