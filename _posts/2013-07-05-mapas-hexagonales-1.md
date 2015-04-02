---
layout: post
title: "Mapas hexagonales (I) Introducción"
date: 2013-07-05 21:07:35
categories: programming java hexagonal-maps 
---

# Introducción

Con esta entrada inauguro una serie dedicada a los **mapas hexagonales** y su representación mediante un lenguaje de programación orientada a objetos como Java. Y empiezo la serie con una introducción al concepto de teselado y las propiedades geométricas y matemáticas del hexágono. 

# Teselados

![]({{ site.url }}/assets/mapas-hexagonales-1/villa-tejada.jpg "Mosaico romano decorativo (Villa Tejada)")
*Mosaico romano decorativo (Villa Tejada)*

Un **teselado** es una superficie recubierta a base de pequeñas piezas puestas unas junto a otras tratando de no dejar huecos. Los teselados han sido utilizados en todo el mundo desde tiempos  antiguos para recubrir suelos y paredes, e igualmente como motivos decorativos de muebles, alfombras, tapices, ropas. El ejemplo típico son los mosaicos romanos, hechos a base de pequeñas piezas de piedra, terracota o vidrio coloreado denominados [teselas](http://es.wikipedia.org/wiki/Tesela). En el ámbito matemático se dice que una figura es **teselante** cuando acoplando piezas de ese tipo es posible recubrir por completo el plano sin dejar huecos ni fisuras; la configuración resultante recibe el nombre de  teselado. Los matemáticos y  los geómetras se han interesado especialmente por los teselados poligonales, es decir, hechos a base de teselas poligonales. 

## Tipos de teselado

  1. **Teselado regular**: cuando todos los polígonos del teselado son regulares e iguales entre sí (se usa un único polígono regular). Sólo existen tres teselados  regulares: la **malla** de triángulos equiláteros, el **reticulado** cuadrado como el del tablero de ajedrez y la **configuración hexagonal**, como la de los paneles de una colmena.
  2. **Teselado semi-regular**: Si se utilizan 2 tipos de polígonos como teselas y el mismo patrón para todos los vértices. Sólo existen 8 teselaciones semi-regulares
  3. **Teselado demi-regular**: utiliza combinaciones de los tres teselados regulares y los 8 teselados semi-regulares.

![](teselados.jpg "Teselados regulares")
*Teselados regulares*

## De teselados y videojuegos

En en el mundo de los videojuegos, se habla de **_tile-based games_** para referirse a cualquier juego 2D cuyos gráficos resultan de la composición de fragmentos más pequeños, denominados **_tiles._** En español, el término más preciso para traducir tile sería tesela, de ahí que podamos hablar de de **juegos teselados** o con **gráficos teselados**, aunque no son términos habituales. Y si lo que representamos es un mapa, podríamos hablar de **mapa teselado**. En los juegos de estrategia es muy habitual el uso de mapas teselados. Las teselas pueden ser regulares o irregulares. Los teselados irregulares aparecen en los **mapas de regiones. **Ejemplos de este tipo de mapas los encontramos por ejemplo en los juegos de [Paradox](http://www.paradoxplaza.com/), como la serie de [Europa Universalis](http://es.wikipedia.org/wiki/Europa_Universalis), y los juegos de [AGEOD](http://www.ageod.com/), aunque desde un punto de vista técnico, los gráficos usados en la mayoría de mapas de regiones no son teselados, pues internamente utilizan una única imagen para representar todo el mapa, aunque la apariencia es la de un teselado irregular.

![](europa-universalis-ii.jpg "Europa Universalis II (2001, Paradox)")
*Europa Universalis II (2001, Paradox)*

![](american-civil-war.jpg "American Civil War (2007, AGEOD)")
*American Civil War (2007, AGEOD)*

Entre los mapas teselados regulares típicamente encontramos los mapas de **teselado cuadrado** o rectangular y los de **teselado hexagonal**. Los mapas de teselado cuadrado se usan más en los juegos de estrategia abstractos, como ajedrez y damas, así como en juegos de RTS, aunque en estos casos suelen ocultarse las fronteras que separan a unas teselas de otras (internamente el gráfico del mapa se construye como un teselado, pero visualmente se disimula, para dar una sensación de continuidad en el terreno). Algunos ejemplos de juegos con teselado cuadrado (oculto) son los primeros [Warcraft ](https://es.wikipedia.org/wiki/Warcraft)y [Close Combat](http://en.wikipedia.org/wiki/Close_Combat_\(series\)). Por el contrario, los mapas de teselado hexagonal son muy habituales en los juegos de guerra por turnos. Las siguientes imágenes muestran, mediante varios ejemplos, la evolución de los gráficos de juegos de guerra por turnos basados en mapas hexagonales. La rejilla constituida por las fronteras de las teselas puede o no aparecer, y muy a menudo se da la opción de mostrarlas u ocultarlas a gusto del usuario.

![](desert-rats.jpg "Desert Rats (1985, Cases Computer Simulations)")
*Desert Rats (1985, Cases Computer Simulations)*

![](panzer-general.jpg "Panzer General (1994, Strategic Simulations Inc)")
*Panzer General (1994, Strategic Simulations Inc)*

![)](war-in-the-east.jpg "War in the East (2010, 2by3 Games)")
*War in the East (2010, 2by3 Games)*

El sistema más simple de teselado es el cuadrado, todos los cálculos necesarios para representar un mapa e interaccionar con él son extremadamente simples y precisos. En general, con este sistema se permite mover en las 4 direcciones. Una variante que también permite movimiento en 4 direcciones es el teselado romboidal o en diamante, muy utilizado para obtener perspectivas isométricas. Un ejemplo clásico es la serie [Civilization](http://es.wikipedia.org/wiki/Civilization) (imagen siguiente). El número de direcciones se puede duplicar, pasando de 4 a 8, si se permite el movimiento en diagonal, pero ésto introduce dificultades por tener distancias variables entre teselas adyacentes, por lo que no es muy utilizado.

![](civilization-iii.jpg "Civilization III (2001, Firaxis Games)")
*Civilization III (2001, Firaxis Games)*

El teselado hexagonal proporciona 6 direcciones de movimiento con distancia homogénea. Si además se utiliza un sistema gráfico de transiciones entre teselas, entonces el resultado resulta visualmente más atractivo que el teselado cuadrado. Seguramente dedicaré una entrada al tema de las transiciones, de momento en vez de explicarlo lo mostraré visualmente con un par de imágenes.

![](peoples-tactics.jpg "People's Tactics (2004, Victor Reijkersz)")
*People's Tactics (2004, Victor Reijkersz)*

![](toaw-iii.jpg "The Operational Art of War III (2006, Matrix Games)")
*The Operational Art of War III (2006, Matrix Games)*

¿Se aprecia la diferencia? En el primer caso no se usan transiciones y en el segundo caso sí. A pesar de las ventajas del teselado hexagonal, su implementación resulta bastante más compleja de implementar que el teselado cuadrado, como comprobaremos al analizar  sus propiedades matemáticas. Por otro lado, a pesar de las dificultades asociadas al movimiento diagonal, su implementación resulta mucho menos problemática en el ámbito de los videojuegos que en el ámbito de los juegos de tablero. ¿Cómo se explica entonces la abrumadora preponderancia  de los mapas hexagonales en el ámbito de los videojuegos? Posiblemente tenga bastante que ver la gran influencia de los juegos de tablero, donde el movimiento diagonal sí que supone un gran obstáculo.

Y eso es todo por ahora, en la [próxima entrega](../mapas-hexagonales-2) de la serie veremos las propiedades geométricas del hexágono y abordaremos el tratamiento de mapas hexagonales  en un videojuego.

# Enlaces

Para saber más sobre los teselados:

* En la Wolfang MathWorld, en la categoría [Tiling](http://mathworld.wolfram.com/topics/Tiling.html).
* En la Wikipedia, bajo el término [Teselado](http://es.wikipedia.org/wiki/Teselado).
* En la página [Teselaciones](http://www.ceibal.edu.uy/contenidos/areas_conocimiento/mat/teselacionesplano/index.html) del Portal Ceibal.
* En el [Club de la Matemática](http://elclubdelamatematica.blogspot.com.es/2010/02/mosaico-o-teselado.html)