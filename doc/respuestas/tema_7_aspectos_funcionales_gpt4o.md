<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

### Respuesta

Un **puntero a función** es una variable que almacena la dirección de memoria de una función, permitiendo invocarla de forma indirecta. En C, las funciones no son objetos, pero sí tienen una dirección que puede asignarse a un puntero con la firma adecuada. Este mecanismo permite pasar funciones como parámetros, almacenarlas en estructuras de datos o decidir dinámicamente qué función ejecutar, ideas que más adelante enlazan con conceptos funcionales de lenguajes modernos.

Para definir un puntero a función es necesario especificar el tipo de valor que devuelve la función y el tipo de sus parámetros. En el siguiente ejemplo se define una función que recibe una cadena de caracteres y la convierte a mayúsculas, devolviendo la misma cadena ya modificada. Posteriormente, se declara un puntero a dicha función y se inicializa con su dirección.

```c
#include <stdio.h>
#include <ctype.h>

char* convertirAMayusculas(char* cadena) {
    for (int i = 0; cadena[i] != '\0'; i++) {
        cadena[i] = toupper(cadena[i]);
    }
    return cadena;
}

int main() {
    char texto[] = "Hola Mundo";

    char* (*aMayusculas)(char*);
    aMayusculas = convertirAMayusculas;

    char* resultado = aMayusculas(texto);
    printf("%s\n", resultado);

    return 0;
}
```

En este código, el puntero `aMayusculas` tiene exactamente la misma firma que la función a la que apunta, lo que permite invocarla como si fuese una función normal. El uso de punteros a funciones introduce una forma básica de **abstracción del comportamiento**, separando qué se hace de cuándo o cómo se decide hacerlo, lo cual es un antecedente directo de los enfoques funcionales que aparecen más adelante en otros lenguajes y paradigmas.



## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta

Una **función lambda** es una forma de definir una función **anónima**, es decir, sin nombre, que puede asignarse a una variable, pasarse como argumento o devolverse como resultado. Su objetivo principal es tratar el comportamiento como un valor, de manera similar a como en C se hacía con punteros a funciones, pero con una sintaxis más compacta y expresiva. Las funciones lambda son un elemento clave de los enfoques funcionales, ya que permiten escribir código más declarativo y centrado en *qué se hace* en lugar de *cómo se hace*.

En **JavaScript**, las funciones son ciudadanos de primera clase, por lo que pueden asignarse directamente a variables sin ningún tipo adicional. Una función lambda se define usando una sintaxis concisa, normalmente con la flecha `=>`. En el siguiente ejemplo, la variable local `aMayusculas` referencia una función lambda que recibe una cadena y devuelve una nueva cadena en mayúsculas.

```javascript
let aMayusculas = (cadena) => cadena.toUpperCase();

let resultado = aMayusculas("Hola Mundo");
console.log(resultado);
```

En **Java**, a partir de Java 8, las funciones lambda se introducen junto con las **interfaces funcionales**, que definen un único método abstracto. Para simplificar, se puede usar la interfaz genérica `Function<T, R>`, que representa una función que recibe un valor de tipo `T` y devuelve otro de tipo `R`. La variable `aMayusculas` actúa aquí como una referencia a una función.

```java
import java.util.function.Function;

public class Ejemplo {
    public static void main(String[] args) {
        Function<String, String> aMayusculas = s -> s.toUpperCase();

        String resultado = aMayusculas.apply("Hola Mundo");
        System.out.println(resultado);
    }
}
```

La diferencia fundamental con el enfoque clásico basado en métodos es que las funciones lambda permiten **pasar comportamientos como datos**, sin necesidad de crear clases adicionales ni usar herencia. Esto conecta directamente con la idea vista en C de punteros a funciones, pero con mayor seguridad de tipos, claridad y facilidad de uso, especialmente cuando se combina con genéricos y bibliotecas modernas.



## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta

El **paradigma funcional** es un estilo de programación que se centra en la **evaluación de funciones** y en la composición de las mismas para resolver problemas, evitando en la medida de lo posible el uso de estados mutables y efectos secundarios. En este paradigma, una función se concibe como una transformación de entradas en salidas, de forma similar a las funciones matemáticas. La atención se pone en *qué resultado se quiere obtener* más que en la secuencia de pasos necesarios para lograrlo.

Lenguajes tradicionalmente orientados a objetos, como **Java a partir de la versión 8**, se califican como **multi‑paradigma** porque permiten combinar varios enfoques de programación dentro del mismo lenguaje. Java mantiene su base orientada a objetos (clases, herencia, polimorfismo), pero incorpora elementos funcionales como funciones lambda, interfaces funcionales y operaciones sobre colecciones con un estilo declarativo. Esto permite elegir el paradigma más adecuado para cada problema sin abandonar el lenguaje.

Decir que las funciones son **“ciudadanos de primera clase”** significa que las funciones se tratan de la misma forma que otros valores del lenguaje. Es decir, pueden asignarse a variables, pasarse como argumentos a otras funciones, devolverse como resultado y almacenarse en estructuras de datos. Este concepto enlaza directamente con los punteros a funciones en C, pero en los lenguajes modernos se ofrece con una sintaxis más clara y con un mayor control de tipos.

La introducción de estas ideas funcionales no reemplaza la programación orientada a objetos en Java, sino que la complementa. El resultado es un lenguaje más expresivo y flexible, capaz de abordar problemas desde distintas perspectivas según convenga, lo que justifica su consideración como lenguaje multi‑paradigma.



## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta

La **sintaxis básica de una función lambda en Java** está diseñada para expresar, de forma concisa, la implementación de un método que pertenece a una **interfaz funcional** (una interfaz con un único método abstracto). Una función lambda no define un método con nombre, sino una expresión que describe directamente el comportamiento que se quiere implementar. Su objetivo es reducir el código repetitivo frente a clases anónimas y facilitar el paso de comportamientos como parámetros.

La forma general de una lambda en Java es: **parámetros → cuerpo**. Los parámetros aparecen a la izquierda del operador `->` y el cuerpo a la derecha. Si hay un solo parámetro, pueden omitirse los paréntesis, y si el cuerpo es una única expresión, también pueden omitirse las llaves y la instrucción `return`. El compilador infiere automáticamente los tipos de los parámetros a partir del contexto, por lo que normalmente no es necesario indicarlos explícitamente.

```java
Function<String, String> aMayusculas = s -> s.toUpperCase();
```

Cuando el cuerpo contiene más de una instrucción, deben usarse llaves y una sentencia `return` explícita si la función devuelve un valor. En este caso, la lambda se asemeja más a un bloque de código tradicional, aunque sigue sin tener nombre propio.

```java
Function<String, String> aMayusculas = s -> {
    String resultado = s.toUpperCase();
    return resultado;
};
```

Esta sintaxis permite expresar lógica de forma clara y directa, destacando **qué hace la función** sin introducir estructuras adicionales. El resultado es un código más legible y cercano a un enfoque funcional, manteniendo al mismo tiempo la seguridad de tipos propia de Java.



## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta

Recibir una función como parámetro de un método significa separar **el dato** del **comportamiento** que se aplica sobre él. En lugar de codificar dentro del método qué transformación se realiza, el método se limita a invocar la función recibida. Este enfoque es característico del paradigma funcional y permite escribir código más reutilizable, flexible y fácil de extender, ya que el método `transformar` puede trabajar con cualquier función que cumpla la firma esperada.

En **JavaScript**, las funciones son ciudadanos de primera clase, por lo que pueden pasarse directamente como argumentos sin necesidad de tipos adicionales. El método `transformar` recibe una cadena y una función transformadora, y simplemente invoca dicha función con la cadena como argumento. La función concreta, como `aMayusculas`, se define externamente y se pasa al método.

```javascript
let aMayusculas = cadena => cadena.toUpperCase();

function transformar(texto, transformador) {
    return transformador(texto);
}

let resultado = transformar("Hola Mundo", aMayusculas);
console.log(resultado);
```

