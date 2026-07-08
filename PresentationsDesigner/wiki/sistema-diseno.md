# Sistema de Diseño y Tokens CSS (v2 — Premium con gradientes)

Reglas de maquetación, tokens y patrones para construir decks HTML/CSS de nivel
"diseñador profesional", con **dinamismo entre láminas**, **gradientes** y **fuentes
locales**. La lámina de referencia verificada es
`presentaciones/miami-aqua-tours-ampliado/`. Ver marca en [[marca-campuslands]] y
narrativa flexible en [[plantilla-base]].

---

## 1. Fuentes locales (`@font-face`)

⚠️ **REGLA DE AUTOCONTENCIÓN (obligatoria — evita el bug de tipografías en Vercel):**
las fuentes de cada deck viven **dentro** de su propia carpeta, en
`presentaciones/<slug>/assets/fonts/`, y se referencian con ruta relativa **local**
`assets/fonts/…`. **NUNCA** usar `../../recursos/fonts/`: esa ruta sube fuera de la
carpeta del deck y, al desplegar la presentación desde su propia raíz (Vercel), los
`.ttf` dan 404 y el navegador cae a fuentes de respaldo. `recursos/fonts/` es solo la
**fuente de verdad**; al crear un deck se copian a `assets/fonts/` los pesos que se usen.

Al construir: `mkdir -p assets/fonts` y copiar desde `recursos/fonts/` los `.ttf` usados.
Bloque canónico (cargar solo los pesos que se usen):

```css
@font-face { font-family:'Playfair Display'; src:url('assets/fonts/PlayfairDisplay-Bold.ttf')       format('truetype'); font-weight:700; font-style:normal; font-display:swap; }
@font-face { font-family:'Playfair Display'; src:url('assets/fonts/PlayfairDisplay-Black.ttf')      format('truetype'); font-weight:900; font-style:normal; font-display:swap; }
@font-face { font-family:'Playfair Display'; src:url('assets/fonts/PlayfairDisplay-Italic.ttf')     format('truetype'); font-weight:400; font-style:italic; font-display:swap; }
@font-face { font-family:'Playfair Display'; src:url('assets/fonts/PlayfairDisplay-BlackItalic.ttf')format('truetype'); font-weight:900; font-style:italic; font-display:swap; }
@font-face { font-family:'DM Serif Display'; src:url('assets/fonts/DMSerifDisplay-Regular.ttf')     format('truetype'); font-weight:400; font-style:normal; font-display:swap; }
@font-face { font-family:'Montserrat';       src:url('assets/fonts/Montserrat-Medium.ttf')          format('truetype'); font-weight:500; font-style:normal; font-display:swap; }
@font-face { font-family:'Montserrat';       src:url('assets/fonts/Montserrat-SemiBold.ttf')        format('truetype'); font-weight:600; font-style:normal; font-display:swap; }
@font-face { font-family:'Montserrat';       src:url('assets/fonts/Montserrat-Bold.ttf')            format('truetype'); font-weight:700; font-style:normal; font-display:swap; }
@font-face { font-family:'Poppins';          src:url('assets/fonts/Poppins-Light.ttf')              format('truetype'); font-weight:300; font-style:normal; font-display:swap; }
@font-face { font-family:'Poppins';          src:url('assets/fonts/Poppins-Regular.ttf')            format('truetype'); font-weight:400; font-style:normal; font-display:swap; }
@font-face { font-family:'Poppins';          src:url('assets/fonts/Poppins-SemiBold.ttf')           format('truetype'); font-weight:600; font-style:normal; font-display:swap; }
```

---

## 2. Tokens (`:root`)

```css
:root{
  --bg-0:#05070F; --bg-1:#0A0E1A; --bg-2:#0D1424; --bg-deep:#0B1120; /* fondo profundo — TEMATIZABLE por cliente, ver [[temas-por-cliente]] */
  --bg-glow-a:rgba(53,208,240,.12); --bg-glow-b:rgba(139,92,246,.12);
  --text-hi:#FFFFFF; --text-mid:#C3CBDA; --text-lo:#7A8699; --text-faint:#4A5468;
  --cyan:#35D0F0; --blue:#4A7DFF; --violet:#8B5CF6; --magenta:#C05CF6; --amber:#F5A623; --lime:#A6E22E;
  --grad-brand:linear-gradient(100deg,#35D0F0 0%,#4A7DFF 42%,#8B5CF6 76%,#C05CF6 100%);
  --grad-cyan:linear-gradient(120deg,#35D0F0,#4A7DFF);
  --grad-violet:linear-gradient(120deg,#6E8BFF,#C05CF6);
  --font-display:'Playfair Display',Georgia,serif;
  --font-serif-alt:'DM Serif Display',Georgia,serif;
  --font-label:'Montserrat','Segoe UI',sans-serif;
  --font-body:'Poppins','Segoe UI',sans-serif;
  --slide-w:11in; --slide-h:6.1875in; --pad:0.72in; --radius:14px;
}
```

### Fondo con vida (usar en `.slide`, variando ángulos por lámina)
```css
background:
  radial-gradient(120% 90% at 82% 18%, var(--bg-glow-a) 0%, transparent 45%),
  radial-gradient(120% 120% at 12% 92%, var(--bg-glow-b) 0%, transparent 50%),
  linear-gradient(150deg, var(--bg-0) 0%, var(--bg-1) 55%, var(--bg-deep) 100%);
```

> **Fondo tematizable:** `.slide`, `.glow-a` y `.glow-b` usan `var(--bg-deep)` (nunca un
> color profundo hardcodeado). Cada cliente tiñe `--bg-0/1/2/-deep` + glows hacia el hue de
> su logo. Receta, contrato de tokens y catálogo de paletas en [[temas-por-cliente]].

---

## 3. Utilidades de marca (copiar tal cual)

* **Texto en gradiente** — `background:var(--grad-brand); -webkit-background-clip:text; background-clip:text; -webkit-text-fill-color:transparent;`
* **Eyebrow** — `font-family:var(--font-label); font-weight:600; font-size:9.5pt; letter-spacing:.34em; text-transform:uppercase; color:var(--cyan);`
* **Marco de brackets** — 4 `<span class="corner tl/tr/bl/br">` con `border` en 2 lados, color `rgba(74,125,255,.55)`.
* **Anillos concéntricos** — contenedor `.deco-rings` con 3–4 `<span>` circulares de `border rgba(120,150,220,.06–.20)` para profundidad.
* **Badge confidencial** — Montserrat 600 + `::before` punto ámbar con `box-shadow` glow.
* **Barra/píldora de acento** — `background:var(--grad-brand); border-radius:99px;` para separadores intencionales (no líneas planas).

El código de referencia completo vive en
`presentaciones/miami-aqua-tours-ampliado/styles.css`; clónalo como punto de partida.

---

## 4. Dinamismo: NO todas las láminas iguales

La grilla base es flexible, no una plantilla única. Alterna estos arquetipos de layout
para dar ritmo (mín. 4 tratamientos distintos por deck):

1. **Portada** — grid `auto 1fr auto`: topbar (logos) / cuerpo (título gradiente) / meta 4-col.
2. **Métricas** — números gigantes Playfair en tarjetas `--bg-2` con borde superior en gradiente.
3. **Split 2 columnas** — texto a la izq, figura/diagrama a la der (o invertido en la siguiente).
4. **Diagrama de capas / flujo** — bloques conectados, cada capa con su gradiente.
5. **Grid de módulos** — cuadrícula de tarjetas con ícono + título + detalle.
6. **Cita / statement** — una frase enorme centrada con palabra clave en gradiente (respiración total).
7. **Timeline** — fases horizontales con hitos y una línea guía en `--grad-cyan`.
8. **Tabla antes/después** — dos columnas contrastadas (rojo tenue vs verde/cian).
9. **Cierre / CTA** — asimétrico, gran gradiente, datos de contacto.

