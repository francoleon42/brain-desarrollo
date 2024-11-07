![[Pasted image 20241031160713.png]]
 spring batch?

## JPA HIBERNATE 
Conceptos básicos de JPA:

ORM (Object-Relational Mapping): Aprender cómo JPA permite mapear objetos Java a tablas de bases de datos y cómo manejar la persistencia de objetos.
Entidades: Comprender cómo las clases de Java se anotan con @Entity para representar tablas de la base de datos.
Primary Key: Aprender sobre la clave primaria (@Id) y cómo generar valores automáticamente con anotaciones como @GeneratedValue.
Relaciones entre entidades:

Conocer cómo representar relaciones entre entidades: OneToOne, OneToMany, ManyToOne y ManyToMany.
Cardinalidad y cascade: Saber cómo la cardinalidad afecta las consultas y transacciones, y entender cuándo y cómo usar cascade.
Consultas con JPQL:

Aprender a escribir consultas con JPQL (Java Persistence Query Language) y cómo utilizar criterios de búsqueda, filtros y proyecciones.
Consultas nativas: Conocer la diferencia y el uso de consultas nativas en SQL cuando JPQL no es suficiente.
Hibernate como implementación de JPA:

Hibernate es una implementación de JPA y añade varias características adicionales, como lazy loading (carga perezosa), fetching strategies, y second-level cache (caché de segundo nivel).
Configuración: Aprender a configurar Hibernate y entender sus archivos de configuración (hibernate.cfg.xml o application.properties).
Transacciones y manejo de la concurrencia:

Comprender el manejo de transacciones en JPA, incluyendo la propagación de transacciones y el uso de @Transactional.
Entender problemas de concurrencia como lectura sucia, lectura no repetible y cómo gestionarlos con niveles de aislamiento.
Optimización:

Cache: Explorar el uso de caché en Hibernate (caché de primer y segundo nivel) para mejorar el rendimiento.
Batch Processing: Implementar el procesamiento en lotes para mejorar la eficiencia en grandes volúmenes de datos.




---
En Hibernate, la propiedad ddl-auto tiene varias configuraciones posibles que controlan cómo se maneja el esquema de la base de datos:
validate: Verifica que las tablas y columnas existan, pero no realiza ningún cambio en el esquema.
update: Actualiza el esquema de la base de datos para que coincida con las entidades, sin perder datos.
create: Crea el esquema de la base de datos, descartando los datos existentes.
create-drop: Crea el esquema de la base de datos al inicio de la aplicación y lo elimina al final.
none: No realiza ninguna acción con el esquema de la base de datos.
