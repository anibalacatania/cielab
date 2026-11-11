# CIELAB de vinos a partir de espectros

App web estática que calcula las coordenadas CIELAB (L\*, a\*, b\*, C\*, h) de vinos a partir de sus espectros de absorbancia (380–780 nm). Funciona íntegramente en el navegador: los datos no salen de la computadora.

## Formato de entrada

- Archivo `.xlsx`, `.xls`, `.csv` o `.txt` (o datos pegados desde Excel).
- **Primera columna:** longitud de onda en nm (por ejemplo `longdeonda`).
- **Resto de columnas:** una columna de absorbancia por muestra; el encabezado se usa como nombre (`tratamiento`).
- Puede haber filas vacías arriba del encabezado; se detecta solo.

## Cálculo

Reproduce el script de R original (`colorscience`):

1. T = 10^(−A)
2. `spectra2XYZ(..., ciexyzIn = ciexyz64)` → iluminante D65, observador CIE 1964 10°
3. `XYZ2Lab(illuminant = 'D65', observer = 10)`
4. C\* = √(a\*² + b\*²), h = atan2(b\*, a\*)·180/π

Las tablas de D65 y de las funciones de igualación 10° se copiaron del paquete `colorscience`. Se verificó con las 130 muestras de `Datos_CIELAB_2026.xlsx` contra una reimplementación independiente (diferencias < 1e-11).

Opciones extra: factor de paso óptico (multiplica la absorbancia; 1 = igual que R) y h en 0–360°.

## Publicar en GitHub Pages

1. Crear un repositorio nuevo en GitHub (por ejemplo `cielab`).
2. Subir `index.html`, `cielab.js` y este `README.md` a la raíz.
3. *Settings → Pages → Build and deployment*: Source = **Deploy from a branch**, Branch = **main** / **(root)** → Save.
4. En un minuto queda en `https://<tu-usuario>.github.io/cielab/`.

La lectura de Excel usa [SheetJS](https://sheetjs.com) desde su CDN.