Reglas de ritmo: varía el **ángulo del glow**, cuál **gradiente** domina, y la
**posición** del bloque protagonista (izq / centro / der) entre láminas contiguas.

---

## 5. Anti-patrones (CERO TOLERANCIA)

* 🛑 **Subrayados/`hr` decorativos bajo títulos.** Jerarquiza con escala, peso, itálica y gradiente.
* 🛑 **Barras rectangulares planas** como separadores en header/footer. El header/footer flotan sobre el fondo. Si necesitas separador, usa una **píldora en gradiente**, no un bloque plano.
* 🛑 **Texto corrido centrado.** Alinear a la izquierda. El centro se reserva para el título de portada, métricas individuales o láminas tipo "statement".
* 🛑 **Desbordamiento / falta de aire.** Deja ≥30% de aire negativo. Si no cabe, **divide en más láminas** (el conteo es libre; ver [[plantilla-base]]).
* 🛑 **Color plano donde debería haber gradiente.** Priorizar gradientes (marca).
* 🛑 **Reusar la misma paleta entre clientes.** Cada deck deriva su paleta —**fondo incluido**— del logo del cliente (ver [[temas-por-cliente]]). El cian/violeta es solo el tema *default* de Campuslands.
* 🛑 **Logos deformados o sobre caja innecesaria.** Fijar solo `height`; verificar transparencia.

### Trampa técnica verificada (print)
El texto en gradiente con `background-clip:text` debe ir en un elemento **`inline`**
(p. ej. `<em>` con `display:inline` y salto con `<br>`). Si el elemento es `display:block`,
Chrome headless dibuja un **hairline** del gradiente en el borde de la caja al exportar a
PDF. Confirmado en la portada de Miami Aqua Tours.

---

## 6. Grilla base e impresión

```css
.slide{
  position:relative; width:var(--slide-w); height:var(--slide-h);
  overflow:hidden; border-radius:var(--radius); page-break-after:always; break-after:page;
}
@page{ size:11in 6.1875in; margin:0; }
@media print{
  html,body{ width:11in; background:none; padding:0;
    -webkit-print-color-adjust:exact; print-color-adjust:exact; }
  .slide{ width:11in; height:6.1875in; margin:0; border-radius:0; box-shadow:none;
    page-break-after:always; break-after:page; }
}
```

---

## 7. Verificación obligatoria antes de entregar

1. **Preview en navegador** (`preview_start` + servidor estático) y captura de cada lámina.
2. **Export a PDF** con Chrome headless (ver [[despliegue]]) y **leer el PDF** para revisar
   fidelidad: colores, gradientes, fuentes locales aplicadas, y **ningún hairline**.
3. **Alineación:** verificar posición de logos, tarjetas y figuras (usar `preview_inspect`
   sobre bounding boxes si hay duda). Nada desalineado ni desbordado.
4. El nº de páginas del PDF debe igualar el nº de láminas construidas.
5. **Chequeo contra el borde de `.slide` NO basta.** En el grid `.internal` (filas fijas
   `38px 1fr 20px`), el contenido de `.s-body` puede desbordar *dentro* de la caja de `.slide`
   e invadir visualmente la fila del `.s-footer` sin que ningún elemento cruce el borde exterior
   de la lámina — un check de "overflow contra `.slide`" no lo detecta. Medir siempre, por cada
   lámina interna, el **gap entre el último elemento de `.s-body` y el `top` de `.s-footer`**
   (debe ser positivo, no solo ≥0 en el borde exterior). Caso real: `presentaciones/ve-a-la-segura/`
   tenía una lámina de cierre con -367px de colisión con el footer que este chequeo adicional detectó.
