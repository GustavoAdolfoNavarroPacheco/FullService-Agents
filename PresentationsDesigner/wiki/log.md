# Bitácora de Actividades (Log)

Registro cronológico de las construcciones, despliegues y mantenimiento de la wiki y presentaciones.

---

## [2026-07-10] build | Multinal S.A.S. — demo interactiva Escenario B (`multinal-escenario-b-demo/`)

* **Encargo:** duplicar `multinal-escenario-c-demo/` (app demo con 3 apartados: Presentación,
  Simulador Interactivo, Panel de Movimientos) y reemplazar **únicamente** el apartado
  "Presentación" por el contenido del deck `multinal-escenario-b/` (19 láminas), dejando
  Simulador y Panel intactos. Carpeta final renombrada a `multinal-escenario-b-demo/`.
* **Hallazgo técnico:** las 26 láminas de Escenario C dentro del demo usan un sistema de
  componentes propio del shell (`.slide-frame`, `.slide-header`, navegación por
  `data-slide`/`jumpToSlide()` en `script.js`), mientras que el deck de Escenario B usa un sistema
  de diseño completamente distinto (`.s-header`, `.s-body`, `.feat-grid`, tokens propios). No era
  un simple copiar/pegar — se consultó al usuario cómo integrar ambos.
* **Decisión (confirmada con el usuario):** insertar el deck B **tal cual, con su propio diseño**
  (no re-maquetarlo al estilo del shell C). Para lograrlo sin romper nada:
  - Las 19 láminas de B se namespacearon bajo la clase `.b-deck` en una hoja de estilos nueva
    (`docs/style-escenario-b.css`), con todos sus tokens y clases genéricas prefijadas `b-`
    (`.b-gradient-text`, `.b-contact-card`, `.b-badge-confidential`, etc.) para no chocar con las
    clases de igual nombre ya usadas por el shell/Simulador/Panel (`.gradient-text`,
    `.contact-card`, `.badge-confidential`).
  - Cada `<section>` de lámina B conserva las clases `slide` + `data-slide="N"` (1–19) que la
    navegación existente (`navigateSlide()`, contador, barra de progreso) ya sabe manejar —
    solo se le sumó la clase `bdeck-slide` (en vez de `.slide` propio de B) para aplicar su estilo
    visual sin pisar el `.slide` de posicionamiento/show-hide del shell.
  - Se copiaron a `docs/assets/` el logo `logo-cliente.png` y las 14 fuentes locales (`fonts/`)
    que el deck B requiere vía `@font-face`.
  - `state.totalSlides` en `script.js`: `26` → `19`; contador inicial `1 / 26` → `1 / 19`.
* **Verificación:** confirmado con diff que `style.css`, `script.js` (salvo `totalSlides`) y
  `README.md` quedaron sin cambios frente al original; las secciones de Simulador y Panel en
  `index.html` son **byte-idénticas** al original. Navegación probada vía JS en el navegador
  (loop completo de 19 láminas, cambio entre las 3 vistas) — sin errores de consola ni fallos de
  red. El screenshot del navegador del entorno falló por un problema de infraestructura ajeno al
  contenido (se reproduce en una página en blanco); se verificó por `get_page_text` + pruebas
  funcionales en su lugar.
* **Pendiente/nota para el usuario:** el `<title>` y la meta-descripción del `<head>` de
  `multinal-escenario-b-demo/docs/index.html` siguen diciendo "Escenario C" (no se tocaron,
  siguiendo la instrucción de modificar solo el apartado Presentación). Avisar si se desea
  actualizarlos también.

## [2026-07-10] build | Multinal S.A.S. — Plataforma Empresarial 100% Propia, Escenario C (26 láminas)

* **Encargo:** Escenario C (la opción más ambiciosa de 2 para Multinal), a partir de 6 documentos
  fuente (Documento 6C, Alcances RFI/RFP, Cadena de Valor AS-IS, Mapa de Procesos AS-IS,
  Arquitectura Tecnológica AS-IS) y `Multinal SAS - Cotizacion C.xlsx` (11 hojas PRY).
* **Decisión de granularidad (confirmada con el usuario):** el ERP propio agrupa 6 funciones de
  negocio (Compras/Inventarios/Comercial/Facturación/Cartera/Despachos) — cada una recibió su
  propia lámina en vez de una sola lámina "PRY-ERP", igual criterio aplicado por consistencia al
  WMS propio (Recepción/Ubicaciones/Picking-Packing/Despachos-Trazabilidad, 4 láminas).
* **Estructura (26 láminas):** Portada (badge "Escenario C") · Diagnóstico · Visión y 6 Principios
  de Diseño (nueva lámina, sin precedente en B) · Mapa del Ecosistema (5 plataformas + banda de 6
  agentes) · ERP (6 láminas) · WMS (4 láminas) · CRM Ampliado · Firma Electrónica · Gobierno de
  Datos · 6 Agentes de IA (Logístico y Fidelización 100% nuevos; Comercial/Compras/Inventarios/
  Cartera con tarjetas "BASE" heredadas del agente homólogo del Escenario B + tarjetas
  "AMPLIACIÓN C" para las capacidades nuevas — decisión de diseño para no fabricar alcance no
  sustentado en la fuente) · Sustitución de la Arquitectura Legada (nueva lámina: estado de
  ILIMITADA/Siigo/Trazabilidad/Pedbox/Excel) · Inversión y Alcance (**sin cifra**) · Próximos pasos.
* **Verificación:** medición programática de bounding boxes en las 26 láminas → 1 intrusión de
  1px detectada y corregida (texto de `pay-note` recortado); 0 problemas tras el ajuste. Revisión
  visual de portada y 2 láminas densas (CRM Ampliado 4 tarjetas, Sustitución Legada 5 tarjetas).
* **Archivos:** `presentaciones/multinal-escenario-c/` (index.html, styles.css idéntico al de
  Escenario B, assets/ reutilizados), PDF de 26 páginas exportado con Chrome headless.

## [2026-07-10] marca | Nuevo estándar obligatorio: fondo gris claro frío + gradiente de marca

* **Decisión del usuario:** el fondo gris claro neutro usado en Multinal Escenario B deja de ser
  una excepción de un solo deck y pasa a ser el **estándar obligatorio para toda presentación
  nueva** (`CLAUDE.md` Regla 2, reemplaza la regla de fondo oscuro de 2026-07-08). Se le suma un
  requisito nuevo: la gama del **gradiente de marca del cliente** debe superponerse como un
  `--bg-wash` sutil (~5–9% opacidad) sobre la base gris — no queda en fondo gris plano.
* **Logo Campuslands corregido:** el PNG maestro (`recursos/Logo Campuslands Horizontal
  Azul.png`) tiene relleno transparente muy asimétrico (138px arriba vs. 80px abajo sobre 885px
  de alto) que lo hacía ver chico y descentrado a igual `height` CSS. Se recortó a su contenido
  visible + padding simétrico ~6% para el deck de Multinal. Nueva regla en `CLAUDE.md` y
  `wiki/temas-por-cliente.md`: **recortar siempre** el logo maestro antes de copiarlo a
  `assets/` de un deck nuevo, nunca usar el PNG de `recursos/` tal cual.
* **Pendiente (no ejecutado en esta sesión):** los decks previos a esta fecha (Miami Aqua
  Tours, GreenMetal, Mchaileh, Compumax ×2, Ve a la Segura) siguen en el sistema oscuro legado
  y probablemente arrastran el mismo defecto de logo sin recortar — no se tocaron porque el
  usuario pidió actualizar específicamente el deck de Multinal; queda como trabajo futuro si se
  solicita.
* **`wiki/temas-por-cliente.md`** — receta §2 reescrita para el estándar claro; catálogo de
  Multinal actualizado con los tokens reales (antes tenía los tokens oscuros previos a este
  cambio).

## [2026-07-10] ajuste | Multinal Escenario B — correcciones de dato + tema claro (excepción)

* **Correcciones de contenido** (fuente: `_FullServices Cotizaciones Multinal (Proyectos).xlsx`,
  hoja `PRY-008 - Gobierno de Datos`):
  * Slide "Portal de Clientes" (PRY-002): se retiró la mención a FedEx en "Tracking logístico de
    despachos" — Multinal opera flota propia de vehículos de despacho, no usa FedEx.
  * Slide "Portal de Proveedores" (PRY-003): la tarjeta de escalabilidad ahora dice "integración
    con Cadena de Valor" (antes "Workflow").
  * **Renombrado global de PRY-006**: "Motor de Workflow Corporativo" → **"Cadena de Valor"** en
    su propia lámina, el Mapa del Ecosistema y el tag del Agente 7E.
  * **Nuevo módulo PRY-008 — Gobierno de Datos** (lámina 17/19): MDM, diccionario corporativo,
    depuración del legado, linaje y — punto clave del cliente — estándares de captura por
    **código de barras** que reemplazan los catálogos hoy dispersos en **Excel anidados** por
    línea de producto, habilitando el mapeo automático del Portal de Proveedores (PRY-003). El
    deck pasa de 13 a **14 módulos** (18 → **19 láminas**); actualizado el conteo en portada,
    kicker, mapa del ecosistema e Inversión y Alcance.
