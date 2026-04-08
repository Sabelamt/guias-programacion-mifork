<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### Respuesta

A composición en C consiste en construír estruturas máis complexas combinando estruturas máis simples. Neste caso, resulta natural representar un punto mediante dúas coordenadas, e unha liña mediante dous puntos. A idea clave é que a estrutura máis grande **contén** internamente outras estruturas, reflectindo a relación “unha liña ten-dous puntos”. Isto permite organizar mellor os datos e facilita a creación de funcións que operen sobre eles.

Para calcular a distancia entre dous puntos, adoita empregarse a fórmula da distancia euclidiana, baseada na raíz cadrada da suma dos cadrados das diferenzas en cada coordenada. De maneira similar, a lonxitude dunha liña defínese simplemente como a distancia entre os dous puntos que a compoñen. Desta forma, a función que calcula a lonxitude dunha liña pode reutilizar directamente a función da distancia entre puntos, o que exemplifica un deseño modular e limpo.

A continuación móstrase un exemplo completo en C empregando `struct` e funcións separadas:

```c
#include <stdio.h>
#include <math.h>

struct Punto {
    double x;
    double y;
};

struct Linea {
    struct Punto p1;
    struct Punto p2;
};

double distancia(struct Punto a, struct Punto b) {
    double dx = b.x - a.x;
    double dy = b.y - a.y;
    return sqrt(dx*dx + dy*dy);
}

double longitudLinea(struct Linea l) {
    return distancia(l.p1, l.p2);
}

int main() {
    struct Punto A = {0.0, 0.0};
    struct Punto B = {3.0, 4.0};
    struct Linea L = {A, B};

    printf("Distancia entre puntos: %.2f\n", distancia(A, B));
    printf("Longitud de la línea: %.2f\n", longitudLinea(L));

    return 0;
}
```

Este exemplo mostra claramente como a composición facilita que estruturas pequenas se integren noutras máis complexas, mantendo un código organizado e reutilizable. Ademais, a separación en funcións fai que cada operación estea ben definida e se poida empregar en distintos contextos do programa.




## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta

Na orientación a obxectos, a composición exprésase facendo que unha clase conteña obxectos doutra clase como parte do seu estado interno. Neste exemplo, resulta natural que unha liña estea composta por dous puntos, igual que no exemplo en C, pero agora introdúcense características propias de Java como a encapsulación e a inmutabilidade. Isto permite garantir que nin os atributos dun punto nin os dunha liña poidan modificarse unha vez creados, mellorando a seguridade e coherencia dos datos respecto á versión en C.

Para conseguir a inmutabilidade en Java, defínense os atributos como `private final` e non se ofrecen métodos que permitan modificalos. Isto asegura que, unha vez construído un `Punto`, as súas coordenadas non cambien, e do mesmo xeito unha `Linea` non pode alterar os puntos que a compoñen. A composición queda así reforzada pola ocultación da información: só se expón o imprescindible e no formato adecuado, mentres que os detalles de implementación permanecen protexidos. Ademais, a función de distancia entre puntos convértese agora nun método pertencente á propia clase `Punto`, facendo o código máis natural e orientado aos datos que manipula.

A continuación preséntase un exemplo funcional de composición en Java con obxectos inmutables:

```java
public final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double getX() { return x; }
    public double getY() { return y; }

    public double distanciaA(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx*dx + dy*dy);
    }
}

public final class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public Punto getP1() { return p1; }
    public Punto getP2() { return p2; }

    public double longitud() {
        return p1.distanciaA(p2);
    }
}
```

Este código exemplifica claramente como a orientación a obxectos permite organizar mellor os datos e reforzar a idea de composición. A inmutabilidade garante que os obxectos permanezan consistentes ao longo da execución, evitando erros comúns derivados de modificacións inesperadas. Ademais, a responsabilidade de cada clase queda ben dividida, e a reutilización do método `distanciaA` fai que a clase `Linea` resulte máis simple e coherente.





## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta

