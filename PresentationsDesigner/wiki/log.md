# Bitácora de Actividades (Log)

Registro cronológico de las construcciones, despliegues y mantenimiento de la wiki y presentaciones.

---

## [2026-07-09] ajuste | GreenMetal: timeline de 6 fases en 2 filas + precio actualizado

* **Lámina 04 (`presentaciones/greenmetal-fyswap/index.html` + `styles.css`):** la línea de 6 fases
  en una sola fila quedaba pequeña y dejaba mucho espacio vacío debajo. Se reestructuró en
  `.timeline` con dos `.timeline-row` (3 fases arriba, 3 abajo), cada fila con su propia línea
  conectora; se agrandaron nodos, títulos y texto de cada fase para aprovechar el espacio.
  Verificado sin overflow (bottom de `.timeline` a 447px dentro de una lámina de 594px).
* **Precio actualizado (lámina 07 · Inversión):** de `$42.182.849,99` a **`$60.217.679`**.
* Exportado PDF actualizado (`greenmetal-fyswap.pdf`).

---

## [2026-07-08] ajuste | Mchaileh re-tematizado a verde de marca + 3 reglas nuevas

* **Re-tema Mchaileh (`presentaciones/mchaileh-crm-ia/styles.css`):** el deck seguía en la paleta
  *default* cian/violeta pese a que su logo es 100% verde (lima + hoja/esmeralda). Derivé la paleta
  del logo: gradiente **`#9BD534→#4FD07A→#22B06B→#1FA95F`** (hoja/kelly-forward → esmeralda, sin teal)
  sobre **fondo bosque teñido** (`--bg-0:#040A06 … --bg-deep:#081A0F`). Reemplacé todos los rgba
  cian/violeta/azul hardcodeados por verdes; introduje `--bg-deep` (antes `#0B1120` fijo en `.slide`,
  `.glow-a`, `.glow-b`). Se conserva el rojo semántico en la columna "Antes/HOY". Verificado en navegador.
* **Distinción vs GreenMetal:** GreenMetal = lima-forward → teal, fondo casi neutro; Mchaileh = hoja
  → esmeralda, fondo verde-bosque. Catálogo actualizado en `wiki/temas-por-cliente.md`.
* **Regla 1 (título):** `<title>` ahora = **razón social / nombre corporativo completo** (con sufijo
  legal), p. ej. `Compumax Computer S.A.S.`. Antes era el nombre corto a secas. (`CLAUDE.md` §head.)
* **Regla 2 (color):** todo diseño se inspira en los colores de marca **sobre el oscuro base actual**;
  solo cambia el hue de acento (Azul→Rojo, cian→verde) y el tinte del fondo. (`CLAUDE.md` §Build.3.)
* **Regla 3 (registro):** cada empresa tematizada se registra en `wiki/index.md` **y** en un tile de
  `presentaciones/_temas-demo/index.html` (+ bloque en `temas-por-cliente.md`). (`CLAUDE.md` §Build.9.)
* **Demo de temas:** tile Mchaileh actualizado al verde de marca y **agregado tile de Ve a la Segura**
  (faltaba). Pendiente: export a PDF de Mchaileh y compartir link.

## [2026-07-08] marca | Favicon (isotipo Campuslands) + título = nombre de empresa en toda web

* **Regla nueva estipulada en `CLAUDE.md`** (sección "Requisitos de `<head>`"): toda web debe tener
  (1) favicon = isotipo de Campuslands **sin texto** (casco de astronauta), en `assets/favicon.png`
  de cada deck, referenciado con `<link rel="icon" ...>`; (2) `<title>` = **nombre de la empresa**
  cliente, limpio (sin "Propuesta…", sin S.A.S).
* **Isotipo creado:** recorté el texto del "Logo Campuslands Vertical Azul.png" → `recursos/isotipo-campuslands.png`
  y `recursos/favicon-campuslands.png` (256×256). Fuente de verdad del favicon.
* **Regla nueva de flujo:** al terminar de crear/modificar cualquier presentación, **compartir en el chat**
  el link de Vercel `https://fullservice-presentaciones.vercel.app/<slug>/index.html`.
* **Aplicado a los 8 decks existentes** (compumax-asistente-ia, compumax-mejoras-multitipo, greenmetal-fyswap,
  mchaileh-crm-ia, miami-aqua-tours-ampliado, miami-aqua-tours, ve-a-la-segura, _temas-demo). Verificado en
  navegador: favicon 200, título = nombre de empresa.

## [2026-07-08] ajuste | Fix tipografías en producción (Vercel): fuentes autocontenidas por deck

* **Problema:** en Vercel las tipografías caían a fuentes de respaldo (serif/itálica genérica). Causa: los `@font-face` apuntaban a `../../recursos/fonts/…`, ruta que sube fuera de la carpeta del deck; al desplegar cada presentación desde su propia raíz, `recursos/` queda fuera del despliegue → 404 → fallback. También violaba la regla "cada deck autocontenido".
* **Solución:** cada presentación ahora empaqueta sus `.ttf` en su propio `assets/fonts/` y los `@font-face` apuntan a `assets/fonts/…` (ruta relativa local). Verificado sirviendo cada deck desde su raíz: fuentes 200, ruta vieja 404; render correcto en navegador sin errores de consola.
* **Alcance:** 6 decks premium (compumax-asistente-ia, compumax-mejoras-multitipo, greenmetal-fyswap, mchaileh-crm-ia, miami-aqua-tours-ampliado, ve-a-la-segura) + `_temas-demo`.
* **miami-aqua-tours (legacy):** usaba Cambria/Calibri (fuentes de sistema, propietarias, prohibidas por el CLAUDE.md). Convertida a Playfair Display (títulos) + Poppins (cuerpo) empaquetadas. Cambio visual menor, aprobado por el usuario.
* **Convención nueva a mantener:** NUNCA usar `../../recursos/fonts/` en un deck; siempre copiar las fuentes usadas a `presentaciones/<slug>/assets/fonts/` y referenciarlas con ruta relativa local. `recursos/fonts/` es solo la fuente de verdad.

## [2026-07-08] ajuste | Compumax mejoras multi-tipo: inversión simplificada y eliminación de cierre

* Lámina de inversión (pág. 5): se retiró la sección "Forma de pago" (caption + filas 40/40/20). Se conserva título, "Inversión Total" + monto, texto descriptivo y nota "Valor en pesos…". Recompuesta como hero centrado (`.invest-solo`).
* Eliminada por completo la última lámina "Próximos Pasos".
* El deck pasó de 6 a **5 láminas**; footer de la última queda en 05; gap con footer verificado positivo. PDF re-exportado (5 páginas).
* Nota: cambios **específicos de este deck** por pedido del usuario; NO se promovieron al sistema de diseño ni a la plantilla base.

## [2026-07-08] ajuste | Compumax mejoras multi-tipo: unificación de láminas y nuevo precio

* Unificación de contenido: pág. 3 (NLP) + pág. 4 (Catálogo FORZA) → una sola lámina "Inteligencia de tipo y catálogo"; pág. 5 (Ajustes conversacionales) + pág. 6 (QA) → una sola lámina "Conversación y calidad".
* Nuevo componente de diseño `.duo` (dos bloques temáticos por lámina, con barra de acento azul + violeta) agregado al `styles.css` del deck.
* El deck pasó de 8 a **6 láminas**; footers y numeración renumerados; verificado gap positivo con el footer en todas las láminas internas.
* Precio de cotización actualizado: $11.532.730 → **$8.688.486 COP** (pago 40/40/20 sin cambios).
* Re-exportación a PDF verificada (6 páginas, sin desbordes).

## [2026-07-08] build | Cotización de mejoras multi-tipo de inmueble para Compumax

* Nueva presentación independiente `presentaciones/compumax-mejoras-multitipo/`: adenda técnica al Asistente de IA Inmobiliario de Compumax ya entregado (sin usar la palabra "Fase 2" por pedido del usuario).
* Alcance derivado de la hoja "Compumax" del archivo `FullServices NAL 2026 - Compumax Multi-Tipo (1).xlsx`: detección NLP del tipo de inmueble, filtro de catálogo por tipo integrado a FORZA, ajustes conversacionales no-bloqueantes por fase y QA de regresión end-to-end. Incorpora **Oficinas** como quinto tipo (junto a Apartamento/Casa/Local/Lote ya cubiertos).
* 8 láminas con el mismo sistema v2 y gradiente azul de marca Compumax; se reutiliza el logo de Campuslands y el wordmark recreado de Compumax en cada header.
* Inversión $11.532.730 COP · pago 40/40/20 · sin lámina de cronograma (a pedido del usuario).
* Exportación a PDF verificada (8 páginas, sin desbordes).

## [2026-07-07] build | Creación y compilación de la propuesta técnico-comercial para Ve a la Segura

* Generación de propuesta de alcance en HTML/CSS estructurada en 8 diapositivas para Agente Conversacional Orbit.
* Implementación del sistema v2 Premium con paleta de color ámbar y variable `--bg-deep` como placeholder ante falta del logo del cliente.
* Exportación a PDF exitosa. Valores de inversión marcados como "Pendiente".

## [2026-07-06] setup | Inicialización de la base de conocimiento (wiki) para el Agente de Presentaciones