* **⛔ Excepción de tema — fondo claro (solo este deck):** por pedido explícito del usuario
  ("SOLO SERA EN ESTA PRESENTACION"), `presentaciones/multinal-escenario-b/styles.css` rompe la
  regla de fondo oscuro obligatorio del sistema de diseño: fondo gris claro frío (`--bg-0:#F2F3F5`
  → `--bg-2:#FFFFFF`), texto oscuro (`--text-hi:#14161B`), acentos naranja/índigo profundizados
  para contraste (`--cyan:#A6480A`, `--violet:#5A3FBF`), punto "Confidencial" en azul-cian frío
  más saturado. El logo de Campuslands se cambió a la variante **azul** (antes blanca, invisible
  sobre fondo claro). Esta excepción **no aplica a ningún otro deck** ni cambia la convención
  general de `wiki/sistema-diseno.md`.

---

## [2026-07-09] build | Multinal S.A.S. — Ecosistema Digital Corporativo, Escenario B (18 láminas)

* **Encargo:** Escenario B de 2 propuestas para Multinal S.A.S., a partir de `Multinal SAS -
  Alcances por Proyecto B.xlsx` y `Multinal SAS - Cotizacion B.xlsx`. Regla del cliente: cada
  módulo/agente en **una sola lámina** (nunca repartido en 2), sin límite de número de hojas.
* **Estructura (18 láminas):** Portada (rotulada "Escenario B") · Diagnóstico · Mapa del
  Ecosistema (13 módulos) · PRY-001 a PRY-006 (CRM, Portal Clientes, Portal Proveedores,
  Repositorio Documental, BI, Workflow — una lámina c/u con 5–7 funcionalidades desglosadas de
  la cotización con numeración RF) · PRY-007A a 007G (7 agentes de IA, uno por lámina, con tag
  de integración al módulo core relacionado) · Inversión y Alcance (**sin cifra** — "Valor por
  definir", misma estructura visual `.invest`/`.pay` que otros decks) · Próximos pasos.
* **Paleta:** derivada del logo real (`recursos/Multinal S.A.S.png`, muestreado con PIL) —
  naranja `#FF7A00` del isotipo → índigo `#312883` del wordmark, fondo ámbar-negro teñido, punto
  "Confidencial" en cian frío (regla de marca cálida). Registrada en [[temas-por-cliente]] y
  `presentaciones/_temas-demo/index.html`.
* **Bug corregido en QA:** el primer borrador desbordaba el `.feat-grid` de los módulos de
  5–7 tarjetas hacia el footer (30–110px de intrusión, detectado por el usuario como
  "superposición de texto"); se compactó `feat-card`/`feat-grid` (paddings, fuentes, gaps) y se
  redujo `s-title`/`s-lead`. Verificado con medición programática (bounding boxes) en las 18
  láminas + revisión visual del PDF renderizado con PyMuPDF — 0 desbordes tras el fix.
* **Archivos:** `presentaciones/multinal-escenario-b/` (index.html, styles.css, assets/),
  PDF de 18 páginas exportado con Chrome headless.

## [2026-07-09] lint | Nueva norma obligatoria: balance de espacio (ni vacío ni apretado)

* **Motivo:** tras el ajuste de la timeline de GreenMetal (6 fases en 1 fila dejaba ~40% de la
  lámina vacío debajo), el usuario pidió formalizar la regla: el agente debe evitar espacios
  "vacíos" en las láminas, pero sin caer en el extremo opuesto de apretar todo.
* **`CLAUDE.md`** — nueva regla obligatoria en "Estándar de calidad de diseño": si un bloque deja
  una franja vacía notable (>15–20% de la altura de `.s-body`), redistribuir contenido (más
  filas/columnas, tarjetas/tipografía más grandes, o mayor `gap`/padding) antes de entregar.
* **`wiki/sistema-diseno.md`** — nuevo anti-patrón "Espacio vacío/muerto sin usar" (§5) y nueva
  sección **§5.1 Balance de espacio: ni vacío ni apretado**, con checklist de decisión y el caso
  real de GreenMetal como referencia.

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

