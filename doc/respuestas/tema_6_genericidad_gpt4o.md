<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### Respuesta

En lenguajes sin mecanismos de genericidad propiamente dichos, como C o Java sin usar genéricos, es habitual recurrir a un **tipo común** para poder almacenar datos de distintos tipos en una misma estructura. En C se emplea `void*`, que es un puntero genérico, mientras que en Java se utiliza la clase base `Object`, de la que heredan todas las clases. En ambos casos, la idea es similar: sacrificar información de tipo para ganar flexibilidad en el almacenamiento.

En C, puede definirse una estructura de datos basada en un array de punteros `void*`. Este array permite almacenar direcciones de memoria de cualquier tipo de dato. La estructura no conoce el tipo real de los elementos, por lo que es responsabilidad del programador recordar qué tipo se ha almacenado y realizar las conversiones necesarias al acceder a los datos. Esto permite construir, por ejemplo, una lista o un vector capaz de guardar enteros, reales o estructuras, aunque sin comprobación de tipos en tiempo de compilación.

```c
typedef struct {
    void* datos[10];
    int numElementos;
} ListaGenerica;

/* Almacenamiento */
int x = 5;
double y = 3.14;
lista.datos[0] = &x;
lista.datos[1] = &y;

/* Recuperación */
int valor = *(int*)lista.datos[0];
```

En Java, una solución equivalente consiste en usar un array de `Object`. Dado que todas las clases heredan de `Object`, cualquier instancia puede almacenarse en ese array. Al recuperar los elementos, es necesario realizar una conversión explícita (casting) al tipo concreto esperado. Al igual que en C, esta técnica proporciona flexibilidad, pero pierde seguridad de tipos y puede producir errores en tiempo de ejecución si la conversión no es correcta.

```java
Object[] datos = new Object[10];
datos[0] = Integer.valueOf(5);
datos[1] = "Hola";

int x = (Integer) datos[0];
String s = (String) datos[1];
```

Este tipo de soluciones motivó la aparición de la **genericidad**, ya que permite reutilizar estructuras de datos sin perder información de tipo ni depender de conversiones manuales, aumentando así la seguridad y claridad del código.

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### Respuesta

La **programación genérica** consiste en diseñar algoritmos y estructuras de datos que puedan operar con **distintos tipos de datos**, sin necesidad de reescribir el mismo código para cada tipo concreto. La idea principal es separar la lógica del algoritmo del tipo de datos que maneja, permitiendo así una mayor reutilización del código y una mayor claridad en su diseño. En lenguajes como Java, la programación genérica está pensada para que esa flexibilidad se consiga manteniendo la seguridad de tipos en tiempo de compilación.

Desde un punto de vista conceptual, la programación genérica busca resolver un problema habitual: muchas estructuras de datos (listas, pilas, colas, vectores) funcionan igual independientemente del tipo de elementos que almacenan. En lugar de crear una versión para enteros, otra para reales y otra para objetos, se define una única estructura genérica que se adapta al tipo concreto al usarse. Esto evita duplicación de código y reduce errores asociados a conversiones incorrectas.

El ejemplo anterior usando `void*` en C o `Object` en Java **no es programación genérica propiamente dicha**, sino una técnica previa que intenta imitarla. Aunque permite almacenar cualquier tipo de dato, se pierde la información de tipo original y se requieren conversiones explícitas al recuperar los elementos. Estas conversiones no se comprueban en tiempo de compilación, por lo que los errores solo aparecen en ejecución. La programación genérica real surge precisamente para ofrecer esa flexibilidad **sin perder seguridad de tipos**, algo que los ejemplos anteriores no garantizan.


## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Respuesta

El principal problema respecto al **chequeo de tipos** al emplear `void*` en C o `Object` en Java es que se **pierde la información del tipo real** de los datos almacenados. La estructura de datos deja de conocer qué tipo concreto contiene cada elemento, por lo que el compilador no puede verificar si las operaciones realizadas sobre esos datos son correctas. Como consecuencia, se elimina el control estático de tipos que normalmente ayudan a detectar errores durante la compilación.

Al recuperar un valor almacenado mediante `void*` u `Object`, es obligatorio realizar una **conversión explícita** al tipo esperado. El compilador asume que dicha conversión es correcta, aunque no lo sea realmente. Si el programador se equivoca y convierte un dato a un tipo incompatible, el error no se detecta en tiempo de compilación, sino en tiempo de ejecución. En C esto puede provocar comportamientos indefinidos o errores difíciles de depurar, y en Java suele manifestarse como una excepción `ClassCastException`.

Otro problema importante es que estas estructuras permiten **mezclar tipos sin ningún control**, lo que hace que el uso correcto de la estructura dependa exclusivamente de la disciplina del programador. No existe ninguna garantía formal de que todos los elementos almacenados pertenezcan a un mismo tipo lógico, ni tampoco de que se recuperen correctamente. Esto reduce la legibilidad del código y aumenta la probabilidad de errores, especialmente en programas grandes o mantenidos por varias personas.

Estos inconvenientes justifican la introducción de la **programación genérica**, que permite definir estructuras de datos flexibles pero con chequeo de tipos en tiempo de compilación. Así se evita el uso de conversiones explícitas peligrosas y se logra un equilibrio entre reutilización de código y seguridad, algo que no se consigue usando únicamente `void*` u `Object`.




## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Respuesta

Los **parámetros de tipo** son un mecanismo propio de la programación genérica que permite definir clases, interfaces o métodos utilizando **tipos aún no concretados**, que se especifican posteriormente cuando se usan. En lugar de trabajar directamente con tipos concretos como `int`, `String` u `Object`, se emplean identificadores simbólicos (como `T`, `E` o `K`) que representan un tipo genérico. De este modo, el código se escribe una sola vez y se adapta automáticamente al tipo real que se indique al utilizarlo.

Desde un punto de vista conceptual, un parámetro de tipo actúa como una **variable de tipo**, de forma similar a cómo una variable normal representa un valor. Cuando se crea una instancia de una clase genérica, o se invoca un método genérico, ese parámetro se sustituye por un tipo concreto. A partir de ese momento, el compilador conoce exactamente qué tipo se está usando y puede realizar el chequeo de tipos correspondiente, algo que no ocurre cuando se usa `Object` o `void*`.

En Java, los parámetros de tipo se definen entre los símbolos `< >` y permiten indicar que una estructura de datos trabajará siempre con un tipo coherente. Por ejemplo, una clase `Caja<T>` almacena un único elemento de tipo `T`, pero ese tipo puede ser `Integer`, `String` u otro cualquiera según el uso. Esto evita conversiones explícitas y garantiza que solo se puedan almacenar y recuperar valores del tipo correcto.

Gracias a los parámetros de tipo, la programación genérica logra su objetivo principal: **reutilizar código sin perder seguridad de tipos**. El compilador detecta errores antes de la ejecución, el código resulta más claro y se eliminan muchos problemas asociados a las conversiones manuales, mejorando la robustez y mantenibilidad de los programas.



## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta

En **Java**, la programación genérica se materializa mediante *generics*, que permiten definir colecciones tipadas con total seguridad en tiempo de compilación. Al instanciar una lista indicando `String` como parámetro de tipo, se garantiza que solo se puedan almacenar objetos de ese tipo. El compilador impide introducir elementos incompatibles y, al recorrer la colección, no es necesario realizar conversiones explícitas, ya que cada elemento se reconoce directamente como `String`.

```java
import java.util.ArrayList;
import java.util.List;

List<String> lista = new ArrayList<>();

lista.add("Hola");
lista.add("Mundo");
// lista.add(5); // Error de compilación

for (String s : lista) {
    System.out.println(s.toUpperCase());
}
```

Este ejemplo muestra que la lista solo admite cadenas y que cada elemento recuperado es de tipo `String` con seguridad. Cualquier intento de insertar un tipo distinto se detecta en tiempo de compilación, evitando errores en ejecución. Además, el código resulta más legible y no depende de conversiones manuales como ocurría con `Object`.

En **C++**, la programación genérica se implementa mediante *templates*. La biblioteca estándar proporciona estructuras como `std::vector`, que pueden parametrizarse con un tipo concreto. Al crear un `std::vector<std::string>`, se asegura que todos sus elementos sean cadenas y que las operaciones sobre ellos sean coherentes con ese tipo, también con chequeo estático.

```cpp
#include <iostream>
#include <vector>
#include <string>

int main() {
    std::vector<std::string> v;

    v.push_back("Hola");
    v.push_back("Mundo");
    // v.push_back(5); // Error de compilación

    for (const std::string& s : v) {
        std::cout << s << std::endl;
    }
}
```

En este caso, el compilador de C++ controla que solo se inserten `std::string` y que cada elemento recorrido sea de ese tipo concreto. Tanto en Java como en C++, estos mecanismos muestran cómo la programación genérica permite reutilizar estructuras de datos manteniendo la seguridad de tipos, superando claramente las limitaciones de soluciones basadas en `void*` u `Object`.



## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta

Cuando se instancia una clase con **parámetros de tipo**, el compilador utiliza esa información para **comprobar que los tipos usados son coherentes**, pero la forma en que lo hace depende del lenguaje. En general, la instanciación consiste en sustituir el parámetro de tipo por el tipo concreto indicado y verificar que todas las operaciones realizadas sobre ese tipo son válidas. Gracias a ello, se detectan errores de tipo en tiempo de compilación y se evita el uso de conversiones explícitas inseguras.

En **Java**, el compilador aplica un mecanismo llamado **type erasure** (borrado de tipos). Esto significa que la información de los parámetros de tipo solo existe durante la compilación. Una vez generado el bytecode, los tipos genéricos se reemplazan por su límite superior (normalmente `Object`), y las comprobaciones necesarias se insertan automáticamente. En tiempo de ejecución no se conserva información sobre el tipo genérico, lo que explica por qué no es posible, por ejemplo, crear un `new T()` o comprobar `instanceof T`.

En **C++**, el enfoque es diferente y se basa en la **instanciación de plantillas (templates)**. Cada vez que se usa una plantilla con un tipo concreto, el compilador genera una versión específica del código para ese tipo. Así, un `vector<string>` y un `vector<int>` son tipos distintos con código distinto generado en compilación. En este caso, la información de tipo sí se conserva y no existe un borrado equivalente al de Java.

Por tanto, **C++ y Java no hacen lo mismo** al implementar la programación genérica. Java prioriza la compatibilidad y el control en tiempo de compilación mediante el borrado de tipos, mientras que C++ genera código especializado para cada instanciación. Ambos enfoques proporcionan seguridad de tipos, pero con implicaciones distintas en rendimiento, flexibilidad y comportamiento en tiempo de ejecución.



## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### Respuesta

Para definir una nueva clase genérica en Java se emplean **parámetros de tipo** en la declaración de la clase. En este caso, la clase `Par` permite almacenar dos valores cuyos tipos pueden ser distintos y se representan mediante dos parámetros de tipo, por ejemplo `A` y `B`. La clase no conoce de antemano los tipos concretos, pero el compilador los comprobará cuando la clase se instancie. Esto permite reutilizar la clase para múltiples combinaciones de tipos manteniendo la seguridad en tiempo de compilación.

La clase `Par` puede incluir un constructor que reciba ambos valores y métodos *getter* para acceder a cada uno de ellos. Gracias a la programación genérica, los métodos devuelven directamente el tipo correcto, sin necesidad de conversiones explícitas ni uso de `Object`. El compilador garantiza que los tipos usados al crear el objeto coinciden con los tipos que se obtienen al acceder a sus valores.

```java
public class Par<A, B> {
    private A primero;
    private B segundo;

    public Par(A primero, B segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public A getPrimero() {
        return primero;
    }

    public B getSegundo() {
        return segundo;
    }
}
```

Un uso habitual de una clase como `Par` es devolver más de un resultado desde una función. Por ejemplo, una función que calcule la **media** y la **desviación típica** de un array de `double` puede devolver ambos valores agrupados en un `Par<Double, Double>`. De este modo, el tipo de cada valor queda claramente especificado y comprobado por el compilador.

```java
public static Par<Double, Double> mediaYDesviacion(double[] datos) {
    double suma = 0.0;
    for (double x : datos) {
        suma += x;
    }
    double media = suma / datos.length;

    double sumaCuadrados = 0.0;
    for (double x : datos) {
        sumaCuadrados += (x - media) * (x - media);
    }
    double desviacion = Math.sqrt(sumaCuadrados / datos.length);

    return new Par<>(media, desviacion);
}
```

En este ejemplo, al recibir el resultado, se sabe con certeza que el primer valor del `Par` es un `Double` que representa la media y el segundo es un `Double` correspondiente a la desviación típica. La programación genérica permite así expresar con claridad la intención del código y evitar errores de tipo en tiempo de compilación.



## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### Respuesta

En Java es posible definir **métodos genéricos** declarando parámetros de tipo a nivel de método, independientemente de que la clase sea genérica o no. Un método genérico indica explícitamente que trabajará con un tipo aún no concretado, que se fijará en el momento de la llamada. Esto permite expresar relaciones entre los parámetros y el valor de retorno, algo que no es posible usando simplemente `Object`.

Si el método `seleccionaUno` se define usando `Object`, puede recibir cualquier combinación de tipos y devolver un `Object`. Esto obliga a realizar **downcasting** al recuperar el resultado y no existe ninguna garantía de que ambos objetos sean del mismo tipo. El compilador no puede detectar errores si se pasan, por ejemplo, un `String` y un `Integer`, lo que puede provocar fallos en tiempo de ejecución.

```java
public static Object seleccionaUno(Object a, Object b) {
    return Math.random() < 0.5 ? a : b;
}

// Uso
String s = (String) seleccionaUno("Hola", "Mundo"); // casting necesario
```

En cambio, al definir el método con un **parámetro de tipo**, se indica que ambos parámetros y el valor devuelto deben ser del **mismo tipo concreto**. El compilador fuerza esta restricción, impide mezclar tipos distintos y elimina la necesidad de conversiones explícitas. De este modo, se gana seguridad y claridad en el uso del método.

```java
public static <T> T seleccionaUno(T a, T b) {
    return Math.random() < 0.5 ? a : b;
}

// Uso
String s = seleccionaUno("Hola", "Mundo"); // sin casting
// seleccionaUno("Hola", 5); // Error de compilación
```

Así, los métodos genéricos permiten evitar el *downcasting* y garantizan la coherencia de tipos entre los argumentos, algo que no se puede asegurar cuando se utilizan parámetros de tipo `Object`.



## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta

En Java **sí se pueden establecer restricciones en los parámetros de tipo**, mediante lo que se denomina *parámetros de tipo acotados*. Esto permite indicar que un tipo genérico debe ser, como mínimo, una subclase de otra clase o implementar cierta interfaz. De este modo, el compilador permite usar métodos definidos en esa clase base. En el caso de los números, Java proporciona la clase abstracta `Number`, de la que heredan tipos como `Integer`, `Double` o `Float`, lo que permite definir restricciones del estilo `<T extends Number>`.

Una primera solución, sin usar genericidad avanzada, consiste en definir las coordenadas del punto directamente como `Number`. Esto permite almacenar cualquier tipo numérico, pero pierde información concreta sobre el tipo real. Al trabajar con los valores, es necesario convertirlos a `double` usando métodos como `doubleValue()`, lo que provoca que todos los cálculos se realicen de forma genérica y menos expresiva.

```java
public class Punto {
    private Number x;
    private Number y;

    public Punto(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() {
        return x;
    }

    public Number getY() {
        return y;
    }

    public double calcularDistanciaA(Punto otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

Una segunda solución utiliza **parámetros de tipo acotados** para reforzar el chequeo de tipos y expresar claramente que todas las coordenadas de un `Punto` son del mismo tipo numérico. Con `<T extends Number>`, el compilador garantiza que `T` es un número y que tanto `x` como `y` comparten exactamente el mismo tipo. Esto mejora la legibilidad y reduce errores conceptuales en el uso de la clase.

```java
public class Punto<T extends Number> {
    private T x;
    private T y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() {
        return x;
    }

    public T getY() {
        return y;
    }

