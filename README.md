# Resumen de tarjetas → Dashboard

Arrastrás el PDF del resumen de tu tarjeta de crédito y te muestra en qué se te fue la plata: saldo, pago mínimo, gastos por categoría, los comercios donde más gastaste y las cuotas que vienen los próximos meses.

Funciona con resúmenes **Visa** y **Cabal** (formato Banco Credicoop, Argentina).

> 🔒 **Todo se procesa en tu navegador.** El PDF no se sube a ningún servidor. No hay backend.

## Cómo usarlo

1. Descargá o cloná el repo.
2. Abrí `index.html` en el navegador (Chrome, Firefox o Edge).
3. Arrastrá uno o varios PDF de resúmenes. Para probar, usá los de la carpeta `ejemplos/`.

## Qué muestra

- Saldo actual en $ y U$S, pago mínimo, cierre y vencimiento.
- Consumos del período, con un **check que valida que la suma coincide con el "Total Consumos" del resumen**.
- Cargos del banco (comisión, IVA, sellos) separados de los consumos.
- Gastos por categoría y top de comercios.
- Cuotas a vencer por mes.
- Tabla de movimientos con búsqueda y filtros.
- Si cargás varias tarjetas, las muestra por separado o todas juntas.

## Cómo se hizo

La app la escribió **Claude Code** (extensión de VS Code), con el modelo **Claude Opus 5.5** (esfuerzo *Medium*), a partir de un único prompt.

- Tiempo de construcción: **7 min 45 s**, desde que se envió el prompt hasta que la app estuvo lista.
- Antes de escribir el prompt analicé cómo arma cada banco su PDF (fechas, cuotas, pagos, impuestos). Ese trabajo previo no está incluido en el tiempo.

### El prompt

```
En esta carpeta hay dos resúmenes de tarjeta de crédito en PDF (uno Cabal y uno Visa, de un banco argentino). Armame una web de una sola página (index.html, sin backend ni build) que:

1. Me deje arrastrar uno o varios PDF de resúmenes.
2. Lea el PDF en el navegador con pdf.js (desde CDN). No se sube nada a ningún servidor: mostralo en la UI.
3. Detecte si es Cabal o Visa y extraiga:
   - Vencimiento, cierre, saldo actual en $ y U$S, pago mínimo.
   - Cada consumo: fecha, comercio, cuota (si tiene) y monto en pesos o dólares.
   - Cargos del banco (comisión, IVA, sellos) por separado de los consumos.
   - Las cuotas a vencer de los próximos meses.
4. Muestre un dashboard con: tarjetas de totales (saldo, pago mínimo, consumos, cargos), gráfico de gastos por categoría, top comercios, cuotas a vencer por mes y la tabla de movimientos.

Detalles de formato de los PDF:
- Montos con formato argentino: 12.345,67. Un pago/crédito viene con el signo menos al final (512.640,25−, ojo que es el carácter U+2212, no un guion).
- Cabal: fechas dd/mm/aa; la línea es "fecha  comprobante  detalle  pesos  dólares"; las cuotas aparecen en el detalle como "cta 04/06". La tabla "CUOTAS A VENCER" tiene meses como "Octubre−26" y montos "$ 65.350,50".
- Visa: fechas con guiones dd−mm−aa (U+2212) y en el encabezado "07 Oct 26" (meses en español, ej. "Ago"); la línea es "fecha  detalle  cuota  comprobante  pesos  dólares"; las cuotas vienen como "C.06/12". En la línea de IVA hay dos números (base e impuesto): el que cuenta es el último. "Cuotas a vencer" tiene meses "Octubre/26" y montos "$106.050,25".
- Ignorá las líneas de SALDO ANTERIOR y SU PAGO para los gastos, y los textos legales.
- Reconstruí las líneas agrupando los items de pdf.js por coordenada Y.

Categorías simples por palabras clave (supermercado, combustible —YPF—, farmacia, streaming —Netflix/Spotify/Disney—, gastronomía, hogar/electro, otros). "MERPAGO" es Mercado Pago: sacalo del nombre para mostrar el comercio.

Diseño: limpio, modo oscuro, responsive, que se vea bien en un video. Validá que la suma de consumos coincida con el "Total Consumos" del resumen y mostrá un check verde si coincide.
```

## Ejemplos

Los PDF de `ejemplos/` tienen **datos ficticios**: el titular, el banco, los números de cuenta, los comercios y los montos son inventados. Respetan el formato de los resúmenes reales para poder probar la app sin exponer datos de nadie.

## Limitaciones

- Solo reconoce los formatos Visa y Cabal de Banco Credicoop. Otros bancos arman el PDF distinto y el lector habría que adaptarlo (se aceptan PRs 🙂).
- Si el banco cambia el formato del resumen, el lector puede dejar de funcionar.
- No está afiliado a ningún banco ni emisora de tarjetas.

## Licencia

MIT

---

Hecho por [Nicolás Pelichotti](https://www.linkedin.com/in/nicolaspelichotti).