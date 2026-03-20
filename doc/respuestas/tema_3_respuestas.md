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
En los lenguajes de programación orientada a objetos modernos, las excepciones son efectivamente objetos. A diferencia de los códigos de error numéricos de C, una excepción es una instancia de una clase específica que hereda de una jerarquía común (en Java, la clase base es Throwable). Esto permite que el error no sea solo una señal de "algo falló", sino una entidad con estado y comportamiento propio.

Desde la perspectiva de la encapsulación, tratar las excepciones como objetos permite agrupar toda la información relevante del error en un solo paquete. Un objeto excepción puede contener campos privados para códigos de error específicos, marcas de tiempo, identificadores de usuario o el estado del sistema en el momento del fallo. Al exponer estos datos a través de métodos públicos, se mantiene la integridad de la información del error mientras se ofrece una interfaz clara para que el manejador tome decisiones informadas.

La naturaleza de objeto de las excepciones permite, por tanto, la creación de excepciones personalizadas. Simplemente se debe definir una nueva clase que extienda de Exception o RuntimeException. Esto es fundamental en el diseño de software profesional, ya que permite al programador definir errores semánticos específicos de su dominio (como SaldoInsuficienteException o UsuarioNoEncontradoException), facilitando un control de errores mucho más preciso y descriptivo que el uso de excepciones genéricas.

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta
Mientras que en C un error suele reducirse a un entero devuelto por una función o guardado en la variable global errno, un objeto excepción en Java transporta una carga informativa mucho más rica. La información más crítica es el mensaje de error descriptivo y, sobre todo, la traza de la pila (stack trace). La traza de la pila es una lista detallada de todos los métodos que estaban activos en el momento del lanzamiento, indicando el archivo y la línea exacta donde se originó el problema.

Otra pieza de información esencial es la causa original (conocida como exception chaining). En sistemas complejos, una excepción de bajo nivel (como un error de red) puede ser capturada y envuelta en una de alto nivel (como un error de servicio). El objeto excepción permite almacenar la referencia a la excepción original, permitiendo que el desarrollador rastree el fallo a través de diferentes capas de abstracción sin perder el contexto inicial.

Esta riqueza informativa permite que los manejadores de excepciones no solo sepan que ocurrió un error, sino que posean un "mapa" completo de la ejecución del programa hasta ese punto. En C, una vez que un error se propaga fuera de la función, se pierde el contexto de qué línea exacta lo causó; en Java, esa información viaja encapsulada dentro del objeto hasta que alguien decide procesarla o imprimirla.

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta
Se permite, y a menudo es recomendable, definir múltiples bloques catch para un único bloque try. Esto se hace para capturar diferentes tipos de excepciones que podrían surgir de una misma secuencia de instrucciones. Por ejemplo, al leer un fichero, se podría querer capturar una FileNotFoundException de forma distinta a una IOException general o a una SecurityException.

En cuanto a la ejecución, solo se ejecuta un bloque catch por cada excepción lanzada: el primero cuya clase coincida o sea una superclase de la excepción lanzada. El orden de los bloques es, por tanto, crucial. Se debe colocar siempre las excepciones más específicas arriba y las más genéricas abajo. Si se colocara catch (Exception e) al principio, este capturaría absolutamente todo, dejando los bloques inferiores como código inalcanzable, lo cual generaría un error de compilación.

Una vez que un bloque catch termina de ejecutarse, el control del programa salta directamente al final de toda la estructura try-catch (o al bloque finally, si existe). No se "siguen probando" los demás bloques catch una vez que uno ha sido seleccionado, garantizando que el manejo del error sea único y predecible.

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta
Para garantizar la liberación de recursos críticos en presencia de errores, Java proporciona el bloque finally. Este bloque se sitúa después de los bloques catch y contiene código que se ejecutará sin importar si el bloque try terminó con éxito o si se lanzó una excepción que fue (o no) capturada. Es la herramienta estándar para cerrar descriptores de ficheros, conexiones de red o liberar bloqueos de memoria.