En **Java**, este mismo patrón se implementa mediante interfaces funcionales. El método `transformar` recibe un `String` y una referencia a una función representada por `Function<String, String>`. Desde dentro del método, la función se ejecuta mediante el método `apply`. El compilador garantiza que la función recibida tiene la firma correcta, reforzando el chequeo de tipos en tiempo de compilación.

```java
import java.util.function.Function;

public class Ejemplo {

    public static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {
        Function<String, String> aMayusculas = s -> s.toUpperCase();

        String resultado = transformar("Hola Mundo", aMayusculas);
        System.out.println(resultado);
    }
}
```

En ambos lenguajes, este estilo muestra claramente cómo **el comportamiento se pasa como un valor**, lo que permite cambiar la lógica aplicada sin modificar el método que la utiliza. Esta idea conecta directamente con los punteros a funciones de C, pero se expresa de forma más segura, clara y flexible en lenguajes modernos con soporte funcional.



## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta

Invocar una función mediante una **lambda definida directamente en la llamada** refuerza la idea de que el comportamiento puede declararse *en el propio lugar donde se usa*. En lugar de definir una función previamente y asignarla a una variable, se pasa la función lambda como argumento de forma inmediata. Este estilo es habitual en programación funcional y resulta especialmente útil cuando la lógica es simple y solo se utiliza una vez.

En **JavaScript**, esta forma de trabajar es muy natural, ya que las funciones anónimas forman parte del lenguaje desde sus inicios. En el siguiente ejemplo, `transformar` se invoca pasando directamente una función lambda que invierte la cadena recibida, sin necesidad de definirla previamente en otra variable.

```javascript
function transformar(texto, transformador) {
    return transformador(texto);
}

let resultado = transformar("Hola Mundo", cadena =>
    cadena.split("").reverse().join("")
);

console.log(resultado);
```

En **Java**, el mismo enfoque se aplica utilizando una función lambda compatible con la interfaz funcional `Function<String, String>`. La lambda se define justo en el momento de la llamada al método `transformar`, lo que permite expresar claramente la transformación sin añadir métodos auxiliares ni variables innecesarias.

```java
import java.util.function.Function;

public class Ejemplo {

    public static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {
        String resultado = transformar("Hola Mundo",
            s -> new StringBuilder(s).reverse().toString()
        );

        System.out.println(resultado);
    }
}
```

Este estilo hace que el código sea más expresivo y cercano al problema que se quiere resolver, ya que la transformación aparece exactamente donde se aplica. Además, reduce la proliferación de métodos pequeños y refuerza la separación entre la estructura del algoritmo y el comportamiento que se desea ejecutar en cada caso.



## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta

Un **cierre** (*closure*) es una función que, además de su propio código, **captura y mantiene acceso a variables del contexto donde fue definida**, incluso cuando esa función se ejecuta fuera de dicho contexto. En el caso de las funciones lambda, un closure permite que la función utilice variables locales externas como si fuesen parte de su propio estado. Esta idea es fundamental en la programación funcional, ya que permite crear funciones parametrizadas por su entorno sin necesidad de usar atributos de clase.

En **Java**, las funciones lambda pueden acceder a **variables locales del método que las contiene**, siempre que dichas variables sean *finales o efectivamente finales* (es decir, que no se modifiquen después de su inicialización). El compilador garantiza esta restricción para evitar problemas de consistencia, ya que las variables locales viven en la pila y la lambda puede ejecutarse más tarde. A pesar del *type erasure*, el valor de la variable se captura correctamente.

Partiendo del ejemplo anterior del método `transformar`, se puede crear una función lambda que concatene a la cadena de entrada otra cadena definida fuera de la lambda. Esa cadena externa forma parte del **closure**, ya que la lambda depende de ella aunque no sea un parámetro explícito.

