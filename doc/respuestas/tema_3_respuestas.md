<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

### Respuesta
En el lenguaje C, al carecer de un sistema de excepciones, el control de errores debe realizarse de forma manual mediante la gestión de valores de retorno o estados globales. El objetivo es que la lógica de la función informe al llamador de que algo ha fallado, para que este último decida cómo proceder (por ejemplo, imprimiendo un mensaje al usuario).

La primera opción consiste en reservar un valor de retorno especial que se encuentre fuera del dominio de los resultados válidos. Dado que la raíz cuadrada de un número positivo siempre es positiva, se puede devolver un valor negativo (como -1.0) para indicar una entrada inválida.

float raiz(float n) {
    if (n < 0) return -1.0; // Valor centinela para error
    return sqrt(n);
}
// Uso: if (raiz(x) < 0) printf("Error");

La segunda opción es utilizar un parámetro por referencia (puntero) para devolver el estado del cálculo, dejando el valor de retorno de la función exclusivamente para indicar si la operación tuvo éxito o fracasó. Esta técnica es más robusta porque permite distinguir claramente entre el resultado matemático y el estado de ejecución.

int raiz(float n, float *resultado) {
    if (n < 0) return 0; // 0 indica error
    *resultado = sqrt(n);
    return 1; // 1 indica éxito
}
// Uso: if (!raiz(x, &res)) printf("Error");



## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta
Una excepción es un evento extraordinario que ocurre durante la ejecución de un programa y que interrumpe el flujo normal de las instrucciones. En lenguajes como Java, una excepción no es simplemente un código numérico, sino un objeto que contiene información detallada sobre el error (qué ocurrió, dónde y en qué estado se encontraba la pila de llamadas).

Cuando un programador implementa una función, utiliza las excepciones para señalar que las precondiciones no se han cumplido o que ha surgido un imprevisto que impide devolver un resultado válido. El objetivo es delegar la responsabilidad del error: la función no intenta "arreglar" el problema ni imprimir mensajes (lo cual rompería la encapsulación), sino que simplemente informa de la anomalía al nivel superior.

Desde el punto de vista del programador que llama a la función, el objetivo de las excepciones es separar la lógica principal del programa (el "camino feliz") del código de gestión de errores. Esto evita que el código se llene de estructuras if-else repetitivas tras cada llamada a función, permitiendo agrupar el control de múltiples fallos en un único bloque especializado.

## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta
En Java, la gestión de errores se realiza mediante el bloque try-catch. La función raiz dejará de devolver valores mágicos (como -1.0) y en su lugar creará y enviará un objeto de excepción si detecta un parámetro inválido.

public class Calculadora {
    public double raiz(double n) {
        if (n < 0) {
            // Se lanza una excepción estándar para argumentos inválidos
            throw new IllegalArgumentException("No se puede calcular la raíz de un negativo");
        }
        return Math.sqrt(n);
    }

    public static void main(String[] args) {
        Calculadora calc = new Calculadora();
        try {
            double resultado = calc.raiz(-5.0);
            System.out.println("Resultado: " + resultado);
        } catch (IllegalArgumentException e) {
            // Se captura el error desde fuera de la función raiz
            System.out.println("Error detectado: " + e.getMessage());
        }
    }
}

Este enfoque permite que el método main tenga el control total sobre la interacción con el usuario. Si la función raiz fuera utilizada en una aplicación web o en un sistema embebido, el control del error se adaptaría a dicho entorno sin necesidad de modificar ni una sola línea de código dentro de la clase Calculadora.

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta
Lanzar una excepción (throw) es el acto de crear un objeto de excepción y entregarlo al entorno de ejecución cuando se detecta un error. Controlar o capturar (catch) es interceptar ese objeto en algún punto de la pila de llamadas para procesar el error y evitar que el programa aborte. Si una función lanza una excepción y no hay un bloque catch inmediato, la excepción se propaga hacia la función que la llamó, y así sucesivamente hacia arriba en la pila de llamadas.

Cuando una excepción se propaga, las funciones por las que pasa van terminando su ejecución de forma abrupta. El entorno de ejecución va eliminando los marcos de la pila (stack frames) uno a uno buscando un manejador compatible. Es importante destacar que las funciones que no controlan la excepción NO se reanudan; una vez que la excepción sale de la función, esa función se da por finalizada sin ejecutar el resto de sus instrucciones.

En el ejemplo de la raíz, si el método main llama a una función A, y A llama a raiz(-5), la excepción se lanza en raiz. Como raiz no tiene un try-catch, "muere" y la excepción viaja a A. Si A tampoco la controla, "muere" también y llega a main. Solo si main tiene el catch, el programa continúa su ejecución después de dicho bloque catch. Si nadie la captura, el hilo de ejecución termina y se muestra el error por consola.

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta
La principal ventaja de la propagación natural es la limpieza y legibilidad del código. En C, si se tiene una cadena de llamadas A -> B -> C -> D y ocurre un error en D, cada una de las funciones intermedias (C y B) debe incluir lógica explícita para comprobar el valor de retorno de la siguiente y devolverlo hacia arriba. Esto ensucia la lógica de negocio con constantes comprobaciones de errores que no le corresponden a ese nivel de abstracción.

En Java, gracias a la propagación, las funciones B y C pueden ignorar por completo la existencia del error si no tienen nada útil que hacer con él. La excepción "salta" automáticamente a través de ellas hasta encontrar un nivel que sepa cómo gestionar el problema. Esto permite centralizar la gestión de errores en los niveles superiores de la arquitectura, donde se toman las decisiones de control, manteniendo las funciones de cálculo puras y enfocadas en su tarea principal.

Además, este sistema garantiza que los errores no puedan ser ignorados por accidente. En C, es muy común olvidar comprobar el valor de retorno de una función, lo que lleva a que el programa continúe ejecutándose con datos corruptos. En Java, si no se captura una excepción, el programa se detiene de forma segura, obligando al desarrollador a ser consciente de que ha ocurrido un fallo que debe ser atendido.

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

