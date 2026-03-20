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


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta


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
