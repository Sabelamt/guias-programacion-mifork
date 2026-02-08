<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### Respuesta
En la Programación Orientada a Objetos, la encapsulación y la ocultación de información buscan que los datos internos de un objeto queden protegidos frente al exterior. La encapsulación consiste en agrupar en una misma unidad (la clase) tanto los datos como los métodos que operan sobre ellos, asegurando que el objeto controle cómo se accede y cómo se modifican sus atributos. La ocultación de información complementa este mecanismo limitando el acceso directo desde fuera de la clase mediante modificadores como private o protected, obligando a interactuar con el objeto únicamente a través de métodos públicos bien definidos.
Este enfoque permite que el estado interno de un objeto no pueda alterarse de forma accidental o incorrecta, lo que reduce errores y facilita mantener coherencia en los datos. Además, la ocultación permite modificar la implementación interior sin afectar al código externo que utiliza la clase, siempre que se mantenga la misma interfaz pública. Esto hace que el software sea más robusto, modular y fácil de mantener.
También se gana seguridad, ya que solo se exponen las operaciones necesarias y se evita que partes del programa manipulen datos sensibles sin control. Por último, favorece la reutilización de código: al tener clases bien encapsuladas, cada una cumple una función clara y puede utilizarse en otros proyectos sin necesidad de conocer su lógica interna.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.
La interfaz pública de una clase u objeto se entiende como el conjunto de métodos y, en menor medida, constantes públicas que se ponen a disposición del exterior para interactuar con dicho objeto. Esta interfaz define qué operaciones se pueden realizar sobre la instancia, actuando como un contrato que indica cómo deben comunicarse otras partes del programa con esa clase. No describe cómo funcionan internamente esas operaciones, sino únicamente qué acciones ofrece y qué parámetros necesitan.
Su relación con la ocultación de información es directa, porque la interfaz pública actúa como la única vía oficial de acceso al objeto, mientras que los detalles internos quedan escondidos mediante atributos y métodos privados. Gracias a esta separación, se permite modificar libremente la implementación interna sin afectar al código externo, siempre que la interfaz pública se mantenga estable. Además, esta ocultación reduce la probabilidad de errores, ya que impide el acceso directo a datos sensibles o inconsistentes, obligando a usar métodos controlados que aseguran un manejo correcto del estado del objeto.
Por último, la existencia de una interfaz pública clara mejora la modularidad y la comprensión del programa, ya que quien utiliza la clase solo necesita entender la parte visible de la misma. Esta forma de trabajar facilita la reutilización del código, el mantenimiento y la ampliación futura del sistema.

### Respuesta


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta
La interfaz pública debe diseñarse con cuidado porque representa el contrato mediante el cual el resto del programa interactúa con la clase. Si este contrato es confuso, demasiado amplio o mal estructurado, se dificulta el uso correcto de la clase y aumenta el riesgo de errores. Además, una interfaz excesivamente expuesta puede permitir manipulaciones no deseadas del estado interno, rompiendo la idea de encapsulación y reduciendo la robustez del sistema. Por tanto, definir qué métodos deben ser realmente públicos y cuáles deben permanecer escondidos es una decisión crucial en el diseño orientado a objetos.
Modificar la interfaz pública no suele ser fácil, porque cualquier cambio afecta al código externo que ya utiliza esa clase. Si se renombra un método, se cambian los parámetros o se elimina una operación, todas las partes que dependían de esa interfaz dejarán de funcionar correctamente. En proyectos grandes, este impacto puede ser considerable, ya que la interfaz actúa como un punto de unión entre múltiples módulos. Por esta razón, suele recomendarse diseñar una interfaz pública estable, mínima y bien pensada desde el principio.
Al mantener una interfaz clara y consistente, se facilita la evolución interna de la clase sin romper compatibilidades. La implementación puede cambiar todo lo que sea necesario —optimización, refactorización, reorganización— siempre que la interfaz permanezca igual. Esto permite mejorar el código sin generar efectos colaterales, lo que demuestra la importancia de dedicar tiempo a que la interfaz pública sea sencilla, coherente y duradera.

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta
Las invariantes de clase son condiciones lógicas que deben cumplirse siempre que un objeto esté en un estado válido. Dichas condiciones suelen expresar propiedades que los atributos de la clase deben mantener de forma permanente, excepto durante la ejecución interna de un método. Por ejemplo, en una clase que represente una cuenta bancaria, una invariante podría ser que el saldo nunca sea negativo. Estas reglas definen qué significa que un objeto esté correctamente construido y operando dentro de los límites esperables.
La ocultación de información ayuda a mantener estas invariantes porque impide que el código externo modifique los atributos directamente. Si los datos internos estuvieran expuestos, cualquier parte del programa podría alterarlos sin pasar por controles, pudiendo romper las condiciones que deben mantenerse. Al restringir el acceso mediante atributos privados y métodos públicos cuidadosamente diseñados, se garantiza que solo las operaciones permitidas modifiquen el estado y que todas ellas incluyan las validaciones necesarias.
Además, al ocultar los detalles internos, la clase puede reorganizar su implementación sin comprometer las invariantes. Esto permite que los métodos públicos actúen como guardianes que aseguran que cada cambio en el estado respete las reglas establecidas. Gracias a este control centralizado, se consigue un diseño más robusto, coherente y resistente a errores provenientes de otras partes del programa.

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta
public class Punto {
    // Atributos ocultos (estado interno)
    private double x;
    private double y;

    // Constructor público (parte de la interfaz)
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Métodos de acceso controlado (parte de la interfaz)
    public double getX() { return x; }
    public double getY() { return y; }

    // Métodos de modificación controlada (opcionalmente parte de la interfaz)
    public void setX(double x) { this.x = x; }
    public void setY(double y) { this.y = y; }

    // Operación pública (parte de la interfaz)
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

En esta clase, la ocultación de información se implementa declarando los atributos x e y como private, de modo que no puedan modificarse directamente desde fuera de la clase. El acceso y la modificación del estado se realizan a través de métodos públicos (getX, getY, setX, setY) que constituyen un punto de control único. El método calcularDistanciaAOrigen ofrece una operación pública que trabaja con el estado interno sin exponer su representación. Si en el futuro cambiase la forma de almacenar las coordenadas (por ejemplo, en módulo–ángulo en lugar de x/y), la implementación podría modificarse sin afectar al código cliente, siempre que la interfaz pública se mantenga.
La interfaz pública de Punto está compuesta por: el constructor público Punto(double, double), los métodos públicos getX(), getY(), setX(double), setY(double) y calcularDistanciaAOrigen(). En cuanto a los modificadores: public significa que el miembro es accesible desde cualquier clase; private significa que el miembro solo es accesible desde dentro de la propia clase. Esta separación permite proteger la coherencia del objeto y mantener las invariantes (por ejemplo, si se quisiesen validar rangos o formatos), al tiempo que se ofrece una API clara para el exterior.


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta
En Java, public puede aplicarse a clases, interfaces, enums y records de nivel superior (top‑level), lo que permite que sean accesibles desde cualquier paquete. También puede aplicarse a miembros de una clase (campos, métodos, constructores) y a tipos anidados (clases, interfaces, enums y records definidos dentro de otra clase), otorgándoles visibilidad global. Si una clase de nivel superior no lleva modificador, su acceso es package‑private (accesible solo dentro del mismo paquete), pero no puede marcarse como private ni protected.
El modificador private no es válido para tipos de nivel superior; solo puede usarse en miembros (campos, métodos, constructores) y en tipos anidados dentro de una clase. Esto permite, por ejemplo, ocultar detalles de implementación, restringir constructores (patrones como singleton o clases utilitarias) y garantizar invariantes. En cambio, parámetros, variables locales y clases locales (definidas dentro de métodos) no admiten public ni private como modificadores de acceso.
En resumen:

public: válido para tipos top‑level (clase/interfaz/enum/record) y para miembros y tipos anidados, con acceso universal.
private: válido solo para miembros y tipos anidados dentro de una clase, con acceso restringido a la propia clase.
Top‑level: solo pueden ser public o package‑private (sin modificador); no private/protected.


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta
En Programación Orientada a Objetos existen más tipos de visibilidad además de la pública y la privada, aunque su disponibilidad concreta depende del lenguaje. La idea general es ofrecer distintos niveles de acceso que permitan modular el grado de encapsulación. Algunos lenguajes introducen visibilidades intermedias para facilitar la colaboración entre clases relacionadas o pertenecientes al mismo módulo, sin llegar a exponer completamente la implementación al exterior.
En Java, además de public y private, existen otros dos modificadores: protected y package‑private (este último aparece cuando no se usa ningún modificador). protected permite el acceso desde la propia clase, desde las clases del mismo paquete y desde las subclases, incluso aunque estén en paquetes distintos. El acceso package‑private restringe el uso a cualquier clase del mismo paquete, actuando como un ámbito de visibilidad intermedio útil para organizar el código en módulos lógicos. Gracias a estas opciones, Java permite un control bastante fino de la encapsulación sin complicar en exceso el modelo.
En otros lenguajes también aparecen visibilidades adicionales. C++, por ejemplo, dispone de public, protected y private, pero su protected funciona de manera ligeramente distinta a la de Java, y además incorpora mecanismos como friend, que permiten a clases o funciones específicas acceder a los miembros privados de otra clase. Lenguajes como C# añaden todavía más granularidad con modificadores como internal o protected internal, pensados para controlar la visibilidad dentro de ensamblados. Estos matices muestran que la encapsulación no se limita a dos niveles, sino que ofrece diferentes capas según la filosofía de cada lenguaje y las necesidades de modularización del programa.

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta
Los miembros privados están ocultos para otras clases, pero no para otras instancias de la misma clase. En Java, el modificador private restringe el acceso al ámbito de la clase, no al de la instancia. Esto significa que cualquier método de Punto puede acceder a los campos privados x e y de cualquier objeto Punto, incluido el que se recibe como parámetro. Lo que se impide es que otras clases (externas a Punto) accedan directamente a esos campos.
A modo de ejemplo, se añade un método calcularDistanciaAPunto(Punto otro) dentro de la propia clase. Obsérvese que el método accede a otro.x y otro.y sin necesidad de getters, porque ese acceso ocurre desde dentro de la misma clase:
public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    public double calcularDistanciaAPunto(Punto otro) {
        if (otro == null) {
            throw new IllegalArgumentException("El punto de destino no puede ser null");
        }
        double dx = this.x - otro.x; // Válido: acceso a campos privados de otra instancia de la MISMA clase
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
En consecuencia, la respuesta correcta es: (a) están ocultos para otras clases, no para otras instancias de la misma clase. Este comportamiento permite implementar operaciones entre objetos del mismo tipo (como comparaciones, copias, distancias, igualdad estructural, etc.) sin exponer los atributos al exterior, manteniendo la encapsulación y las invariantes, pero conservando la eficiencia y claridad dentro de la propia clase.

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta
Los métodos getter y setter son operaciones públicas que permiten acceder y modificar, respectivamente, los atributos privados de un objeto. En los lenguajes orientados a objetos se utilizan para mantener la encapsulación, ya que evitan que el código externo manipule directamente los campos internos de una clase. Un getter devuelve el valor de un atributo, mientras que un setter permite cambiarlo de forma controlada. Por convención, suelen seguir el formato getNombreAtributo() y setNombreAtributo(...).
La utilidad de estos métodos radica en que proporcionan un punto de control sobre cómo se leen o modifican los datos internos. Un setter, por ejemplo, puede incluir validaciones para evitar que se asigne un valor inválido, manteniendo así las invariantes de la clase. Del mismo modo, un getter puede devolver cálculos derivados, copias defensivas o formatos específicos sin exponer el estado original directamente. Esta capa intermedia añade seguridad y robustez al diseño orientado a objetos.
Además, los getters y setters permiten modificar la implementación interna sin afectar al código que utiliza la clase. Si en algún momento se cambia la representación interna de los datos, la interfaz pública (los métodos) puede mantenerse igual, evitando romper dependencias externas. Por eso forman parte fundamental de la encapsulación: permiten exponer solo lo necesario, de manera controlada y estable, manteniendo oculta la complejidad o sensibilidad del estado interno del objeto.

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta
Cuando en Programación Orientada a Objetos se afirma que la ocultación de información mejora la “seguridad”, no se está hablando de seguridad en el sentido de proteger el programa frente a ataques externos o hacking. El término se refiere a una seguridad lógica o estructural, cuyo objetivo es evitar que otras partes del programa utilicen los objetos de manera incorrecta, rompan invariantes o generen estados inválidos. Es, por tanto, un concepto más cercano a la robustez y fiabilidad interna que a la ciberseguridad.
La ocultación de información ayuda a garantizar que el estado interno del objeto solo pueda modificarse mediante operaciones controladas, lo que reduce la aparición de errores sutiles en el software. Si los atributos fueran públicos, cualquier parte del programa podría alterarlos sin restricciones, ocasionando comportamientos incoherentes o difíciles de depurar. Al emplear campos privados y métodos públicos cuidadosamente diseñados, se consigue que todas las modificaciones del estado pasen por validaciones, preservando así la integridad del objeto.
Esta forma de “seguridad” también facilita el mantenimiento, ya que evita dependencias excesivas entre módulos y permite cambiar la implementación interna sin afectar al exterior. No obstante, no debe confundirse con la protección frente a amenazas reales: la encapsulación no evita accesos maliciosos, inyecciones, vulnerabilidades o ataques de red. Su propósito es garantizar que, dentro del propio programa, los objetos se utilicen correctamente y mantengan su coherencia interna.

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta
Un miembro de instancia es aquel que pertenece a cada objeto individual creado a partir de una clase. Esto implica que cada instancia mantiene su propia copia de los atributos y puede tener valores distintos para cada uno de ellos. Por ejemplo, en una clase Punto, cada objeto tendría sus propios valores de x e y. Los métodos de instancia también dependen del estado concreto de cada objeto, ya que operan sobre sus atributos individuales.
Por el contrario, un miembro de clase (marcado con static en Java) pertenece a la clase en sí misma y no a cada objeto. Esto significa que existe una única copia compartida entre todas las instancias, y puede utilizarse incluso sin crear objetos. Un ejemplo típico es un contador de instancias o una constante compartida. Los métodos estáticos tampoco pueden acceder directamente a los atributos de instancia si no se les proporciona una referencia explícita a un objeto.
En cuanto a la ocultación, los miembros de clase también pueden ocultarse usando private. Esto permite, por ejemplo, proteger constantes internas, contadores, cachés o métodos auxiliares que no deban ser accesibles desde fuera de la clase. De esta manera, se aplica la misma lógica de encapsulación tanto a los elementos estáticos como a los de instancia, garantizando que el acceso a los recursos compartidos siga estando estrictamente controlado y coherente con el diseño de la clase.

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta
Sí, tiene sentido que los constructores sean privados en ciertos diseños, porque permite **controlar completamente cómo y cuándo se crean las instancias** de una clase. Cuando un constructor es privado, ninguna otra clase puede invocarlo directamente, lo que obliga a utilizar métodos internos o estáticos para obtener las instancias. Este mecanismo se emplea cuando no interesa que el usuario del código pueda crear objetos libremente, ya sea por motivos de eficiencia, restricciones lógicas o control de recursos.

Un caso muy habitual es el **patrón Singleton**, donde solo debe existir una única instancia de la clase. Al declarar el constructor como privado, se evita que alguien pueda crear más objetos desde el exterior. Otro ejemplo frecuente es el uso de **métodos fábrica** (*factory methods*), donde el constructor privado obliga a crear instancias a través de métodos estáticos que pueden aplicar validaciones, elegir subclases, reutilizar objetos o gestionar cachés internas.

También resulta útil cuando se desea que una clase actúe únicamente como **contenedor de métodos estáticos** y no tenga sentido instanciarla. En ese caso, el constructor privado impide la creación accidental de objetos. En general, hacer privados los constructores permite reforzar la encapsulación, controlar la creación de instancias y mantener las invariantes de la clase de forma más estricta.


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta
En Java, los **miembros de clase** se indican con el modificador `static`. Un miembro `static` pertenece a la **clase** y no a cada instancia, por lo que existe una única copia compartida por todos los objetos. Esto permite mantener información agregada (por ejemplo, contadores, cachés o, como en este caso, los máximos globales) y acceder a ella sin necesidad de crear instancias, mediante `NombreDeLaClase.miembro`. Al combinar `static` con `private` y **métodos de acceso públicos**, se mantiene la encapsulación también para los miembros de clase.

A continuación, se extiende la clase `Punto` para incluir dos **miembros de clase** que registren los máximos `x` e `y` vistos hasta el momento. Se emplean campos `private static` para almacenar los máximos y métodos `public static` para consultarlos. La actualización de dichos máximos se realiza en el **constructor** y en los **setters**, de modo que cualquier cambio en una coordenada pasa por el mismo punto de control y mantiene la invariante “`maxX` y `maxY` reflejan los mayores valores asignados a cualquier `Punto`”.

```java
public class Punto {
    // --- Estado de instancia (oculto) ---
    private double x;
    private double y;

    // --- Estado de clase (compartido por todas las instancias) ---
    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    // --- Constructor (interfaz pública) ---
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        actualizarMaximos(x, y);
    }

    // --- Getters (interfaz pública) ---
    public double getX() { return x; }
    public double getY() { return y; }

    // --- Setters (interfaz pública) ---
    public void setX(double x) {
        this.x = x;
        actualizarMaximos(x, this.y);
    }

    public void setY(double y) {
        this.y = y;
        actualizarMaximos(this.x, y);
    }

    // --- Operación de instancia (interfaz pública) ---
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    // --- Miembros de clase: consulta de máximos (interfaz pública de clase) ---
    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }

    // --- Método de clase auxiliar (oculto a la interfaz pública externa) ---
    private static void actualizarMaximos(double x, double y) {
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }
}
```

Con este diseño, `maxX` y `maxY` **no** forman parte del estado de cada objeto, sino de la **clase**; por eso se consultan con `Punto.getMaxX()` y `Punto.getMaxY()` sin necesidad de instanciar. La encapsulación se preserva marcando los campos estáticos como `private` y exponiendo únicamente los getters estáticos. Si se quisiese reforzar el diseño en entornos concurrentes, podría evaluarse la sincronización de `actualizarMaximos` o el uso de tipos atómicos; para el contexto introductorio, la solución mostrada ilustra de forma clara cómo se **indican** y **utilizan** los miembros de clase en Java.


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta
Aquí tienes **solo el método**, tal como pides.  
Sí, en un método factoría **se usa `static`**, porque crea objetos **sin necesidad de tener una instancia previa**.

```java
public static Punto crearRedondeado(double x, double y) {
    long rx = Math.round(x);
    long ry = Math.round(y);
    return new Punto(rx, ry);
}
```

Si quieres, puedo explicarte por qué `static` es necesario y cómo encaja esto con la encapsulación.


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta
A continuación se muestra una **nueva implementación** de `Punto` que **cambia la representación interna** a un arreglo `double[2]` (`coords[0]` para `x` y `coords[1]` para `y`), **sin modificar la interfaz pública** previamente expuesta: mismos constructores y métodos públicos (`getX`, `getY`, `setX`, `setY`, `calcularDistanciaAOrigen`, `calcularDistanciaAPunto`, y los estáticos `getMaxX`, `getMaxY`). Este cambio ilustra cómo la **ocultación de información** permite alterar la estructura interna sin afectar al código cliente.

Se mantienen también los **miembros de clase** para registrar máximos globales y la lógica de actualización centralizada. La clase valida argumentos allí donde tiene sentido (por ejemplo, comprobación de `null` en `calcularDistanciaAPunto`). De esta manera, se preservan las **invariantes** y el contrato público sin que el exterior necesite conocer si la representación es por atributos sueltos o mediante un arreglo.

```java
public class Punto {
    // --- Representación interna ocultada: array de dos posiciones ---
    private final double[] coords = new double[2]; // coords[0] = x, coords[1] = y

    // --- Estado de clase (compartido por todas las instancias) ---
    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    // --- Constructor (interfaz pública, sin cambios de firma) ---
    public Punto(double x, double y) {
        coords[0] = x;
        coords[1] = y;
        actualizarMaximos(x, y);
    }

    // --- Getters (interfaz pública, sin cambios) ---
    public double getX() { return coords[0]; }
    public double getY() { return coords[1]; }

    // --- Setters (interfaz pública, sin cambios) ---
    public void setX(double x) {
        coords[0] = x;
        actualizarMaximos(x, coords[1]);
    }

    public void setY(double y) {
        coords[1] = y;
        actualizarMaximos(coords[0], y);
    }

    // --- Operación pública de instancia (sin cambios) ---
    public double calcularDistanciaAOrigen() {
        double x = coords[0];
        double y = coords[1];
        return Math.sqrt(x * x + y * y);
    }

    // --- Operación pública adicional (ya introducida en el ejercicio 8) ---
    public double calcularDistanciaAPunto(Punto otro) {
        if (otro == null) {
            throw new IllegalArgumentException("El punto de destino no puede ser null");
        }
        double dx = this.coords[0] - otro.coords[0]; // acceso válido: misma clase
        double dy = this.coords[1] - otro.coords[1];
        return Math.sqrt(dx * dx + dy * dy);
    }

    // --- Miembros de clase públicos para consulta de máximos (sin cambios) ---
    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }

    // --- Lógica interna de actualización de máximos (oculta) --- 
    private static void actualizarMaximos(double x, double y) {
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    // --- (Opcional) Método factoría del ejercicio 14; no modifica la interfaz existente ---
    public static Punto crearRedondeado(double x, double y) {
        long rx = Math.round(x);
        long ry = Math.round(y);
        return new Punto(rx, ry);
    }
}
```

Este cambio de implementación demuestra la **ventaja práctica de la encapsulación**: el código cliente puede seguir invocando exactamente los mismos métodos y constructor, mientras que la clase es libre de evolucionar su representación interna. Si más adelante interesara almacenar las coordenadas en otra forma (por ejemplo, **módulo/ángulo**), bastaría con modificar la implementación y conservar las firmas públicas, manteniendo así la compatibilidad binaria y de uso.


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta
En Programación Orientada a Objetos **no es mejor declarar un atributo como público**, incluso aunque exista un *getter* y un *setter*. La razón es que un atributo público expone directamente la representación interna del objeto, sin ningún tipo de control ni punto de validación. Al usar *getters* y *setters* públicos, el acceso sigue siendo posible, pero **pasa por un método**, lo que permite en cualquier momento introducir validaciones, comprobaciones, logs o cambios en la implementación sin afectar al código externo. Esto mantiene la clase protegida frente a usos incorrectos, algo que no sería posible si el atributo fuese público.

La convención más habitual (y prácticamente universal) es declarar **todos los atributos como `private`** y exponer solo los métodos necesarios mediante una interfaz pública bien diseñada. Incluso en casos donde inicialmente un atributo no necesita validación, mantenerlo privado evita comprometer el futuro: si más adelante se necesita una comprobación en el *setter*, o computar un valor derivado en el *getter*, la interfaz pública no cambia. Esta práctica también permite modificar la representación interna sin alterar la API externa, como se vio en el ejercicio donde los `double` se sustituyeron por un array.

Esto tiene relación directa con las **invariantes de clase**. Una invariante es una condición lógica que el objeto debe cumplir siempre para estar en un estado válido, y permitir acceso directo a los atributos haría imposible garantizarla. Si los campos son públicos, cualquier parte del programa podría romper la invariante asignando valores inconsistentes; en cambio, si son privados, los métodos *setter* pueden verificar y preservar dichas condiciones cada vez que se modifica el estado. Por tanto, el uso de atributos privados no es una formalidad estilística, sino un mecanismo fundamental para asegurar la correcta encapsulación, la coherencia interna del objeto y el cumplimiento de sus invariantes.
En Programación Orientada a Objetos **no es mejor declarar un atributo como público**, incluso aunque exista un *getter* y un *setter*. La razón es que un atributo público expone directamente la representación interna del objeto, sin ningún tipo de control ni punto de validación. Al usar *getters* y *setters* públicos, el acceso sigue siendo posible, pero **pasa por un método**, lo que permite en cualquier momento introducir validaciones, comprobaciones, logs o cambios en la implementación sin afectar al código externo. Esto mantiene la clase protegida frente a usos incorrectos, algo que no sería posible si el atributo fuese público.

La convención más habitual (y prácticamente universal) es declarar **todos los atributos como `private`** y exponer solo los métodos necesarios mediante una interfaz pública bien diseñada. Incluso en casos donde inicialmente un atributo no necesita validación, mantenerlo privado evita comprometer el futuro: si más adelante se necesita una comprobación en el *setter*, o computar un valor derivado en el *getter*, la interfaz pública no cambia. Esta práctica también permite modificar la representación interna sin alterar la API externa, como se vio en el ejercicio donde los `double` se sustituyeron por un array.

Esto tiene relación directa con las **invariantes de clase**. Una invariante es una condición lógica que el objeto debe cumplir siempre para estar en un estado válido, y permitir acceso directo a los atributos haría imposible garantizarla. Si los campos son públicos, cualquier parte del programa podría romper la invariante asignando valores inconsistentes; en cambio, si son privados, los métodos *setter* pueden verificar y preservar dichas condiciones cada vez que se modifica el estado. Por tanto, el uso de atributos privados no es una formalidad estilística, sino un mecanismo fundamental para asegurar la correcta encapsulación, la coherencia interna del objeto y el cumplimiento de sus invariantes.


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta
Una clase se considera **inmutable** cuando, una vez creada una instancia, su estado interno **no puede cambiar bajo ninguna circunstancia**. Esto significa que todos sus atributos deben inicializarse en el constructor y permanecer constantes durante toda la vida del objeto. Para lograrlo, los campos suelen declararse `private` y `final`, y la clase no expone métodos que modifiquen esos valores. El objetivo es que cada objeto represente un valor fijo e inalterable, como ocurre con tipos como `String` en Java.

Un **método modificador** es cualquier método que *cambia el estado interno* del objeto. Esto incluye a los *setters*, pero también a otros métodos que, aunque no se llamen `setAlgo`, alteran las variables internas; por ejemplo, un método `mover(double dx, double dy)` en un `Punto` sería un modificador. Por tanto, un método modificador **no es necesariamente** un setter: se considera modificador siempre que altere atributos, independientemente del nombre o forma.

La inmutabilidad aporta varias ventajas importantes. Al no poder cambiar el estado, los objetos son **intrínsecamente seguros frente a modificaciones incorrectas**, lo que simplifica la preservación de invariantes y evita muchos errores. Además, los objetos inmutables son **más fáciles de razonar**, pueden compartirse libremente sin riesgo y suelen ser automáticamente **thread-safe**, ya que no existe estado que sincronizar. Por estas razones, la inmutabilidad es muy apreciada para representar valores lógicos, matemáticos o de configuración, donde el cambio constante no tiene sentido o puede resultar peligroso.


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta
No es recomendable incluir métodos *setter* siempre ni como una convención automática. De hecho, en diseño orientado a objetos bien estructurado, añadir *setters* sin una necesidad real **puede debilitar la encapsulación**, porque expone la posibilidad de modificar cualquier atributo sin control y convierte la clase en una simple bolsa de datos. Si todos los atributos son libremente modificables, se pierde la capacidad de imponer reglas, validar cambios o mantener invariantes internas, haciendo que la clase sea menos robusta y más propensa a errores.

En lugar de incluir *setters* por defecto, es más apropiado **reflexionar sobre si realmente es necesario que un atributo pueda cambiar** después de la construcción del objeto. Si el cambio no forma parte del comportamiento previsto del objeto, lo correcto es no ofrecer un *setter*. Esta decisión no solo refuerza la encapsulación, sino que también simplifica el diseño y evita usos indebidos por parte del código cliente. Muchos atributos deberían ser inmutables después de inicializarse, incluso si la clase no es completamente inmutable.

Además, limitar los *setters* facilita preservar las **invariantes de clase**, ya que solo los métodos que realmente tengan sentido para modificar el estado podrán hacerlo, y siempre a través de lógica controlada. Por ejemplo, un objeto puede necesitar ajustarse mediante operaciones de alto nivel (como “mover” o “rotar”), pero no mediante asignaciones directas a sus atributos internos. Diseñar una interfaz pública centrada en operaciones válidas, en lugar de exponer *setters*, ayuda a mantener la coherencia del objeto y a comunicar mejor su intención de uso.


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta
La clase `String` en Java es **inmutable**. Esto significa que, una vez creada una cadena, su contenido no puede modificarse. Cualquier operación que “parezca” cambiar la cadena realmente genera un **nuevo objeto `String`** con el resultado, mientras que el objeto original permanece intacto. Esta decisión de diseño simplifica la seguridad y la coherencia del lenguaje, ya que evita modificaciones inesperadas del contenido al compartir referencias entre distintos puntos del programa.

Cuando se concatenan dos cadenas con el operador `+`, Java crea internamente **un nuevo objeto `String`** que contiene el resultado de la concatenación. Esta operación puede ser costosa si se repite muchas veces dentro de un bucle, porque cada concatenación implica crear un nuevo objeto y copiar el contenido de las cadenas previas. Por este motivo, concatenaciones repetidas con `String` se consideran ineficientes.

Si una operación requiere construir progresivamente una cadena muy larga o realizar muchas concatenaciones en bucles, se recomienda usar **`StringBuilder`** (o `StringBuffer` si se necesita sincronización). `StringBuilder` es **mutable**, por lo que permite añadir texto sin crear objetos adicionales en cada paso. Tras terminar la construcción, puede obtenerse la cadena final mediante `toString()`. Este enfoque reduce considerablemente el coste de tiempo y memoria, especialmente en operaciones intensivas de concatenación.


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta
En Programación Orientada a Objetos, los objetos pueden compararse por **identidad** o por **contenido**, según lo que el lenguaje o el diseño de la clase establezcan. La identidad se refiere a si dos referencias apuntan al **mismo objeto en memoria**; el contenido, en cambio, evalúa si los valores internos de ambos objetos son equivalentes aunque sean instancias distintas. En Java, el operador `==` compara siempre **identidad**, no contenido, por lo que dos objetos distintos aunque tengan los mismos valores no serán considerados iguales con `==`.

El método `equals` en Java es la herramienta estándar para comparar el **contenido lógico** de dos objetos. Todas las clases heredan de `Object` un método `equals`, pero su implementación por defecto solo compara identidad, es decir, actúa como `==`. Por tanto, para que dos objetos de una clase propia se consideren iguales “por contenido”, la clase debe **sobrescribir** `equals` y definir allí qué atributos deben compararse. Este patrón permite expresar igualdad lógica (por ejemplo, dos puntos con mismas coordenadas) sin que sean necesariamente la misma instancia.

En el caso concreto de las cadenas, **no debe usarse `==` para comparar su contenido**, porque solo comprobaría si ambas referencias apuntan al mismo objeto. Para evaluar si dos cadenas contienen las mismas letras en el mismo orden, debe usarse el método `equals`, es decir:

```java
if (cadena1.equals(cadena2)) { ... }
```

Este método ya está sobrescrito adecuadamente en `String`, de modo que compara carácter a carácter, representando igualdad de contenido.

Esta distinción entre identidad y contenido es fundamental en POO, ya que permite elegir entre comparar objetos como “valores” (igualdad lógica) o como “entidades únicas” (igualdad por identidad). La correcta implementación de `equals` y el uso de `==` solo para identidad evita errores comunes y garantiza que el comportamiento sea coherente con la intención del diseño.


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta
Las **clases *wrapper*** son clases que encapsulan un tipo primitivo dentro de un objeto, permitiendo tratar valores básicos (como números o booleanos) como instancias de una clase. En Java, por ejemplo, cada tipo primitivo tiene su clase envoltorio correspondiente (`int → Integer`, `double → Double`, `boolean → Boolean`, etc.). El objetivo es poder usar estos valores en contextos donde se requieren objetos, como colecciones genéricas (`ArrayList`, `HashMap`) o métodos que operan exclusivamente con referencias. Este proceso puede realizarse explícitamente creando un objeto wrapper, aunque Java también ofrece conversión automática.

En Java, este mecanismo se apoya en el *autoboxing* y *unboxing*, que convierten automáticamente un valor primitivo en su wrapper y viceversa. Por ejemplo, al insertar un `int` en un `ArrayList<Integer>`, la conversión a `Integer` ocurre de forma implícita. Aunque sea automático, sigue siendo útil entenderlo, ya que estas conversiones pueden generar objetos adicionales y, por tanto, tener impacto en el rendimiento. La creación explícita también es posible usando constructores antiguos (hoy desaconsejados) o métodos estáticos como `Integer.valueOf()`.

Las clases wrapper tienen varias ventajas: permiten almacenar tipos primitivos en estructuras de datos orientadas a objetos, ofrecen métodos adicionales (por ejemplo, para convertir cadenas a números o para comparar valores), y posibilitan trabajar con valores *null* para representar ausencia, algo que los tipos primitivos no permiten. Estas características facilitan una integración más flexible con APIs, bibliotecas y herramientas que operan mediante objetos en lugar de valores básicos.

No todos los lenguajes orientados a objetos tienen tipos primitivos ni necesitan wrappers. Lenguajes como **Python** o **Ruby** no distinguen entre tipos primitivos y objetos: todo es un objeto desde el inicio. En ellos, no se requiere un mecanismo de envoltorio porque números, cadenas y booleanos ya son instancias de clases. En cambio, lenguajes como **Java** o **C#** sí diferencian entre valores primitivos y objetos por razones de eficiencia, por lo que necesitan wrappers para proporcionar comportamiento orientado a objetos de forma unificada.


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta
Un **tipo de dato enumerado** en POO es un tipo cuyos valores posibles están **predefinidos y limitados** a un conjunto cerrado. Estos valores representan opciones fijas, como días de la semana, estados de un proceso o direcciones. La idea es modelar conceptos que no deben admitir valores arbitrarios, sino únicamente aquellos que forman parte del dominio lógico del programa. Con ello se evita el uso de constantes sueltas o números mágicos, favoreciendo la claridad y la seguridad del código.

En Java, un tipo enumerado (`enum`) es realmente una **clase especial**, aunque más restringida y diseñada específicamente para representar conjuntos finitos de valores. Cada elemento del `enum` es una instancia única y final de esa clase. Esto implica que los enumerados pueden tener métodos, atributos, constructores privados e incluso implementar interfaces. Por tanto, no son simples constantes, sino objetos con comportamiento si se necesita, aunque su aspecto básico siga siendo el de un conjunto cerrado de valores bien definidos.

Los enumerados en Java aportan ventajas importantes en términos de **encapsulación**. Al ser clases, pueden ocultar detalles internos, controlar su propio estado y exponer únicamente la interfaz necesaria. También impiden que se creen nuevos valores fuera de los definidos en el `enum`, lo que protege la integridad del dominio del problema y fortalece las invariantes. Además, pueden agrupar dentro de sí mismos toda la lógica relacionada, evitando que la responsabilidad quede dispersa por el código y permitiendo un diseño más limpio, coherente y seguro.


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
A continuación se presenta un **tipo enumerado** `Mes` que define las doce instancias del año y expone: `getDias()` (días del mes, con febrero fijado a 28 por simplicidad) y `getOrdinal()` (posición 1–12). Además, se incluyen los cuatro métodos estacionales que, dado el hemisferio (`true` = norte, `false` = sur), devuelven si el mes **contiene días** de **invierno**, **primavera**, **verano** u **otoño** (criterio meteorológico: trimestres completos).

```java
public enum Mes {
    ENERO(31, 1),
    FEBRERO(28, 2),
    MARZO(31, 3),
    ABRIL(30, 4),
    MAYO(31, 5),
    JUNIO(30, 6),
    JULIO(31, 7),
    AGOSTO(31, 8),
    SEPTIEMBRE(30, 9),
    OCTUBRE(31, 10),
    NOVIEMBRE(30, 11),
    DICIEMBRE(31, 12);

    // Atributos privados (encapsulación)
    private final int diasBase;
    private final int numeroEnAnio; // 1..12

    // Constructor privado del enum
    Mes(int diasBase, int numeroEnAnio) {
        this.diasBase = diasBase;
        this.numeroEnAnio = numeroEnAnio;
    }

    // Interfaz pública
    public int getDias() {
        // Nota: febrero se fija a 28 por simplicidad; si se quisiera contemplar años bisiestos,
        // podría añadirse una sobrecarga getDias(boolean esBisiesto).
        return diasBase;
    }

    public int getOrdinal() {
        return numeroEnAnio;
    }

    public boolean esDeInvierno(boolean enHemisferioNorte) {
        return enHemisferioNorte
                ? (this == DICIEMBRE || this == ENERO || this == FEBRERO)
                : (this == JUNIO || this == JULIO || this == AGOSTO);
    }

    public boolean esDePrimavera(boolean enHemisferioNorte) {
        return enHemisferioNorte
                ? (this == MARZO || this == ABRIL || this == MAYO)
                : (this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE);
    }

    public boolean esDeVerano(boolean enHemisferioNorte) {
        return enHemisferioNorte
                ? (this == JUNIO || this == JULIO || this == AGOSTO)
                : (this == DICIEMBRE || this == ENERO || this == FEBRERO);
    }

    public boolean esDeOtoño(boolean enHemisferioNorte) {
        return enHemisferioNorte
                ? (this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE)
                : (this == MARZO || this == ABRIL || this == MAYO);
    }
}
```

Este diseño utiliza **atributos privados** y un **constructor privado** propio de los `enum` para encapsular la información de cada mes. El método `getOrdinal()` devuelve el número de mes **1–12** sin depender de `ordinal()` del propio `Enum`, lo que evita confusiones con el índice base 0. En cuanto a `getDias()`, se adopta el valor **28** para **febrero** por simplicidad; si se necesitara precisión en años bisiestos, resultaría natural añadir una variante `getDias(boolean esBisiesto)` que devolviera **29** cuando corresponda.

La lógica de estaciones sigue el criterio **meteorológico** por trimestres completos y considera el parámetro del hemisferio para **invertir** correctamente las estaciones. Al encapsular este comportamiento **dentro** del `enum`, se obtiene una interfaz clara y se impide la creación de valores fuera del conjunto definido, reforzando la **encapsulación** y las **invariantes** del dominio (solo existen 12 meses válidos y cada uno conoce sus propiedades).
