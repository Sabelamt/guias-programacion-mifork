<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### Respuesta
El **polimorfismo** es un concepto de la programación orientada a objetos que permite que un mismo mensaje (por ejemplo, una llamada a un método) produzca comportamientos distintos dependiendo del tipo real del objeto que lo recibe. En Java, esto ocurre cuando una variable de un tipo padre (clase base) referencia objetos de diferentes clases hijas, y al invocar un método se ejecuta la versión correspondiente al objeto concreto. Su utilidad principal es permitir escribir código más flexible y reutilizable, evitando depender de tipos concretos y reduciendo la necesidad de estructuras condicionales como `if` o `switch`.

Desde un punto de vista práctico, el polimorfismo se apoya directamente en la **herencia**. Una clase base define un comportamiento general, y las clases derivadas pueden proporcionar implementaciones específicas de dicho comportamiento. De esta forma, se puede trabajar con objetos distintos a través de una misma referencia común, lo que facilita la extensión del programa sin necesidad de modificar el código ya existente. Esto supone una mejora respecto al estilo estructurado de C/C++, donde el comportamiento suele decidirse explícitamente en tiempo de ejecución mediante condiciones.

La **sobreescritura de métodos** consiste en que una clase hija proporcione su propia implementación de un método que ya existe en la clase padre, manteniendo exactamente la misma firma (nombre, parámetros y tipo de retorno). Cuando un método está sobreescrito y se invoca a través de una referencia del tipo padre, Java decide en tiempo de ejecución qué versión ejecutar según el tipo real del objeto. Este mecanismo es la base del polimorfismo dinámico y permite que cada clase hija adapte el comportamiento heredado a sus necesidades sin romper la estructura común.

```java
class Animal {
    void hacerSonido() {
        System.out.println("El animal hace un sonido");
    }
}

class Perro extends Animal {
    @Override
    void hacerSonido() {
        System.out.println("El perro ladra");
    }
}

Animal a = new Perro();
a.hacerSonido(); // Se ejecuta el método de Perro
```

En este ejemplo, aunque la variable es de tipo `Animal`, el método que se ejecuta es el de `Perro`, lo que ilustra tanto la sobreescritura como el funcionamiento del polimorfismo.


## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta
La **ligadura dinámica** o **enlace tardío** consiste en que la decisión de qué versión concreta de un método se ejecuta se pospone hasta **tiempo de ejecución**, en lugar de resolverse en tiempo de compilación. Es decir, el programa no decide qué método llamar basándose solo en el tipo de la variable, sino en el **tipo real del objeto** al que dicha variable hace referencia en ese momento. Este mecanismo es especialmente relevante en programación orientada a objetos, ya que permite que el comportamiento de un programa se ajuste dinámicamente según los objetos utilizados.

La relación con el **polimorfismo** es directa y fundamental. El polimorfismo dinámico se apoya precisamente en la ligadura dinámica para funcionar: cuando se llama a un método sobre una referencia de una clase base, Java (u otros lenguajes orientados a objetos) determina en tiempo de ejecución cuál es la implementación correcta según la clase concreta del objeto. Sin ligadura dinámica, la sobreescritura de métodos no tendría el efecto deseado, ya que siempre se ejecutaría la versión conocida en compilación, anulando el comportamiento polimórfico.

En **C++**, la ligadura dinámica **no es el comportamiento por defecto**. Para que se produzca, es necesario declarar explícitamente los métodos como `virtual` en la clase base. Si no se hace, el enlace es estático y el método que se ejecuta depende únicamente del tipo del puntero o referencia. En cambio, en **Java** ocurre lo contrario: **la ligadura dinámica es el comportamiento por defecto** para los métodos de instancia, salvo en casos concretos como métodos `static`, `final` o `private`. Por tanto, en Java no es necesario indicar explícitamente que un método es virtual, lo que simplifica el uso del polimorfismo y reduce errores comunes.

