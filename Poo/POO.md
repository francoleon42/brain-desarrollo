
### Conceptos base de programación orientado a objetos

## 1.Principios SOLID

**S**→ una sola responsabilidad por clase

**O** → Abierto a expansión pero no a modificación , agregar funciones sin cambiar el comportamiento.

**L** → Las Subclases deben de poder reemplazar todo lo que haga la clase padre , regla 100%


**I** → Las interfaces deben de ser lo más específicas posibles y no generales.
Explicación : este principio sugiere que es mejor tener muchas interfaces específicas y pequeñas en lugar de una sola interfaz general y grande. Los clientes de la interfaz solo deberían conocer los métodos que realmente utilizan y no verse obligados a implementar métodos que no les son necesarios.

**D →** Inversión de dependencia las clases de alto nivel no deben de depender de clases inferiores , sino de abstracciones o interfaces.

Explicación: básicamente es implementar abstracciones con las funcionalidades necesarias  y  depender de esa interfaz para utilizar sus métodos.

Después al utilizar el método de esa interfaz , se ejecutará en  la forma que las clase que implementa esa interfaz indicaron.

##   2. Clases y Objetos

- **Clase:** Una plantilla para crear objetos. Define un tipo de objeto según sus propiedades y métodos.
    
- **Objeto:** Una instancia de una clase. Los objetos son entidades concretas que se crean utilizando la definición de clase.
    
- Para saber  si es una clase se debe de pensar si tiene comportamiento(funciones) o estado(atributos ) propio.

## 3. Cohesión y  Acoplamiento
* **Acoplamiento**: Se refiere al grado de dependencia entre dos o mas clases. Posible solución con inversión de dependencias(Soli**d**) Ejemplo:
`interface Cliente {`
    `String getNombre();`
`}`
`// Clase Pedido depende de la interfaz Cliente en vez de la clase específica`
`class Pedido {`
    `private Cliente cliente;`

    `public Pedido(Cliente cliente) {`
        `this.cliente = cliente;`
    `}`

    `public void procesarPedido() {`
        `System.out.println("Procesando pedido para " + cliente.getNombre());`
    `}`
`}`
`class ClienteOnline implements Cliente {`
    `private String nombre;`

    `public ClienteOnline(String nombre) {`
        `this.nombre = nombre;`
    `}`

    `@Override`
    `public String getNombre() {`
        `return nombre;`
    `}`
`}`
* Cohesión : Se refiere al grado en que las responsabilidades de una clase están relacionadas entre sí. Es un principio que mide qué tan enfocada está una clase en cumplir con una tarea específica o representar una entidad bien definida.
* 
En diseño de software, se busca **alta cohesión** y **bajo acoplamiento**. Esto significa que cada clase debe enfocarse en una única responsabilidad (alta cohesión) y estar lo menos dependiente posible de otras clases (bajo acoplamiento).
De otra manera seria la **S  y D** de principios SOLID. 

## 4. Herencia [[Herencia]]
## 5. Polimorfismo
- **Definición:** La capacidad de los objetos de diferentes clases de ser tratados como objetos de una clase común. Es más comúnmente aplicado a través de interfaces y herencia.
###### **Hay dos formas de polimorfismo:**     
- **Sobrecarga de métodos:** Permite definir múltiples métodos con el mismo nombre en la misma clase pero con diferentes parámetros.
    
- **Sobreescritura de métodos:** Una subclase puede proporcionar una implementación específica de un método ya definido en su superclase. 
  Esto permite a las subclases comportarse de manera diferente al ser invocadas a través de una referencia de la clase base.
  Ejemplo de sobreescritura: 
	`class Animal {`
		`public void hacerSonido() {`
			`System.out.println("El animal hace un sonido");`
		`}`
	`}`
	`class Perro extends Animal {`
		`@Override`
		`public void hacerSonido() {`
			`System.out.println("El perro ladra");`
		`}`
	`}`
	`class Gato extends Animal {`
		`@Override`
		`public void hacerSonido() {`
			`System.out.println("El gato maúlla");`
		`}`
	`}`
## 6. Encapsulamiento
El encapsulamiento es un principio de POO que consiste en restringir el acceso a los atributos y métodos de un objeto, protegiendo así el estado interno del objeto y evitando modificaciones no controladas.
- **Modificadores de Acceso**:
    - **`private`**: Solo accesible dentro de la misma clase.
    - **`protected`**: Accesible dentro de la clase y sus subclases.
    - **`public`**: Accesible desde cualquier lugar.
	- **`package-private`** (sin modificador): Accesible solo dentro del mismo paquete.
*  **Getters y Setters**:
## 7. Abstracción
- **Definición**: La abstracción es un principio de POO que consiste en ocultar los detalles de implementación y exponer solo las características relevantes de un objeto. Permite centrarse en lo que un objeto hace en lugar de cómo lo hace.
###### **Maneras de realizarlo**
- Clases abstractas: Clases que no pueden ser instanciadas por sí mismas y están destinadas a ser subclases.
    
- Interfaces: Definen métodos que una clase debe implementar sin proporcionar la implantación del método.
###### **Encapsulamiento vs Abstracción**:
 Aunque ambos conceptos están relacionados, el encapsulamiento se centra en proteger el estado interno de un objeto, mientras que la abstracción se refiere a ocultar detalles innecesarios y mostrar solo la funcionalidad relevante al usuario.

