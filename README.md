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
- tercera textura visual para cubrir el terreno superior/exterior, sin huecos blancos.
- semilla aleatoria nueva en cada creación, con opción de fijarla para reproducir un diseño.

La compilación se genera automáticamente desde BrowEdit3 v3.639 aplicando los parches de `patches/`.

El ejecutable compilado se publica como artefacto **HikariRO-BrowEdit-Mod** en GitHub Actions.

El ejecutable incluye la versión en su propio nombre y en `BUILD_INFO.txt` para
evitar confundirlo con instalaciones anteriores de BrowEdit3.