```java
import java.util.function.Function;

public class Ejemplo {

    public static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {
        String sufijo = " !!!";  // Variable local externa

        String resultado = transformar("Hola Mundo",
            s -> s + sufijo
        );

        System.out.println(resultado);
    }
}
```

En este caso, la función lambda accede a la variable local `sufijo` definida fuera de ella, formando un cierre. Aunque `sufijo` pertenece al contexto del método `main`, la lambda puede usarla con normalidad. Este mecanismo permite escribir código más expresivo y flexible, combinando datos y comportamiento sin recurrir a clases adicionales ni a estado mutable compartido.



## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta

Aunque tanto las **funciones lambda** como los **punteros a funciones en C** permiten tratar el comportamiento como un valor, existen diferencias fundamentales entre ambos mecanismos. Un puntero a función en C es únicamente una **dirección de memoria** que apunta a una función concreta con una firma determinada. No tiene información adicional asociada y no captura ningún contexto: solo permite invocar una función existente de forma indirecta, delegando toda la responsabilidad de la corrección al programador.

Una función lambda, en cambio, es una **construcción de más alto nivel**. No solo representa código ejecutable, sino que también puede capturar variables de su entorno, formando un **closure**. Esto permite que la función tenga un estado implícito sin necesidad de usar variables globales ni estructuras adicionales. Este concepto no existe en C con punteros a funciones tradicionales, donde cualquier estado debe gestionarse explícitamente mediante parámetros o estructuras auxiliares.

Otra diferencia clave está en la **seguridad de tipos**. En C, el uso de punteros a funciones no impide realizar conversiones incorrectas, lo que puede provocar errores graves en tiempo de ejecución. En Java, las funciones lambda están ligadas a interfaces funcionales y genéricos, por lo que el compilador verifica que la firma encaja exactamente con el contexto en el que se usan. Esto refuerza el chequeo de tipos y reduce la posibilidad de errores.

Por último, las funciones lambda están pensadas para integrarse de forma natural en un estilo de programación declarativo y funcional, especialmente cuando se combinan con colecciones y operaciones de alto nivel. Los punteros a funciones en C son una herramienta más básica y mecánica, poderosa pero menos expresiva, que no incorpora de forma directa conceptos como closures, inferencia de tipos o composición funcional.



## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta

Devolver funciones implica que una función no solo ejecuta un cálculo inmediato, sino que **construye y devuelve otra función** como resultado. En un enfoque funcional, esto permite generar comportamientos personalizados a partir de ciertos datos de entrada. En este caso, la función `crearDescuento` recibe un porcentaje y devuelve una función que aplica ese descuento a una cantidad. El tipo `Function<Double, Double>` representa precisamente una función que recibe un número real y devuelve otro.

La clave de este diseño es que la función devuelta **recuerda el porcentaje de descuento con el que fue creada**, aunque ese porcentaje ya no se pase como argumento al invocar la función de descuento. Esto es posible gracias a un **closure**: la función lambda captura la variable `porcentaje` del contexto donde fue definida. Aunque el método `crearDescuento` termine su ejecución, la lambda conserva el valor capturado y puede seguir utilizándolo.

```java
import java.util.function.Function;

public class Ejemplo {

    public static Function<Double, Double> crearDescuento(double porcentaje) {
        return cantidad -> cantidad * (1 - porcentaje / 100);
    }

    public static void main(String[] args) {
        Function<Double, Double> descuento10 = crearDescuento(10);
        Function<Double, Double> descuento25 = crearDescuento(25);

        double precio = 200.0;

        System.out.println(descuento10.apply(precio)); // 180.0
        System.out.println(descuento25.apply(precio)); // 150.0
    }
}
```

En este ejemplo se crean dos funciones distintas, una con un 10 % de descuento y otra con un 25 %, ambas a partir del mismo método generador. Cada una mantiene su propio valor de `porcentaje`, lo que demuestra que la variable capturada forma parte del **estado interno del closure**. Este estilo permite construir funciones especializadas sin recurrir a atributos de clase ni a objetos adicionales, combinando datos y comportamiento de forma clara y segura en tiempo de compilación.



## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta

En Java, una **interfaz funcional** es una interfaz que define **exactamente un único método abstracto**, y que sirve como **tipo de una función lambda**. Dado que Java es un lenguaje con comprobación estática de tipos, toda función lambda debe tener un tipo bien definido en tiempo de compilación, y ese tipo viene dado precisamente por una interfaz funcional. La lambda no tiene tipo por sí misma, sino que se interpreta como una implementación concreta del método abstracto de dicha interfaz.

El requisito fundamental de una interfaz funcional es que tenga **un solo método abstracto**. Puede incluir otros métodos siempre que sean *default* o *static*, ya que estos ya tienen implementación y no cuentan para este cómputo. Tampoco se tienen en cuenta los métodos heredados de `Object`, como `toString` o `equals`. Para documentar y reforzar esta intención, Java proporciona la anotación `@FunctionalInterface`, que no es obligatoria, pero permite al compilador comprobar que la interfaz cumple realmente las condiciones exigidas.

Ejemplos típicos de interfaces funcionales son `Runnable`, `Comparator<T>` o las interfaces genéricas del paquete `java.util.function`, como `Function<T, R>`, `Predicate<T>` o `Consumer<T>`. Cuando se escribe una función lambda, el compilador analiza el contexto y asocia la lambda a la interfaz funcional correspondiente, utilizando la firma de su único método abstracto para comprobar los tipos de parámetros y del valor de retorno.

Este mecanismo permite integrar las funciones lambda de forma segura en Java, manteniendo el sistema de tipos del lenguaje. Gracias a las interfaces funcionales, se puede trabajar con funciones como valores sin perder comprobación estática, combinando así el enfoque funcional con la robustez propia de un lenguaje fuertemente tipado.



## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta

Una **interfaz funcional definida a mano** permite expresar explícitamente el tipo de una función que se desea utilizar mediante lambdas. En este caso, se quiere representar una transformación que recibe un `String` y devuelve otro `String`. Definir esta interfaz hace más claro el propósito del código y evita depender directamente de interfaces genéricas como `Function<T, R>`, mejorando la expresividad y la legibilidad.

Para que una interfaz sea funcional, debe cumplir el requisito de tener **un único método abstracto**. En este ejemplo, la interfaz `Transformador` define un único método que transforma una cadena de texto en otra. Es recomendable añadir la anotación `@FunctionalInterface`, ya que permite al compilador verificar que la interfaz cumple correctamente las condiciones y documenta su intención funcional.

```java
@FunctionalInterface
public interface Transformador {
    String transformar(String texto);
}
```

Una vez definida, esta interfaz puede utilizarse como tipo de una función lambda de forma directa. El compilador comprobará que la lambda proporcionada coincide exactamente con la firma del método `transformar`. De este modo, se integra el enfoque funcional dentro de Java manteniendo una comprobación estática de tipos clara y explícita, sin necesidad de crear clases adicionales.



## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta

Una interfaz funcional puede hacerse **genérica** utilizando parámetros de tipo, de forma que no quede limitada a trabajar solo con `String`. Al introducir genéricos, la interfaz expresa de manera clara que representa una transformación **de un tipo de entrada a un tipo de salida**, manteniendo la comprobación estática de tipos. Este enfoque es análogo al uso de `Function<T, R>`, pero permite definir una interfaz con un nombre más significativo dentro del dominio del problema.

Para que la interfaz siga siendo funcional, debe seguir cumpliendo el requisito de tener **un único método abstracto**. En este caso, se definen dos parámetros de tipo: uno para el tipo de entrada y otro para el tipo de salida. El método abstracto utiliza esos parámetros como tipo de su argumento y de su valor de retorno, reforzando la seguridad de tipos en tiempo de compilación.

```java
@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T valor);
}
```

A partir de esta interfaz genérica, se puede definir fácilmente un transformador concreto mediante una función lambda. Por ejemplo, un transformador que recibe un `Double` y devuelve un `Integer` redondeando el valor puede declararse de forma clara y segura, sin conversiones explícitas en el uso posterior.

```java
Transformador<Double, Integer> redondear =
        d -> (int) Math.round(d);

Integer resultado = redondear.transformar(3.6);
```

En este ejemplo, el compilador garantiza que el transformador solo acepta valores de tipo `Double` y que siempre devuelve un `Integer`. El uso de genéricos permite así expresar con precisión la intención de la función, mejorar la legibilidad del código y reducir errores, integrando de forma natural el estilo funcional dentro del sistema de tipos de Java.



## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta

Java proporciona un conjunto amplio de **interfaces funcionales predefinidas** en el paquete `java.util.function`, cuyo objetivo es cubrir los casos más habituales del paradigma funcional sin necesidad de definir nuevas interfaces. Estas interfaces representan patrones comunes como funciones que transforman datos, predicados que devuelven valores lógicos o consumidores que realizan acciones sin devolver resultado. De este modo, se fomenta la reutilización y la homogeneidad del código.

Las más importantes son **`Function<T, R>`**, que representa una función que transforma un valor de tipo `T` en otro de tipo `R`; **`Predicate<T>`**, que recibe un valor y devuelve un `boolean`; y **`Consumer<T>`**, que recibe un valor pero no devuelve resultado, utilizándose normalmente para realizar efectos como imprimir o modificar un estado externo. También es habitual **`Supplier<T>`**, que no recibe parámetros y devuelve un valor, actuando como generador de datos.

Además de estas interfaces genéricas, Java incluye **versiones especializadas para tipos primitivos** (`int`, `double`, `long`) con el fin de evitar costes de *boxing* y *unboxing*. Ejemplos de estas son `IntFunction<R>`, `DoublePredicate`, `IntConsumer`, `DoubleSupplier` o `ToIntFunction<T>`. Estas variantes permiten escribir código funcional eficiente sin perder seguridad de tipos.

En conjunto, estas interfaces funcionales predefinidas hacen que, en la mayoría de los casos, no sea necesario crear interfaces propias como `Transformador<T, R>`, ya que `Function<T, R>` cubre exactamente ese propósito. Su uso estandarizado facilita la lectura del código, la interoperabilidad con APIs modernas de Java y la adopción de un estilo funcional dentro de un lenguaje con tipado estático y orientación a objetos.



## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta

El método **`forEach`** de la interfaz `List` permite recorrer los elementos de una colección aplicando una función a cada uno de ellos. Desde un punto de vista funcional, `forEach` sustituye al bucle `for` tradicional cuando el objetivo del recorrido es **aplicar una acción a cada elemento**, no controlar explícitamente índices ni modificar la estructura. El recorrido se expresa declarando *qué se quiere hacer con cada elemento*, y no *cómo recorrer la colección*.

Este método recibe como parámetro una **función lambda**, que en Java se expresa mediante una interfaz funcional del tipo `Consumer<T>`. Cada elemento de la lista se pasa automáticamente como argumento a dicha función. En el ejemplo siguiente, se recorre una lista de enteros y se muestra un mensaje únicamente cuando el valor es positivo, sin necesidad de variables auxiliares ni estructuras de control explícitas.

```java
import java.util.List;
import java.util.Arrays;

public class Ejemplo {
    public static void main(String[] args) {
        List<Integer> numeros = Arrays.asList(-3, 5, 0, 8, -1);

        numeros.forEach(n -> {
            if (n > 0) {
                System.out.println(n + " es positivo");
            }
        });
    }
}
```

Este estilo de código resulta más declarativo y expresivo que un bucle `for` clásico, ya que el foco está en la operación aplicada a cada elemento, no en el mecanismo de iteración. Además, refuerza la idea funcional de tratar el **comportamiento como un valor**, integrándose de forma natural con el resto de constructos funcionales introducidos en Java.


## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta
### Respuesta