El uso de finally con catch es el escenario más común, donde se intenta una operación, se manejan los errores conocidos y se asegura la limpieza final. Por otro lado, el uso de try-finally (sin catch) es sumamente útil cuando una función no sabe cómo manejar el error y prefiere que este se propague, pero aun así tiene la responsabilidad de cerrar sus propios recursos antes de "morir".

// Ejemplo con try-catch-finally
try {
    abrirFichero();
    leerDatos();
} catch (IOException e) {
    System.out.println("Error al leer");
} finally {
    // Se ejecuta ocurra o no el error
    cerrarFichero();
}

// Ejemplo con try-finally (la excepción se propaga hacia arriba)
try {
    conectarseABaseDeDatos();
    ejecutarConsulta();
} finally {
    // Se garantiza el cierre incluso si la consulta falla y la excepción viaja al main
    cerrarConexion();
}

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta
Es perfectamente válido que un bloque finally aparezca sin necesidad de un bloque catch. Esta estructura se emplea cuando el objetivo primordial no es gestionar el error en ese punto, sino asegurar la integridad del estado del programa o la liberación de recursos antes de que la excepción continúe su camino por la pila de llamadas. El único requisito es que un bloque try vaya seguido de al menos un catch o de un finally.

Se garantiza que el código dentro de finally se ejecuta siempre, independientemente de si se lanzó una excepción o no. Incluso en situaciones de éxito donde el código del try se completa normalmente, el flujo pasará por el finally antes de continuar con la siguiente instrucción fuera del bloque. Solo existen casos extremos donde no se ejecutaría, como un fallo catastrófico de la Máquina Virtual, un corte de energía o la llamada explícita a System.exit().

Un detalle técnico fundamental es el comportamiento ante la sentencia return. Si existe un return dentro del bloque try, la Máquina Virtual no sale de la función inmediatamente; primero ejecuta todo el contenido del bloque finally y, solo entonces, se hace efectivo el retorno del valor. Este comportamiento asegura que nunca se deje un recurso abierto por el hecho de querer finalizar una función prematuramente tras un éxito.


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta
En Java, la distinción entre excepciones radica en si el compilador obliga o no a gestionarlas. Las excepciones controladas (checked exceptions) son aquellas que heredan de Exception (pero no de RuntimeException). El compilador exige que estas excepciones sean capturadas con un try-catch o declaradas con throws. Representan condiciones que el programa debería prever y de las que, en teoría, podría recuperarse, como un archivo que no se encuentra o un error de red.

Las excepciones no controladas (unchecked exceptions) son aquellas que heredan de la clase RuntimeException. El compilador no obliga a tratarlas ni a declararlas. Estas excepciones suelen representar errores de programación, fallos en la lógica o violaciones de contratos de uso que deberían haberse evitado durante el desarrollo, más que errores de los que el programa deba recuperarse en tiempo de ejecución.

Situaciones para excepciones controladas (Checked):

Fallo en la conexión con una base de datos externa.

Intento de lectura de un archivo que ha sido bloqueado por el sistema operativo.

Error de protocolo en una comunicación a través de un socket.

Expiración de un certificado de seguridad durante una transacción.

Situaciones para excepciones no controladas (Unchecked):

Acceso a un índice fuera de los límites de un array (ArrayIndexOutOfBoundsException).

Llamada a un método sobre una referencia que apunta a null (NullPointerException).

Paso de un argumento con un valor inválido a un método (IllegalArgumentException).

Estado ilegal de un objeto para realizar una operación concreta (IllegalStateException).

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta
La palabra reservada throws se utiliza en la firma de un método para declarar que dicho método puede lanzar una o varias excepciones específicas durante su ejecución sin capturarlas internamente. Funciona como un aviso para cualquier otro método que decida llamar a este: "ten cuidado, esta función podría fallar por estas razones y tú deberás decidir qué hacer si eso ocurre".

Es la alternativa directa al bloque try-catch porque, en lugar de resolver el problema en el lugar donde ocurre, delega la responsabilidad al método llamador. Mientras que el catch detiene la propagación y maneja el error, el throws permite que la excepción siga su curso natural hacia arriba en la pila de llamadas. Esto es especialmente útil cuando el método actual es una función de bajo nivel que no tiene suficiente contexto para decidir cómo informar al usuario o cómo reaccionar ante el fallo.

Desde el punto de vista del diseño, el uso de throws documenta el comportamiento del método y obliga al programador que lo utiliza a ser consciente de los riesgos. Si un método declara que lanza una excepción controlada, el llamador se verá obligado por el compilador a elegir entre capturarla él mismo o volver a declararla con throws, subiendo un escalón más en la jerarquía de llamadas.

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta
En este ejemplo, se observa cómo el método procesarArchivo declara mediante throws que puede producirse una FileNotFoundException. Al no incluir un bloque catch, cualquier error de este tipo "saltará" fuera del método hacia quien lo haya invocado. Sin embargo, se emplea finally para asegurar que el flujo de limpieza de recursos se ejecute siempre.

import java.io.*;

public class GestorArchivos {
    public void procesarArchivo(String ruta) throws FileNotFoundException {
        // Se asume que FileReader puede lanzar FileNotFoundException (controlada)
        FileReader lector = new FileReader(ruta);
        
        try {
            // Lógica de lectura aquí...
            System.out.println("Leyendo archivo...");
        } finally {
            // Se garantiza la ejecución de este bloque aunque 
            // no haya catch y la excepción se esté propagando.
            System.out.println("Cerrando recursos de forma segura.");
            try {
                lector.close();
            } catch (IOException e) {
                // Manejo mínimo del cierre
            }
        }
    }
}

Es importante notar que el finally se ejecutará justo después de que se produzca el error en el try (si ocurre), y antes de que la excepción sea enviada definitivamente al método llamador. Esto garantiza que, aunque el método "muera" debido a la excepción, no deje archivos abiertos o conexiones activas.

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta
Técnicamente, Java permite incluir excepciones no controladas (descendientes de RuntimeException) en la cláusula throws de un método. No es un error de compilación ni una infracción de las reglas del lenguaje. Sin embargo, a diferencia de las excepciones controladas, el compilador ignorará esta declaración a efectos de obligar al uso de try-catch.

El método llamador no tiene la obligación de poner un bloque try-catch aunque vea una RuntimeException en la firma del método que invoca. El programa compilará perfectamente sin él. Si la excepción llega a producirse y nadie la captura, el hilo de ejecución simplemente terminará como ocurre con cualquier error de tiempo de ejecución no gestionado.

El sentido de incluir estas excepciones en el throws es puramente de documentación y comunicación. Sirve para advertir de forma explícita al programador que utiliza la función sobre condiciones de error importantes que podrían ocurrir si no se respetan las precondiciones. Es una forma de decir: "aunque no te obligo a capturarla, quiero que sepas que puedo lanzar esta excepción si me pasas datos incorrectos".

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta
La recomendación general es usar excepciones controladas para condiciones de error que son externas al control directo del programador y de las que el sistema puede recuperarse razonablemente (fallos de red, archivos ausentes, falta de permisos). Se usan cuando el error es una posibilidad real del entorno. En cambio, se recomiendan las no controladas para errores que el programador podría haber evitado mediante una lógica correcta, como pasar un puntero nulo o un índice de array inexistente.

Java es prácticamente el único lenguaje de uso masivo que implementa de forma estricta las excepciones controladas. Otros lenguajes modernos y populares, como C#, Python, C++, Kotlin o Swift, han optado por un modelo donde todas las excepciones son no controladas. En estos lenguajes, el compilador nunca obliga al programador a escribir un try-catch, dejando la responsabilidad de la robustez totalmente en manos del desarrollador.

En los lenguajes donde solo existe una opción, la más habitual es, por tanto, la no controlada. La tendencia en el diseño de lenguajes ha evolucionado hacia este modelo porque las excepciones controladas en Java, aunque bienintencionadas, a menudo llevan a un exceso de código repetitivo y a que los programadores acaben "silenciando" errores con bloques catch vacíos solo para que el código compile, lo cual resulta contraproducente para la seguridad del software.

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

