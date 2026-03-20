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
La encapsulación se define como el mecanismo que permite agrupar datos (atributos) y los métodos que operan sobre ellos dentro de una misma unidad o clase. Se busca con esto que el objeto sea una entidad autónoma donde el estado y el comportamiento estén vinculados de forma indisoluble, evitando que los datos queden dispersos por el programa como ocurre con las variables globales en lenguajes procedurales como C.

Por su parte, la ocultación de información es la capacidad de restringir el acceso a los detalles internos de un objeto. Mientras que la encapsulación "empaqueta" los elementos, la ocultación decide qué partes son visibles desde el exterior. El objetivo principal es proteger la integridad del objeto, impidiendo que código externo modifique su estado interno de manera arbitraria o dependa de detalles de implementación que podrían cambiar en el futuro.

Entre las ventajas de la ocultación de información se encuentran:

Reducción de la complejidad: El usuario del objeto solo necesita saber qué hace el objeto, no cómo lo hace.

Aislamiento de cambios: Se pueden modificar los detalles internos de una clase (como cambiar un tipo de dato o un algoritmo) sin que el resto del programa se vea afectado, siempre que la parte visible se mantenga igual.

Aumento de la robustez: Se evita la manipulación accidental de datos sensibles, lo que reduce la aparición de errores difíciles de rastrear.

## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta
La interfaz pública se entiende como el conjunto de métodos y propiedades que una clase expone al mundo exterior. Representa el "contrato" o la lista de servicios que el objeto promete realizar para quienes interactúen con él. En términos prácticos, está compuesta por todos aquellos miembros marcados con el modificador de acceso public.

La relación con la ocultación de información es de complementariedad. Mientras que la ocultación "encierra" los detalles complejos y los datos en bruto tras una barrera de privacidad, la interfaz pública es la "ventana" o el panel de control a través del cual se interactúa de forma segura. Se dice que la interfaz es el "qué" hace el objeto, mientras que la parte oculta es el "cómo" lo hace.

En este sentido, una buena interfaz pública debe ser mínima y suficiente. Se busca ofrecer solo lo estrictamente necesario para que el objeto sea útil, manteniendo el resto de la implementación en el ámbito privado para conservar la flexibilidad y la seguridad del diseño.

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta
El diseño de la interfaz pública requiere especial cuidado porque constituye un compromiso con el resto de los programadores o módulos que utilizarán dicha clase. Una vez que una interfaz es publicada y utilizada por otros componentes, cualquier cambio en ella (como renombrar un método o cambiar el tipo de un parámetro) genera un "efecto dominó" que obliga a modificar y recompilar todo el código dependiente.

Por lo tanto, no es fácil cambiar la interfaz pública una vez que el sistema ha crecido. Si se altera un método público, se rompe la compatibilidad hacia atrás. Es comparable a cambiar la forma de los enchufes en una casa: todos los electrodomésticos comprados anteriormente dejarían de funcionar. Por el contrario, los detalles internos (privados) pueden cambiarse con total libertad siempre que el resultado final entregado por la interfaz pública sea el mismo.

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta
Las invariantes de clase se definen como aquellas condiciones o reglas de negocio que deben cumplirse siempre para que un objeto se considere en un estado válido. Por ejemplo, en una clase que represente una fecha, una invariante sería que el valor del mes debe estar siempre entre 1 y 12. Si un objeto permite que su mes sea 13, se dice que ha violado su invariante y su estado es corrupto.

La ocultación de información es la herramienta fundamental para garantizar que estas invariantes no se rompan. Al declarar los atributos como privados, se impide que un agente externo asigne valores inválidos directamente. El acceso se canaliza a través de métodos públicos (como setters), donde se puede incluir lógica de validación que rechace cualquier valor que ponga en peligro la integridad de la invariante.