A **multiplicidad** en composición expresa cantos obxectos dun tipo forman parte doutro obxecto, indicando a relación estrutural entre eles. Úsase para describir se un obxecto contén un único elemento, varios ou ningún, e tamén para indicar se a relación é de obrigatoriedade ou de opción. Nas relacións de composición, estas cantidades adoitan ser fixas e fortes, porque os obxectos contidos non existen de maneira independente ao obxecto que os compón.

No exemplo anterior, unha `Linea` está composta exactamente por **dous** puntos. Isto significa que desde o punto de vista de `Linea` cara a `Punto` a multiplicidade é **2**, normalmente expresada como **“2”** ou **“\[2]”**, pois non pode haber nin menos nin máis puntos formando unha liña no modelo definido. Esta relación expresa que a liña depende completamente deses dous puntos para existir, polo que a súa estrutura é estritamente fixa.

Na dirección contraria, desde `Punto` cara a `Linea`, a multiplicidade non está restrinxida polo modelo. Un punto pode formar parte de ningunha liña, dunha soa, ou potencialmente de moitas. Polo tanto, a multiplicidade exprésase como **0..**\*, indicando que un punto pode non pertencer a ningunha liña ou pode participar en varias. Neste caso, a composición aplícase no sentido de `Linea` → `Punto`, pero non ao revés, xa que o punto non depende da liña para existir.

En resumo:

*   **De `Linea` a `Punto`: multiplicidad 2**.
*   **De `Punto` a `Linea`: multiplicidad 0..**\*.

***
CLASE____:
Multiplicidad de A y B (p. ej entre linea y pnt)
Notacion UML
 ```mermaid
 graph TD
 a[Linea; con atributos p1,Punto y p2.Punto + los métodos];
 b[Punto; con atributos x double, y double]
 ```
 -1 linea se relaciona como min con 3 Puntos y como máx con 2 pntd
 -Un punto se relaciona cm min con 0 Lineas y cm max con muchas lineas



## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta


A diferenza entre **composición forte** e **composición débil** refírese ao grao de dependencia que existe entre os obxectos implicados. Na composición forte, o obxecto composto é totalmente dono dos obxectos que contén; forman parte esencial da súa estrutura e non teñen sentido nin existencia independente fóra del. Pola contra, na composición débil, aínda que existe unha relación de inclusión ou pertenza, os obxectos contidos manteñen unha existencia máis autónoma, podendo ser compartidos ou existir sen depender estritamente do obxecto que os agrupa.

En termos de ciclo de vida, a composición forte implica que, cando o obxecto contedor morre, tamén deben desaparecer todos os obxectos que forman parte del. Isto ocorre porque a súa creación e destrución están estritamente ligadas. Pola contra, na composición débil, a destrución do obxecto contedor non obriga á destrución dos obxectos contidos, que poden seguir existindo e incluso ser utilizados por outros obxectos ou estruturas diferentes.

Na nomenclatura habitual de orientación a obxectos, a composición débil coñécese como **“asociación”** ou **“agregación”**, xa que representa relacións máis frouxas e menos restritivas. Pola súa parte, a composición forte é a que se denomina **“composición”** propiamente dita, porque establece unha dependencia estrutural e vital máis marcada entre os obxectos implicados.

***
CLASE____:

FUERTE:El contenedor por ej Linea es el que crea los obj x ej Punto y y estos no vienen mas allá del contenedor. ->El ciclo de vida del contenido esta vinculado el contenedor

DÉBIL: El contenedor y contenido tienen ciclos de vida independientes p ej los obj Punto pueden vivir sin estar en obj Linea




## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta

Cando unha clase utiliza outra só como parámetro dun método, como valor de retorno, ou como variable local dentro dunha función, non se está a falar de composición, senón dunha **dependencia**. Neste tipo de relación, un obxecto simplemente necesita colaborar temporalmente con outro para realizar unha tarefa, pero non forma parte do seu estado interno nin do seu ciclo de vida. A relación é espontánea e limitada ao contexto da chamada ao método, polo que non implica posesión nin responsabilidade sobre o outro obxecto.

A dependencia é, polo tanto, unha relación moito máis débil ca a composición. O obxecto que recibe ou devolve outro non controla a súa creación nin a súa destrución; simplemente o utiliza para completar unha función momentánea. Mesmo no caso de que se cree un obxecto con `new` dentro dun método, a relación segue sendo de dependencia, xa que o obxecto creado non se conserva como parte do estado interno da clase, senón que adoita empregarse de maneira puntual e desaparece cando remata a execución do método e non quedan referencias a el.

En resumo, cando as clases colaboran unicamente a través de parámetros, retornos ou variables locais, a relación denomínase **dependencia**, non composición. A composición só existe cando unha clase **contén** e **posúe** obxectos doutra, formando parte permanente do seu estado interno e quedando ligados ao seu ciclo de vida.

***
CLASE____:
Ej de dependencia: Punto dspnd de String y de StringBuilder
```java
class Punto{

    public String toString(){
        StringBuilder sb0new StringBuilder();
    }
}
class OperadorFicheiro{
    public static String LeeFicheiro(Path p)
}
```


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta


Para representar a relación entre `Linea` e `Punto` como **composición forte**, a liña debe ser dona absoluta dos puntos. Isto implica que os puntos se crean *dentro* da clase `Linea` e non se reciben desde fóra. Deste modo, o ciclo de vida dos puntos está completamente ligado ao da liña: cando a liña deixa de existir, tamén o fan os puntos. Ademais, a clase non expón directamente estes puntos nin permite modificalos desde fóra, reforzando a idea de posesión exclusiva e inmutabilidade forte.

Pola contra, nun deseño de **composición débil**, a liña utiliza puntos creados externamente. Isto significa que os puntos existen antes de que a liña se cree e poden seguir existindo despois de destruíla. Neste caso non se fala de posesión, senón de agregación: a liña “ten” puntos, pero non controla o seu ciclo de vida. Os puntos poden incluso ser compartidos entre varias liñas, polo que a relación é moito máis flexible pero menos estrita.

A continuación móstranse as dúas aproximacións, mantendo a inmutabilidade para facilitar a comparación e seguindo un estilo similar ao das preguntas anteriores.

***

# ✔ Composición fuerte (creación interna dos puntos)

```java
public final class LineaFuerte {
    private final Punto p1;
    private final Punto p2;

    public LineaFuerte(double x1, double y1, double x2, double y2) {
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }

    public double longitud() {
        return p1.distanciaA(p2);
    }
}
```

Nesta versión, os puntos créanse *dentro* de `LineaFuerte`.  
A liña controla totalmente a existencia dos puntos → **composición forte**.

***

# ✔ Composición débil (puntos externos, non creados pola liña)

```java
public final class LineaDebil {
    private final Punto p1;
    private final Punto p2;

    public LineaDebil(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distanciaA(p2);
    }
}
```

Nesta versión, a liña recibe puntos xa existentes, podendo ser compartidos.  
A liña non controla o seu ciclo de vida → **composición débil** (agregación).
 ***
 CLASE:___
 ```java
class Linea{
    private Punto p1;
    private Punto p2;

    public Linea(double x1, double y1, double x2, double y2){

    }
}
 ```




## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta

En Java, incluso nunha composición forte, o contedor **non destrúe explicitamente** os obxectos que contén. Isto débese a que Java non segue o modelo de destrución manual de linguaxes como C++; no seu lugar, emprega un **sistema de garbage collection**. Este mecanismo libérase automaticamente da memoria cando detecta que un obxecto xa non é accesible desde ningún punto do programa. Así, cando unha instancia de `Linea` desaparece —por exemplo, cando deixa de haber referencias a ela— os `Punto` que contiña tamén deixan de ser accesibles, polo que o recolector de lixo os eliminará automaticamente.

