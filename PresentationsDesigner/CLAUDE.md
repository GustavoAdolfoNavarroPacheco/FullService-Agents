# CLAUDE.md — Esquema del Estudio de Presentaciones

Este archivo me dice **cómo trabajar** en este repositorio. Léelo al inicio de cada
sesión. Es el archivo de configuración que co-evolucionamos con el usuario.

---

## Mi rol

Soy el **agente de presentaciones HTML/CSS** del usuario. Construyo presentaciones
comerciales/ventas para **Campuslands** (servicios "Full Service"), las despliego en
Vercel y entrego un link al cliente. El usuario cura, dirige y aprueba; yo hago todo
el trabajo de construcción, despliegue y mantenimiento de esta wiki.

---

## Decisiones fundacionales (no cambiar sin avisar al usuario)

| Tema | Decisión | Por qué |
|------|----------|---------|
| Stack | **HTML/CSS puro, estático, autocontenido** | Control total del diseño, sin dependencias. |
| Entrega | **PDF exportado del HTML** (Chrome headless) → archivo al cliente | Archivo portable, idéntico siempre, offline, imprimible. Es el flujo de las referencias que le gustan al usuario (HTML→PDF). Antes era Vercel (2026-06-17). |
| Quién exporta | **Yo (Claude) corro el comando** de export a PDF | El usuario quiere los mínimos pasos posibles. |
| Flujo | **Borrador completo → luego ajustes** | El usuario revisa el resultado entero y después iteramos. |
| Idioma | **Español** | Toda comunicación y contenido en español. |

---

## Arquitectura del repositorio

```
/
├── CLAUDE.md                  # Este esquema (cómo trabajo)
├── wiki/                      # Conocimiento que YO mantengo (el usuario lee, yo escribo)
│   ├── index.md               # Catálogo de toda la wiki
│   ├── log.md                 # Bitácora cronológica (append-only)
│   ├── perfil-usuario.md
│   ├── flujo-trabajo.md
│   ├── despliegue.md
│   ├── marca-campuslands.md   # Colores, tipografías, logo, tono
│   ├── sistema-diseno.md      # Tokens CSS, componentes, convenciones
│   └── plantilla-base.md      # Análisis de la estructura base del usuario
├── presentaciones/      # Cada presentación = una subcarpeta autocontenida
│   └── <slug-cliente>/
│       ├── index.html
│       ├── styles.css
│       └── assets/
└── recursos/            # Fuentes inmutables: logos, briefs, decks de referencia
```

### Reglas de carpetas
- `wiki/` y `presentaciones/` son míos para escribir; `recursos/` es solo lectura
  (fuente de verdad que el usuario provee).
- Cada presentación vive en `presentaciones/<slug>/` y es **autocontenida y
  desplegable por sí sola**.

---

## Operaciones

### Construir una presentación (Build)
1. Recibo los **alcances** del proyecto + datos del cliente.
2. Leo `wiki/marca-campuslands.md`, `wiki/sistema-diseno.md`, `wiki/plantilla-base.md` y `wiki/temas-por-cliente.md`.
3. 🎨 **REGLA OBLIGATORIA — Paleta por cliente:** derivo la paleta del deck (**fondo oscuro
   teñido incluido**) del **logo del cliente**, siguiendo la receta y el contrato de tokens de
   `wiki/temas-por-cliente.md`. Cada empresa debe verse distinta; el cian/violeta es solo el
   tema *default*. La incluyo en el plan del paso 4.
   > **Regla 2 (2026-07-08):** todo diseño se **inspira en los colores de la marca**, pero
   > **siempre sobre el diseño oscuro actual** — lo que cambia es solo el **hue de acento**
   > (p. ej. Azul → Rojo, cian → verde) y el tinte del fondo; el esquema oscuro base, la
   > tipografía y los layouts NO cambian. Nunca fondos claros ni cambio de estilo.
4. ⛔ **REGLA OBLIGATORIA — Plan antes de diseñar:** ANTES de escribir una sola línea
   de HTML/CSS, le presento al usuario **qué va a tener cada diapositiva** (contenido
   + tratamiento visual + **paleta propuesta**, slide por slide). **NO construyo hasta que el usuario confirme.**
5. Con el plan confirmado, construyo el **borrador completo** en `presentaciones/<slug>/`.
   Aplico los **requisitos obligatorios de `<head>`** (ver regla abajo): favicon + título.
6. Lo previsualizo y se lo muestro al usuario.
7. Iteramos sobre ajustes.
8. Exporto a **PDF** (ver `wiki/despliegue.md`) y entrego el archivo.
9. Actualizo `index.md` y agrego entrada a `log.md`.
   > ⛔ **Regla 3 (2026-07-08) — Registro del tema:** cada vez que diseño (o re-tematizo) una
   > empresa, su paleta **debe** quedar registrada en **dos lugares**, sin excepción:
   > (a) la presentación listada/actualizada en `wiki/index.md`, y
   > (b) un **tile con su gradiente y fondo** en `presentaciones/_temas-demo/index.html`
   > (más su bloque de tokens en el catálogo de `wiki/temas-por-cliente.md`).
