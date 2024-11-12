
## 1. **Fundamentos de ORM (Mapeo Objeto-Relacional)**

#### JPA
* Es una **especificacion** esto significa que JPA es un conjunto de normas y reglas definidas dentro de la plataforma Java para hacer el mapeo objeto-relacional (ORM) en aplicaciones Java.
* JPA define un conjunto de interfaces y anotaciones que indican cómo las entidades en Java deben comportarse al interactuar con una base de datos.
* Al ser solo una especificación, JPA no tiene una implementación real; solo define el "qué hacer" y no el "cómo hacerlo".
**En que seccion del codigo se ve ?**
	**Anotaciones** (`@Entity`, `@Table`, `@Id`, `@GeneratedValue`)
	**Interfaz `JpaRepository`**
	**Consultas derivadas**: Los métodos como `findByEmail`
#### Hibernate
Es una **implementación de la especificación JPA**, es decir, una biblioteca que pone en práctica las normas definidas en JPA.
- Hibernate también tiene su propio lenguaje de consulta llamado HQL (Hibernate Query Language) y un sistema de caché avanzado, que son adicionales a lo que JPA requiere.
- Puedes usar Hibernate de dos maneras:
    1. Como una implementación JPA (usando solo las funcionalidades definidas por JPA).
    2. O directamente como Hibernate, aprovechando todas las funcionalidades extendidas que ofrece.
**Donde se utiliza ?** 
Aunque en el código no interactúas directamente con Hibernate
	**Implementación de las operaciones :**  Hibernate se encarga de ejecutar las consultas SQL. Maneja internamente las entidades, el mapeo de relaciones, y la sincronización con la base de datos.	
	**Configuración específica de Hibernate**: Cuando defines propiedades como `hibernate.hbm2ddl.aut`.
	**Optimización de rendimiento y características adicionales**: Hibernate aporta características como el uso de cachés de primer y segundo nivel, la generación automática de esquemas, y el manejo de estrategias de `fetching` (carga) para mejorar el rendimiento de la aplicación.
		
	
## 2. **Configuración de Hibernate con Spring Boot**
- **Archivo de Configuración**: (Configuracion basica)
		`spring:`
		  `datasource:`
			`url: jdbc:mysql://localhost:3306/tu_base_datos`
			`username: usuario`
			`password: contraseña`
			`driver-class-name: com.mysql.cj.jdbc.Driver`
		
		  `jpa:`
			`hibernate:`
			  `ddl-auto: update # Opt: none, validate,update,create,create-drop`
			`show-sql: true       # Muestra las consultas SQL en la consola`
			`properties:`
			  `hibernate:`
				`format_sql: true`
				`dialect: org.hibernate.dialect.MySQLDialect`

* datasource : aca se ponene los datos de la base de datos a conectar .
	* **`url`**: Si utilizas Para H2, puedes usar:
		- **`jdbc:h2:mem:testdb`** para una base de datos en memoria (solo durante la ejecución).
		- **`jdbc:h2:file:/path_to_db`** para una base de datos persistente (almacenada en disco).
	* `driver-class-name` en la configuración de Spring Boot define el nombre de la clase del controlador JDBC (driver) que se utiliza para establecer la conexión con la base de datos.
- **`ddl-auto`**: Controla la forma en que Hibernate maneja el esquema de la base de datos.
	**none:** No se realiza ninguna acción en la base de datos.
	**validate :** Valida que el esquema de la base de datos coincida con las entidades (no realiza cambios).
	**update:** Actualiza la base de datos para que coincida con las entidades, sin eliminar datos.
	**create:** Elimina las tablas existentes y las crea de nuevo, perdiendo los datos.
	**create-drop:** Similar a `create`, pero elimina las tablas al cerrar la sesión de la aplicación.
- **`show-sql`** y **`format_sql`**:
    - `show-sql`: Muestra las consultas generadas por Hibernate en la consola.
    - `format_sql`: Mejora la legibilidad de las consultas al formatearlas en varias líneas.
    - `dialect` : cambiar el dialecto en funcion de la base de datos que estes usando.

		
