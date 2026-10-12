# Actividad-SRP-Relaciones

Actividad 1
1.  Asociación entre Prestamo y Socio: Prestamo mantiene una referencia a Socio como atributo de clase. Esto porque el préstamo debe recordar qué socio realizó la transacción, pero el socio sigue existiendo de forma independiente en el sistema si el préstamo finaliza o se elimina.
Asociación entre Prestamo y Libro: Prestamo tiene una referencia a Libro como atributo de clase. Un libro existe por si mismo en el catálogo de la biblioteca independientemente de si está prestado o no, y perdura tras la devolución.
Dependencia entre Préstamo e Impresora: La clase Prestamo utiliza a la clase Impresora de forma puntual dentro de la firma/ejecución de un método (como parámetro en devolver). Porque Prestamo no guarda a la impresora como atributo, únicamente la usa temporalmente para emitir el comprobante.
2. Relación Prestamo – Socio:
 - Un Prestamo está asociado exactamente a 1 Socio. Esto significa que cada registro de préstamo pertenece de manera unívoca a un cliente específico de la biblioteca.
 - Un Socio puede estar asociado a 1, o a 0, préstamos en el historial. Significa que una persona puede realizar múltiples préstamos a lo largo del tiempo.
Relación Prestamo – Libro:
 - Un Préstamo se refiere exactamente a 1 Libro. Indica que cada transacción individual documenta la salida de una unidad documental.
 - Un Libro puede estar presente en 1, o 0, préstamos dentro del historial. Significa que el mismo libro puede ser prestado sucesivamente a distintos socios en diferentes momentos.
3. No, no bastaría. Las dependencias son relaciones débiles y temporales (ej: recibir un objeto por parámetro). El enunciado establece explícitamente que “Cada préstamo guarda un socio y un libro” y  que “Al devolver el libro, el socio y el libro siguen registrados. El préstamo se conserva en el historial”.  Esto exige una relación estructural de asociación, donde la clase Prestamo retenga como atributos persistentes las referencias a Socio y Libro. Si solo fueran dependencias, el préstamo olvidaría que el socio y que libro estuvieron involucrados una vez finalizada la ejecución del método.
4. Los atributos que Prestamo debería guardar son:
 - socio (de tipo Socio): Para mantener la identidad de la persona responsable del préstamo en el historial.
 - libro (de tipo Libro): Para asociar directamente cuál es la obra prestada.
 - devuelto (de tipo boolean): Para registrar el estado operativo del préstamo y saber si el ítem ya fue retornado o sigue pendiente,
Según el enunciado, el préstamo actúa como un registro histórico que vincula al usuario con la obra y mantiene su estado de devolución.
5. Para el método devolver, se propone los siguientes parámetros:
 - diasAtraso (tipo int): Representa la cantidad de días de demora en la entrega. Sirve para calcular el monto total de la multa ($500 por día si diasAtraso > 0).
 - impresora (tipo Impresora): Representa el recurso o dispositivo encargado del servicio de impresión. Sirve para invocar su método imprimir(…) y emitir el comprobante físico/digital del cierre del préstamo sin que Prestamo tenga que instanciar o guardar la impresora. 


Actividad 2
1. Asociación entre Préstamo y Socio: Se refiere en la declaración del atributo private Socio socio; dentro de la clase Parametro y en la asignación realizada en su constructor this.socio = socio;. Muestra una relación de permanencia donde un objeto Socio es guardado como parte del estado de Prestamo.
Asociación entre Préstamo y Libro: Se refleja en el atributo private Libro libro; de la clase Prestamo y su asignación en el constructor this.libro = libro;. Es una relación de agregación estructural donde el objeto Libro persiste dentro del objeto Prestamo.
Dependencia entre Prestamo e Impresora: Se refleja en la firma del método public void devolver(int díasAtrasos, Impresora impresora) donde Impresora entra como parámetro y dentro del cuerpo mediante la llamada impresora.imprimir(comprobante);. Prestamo usa la funcionalidad de Impresora de forma efímera durante la ejecución de ese método sin almacenarla en un atributo de la clase.
2. Tomando en cuenta la regla de contar cada relación una sola vez:
 - Asociaciones: 2 en total (Prestamo -> Socio ; Prestamo -> Libro).
 - Dependencias: 1 en total (Prestamo -> Impresora).
3. No existe una relación directa entre Socio y Libro. Al revisar el código de la clase Socio, esta solo posee el atributo nombre y no conoce a Libro. De igual forma, la clase Libro solo posee el atributo titulo y no referencia a Socio. Ambas clases se vinculan únicamente de forma indirecta a través de la clase intermedia Prestamo, la cual contiene una referencia a cada una de ellas.
4. No aplica el Principio de Responsabilidad única (SRP). Una clase debe de tener una sola razón para cambiar. Actualmente, la clase Prestamo asume múltiples responsabilidades.
 - Gestión del dominio del préstamo: Manejar el estado (devuelve) y las referencias a los involucrados.
 - Lógica de negocio/cálculo financiero: Calcular el monto de la multa en función de los días de atraso.
 - Formateo e impresión de salida: Construir el texto del comprobante y gestionar su envio a la impresora.
Para cumplir con SRP, conviene dividir el diseño en clases con responsabilidades específicas:
 - Prestamo: Mantiene únicamente el estado de préstamo, sus fechas/días y referencias Socio y Libro.
 - CalcularMulta (o método dentro del modelo de reglas del negocio):  Encargado de tomar los días de atraso y determinar el costo económico.
 - GeneradorComprobante: Encargado de estructurar el texto o formato del comprobante a partir del préstamo y llamar a la Impresora para su emisión.