Sin la ocultación, el programador dependería de la "buena voluntad" de otros usuarios del código para no introducir datos erróneos. Gracias a ella, la propia clase se convierte en la responsable de supervisar su estado, asegurando que, pase lo que pase fuera, sus datos internos siempre respeten las reglas establecidas en su diseño.

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta
Se presenta a continuación la implementación de la clase Punto aplicando los principios de ocultación. Los atributos se mantienen privados para evitar modificaciones directas, y se ofrece el comportamiento a través de un método público.

public class Punto {
    // Atributos privados: Ocultación de información
    private double x;
    private double y;

    // Constructor público
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Método público: Parte de la interfaz
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(Math.pow(this.x, 2) + Math.pow(this.y, 2));
    }
}

La interfaz pública de esta clase está compuesta por el constructor Punto(double x, double y) y el método calcularDistanciaAOrigen(). Estos son los únicos puntos de contacto que un programador externo puede utilizar. Los atributos x e y no forman parte de la interfaz, ya que son invisibles desde fuera de la clase.

En cuanto a los modificadores:

private: Indica que el miembro solo es accesible desde dentro de la propia clase. Se utiliza para ocultar el estado y los detalles de implementación.

public: Indica que el miembro es accesible desde cualquier otra clase. Se utiliza para definir la interfaz y permitir que el objeto preste sus servicios al sistema.

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta
Los modificadores de acceso se aplican fundamentalmente a dos niveles dentro de la estructura de un programa en Java: a los miembros de una clase (atributos y métodos) y a las clases en sí mismas. Cuando se aplican a los miembros, se determina si otros objetos o clases pueden interactuar con esos datos o comportamientos específicos, siendo esta la base del encapsulamiento.

En el caso de las clases de nivel superior (aquellas definidas directamente en un archivo .java), solo se permite el uso del modificador public o la ausencia de modificador (visibilidad de paquete). No es posible declarar una clase de nivel superior como private, ya que esto impediría que cualquier otra parte del programa pudiera instanciarla, careciendo de utilidad práctica. Sin embargo, en las clases anidadas (clases dentro de otras clases), sí se permite el uso de private.

Finalmente, los constructores también reciben estos modificadores. Un constructor public permite la creación de instancias desde cualquier punto del código, mientras que un constructor private restringe la creación de objetos a métodos internos de la propia clase. Esta última técnica es común en ciertos patrones de diseño donde se desea controlar estrictamente cuántas instancias de una clase existen en memoria.

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta
Además de los niveles extremos de visibilidad (público y privado), la mayoría de los lenguajes de programación orientada a objetos introducen niveles intermedios para facilitar la colaboración entre clases relacionadas o dentro de una jerarquía. El nivel más común es protected, el cual permite el acceso a los miembros no solo a la propia clase, sino también a todas aquellas que hereden de ella (subclases), incluso si se encuentran en diferentes paquetes o módulos.

En Java, existe un cuarto nivel denominado "visibilidad de paquete" (o package-private), que se aplica por defecto cuando no se especifica ningún modificador. Este nivel permite que cualquier clase que resida en el mismo paquete acceda a los miembros, pero prohíbe el acceso desde fuera de él. Es un nivel intermedio de confianza que resulta muy útil para organizar módulos de software donde varias clases cooperan estrechamente entre sí.

Otros lenguajes implementan esquemas similares pero con matices distintos. En C++, por ejemplo, se utilizan los mismos términos (public, protected, private), pero la visibilidad se define por bloques dentro de la clase. En lenguajes como Python, la visibilidad no es restrictiva por compilación; en su lugar, se sigue una convención donde el uso de uno o dos guiones bajos iniciales (_variable o __variable) indica al programador que ese miembro debe tratarse como privado o protegido, apelando a la responsabilidad del desarrollador.

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta
La respuesta correcta es la (a): otras clases. En Java, la visibilidad private se define a nivel de clase y no a nivel de instancia. Esto significa que un objeto de la clase Punto puede acceder libremente a los atributos privados de otro objeto de la clase Punto, siempre que dicho acceso se produzca dentro del código de la propia clase.