## 3. **Entidades y Anotaciones Básicas de JPA/Hibernate**
`@Entity`
`@Table(name = "producto")`
`public class Producto {`

    `@Id`
    `@GeneratedValue(strategy = GenerationType.IDENTITY)`
    `private Long id;`
    
	@Column(name = "nombre_producto", nullable = false, unique = true) 
	private String nombre;`
`}`
* `@GeneratedValue`:  Define la estrategia para la generación de valores de clave primaria, como `AUTO`, `IDENTITY`, `SEQUENCE`, y `TABLE`.
* `@Column` Se usa para personalizar el mapeo de una columna en la base de datos. Puedes especificar el nombre de la columna, si es única, si permite valores nulos, entre otras propiedades.

## 4. **Gestión de Relaciones y Cardinalidad**

##### Cardinalidad y sus relaciones.
-  **@OneToOne** Define una relación de uno a uno entre dos entidades.
	`@OneToOne(mappedBy = "empleado")` `// Indica que la relación está mapeada en "empleado" en la clase Oficina` `osea que la fk esta en oficina`
	`private Oficina oficina;

	`@OneToOne` 
	`@JoinColumn(name = "empleado_id") // Aquí se define la clave foránea private Empleado empleado;`
		`
- **@OneToMany:** Una entidad está relacionada con varias instancias de otra entidad. 
		-mappedBy , indica que la otra entidad en este caso producto sera el encargado de manejar la clave foranea, ya que de un solo lado queremos tener la FK que una las tablas. 
		En el siguiente ejemplo dice que sera mapeado por categoria, osea que producto tendra la FK de Categoria.
		 `@OneToMany(mappedBy = "categoria") 
		 `private List<Producto> productos;  
- **@ManyToOne:**  Varios objetos de una entidad están relacionados con un único objeto de otra entidad.
	- @JoinColumn , se utiliza para indicicar que es la fk de la otra tabla relacion.
		`@ManyToOne 
		`@JoinColumn(name = "categoria_id")`  `// FK hacia Categoria`
		`private Categoria categoria;`
* @**ManyToMany**, se crear una tabla interbmedia que contenga las Pk de ambas entidades

##### **Cascade**
El uso de `cascade` permite manejar las relaciones entre entidades de manera eficiente y evita tener que gestionar manualmente cada entidad relacionada al realizar operaciones en la entidad principal.
###### Opciones comunes de `cascade`:
- **`CascadeType.PERSIST`**: Propaga la operación de guardado.
- **`CascadeType.MERGE`**: Propaga la operación de actualización.
- **`CascadeType.REMOVE`**: Propaga la operación de eliminación.
- **`CascadeType.REFRESH`**: Propaga la operación de sincronización.
- **`CascadeType.DETACH`**: Propaga la desvinculación de la sesión de persistencia.
- **`CascadeType.ALL`**: Propaga todas las operaciones anteriores.
Ejemplo:
	`@Entity`
	`public class Departamento {`
	    `@Id`
	    `private Long id;`
	
	    `@OneToMany(cascade = CascadeType.REMOVE, mappedBy = "departamento")`
	    `private List<Empleado> empleados;`
	`}`
	
	`@Entity`
	`public class Empleado {`
	    `@Id`
	    `private Long id;`
	
	    `@ManyToOne`
	    `@JoinColumn(name = "departamento_id")`
	    `private Departamento departamento;`
	`}`
Si eliminas un `Departamento`, todos los `Empleados` asociados se eliminarán automáticamente debido al `cascade = CascadeType.REMOVE`.
	`****`
##### **Estrategias de Carga (Fetching)**
###### 1. **`LAZY` (Carga Perezosa)**
Con `LAZY`, la entidad relacionada **no se carga automáticamente** cuando se recupera la entidad principal. En cambio, solo se carga cuando realmente accedemos a esa relación. Esto significa que Hibernate diferirá la carga de datos hasta que se necesiten, lo que puede optimizar la aplicación al reducir la cantidad de datos recuperados inicialmente.

- **Ventaja**: Ahorra memoria y tiempo de procesamiento al cargar solo las relaciones que se necesitan.
- **Inconveniente**: Si se accede a varias relaciones `LAZY` en un bucle o después de que la sesión de Hibernate haya cerrado, puede provocar errores o múltiples consultas a la base de datos (problema N+1).
**Ejemplo de uso**:
*   `class Producto {`
	 `@ManyToOne(fetch = FetchType.LAZY) 
	`private Categoria categoria;`
	`}`
	La categoría de un producto solo se carga cuando se accede a ella explícitamente. por ejemplo utilziando producto.getCategoria()
	`
* `class Categoria{`
	`@OneToMany(mappedBy = "categoria", cascade = CascadeType.ALL, fetch = FetchType.LAZY) 
    `private List<Producto> productos;``
	`}`
   Los productos de una categoría solo se cargan cuando se accede a ellos. utilizando categoria.getProductos()`.

###### 2. **`EAGER` (Carga Ansiosa)**
La diferncia es que las relaciones EAGER  hace que a la hora de consultar por la entidad principal te devuelva los datos de sus relaciones EAGER.
La entidad relacionada **se carga inmediatamente** junto con la entidad principal. Cuando recuperas una entidad con una relación EAGER
- **Ventaja**: Evita problemas de carga perezosa (como el problema N+1 o errores de sesión cerrada) y es útil cuando siempre se necesitan las relaciones relacionadas.
- **Inconveniente**: Carga todos los datos relacionados de inmediato, lo que puede afectar el rendimiento y la memoria si no todas las relaciones son necesarias.
**Ejemplo de uso**:
	 `class Producto {`
	`@OneToOne(fetch = FetchType.EAGER) 
	`private DetalleProducto detalle;`
	`}`
   Carga el `DetalleProducto` junto con el `Producto` inmediatamente a la hora de obtener un cosnulta del producto , evitando consultas adicionales.


MA
- Configuración de relaciones entre entidades (`@OneToOne`, `@OneToMany`, `@ManyToOne`, `@ManyToMany`) y cómo funciona el `cascade` y `fetch`.
## 5. **Optimización y Rendimiento**
#### Cache en hibernate
###### Cache de primer nivel(L1):
Es un cache interno de hibernate, Hibernate guarda temporalmente las entidades que ya se han consultado y las reutiliza, evitando consultas reduntdantes a la BD.
* Por defecto viene activado
* Alcance limitado : Solo esta disponiblee dentro de la misma sesion. Una vez cerrada los datos se eleminan.
`Producto producto1 = productoRepository.findById(1L); // Va a la base de datos`

`// Segunda vez que pides el mismo producto`
`Producto producto2 = productoRepository.findById(1L); // Usará el caché L1`
###### Cache de segundo nivel(L2):
Este cache es mas global, pueden ser utilizadas en multiples sesiones. Permite ahorrar recursos al reducir las consultas a la BD.

* Debe configurarse explicitamente
* Hibernate necesita herramientas externas especializadas como Ehcache, Infinispan, o Redis.
Ejemplo configurando hibernate para L2 y usando Ehcache :
`spring:`
  `jpa:`
    `properties:`
      `hibernate:`
        `cache:`
          `use_second_level_cache: true`  `# Habilita el cache de segundo nivel`
          `use_query_cache: true`  `# Habilita el cache de **consultas**`
          `region:`
		    `# Utiliza Ehcache`
            `factory_class: org.hibernate.cache.ehcache.EhCacheRegionFactory`

Ejemplo de uso : 
    `@Transactional` `// recomendable usarlo para que no haya inconsistencias`
    `public Producto obtenerProducto(Long id) {`
`//Hibernate buscará en el caché de segundo nivel o despues irá a la base de datos`
        `Optional<Producto> productoOpt = productoRepository.findById(id);`
        `//resto codigo`
    }


#### Problema de N+1 y uso de JOIN FETCH 
Cuando tienes una relación entre entidades (por ejemplo, una relación entre **`Autor`** y **`Libro`**), si esa relación está configurada como **`LAZY`** (carga diferida), Hibernate no cargará automáticamente los datos relacionados cuando consultes el **`Autor`**. En su lugar, Hibernate solo cargará los libros asociados cuando accedas explícitamente a la propiedad **`libros`**. Esto puede ser ineficiente, ya que podría generar muchas consultas SQL adicionales (una para cada entidad relacionada), lo cual es lo que se conoce como **N+1 consultas**.

Ejemplo Practico:
1- **Entidad `Autor`**: Tiene una relación **OneToMany** con la entidad `Libro`.
	`class Autor{` 
	`@OneToMany(mappedBy = "autor", fetch = FetchType.LAZY)` 
	`private List<Libro> libros;`
	`}`
2-**Entidad `Libro`**: Representa los libros escritos por los autores.
		`@ManyToOne(fetch = FetchType.LAZY)` 
		`@JoinColumn(name = "autor_id")` 
		`private Autor autor;`
----		

`List<Autor> autores = autorRepository.findAll();`
Bueno como tenes fetch = Lazy en la relacion ahi no te traera los datos de Libro.
Para no tener que hacer dos consultas, puedes utilizar un **`JOIN FETCH`** para optimizar la consulta y asi traerto la informacion de los libros junto con el autor.

	`public interface AutorRepository extends JpaRepository<Autor, Long> {`
	    `@Query("SELECT a FROM Autor a JOIN FETCH a.libros")`
	    `List<Autor> findAllAutoresConLibros();`
	`}`
###### "Observacion":
* Podrias utilizar en vez de **`LAZY`** , **`EAGER`**  y obtendiras el mismo resultado ,pero utilizando **`JOIN FETCH`** puede hacer que solo para esta consulta te traiga los datos de la relacion por lo que es productivo si es que no en todas las consultas te interezaria traerte las informacion de la entidad relacion .

## 6. **Consultas 

- Entiende las diferencias entre JPQL, HQL y SQL nativo, y cuándo es mejor usar cada uno.
##### JPQL 
**Java Persistence Query Language (JPQL)** es un lenguaje de consultas orientado a objetos similar a SQL, pero trabaja con las entidades y sus atributos en lugar de tablas y columnas.
	`@Query("SELECT a FROM Autor a WHERE a.nombre = :nombre")`
	`List<Autor> findByNombre(@Param("nombre") String nombre);`
##### **Query Methods de Spring Data JPA**:	
Son métodos que se nombran de acuerdo a ciertas convenciones (como `findByNombre`), y Spring genera automáticamente las consultas correspondientes.
-Algunas de las palabras clave más comunes para las firmas de los metodos:
- `And`, `Or`: Para combinar condiciones
- `Is`, `Equals`: Para igualdad
- `Between`: Para rangos
- `LessThan`, `GreaterThan`, `Before`, `After`: Para comparaciones
- `Like`, `Containing`, `StartingWith`, `EndingWith`: Para búsquedas de texto
- `OrderBy`: Para ordenar los resultados

Ejemplo completo : Si tenés una entidad `Producto` y querés encontrar productos cuyo precio sea mayor que un valor y ordenados por nombre, podés definir en el repositorio:
	`List<Producto> findByPrecioGreaterThanOrderByNombreAsc(double precio);` 
##### SQL Nativo
**Consultas SQL nativas** te permiten escribir consultas directas utilizando el lenguaje SQL que tu base de datos entiende. Esto es útil cuando necesitas optimizar el rendimiento o acceder a funciones específicas de la base de datos.
`@Query(value = "SELECT * FROM autor WHERE nombre = :nombre", nativeQuery = true)`
`List<Autor> findByNombreSQL(@Param("nombre") String nombre);`