## 8. Relaciones entre Clases
###### **Tipos de relaciones de manera conceptual**
- ***Asociación:*** Una relación donde todos los objetos tienen su propio ciclo de vida y no hay un propietario. 
- ***Agregación:*** Una forma de asociación con una relación de "todo/parte" donde las partes pueden existir independientemente del todo.
    **Ejemplo**: `class Clase { private Estudiante estudiante; }` donde un `Estudiante` puede existir sin estar asociado a una `Clase`.
- ***Composición:*** Una forma más fuerte de agregación donde las partes no pueden existir independientemente del todo.Comparten el ciclo de vida
    **Ejemplo**: `class Auto { private Motor motor; }` donde `Auto` tiene un `Motor`.
* ***Dependencia funcional*** : Se refiere a la relación donde una clase necesita otra para funcionar, pero no tiene una relación de posesión.
	**Ejemplo**: Un objeto `Cliente` que necesita un `Pedido` para realizar una compra.
**Cardinalidad** : 
 * una a muchos 
 * uno a uno 
 * muchos a muchos 

  
## 9.  Excepciones
Las excepciones en Java representan errores o eventos inusuales que ocurren durante la ejecución de un programa.
#### Jerarquía
![[Pasted image 20241029181415.png]]
#### Ckeded  y  unckecked  Exceptions
 ######  **Checked Exceptions**: 
 Son verificadas en tiempo de compilación, deben capturarse o declararse en el método . Heredan de `Exception`, pero **no de `RuntimeException`**.
	 - ==Deben ser declaradas en la firma del método usando throws o bien capturadas con un bloque try-catch==. Esto se debe a que el compilador las verifica en tiempo de compilación y obliga al desarrollador a manejarlas
	 
###### **Unchecked Exceptions**: 
 No son verificadas en compilación, no requieren ser capturadas o declaradas , Se derivan de la clase Runtime Exception  , estas incluyen: 
    - **ArithmeticException**: Ocurre en errores aritméticos, como dividir por cero.
    - **IndexOutOfBoundsException**: Ocurre cuando se accede a un índice inválido en una lista o array.
    - **IllegalArgumentException**: Se lanza cuando un argumento pasado a un método es inapropiado.
    - **NumberFormatException**: Ocurre cuando se intenta convertir una cadena a un número en un formato inválido.
    - **NullPointerException**: Se lanza cuando el programa intenta acceder a un objeto que es `null`.
Fijese que ==no hace falta poner en la firma del metodo throws  ni usar un try-catch== , ya que al ser unchecked el compilador no obliga a que las capturemos : 
	`public void dividir(int a, int b) {`
		`if (b == 0) {`
		`throw new ArithmeticException("División por cero no permitida");`
		`}`
	`}`


#### Creación de excepciones
-**Con bloques :**  `try`, `catch`, `finally`
- **try**: Define un bloque de código donde se puede lanzar una excepción.
- **catch**: Captura la excepción y permite manejarla. Se puede tener múltiples bloques `catch` para diferentes tipos de excepción.
- **finally**: Se ejecuta siempre, independientemente de si ocurre o no una excepción, útil para liberar recursos como archivos o conexiones de base de datos.
		`try {`
		    `int result = 10 / 0;`
		`} catch (ArithmeticException e) {`
		    `System.out.println("No se puede dividir por cero");`
		`} finally {`
		    `System.out.println("Finalización del bloque try-catch");`
		`}`

**-Creación de excepciones Personalizadas**: Java permite crear excepciones personalizadas para casos específicos en la lógica de negocio. Las excepciones personalizadas deben extender de Exception o RuntimeException, Las excepciones personalizadas deben extender `Exception` (si es checked) o `RuntimeException` (si es unchecked).
Ejemplo con checked:
		`public class MiExcepcionPersonalizada extends Exception {`
		    `public MiExcepcionPersonalizada(String mensaje) {`
		        `super(mensaje);`
		    `}`
		`}`
			
		  // Uso:
	    public void verificarEdad(int edad) throws MiExcepcionPersonalizada {
	        if (edad < 18) {
	            throw new MiExcepcionPersonalizada("Debe ser mayor de 18 años.");
	        }
	    }
}


## 10 . Collections[[Collections]]



## 11. Métodos Estáticos y Variables Estáticas
- Métodos Estáticos: Métodos que pertenecen a la clase en lugar de a instancias individuales. Se llaman usando el nombre de la clase.

- Variables Estáticas: Variables que pertenecen a la clase y se comparten entre todas las instancias de la clase.
    
Los métodos y variables estáticas funcionan diferente a los métodos y variables de instancia porque pertenecen a la **clase en sí misma**, no a objetos específicos de la clase. Aquí te explico cómo es posible que se comporten de esta manera:

1. **Almacenamiento en memoria**:
    
    - Las variables y métodos estáticos se almacenan en una parte especial de la memoria llamada **área de clase o área de método**. Esto significa que solo existe **una copia** de cada variable o método estático para toda la aplicación, independientemente del número de instancias que se creen de la clase.
    - Por eso, al declarar una variable como `static`, esa variable será compartida y accesible para todos los objetos de la clase, ya que todos los objetos "miran" a la misma referencia en memoria.
2. **Acceso a métodos y variables estáticos**:
    
    - Dado que los métodos estáticos pertenecen a la clase, no requieren una instancia para ser llamados. Puedes llamarlos directamente con `NombreDeLaClase.metodoEstatico()`.
    - Los métodos estáticos no tienen acceso a variables de instancia, ya que estas están asociadas a un objeto específico. Esto evita que un método estático dependa de instancias, y así se puede llamar directamente desde la clase sin necesidad de crear objetos.
