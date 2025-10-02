# 📑 Agente de Extracción de Catálogo de Partes

Este agente permite extraer de manera **estructurada y validada** la información contenida en un catálogo de repuestos en PDF, **figura por figura**, transformando las tablas en un formato tabular listo para Excel/Google Sheets.

El flujo de trabajo es **interactivo**: el agente procesa una figura, muestra la tabla al usuario, espera confirmación, y solo entonces continúa con la siguiente figura. Esto garantiza **precisión** y **control manual** en la extracción.

---

## 🚀 Funcionalidades

- Procesa un catálogo de repuestos en **formato PDF**.  
- Identifica automáticamente las tablas por cada **FIG. X [NOMBRE]**.  
- Extrae y organiza la información en columnas estructuradas.  
- **Elimina guiones** en los números de parte.  
- Mantiene la **descripción exacta** (en español e inglés, con saltos de línea `<br>`).  
- Maneja casos especiales como cantidades con letras (`UR`, `AP`, `LM`).  
- Exporta en **formato tabla Markdown**, lista para pegar en Excel o Google Sheets.  

---

## 📥 Entrada

- Archivo PDF del catálogo de partes.  
- Opcional: figura específica o rango de figuras a procesar.  

---

## 📤 Salida

Tablas en **formato Markdown** con las siguientes columnas:

1. **REF N** → número de referencia asociado al dibujo de la figura.  
2. **FIGURA** → nombre de la figura (ejemplo: `FIG. 1 CILINDRO`).  
3. **CODIGO N PART N (sin guiones)** → número de parte sin guiones.  
4. **DESCRIPCION (exacta)** → texto original del PDF (manteniendo puntos iniciales y líneas en español/inglés separadas con `<br>`).  
5. **B731** → cantidad correspondiente al modelo.  
6. **OBSERVACIONES / REMARKS** → notas adicionales del catálogo, si existen.  

---

## 🔄 Flujo de Trabajo

1. Detectar la **Figura N** en el PDF.  
2. Extraer todas las filas hasta la siguiente figura.  
3. Normalizar el código (quitar guiones).  
4. Construir la tabla con 6 columnas.  
5. Mostrar al usuario y preguntar:  
   > “¿Confirmas que la extracción de la Figura N es correcta?”  
6. Si el usuario valida, continuar con la siguiente figura.  
7. Si solicita cambios, ajustar y volver a mostrar.  

---

## ⚠️ Casos Especiales

- Si la columna **B731** contiene letras (`UR`, `AP`, `LM`), mover esas letras a **OBSERVACIONES / REMARKS** y dejar **B731** vacío.  
- Si el catálogo presenta diferencias entre imagen y texto, mostrar ambas y pedir confirmación.  
- No generar traducciones inexistentes: mantener solo la información original.  

---

## 📊 Ejemplo de Salida

```markdown
| REF N | FIGURA         | CODIGO N PART N (sin guiones) | DESCRIPCION (exacta)                                  | B731 | OBSERVACIONES / REMARKS |
|-------|----------------|-------------------------------|-------------------------------------------------------|------|--------------------------|
| 1     | FIG. 1 CILINDRO | 2GSE110200                    | CULATA DE CILINDRO COMPLETA<br>CYLINDER HEAD ASSY      | 1    |                          |
| 2     | FIG. 1 CILINDRO | 9502206012                    | .PERNO DE BRIDA<br>.BOLT, FLANGE                      | 1    |                          |
| 3     | FIG. 1 CILINDRO | 9043006817                    | .JUNTA<br>.GASKET                                    | 1    |                          |