    public double calcularDistanciaA(Punto<T> otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

Respecto al **type erasure**, en ambos casos el tipo genérico `T` se elimina tras la compilación y se sustituye por su límite superior, que en este caso es `Number`. Por tanto, el tipo final que existe en el bytecode es `Punto` con campos de tipo `Number`. La diferencia es que, aunque en tiempo de ejecución el tipo sea el mismo, el uso de genéricos permite al compilador realizar un chequeo de tipos mucho más estricto durante la compilación.



## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta

Ambas soluciones permiten trabajar con distintos tipos de números sin duplicar la clase `Punto`, pero **no refuerzan el chequeo de tipos de la misma forma**. En la solución sin genéricos, al declarar las coordenadas como `Number`, no existe ninguna restricción que obligue a que ambas coordenadas sean del mismo tipo numérico. Por tanto, es perfectamente posible crear un punto con una coordenada entera y la otra real, por ejemplo pasando un `Integer` para `x` y un `Double` para `y`, sin que el compilador muestre ningún error.

En cambio, en la solución con genéricos `<T extends Number>`, el compilador **fuerza que ambas coordenadas sean exactamente del mismo tipo**. Al crear un `Punto<Integer>`, tanto `x` como `y` deben ser `Integer`, y no es posible mezclar tipos como `Integer` y `Double`. Cualquier intento de hacerlo es detectado en tiempo de compilación. Este refuerzo del chequeo de tipos evita errores conceptuales y deja más claro el modelo de datos que se está utilizando.

En cuanto al tipo devuelto por los métodos, en la solución sin genéricos el método `getX()` devuelve un `Number`. Esto implica que el código que use ese valor no sabe si en realidad se trata de un `Integer`, un `Double` u otro subtipo, y deberá tratarlo de forma genérica o recurrir a conversiones explícitas. Se pierde, por tanto, información sobre el tipo concreto de la coordenada.

Por el contrario, en la solución con genéricos, el método `getX()` devuelve el tipo `T`, que corresponde exactamente al tipo numérico con el que se haya instanciado el `Punto`. Si se trabaja con un `Punto<Double>`, el compilador sabe que `getX()` devuelve un `Double`. Aunque en tiempo de ejecución el tipo se borre por *type erasure*, en tiempo de compilación se mantiene un chequeo de tipos mucho más preciso, que es la principal ventaja de usar genéricos en este contexto.



## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

### Respuesta

El problema del diseño original es que el método `distanciaA` acepta cualquier `Punto`, lo que obliga a comprobar en tiempo de ejecución si el objeto recibido es del tipo correcto mediante `instanceof` y *downcasting*. Este enfoque rompe el polimorfismo limpio y desplaza los errores al tiempo de ejecución. Para evitarlo, se puede usar **genericidad con autorreferencia** (*F-bounded polymorphism*), de modo que cada tipo de punto solo pueda calcular distancias respecto a puntos del **mismo tipo concreto**, con chequeo completo en compilación.

La idea consiste en parametrizar la interfaz `Punto` con un parámetro de tipo que represente el tipo concreto que implementa la interfaz. Ese parámetro se usa después como tipo del argumento del método `distanciaA`. Así, al implementar la interfaz, cada clase fija ese parámetro a su propio tipo, forzando que la sobreescritura del método solo acepte puntos homogéneos, sin necesidad de comprobaciones dinámicas.

```java
public interface Punto<T extends Punto<T>> {
    double distanciaA(T p);
}
```

A partir de esta interfaz genérica, las implementaciones quedan completamente tipadas. Cada clase indica explícitamente su propio tipo al implementar la interfaz, y el compilador garantiza que nunca se pasará un punto de otra dimensión por error.

```java
public class Punto2D implements Punto<Punto2D> {
    private final double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double distanciaA(Punto2D p) {
        return Math.sqrt(
            Math.pow(x - p.x, 2) +
            Math.pow(y - p.y, 2)
        );
    }
}
```

```java
public class Punto3D implements Punto<Punto3D> {
    private final double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double distanciaA(Punto3D p) {
        return Math.sqrt(
            Math.pow(x - p.x, 2) +
            Math.pow(y - p.y, 2) +
            Math.pow(z - p.z, 2)
        );
    }
}
```

Con este diseño, el compilador impide directamente llamadas incorrectas como calcular la distancia entre un `Punto2D` y un `Punto3D`. Se elimina por completo el uso de `instanceof`, el *downcasting* y las excepciones manuales, reforzando el chequeo de tipos en tiempo de compilación y expresando con claridad que la distancia solo tiene sentido entre puntos del mismo tipo dimensional.



## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta

Aunque `String` es subtipo de `Object`, **`List<String>` no es subtipo de `List<Object>`** en Java. Esto se debe a que los genéricos en Java son **invariantes** respecto a su parámetro de tipo. Si se permitiera esa relación, sería posible insertar cualquier `Object` (por ejemplo, un `Integer`) en una lista que en realidad contiene `String`, rompiendo la seguridad de tipos. Por este motivo, el compilador prohíbe asignaciones o llamadas que asumirían esa relación de subtipo entre listas genéricas.

En cambio, los **arrays sí son covariantes** en Java, lo que implica que `String[]` **sí es subtipo de** `Object[]`. Esto permite, por ejemplo, asignar un array de `String` a una referencia de tipo `Object[]`. Sin embargo, esta decisión tiene un coste: el compilador no puede evitar ciertos errores, y estos solo se detectan en **tiempo de ejecución**. Si a través de una referencia `Object[]` se intenta almacenar un objeto que no sea `String`, se produce una excepción `ArrayStoreException`.

Este comportamiento explica por qué los arrays pueden fallar en ejecución mientras que los genéricos fallan en compilación. Los arrays conocen su tipo real en tiempo de ejecución y realizan comprobaciones dinámicas, mientras que los genéricos usan **type erasure**, por lo que la seguridad debe garantizarse estáticamente. Para evitar errores tardíos, Java opta por hacer los genéricos invariantes por defecto.

A partir de estos ejemplos, se puede definir que un tipo genérico es **covariante** si `Contenedor<Subtipo>` es subtipo de `Contenedor<Supertipo>` (como ocurre con los arrays), **contravariante** si ocurre la relación inversa, e **invariante** si no existe relación de subtipo en ningún sentido (como `List<T>` en Java). La invariancia es una elección de diseño que prioriza la seguridad de tipos en tiempo de compilación frente a la flexibilidad.



## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta

Un **wildcard** (`?`) en Java representa un **tipo desconocido** dentro de un tipo genérico y se utiliza para expresar variancia de forma controlada. Permite escribir métodos más flexibles sin perder seguridad de tipos. En lugar de exigir un tipo genérico exacto, el wildcard indica un rango de tipos permitidos, que el compilador utiliza para comprobar qué operaciones son seguras. Los wildcards no definen un tipo concreto, sino una **restricción** sobre qué tipos pueden aparecer.

La forma `List<? extends T>` expresa **covarianza**: la lista puede ser de `T` o de cualquier subtipo de `T`. Se utiliza cuando el método **consume** valores de la colección (los lee), pero no necesita añadir nuevos elementos. El compilador garantiza que todo elemento leído es, al menos, de tipo `T`. Sin embargo, no permite insertar elementos (salvo `null`), ya que el tipo concreto de la lista es desconocido dentro de ese rango. Este patrón se resume a menudo como *“producer extends”*.

```java
public static double suma(List<? extends Number> numeros) {
    double total = 0.0;
    for (Number n : numeros) {
        total += n.doubleValue();
    }
    return total;
}
```

Por el contrario, `List<? super T>` expresa **contravarianza**: la lista puede ser de `T` o de cualquier supertipo de `T`. Se utiliza cuando el método **produce** valores que se insertan en la colección. En este caso, sí se pueden añadir elementos de tipo `T`, pero al leerlos solo se garantiza que son `Object`, ya que el supertipo exacto es desconocido. Este patrón se resume como *“consumer super”*.

```java
public static void añadeEnteros(List<? super Integer> lista) {
    lista.add(1);
    lista.add(2);
    lista.add(3);
}
```

A partir de estos usos, se puede concluir que los wildcards permiten recuperar covarianza y contravarianza de forma segura en Java: `? extends T` se usa para **leer con seguridad**, `? super T` para **escribir con seguridad**, y los tipos genéricos sin wildcards (como `List<T>`) permanecen **invariantes**, priorizando el chequeo de tipos en tiempo de compilación.

