<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Respuesta
Para comprender este paradigma se deben identificar cuatro pilares: la abstracción, que simplifica la realidad extrayendo solo las propiedades esenciales, y el encapsulamiento, que agrupa datos y métodos restringiendo el acceso externo para proteger la integridad de la información.
A estos se añaden la herencia, que permite crear nuevas clases a partir de otras para reutilizar código y establecer jerarquías, y el polimorfismo, que otorga a distintos objetos la capacidad de responder de manera diferente a un mismo mensaje o llamada a un método.

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta
Entre los lenguajes más destacados se encuentra Java, reconocido por su enfoque estrictamente orientado a objetos, y C++, que nació como una evolución de C para incorporar clases manteniendo el control de bajo nivel.
También son fundamentales Python, muy valorado por su sintaxis legible y versatilidad multiparadigma, y C#, el estándar desarrollado por Microsoft para aplicaciones empresariales y el desarrollo de videojuegos profesionales.

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta
La programación estructurada se basa en organizar el software mediante una secuencia lógica de instrucciones, apoyándose en estructuras de control como bucles y condicionales. Su propósito es mejorar la claridad del flujo del programa, aunque mantiene los datos y las funciones que los manipulan como entidades separadas.
Por su parte, la programación modular avanza un paso más al dividir el código en bloques o módulos independientes. Cada módulo agrupa funciones relacionadas con una tarea específica, lo que facilita el mantenimiento del sistema y permite la reutilización de componentes en diferentes partes de una aplicación.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta
Un objeto se define fundamentalmente por su estado y su comportamiento. El estado está compuesto por los valores de sus atributos (equivalentes a los campos de una struct en C), mientras que el comportamiento se determina mediante los métodos o funciones que el objeto puede ejecutar.
El tercer elemento esencial es la identidad, que es la propiedad que distingue a un objeto de cualquier otro dentro del sistema. Gracias a la identidad, el entorno de ejecución puede reconocer dos objetos como entidade

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta
Una clase se describe como el molde o plano que especifica qué datos y funciones tendrá un tipo de objeto. No es lo mismo que un objeto; mientras la clase es la definición abstracta, el objeto es la entidad real que ocupa espacio en la memoria durante la ejecución del programa.
El término instancia se utiliza para designar a un objeto concreto que ha sido creado a partir de una clase. Se dice que el objeto es una "instancia de la clase" porque representa un ejemplar específico de ese modelo general previamente definido.
Aunque la mayoría de los lenguajes populares utilizan clases, no todos los lenguajes orientados a objetos se basan en ellas. Existen lenguajes basados en prototipos, como JavaScript, donde los objetos pueden crearse clonando otros objetos directamente sin necesidad de definir una clase formal.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta
Los objetos se almacenan generalmente en una región de memoria llamada Heap (montón).

Heap vs. Stack: Mientras que las variables locales y las llamadas a funciones viven en el Stack (pila), que es rápido pero pequeño, los objetos viven en el Heap, que es mucho más grande y flexible.

¿Es igual en todos? No exactamente. En lenguajes como C++, tú decides si un objeto va al Stack o al Heap. En Java o C#, casi todos los objetos van directos al Heap.

Recolección de basura (Garbage Collection): Es un proceso automático que busca objetos en el Heap que ya no están siendo utilizados (nadie los referencia) y libera esa memoria para que el programa no se quede sin espacio. ¡Es como tener un equipo de limpieza que pasa por tu código para que tú no tengas que borrar manualmente cada objeto!

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta
Respuesta
Método: Es un bloque de código dentro de una clase que define un comportamiento o acción que el objeto puede realizar. Es el equivalente a una función, pero "vive" dentro de la clase.

Sobrecarga de métodos: Es la capacidad de definir varios métodos con el mismo nombre dentro de la misma clase, siempre que su lista de parámetros sea distinta (diferente número de argumentos o diferentes tipos).

Ejemplo rápido: Puedes tener un método dibujar(String color) y otro dibujar(int grosor). El compilador sabe cuál usar dependiendo de lo que le pases.

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta
class Punto {
    int x; // Visibilidad por defecto
    int y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

public class Main {
    public static void main(String[] args) {
        // Crear una instancia (objeto)
        Punto miPunto = new Punto();
        miPunto.x = 3;
        miPunto.y = 4;

        // Uso del método
        double distancia = miPunto.calculaDistanciaAOrigen();
        System.out.println("La distancia al origen es: " + distancia);
    }
}

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta
Respuesta
Punto de entrada: Es el método public static void main(String[] args). Sin esto, la JVM no sabe por dónde empezar a ejecutar tu programa.

¿Qué es static? Significa que el método o atributo pertenece a la clase en sí, y no a una instancia (objeto) específica. Puedes usarlo sin crear un objeto con new.

¿Solo para main? ¡No! Se usa para métodos de utilidad (como Math.sqrt()) o para contar cuántos objetos se han creado de una clase.

Combinación con final: Se usa para crear constantes. Por ejemplo: static final double PI = 3.14159;. Al ser static, solo existe una copia; al ser final, su valor no puede cambiar.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta
Para trabajar con Java desde la terminal, el flujo es:

Compilar: Usas el comando javac Punto.java. Esto genera un archivo llamado Punto.class.

Ejecutar: Usas el comando java Punto (sin la extensión).

Conceptos clave:

¿Es compilado? Sí, pero no a código máquina directamente. Es un lenguaje híbrido.

Byte-code: Es el código intermedio (el contenido de los archivos .class) que genera el compilador. No lo entiende tu procesador, pero sí la JVM.

Máquina Virtual (JVM): Es el software que interpreta o compila el byte-code en tiempo real para que tu ordenador lo entienda. Esto es lo que permite que Java sea "escríbelo una vez, ejecútalo en cualquier parte".

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta

new: Es el operador encargado de instanciar un objeto. Su función es pedirle a la Máquina Virtual (JVM) que reserve un espacio de memoria en el Heap para el nuevo objeto y devuelva la dirección de memoria (referencia) donde se encuentra.

Constructor: Es un método especial que se ejecuta automáticamente al usar new. Su objetivo es inicializar el estado del objeto (darle valores iniciales a sus atributos). Se reconoce porque tiene el mismo nombre que la clase y no tiene tipo de retorno.

Ejemplo de la clase Empleado:
public class Empleado {
    String dni;
    String nombre;
    String apellidos;

    // Este es el constructor
    public Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta

this es una palabra reservada que hace referencia a "este objeto" (la instancia actual en la que se está ejecutando el código). Se usa principalmente para evitar ambigüedades cuando el nombre de un parámetro es igual al de un atributo.

En otros lenguajes: No siempre se llama igual. En Python se usa self, en PHP se usa $this, y en Visual Basic se usa Me.

Ejemplo en Punto:
public class Punto {
    int x;
    int y;

    public void setPosicion(int x, int y) {
        this.x = x; // 'this.x' es el atributo, 'x' es el parámetro
        this.y = y;
    }
}


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta

Para calcular la distancia entre dos puntos $(x_1, y_1)$ y $(x_2, y_2)$, usamos la fórmula: sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}

public double distanciaA(Punto otroPunto) {
    int dx = this.x - otroPunto.x;
    int dy = this.y - otroPunto.y;
    return Math.sqrt(dx * dx + dy * dy);
}

## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta

Esta es la "pregunta trampa" clásica de Java. La respuesta técnica es que Java siempre pasa todo por valor, pero el comportamiento varía según el tipo:

Objetos (como Punto): Lo que se pasa por valor es la referencia (la dirección de memoria). Por tanto, si modificas un atributo del objeto dentro del método (punto.x = 10), los cambios SÍ afectan al objeto original fuera del método, porque ambos apuntan al mismo sitio en el Heap.

Primitivos (como int): Se pasa una copia literal del valor. Si cambias el int dentro del método, NO afecta a la variable original.

Resumen: Los objetos se comportan como si fueran "por referencia" (puedes mutarlos), pero los primitivos son estrictamente "por copia".


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta

¿Qué es? Es un método heredado de la clase base Object. Su propósito es devolver una representación en texto (un String) del objeto. Si no lo sobrescribes, Java imprime algo feo como Punto@1a2b3c.

¿Existe en otros? Sí, casi todos tienen un equivalente. En Python es __str__ o __repr__, en C# es ToString(), y en JavaScript es toString().

Ejemplo en Punto:
@Override
public String toString() {
    return "Punto(x=" + x + ", y=" + y + ")";
}


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta

En esencia, sí: una clase es la evolución natural de un struct. Si lo piensas, ambos son contenedores que agrupan datos relacionados. Sin embargo, para que un struct de C se convierta en una clase de Java, le faltan tres pilares fundamentales:

Comportamiento (Métodos): Un struct solo guarda datos (estado). Una clase guarda datos y las funciones que operan sobre esos datos (métodos).

Encapsulamiento (Visibilidad): En un struct, todo es público por defecto. No existen private o protected para proteger los datos de usos indebidos.

Mecanismos de Herencia y Polimorfismo: Un struct no puede "heredar" de otro de forma nativa, ni puedes redefinir comportamientos de la misma manera que lo haces en la Programación Orientada a Objetos (POO).

En resumen: Un struct es una caja de herramientas pasiva. Una clase es un objeto inteligente que sabe qué datos tiene y qué puede hacer con ellos.


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta

Para "emular" una clase en C, tenemos que separar los datos de la lógica, pero forzarlos a trabajar juntos. El truco está en pasar el struct como argumento a la función.

Ejemplo en C:
#include <stdio.h>
#include <math.h>

// Los datos (atributos)
struct Punto {
    int x;
    int y;
};

// El "método" (necesita recibir el struct explícitamente)
double calculaDistanciaAOrigen(struct Punto *self) {
    return sqrt(self->x * self->x + self->y * self->y);
}

int main() {
    struct Punto miPunto = {3, 4};
    // Llamamos a la función pasando la dirección del struct
    double d = calculaDistanciaAOrigen(&miPunto);
    printf("Distancia: %f", d);
    return 0;
}

¿Qué ha pasado con this?
En Java, this es implícito: el lenguaje lo pasa por ti "por detrás" cada vez que llamas a un método. En C, ese this tiene que ser explícito. En el ejemplo de arriba, el parámetro struct Punto *self es exactamente lo mismo que el this de Java.

Cuando en Java haces miPunto.calculaDistancia(), la Máquina Virtual realmente está haciendo algo muy parecido a calculaDistancia(miPunto). ¡La magia de la POO es simplemente que el lenguaje te ahorra escribir ese parámetro!