Este comportamento explica por que non se observan chamadas a destrutores nin métodos equivalentes en Java. As clases non teñen destrutores como en C++, e os programadores non deben nin poden liberar obxectos manualmente. A composición forte exprésase só no **modelo conceptual e estrutural**: a liña crea e posúe os puntos, e estes só existen mentres a liña exista. Pero a responsabilidade real da destrución recae integramente no recolector de lixo, que se ocupa de eliminar os obxectos cando xa non teñen referencias vivas.

Por tanto, nunha composición forte en Java, a relación de ciclo de vida exprésase en termos de accesibilidade: cando o contedor deixa de ser accesible, os obxectos que compón tamén deixan de selo. Non fai falla destruír nada de forma explícita; o mecanismo de xestión automática da memoria garante a eliminación segura e consistente dos obxectos, reforzando a idea de que a composición forte se centra na propiedade lóxica e non nun control manual da destrución.

***
CLASE:___
En java la vide de Punto es inaccesible , en el ejemplo ocurre cuando Linea deja de serlo a su vez.
Por tanto, cuando Linea "es basura", tambien lo serán sus puntos y serán eliminados: de memmoria por el recoñector de basura



## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta

Nunha composición débil, o departamento non crea nin destrúe profesores: simplemente **os usa e os almacena**, pero estes existen de maneira independente e poden ser compartidos por outros contextos. A clave está en que o departamento **agrega** profesores, pero non controla o seu ciclo de vida; isto encaixa co modelo pedido, onde se empregan `Profesor[]` internamente, pero sen expor directamente o array. A interacción co exterior realízase mediante métodos controlados, mantendo a encapsulación e permitindo engadir e eliminar profesores sen violar a estrutura interna. Ademais, o departamento debe garantir que sempre exista un director e que este forme parte da lista de profesores.

Para manter a invariante da clase, é necesario comprobar dúas condicións fundamentais: que **sempre exista un director** desde a creación do departamento e que o director **forme parte da lista de profesores** en todo momento. Ao cambiar o director, debe verificarse que o novo profesor pertenza ao departamento; ao eliminar un profesor, hai que impedir eliminar ao director, ou en caso contrario lanzar unha excepción. Ademais, como o array é dun tamaño fixo (máximo 50), a operación de engadir incrementa o número de profesores e a eliminación debe desprazar cara atrás o resto dos elementos para manter a estrutura compacta.

A interface pública deixa claro que o número de profesores pode consultarse mediante un método específico, e que un profesor pode obterse pola súa posición. Non se expón o array completo, evitando romper a encapsulación. O resultado é unha composición débil ben estruturada, que protexe as invariantes da clase e garante a consistencia interna do departamento mediante excepcións adecuadas cando as operacións violan as restricións impostas polo modelo.

***

# ✔ Código completo (Composición débil con invariantes e encapsulación)

```java
public final class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre no puede ser vacío");
        }
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}
```

```java
public final class Departamento {
    private static final int MAX_PROFESORES = 50;

    private final Profesor[] profesores;
    private int numProfesores;

    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("Debe haber siempre un director inicial");
        }

        this.profesores = new Profesor[MAX_PROFESORES];
        this.profesores[0] = directorInicial;
        this.numProfesores = 1;
        this.director = directorInicial;
    }

    // --- Métodos de consulta ---
    public int getNumeroProfesores() {
        return numProfesores;
    }

    public Profesor getProfesor(int posicion) {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new IndexOutOfBoundsException("Posición inválida");
        }
        return profesores[posicion];
    }

    public Profesor getDirector() {
        return director;
    }

    // --- Métodos para modificar la lista ---
    public void añadirProfesor(Profesor p) {
        if (p == null) {
            throw new IllegalArgumentException("El profesor no puede ser nulo");
        }
        if (numProfesores >= MAX_PROFESORES) {
            throw new IllegalStateException("No caben más profesores");
        }
        profesores[numProfesores] = p;
        numProfesores++;
    }

    public void eliminarProfesor(int posicion) {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new IndexOutOfBoundsException("Posición inválida");
        }
        Profesor eliminado = profesores[posicion];

        if (eliminado == director) {
            throw new IllegalStateException("No se puede eliminar al director del departamento");
        }

        // Desplazar hacia atrás
        for (int i = posicion; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }

        profesores[numProfesores - 1] = null;
        numProfesores--;
    }

    // --- Cambio de director ---
    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El director no puede ser nulo");
        }

        // Comprobar que pertenece al departamento
        boolean esta = false;
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == nuevoDirector) {
                esta = true;
                break;
            }
        }

        if (!esta) {
            throw new IllegalStateException("El director debe ser profesor del departamento");
        }

        this.director = nuevoDirector;
    }
}
```

