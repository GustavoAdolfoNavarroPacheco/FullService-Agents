# Theming por Cliente — Paleta derivada del logo

> **Regla nueva (2026-07-07):** cada presentación usa una **paleta propia derivada del
> logo del cliente**, incluyendo el **fondo oscuro teñido** hacia el color de marca. Se
> conserva el mismo *estilo* de la plantilla (dark premium, gradientes, fuentes locales,
> layouts); lo único que cambia por cliente es un bloque acotado de tokens. Ver
> [[sistema-diseno]] (contrato de tokens) y [[marca-campuslands]] (la paleta cian/violeta
> pasa a ser solo el tema *default* de Campuslands, no la de todos los decks).

Prueba visual de la gama: [`presentaciones/_temas-demo/index.html`](file:///C:/Users/Full%20Service/Downloads/PresentationsDesigner/presentaciones/_temas-demo/index.html)
(misma lámina, 6 paletas con fondo teñido distinto).

---

## 1. Contrato de theme-tokens (lo único que cambia por cliente)

El resto de `styles.css` es **idéntico** entre decks. Solo se re-definen estos tokens en
`:root`:

```
--bg-0 --bg-1 --bg-2 --bg-deep          /* fondo oscuro TEÑIDO al hue de marca */
--bg-glow-a --bg-glow-b                  /* glows con acentos de marca */
--cyan --blue --violet --magenta         /* 4 paradas del set (nombres se conservan) */
--grad-brand --grad-cyan --grad-violet   /* gradientes construidos con esas 4 paradas */
--card-border                            /* rgba del hue a ~.14–.20 */
--text-mid --text-lo --text-faint        /* tinte MUY leve hacia el hue */
--dot-confidential --dot-confidential-glow /* punto "Confidencial"/acento cálido */
```

> **Cambio estructural obligatorio:** se introduce `--bg-deep` (antes el color profundo
> `#0B1120` estaba *hardcodeado* en `.slide`, `.glow-a`, `.glow-b`). El nuevo `styles.css`
> usa `var(--bg-deep)` en esos tres lugares para que el fondo sea 100% tematizable.

---

## 2. Receta: del logo a la paleta

1. **Extrae** 1–2 colores dominantes del logo (los de marca; ignora negros/grises
   neutros). Define el **hue base H** (y un hue secundario si aplica).
2. **Gradiente insignia (`--grad-brand`):** 3–4 paradas **análogas** alrededor de H
   (rota ±25–55°), de claro-brillante a medio. Deriva `--grad-cyan` (2 paradas frías del
   set) y `--grad-violet` (2 alternas) para variar entre láminas.
3. **Sólidos** `--cyan/--blue/--violet/--magenta`: remapea a las 4 paradas (los nombres se
   conservan por compatibilidad aunque el color ya no sea literalmente cian/violeta).
4. **Fondos** `--bg-0/1/2` y `--bg-deep`: toma el hue H con **saturación baja (S≈10–22%)**
   y **luminosidad muy baja (L≈4–9%)**. Así el negro-base queda teñido (verde-negro,
   azul-noche, ámbar-negro…). `--bg-0` es el más profundo; `--bg-2` (tarjetas) el más claro.
5. **Glows** `--bg-glow-a/b`: dos acentos a alpha **.10–.14**.
6. **Texto** `--text-mid/lo/faint`: tinte muy desaturado hacia H (cohesión, no color pleno).
   `--text-hi` siempre `#FFFFFF`.
7. **Bordes** `--card-border`: `rgba(hue, .14–.20)`.

### Reglas de seguridad
- **Legibilidad primero:** fondos muy oscuros (contraste AA con blanco); nada neón saturado
  como fondo. Los gradientes brillantes solo en texto clave, bordes, píldoras y glows.
- **Marca cálida** (ámbar/rojo/naranja): el punto "Confidencial" y alertas se ponen en un
  **acento frío** (cian) para que resalte; con marca fría, el punto sigue en ámbar.
- **Dos clientes con el mismo color** (p. ej. GreenMetal y Mchaileh, ambos verdes): sepáralos
  por **sub-hue** (lima-forward vs esmeralda/teal-forward) para que no se vean iguales.

---

## 3. Catálogo de paletas por cliente

Bloques listos para pegar en el `:root` de cada `styles.css` (ver valores completos en la
demo). Solo se muestran los tokens que cambian.

### Campuslands — default (tech · azul-violeta)
```css
--bg-0:#05070F; --bg-1:#0A0E1A; --bg-2:#0D1424; --bg-deep:#0B1120;
--bg-glow-a:rgba(53,208,240,.12); --bg-glow-b:rgba(139,92,246,.12);
--cyan:#35D0F0; --blue:#4A7DFF; --violet:#8B5CF6; --magenta:#C05CF6;
--grad-brand:linear-gradient(100deg,#35D0F0 0%,#4A7DFF 42%,#8B5CF6 76%,#C05CF6 100%);
--grad-cyan:linear-gradient(120deg,#35D0F0,#4A7DFF);
--grad-violet:linear-gradient(120deg,#6E8BFF,#C05CF6);
```

### Compumax — azul de marca
```css
--bg-0:#04070E; --bg-1:#061019; --bg-2:#0A1622; --bg-deep:#07172A;
--bg-glow-a:rgba(27,155,215,.14); --bg-glow-b:rgba(46,107,240,.12);
--cyan:#35D0F0; --blue:#1B9BD7; --violet:#2E6BF0; --magenta:#6E8BFF;
--grad-brand:linear-gradient(100deg,#35D0F0 0%,#1B9BD7 40%,#2E6BF0 74%,#6E8BFF 100%);
--grad-cyan:linear-gradient(120deg,#35D0F0,#1B9BD7);
--grad-violet:linear-gradient(120deg,#2AC0E8,#4A7DFF);
--card-border:1px solid rgba(90,150,210,.18);
--text-mid:#BFD3E6; --text-lo:#7690A8; --text-faint:#45566B;
```

### GreenMetal — verde lima/teal
```css
--bg-0:#040A06; --bg-1:#07130D; --bg-2:#0B1B14; --bg-deep:#08160F;
--bg-glow-a:rgba(47,224,194,.13); --bg-glow-b:rgba(55,200,113,.12);
--cyan:#2FE0C2; --blue:#37C871; --violet:#7BD13B; --magenta:#A6E22E;
--grad-brand:linear-gradient(100deg,#A6E22E 0%,#37C871 42%,#1FB6A6 74%,#2FE0C2 100%);
--grad-cyan:linear-gradient(120deg,#2FE0C2,#37C871);
--grad-violet:linear-gradient(120deg,#8BD84A,#1FB6A6);
--card-border:1px solid rgba(120,200,160,.18);
--text-mid:#C3D6C9; --text-lo:#7E9187; --text-faint:#495A50;
```

### Mchaileh — verde esmeralda/teal (distinto de GreenMetal)
```css
--bg-0:#04090A; --bg-1:#061313; --bg-2:#0A1B1D; --bg-deep:#07171C;
--bg-glow-a:rgba(52,224,176,.13); --bg-glow-b:rgba(58,160,224,.11);
--cyan:#34E0B0; --blue:#22C58A; --violet:#1FA6B8; --magenta:#3AA0E0;
--grad-brand:linear-gradient(100deg,#34E0B0 0%,#22C58A 40%,#1FA6B8 74%,#3AA0E0 100%);
--grad-cyan:linear-gradient(120deg,#34E0B0,#22C58A);
--grad-violet:linear-gradient(120deg,#2ACF9E,#3AA0E0);
--card-border:1px solid rgba(90,190,180,.18);
--text-mid:#BFD8D3; --text-lo:#78948F; --text-faint:#46605C;
```

### Ve a la Segura — "concierto" (violeta-negro · rosa→magenta→violeta→índigo)
```css
--bg-0:#0A0612; --bg-1:#120B1E; --bg-2:#1A1129; --bg-deep:#150D22;
--bg-glow-a:rgba(255,111,184,.13); --bg-glow-b:rgba(139,92,246,.13);
--cyan:#FF6FB8; --blue:#E14FD1; --violet:#9B5DE5; --magenta:#6C63FF;
--grad-brand:linear-gradient(100deg,#FF6FB8 0%,#E14FD1 38%,#9B5DE5 72%,#6C63FF 100%);
--grad-cyan:linear-gradient(120deg,#FF6FB8,#E14FD1);
--grad-violet:linear-gradient(120deg,#9B5DE5,#6C63FF);
--card-border:1px solid rgba(180,130,220,.18);
--text-mid:#DCC9EC; --text-lo:#8E7CA3; --text-faint:#55465F;
--lime:#4ADE80; /* check "✓" verde-menta, más fresco que el lima por defecto sobre violeta */
```
Usado sin logo oficial (evento/boletería, sin PNG de marca): wordmark recreado con `.wm` en vez de
`<img>` — ver contrato de wordmark abajo.

### Ejemplos de rango (para marcas fuera de la gama fría)
**Cálido (ámbar/coral)** — punto Confidencial en cian:
```css
--bg-0:#0C0703; --bg-1:#160D06; --bg-2:#20140A; --bg-deep:#1A0F07;
--bg-glow-a:rgba(245,148,60,.14); --bg-glow-b:rgba(224,72,106,.12);
--grad-brand:linear-gradient(100deg,#FFC24B 0%,#F5943C 40%,#F0663C 74%,#E0486A 100%);
--dot-confidential:#35D0F0; --dot-confidential-glow:rgba(53,208,240,.7);
```
**Púrpura/magenta:**
```css
--bg-0:#08050F; --bg-1:#100A1B; --bg-2:#160F24; --bg-deep:#130C22;
--bg-glow-a:rgba(181,123,255,.14); --bg-glow-b:rgba(224,92,192,.12);
--grad-brand:linear-gradient(100deg,#B57BFF 0%,#8B5CF6 40%,#C05CF6 74%,#E05CC0 100%);
```

---

## 4. Aplicación

- **Decks nuevos:** derivar la paleta del logo en el paso de plan (ver [[flujo-trabajo]] y
  el CLAUDE.md raíz) y pegar el bloque de theme-tokens antes de construir.
- **Decks existentes** (Compumax, GreenMetal, Mchaileh, Miami): pueden re-tematizarse con
  su bloque de esta página cuando el usuario lo pida (cambia solo `:root` + los 3 usos de
  `--bg-deep`; el resto no se toca).
