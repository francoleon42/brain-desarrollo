##### **Que es ?**
La Inyección de **Dependencias es un patrón de diseño** que permite a los objetos recibir (o inyectar) sus dependencias en lugar de crearlas ellos mismos. Esto facilita el mantenimiento, la prueba, y el cambio de configuraciones, además de hacer el código más modular. Spring gestiona estas dependencias automáticamente, permitiendo que la configuración y los objetos se carguen solo cuando son necesarios.

##### *Relacion con Inversión de Control LoC *
**Inversión de Control (IoC)** es un **principio de diseño** . Es una idea general que sugiere que el control de ciertos aspectos del programa (como la creación de objetos y la gestión de dependencias) debe transferirse desde el código de la aplicación a un framework o contenedor. 
En IoC, en lugar de que la aplicación "controle" operaciones como creacion de objetos o gestion de dependencias ,hacer que un contenedor (como el contenedor IoC de Spring) asuma esa responsabilidad, permitiendo que la aplicación solo se enfoque en su lógica principal.
- **IoC** es el **principio** en el que se basa **DI**
- **DI** es el **patrón** que permite implementar IoC 
##### **¿Por qué es importante?**
* ***Desacoplamiento**:La DI permite que los componentes de tu aplicación estén menos acoplados entre sí.
- **Facilidad para las pruebas**: Permite el uso de mocks y objetos simulados en pruebas, sin necesidad de depender de implementaciones reales.
- **Facilita la gestión de dependencias**:  Gestiona automáticamente el ciclo de vida de los objetos, lo que simplifica la creación y destrucción de instancias
##### **Como funciona en spring ?** 
**Tipos de inyecciones**
- **Por Constructor**: Inyecta dependencias mediante el constructor de la clase. Si utilizas Lombok , se puede utlizar `RequiredArgsConstructor`.
- **Por Setter**: Inyecta dependencias mediante métodos setter.
- **En las variables de instancia**: Utiliza anotaciones directamente en los campos de la clase. Aunque no es la opción más recomendada, es simple y muy común en proyectos donde se busca minimizar el código boilerplate.
**Anotaciones**
- `@Autowired`  , es el que se utiliza para hacer la inyección de dependencias .
- *`@Component`, `@Service`, `@Repository`, `@Controller`: Declaran que una clase es un componente administrado por Spring y está disponible para ser inyectada.
- `@Qualifier`: Se usa cuando tienes múltiples implementaciones de una interfaz y necesitas especificar cuál deseas inyectar. (Otra manera A veces, es útil designar una implementación por defecto usando `@Primary`)
  Ejemplo: 
	  Digamos que tenemos muchas implementaciones de PaymentService y una es 
		paypalPaymentService  deberíamos de aclarar utilizando @Qualifier.
	`@Service`
	`@Qualifier("paypalPaymentService")`
	`public class PayPalPaymentService implements PaymentService {`
	    `@Override`
	    `public void processPayment(double amount) {`
	        `System.out.println("Processing PayPal payment of $" + amount);`
	    `}`
	`}`
	Y para inyectar una implementación específica en una clase, utilizamos `@Qualifier` junto con `@Autowired`:
	
	`@Component`
	`public class PaymentProcessor {`	
	    `private final PaymentService paymentService;`

	    @Autowired
	    public PaymentProcessor(@Qualifier("paypalPaymentService") PaymentService paymentService) {
	        this.paymentService = paymentService;
	    }
	`}`

##### **Configurar Perfiles**
Podes configurar para que beans solo se carguen en determinados perfiles que tengas en los archivoss application
- `application-dev.properties` o `application-dev.yml`
- `application-prod.properties` o `application-prod.yml`

`@Service`
`@Profile("dev") // Este bean solo se cargará en el perfil "dev"`
`public class ServicioNotificacionDev implements ServicioNotificacion {`
    `@Override`
    `public void enviarMensaje(String mensaje) {`
        `System.out.println("Enviando mensaje en entorno de desarrollo: " + mensaje);`
    `}`
`}`

`@Service`
`@Profile("prod") // Este bean solo se cargará en el perfil "prod"`
`public class ServicioNotificacionProd implements ServicioNotificacion {`
    `@Override`
    `public void enviarMensaje(String mensaje) {`
        `// Implementación para producción`
        `System.out.println("Enviando mensaje en entorno de producción: " + mensaje);`
    `}`
`}`

##### **Etapas del ciclo de vida de un Bean:** 
* **Creación de bean** 
* **Inicializacion :** Después de que se crea el bean, se inicializa. Esto implica que se configuran las propiedades del bean y se realizan tareas adicionales de configuración.
	* **`@PostConstruct`**: Si el bean tiene este método anotado, se invocará después de que todas las propiedades se hayan establecido.
	* **Implementación de `InitializingBean`**: Si el bean implementa esta interfaz, el método `afterPropertiesSet()` se invocará para la inicialización.
* **Uso en el tiempo de ejecución**
* **Destrucción** : Cuando el contexto de la aplicación se cierra o el bean ya no se necesita, se destruye. Es importante liberar recursos y realizar cualquier limpieza necesaria.
	* **`@PreDestroy`**: Si el bean tiene este método anotado, se invocará antes de que se destruya el bean.
	- **Método de destrucción**: Puedes definir un método personalizado para la destrucción del bean, que se puede especificar en la configuración del bean.
	- **Implementación de `DisposableBean`**: Si el bean implementa esta interfaz, el método `destroy()` se invocará antes de la destrucción.
##### **Distintos ciclos de vida de Beans**
**_— Singleton_**: Cuando definimos un bean con el alcance singleton, el contenedor crea una sola instancia de ese bean; todas las solicitudes de ese nombre de bean devolverán el mismo objeto, que se almacena en caché. Cualquier modificación al objeto se reflejará en todas las referencias al bean. Este ámbito es el valor predeterminado si no se especifica ningún otro ámbito.
— **_Prototype_**: se crea una nueva instancia cada vez que se solicita el bean. A diferencia de los beans de alcance singleton, Spring no gestiona el ciclo de vida de los beans de alcance prototype más allá de su creación. Esto significa que tú eres responsable de liberar cualquier recurso que utilicen esos beans cuando ya no sean necesarios.

**— Ciclos de vida únicamente válidos en el contexto de aplicaciones web:**  
- **_Request_**: se crea una única instancia por solicitud http.  
- **_Session_**: se crea una única instancia por sesión http.  
- **_Global-session_**: se crea una única instancia por sesión global. Se usa típicamente en contexto de Portlet.  
- **_Application_**: el ciclo de vida del bean se acopla al ciclo de vida de un ServletContext.  
- **_Websocket_**: el ciclo de vida se acopla al ciclo de vida de un WebSocket.