***
Clase:__
```java

antesd de nada clase profesor....

public class Departamento{
    //Composicion debil
    // 1 dDepartamento como min 0 y como mas muchos Profesores
    //1 profesor como min 0 y cm mas muchos departamentos

    private Profesor[] profesores = new Profesor [50];
    private int numProfesores= 0 ;

    //Composicion debil
    // 1 dDepartamento como min 1 y como max 1 Profesor director
    //1 profesor puede ser director como min de 0 y cm mas de muchos departamentos
    private Profesor director;
    Private Departamento(Profesor director){
        // o. si director null lanzamos IAE
        //1. añadimos el director a l conjunto de profesores
        //2.Establecemos ese profesor como director
    }

    public int GetNumProfesores(){
        return this.numProfesores;
    }

    public Profesor getProfesor(int pos){
        //0. validamos pos, y si no valida lanza IAE
        return this.profesores[pos];
    }

    public void addProfesor(Profesor p){
        //0. si no hay mas sitio,lanza AIOB
    }

    public void eliminarProfesor(int pos){
        //0.Si pos no esta en el rango correcto (0- numProfesores),
        //lanzar IAE
        //1. si el profesor en pos ES EL DIRECTOR , lanzar IAE
        //2. eliminar elemento pos array (bucle de copia del sig y establecer el null a la ultim pos usada)
    }

    public void cambiarDirector(Profesor nuevoDirector){
        //0.si el nv director null IAE
        //1si le nv directo no lo encuentro bucle de busqueda, lanzo IAE, diciendo q hay q metermlo en el depart primero
        //2
    }
    public getProfesor(){
        return this.profesor;
    }
    
}

```

Hay 2 composiciones débiles
No se expone el arraya al exterior(imposible garantizar invariate de clase)
En los medodos que gestionan el departamento se controla q no se viole la invariante de clase.


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta

O uso de `List<Profesor>` en lugar dun array primitivo simplifica bastante a implementación do departamento. As listas permiten engadir e eliminar elementos sen xestionar manualmente índices nin desprazamentos, e ademais non teñen un tamaño fixo como o array limitado a 50 posicións. Isto aforra boa parte do código dedicado ao control de límites, ao desprazamento ao eliminar un profesor e á comprobación de espazo dispoñible. A lóxica céntrase así nas invariantes da clase e na relación débil cos profesores, non en tarefas mecánicas de administración da estrutura interna.

Con respecto ao método `getProfesor(int pos)`, se no seu lugar existise un método que devolvese *todos* os profesores, non se debería devolver directamente a lista interna. Isto rompería a encapsulación porque quen recibise esa lista podería modificala libremente desde fóra, alterando o estado interno do departamento sen pasar polas comprobacións e restricións impostas polos métodos públicos. Para manter a seguridade da clase, resulta necesario devolver unha lista **copia** ou unha **lista non modificable**, evitando así que o chamador poida engadir ou eliminar profesores á marxe das regras impostas polo modelo.

Este enfoque permite manter a composición débil de forma limpa e segura, reservando para a clase a responsabilidade de garantir as invariantes: que sempre exista un director e que o director pertenza ao departamento. A lista externa que se devolve ao usuario é simplemente unha visión parcial e controlada da colección, preservando a integridade interna do obxecto.

***

# ✔ Versión con `List<Profesor>` (composición débil, invariantes, encapsulación)

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public final class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre no puede ser vacío");
        }
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}
```

```java
public final class Departamento {

