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

Desde v3.655, `Remake strength` se calcula únicamente sobre bloques que contienen
una cantidad significativa de terreno visible. Los bloques negros o casi vacíos
no se seleccionan para el intercambio, por lo que el resultado siempre modifica
zonas construidas del mapa y no una porción invisible del rectángulo exterior.

Desde v3.656, todos los bloques con terreno visible participan en la
reconstrucción y `Remake strength` controla cuántas posiciones anteriormente
vacías se incorporan al nuevo trazado. De este modo también cambia la silueta
general del mapa y se evita generar una copia apenas modificada del original.

Desde v3.657, los bloques trasladados se normalizan a una altura común antes de
reparar sus uniones. El mismo desplazamiento vertical se aplica al terreno GND,
las colisiones GAT y los objetos RSW, evitando fragmentos suspendidos, raíles
enterrados y desniveles heredados de la posición original.

Desde v3.658, la distribución deja de ser una permutación aleatoria simple. El
mapa se ensambla progresivamente comparando los bordes de cada fragmento: suelo,
GAT transitable, paredes y alturas. Se generan 256 trazados compatibles y se
evalúa la conectividad de los 32 mejores antes de elegir el resultado final.

Desde v3.659, la interfaz dispone de localización centralizada, arranca en
castellano de forma predeterminada y permite cambiar entre `Español` y `English`
desde Configuración. La elección queda guardada en `config.json`; nombres de
archivos, mapas, texturas y modelos no se traducen.

Desde v3.660, el resultado se valida por regiones transitables después de
ensamblar y reparar los fragmentos. Las zonas útiles que hayan quedado aisladas
se conectan con transiciones de tres celdas GAT: se crea al mismo tiempo el
suelo GND, se interpolan las alturas, se abren las paredes interiores y se
sincroniza la colisión. La creación se cancela si no es posible obtener un
trazado conectado, evitando guardar remakes con sectores inaccesibles.

Desde v3.661, la conectividad exige que cada celda tenga simultáneamente GAT
transitable y superficie GND visible; de este modo los huecos negros ya no
pueden contar como caminos. Las transiciones automáticas son más anchas. La
ventana inicial de Configuración tiene un tamaño mínimo práctico, recupera el
foco al cerrar los selectores de carpetas/GRF y no se muestran paneles de
edición antes de abrir o crear un mapa.

Desde v3.662, una fase conservadora cierra únicamente huecos de una celda
rodeados por al menos tres superficies visibles, redondeando bordes de bloques
sin rellenar fosos o salas grandes. La búsqueda de conexiones penaliza además
las diferencias de altura para preferir galerías próximas y pendientes suaves.
La ventana de remake explica la semilla de generación y permite obtener otra
semilla mediante un botón dedicado.

Desde v3.663, los bloques con terreno se colocan una única vez dentro de una
huella nueva, compacta, contigua e irregular. Los bloques vacíos ocupan el resto
mediante una permutación completa, eliminando restos del trazado original y
evitando habitaciones duplicadas. La intensidad controla la variación del
crecimiento de la nueva silueta. El historial de cambios permanece cerrado al
iniciar y puede abrirse manualmente desde el menú.

Desde v3.664, las vistas de mapa y sus framebuffer validan el área disponible
antes de redimensionarse o renderizarse. Esto evita enviar dimensiones negativas
a `glTexImage2D` y `glViewport` durante la restauración del diseño de ventanas,
eliminando los errores `GL_INVALID_VALUE` al abrir mapas en paneles pequeños.

Desde v3.665, el remake incorpora una fase final orientada a mapas jugables:
elimina restos de bloques descartados y del borde no cubierto, rellena cavidades
interiores compactas conservando textura y suavizando alturas, descarta objetos
que quedarían flotando en fragmentos eliminados y garantiza una única región
GAT accesible. Los grandes vacíos conectados con el exterior se conservan como
límites naturales de la dungeon.

Desde v3.666, el generador evalúa 512 distribuciones completas, coloca los
bloques más densos en el interior de la nueva silueta y penaliza huecos negros
encerrados, trazados fragmentados y resultados visualmente peores que el mapa
original. Si ningún candidato alcanza el umbral, cancela y elimina los archivos
temporales. El panel `Editar alturas` permanece cerrado al cargar un mapa y se
abre únicamente al seleccionar expresamente el modo de alturas.

Desde v3.667, ningún bloque que contenga suelo visible se descarta como vacío:
esto conserva pasillos estrechos, raíles y puentes que antes podían quedar
cortados. La selección del resultado penaliza también las islas independientes
de suelo y bloquea cualquier celda GAT transitable situada sobre terreno negro.
Si el nombre de salida ya existe, el programa permite sobrescribirlo mediante
una confirmación explícita; conserva temporalmente los archivos RSW, GND y GAT
anteriores y los restaura automáticamente si la generación no termina bien.

Desde v3.668, el remake deja de recolocar cuadrados independientes. Primero
detecta regiones de terreno unidas por superficies anchas y desplaza cada sala,
plataforma o conjunto estructural como una sola unidad. Las alturas relativas y
las paredes internas de cada región se conservan; solamente se reconstruyen las
conexiones estrechas entre regiones. Si las regiones no caben sin solaparse, la
generación se cancela y el mapa anterior permanece intacto.

Desde v3.669, la detección de regiones ajusta automáticamente su umbral para
mapas que inicialmente forman una única superficie conectada. Si el empaquetado
completo no encuentra una distribución válida, usa una reconstrucción
conservadora moviendo una región completa a terreno vacío. La ventana informa
del resultado o del motivo del fallo y limpia también los archivos auxiliares
`.rsw.extra.json` cuando una operación se cancela.

## Descargas verificadas

Las versiones para Windows se publican en **GitHub Releases** como un ZIP
estable acompañado de su suma `SHA-256`. El artefacto de GitHub Actions se
mantiene como resultado técnico de la compilación; para el uso normal se
recomienda descargar siempre la versión publicada en Releases.

Desde v3.670, los mapas densos que no contienen un espacio vacío suficientemente
grande para trasladar una región completa utilizan una reconstrucción mínima de
respaldo. Se intercambian dos fragmentos de terreno con densidad compatible y
el resultado pasa por las mismas reparaciones de uniones y validaciones de
conectividad antes de aceptarse. Así se evita cancelar o duplicar el mapa cuando
el empaquetado por regiones no dispone de espacio libre.

Desde v3.671, el intercambio de fragmentos de emergencia queda eliminado porque
podía producir cortes rectos en mapas densos. En estos casos se amplía únicamente
el mapa de salida con un borde bloqueado y alineado al tamaño de fragmento. Ese
espacio permite recolocar regiones completas conservando su geometría interna,
paredes, alturas, objetos y rutas; el mapa fuente mantiene sus dimensiones.
