---
layout: post
title:  "Utilización de tipos enumerados en Java"
date:   2013-07-05 18:19:49
categories: programming java
---

# Introducción

En programación, un **tipo enumerado** (también llamado _enumeración_ o _enum_) es un tipo de datos que consiste en un conjunto de valores con nombre llamados elementos, miembros o _enumeradores_ del tipo. Los nombres de los elementos son por lo general identificadores que se comportan como constantes en el lenguaje de programación utilizado. A una variable que ha sido declarada como de un tipo enumerado se le puede asignar como valor cualquiera de los _miembros_ de ese tipo enumerado. 

En general, los elementos de una enumeración deben ser distintos, aunque algunos lenguajes pueden permitir que el mismo elemento aparezca dos veces en la declaración del tipo. En algunos lenguajes, la declaración de un tipo enumerado también define un ordenamiento de sus elementos; en otros, los elementos están desordenados; y en otros más, un orden implícito surge del compilador que representa los elementos como enteros. 

# Tipos enumerados en Java

El lenguaje Java incluye los tipos enumerados desde la vesión J2SE 5. Su sintaxis es similar a la de C, sin embargo Java no trata a las enumeraciones como si fueran constantes de tipo entero, y no permite mezclar valores de enumeración y números enteros. 

Un tipo **[enum](http://docs.oracle.com/javase/tutorial/java/javaOO/enum.html)** en Java es una clase especial generada por el compilador a partir de la clase _Enum_ ( técnicamente, cualquier tipo enum extiende _Enum<E extends Enum<E>>_), y los elementos de la enumeración se comportan como instancias globales estáticas de esa clase. Un tipo enum puede tener atributos, métodos de instancia y un constructor. Los argumentos para el constructor se pueden especificar por separado para cada elemento de la  enumeración. 

El siguiente fragmento de código muestra la definición de una enumeración en Java. El tipo _TerrainType_ define diferentes tipos de terreno, caracterizados por un único atributo, _movementCost._

{% highlight java %}
public enum TerrainType implements MovementEffects{
    OPEN(0),
    SAND(1),
    HILLS(2),
    MOUNTAINS(3),
    ALPINE(IMPASSABLE),
    MARSH(3),
    WATER(IMPASSABLE),
    CROPLANDS(1),
    URBAN(0),
    ROCKY(2),
    ESCARPMENT(3),
    RIVER(2),
    SUPER_RIVER(IMPASSABLE),
    FOREST(2),
    LIGHT_WOODS(1),
    ROAD(0);
 
    private final int movementCost;
 
 
    private TerrainType(final int movementCost) {
        this.movementCost = movementCost;
    }
 
    public int getMovementCost() {
        return movementCost;
    }
}
{% endhighlight %}

A cada elemento o valor de enumeración se le asigna un valor entero, que se puede consultar mediante el método _ordinal()_ y corresponde al orden en que se declaran en el código fuente , a partir de 0. Además, a cada elemento le corresponde un nombre único, de tipo String,  que se puede obtener mediante el método name(). Este método se hereda de Enum, y devuelve una cadena con el mismo texto usado para declarar el elemento del enum. Por ejemplo, TerrainType.FOREST.name() devuelve "FOREST". 

## Utilización básica de los enum

El código siguiente muestra algunos ejemplos básicos de utilización de los enum en Java, usando como referencia la clase TerrainType previamente definida.

{% highlight java %}
@Test
    public void testEnumManipulation() throws Exception {
        System.out.println("*** Testing enum manipulation ***\n");
        System.out.println("Listing all enum values...");
        TerrainType[] allTerrainTypes = TerrainType.values();
        for (TerrainType tt : allTerrainTypes) {
            System.out.println(tt.ordinal() + ": " + tt.name());
        }
        System.out.println("Basic usage of enums...");
        TerrainType t1 = TerrainType.FOREST;
        TerrainType t2 = TerrainType.valueOf("FOREST");
        TerrainType t3 = allTerrainTypes[13];
        TerrainType t4 = Enum.valueOf(TerrainType.class, "RIVER");
        assert t1 == t2;
        assert t1.equals(t2);
        assert t1 == t3;
        assert t1 != t4;
 
        switch (t1) {
            case FOREST:
                assert t1 == TerrainType.FOREST;
                break;
        }
        assert t1.compareTo(t4) == 2;
        try {
            assert TerrainType.valueOf("Forest") == t1;
        } catch (IllegalArgumentException e) {
            System.out.println("Catched exception: " + e.getMessage());
        }
    }
{% endhighlight %}

Algunos métodos muy utilizados de la clase Enum son: 

  * `TerrainType.values()`: devuelve un array con todos los elementos del enum TerrainType en el orden correspondiente a su ordinal (es decir, el orden en que son declarados).
  * `TerrainType.FOREST.`[`name()`](http://docs.oracle.com/javase/7/docs/api/java/lang/Enum.html#name): devuelve el nombre asociado al elemento. Si no se sobreescribe, este método devuelve el identificador del elemento como String, pero se puede sobreescribir para ofrecer nombres personalizados.
  * `TerrainType.FOREST.`[`ordinal()`](http://docs.oracle.com/javase/7/docs/api/java/lang/Enum.html#ordinal): devuelve el orden de un elemento del enum en particular
  * `TerrainType`.[`valueOf()`](http://docs.oracle.com/javase/7/docs/api/java/lang/Enum.html#valueOf(java.lang.Class, java.lang.String)`("FOREST")`:  busca y devuelve  el elemento cuyo nombre coincide con el argumento. Lanza una excepción de tipo  `IllegalArgumentException` si no encuentra ningún elemento con ese nombre.
  * 

Una característica que hace a los enum ventajosos comparado con constantes de tipo int o String es que los enum son estrictamente tipados. Si decimos que un método de una clase recibe un enum el compilador comprobará en tiempo de compilación que cuando lo usamos le pasemos realmente un valor de ese enum, cosa que no sucede con constantes definidas como int o String.

Otra de las ventajas de los enum es que son **polimórficos**, pueden tener comportamiento exclusivo. Se pueden definir métodos comunes para un tipo enum, o bien métodos propios para cada elemento enumerado. Esta característica dota a los enum de una gran versatilidad, se puede por ejemplo crear una máquina de estados con un sólo tipo enum, definiendo cada estado como un elemento de esa enum con su propio comportamiento. Un método "siguiente" que devolviera un enum del mismo tipo, implementado individualmente para cada elemento del enum implementaría la transición entre estados.

A diferencia de las constantes de tipo String, que para comparar si son iguales tenemos que usar el método _equals_,  con valores de tipo enum se puede usar tanto el método _equal_ como el operador == , con la ventaja de que el operador == nos evita el problema de un posible _NullPointerException_, y además se comprueba que se estén comparando dos valores del mismo enum (comprobación de tipos).  

Por otro lado, conviene saber que los tipos enum implementan la interfaz [Comparable](http://docs.oracle.com/javase/7/docs/api/java/lang/Comparable.html), para lo cual se utiliza el valor ordinal. Más concretamente. El resultado de una comparación con el método [`compareTo`](http://docs.oracle.com/javase/1.5.0/docs/api/java/lang/Enum.html#compareTo) devuelve la diferencia entre los valores ordinales de los elementos comparados. Esta comparación permite ordenar los elementos de una enumeración directamente, sin necesidad de crear un _Comparable_. Además, los enums pueden ser usados en expresiones _switch_ , cosa que con constantes de tipo String solo podemos hacer a partir de la versión 7 de Java.. 

## Colecciones optimizadas con enum

La librería estándar de Java proporciona clases de utilidad para su uso con las enumeraciones. En particular, proporciona 2 tipos de colección exclusivas para elementos de tipo enum. 

  * La clase [**EnumSet**](http://docs.oracle.com/javase/7/docs/api/java/util/EnumSet.html) implementa un conjunto de valores de enumeración como un vector de bits , lo que hace que sea tan compacto y eficiente como la manipulación de bits explícita , pero más seguro .
  * La clase [**EnumMap**](http://docs.oracle.com/javase/7/docs/api/java/util/EnumMap.html) implementa un mapa de valores de enumeración de objetos. Se implementa como un vector, con los ordinales de los valores enumerados como índice.

Veamos su utilidad con más detalle. 

### EnumSet

{% highlight java %}
public abstract class EnumSet<E extends Enum<E>>
    extends AbstractSet<E>
    implements Cloneable, Serializable
{% endhighlight %}

La clase EnumSet es una implementación especializada de la interfaz Set para su uso con tipos enum. Todos los elementos de un EnumSet deben pertenecer al mismo tipo enum, el cual es especificado de forma implícita o explícita cuando el EnumSet es creado.

No se permiten elementos nulos, cualquier intento de insertar un null en un EnumSet lanzará una excepción de tipo NullPointerException.

El iterador devuelto por el método iterator recorre los elementos de la colección en su orden natural (orden en que fueron declarados).

Internamente, un EnumSet<E> se representa como un vector de bits donde cada bit se corresponde con uno de los elementos del tipo enum E, siendo el bit menos significativo el que representa al elemento con ordinal cero, y el resto de bits se corresponden con los demás elementos en orden creciente. Así pues, el número de bits necesario para representar un objeto de tipo  EnumSet<E> es precisamente el número de elementos de E, su talla. Sin embargo, a nivel de implementación Java utiliza bloques de tipo long, es decir de 64 bits. Más concretamente, EnumSet es una clase abstracta con 2 clases derivadas: RegularEnumSet y JumboEnumSet. La primera utiliza un long y la segunda un array de long. La clase EnumSet proporciona una serie de factorías (métodos que utilizan el patrón de diseño Factoría) que devuelven instancias del tipo apropiado según la talla de E. Si nuestro enum tiene no más de 64 elementos se utiliza un RegularEnumSet, y un JumboEnumSet en caso contrario.

Esta representación es extremadamente compacta y eficiente. La complejidad espacial y temporal de los métodos de estas clases debería ser suficientemente buena para usarlos como alternativa a la representación de bits explícita (las llamadas banderas o flags) y además proporciona seguridad de tipo (type-safety).  Incluso las operaciones cuyos argumentos son también colecciones, como containsAll y retainAll, deberían ejecutarse muy rápido si su argumento es también un EnumSet.

El siguiente fragmento muestra los procedimientos para crear objetos de tipo EnumSet y realizar operaciones básicas, que incluyen:

* Diferentes métodos factoría para la creación de objetos de tipo EnumSet: EnumSet.of, EnumSet.range, EnumSet.allOf, EnumSet.noneOf, EnumSet.complementOf, EnumSet.copyOf
* Búsqueda:  método contains, es un método definido en el interfaz Set, el cual nos dice si un elemento pertenece al Set o no.
* Modificacion: métodos add, remove, addAll, removeAll, retainAll

{% highlight java %}
@Test
    public void testEnumSet() throws Exception {
        System.out.println("\n*** Testing enum sets manipulation ***\n");
        System.out.println("Creating enum sets...");
        Set<TerrainType> anyTerrain = EnumSet.allOf(TerrainType.class);
        Set<TerrainType> emptyTerrain = EnumSet.noneOf(TerrainType.class);
        Set<TerrainType> stream = EnumSet.of(TerrainType.RIVER, TerrainType.SUPER_RIVER);
        Set<TerrainType> woods = EnumSet.of(TerrainType.FOREST, TerrainType.LIGHT_WOODS);
        Set<TerrainType> water = EnumSet.of(TerrainType.WATER);
        Set<TerrainType> land = EnumSet.complementOf((EnumSet<TerrainType>) water);
        Set<TerrainType> anyMountain = EnumSet.range(TerrainType.HILLS, TerrainType.ALPINE);
        System.out.println("stream = " + stream);
        System.out.println("woods = " + woods);
        System.out.println("water = " + water);
        System.out.println("land = " + land);
        System.out.println("anyMountain = " + anyMountain);
        System.out.println();
        System.out.println("Basic Set operations...");
        assert anyTerrain.contains(TerrainType.MARSH);
        assert !woods.contains(TerrainType.MARSH);
        assert land.containsAll(woods);
        assert !woods.containsAll(stream);
        System.out.println("Union of enum set...");
        Set<TerrainType> terrainSet = EnumSet.copyOf(land);
        terrainSet.addAll(water);
        assert terrainSet.equals(anyTerrain);
        assert !terrainSet.equals(land);
        System.out.println("Intersection of enum set...");
        terrainSet.remove(TerrainType.LIGHT_WOODS);
        terrainSet.retainAll(woods);
        assert terrainSet.contains(TerrainType.FOREST);
        assert !terrainSet.contains(TerrainType.LIGHT_WOODS);
    }
{% endhighlight %}

### EnumMap

{% highlight java %}
public class EnumMap<K extends Enum<K>,V>
    extends AbstractMap<K,V>
    implements Serializable, Cloneable
{% endhighlight %}

La clase EnumMap es una implementación especializada de la interfaz Map para su uso con claves (K) de tipo enum. Todas las claves de un EnumMap deben pertenecer al mismo tipo enum, el cual es especificado de forma implícita o explícita cuando el EnumMap es creado.

Todo lo que se cumple para los elementos de un EnumSet se cumple para las claves de un EnumMap, como que su iterador recorre las claves en su orden natural.

A diferencia de los EnumSet, que se crean mediante métodos factoría, los EnumMap se crean mediante constructores, como por ejemplo:

{% highlight java %}
public EnumMap(Class<K> keyType)
{% endhighlight %}

El siguiente fragmento de código muestra la creación y uso de EnumMap.

{% highlight java %}
    @Test
    public void testEnumMap() throws Exception {
        System.out.println("\n*** Testing enum maps manipulation ***\n");
        System.out.println("Creating enum maps...");
        Map<TerrainType, String> enumMap = new EnumMap<TerrainType, String>(TerrainType.class);
        Set<TerrainType> woods = EnumSet.of(TerrainType.FOREST, TerrainType.LIGHT_WOODS);
        for (TerrainType terrainType : woods) {
            enumMap.put(terrainType, terrainType.name().toLowerCase() + ".png");
        }
        System.out.println(enumMap);
        for (Map.Entry<TerrainType, String> entry : enumMap.entrySet()) {
            assert entry.getValue().equals(entry.getKey().name().toLowerCase() + ".png");
        }
    }
{% endhighlight %}

Los EnumMap son una alternativa muy interesante a los HashMap cuando las claves son constantes, pues resultan más compactos y eficientes.