# HikariRO BrowEdit Mod

Versión personalizada de [BrowEdit3](https://github.com/Borf/BrowEdit3) para crear mapas de HikariRO.

## Generador procedural

Esta versión añade una plantilla **Procedural** en `File > New` con:

- perfiles de dungeon, field y pueblo;
- semilla reproducible;
- selección de texturas desde un mapa de referencia;
- generación automática de terreno, paredes y GAT;
- rutas conectadas y zonas bloqueadas coherentes.
- habitaciones separadas y totalmente conectadas;
- caminos alternativos, separación y altura de pared configurables;
- UV de suelo y pared tomados del tipo de superficie correcto.
- selector visual con miniaturas y búsqueda para las texturas de suelo y pared.
- plano de agua desactivado por defecto para evitar solapamientos con el suelo.
- tercera textura visual para crear un borde superior de roca alrededor de las galerías, sin cubrir el mapa completo.
- semilla aleatoria nueva en cada creación, con opción de fijarla para reproducir un diseño.
- refresco completo del renderizador y ocultación inicial de celdas vacías para mostrar únicamente el terreno generado.

La compilación se genera automáticamente desde BrowEdit3 v3.639 aplicando los parches de `patches/`.

El ejecutable compilado se publica como artefacto **HikariRO-BrowEdit-Mod** en GitHub Actions.

El ejecutable incluye la versión en su propio nombre y en `BUILD_INFO.txt` para
evitar confundirlo con instalaciones anteriores de BrowEdit3.

## Remake current map

`File > Remake current map...` crea un mapa nuevo usando el mapa abierto como
plantilla completa. Reorganiza bloques de terreno junto con su GAT y traslada
los objetos contenidos en cada bloque. Conserva las texturas, UV, modelos,
iluminación y agua del original. La semilla permite repetir el resultado y el
mapa fuente nunca se sobrescribe.

Desde v3.650, la selección del nuevo diseño compara primero los bordes de los
bloques y solo valida la conectividad de los mejores candidatos. Esto evita que
la interfaz quede bloqueada durante cientos de análisis completos. El quadtree
se recalcula al guardar el resultado, no durante su previsualización.

Desde v3.651, la selección también compara las alturas GND y la presencia de
superficie a ambos lados de cada unión. El control `Remake strength` permite
limitar el porcentaje de bloques desplazados; el valor inicial del 30 % conserva
gran parte de la estructura original y reduce paredes y desniveles incompatibles.

Desde v3.652, las uniones modificadas se reparan después de reorganizar el mapa:
los caminos compatibles comparten exactamente la misma altura GND/GAT, se
elimina la pared que los separaba y las salidas incompatibles se cierran con
pared y colisión. Las uniones originales que no se desplazan no se modifican.

Desde v3.653, una unión con suelo y GAT transitables en ambos lados siempre se
conserva, aunque exista una diferencia de altura, creando una transición en vez
de cerrarla. La búsqueda evalúa 256 distribuciones de bordes y comprueba la
conectividad completa de las 8 mejores.

Desde v3.654, cuando dos bloques afectados no tienen ninguna unión directa pero
sus aperturas están separadas por un máximo de 8 celdas, se crea una transición
corta de dos celdas de anchura. La transición interpola el terreno, conserva una
textura de suelo válida, elimina las paredes interiores y abre el GAT necesario.