Este comportamiento es fundamental para implementar operaciones que involucran a dos objetos del mismo tipo, como comparaciones de igualdad o cálculos geométricos. Si la visibilidad fuera por instancia, el objeto tendría que recurrir a métodos públicos para conocer los datos del "otro" objeto, lo cual sería ineficiente y complicaría innecesariamente el diseño interno de la clase.

A continuación se muestra el ejemplo solicitado:
public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAPunto(Punto otro) {
        // Se accede directamente a 'otro.x' y 'otro.y' aunque sean private
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
Como se observa en el método calcularDistanciaAPunto, el código accede a otro.x y otro.y sin problemas. Dado que el método pertenece a la clase Punto, el compilador permite el acceso a cualquier miembro privado de cualquier objeto que sea de tipo Punto.

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta
Los métodos getter y setter (también conocidos como accesores y mutadores) son métodos públicos diseñados para leer o modificar el valor de un atributo privado, respectivamente. Su uso es una pieza clave de la encapsulación, ya que permiten que la clase mantenga el control total sobre sus datos internos, en lugar de exponer sus variables directamente al exterior.

El método getter tiene como única misión retornar el valor de un atributo. Esto permite que el atributo sea de "solo lectura" para el resto del programa si no se proporciona un setter equivalente. Por otro lado, el método setter recibe un parámetro y lo asigna al atributo interno. La gran ventaja del setter es que permite incluir lógica de validación; por ejemplo, se podría impedir que se asigne un valor negativo a un atributo que represente una edad o un precio.

El empleo de estos métodos mejora la mantenibilidad del código. Si en el futuro se decidiera cambiar la forma en que se almacena un dato (por ejemplo, pasar de almacenar la "edad" a almacenar la "fecha de nacimiento"), se podría modificar el interior del getter para que calcule la edad al vuelo, sin que las clases que llaman al método noten cambio alguno en la interfaz.

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta
En el contexto de la ingeniería de software y la POO, el término "seguridad" no se refiere habitualmente a la protección contra ciberataques externos o "hackeos" malintencionados. En su lugar, se hace referencia a la seguridad interna del código y a la robustez del sistema frente a errores humanos durante el desarrollo. Se busca evitar que un programador, por desconocimiento o descuido, altere el estado de un objeto de una manera que lo deje en un estado inconsistente o inválido.

La ocultación de información garantiza la integridad de las invariantes de clase. Al prohibir el acceso directo a los datos, se asegura que ninguna parte externa del programa pueda corromper la lógica interna del objeto. Por tanto, la "seguridad" aquí descrita es sinónimo de estabilidad y fiabilidad técnica, permitiendo que grandes equipos de trabajo colaboren en un mismo proyecto sin miedo a que los cambios en una zona del código provoquen fallos catastróficos e impredecibles en otras.

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta
La diferencia fundamental radica en el ámbito de existencia y la propiedad de los datos. Un miembro de instancia (atributo o método) pertenece a un objeto específico creado con la palabra reservada new. Cada objeto tiene su propia copia de los atributos de instancia, lo que permite que diferentes instancias de una misma clase tengan estados distintos (por ejemplo, dos objetos Punto con diferentes coordenadas x e y).
Por el contrario, un miembro de clase pertenece a la definición de la clase en sí y no a un objeto concreto. Solo existe una única copia de este miembro para todos los objetos de esa clase, compartiendo el mismo valor y espacio de memoria. En lenguajes como C, esto guarda similitud con las variables globales o estáticas, donde la información persiste independientemente de cuántas veces se acceda a ella.
Respecto a la ocultación, los miembros de clase también pueden (y suelen) ocultarse utilizando los modificadores de acceso. Es una práctica recomendada declarar los atributos de clase como private para evitar que agentes externos modifiquen valores compartidos que podrían afectar al comportamiento de todos los objetos del sistema simultáneamente.

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta
Desde una perspectiva inicial, un constructor privado puede parecer contradictorio, ya que impediría el uso de new desde fuera de la clase. Sin embargo, este diseño tiene mucho sentido en varios escenarios avanzados de la programación orientada a objetos. El uso principal es el control estricto sobre la creación de instancias, permitiendo que sea la propia clase la que decida cuándo y cómo se crea un objeto.

Un constructor privado es esencial en patrones de diseño como el Singleton, donde se garantiza que solo exista una única instancia de la clase en todo el programa. También se emplea en clases de utilidad que solo contienen métodos estáticos (como la clase Math de Java), donde no tiene sentido crear objetos porque no hay un estado interno que manejar.

Finalmente, los constructores privados permiten implementar métodos factoría. Estos métodos ofrecen una forma más descriptiva de crear objetos, pudiendo retornar instancias previamente creadas (caché) o incluso subclases específicas, algo que un constructor estándar no puede hacer de forma tan flexible.

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta
En Java, los miembros de clase se indican mediante la palabra reservada static. Al anteponer este modificador a la declaración de un atributo o método, se le indica a la Máquina Virtual que dicho elemento debe cargarse en una zona de memoria especial (área estática) asociada a la clase, y no en el heap junto a cada instancia individual.

A continuación, se presenta la implementación de la clase Punto con la lógica para rastrear los valores máximos de las coordenadas globales. Se asume que estos valores se actualizan cada vez que se crea un nuevo punto o se modifican sus valores:
public class Punto {
    private double x;
    private double y;

    // Miembros de clase (compartidos por todos los objetos)
    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        actualizarMaximos(x, y);
    }

    private static void actualizarMaximos(double x, double y) {
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }
}

Es importante notar que los métodos que acceden a maxX y maxY también se declaran como static. Esto permite consultar los valores máximos sin necesidad de tener un objeto Punto concreto, simplemente llamando a Punto.getMaxX().

## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta
Para implementar este comportamiento se debe utilizar obligatoriamente el modificador static. Esto se debe a que un método factoría tiene la responsabilidad de crear el objeto; por lo tanto, debe poder invocarse antes de que la instancia exista. Si el método no fuera estático, se caería en una paradoja donde se necesitaría un objeto para poder llamar al método que crea objetos.

public static Punto crearPuntoRedondeado(double x, double y) {
    double xRedondeado = Math.round(x);
    double yRedondeado = Math.round(y);
    return new Punto(xRedondeado, yRedondeado);
}

El uso de static en este contexto permite que la sintaxis de uso sea limpia y legible, como por ejemplo: Punto p = Punto.crearPuntoRedondeado(10.7, 4.2);. Esto separa la lógica de construcción compleja (el redondeo) de la simple asignación de valores que suele realizar el constructor básico.
## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta
Este ejercicio ilustra la potencia de la ocultación de información. Al cambiar la estructura interna de la clase (pasar de dos variables a un array), el código externo que utiliza la clase Punto no debería sufrir ningún cambio ni enterarse de la reestructuración, siempre que las firmas de los métodos públicos se mantengan idénticas.

public class Punto {
    // Implementación interna oculta: ahora es un array
    private double[] coordenadas = new double[2];

    public Punto(double x, double y) {
        this.coordenadas[0] = x; // Índice 0 para x
        this.coordenadas[1] = y; // Índice 1 para y
    }

    // La interfaz pública se mantiene igual (mismos nombres y tipos)
    public double getX() {
        return this.coordenadas[0];
    }

    public double getY() {
        return this.coordenadas[1];
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(Math.pow(coordenadas[0], 2) + Math.pow(coordenadas[1], 2));
    }
}

Gracias a que los datos están "encapsulados" tras los métodos, se ha podido modificar la eficiencia o la organización del almacenamiento interno sin romper el contrato con los usuarios de la clase. En C, si se cambiaran dos variables double por un array en un struct, todas las funciones que accedieran a p.x dejarían de compilar; en Java, este problema se evita mediante la interfaz pública.

## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.

### Respuesta


## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