    private final List<Profesor> profesores;
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("Debe haber siempre un director inicial");
        }

        this.profesores = new ArrayList<>();
        this.profesores.add(directorInicial);
        this.director = directorInicial;
    }

    // --- Consultas ---
    public int getNumeroProfesores() {
        return profesores.size();
    }

    public Profesor getProfesor(int pos) {
        return profesores.get(pos); // List ya valida los límites
    }

    public Profesor getDirector() {
        return director;
    }

    // --- Operaciones sobre la lista ---
    public void añadirProfesor(Profesor p) {
        if (p == null) {
            throw new IllegalArgumentException("El profesor no puede ser nulo");
        }
        profesores.add(p);
    }

    public void eliminarProfesor(int pos) {
        Profesor eliminado = profesores.get(pos);

        if (eliminado == director) {
            throw new IllegalStateException("No se puede eliminar al director del departamento");
        }

        profesores.remove(pos);
    }

    // --- Cambio de director ---
    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El director no puede ser nulo");
        }
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalStateException("El nuevo director debe ser profesor del departamento");
        }
        this.director = nuevoDirector;
    }

    // --- Devolver listaxe sen romper encapsulación ---
    public List<Profesor> getProfesores() {
        // Opción A: devolver unha copia
        // return new ArrayList<>(profesores);

        // Opción B: devolver lista non modificable
        return Collections.unmodifiableList(profesores);
    }
}
```

***

# ✔ Que parte do código se aforrou usando `List`?

*   Non hai que comprobar límites máximos nin tamaño do array.
*   Non hai que desprazar elementos ao eliminar.
*   Non hai que levar contadores de cantos profesores hai.
*   Non hai que asignar valores `null` manualmente ao final.
*   Non hai que xestionar espazo dispoñible para engadir profesores.

***

# ✔ Por que non devolver directamente a lista interna?

Porque permitiría ao usuario facer:

```java
dep.getProfesores().clear();   // elimina todo
dep.getProfesores().remove(0); // elimina ao director
```

Violando invariantes e deixando o obxecto nun estado ilegal.

***

# ✔ Como resolvelo?

Dúas solucións correctas:

1.  **Devolver unha copia**:
    ```java
    return new ArrayList<>(profesores);
    ```
2.  **Devolver unha lista non modificable**:
    ```java
    return Collections.unmodifiableList(profesores);
    ```

Ambas evitan que a lista interna sexa modificada desde fóra.

***
CLASE:__
para completar el 8

```java
en el codio¡go anterior, no afectamos  andie q nos estuviera utilizando
//ACCESO A LA LISTA INTERNA?? (cuidado invariantes de clase)
//Composicion debil 1 version List
private List<Profesor> profesores = new ArrayList<>();

public List <Profesor> getProfesores(){
    return this.profesores;
    problema -->La lista e smutable poerdemos control sobre invariante de clase

    //si insisto q me gusta interfaz pero quiero garantixar q solo se use para recorrerlos, no modificar puedo: 1 devolver una copia 2. devolver envoltorio no mutable
}
   
```

con list<Profesor>
no cambia interfaz publica
es mas facil implementar algn met delegando met lis
si devuelve hax copi para proteger



## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta

As **composicións recursivas** aparecen cando unha clase inclúe no seu interior unha referencia a outro obxecto da mesma clase. Neste tipo de deseño, a estrutura pode repetirse sobre si mesma formando cadeas ou árbores. No caso dunha `Persona`, é posible que unha persoa teña unha nai, e esa nai sexa tamén unha `Persona`, e así sucesivamente. Se se garante a inmutabilidade, a relación faise aínda máis segura, pois cada obxecto queda fixo unha vez construído e non poden alterarse nin a súa identidade nin a relación coa súa nai. Este tipo de composición é útil para representar elementos xerárquicos ou ligados de maneira natural nunha cadea.

A implementación require que a clase `Persona` teña un atributo privado e final que represente a nai, podendo ser nulo no caso de non existir esa información. O constructor establece toda a información de forma definitiva, e os métodos públicos permiten acceder á nai e ao nome. No `main` pode crearse unha pequena árbore familiar que mostre a orde xerárquica desde a avoa ata o neto, enlazando cada persoa coa súa nai e demostrando o carácter recursivo da composición.

Existen outros exemplos habituais de composición recursiva, como as estruturas de directorios dun sistema de ficheiros, onde un directorio pode conter outros directorios do mesmo tipo; as listas ligadas, onde cada nodo contén unha referencia ao seguinte nodo; as árbores binarias, onde cada nodo ten fillos que tamén son nós; ou os comentarios anidados en sistemas de foros. Todos estes casos comparten a mesma idea fundamental: unha clase que se compón de instancias da súa propia clase.

***

# ✔ Código: Composición recursiva con `Persona` inmutable

```java
public final class Persona {
    private final String nombre;
    private final Persona madre; // puede ser null

