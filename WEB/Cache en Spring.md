#### Porque utilizar cache ?

==***CHACHE DISTRIBUIDA* (Redis)*==
## CHACHE LOCAL

### Cache Aside Strategy
![[Pasted image 20241114172354.png]]
**Esta estrategia consiste :**
La app consulta primero a la cache  , sino esta va a la DB y despues la guarda en Cache para que en la proxima consulta si este en cache.
#### Primero debes de agregar estas dos dependencias
* Spring boot started cache 
* En este caso usaremos el manejador de cache **"Caffeine"**, una librerira que permite la gestion de **"cache local"**.
			`<dependency>`
				`<groupId>org.springframework.boot</groupId>`
				`<artifactId>spring-boot-starter-cache</artifactId>`
			`</dependency>`

			<dependency>
				<groupId>com.github.ben-manes.caffeine</groupId>
				<artifactId>caffeine</artifactId>
			</dependency>
#### Crear Cache Manager
Es un manager que hay que crear para este bean tenga los elementos que guardara de la cache.
En este ejemplo voy a hacer que tenga dos cache uno para libro y otro para autor. 

`@Configuration`
`@EnableCaching`
`public class CacheConfig {`
	`public static final String BOOKS_CACHE = "BOOKS_CACHE"; 
	`public static final String AUTHORS_CACHE = "AUTHORS_CACHE";`
    
	`@Value("${cache.books.ttl:24}")` 
	`private long cacheBooksTtl;` 
	
	`@Value("${cache.books.max-size:5}")`
	`private long cacheBooksMaxSize;` 
	
	`@Value("${cache.authors.ttl:24}")` 
	`private long cacheAuthorsTtl;` 
	
	`@Value("${cache.authors.max-size:10}")` 
	`private long cacheAuthorsMaxSize;`

    @Bean
    public CacheManager cacheManager() {
        List<CaffeineCache> caches = new ArrayList<>();

	    caches.add(buildCache(BOOKS_CACHE, cacheBooksTtl, TimeUnit.HOURS, cacheBooksMaxSize)); // agrego cache para libros
		caches.add(buildCache(AUTHORS_CACHE, cacheAuthorsTtl, TimeUnit.HOURS, cacheAuthorsMaxSize)); // agrego cach para autores

        SimpleCacheManager manager = new SimpleCacheManager();
        manager.setCaches(caches);
        return manager;
    }


    private static CaffeineCache buildCache(String name, long ttl, TimeUnit ttlUnit, long size) {
        return new CaffeineCache(name, Caffeine.newBuilder()
                .expireAfterWrite(ttl, ttlUnit)   // metodo para expirar el cache
                //.expireAfterAccess(ttl, ttlUnit)
                .maximumSize(size)
                .build());
    }
`}`
###### Explicacion de codigo :
* **`@EnableCaching`**: Habilita el uso de caché en la aplicación.
- **`@Value`**: Obtiene valores de configuración (TTL y tamaño máximo) de las propiedades, o usa valores predeterminados.
- **`expireAfterWrite(ttl, ttlUnit)`**: Define cuánto tiempo permanecerán los datos en el caché antes de expirar.
- **`maximumSize(size)`**: Limita el número máximo de elementos en el caché. Si se supera, se eliminarán elementos para hacer espacio.

#### Implementacion de cache en los Servcicios (ejemplo autor y libro)
`@Service`
`public class LibroService {`

    @Cacheable(value = CacheConfig.BOOKS_CACHE, key = "#isbn") 
    public Book obtenerLibroPorIsbn(String isbn) {
     // Lógica para obtener el libro desde la base de datos 
    }
	`// CacheEvict: Evita almacenar en caché el libro con un ISBN específico cuando se elimina 
	`@CacheEvict(value = CacheConfig.BOOKS_CACHE, key = "#isbn")` 
	`public void eliminarLibro(String isbn) {` 
		`libroRepository.deleteByIsbn(isbn);` 
	`}` 
	`// CacheEvict: Actualiza el libro y limpia la caché asociada con ese libro` 
	`@CacheEvict(value = CacheConfig.BOOKS_CACHE, key = "#libro.isbn")` 
	`public void actualizarLibro(Libro libro) {` 
		`libroRepository.save(libro);` 
	`}`	
}

`@Service`
`public class Autor {`

	`@Cacheable(value = CacheConfig.AUTHORS_CACHE, key = "#authorId")` 
	`public Author obtenerAutorPorId(Long authorId) {` 
		`// Lógica para obtener el autor desde la base de datos` 
	`}`
	`@CacheEvict(value = CacheConfig.AUTHORS_CACHE, key = "#id")` 
	`public void eliminarAutor(Long id) {` 
		`autorRepository.deleteById(id);` 
	`}` 
`// CacheEvict: Actualiza el autor y limpia la caché asociada con ese autor 
	`@CacheEvict(value = CacheConfig.AUTHORS_CACHE, key = "#autor.id")` 
	`public void actualizarAutor(Autor autor) {`
		`autorRepository.save(autor);`
	`}`
`}`
###### Explicacion de codigo:
**`@Cacheable`**: Usa la estrategia Cache Aside. Primero consulta el caché (basado en el `isbn`), y si no está, llama a `findByIsbn` en el repositorio y guarda el resultado en caché. (Solamente para LECTURAS) .
**`@CacheEvict`**: Limpia el caché (o invalida la entrada) cuando se guarda o actualiza el libro, o borra. Esto asegura que el caché no devuelva datos obsoletos en futuras solicitudes.
    - **`key = "#isbn"`**: Define que la clave del caché será el ISBN del libro.
- **`unless="#result == null"`**: Evita almacenar en caché si el resultado es `null`.

