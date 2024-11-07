![[Pasted image 20241029153440.png]]
**Son :** 
Las collections son estructuras de datos que permiten almacenar, modificar y manipular grupos de objetos en memoria, como listas, conjuntos y mapas.

### Tipos
- ==**Listas (`List`)**:== Colecciones ordenadas que permiten elementos duplicados. Ejemplo: `ArrayList`, `LinkedList`.
	- **ArrayList**: Ideal para acceso rápido, pero más lenta en inserciones y eliminaciones intermedias.
	- **LinkedList**: Más rápida para inserciones y eliminaciones, especialmente en el inicio y el final, pero más lenta en acceso aleatorio.
- ==**Conjuntos (`Set`)**:== Colecciones que no permiten duplicados y no están ordenadas. Ejemplo: `HashSet`, `TreeSet`.
	- **TreeSet**: Ordena elementos de forma natural o con un comparador, pero es más lenta en inserción y búsqueda.
	- **HashSet** es eficiente para búsqueda, pero no garantiza el orden. Un **LinkedHashSet** podría ser útil si quieres conservar el orden de inserción.
	- **TreeSet**: Vale la pena mencionar que `TreeSet` es más lento que `HashSet` pero permite orden natural o específico con un `Comparator`.
- ==**Mapas (`Map`)**:== Estructuras de datos clave-valor. No permite claves duplicadas, pero los valores sí pueden repetirse. Ejemplo: `HashMap`, `TreeMap`.
	- **HashMap**: Eficiente para pares clave-valor sin un orden específico.
	- **TreeMap**: Mantiene las claves ordenadas de manera natural o con un comparador.
* ==**Queque**==
	*  **Priority Queue** : Estructura que sigue la política de primero en entrar, primero en salir (FIFO). Ejemplo: `PriorityQueue`.
	- **Deque (`Deque`)**: Permite añadir y eliminar elementos tanto al inicio como al final, útil para pilas y colas. Ejemplo: `ArrayDeque`.****
 
##### **Algoritmos de Collections (Clase `Collections`)**:
- La API de collections incluye una clase llamada `Collections` que proporciona métodos estáticos para operaciones comunes, como:
    - **Ordenamiento**: `Collections.sort(lista)`
    - **Buscar Mínimo/Máximo**: `Collections.min(lista)`, `Collections.max(lista)`
    - **Buscar un Elemento**: `Collections.binarySearch(lista, elemento)` (requiere lista ordenada).
    - Para colecciones que no deben modificarse después de ser creadas, usa `Collections.unmodifiableList(lista)`. Esto es útil en colecciones compartidas o en contextos de concurrencia.
##### Uso de Iteradores
Cuando necesitas eliminar elementos mientras recorres una colección, es más seguro utilizar un **iterador** en lugar de un bucle `for-each`, ya que el iterador permite eliminar elementos de forma segura sin causar errores como `ConcurrentModificationException`.
`List<String> lista = new ArrayList<>(Arrays.asList("A", "B", "C"));`
`for (String elemento : lista) {`
    `if (elemento.equals("B")) {`
        `lista.remove(elemento); // Esto provocará ConcurrentModificationException`
    `}`
`}`

#####  Straems y manipulación Funcional
Los Streams en Java permiten manejar y transformar colecciones de manera moderna y concisa 