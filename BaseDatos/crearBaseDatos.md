= CREAR UNA BASE DE DATOS SQL_SERVER LOCAL CON DOCKER 

1- tener instalado docker 
2- descargar sql server en docker :
sudo docker pull mcr.microsoft.com/mssql/server:2022-latest

3- crear una imagen de sqlServr en docker
ejecutar: sudo docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=1234" -p 1433:1433 --name sql1 --hostname sql1 -d mcr.microsoft.com/mssql/server:2022-latest

accedes con 
		host : localhost 
		puerto: 1433
		usernam : su
		password : 1234
		
		
Configurar apllication.yml

spring:
  datasource:
    url: jdbc:sqlserver://${HOST_SQL}:1433;databaseName=${DB_NAME};encrypt=false
    driver-class-name: com.microsoft.sqlserver.jdbc.SQLServerDriver  // driver para sqlServer
    username: ${DB_USERNAME} // su
    password: ${DB_PASSWORD} // 1234
  jpa:
    show-sql: true
    database-platform: org.hibernate.dialect.SQLServerDialect  //plataforma para jpa al usar sqlServer 
    generate-ddl: true
    hibernate:
      ddl-auto: update

