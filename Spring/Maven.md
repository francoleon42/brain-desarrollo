
Maven es una herramienta de gestión de proyectos y construcción automática.
 Simplifica el proceso de compilación, prueba y empaquetado de aplicaciones Java, como también facilita la gestión de dependencias (librerías y frameworks que tu proyecto necesita).

Gestiona dependencias , que deben de ser definidas en el archivo pom.xml (proyect Object Model ). Luego, descarga automaticamente las bibliotecas desde los repositorios.

#####  **Ciclo de vida de construcción (Build Lifecycle)**:
Maven sigue un ciclo de vida de construcción que define las diferentes etapas del proceso de construcción. Algunas de las etapas más comunes son:
- `mvn clean`: Limpia el proyecto eliminando archivos generados.
- `mvn install`: Construye el proyecto y lo instala en el repositorio local.
- `mvn package`: Empaqueta el proyecto en un archivo `jar` o `war` que puedes ejecutar o desplegar.
- `mvn spring-boot:run`: Ejecución rápida de aplicaciones Spring Boot, útil para pruebas.
##### Perfiles
Maven permite configurar "perfiles" para diferentes entornos (por ejemplo, desarrollo, pruebas, producción). Esto es útil para ajustar configuraciones sin cambiar el código.
Ponele que quieras ejecutar el perfil de desarrollo : **`mvn clean install -Pdev`**
El pom se vería algo así : 
			`<project ...>`
	    `<!-- Otras configuraciones del proyecto -->`
	
	    `<profiles>`
	        `<!-- Perfil para Desarrollo -->`
	        `<profile>`
	            `<id>dev</id>`
	            `<properties>`
	                `<db.url>jdbc:mysql://localhost:3306/dev_db</db.url>`
	                `<db.username>dev_user</db.username>`
	                `<db.password>dev_password</db.password>`
	            `</properties>`
	        `</profile>`
	
	        `<!-- Perfil para Producción -->`
	        `<profile>`
	            `<id>prod</id>`
	            `<properties>`
	                `<db.url>jdbc:mysql://localhost:3306/prod_db</db.url>`
	                `<db.username>prod_user</db.username>`
	                `<db.password>prod_password</db.password>`
	            `</properties>`
	        `</profile>`
	    `</profiles>`
	`</project>`
	