En **Python**, la ligadura dinámica es aún más implícita y flexible. Al ser un lenguaje dinámicamente tipado, las llamadas a métodos se resuelven siempre en tiempo de ejecución, basándose únicamente en los métodos disponibles en el objeto en ese momento. No existen palabras clave equivalentes a `virtual`, ni restricciones de типado como en C++ o Java. Esto hace que el polimorfismo y la ligadura dinámica sean naturales y automáticos, aunque también exige mayor disciplina por parte del programador para evitar errores que solo se detectan al ejecutar el programa.


## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta
A continuación se muestra un ejemplo sencillo en Java que ilustra el polimorfismo mediante herencia y sobreescritura de métodos. Se define una clase base `Soldado` con un método `saludar`, que representa un comportamiento general común a todos los soldados. Esta clase actúa como tipo padre y permite trabajar de forma abstracta con distintos tipos de soldados sin conocer su clase concreta.

Se crean dos subclases: `Zapador` y `Artillero`. La clase `Zapador` **sobreescribe completamente** el método `saludar`, proporcionando un comportamiento específico distinto al definido en `Soldado`. En cambio, `Artillero` no redefine el método, por lo que hereda directamente el comportamiento de la clase base. Esto permite observar cómo, dependiendo del tipo real del objeto, se ejecuta una implementación distinta del mismo método.

El polimorfismo se ilustra creando un array de referencias de tipo `Soldado` que contiene objetos de distintas subclases. Al recorrer el array y llamar al método `saludar` usando referencias de tipo `Soldado`, Java decide en tiempo de ejecución qué versión del método ejecutar. Este mecanismo evita tener que comprobar el tipo concreto del objeto y demuestra la utilidad práctica del polimorfismo para escribir código flexible y extensible.

```java
class Soldado {
    void saludar() {
        System.out.println("El soldado saluda de forma reglamentaria.");
    }
}

class Zapador extends Soldado {
    @Override
    void saludar() {
        System.out.println("El zapador saluda mientras prepara explosivos.");
    }
}

class Artillero extends Soldado {
    // No sobreescribe saludar()
}

public class Main {
    public static void main(String[] args) {
        Soldado[] soldados = new Soldado[2];
        soldados[0] = new Zapador();
        soldados[1] = new Artillero();

        for (Soldado s : soldados) {
            s.saludar();
        }
    }
}
```

En este ejemplo, aunque todas las llamadas al método se realizan sobre referencias de tipo `Soldado`, el comportamiento varía según la clase real del objeto, demostrando claramente el funcionamiento del polimorfismo.


## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta
Sí, **al sobreescribir un método es posible invocar el método de la clase base** y trabajar a partir de su comportamiento. Esto permite reutilizar la funcionalidad ya definida en la clase padre y ampliarla o modificarla parcialmente en la subclase, en lugar de reemplazarla por completo. Esta técnica es habitual cuando el comportamiento general sigue siendo válido, pero se desea añadir una especialización concreta.

En Java, esta invocación se realiza mediante la **palabra clave `super`**, que permite acceder directamente a los métodos (y atributos) de la clase padre. Al llamar a `super.saludar()`, se ejecuta exactamente el método definido en `Soldado`, independientemente de que haya sido sobreescrito en la subclase. De esta forma, el método de la subclase puede ejecutar primero la lógica base y después añadir nuevas acciones.

Aplicando esto al caso del `Zapador`, el método `saludar` no sustituye completamente el comportamiento original, sino que lo extiende. Primero se muestra el saludo estándar del soldado y, a continuación, se añade un mensaje específico del zapador. Este enfoque mantiene la coherencia del comportamiento general y aprovecha la herencia para evitar duplicación de código.

```java
class Soldado {
    void saludar() {
        System.out.println("El soldado saluda de forma reglamentaria.");
    }
}

class Zapador extends Soldado {
    @Override
    void saludar() {
        super.saludar(); // Llamada al método de la clase base
        System.out.println("ZAPADOR A SUS ORDENES");
    }
}
```

