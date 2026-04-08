<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta
### Respuesta

En orientación a objetos, la **herencia** es un mecanismo mediante el cual una clase nueva se define a partir de otra ya existente, estableciendo una relación del tipo **“A es-un B”**. Esto significa que si una clase `Artillero` hereda de `Soldado`, se puede afirmar que *un artillero es un soldado*. Esta relación no es solo conceptual, sino que tiene consecuencias directas en cómo el lenguaje trata a los objetos en tiempo de compilación y ejecución, y permite modelar jerarquías claras entre tipos relacionados.

La primera implicación principal de esta relación es la **compatibilidad de tipos**. En Java, cualquier objeto de una subclase es compatible con el tipo de su superclase, lo que permite tratar instancias de distintas clases derivadas como si fueran objetos del tipo base común. Gracias a esto, se pueden almacenar objetos `Artillero` o `Zapador` en estructuras declaradas como `Soldado`, y utilizar sobre ellos los métodos definidos en la clase base sin conocer su tipo concreto. Esta cualidad resulta especialmente útil para trabajar con colecciones heterogéneas de objetos relacionados.

La segunda implicación es la **herencia de estado y comportamiento**. Las subclases heredan automáticamente los atributos y métodos no privados de la superclase, evitando duplicación de código y garantizando un comportamiento común. En este ejemplo, tanto `Artillero` como `Zapador` heredan el atributo privado `nombre` y el método `saludar()`, reutilizando esa funcionalidad sin necesidad de reescribirla. Cada subclase, además, puede añadir su propio estado y comportamiento específico, como el número de cohetes o de minas, accesibles mediante *getters* propios.

```java
// Clase base
public class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy el soldado " + nombre);
    }
}

// Subclase Artillero
public class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }
}

// Subclase Zapador
public class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }
}

// Uso de la compatibilidad de tipos
public class Main {
    public static void main(String[] args) {
        Soldado[] escuadron = new Soldado[3];
        escuadron[0] = new Soldado("Carlos");
        escuadron[1] = new Artillero("Ana", 5);
        escuadron[2] = new Zapador("Luis", 3);

        for (Soldado s : escuadron) {
            s.saludar();
        }
    }
}
```



## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta
### Respuesta

Al crear un objeto de una clase concreta que hereda de otra (por ejemplo, un `Artillero` o un `Zapador`), **siempre se ejecutan varios constructores**, uno por cada clase de la jerarquía, comenzando por la **clase base** y continuando hacia abajo hasta la clase más derivada. Es decir, aunque se escriba `new Artillero(...)`, primero se ejecuta el constructor de `Soldado` y después el constructor de `Artillero`. Este orden es obligatorio y garantiza que la parte del objeto correspondiente a la clase base quede correctamente inicializada antes de inicializar la parte específica de la subclase.

La palabra clave `super` dentro de un constructor se utiliza para **invocar explícitamente un constructor de la clase base**. Permite pasar parámetros necesarios para inicializar atributos heredados que pertenecen a la superclase. Conceptualmente, `super(...)` indica que parte de la responsabilidad de la construcción del objeto se delega en la clase padre. En Java, esta llamada debe ser siempre la **primera instrucción del constructor**, ya que la inicialización de la clase base no puede depender de una inicialización posterior de la subclase.

Si la clase base **no tiene visible un constructor sin parámetros**, entonces **es obligatorio llamar explícitamente a `super(...)`** con los argumentos adecuados. En caso contrario, el código no compilará, porque Java inserta implícitamente una llamada a `super()` solo cuando existe un constructor vacío accesible. Esto obliga a que el diseñador de la subclase sea consciente de cómo se inicializa la clase base y refuerza la idea de que una subclase no puede ignorar el proceso de construcción de su superclase.


## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta
### Respuesta

Los **atributos privados de una superclase sí forman parte de la instancia en memoria de una subclase**. Cuando se crea un objeto de una subclase, ese objeto contiene **toda la estructura del objeto de la clase base**, junto con los atributos definidos en la propia subclase. En otras palabras, un `Artillero` no es un objeto distinto que “apunta” a un `Soldado`, sino un único objeto que incluye internamente la parte correspondiente a `Soldado`, con todos sus atributos, incluidos los privados.

Sin embargo, que un atributo exista físicamente en memoria **no implica que sea accesible desde el código de la subclase**. En Java, el modificador `private` restringe el acceso **únicamente a la propia clase donde se declara**, incluso aunque una subclase herede de ella. Por tanto, aunque `nombre` forme parte del objeto `Artillero`, no puede usarse directamente desde su código. Esta separación refuerza la encapsulación y obliga a que el acceso al estado interno de la superclase se realice a través de métodos públicos o protegidos definidos por ella.

En el ejemplo de `Soldado`, la subclase `Artillero` hereda el atributo `nombre`, pero no puede referirse a él directamente. En su lugar, debe utilizar métodos de acceso proporcionados por la superclase, como un *getter*. Esto permite a la clase base mantener el control sobre su estado interno, incluso cuando ese estado forma parte de objetos de subclases.

```java
public class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

public class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
        // nombre = "Juan";       // ❌ Error: nombre no es accesible
        System.out.println(getNombre()); // ✅ Acceso correcto mediante método
    }

    public int getCohetes() {
        return cohetes;
    }
}
```

Este comportamiento muestra que la herencia implica **presencia en memoria**, pero no **visibilidad en el código**, y que ambas ideas deben distinguirse claramente al trabajar con jerarquías de clases en Java.


## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta
### Respuesta

La **compatibilidad de tipos** entre una superclase y sus subclases tiene una implicación directa en la **extensibilidad del código**. Gracias a esta propiedad, el código que trabaja con objetos del tipo base no necesita modificarse cuando se añaden nuevas subclases. Mientras el nuevo tipo cumpla la relación “es-un” con la superclase (`es un Soldado`), puede integrarse automáticamente en los mismos mecanismos ya existentes. Esto permite extender el sistema añadiendo funcionalidad sin romper ni reescribir código previo, uno de los objetivos principales del diseño orientado a objetos.

Desde el punto de vista práctico, esto significa que el comportamiento común definido en la clase base puede aplicarse a **nuevos tipos futuros** sin conocerlos de antemano. El código que recorre un array de `Soldado` y llama al método `saludar()` no necesita saber si delante tiene un `Artillero`, un `Zapador` o cualquier otra clase que se añada más adelante. Esta independencia reduce el acoplamiento y hace que el sistema sea más robusto frente a cambios y evoluciones.

Para ilustrarlo, se puede añadir un nuevo tipo de soldado, por ejemplo un `Medico`, que también hereda de `Soldado` y añade su propio estado específico. El código encargado de pedir el saludo a todos los soldados permanece exactamente igual, demostrando que la extensibilidad se logra añadiendo clases nuevas, no modificando las existentes.

```java
// Nueva subclase
public class Medico extends Soldado {
    private int botiquines;

    public Medico(String nombre, int botiquines) {
        super(nombre);
        this.botiquines = botiquines;
    }

    public int getBotiquines() {
        return botiquines;
    }
}

// Código existente (no se modifica)
public class Main {
    public static void main(String[] args) {
        Soldado[] escuadron = new Soldado[4];
        escuadron[0] = new Soldado("Carlos");
        escuadron[1] = new Artillero("Ana", 5);
        escuadron[2] = new Zapador("Luis", 3);
        escuadron[3] = new Medico("Marta", 2);

        for (Soldado s : escuadron) {
            s.saludar(); // Funciona para todos sin cambios
        }
    }
}
```

Este ejemplo muestra que la compatibilidad de tipos permite **extender el programa de forma segura y controlada**, añadiendo nuevos comportamientos especializados sin afectar al código que ya funciona correctamente.


## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta
### Respuesta

En Java es perfectamente posible que una **referencia del supertipo apunte a un objeto real de un subtipo**. Esto ocurre de forma habitual cuando se aprovecha la compatibilidad de tipos derivada de la herencia: una variable de tipo `Soldado` puede referirse a un objeto que en realidad es un `Artillero` o un `Zapador`. Sin embargo, a través de esa referencia solo pueden invocarse los **métodos visibles definidos en el tipo de la referencia**, es decir, en `Soldado`, aunque el objeto real sea más específico. Los métodos públicos exclusivos del subtipo no son accesibles directamente desde una referencia del supertipo.

Este mecanismo se conoce como **upcasting**, y consiste en tratar un objeto de una subclase como si fuera de su superclase. El upcasting es **implícito y seguro**, ya que siempre se cumple que un subtipo “es un” supertipo. Por el contrario, el **downcasting** consiste en convertir una referencia del supertipo a una referencia de un subtipo más concreto. Este proceso es **explícito** y potencialmente peligroso, ya que solo es válido si el objeto real es realmente de ese subtipo; de lo contrario, se produce una excepción en tiempo de ejecución.

Para comprobar de forma segura el tipo real del objeto al que apunta una referencia, Java proporciona el operador **`instanceof`**. Este operador permite preguntar en tiempo de ejecución si un objeto pertenece a una clase concreta o a alguna de sus subclases. Combinando `instanceof` con el downcasting, es posible acceder de manera controlada a métodos específicos de un subtipo, manteniendo al mismo tiempo la flexibilidad de trabajar con referencias del tipo base.

```java
Soldado[] escuadron = new Soldado[3];
escuadron[0] = new Soldado("Carlos");
escuadron[1] = new Artillero("Ana", 5);
escuadron[2] = new Zapador("Luis", 3);

for (Soldado s : escuadron) {
    s.saludar(); // Método del supertipo

    if (s instanceof Artillero) {
        Artillero a = (Artillero) s; // Downcasting seguro
        System.out.println("Cohetes: " + a.getCohetes());
    }
}
```

Este ejemplo muestra cómo el uso de referencias al supertipo permite recorrer colecciones heterogéneas, mientras que `instanceof` y el casting explícito hacen posible acceder, cuando es necesario, a la funcionalidad específica de cada subtipo sin perder seguridad.



## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta


## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta
