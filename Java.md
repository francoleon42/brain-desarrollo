





En Hibernate, la propiedad ddl-auto tiene varias configuraciones posibles que controlan cómo se maneja el esquema de la base de datos:
validate: Verifica que las tablas y columnas existan, pero no realiza ningún cambio en el esquema.
update: Actualiza el esquema de la base de datos para que coincida con las entidades, sin perder datos.
create: Crea el esquema de la base de datos, descartando los datos existentes.
create-drop: Crea el esquema de la base de datos al inicio de la aplicación y lo elimina al final.
none: No realiza ninguna acción con el esquema de la base de datos.





