La palabra clave utilizada para invocar el método de la clase base es **`super`**, y su uso es fundamental cuando se desea construir un comportamiento especializado a partir de uno ya existente, en lugar de reemplazarlo completamente.


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta
Al **sobreescribir un método en Java**, existen restricciones claras sobre su firma. Los **parámetros deben ser exactamente los mismos** en número, orden y tipo que en el método de la clase base; no se permite variarlos. En cuanto al **tipo de retorno**, debe ser el mismo o un tipo más específico (covariante), es decir, una subclase del tipo de retorno original. Además, no se pueden reducir las **visibilidades**: un método `public` no puede pasar a `protected` o `private`, ya que esto rompería el contrato establecido por la clase padre.

Es importante diferenciar entre **sobreescritura (overriding)** y **sobrecarga (overloading)**, ya que son conceptos distintos aunque similares en apariencia. En la sobreescritura, un método de una subclase reemplaza a otro de la clase base manteniendo la misma firma, y su resolución se realiza en **tiempo de ejecución**, estando ligada al polimorfismo. En cambio, la sobrecarga consiste en definir varios métodos con el mismo nombre pero **distintos parámetros** dentro de una misma clase (o jerarquía), y su resolución se realiza en **tiempo de compilación**, sin relación directa con el polimorfismo.

La anotación **`@Override`** se utiliza para indicar explícitamente que un método está destinado a sobreescribir otro definido en la clase base. Su función principal no es cambiar el comportamiento del programa, sino ayudar al compilador a **detectar errores**. Si por equivocación la firma no coincide (por ejemplo, un parámetro mal escrito), el compilador generará un error en lugar de crear un método nuevo sin sobreescribir el original.

Por este motivo, resulta **altamente recomendable usar siempre `@Override`** al sobreescribir métodos. Mejora la legibilidad del código, deja clara la intención del programador y evita errores sutiles que pueden ser difíciles de detectar, especialmente en jerarquías de herencia más grandes o al modificar código existente.


## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta
Sí, **al estudiar Java se emplea el polimorfismo desde etapas muy tempranas**, aunque al principio no siempre se es consciente de ello. Java está diseñado como un lenguaje totalmente orientado a objetos, y muchas de sus clases fundamentales (como `Object`) están pensadas para ser extendidas y personalizadas mediante sobreescritura de métodos. Esto hace que el polimorfismo no sea un concepto aislado o avanzado, sino una característica integrada en el uso cotidiano del lenguaje.

Al **sobreescribir métodos como `toString` o `equals`**, ya se está utilizando polimorfismo. Estos métodos están definidos en la clase base `Object`, y cuando se redefinen en una clase propia, Java decide en tiempo de ejecución qué versión ejecutar según el tipo real del objeto. Por ejemplo, cuando se imprime un objeto con `System.out.println`, internamente se invoca `toString`, y se ejecuta la versión sobreescrita si existe. Esa selección dinámica del método es precisamente polimorfismo.

Aunque en estos casos no siempre se usan explícitamente referencias de tipo `Object`, el mecanismo subyacente es el mismo que en ejemplos más “clásicos” de polimorfismo con jerarquías de clases. El objeto se maneja de forma genérica, pero responde con un comportamiento específico definido en su clase concreta. Por este motivo, se puede afirmar que el polimorfismo forma parte del uso normal de Java desde los primeros programas orientados a objetos.

En conclusión, **sí, se usa polimorfismo desde el principio en Java**, incluso antes de estudiarlo formalmente. La sobreescritura de métodos básicos como `toString` y `equals` no solo personaliza el comportamiento de los objetos, sino que aprovecha directamente el enlace dinámico del lenguaje, haciendo del polimorfismo una herramienta fundamental y cotidiana en Java.


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta


## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta
