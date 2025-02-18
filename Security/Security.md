
## Spring security
Es una poderosa biblioteca de Java diseñada para manejar **autenticación** (quién eres) y **autorización** (qué puedes hacer). Se integra de manera muy natural con Spring Boot y ofrece una seguridad lista para usar, pero también es completamente personalizable.

==**Authentication**==
## **Autorización**
La autorización responde a la pregunta: **¿Qué tienes permitido hacer?**
### **Cómo funciona la autorización en Spring Security**
1. **Roles y permisos**:
    - Se asignan **roles** (por ejemplo, `ROLE_ADMIN`) o permisos (`CAN_VIEW_REPORTS`) a los usuarios.
####   Por Roles : 

1. **Restricciones basadas en roles**:
    - Usa anotaciones como `@PreAuthorize` o `@Secured` para proteger métodos:    
        `@PreAuthorize("hasRole('ADMIN')") 
        `public void deleteUser(Long userId) {}`
    
3. **Configuración de acceso**:
    - Puedes definir reglas específicas de acceso en tu clase de configuración de seguridad:
        `http.authorizeRequests()     
        `.antMatchers("/admin/**").hasRole("ADMIN")`     
        `.antMatchers("/user/**").hasAnyRole("USER", "ADMIN")`     
        `.antMatchers("/public/**").permitAll();`

#### Por permisos
Definen acciones más específicas, como `CAN_VIEW_USER` o `CAN_DELETE_REPORT`.
##### 1. **Modelo de datos**
Define una relación entre los usuarios, roles y permisos. Por ejemplo:
	`@Entity`
	`public class Role {`
	    `@Id`
	    `@GeneratedValue(strategy = GenerationType.IDENTITY)`
	    `private Long id;`
	
	    `private String name; // Ejemplo: ROLE_ADMIN`
	
	    `@ManyToMany(fetch = FetchType.EAGER)`
	    `private Set<Permission> permissions;`
	`}`
	
	`@Entity`
	`public class Permission {`
	    `@Id`
	    `@GeneratedValue(strategy = GenerationType.IDENTITY)`
	    `private Long id;`
	
	    `private String name; // Ejemplo: CAN_VIEW_USER`
	`}`
**En tu clase `User`:**
`@ManyToMany(fetch = FetchType.EAGER)`
`private Set<Role> roles;`
###### 3. **Configurar reglas de acceso con permisos**
Spring Security permite usar anotaciones como `@PreAuthorize` para verificar permisos específicos.
		`@PreAuthorize("hasAuthority('CAN_VIEW_USER')")`
		`public List<User> getAllUsers() {`
		    `return userService.findAll();`
		`}`
**Configurar reglas en `SecurityConfig`:**
	`http.authorizeRequests()`
    `.antMatchers("/admin/**").hasAuthority("CAN_EDIT_USER")`
    `.antMatchers("/reports/**").hasAuthority("CAN_VIEW_REPORTS")`
    `.anyRequest().authenticated();`



#### 5. **Gestionar permisos dinámicamente**

En aplicaciones más avanzadas, puedes almacenar permisos en la base de datos y gestionarlos dinámicamente, asegurándote de que los cambios sean reflejados sin necesidad de reiniciar la aplicación.

Por ejemplo:
- Crear un **end-point** para asignar permisos a un rol:
	`@PostMapping("/roles/{roleId}/permissions") 
	`public ResponseEntity<?> assignPermissionToRole(@PathVariable Long roleId,` `@RequestBody Permission permission) {`
	     `roleService.addPermissionToRole(roleId, permission);`     
	     `return ResponseEntity.ok("Permiso asignado");` 
	`}`


## **JWT (JSON Web Tokens)**
JWT es un estándar para transmitir información de manera segura entre dos partes, como el cliente y el servidor.
### **Estructura de un JWT**
Un JWT tiene tres partes principales:
1. **Header**: Contiene el tipo de token (JWT) y el algoritmo de firma (e.g., HS256).
2. **Payload**: Contiene los datos o claims (como `username`, `roles`, `exp`).
3. **Signature**: Es una firma digital que asegura que el token no ha sido alterado.

Ejemplo de un JWT codificado:
	`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9`
	`eyJzdWIiOiJ1c2VyIiwicm9sZXMiOlsiUk9MRV9BRE1JTiJdfQ`
	`dBjftJeZ4CVP-mB92K27uhbUJU1p1r_wW1gFWFOEjXk`

**0Auth**
IAM