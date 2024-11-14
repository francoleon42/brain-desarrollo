
#### ¿Qué es? 
Un estilo arquitectónico para desarrollar servicios web.
Significa  que que un servidor HTTP o basado en la web que acepta solicitudes a través de HTTP y responde probable en JSON legible por humanos.    
#### ¿Para qué sirve? 
Permite realizar la manipulación básica de datos dentro de una aplicación
## ==Principios Rest== 
 Estos principios nos ayudan a desarrollar servicios más escalables, fiables y flexibles.  y si sigue estos principios completamente se llama **"RestFul"** , estos son los principios:

#### **🔸**1 **Client -Servidor:** 
separa las preocupaciones de la interfaz de usuario (cliente) de las del servidor. Esta separación permite que las interfaces de usuario se desarrollen y evolucionen independientemente de la lógica de backend y almacenamiento de datos.
#### **🔸2 Apátrida o sin estado:**
Cada solicitud del cliente al servidor debe contener toda la información necesaria para entender y procesar la solicitud, sin depender del estado guardado en el servidor. (Cada solicitud es independiente y autónoma).
#### **🔸3 Caché:**
Agregar caché a las respuestas permite que futuras solicitudes puedan ser atendidas desde la caché, sin necesidad de hacer una nueva solicitud al servidor.
####  **🔸4 Uniforme de interfaz :** 
asegura que la comunicación entre el cliente y el servidor sea  a través de una interfaz coherente y estandarizada. Debe cumplir con lo siguiente : 
a) **Identificación de Recursos :** Los recursos se identifican de manera única mediante URLs. Esto significa que cada recurso (por ejemplo, un producto, un usuario) tiene su propia dirección URL específica.
b) **Manipulación de Recursos a través de la Representación:** El cliente puede especificar el formato en el que desea recibir o enviar datos de un recurso. Esto se realiza mediante encabezados HTTP como Accept y Content-Type.
c) **Mensajes Autodescriptivos :** Cada mensaje HTTP (solicitud o respuesta) contiene suficiente información para que el receptor(cliente o servidor) pueda entenderlo y procesarlo sin necesidad de información adicional. Esto incluye los encabezados y el cuerpo del mensaje.
d) **HATEOAS :** El servidor proporciona enlaces en sus respuestas que indican al cliente qué acciones pueden realizarse a continuación. Esto ayuda a los clientes a descubrir cómo interactuar con la API.
#### 🔸**5 Sistema en capas :** 
Se relaciona con el hecho de que puede haber más componentes y subsistemas entre un Cliente y un Servidor.  
Básicamente hay dos cuestiones , “Separación de Responsabilidades”, cada capa tiene una responsabilidad específica. “Intermediarios” : Las capas intermedias pueden ser introducidas entre el cliente y servidor (Proxys , gateways,etc)  
#### 🔸**6 Code on Demand :** 
Es una restricción “opcional” , significa que un Servidor puede enviar un código ejecutable como respuesta al Cliente.
Ejemplo :  cuando un navegador, por ejemplo, recibe una respuesta del servidor con una etiqueta HTML <script/> entonces, cuando se carga el documento HTML, se puede ejecutar el script.

  
## ==Anatomía de un Request ==

#### **🔸Tinen cuatro elementos:** 

