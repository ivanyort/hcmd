# Header Change Mask Decoder

Este proyecto explica, paso a paso, como interpretar el campo `header__change_mask`
producido por los productos de integracion de datos de Qlik. El objetivo es
mostrar que columnas de la change table (tablas `__ct`) fueron actualizadas con
respecto a la tabla de origen.

## Que hace el programa

1. Lee la entrada hexadecimal (`header__change_mask`).
2. Normaliza la entrada (elimina `0x` y completa con cero a la izquierda para
   mantener un numero par de caracteres).
3. Convierte cada byte a binario (8 bits por byte).
4. Aplica la regla del destino Event Hubs:
   - `si`: usa todos los bits (descarta 0).
   - `no`: descarta los ultimos N bits (por defecto 7).
5. Elimina ceros a la izquierda (null-trim).
6. Mapea los bits de derecha a izquierda a las columnas de datos:
   - bit 1 = columna actualizada
   - bit 0 = columna no modificada

## Reglas importantes

- La posicion del bit sigue el ordinal de la columna en la change table.
- Los bits mapean solo columnas de datos; los campos `header__...` no entran en la mascara.
- Los bytes son little-endian y pueden venir sin ceros a la izquierda.
- La cantidad de bits descartados equivale al numero de campos `header__`
  almacenados en la tabla `__ct`. El valor por defecto es 7, pero puede cambiar
  por configuracion. Este valor pasa a 0 cuando el destino es Event Hubs.

## Como usar

1. Abra `index.html` en el navegador.
2. Ingrese el valor hexadecimal de `header__change_mask`.
3. Elija si el destino es Event Hubs (`n` o `y`).
4. Indique cuantos bits finales deben descartarse (por defecto 7).
5. Haga clic en "Decodificar" para ver el resultado y el paso a paso.

## Autor

Ivan Yort <ivan.yort@qlik.com>

## Repositorio

https://github.com/ivanyort/hcmd

