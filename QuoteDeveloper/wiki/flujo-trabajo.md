# Flujo de Trabajo (Cotizaciones)

1. **Recepción de alcances**: El usuario comparte los alcances del proyecto (texto, notas de reunión, brief, etc.) junto con el nombre o razón social del cliente.
2. **Clarificación**: Si hay ambigüedad en el alcance, en la estimación de esfuerzo o en cualquier dato necesario para cotizar, **se debe preguntar directamente al usuario** antes de continuar. No se debe inventar ni completar huecos por cuenta propia.
3. **Generación del PDF**: Se genera el **PDF de cotización de alcance** siguiendo la plantilla visual, organizado por módulos. (La columna *Valor COP* siempre dirá "Pendiente de costear").
4. **Llenado del XLSX**: Una vez el PDF es validado por el usuario, se llena el **XLSX** de cotización copiando la plantilla maestra (`recursos/FullServices - Plantilla Cotizaciones.xlsx`). Se usa el desglose de módulos y detalles técnicos del PDF para llenar únicamente las filas de alcance (L, M, N) y los días estimados (B:K), dejando todo lo demás (fórmulas, totales) intacto.
5. **Verificación**:
   - Ambos archivos reflejan exactamente el mismo alcance.
   - El XLSX recalcula sin errores (usando `scripts/recalc.py`).
   - Ningún estimado de tiempo es menor a 0.5 días.
6. **Entrega**: Se entregan ambos archivos (PDF + XLSX) en la carpeta del cliente `cotizaciones/<slug>/`.
7. **Registro**: Se actualiza `wiki/index.md` y se agrega una entrada a `wiki/log.md`.