La firma de `forEach` en `List` es `forEach(Consumer<? super T> action)` y no `Consumer<T>` por una razón relacionada con la **variancia** y la seguridad de tipos. El método `forEach` **consume** elementos de la lista, es decir, entrega cada elemento a la función recibida. Al usar `? super T`, se permite pasar un `Consumer` que acepte no solo `T`, sino también cualquier **supertipo de T**, como `Object`. Esto hace el método más flexible sin perder seguridad, ya que es siempre válido pasar un objeto de tipo `T` a algo que sabe manejar `T` o cualquiera de sus supertipos.

Este criterio se resume en el principio **PECS**, que significa *Producer Extends, Consumer Super*. Según PECS, cuando un parámetro genérico **produce valores** (se leen datos), se debe usar `? extends T`, y cuando **consume valores** (se escriben o se pasan datos), se debe usar `? super T`. En `forEach`, la lista produce elementos de tipo `T`, pero el `Consumer` los consume, por lo que usar `? super T` es la elección correcta. Esto explica por qué es legal usar un `Consumer<Object>` para recorrer una `List<String>`.

Aplicando este razonamiento al método `transformar` visto anteriormente, si el método recibe un dato y lo pasa a una función transformadora, esa función **consume** el argumento de entrada y **produce** un resultado. Una firma más flexible, siguiendo PECS, sería permitir que la función acepte un supertipo del dato de entrada y produzca un subtipo del resultado esperado. Conceptualmente, esto se expresa usando wildcards en la definición del parámetro funcional.

Por ejemplo, si `transformar` recibe un `String`, la función transformadora debería ser capaz de aceptar un `String` o algo más general (`? super String`), y devolver un `String` o algo más específico (`? extends String`). Esta aplicación de PECS mejora la reutilización del método sin comprometer el chequeo estático de tipos, y muestra cómo los genéricos y los wildcards permiten expresar con precisión los roles de productor y consumidor en un diseño funcional.

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.
### Respuesta

Una **referencia a método** consiste en almacenar en una variable una referencia a un método existente, para poder invocarlo posteriormente sin llamarlo directamente en ese momento. Este mecanismo está estrechamente relacionado con las funciones lambda, ya que ambas permiten tratar métodos y funciones como valores. La diferencia principal es que la referencia a método reutiliza un método ya definido, en lugar de crear una función nueva.

En **JavaScript**, los métodos de un objeto pueden asignarse a variables, pero es necesario tener en cuenta el valor de `this`. Para que el método siga asociado correctamente al objeto original, se suele usar `bind`. En el siguiente ejemplo, se crea una clase `Persona` con un método `saludar`, se instancia un objeto y se obtiene una referencia a su método.

```javascript
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }

    saludar() {
        console.log("Hola, soy " + this.nombre);
    }
}

const p = new Persona("Ana");
const saludo = p.saludar.bind(p);

saludo();
```

En **Java**, las referencias a métodos forman parte explícita del lenguaje desde Java 8 y están integradas con las interfaces funcionales. Un método sin parámetros y sin valor de retorno puede referenciarse mediante una interfaz como `Runnable`. El operador `::` se utiliza para obtener la referencia al método de un objeto concreto.

```java
public class Persona {
    private String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }

    public static void main(String[] args) {
        Persona p = new Persona("Ana");
        Runnable saludo = p::saludar;

        saludo.run();
    }
}
```

En ambos lenguajes, la referencia a métodos permite desacoplar **el lugar donde se define el comportamiento** del lugar donde se ejecuta. Este enfoque mejora la expresividad del código y refuerza el estilo funcional, ya que el comportamiento se maneja como un valor que puede almacenarse, pasarse e invocarse de forma flexible y segura.



## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta

En Java existen **cuatro tipos principales de referencias a método**, todas ellas introducidas para integrarse con las interfaces funcionales y facilitar un estilo de programación funcional más expresivo. Una referencia a método permite reutilizar un método existente allí donde se espera una función lambda compatible, siempre que la firma del método coincida con la del método abstracto de la interfaz funcional utilizada.

La primera es la **referencia a un método estático**, que no depende de ninguna instancia concreta. Se expresa con la sintaxis `Clase::metodoEstatico` y se usa cuando el método pertenece a la clase y no a un objeto. El compilador verifica que la firma del método es compatible con la interfaz funcional esperada.

```java
public class Utilidades {
    public static String aMayusculas(String s) {
        return s.toUpperCase();
    }
}

Function<String, String> f = Utilidades::aMayusculas;
```

La segunda es la **referencia a un método de instancia de un objeto concreto**, que se expresa como `objeto::metodo`. En este caso, el método se invocará siempre sobre esa instancia específica, lo que resulta equivalente a una lambda que llama a ese método sobre dicho objeto.

```java
Persona p = new Persona("Ana");
Runnable saludo = p::saludar;
```

La tercera es la **referencia a un método de instancia sobre cualquier instancia de una clase**, con la forma `Clase::metodoInstancia`. Aquí, el objeto sobre el que se invoca el método se pasa implícitamente como primer parámetro. Este tipo se usa habitualmente en operaciones sobre colecciones, como comparaciones o transformaciones.

```java
Function<String, Integer> longitud = String::length;
```

Por último, existe la **referencia a constructor**, que permite tratar un constructor como una función que crea objetos. Se expresa como `Clase::new` y se utiliza cuando se desea instanciar objetos de forma funcional, por ejemplo en fábricas o transformaciones.

```java
Supplier<Persona> creador = Persona::new;
```

Estos cuatro tipos cubren los casos más habituales de reutilización de comportamiento existente, reforzando la legibilidad del código y evitando lambdas innecesarias cuando ya existe un método adecuado.



## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta

En Java, la clase `Collections` proporciona el método `sort`, que permite ordenar una lista recibiendo como parámetro un **comparador**, es decir, una función que define cómo comparar dos elementos. Desde una perspectiva funcional, ese comparador puede expresarse mediante una **expresión lambda**, evitando la necesidad de crear clases adicionales. En este caso, la lista contiene objetos de tipo `Persona`, que tienen un nombre y una edad, y se desea ordenar primero por edad y, en caso de empate, por orden alfabético del nombre.

Una primera versión consiste en definir **manualmente la lógica de comparación dentro de la lambda**, implementando explícitamente las condiciones. El comparador recibe dos objetos `Persona` y devuelve un entero negativo, cero o positivo según el orden deseado. Esta forma es directa y clara, aunque algo más verbosa cuando los criterios de comparación crecen.

```java
import java.util.*;

class Persona {
    private String nombre;
    private int edad;

    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }

    public String getNombre() {
        return nombre;
    }

    public int getEdad() {
        return edad;
    }
}

List<Persona> personas = Arrays.asList(
    new Persona("Ana", 30),
    new Persona("Juan", 25),
    new Persona("Luis", 30)
);

Collections.sort(personas, (p1, p2) -> {
    if (p1.getEdad() != p2.getEdad()) {
        return Integer.compare(p1.getEdad(), p2.getEdad());
    } else {
        return p1.getNombre().compareTo(p2.getNombre());
    }
});
```

Una segunda versión más expresiva utiliza los **métodos auxiliares de `Comparator`**, que permiten componer comparaciones de forma declarativa. Con `Comparator.comparing` y `thenComparing`, se especifican los criterios de ordenación de manera encadenada, lo que mejora la legibilidad y reduce el código accidental. Esta forma es especialmente recomendable cuando se combinan varios criterios.

```java
Collections.sort(personas,
    Comparator.comparing(Persona::getEdad)
              .thenComparing(Persona::getNombre)
);
```

Ambas versiones producen el mismo resultado, pero la segunda aprovecha mejor los **aspectos funcionales de Java**, haciendo el código más claro y alineado con un estilo declarativo. El uso de lambdas y comparadores compuestos permite centrar la atención en las reglas de ordenación, sin distraerse con detalles de implementación del bucle o del algoritmo de ordenación.

