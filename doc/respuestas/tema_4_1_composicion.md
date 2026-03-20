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
En el lenguaje C, la composición se logra anidando una estructura dentro de otra. Este enfoque permite agrupar datos relacionados para representar conceptos más complejos. En este caso, la estructura Linea se define mediante la inclusión de dos instancias de la estructura Punto, estableciendo una relación de "tiene-un" (o en este caso, "tiene-dos").

La lógica de cálculo se separa de los datos. Se definen funciones globales que reciben estas estructuras como argumentos para realizar las operaciones matemáticas necesarias. A continuación se muestra la implementación técnica de este esquema:

#include <math.h>

struct Punto {
    double x;
    double y;
};

struct Linea {
    struct Punto p1;
    struct Punto p2;
};

double calcularDistancia(struct Punto a, struct Punto b) {
    return sqrt(pow(b.x - a.x, 2) + pow(b.y - a.y, 2));
}

double calcularLongitud(struct Linea l) {
    // La línea compone sus puntos y los envía a la función de distancia
    return calcularDistancia(l.p1, l.p2);
}

Es importante observar que, en C, no existe una restricción nativa que impida modificar las coordenadas de los puntos dentro de una línea una vez creada, a menos que se utilicen punteros constantes o convenciones de programación estrictas. Los datos y las funciones que los manipulan están desacoplados, lo que requiere que el programador gestione manualmente la integridad de las estructuras.

## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta
En Java, la composición implica que una clase contiene referencias a objetos de otra clase como atributos privados. Para garantizar la inmutabilidad y la seguridad del estado, se utiliza el modificador final en los atributos y se prescinde de métodos "setter". De este modo, una vez que un objeto Punto o Linea es construido, sus datos no pueden ser alterados, superando la fragilidad de las estructuras simples de C.

La lógica de cálculo ahora reside dentro de las propias clases. El objeto Linea delega la responsabilidad del cálculo de distancia al objeto Punto, aprovechando que este último sabe cómo interactuar con otros de su misma especie.

public class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaA(Punto otro) {
        return Math.sqrt(Math.pow(otro.x - this.x, 2) + Math.pow(otro.y - this.y, 2));
    }
}

public class Linea {
    private final Punto inicio;
    private final Punto fin;

    public Linea(Punto inicio, Punto fin) {
        this.inicio = inicio;
        this.fin = fin;
    }

    public double calcularLongitud() {
        // La línea usa el comportamiento interno del punto
        return inicio.calcularDistanciaA(fin);
    }
}

Este diseño asegura que la interfaz pública de la clase Linea sea mínima y robusta. Al ser los atributos private final, se garantiza que la línea siempre conecte los mismos dos puntos durante todo su ciclo de vida, evitando efectos secundarios imprevistos en el resto del programa.

## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta
La multiplicidad se define como el número de instancias de una clase que pueden estar vinculadas a una instancia de otra clase en una relación determinada. Es un concepto fundamental en el diseño de sistemas para establecer límites y reglas sobre cómo se estructuran los datos. Se suele representar mediante rangos numéricos (por ejemplo, 1, 0..1, o 1..).
En el ejemplo de la línea y el punto, la multiplicidad se analiza de la siguiente manera:
De Linea a Punto: La multiplicidad es exactamente 2. Por definición, una línea en este modelo requiere un punto de inicio y un punto de fin para existir. No puede tener ni más ni menos para cumplir su propósito funcional.
De Punto a Linea: La multiplicidad es de 0.. (cero a muchos). Un punto puede existir de forma independiente sin pertenecer a ninguna línea, o bien puede ser el extremo de una o múltiples líneas simultáneamente en el espacio.
Establecer correctamente la multiplicidad permite prever qué estructuras de datos serán necesarias para implementar la relación. Mientras que una multiplicidad de $2$ se resuelve con dos atributos simples, una multiplicidad de "muchos" requeriría el uso de colecciones como arrays o listas.

## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta
La distinción entre composición fuerte y débil radica en la dependencia del ciclo de vida entre el objeto "todo" y sus objetos "parte". En la composición fuerte (referida simplemente como composición), existe una propiedad existencial: si el objeto contenedor desaparece, las partes también deben desaparecer. Un ejemplo clásico es un Edificio y sus Habitaciones; las habitaciones no tienen sentido ni existencia independiente fuera del edificio.

En la composición débil (conocida técnicamente como agregación o asociación), la relación es de pertenencia pero no de dependencia vital. Los objetos parte tienen un ciclo de vida propio y pueden sobrevivir a la destrucción del contenedor. Un ejemplo sería un Departamento y sus Profesores; si el departamento se disuelve, los profesores siguen existiendo y pueden vincularse a otros departamentos.

La consecuencia inmediata en la gestión de memoria y el diseño es clara: en la composición fuerte, el contenedor suele ser el responsable de crear y destruir sus partes. En la agregación, el contenedor suele recibir referencias a objetos ya creados externamente, limitándose a usarlos sin ser el dueño absoluto de su existencia.

## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta
En estos escenarios se habla estrictamente de dependencia (o relación de uso). A diferencia de la composición, donde un objeto "tiene" a otro como parte de su estado permanente (un atributo), la dependencia es una relación transitoria. Se describe no como un "A tiene-un B", sino como un "A usa-un B" para realizar una tarea específica en un momento puntual.

La dependencia es el nivel más débil de acoplamiento entre clases. Ocurre cuando un método de la clase A necesita conocer la interfaz de la clase B para invocar un método, para instanciar un objeto temporal que morirá al terminar la función, o para procesar un dato que llega desde el exterior. Una vez que el hilo de ejecución sale del método en cuestión, la relación desaparece.

Es vital distinguir ambos conceptos para mantener un diseño limpio. Abusar de la composición cuando solo se necesita una dependencia aumenta innecesariamente el uso de memoria y la complejidad del objeto. Por el contrario, ignorar una composición necesaria y tratarla como dependencia obligaría a pasar constantemente los mismos objetos como parámetros, ensuciando la interfaz de los métodos.

## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta
