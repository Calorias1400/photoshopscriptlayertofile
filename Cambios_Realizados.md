# Modificaciones Realizadas al Script "Layer to lyrics v2.jsx"

## Resumen de Cambios

Se han realizado las siguientes modificaciones al script para cumplir con los requisitos solicitados:

1. **Orden de exportación de abajo hacia arriba**
2. **Numeración empezando en 1**
3. **Nombre simplificado con prefijo y número**

## Cambios Detallados

### 1. Cambio del Orden de Procesamiento (Líneas 2093-2094)

**ANTES:**
```javascript
for( var i = 0; i < dupObj.artLayers.length; i++) {
```

**DESPUÉS:**
```javascript
var layerCounter = 1; // Counter starting from 1
for( var i = dupObj.artLayers.length - 1; i >= 0; i--) { // Process from bottom to top
```

**Explicación:** El bucle ahora empieza desde la última capa (`length - 1`) y va hacia la primera (`0`), procesando las capas de abajo hacia arriba. También se agregó un contador independiente que empieza en 1.

### 2. Cambio en la Construcción del Nombre del Archivo (Líneas 2189-2192)

**ANTES:**
```javascript
var fileNameBody = fileNamePrefix;
fileNameBody += "_" + zeroSuppress(i, 4);
fileNameBody += "_" + layerName;
```

**DESPUÉS:**
```javascript
var fileNameBody = fileNamePrefix;
fileNameBody += "_" + layerCounter; // Use counter starting from 1
// Remove the layer name from the filename
```

**Explicación:** 
- Se eliminó el uso de `zeroSuppress(i, 4)` que creaba números con ceros (ej: `0001`, `0002`)
- Se eliminó la concatenación del nombre de la capa (`layerName`)
- Ahora usa `layerCounter` que empieza en 1 y es independiente del índice del bucle

### 3. Incremento del Contador (Línea 2201)

**ANTES:**
```javascript
dupObj.artLayers[i].visible = false;
}
```

**DESPUÉS:**
```javascript
dupObj.artLayers[i].visible = false;
layerCounter++; // Increment counter for next layer
}
```

**Explicación:** Se agregó el incremento del contador al final de cada iteración para asegurar que cada capa exportada tenga un número consecutivo.

### 4. Cambio en el Procesamiento de LayerSets (Línea 2215)

**ANTES:**
```javascript
for( var i = 0; i < dupObj.layerSets.length; i++) {
```

**DESPUÉS:**
```javascript
for( var i = dupObj.layerSets.length - 1; i >= 0; i--) { // Process layer sets from bottom to top
```

**Explicación:** Para mantener consistencia, los grupos de capas (layerSets) también se procesan de abajo hacia arriba.

## Resultado Final

### Antes de los cambios:
- Orden: De arriba hacia abajo
- Numeración: Empezaba en 0 con formato `0000`
- Nombre: `PREFIJO_0000_NombreCapa.png`

### Después de los cambios:
- Orden: De abajo hacia arriba (última capa primero)
- Numeración: Empieza en 1 con formato simple
- Nombre: `PREFIJO_1.png`, `PREFIJO_2.png`, etc.

## Ejemplo Práctico

Si tienes un documento con capas llamadas "Fondo", "Texto", "Logo" (de arriba hacia abajo en Photoshop) y eliges "FONDO" como prefijo:

**Antes:**
1. `FONDO_0000_Fondo.png` (primera exportada)
2. `FONDO_0001_Texto.png` 
3. `FONDO_0002_Logo.png` (última exportada)

**Después:**
1. `FONDO_1.png` (Logo - última capa, primera exportada)
2. `FONDO_2.png` (Texto)
3. `FONDO_3.png` (Fondo - primera capa, última exportada)

Los cambios aseguran que el orden de exportación sea inverso (de abajo hacia arriba), la numeración empiece en 1, y el nombre sea limpio con solo el prefijo elegido y el número correspondiente.