- Para el mapeo de resultados con sql a una entidad en particular se utiliza `@SqlResultSetMapping`
##### Criteria API para Consultas Programáticas
La **Criteria API** te permite construir consultas de manera programática. Es útil cuando necesitas consultas dinámicas o cuando las consultas son complejas y dependen de la entrada del usuario.
`// Crear un CriteriaBuilder`
`CriteriaBuilder cb = entityManager.getCriteriaBuilder();`

`// Crear una CriteriaQuery para el tipo de entidad Autor`
`CriteriaQuery<Autor> cq = cb.createQuery(Autor.class);`

`// Definir el root de la consulta (la entidad desde la que se hace la consulta)`
`Root<Autor> autor = cq.from(Autor.class);`

`// Crear la expresión de la condición (autor.nombre = :nombre)`
`Predicate nombrePredicate = cb.equal(autor.get("nombre"), "Juan");`

`// Aplicar la condición a la consulta`
`cq.where(nombrePredicate);`

`// Ejecutar la consulta`
`List<Autor> autores = entityManager.createQuery(cq).getResultList();`
## 7. **Aspectos Avanzados de Mapeo y Herencia**
#### **Mapeo de Herencia en Entidades**

JPA proporciona varias estrategias de herencia, y puedes elegir la adecuada según el diseño de tu base de datos y los requisitos de tu aplicación. Para definir la herencia en JPA:

##### a. **Anotación `@Inheritance`**
Esta anotación en la clase padre define la estrategia de herencia a usar. Existen tres estrategias principales:
1. **SINGLE_TABLE**: Todas las clases de la jerarquía de herencia se almacenan en una sola tabla.
2. **TABLE_PER_CLASS**: Cada clase de la jerarquía tiene su propia tabla.
3. **JOINED**: Crea una tabla separada para cada clase y establece relaciones entre las tablas.
##### b. **Anotación `@DiscriminatorColumn`**

Para las estrategias `SINGLE_TABLE` y `JOINED`, se usa esta anotación para identificar el tipo de cada registro. Esto permite que JPA sepa a qué clase concreta pertenece cada fila en la tabla.

**Ejemplo de ambas anotaciones en uso** (@Inheritance, @DiscriminatorColumn):
	`@Entity`
	`@Inheritance(strategy = InheritanceType.SINGLE_TABLE)`
	`@DiscriminatorColumn(name = "tipo_empleado", discriminatorType = DiscriminatorType.STRING)`
	`public abstract class Empleado {`
		`@Id`
		`private Long id;`
		`private String nombre;`
	`}`
	
	`@Entity`
	`@DiscriminatorValue("GERENTE")`
	`public class Gerente extends Empleado {}`
	
	`@Entity`
	`@DiscriminatorValue("DESARROLLADOR")`
	`public class Desarrollador extends Empleado {}`

#### Usos de @Formula y @Subselect
##### **Anotación `@Formula`**
Esta anotación permite calcular un valor en la entidad sin almacenarlo directamente en la base de datos. Se usa para valores calculados derivados de otros campos.

`@Entity` 
`public class Producto {`  
	`@Id     
	`private Long id;`     
	`private double precioUnitario;`     
	`private int cantidad;`    
	      
	`@Formula("precio_unitario * cantidad")`     
	`private double precioTotal;     // Getters y setters` 
`}`

En este ejemplo, `precioTotal` no es una columna real en la base de datos. En su lugar, se calcula multiplicando `precio_unitario` por `cantidad` cada vez que se carga la entidad.

##### **Anotación `@Subselect`**

`@Subselect` se utiliza para mapear una entidad a una vista o subconsulta compleja en lugar de a una tabla. Es útil cuando necesitas realizar consultas SQL avanzadas sin crear una vista en la base de datos.
	`@Entity` 
	`@Subselect("SELECT p.id, p.nombre, p.precio_unitario * p.cantidad AS precio_total FROM producto p")` 
	`public class ProductoResumen {` 
		`@Id`    
		`private Long id;`  
		`private String nombre;`   
		`private double precioTotal;`   
	`}`

Aquí, `ProductoResumen` es una entidad que se mapea a una consulta SQL en lugar de una tabla. La consulta calcula `precioTotal` en la base de datos y lo asigna al campo correspondiente en la entidad.

## 8. **Transacciones y Concurrencia**
#### Transacciones
##### Propagacion 
La propagación define cómo una transacción actual se comportará cuando un método marcado como `@Transactional` llame a otro método que también tiene esta anotación. Spring ofrece varias opciones para configurar la propagación de transacciones:
- **REQUIRED** (valor por defecto): Si hay una transacción activa, el método se ejecuta dentro de ella; si no hay una transacción activa, crea una nueva.
- **REQUIRES_NEW**: Siempre crea una nueva transacción y suspende cualquier transacción existente. Esto es útil cuando se necesita asegurar que un bloque de código se ejecute en una transacción independiente.
- **NESTED**: Crea una sub-transacción dentro de una transacción existente. Solo es efectivo con bases de datos que soportan sub-transacciones.
- **SUPPORTS**: Ejecuta el método dentro de una transacción si ya hay una, pero no creará una nueva si no existe.
- **NOT_SUPPORTED**: Siempre suspende cualquier transacción existente y ejecuta el método fuera de una transacción.
- **MANDATORY**: Requiere que exista una transacción; si no hay una transacción, lanza una excepción.
- **NEVER**: El método nunca debe ejecutarse dentro de una transacción; si hay una transacción, lanza una excepción.
`@Transactional(propagation = Propagation.REQUIRES_NEW)`
`public void metodoConNuevaTransaccion() {`
	`// Operaciones en una transacción nueva`
`}`
##### Niveles de Isolations
El aislamiento controla cómo se ve el estado de la base de datos desde una transacción en curso, y cómo interactúan las transacciones entre sí. Cada nivel de aislamiento aborda ciertos problemas de concurrencia:

- **READ_UNCOMMITTED**: -Leer datos que aún no se han guardado (confirmado) en la base de datos por otra transacción.
- **READ_COMMITTED**:Solo puedes ver datos que otras transacciones ya han confirmado.
- **REPEATABLE_READ**: Garantiza que si lees algo una vez en tu transacción, seguirá igual hasta que termines, evitando las **lecturas no repetibles**.
- **SERIALIZABLE**: Trata cada transacción como si fuera la única que se ejecuta en ese momento, eliminando todos los problemas de concurrencia.
`@Transactional(isolation = Isolation.REPEATABLE_READ)`
`public void metodoConAislamiento() {`
    `// Operaciones con nivel de aislamiento REPEATABLE_READ`
`}`

## 10. **Auditoría y Validación**
##### Validación de Datos con Bean Validation y Hibernate Validator
La validación permite asegurarse de que los datos de una entidad cumplen con ciertos requisitos antes de almacenarse. Con Bean Validation (API `javax.validation`) y Hibernate Validator, puedes definir reglas de validación directamente en los atributos de las entidades mediante anotaciones

`class Producto {`
    `@NotNull(message = "El nombre no puede ser nulo")`
    `@Size(min = 2, max = 100, message = "El nombre debe tener entre 2 y 100 caracteres")`
    `private String nombre;`
`}`

Spring Boot valida automáticamente las entidades antes de guardarlas si se añaden las anotaciones de validación. En los controladores, puedes forzar la validación de los parámetros de entrada añadiendo `@Valid` en el método:

`@RestController`
`@RequestMapping("/productos")`
`public class ProductoController {`
    `@PostMapping`
    `public Producto crearProducto(@Valid @RequestBody Producto producto) {`
        `// Si el producto no es válido, Spring lanzará una excepción`
        `return productoService.guardar(producto);`
    `}`
`}`
## 10. **Migraciones de Bases de Datos**
- Uso de herramientas de migración de esquemas de base de datos como Flyway o Liquibase junto con Hibernate.
- Gestión de los cambios de esquema a medida que el modelo de datos evoluciona en el tiempo.