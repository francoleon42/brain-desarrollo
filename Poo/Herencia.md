- **Definición**: La herencia es un mecanismo de la POO que permite que una clase (subclase o clase hija) herede atributos y métodos de otra clase (superclase o clase base).
- **Objetivo**: Reutilizar el código y modelar relaciones jerárquicas entre clases. La herencia permite extender el comportamiento de las clases y evitar la duplicación de código

- También utilizado si son estados diferentes de algo, donde cada estado tiene un comportamiento  a parte. 
	- Como en el patrón de diseño State [[Patron State]]

###### **Modificadores de Acceso y Herencia** :  
Los miembros `public` y `protected` de la clase base son accesibles desde las subclases, mientras que los `private` no lo son.
**`super`**: Permite acceder a los miembros (atributos y métodos) de la clase base desde una subclase.
###### **Sobreescritura de Métodos**  : 
La sobreescritura (override) permite a la subclase redefinir un método de la clase base. Esto es fundamental para polimorfismo, donde un método común tiene diferentes implementaciones en distintas clases.
###### **Constructores en Herencia** :
Las subclases pueden invocar el constructor de la clase base usando `super`. Si no se especifica, se llama al constructor sin parámetros por defecto.Primero se llama al constructor de la superclase y luego al de la subclase.
		`class Animal {`
		    `public Animal() {`
		        `System.out.println("Constructor de Animal");`
			 `}`
		`}`

		`class Perro extends Animal {`
			    `public Perro() {`
			        `super();  // Invoca el constructor de la superclase`
			        `System.out.println("Constructor de Perro");`
			     `}`
		  `}`
###### **Polimorfismo en Herencia** : Con herencia, el polimorfismo permite que una referencia de la clase padre apunte a un objeto de la subclase.
	`Animal miAnimal = new Perro();  // Permite tratar al Perro como un Animal`
	`miAnimal.hacerSonido();  // Llamará a "hacerSonido" de Perro`
	
###### **Palabra Clave `final` en Herencia**: 
- **Clases `final`**: Al declarar una clase como `final`, estás impidiendo que cualquier otra clase la extienda. Esto es útil para prevenir modificaciones a la clase, protegiendo su comportamiento.
- **Métodos `final`**: Si declaras un método como `final`, las subclases no pueden sobrescribirlo. Esto asegura que un método específico tenga el mismo comportamiento en todas las clases que lo heredan, útil cuando necesitas garantizar que la funcionalidad no cambie.
	`final class Utilidades {`
	    `public static void imprimirMensaje(String mensaje) {`
	        `System.out.println(mensaje);`
	    `}`
	`}`
	`class Vehiculo {`
	    `public final void arrancar() {`
	        `System.out.println("El vehículo arranca.");`
	    `}`
	`}`
	`class Coche extends Vehiculo {`
		    `// Este método no puede sobrescribirse debido a "final" en arrancar`
	`}`

###### **Problemas de Herencia (Fragilidad de herencia y Sobrecarga de Subclases)**: 
- **Fragilidad de Herencia**: Cuando una clase base cambia, todas las subclases pueden verse afectadas. Si una jerarquía de herencia es demasiado profunda o compleja, un cambio en la clase base puede romper o complicar el comportamiento de múltiples subclases, lo que hace el código difícil de mantener.
- **Fragilidad de sobrecarga de Subclases**: Cuando una clase base tiene demasiada funcionalidad, las subclases heredan comportamiento que tal vez no necesiten. Esto viola el principio de **"una clase debe tener una única responsabilidad"** y puede dificultar la reutilización del código.
**Cómo Evitarlo**: Limitar la jerarquía de herencia y optar por composición en lugar de herencia en muchos casos. Usar herencia solo cuando realmente existe una relación "es un".

##### Interfaces vs Clases Abstractas
- **Interfaces:** ==*Permiten la implementación múltiple*== (una clase puede implementar múltiples interfaces). Todos los métodos de una interfaz son abstractos.
    
- **Clases Abstractas:** Son clases que no se pueden instanciar. Pueden tener métodos abstractos y no abstractos. No permiten la implementación múltiple, pero permiten heredar una estructura base.
##### **Herencia Múltiple y Composición**
**Def** Es cuando una clase hereda de múltiples clases base. 
**Simulación de Herencia Múltiple en Java**: Java permite implementar múltiples interfaces, proporcionando así una forma de herencia múltiple en términos de comportamiento. Las interfaces definen métodos sin implementación, y la clase que las implementa debe definir esos métodos.
* ***Problema del Diamante:** Este problema ocurre cuando una clase deriva de dos clases base que tienen una clase ancestro común, lo que puede generar ambigüedad en la herencia de métodos y atributos.
  * Ejemplo : Cuando creas una instancia de la clase D, el compilador se enfrenta a la siguiente ambigüedad: ¿Qué implementación del método (o atributo) de A debería usar D? La de B o la de C? Esto se debe a que D hereda de ambas y no hay una única ruta clara hacia A
			   ` A `
		      `/   \`
		     `B    C`
		      `\   /`
		       ` D`