    public Persona(String nombre, Persona madre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre no puede estar vacío");
        }
        this.nombre = nombre;
        this.madre = madre; // null permitido
    }

    public String getNombre() {
        return nombre;
    }

    public Persona getMadre() {
        return madre;
    }
}
```

## ✔ Ejemplo de uso (main)

```java
public class Main {
    public static void main(String[] args) {
        Persona abuela = new Persona("Carmen", null);
        Persona madre = new Persona("Laura", abuela);
        Persona hijo  = new Persona("Diego", madre);

        System.out.println("Hijo: " + hijo.getNombre());
        System.out.println("Madre: " + hijo.getMadre().getNombre());
        System.out.println("Abuela: " + hijo.getMadre().getMadre().getNombre());
    }
}
```

Saída esperada:

    Hijo: Diego
    Madre: Laura
    Abuela: Carmen

***

# ✔ Outros exemplos típicos de composición recursiva

*   **Directorios e subdirectorios** (cada directorio contén outros directorios).
*   **Nodos de árbores** (cada nodo contén fillos que tamén son nodos).
*   **Listas ligadas** (cada nodo apunta ao seguinte nodo da mesma clase).
*   **Entradas de menús con submenús** (cada elemento pode conter máis elementos iguais).
*   **Comentarios con respostas anidadas** (cada comentario contén outros comentarios).

***



## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta

As relacións de **composición bidireccional** aparecen cando dous obxectos se compoñen mutuamente, é dicir, cando cada un mantén unha referencia ao outro. Nun deseño deste tipo, o obxecto A contén a B e, ao mesmo tempo, B coñece a A. Esta relación require un coidado especial para manter as **invariantes** e evitar estados incoherentes, xa que agora hai dúas referencias que deben estar sempre coordinadas. Ademais, en Java implica unha responsabilidade adicional: garantir que as dúas partes se constrúan de forma consistente e que non queden referencias circulares mal inicializadas.

No exemplo de `Profesor` e `Departamento`, implementar unha relación bidireccional suporía que o `Departamento` almacenase a todos os profesores e que, ao mesmo tempo, cada `Profesor` tivese unha referencia ao `Departamento` ao que pertence. Isto significa que, ao engadir un profesor ao departamento, debería actualizarse tamén o propio profesor para que garde o seu departamento. Do mesmo xeito, ao eliminar un profesor da lista, a referencia inversa debería eliminarse para manter a consistencia. A relación entre director e departamento tamén debería reflectirse na propia clase `Profesor`, permitíndolle saber que é o director, aínda que esa seria unha ampliación opcional.

Para implementalo correctamente, sería necesario modificar o constructor de `Profesor` para recibir inicialmente o `Departamento`, ou ben proporcionar un método interno que permita ao departamento asignarse como contedor do profesor no momento en que se engade. En ambos casos, o cambio non debería quedar exposto ao exterior para evitar romper a encapsulación. O punto crítico sería garantir que ningún profesor quede nun estado no que crea pertencer a un departamento do que xa non forma parte, e viceversa. A relación bidireccional, aínda que útil en moitos modelos, require manter esta coherencia interna de maneira estrita.

***


