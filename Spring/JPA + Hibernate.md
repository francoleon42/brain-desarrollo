
##### ¿**Que es Java Persistence API (JPA) ?**
Es una especificación de Java que permite el mapeo de objetos en una base de datos relacional. Es un conjunto de reglas y guías para interactuar con bases de datos, y requiere una herramienta como **Hibernate** para funcionar.
Permite automatizar la manipulación de datos en bases de datos a través de objetos Java. Hibernate gestiona gran parte de la lógica que usualmente implementarías manualmente para acceder y manipular una base de datos.
### **¿Qué es Hibernate?**
Es una implementacion de JPA . Hibernate es un framework ORM (Object-Relational Mapping).


Etiquetas

##### Configuración de Hibernate con Spring Boot
La propiedad ddl-auto tiene varias configuraciones posibles que controlan cómo se maneja el esquema de la base de datos:
* **validate:** Verifica que las tablas y columnas existan, pero no realiza ningún cambio en el esquema.
* **update:** Actualiza el esquema de la base de datos para que coincida con las entidades, sin perder datos.
* **create:** Crea el esquema de la base de datos, descartando los datos existentes.
* **create-drop:** Crea el esquema de la base de datos al inicio de la aplicación y lo elimina al final.
* **none:** No realiza ninguna acción con el esquema de la base de datos.
Ejemplo : 
	`jpa:`  
	  `database-platform: org.hibernate.dialect.H2Dialect`  
	  `hibernate:`  
	    `ddl-auto: update`