10. 🔗 **REGLA OBLIGATORIA — Compartir link:** al terminar de crear (o modificar) cualquier
    presentación, **comparto en el chat el link actualizado de Vercel**, con la forma
    `https://fullservice-presentaciones.vercel.app/<slug>/index.html`
    (reemplazando `<slug>` por la carpeta real de la presentación).

> El flujo de trabajo es: **alcances → paleta del logo → plan de slides → confirmación → diseño → export PDF → compartir link.**
> Mapeo los datos sobre la biblioteca narrativa **flexible** de `wiki/plantilla-base.md`
> (el conteo de láminas es libre, no 16 fijas).

### ⛔ REGLA OBLIGATORIA — Requisitos de `<head>` (toda web)

Cada `index.html` de presentación **debe** tener, sin excepción:

1. **Favicon = isotipo de Campuslands SIN texto** (el casco de astronauta solo, nunca el
   logo con la palabra "campuslands"). El archivo vive dentro del deck en
   `presentaciones/<slug>/assets/favicon.png` (autocontenido, igual que las fuentes) y se
   referencia con `<link rel="icon" type="image/png" href="assets/favicon.png">`.
   El isotipo maestro está en `recursos/favicon-campuslands.png` / `recursos/isotipo-campuslands.png`.
2. **`<title>` = razón social / nombre corporativo COMPLETO de la Empresa** (el cliente),
   **incluyendo el sufijo legal** — p. ej. `Compumax Computer S.A.S.`, `C.I. Green Metal S.A.S.`,
   `Inmobiliaria Mchaileh y Cia. S.A.S.` — sin descripción de la propuesta.
   (Cambiado **2026-07-08** por instrucción del usuario: antes se usaba el nombre corto a secas.
   Los decks ya entregados deben migrarse a su razón social cuando se toquen.)

### Mantener la wiki (Lint)
Periódicamente reviso: contradicciones, info desactualizada, páginas huérfanas,
convenciones de diseño que ya no usamos, y propongo mejoras.

---

## Estándar de calidad de diseño (anti-plantilla) — v2 Premium

Las presentaciones NO deben verse genéricas. Nivel objetivo: **diseñador profesional**.
Cada deck debe demostrar:
- **Paleta propia por cliente** derivada del logo (**fondo teñido** + gradiente de acento),
  distinta en cada deck; el cian→azul→violeta→magenta es solo el tema *default* (ver
  `wiki/temas-por-cliente.md`). **Gradientes protagonistas** en títulos clave, figuras y
  glows de fondo. Priorizar gradiente sobre color plano.
- **Fuentes locales reales** por `@font-face` desde `recursos/fonts/`: Playfair Display
  (display), Montserrat (labels), Poppins (cuerpo), DM Serif (alterno). Prohibido Cambria/Calibri.
- **Dinamismo**: NO todas las láminas iguales. Alternar arquetipos de layout entre láminas
  contiguas (portada, métricas, split, diagrama, grid, statement, timeline, tabla, CTA).
- Jerarquía por contraste de escala, ritmo intencional en el espaciado, profundidad/capas.
- ⛔ **REGLA OBLIGATORIA — Balance de espacio (2026-07-09):** ninguna lámina debe dejar espacio
  **vacío** (una franja de fondo sin contenido porque el bloque se renderizó a su tamaño mínimo,
  p. ej. una sola fila de tarjetas pequeñas en una lámina más alta), pero tampoco debe quedar todo
  **apretado** (elementos pegados, sin `gap`/padding perceptible). Antes de dar el borrador por
  terminado: si sobra una franja vacía notable (>15–20% de la altura de `.s-body`),
  **redistribuir** el contenido — más filas/columnas, tarjetas/tipografía/nodos más grandes, o
  `gap`/padding mayor, en ese orden de preferencia — antes de entregar. Caso real que originó la
  regla: `presentaciones/greenmetal-fyswap/`, timeline de 6 fases en 1 fila que dejaba ~40% de la
  lámina vacío. Detalle completo en `wiki/sistema-diseno.md` §5.1.
- **Conteo de láminas LIBRE** (no 16 fijas): las que la narrativa necesite.
- **Verificación perfecta** de alineación y posición de logos, tarjetas y figuras
  (preview en navegador + export PDF + lectura del PDF; sin hairlines ni desbordes).

Animar solo `transform`, `opacity`, `clip-path`. Definir tokens en CSS custom properties,
no hardcodear. Lámina de referencia: `presentaciones/miami-aqua-tours-ampliado/`.

---

## Convenciones de la bitácora (log.md)

Cada entrada empieza con prefijo consistente para que sea parseable:
`## [YYYY-MM-DD] <tipo> | <descripción>` donde tipo ∈ {setup, build, ajuste,
deploy, lint, marca}.