* Creación de la estructura base de la wiki en base al archivo de configuración [CLAUDE.md](file:///c:/Users/Full%20Service/Downloads/PresentationsDesigner/CLAUDE.md) y al manual comercial proporcionado por el usuario.
* Poblado de los siguientes archivos de conocimiento:
  - [index.md](file:///c:/Users/Full%20Service/Downloads/PresentationsDesigner/wiki/index.md): Catálogo y navegación de la wiki.
  - [perfil-usuario.md](file:///c:/Users/Full%20Service/Downloads/PresentationsDesigner/wiki/perfil-usuario.md): Identidad de Campuslands Full Service y audiencias objetivo.
  - [flujo-trabajo.md](file:///c:/Users/Full%20Service/Downloads/PresentationsDesigner/wiki/flujo-trabajo.md): Secuencia de pasos obligatorios (alcance → planificación → diseño → exportación).
  - [despliegue.md](file:///c:/Users/Full%20Service/Downloads/PresentationsDesigner/wiki/despliegue.md): Directivas CSS `@page` y comando de Google Chrome headless para generar PDF.
  - [marca-campuslands.md](file:///c:/Users/Full%20Service/Downloads/PresentationsDesigner/wiki/marca-campuslands.md): Paleta de colores, tipografía y distribución de logotipos.
  - [sistema-diseno.md](file:///c:/Users/Full%20Service/Downloads/PresentationsDesigner/wiki/sistema-diseno.md): Tokens CSS, plantillas en grid y restricciones críticas contra anti-patrones visuales de IA.
  - [plantilla-base.md](file:///c:/Users/Full%20Service/Downloads/PresentationsDesigner/wiki/plantilla-base.md): Especificación secuencial de las 16 diapositivas comerciales estándar.

## [2026-07-06] build | Creación y compilación de la propuesta técnico-comercial para Miami Aqua Tours

* Procesamiento de los requerimientos de la propuesta y extracción del alcance de cotización de `$161.340.551,95 COP` con 22 módulos a partir del archivo Excel `FullServices USA 2026 .xlsx`.
* Creación de la carpeta autocontenida de la presentación en [presentaciones/miami-aqua-tours/](file:///c:/Users/Full%20Service/Downloads/PresentationsDesigner/presentaciones/miami-aqua-tours/) con:
  - [index.html](file:///c:/Users/Full%20Service/Downloads/PresentationsDesigner/presentaciones/miami-aqua-tours/index.html): Estructura semántica de 16 diapositivas.
  - [styles.css](file:///c:/Users/Full%20Service/Downloads/PresentationsDesigner/presentaciones/miami-aqua-tours/styles.css): Estilos e implementaciones del sistema de diseño oscuro de Campuslands con variables CSS oficiales.
  - `assets/`: Logotipo de la marca Campuslands y logotipo del cliente Miami Aqua Tours.
* Exportación a PDF de alta resolución mediante Chrome Headless en modo `--no-margins` a [miami-aqua-tours.pdf](file:///c:/Users/Full%20Service/Downloads/PresentationsDesigner/presentaciones/miami-aqua-tours/miami-aqua-tours.pdf).
* Verificación exitosa del conteo de páginas (16 páginas exactas).

## [2026-07-06] marca | Rediseño del sistema visual a v2 Premium (gradientes + fuentes locales + dinamismo)

* A pedido del usuario, se eleva el nivel de diseño al de un "diseñador profesional",
  tomando como referencia una portada tipo Unidrogas (serif gradiente + brackets + anillos).
* **Cambios en la wiki:**
  - `marca-campuslands.md`: nueva paleta con gradientes protagonistas (cian→azul→violeta→magenta)
    + acento ámbar; tipografía v2 (Playfair Display, Montserrat, Poppins, DM Serif); reglas
    de logos verificadas (logos SÍ en portada; blanco sobre fondo oscuro).
  - `sistema-diseno.md`: bloque `@font-face` a `recursos/fonts/`, tokens v2, utilidades
    (texto en gradiente, brackets, anillos, badge confidencial), 9 arquetipos de layout para
    dinamismo, anti-patrones actualizados (incl. hairline de `background-clip:text` en print),
    y flujo de verificación (preview + PDF).
  - `plantilla-base.md`: **se elimina la regla de 16 láminas fijas**; ahora es biblioteca
    narrativa flexible con conteo libre.
  - `CLAUDE.md` (proyecto y Downloads): estándar de calidad v2; se corrige referencia rota
    `estructura-propuesta.md` → `plantilla-base.md`.
* **Lámina de referencia construida y verificada:** portada de prueba en
  [presentaciones/miami-aqua-tours-ampliado/](file:///c:/Users/Full%20Service/Downloads/PresentationsDesigner/presentaciones/miami-aqua-tours-ampliado/index.html)
  con export a PDF (1 página, sin hairlines, fuentes locales y gradiente OK).

## [2026-07-06] build | Deck completo Miami Aqua Tours v2 (8 láminas) sobre el alcance v0.1

* Se completó la propuesta [miami-aqua-tours-ampliado](file:///c:/Users/Full%20Service/Downloads/PresentationsDesigner/presentaciones/miami-aqua-tours-ampliado/index.html)
  a **8 láminas** (< 10, según pedido), cada una con layout distinto para dinamismo:
  Portada · El Reto (split+cita) · La Solución (4 pilares) · Capacidades Venta (grid M1–M3) ·
  Capacidades Operación (grid M4–M6) · Diferenciador QR (flujo + nota M7) · Inversión (statement) ·
  Cuándo Empezamos (CTA + contacto).
* Fuente de datos: `Miami Aqua Tours - Cotizacion de Alcance.pdf` (v0.1). Cotización de cierre
  **$268.900.920 COP** y contacto **Gabriela Pedraza Rueda** (Directora Full Service Global,
  gabriela.pedraza@campuslands.com, +57 300 302 8555) provistos por el usuario.
* **Verificación:** export a PDF con Chrome headless → **8 páginas exactas** (confirmado por
  `/Count` del PDF; el conteo por form-feeds de pdftotext infla en 1). Se ajustó la paginación a
  `page-break-before` entre láminas para evitar página fantasma. Logos, gradientes, fuentes y
  alineación revisados lámina por lámina.

## [2026-07-06] ajuste | Miami Aqua Tours v2 → precio en USD y fusión de capacidades

* **Precio:** la cotización pasó de `$268.900.920 COP` a **USD $84,000** (por pedido del usuario:
  ya no se cotiza en COP). Actualizado el número grande y el subtexto de la lámina de Inversión.
* **Fusión de láminas:** se unificaron las dos láminas de capacidades (Venta M1–M3 y Operación
  M4–M6) en **una sola** con grid compacto 3×2 (`.cards6`, bullets condensados). El deck bajó de
  **8 a 7 láminas**. Renumerados eyebrows y footers subsiguientes.
* **Verificación:** export a PDF → **7 páginas exactas** (`/Count 7`); sin desbordes en la lámina
  fusionada (medición en navegador: 0 overflow en body y en las 6 tarjetas); precio USD confirmado.

## [2026-07-06] build | Nueva propuesta C.I. Green Metal S.A.S. — FySwap (8 láminas)

* Cliente nuevo: **C.I. Green Metal S.A.S.** ("Minería Urbana", reciclaje/exportación de metal).
  Presentación en [presentaciones/greenmetal-fyswap/](file:///c:/Users/Full%20Service/Downloads/PresentationsDesigner/presentaciones/greenmetal-fyswap/index.html)
  sobre el módulo **FySwap** (Logística de Salida y Aseguramiento de la Calidad).
* **Fuente de datos:** `GreenMetal_Cotizacion_de_Alcance_FySwap_Recalculado.pdf`. Total real
  **$42.182.849,99 COP**, esfuerzo **85 días**, prioridad Alta. Moneda COP (confirmado por el usuario).
* **Logo del cliente:** extraído del PDF fuente `GM-PRO-TI-2026-001.pdf` con PyMuPDF y se le hizo
  transparente el fondo negro → `assets/logo-cliente.png` (verde sobre oscuro, ideal).
* **Diseño:** se clonó el sistema v2 de miami-aqua-tours-ampliado pero con la **paleta desplazada
  al verde de GreenMetal** (lima→esmeralda→teal→cian). Layouts nuevos: `.timeline` (6 fases) y
  `.qc-grid` (5 controles QA/QC). 8 láminas: Portada · Reto · Solución FySwap · Proceso (timeline) ·
  QA/QC · Integraciones & Usuarios · Inversión · Cuándo Empezamos (contacto Gabriela Pedraza).
* **Verificación:** export a PDF → **8 páginas exactas** (`/Count 8`); medición en navegador sin
  desbordes en las láminas internas; logos Campuslands (izq) + Green Metal (der) en todas.

## [2026-07-07] build | Mchaileh S.A.S — CRM Inmobiliario + Asistente IA (13 láminas)

* Cliente nuevo: **Mchaileh S.A.S** (inmobiliaria). Presentación en
  [presentaciones/mchaileh-crm-ia/](file:///c:/Users/Full%20Service/Downloads/PresentationsDesigner/presentaciones/mchaileh-crm-ia/index.html)
  sobre un **CRM inmobiliario con IA integrada** (asistente Luce/Lucía).
* **Fuentes de datos:** `Mchaileh_CRM_Cotizacion_Alcance_Recalculado.pdf` (autoritativa) +
  `FullServices NAL 2026 - Mchaileh.xlsx` (días por especialidad) + `..._v0.1_1.pdf` (alcance abierto).
  Total **$24.917.398,68 COP**, esfuerzo **52.5 días**, prioridad Alta. Moneda **COP** (confirmado).
  Reparto de días: Frontend 11.5 · IA 10 · Backend 13 (BS 4.5 + BM 8.5) · QA 7.5 · UX 5 · DB 3.5 · Impl. 2.
* **Decisiones confirmadas por el usuario (AskUserQuestion):** COP · pago **40/40/20** · desglose
  de inversión **por área (8 filas)**, no ítem por ítem.
* **Logo del cliente:** `mchaileh_logopng.png` era transparente pero con **texto negro** (invisible
  sobre fondo oscuro). Se generó `assets/logo-cliente-blanco.png` con PIL recoloreando los píxeles
  grises/negros a blanco y conservando el verde de la casa/eslogan. Se usa la variante blanca en
  portada e internas.
* **Diseño:** clon del sistema v2 (paleta de marca con gradientes). Layouts nuevos reutilizables:
  `.stats` (4 cifras de línea base), `.arch` (diagrama de 3 capas: canales → núcleo CRM+IA → valor),
  `.cards4` (grid de 4 módulos), `.ba` (antes/después), `.who` (quiénes somos), `.team` (esfuerzo +
  8 chips de rol), `.timeline` (5 fases), `.invest2` (tabla por área + pago 40/40/20). 13 láminas:
  Portada · Lo que nos contaron · Reto · Solución/Arquitectura · Capacidades I · Capacidades II ·
  IA en foco · Antes/Después · Quiénes somos · Equipo & Esfuerzo · Cronograma · Inversión · Cierre.
* **Fix técnico (hairline):** los `<div>` con `background-clip:text` (`.tag`, `.num`, `.n`, `.big`)
  dibujaban la línea del gradiente en el borde de la caja al exportar (confirmado con crop a 320dpi).
  Se corrigió con `display:inline-block; width:fit-content;` para que la caja se ajuste al glifo
  (ver [[sistema-diseno]] §5). Los gradientes inline (`<em>`/`<span>`: título, totales) ya eran limpios.
* **Verificación:** export a PDF → **13 páginas exactas** (11×6.1875in); 0px de desborde interno en
  las 13 láminas (medición en navegador); sin errores de consola; crops a 150–320dpi confirman
  gradientes, fuentes locales y logos correctos. Puerto de preview movido a 5599 (5500 ocupado por VS Code).

## [2026-07-07] build | Compumax S.A.S — Asistente de IA Inmobiliario (14 láminas)

* Cliente nuevo: **Compumax S.A.S** (inmobiliaria/constructora). Presentación en
  [presentaciones/compumax-asistente-ia/](file:///c:/Users/Full%20Service/Downloads/PresentationsDesigner/presentaciones/compumax-asistente-ia/index.html)
  sobre un **Asistente de Ventas Inteligente** (clon Andes Constructora) integrado a **FORZA ERP**.
* **Fuentes de datos:** `Compumax_Cotizacion_Alcance_Recalculado.pdf` (autoritativa, 6 págs) +
  `Compumax_Cotizacion_Alcance_Final.pdf` (alcance detallado) + `FullServices NAL 2026 - Compumax.xlsx`.
  Total **$72.399.974,24 COP**, esfuerzo **154.5 días**, prioridad **Muy Alta**. Moneda COP, pago 40/40/20
  (confirmado por el usuario). Desglose por **6 bloques** (verificado que suman exacto al total):
  Estructura 5.525.993 · F1 Descubrimiento 9.778.955 · F2 Diseño 8.488.483 · F3 Desarrollo 25.162.910 ·
  F4 QA/Piloto 12.298.358 · F5 Transversales 11.145.276.
* **Logo del cliente:** NO vino como archivo (solo imagen en el chat). Confirmado con el usuario:
  **recrear el wordmark**. Se reconstruyó "Compumax" en HTML con Poppins local (`.wm`: "Compu" gris
  #9AA0A6 + "max" azul #1B9BD7), usado en portada e internas. Si luego llega el PNG oficial, se cambia.
* **Diseño:** clon del sistema v2 partiendo de `mchaileh-crm-ia/styles.css`, con la **paleta de
  gradiente sesgada al azul de marca Compumax** (`--grad-brand` cian→#1B9BD7→azul→índigo, sin magenta).
  Layout nuevo: `.statement` + `.type-row` (4 tipos de inmueble: apartamento/casa/local/lote, con SVGs).
  14 láminas: Portada · Oportunidad · Reto · Solución/Arquitectura · Cómo conversa (flow 4 pasos) ·
  Capacidades I · Capacidades II · Multi-tipo de inmueble · Seguridad & cumplimiento · Metodología
  (timeline 5 fases) · Quiénes somos · Equipo & Esfuerzo · Inversión · Cierre.
* **Nota de datos:** los días por especialidad del Excel sumaban ~164 con ruido; se usó el total
  autoritativo del PDF (**154.5 días**) y en la lámina de equipo se muestran las disciplinas sin
  días por rol (para no exponer cifras que no cuadran).
* **Verificación:** export a PDF → **14 páginas exactas** (11×6.1875in); 0px de desborde interno en las
  láminas 2–14 (los 74px de la portada son las `deco-rings` decorativas recortadas por `overflow:hidden`,
  patrón ya validado); wordmark, gradiente azul y fuentes locales confirmados por crops a 150dpi.

## [2026-07-07] marca | Theming por cliente — paleta (y fondo) derivados del logo

* **Motivo:** el usuario pidió que la paleta deje de ser repetitiva; **cada empresa** debe tener
  colores distintos y los **fondos** deben adaptarse al color del logo, manteniendo el estilo premium.
* **Cambio de sistema:** se introduce el token `--bg-deep` (antes `#0B1120` hardcodeado en
  `.slide`/`.glow-a`/`.glow-b`) para que el **fondo sea 100% tematizable**. Se define el **contrato de
  theme-tokens** (lo único que cambia por cliente) y una **receta** logo→paleta (hue análogo para el
  gradiente; fondo teñido con S≈10–22% / L≈4–9%; glows y bordes con el hue; texto tinte leve).
* **Nueva página wiki:** [[temas-por-cliente]] con receta, contrato y **catálogo de paletas** listo
  para pegar (Campuslands default, Compumax azul, GreenMetal lima/teal, Mchaileh esmeralda/teal +
  ejemplos cálido y púrpura). Regla: dos clientes del mismo color se separan por sub-hue.
* **Prueba visual:** [`presentaciones/_temas-demo/`](file:///c:/Users/Full%20Service/Downloads/PresentationsDesigner/presentaciones/_temas-demo/index.html)
  — misma lámina en 6 paletas con fondo teñido distinto (render por Chrome headless, verificado).
* **Docs actualizados:** `CLAUDE.md` (nuevo paso obligatorio "Paleta por cliente" en Build + flujo +
  estándar de calidad), `wiki/sistema-diseno.md` (`--bg-deep` + anti-patrón "no reusar paleta"),
  `wiki/marca-campuslands.md` (cian/violeta = tema default), `wiki/index.md`.
* **Pendiente (opcional, si el usuario lo pide):** re-tematizar los decks existentes (Compumax,
  GreenMetal, Mchaileh, Miami) pegando su bloque de [[temas-por-cliente]] en `:root` — hoy Mchaileh y
  Compumax aún usan base azul-noche; GreenMetal tiene acentos verdes pero fondo aún navy.

## [2026-07-07] build | Ve a la Segura — Agente Conversacional Orbit (8 láminas, corrección de borrador)

* Cliente nuevo: **Ve a la Segura** (nombre legal en documentos fuente: "Vianla Segura"; se usa
  "Ve a la Segura" por ser el nombre que usa el usuario). Presentación en
  [presentaciones/ve-a-la-segura/](file:///c:/Users/Full%20Service/Downloads/PresentationsDesigner/presentaciones/ve-a-la-segura/index.html)
  sobre el **agente conversacional Orbit** (atención multicanal, clasificación de eventos, venta de
  boletería, voz clonada).
* **Fuentes de datos:** `Ve a la segura.pdf` (alcance sin precios, "pendiente de costear") +
  `Ve a la Segura.xlsx` (hoja "Vianla Segura", costeo real). Total **$21.808.295,04 COP**, ~48 días.
  Moneda COP, pago 40/40/20 (confirmado por el usuario en turno previo a compactación).
  **Advertencia del Excel:** "días por especialidad NO validados... no usar como cotización final sin
  validación" — por eso NO se agregó lámina de equipo con días por rol, y la lámina de inversión
  incluye una nota honesta: "Estimación preliminar del alcance v0.1; sujeta a validación técnica final."
* **Este era el primer deck con el sistema de theming por cliente** (ver [[temas-por-cliente]]).
  Sin logo oficial de Ve a la Segura → paleta **"concierto"** confirmada por el usuario (violeta-negro
  + gradiente rosa→magenta→violeta→índigo) y wordmark recreado ("VE A LA **SEGURA**", "SEGURA" en
  gradiente de marca).
* **Continuación de sesión:** el borrador ya existía (creado antes de un cambio de modelo/compactación)
  pero con bugs de implementación. Se analizó y corrigió:
  1. **Paleta incorrecta:** el `:root` tenía la paleta cálida ámbar/coral (ejemplo de
     `wiki/temas-por-cliente.md`) en vez de la violeta "concierto" confirmada. Corregido.
  2. **`--bg-deep` hardcodeado** en `.glow-a`/`.glow-b` (`#0B1120`) en vez de `var(--bg-deep)`. Corregido.
  3. **Wordmark placeholder:** portada y 6 headers usaban un `<h2>`/`<div>` con estilo inline en vez
     del patrón `.wm` (como Compumax). Corregido y generalizado (`.wm .b` ahora usa `var(--grad-brand)`
     en vez de un color hardcodeado, reutilizable para cualquier cliente sin logo).
  4. **Lámina "La Solución" rota:** usaba `class="pillars"`, que no existe en este `styles.css`
     (se perdió al clonar desde Compumax). Convertida a `.type-row`/`.type-card` (sí definidas).
  5. **Lámina "Módulos" rota:** usaba `class="cards6"` (no definida, sin `display:grid`) con
     `grid-template-columns` inline sin efecto. Cambiada a `.cards4` (que sí define `display:grid`).
  6. **Lámina de Inversión con placeholders sin rellenar:** `$ [Por Costear] COP`, `[XX] días`.
     Reemplazada por tabla `.invest2` con el desglose real de 5 bloques (suma exacta al total) y
     pago 40/40/20.
  7. **Lámina de Cierre completamente rota:** sin `<header>` (sin logos), sin `.s-eyebrow`, usando
     `rem` en vez de `pt`/`in` (inconsistente con el resto del sistema) y `padding-top:100px` que
     causaba **367px de colisión con el footer** (el checker de overflow contra el borde de la lámina
     no lo detectaba, porque el desborde ocurría *dentro* de la caja de `.slide`, invadiendo la fila
     del footer del grid interno). Reconstruida con el patrón `.cta` (pasos + tarjeta de contacto)
     que ya estaba definido en el CSS pero sin usar.
  8. Colisión menor similar en la lámina "Humanización" (tarjetas M3/M4 vs. footer, gap -1 a -13px)
     y en "Inversión" (nota de pago desbordaba a 3ª línea). Corregidas con padding/tipografía más
     compactos y texto más corto.
* **Lección de verificación:** el chequeo de overflow contra `.slide` (usado en decks previos) **no
  detecta colisiones internas contra el footer** cuando el layout es CSS Grid con filas fijas. Se
  añadió un chequeo adicional: medir el gap entre el último elemento de `.s-body` y el `top` de
  `.s-footer` (debe ser positivo) en cada lámina interna, no solo el desborde contra el borde exterior.
* **Verificación:** export a PDF → **8 páginas exactas** (11×6.1875in); 0px de desborde contra el
  borde de cada lámina Y gap positivo contra el footer en las 7 láminas internas; sin errores de
  consola; crops a 150–300dpi confirman paleta violeta, wordmark y ausencia de hairlines.

