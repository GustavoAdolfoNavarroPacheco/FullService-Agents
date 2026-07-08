# Guía de Despliegue y Exportación a PDF

Este documento detalla las especificaciones técnicas necesarias para compilar y exportar de forma exitosa las presentaciones desarrolladas en HTML/CSS a archivos PDF limpios y de alta calidad mediante Google Chrome headless.

---

## 1. Configuración CSS Obligatoria para Impresión

Para garantizar que el archivo PDF simule perfectamente una diapositiva en formato horizontal y evite desbordamientos no deseados o márgenes por defecto del navegador, se deben aplicar las siguientes directivas CSS en `styles.css`:

```css
/* Configuración de Página */
@page {
  /* Dimensiones estándar para una proporción de pantalla 16:9 */
  size: 11in 6.1875in; /* Relación de aspecto 16:9 en pulgadas */
  margin: 0; /* Remueve los cabezales y pies de página por defecto de Chrome */
}

/* Optimización de Medios de Impresión */
@media print {
  html, body {
    width: 11in;
    height: 6.1875in;
    margin: 0;
    padding: 0;
    -webkit-print-color-adjust: exact; /* Preserva colores de fondo e imágenes */
    print-color-adjust: exact;
    background-color: #0A0E1A !important; /* Asegura fondo premium en export */
  }

  /* Control de Saltos de Página */
  .slide {
    width: 11in;
    height: 6.1875in;
    page-break-after: always; /* Obliga a cada diapositiva a ser una hoja de PDF */
    break-after: page;
    box-sizing: border-box;
    position: relative;
    overflow: hidden;
  }
}
```

---

## 2. Comando de Exportación a PDF

Para generar el PDF, el agente de Claude ejecutará el comando directo a través del shell de Windows (`powershell`), localizando el ejecutable de Google Chrome.

### Estructura del Comando en Windows (PowerShell):

```powershell
& "C:\Program Files\Google\Chrome\Application\chrome.exe" --headless --disable-gpu --print-to-pdf="C:\Users\Full Service\Downloads\PresentationsDesigner\presentaciones\<slug-cliente>\<slug-cliente>.pdf" --no-margins "file:///C:/Users/Full%20Service/Downloads/PresentationsDesigner/presentaciones/<slug-cliente>/index.html"
```

> [!IMPORTANT]
> * **Rutas Absolutas:** Google Chrome Headless requiere rutas absolutas para el parámetro `--print-to-pdf` y para el archivo de origen `file:///`.
> * **no-margins:** La bandera `--no-margins` es crítica para evitar que Chrome fuerce márgenes blancos alrededor de la lámina azul-negra.

---

## 3. Proceso de Verificación del PDF

Tras la compilación, el agente debe validar visualmente el archivo PDF:
1. **Número de Páginas:** Debe coincidir exactamente con el conteo de diapositivas planificado (por ejemplo, 16 diapositivas = 16 páginas).
2. **Corte de Diapositiva:** Asegurarse de que el texto de una diapositiva no se desborde al inicio de la siguiente debido a un padding excesivo.
3. **Colores y Fuentes:** Confirmar que los colores oscuros del fondo e identidades de marca se rendericen con la opacidad correcta y con las tipografías locales especificadas.