###### - **URL:** 
Ejemplo : [http://api.example.com/](http://api.example.com/) ,  la dirección no solo para identificar un recurso, sino también para especificar cómo acceder a él.
###### - URI:
Ej: /products,  especifica a qué recurso le gustaría acceder el Cliente en una solicitud    
###### - Parámetros:
información que se puede enviar en una solicitud del Cliente para influir en la respuesta del Servidor. Hay diferntes tipo de parametros :
 - **Body Params :** Contiene todos los datos que el servidor necesita , se utiliza solamente en crear y actualizar
 
- **Route Params :** Estos se insertan en la URL con la información para identificar un recurso específico para realizar una acción, como: recuperar, editar, actualizar o eliminar. Ejemplo: [http://api.example.com/products/1](http://api.example.com/products/1)
    
- **Query Params :** También son los parámetros insertados en la URL, pero están relacionados con filtrar y buscar información sobre un recurso, o incluso paginar y ordenar los resultados. 
    Por ejemplo: [http://api.example.com/products?name=laptop&available=true](http://api.example.com/products?name=laptop&available=true)
    En este ejemplo dado, el Cliente comunica al Servidor que la solicitud es recuperar productos con nombre igual a laptop, y disponible igual a verdadero.

- **Encabezados :**  Permite enviar información adicional en una solicitud. Funciona con formato **`clave: valor`** , que se envia een la solicitud y respuesta. Ejemplos: 
	-  **Authorization:** Para enviar tokens o credenciales de autenticación.
	- **User-Agent**:** Identifica al cliente (navegador o aplicación) que realiza la solicitud.
	- **Cache-Control**:** Controla cómo los navegadores y servidores almacenan en caché las respuestas.
		- En este caso, el navegador almacenará en caché la imagen del logo durante 24 horas y, en futuras solicitudes, la cargará desde la caché en lugar de volver a solicitarla al servidor.
		 `GET /assets/logo.png HTTP/1.1`
		 `Host: www.example.com`
		 `Cache-Control: max-age=86400`

	- **Accept y Content-Type**:** `Accept` indica el tipo de contenido esperado por el cliente, y `Content-Type` describe el tipo de contenido enviado.
	- **CORS (Cross-Origin Resource Sharing)**:** Controla quién puede acceder a los recursos desde un dominio diferente, mediante el encabezado `Access-Control-Allow-Origin`.  
## ==Métodos HTTP== 

###### **GET**  
Leer un recurso específico (por un identificador) o una colección de recursos.
###### POST 
Crear un nuevo recurso
###### PUT 
Reemplazar un recurso específico (por un identificador) o una colección de recursos. También se puede utilizar para crear un recurso específico si el identificador de recursos se conoce de antemano.
###### DELETE  
Eliminar un recurso específico mediante un identificador.
###### PATCH  
Actualizar un recurso específico (por un identificador) o una colección de recursos.
Esto se puede considerar de alguna manera como una actualización ‘parcial’ vs el reemplazo que realiza PUT.

## ==Status  Code==

#### Grupo 2  ( indica una solicitud exitosa )
* ***200 (Ok) :** Solicitud exitosa
* ***201 (Creado):** Solicitud de recursos exitosos y creados
* **204 (Sin contenido):** La solicitud tuvo éxito y no hubo contenido adicional en el cuerpo de la respuesta.
#### Grupo 3 ( indica respuestas de redirección)
* **301 (Movido Permanentemente) :**  El recurso solicitado ha cambiado permanentemente y se proporciona una nueva URL en la respuesta.
* **302( Found**): Redirección temporal.
* **304 (No Modificado)** **:**  El recurso solicitado no se modificó y se puede usar la misma versión en caché.
* **307 (Redireccionamiento temporal):** El recurso solicitado ha sido redirigido temporalmente a otra URL
#### Grupo 4 (indica un error en el lado del cliente)
* **400 (Mala Solicitud):** La solicitud no podía ser entendida por el servidor
* **401 (No autorizado):**El cliente no está autenticado para acceder al recurso
* **403 (Forbidden):** El cliente no está autorizado para acceder al recurso
* **404 (No Encontrado):** El servidor no pudo encontrar el recurso solicitado
#### Grupo 5 (Indica un error en el lado del servidor)
* **500 (Error del Servidor Interno):**  Indica que el servidor enfrentó un error inesperado y no pudo devolver la respuesta de la solicitud.
* **503 (Servicio No disponible):** Indica que el servidor no puede procesar la solicitud porque no está disponible, está sobrecargado o en mantenimiento**

