## Autenticación

Específicamente sobre autenticación, Spring Cloud y Spring Security.


### Retomando qué es el API Gateway
El mismo nombre de este recurso ya anticipa su funcionalidad: en español, gateway significa “puerta de entrada”. Situada entre el cliente y los servicios del back end, API Gateway actúa como un proxy inverso que filtra el tráfico de todas las solicitudes y dirige los datos y las llamadas a la ubicación adecuada.

El gateway actúa como un mecanismo de seguridad que garantiza la protección a través de la autenticación de los usuarios, limitando las conexiones y los permisos de los usuarios. Esto facilita el control de acceso al back end y al servicio web, ya que centraliza las solicitudes en un único punto de entrada, lo que permite enrutar el tráfico. Además, la API de negocio se vuelve menos compleja y mucho más sencilla de mantener, ya que la función de control de acceso se concentra en la API del gateway.

En el siguiente video, abordaremos un poco más sobre el tema y hablaremos sobre la autenticación.


(video)

Como lo establece el patrón de diseño EDGE SERVER, spring cloud server es el componente ideal para aplicar seguridad en nuestros microservicios, porque determina quién puede acceder o no, mediante autenticación y autorización.

El procweso de autenticación define si el usuario es quien dice ser y la autorización, si puede o no hacer algo. El gateway permite centralizar, y aprobar o no las solicitudes y e indica qué puede y qué no hacer:
A partir de configuraciones, sipring genera:
+ lista de recursos
+ rutas para acceder a ellos
+ condiciones para tales accesos (autenticación y autorización)

___

## Spring Security - OAuth 2.0
Intuitivamente, podemos relacionar el nombre OAuth 2.0 con autenticación y seguridad. OAuth es un protocolo estándar para la autorización, permite que las aplicaciones web, móviles y de escritorio obtengan acceso limitado a la información del usuario a través del protocolo HTTP. Puede considerarse un marco de autorización que tiene un estándar abierto para la autenticación en sistemas que usan HTTP. Empresas muy grandes y famosas —como Twitter, Google, Facebook y GitHub— utilizan esta tecnología.

Es común encontrar el uso de esta tecnología en las pantallas de inicio de sesión, por ejemplo, en los sitios web de las empresas mencionadas anteriormente. Sin embargo, nos encontraremos mucho con este protocolo en la autenticación de APIs, donde se deberá generar un token para poder acceder a estas, de una forma segura y eficiente.

### ¿Por qué OAuth?
En la práctica, la mayoría de los sitios tienen sus propios mecanismos de autenticación (cuando se registran les pide un correo y una contraseña). Pero, por otro lado, también permiten loguearse con las credenciales de otros proveedores (Google, por ejemplo). Muchos usuarios usan el segundo mecanismo y, a su vez, a las empresas les conviene desde el punto de vista de negocio, porque automatizan el proceso de onboarding (ingreso de usuarios a una nueva plataforma) y, desde el punto de vista tecnológico, delega el proceso de autenticación —con sus costos y riesgos asociados— en un proveedor validado.

#### ¿Piensan que Google, Facebook y Twitter comparten las credenciales de sus usuarios?

La respuesta a esto es **OAuth**, el cual es un estándar que permite compartir <font size="3" color="orange"> información de autorización específica de usuario </font> de un proveedor (Google, Facebook, etc.) a un consumidor (cualquier aplicación), <font size="3" color="orange"> sin revelar la identidad del usuario.</font>

Veamos a continuación el camino de un request con y sin autorización.
![](img/m2c9oauthFlujo.png)

1. El usuario hace un request
2. El Gateway redirige al proveedor de seguridad
3. El usuario se autentica y autoriza a la app
4. Se envía el request y token al servidor de la aplicación
5. El server de aplicación checkea el token

### Roles de OAuth
A continuación conoceremos los roles de OAuth.

   + **RESOURCE OWNER:** es la persona/entidad que da acceso a sus datos. Literalmente el propietario del recurso. Así es como OAuth2.0 clasifica al usuario
   +   **CLIENT:** Es la aplicación que interactúa con el propietario del recurso. En el caso de una aplicación web, sería la aplicación del navegador.
   +   **RESOURCE SERVER:** La API que está expuesta en inernet necesita protección de datos. Para obtener acceso a su contenido, se necesita un token emitido por el authorization server.
   +   **AUTHORIZATION SERVER:** Es responsable de autenticar al usuario y emitir tokens de acceso. Es quien tiene la información del resource owner(usuario). Autentica e interactúa con el usuario después de la identificación del cliente.

## Flujo del proceso de autorización
Ahora que conocemos los pilares que estructuran OAuth 2.0, comprendamos el flujo durante el proceso de autorización. Sin embargo, recordemos que el flujo que se muestra es una descripción general, ya que puede ser un poco diferente según el tipo de concesión de autorización y el uso. Para comprenderlo, hagamos un repaso de los conceptos de autenticación y autorización:

- Autenticación: es el proceso que determina que un usuario es quien dice ser.
- Autorización: es el proceso que determina que un determinado usuario puede ejecutar determinada acción.
  
En la práctica, un usuario provee un usuario, una contraseña y, en algunos casos, una segunda contraseña (llamada segundo factor de autenticación). A partir de esto, se obtiene un **token de sesión** (un string encriptado) que se verifica en el server. **El “uso malicioso” de este token se conoce como session hijacking.**

Respecto de la autorización, proceso posterior a la autenticación, cada vez que se recibe un request se obtiene un objeto con los datos del usuario asociado a esa sesión. Entre estos datos vienen los denominados claims o permissions que son por lo general strings con un determinado protocolo, por ejemplo, JWT. En la práctica, estos permisos no son otra cosa más que “roles”, a los usuarios se les asignan estos roles y las aplicaciones o microservicios controlan que un usuario tenga uno o más roles según el caso.

A continuación, veremos el flujo del proceso de autorización de OAuth 2.0

![](img/m2c9oauth2.png)
1. La aplicación solicita autorización para acceder a los recursos del servidor de los usuarios
2. Si el usuario autoriza la solicitud, la aplicación recibe una concesión de autorización.
3. La aplicación solicita un token de acceso al servidor de autorizaciones, validando la identidad, es decir, realiza la autenticación, otorgando la autorización.
4. Si se autentica la identidad de la aplicación y la concesión de autorización es válida, el servidor de autorización emite un token de acceso a la aplicación. La autorización, ene ste paso, ahora está completa y el cliente ya tendrá un token de acceso par administrar.
5. Cuando la aplicación necesita solicitar un recurso del servidor e recursos, como puede ser una API, simplemente presenta el token de acceso de autenticación.
6. En el último paso, si el token de acceso es válido, el sevidor de recursos proporciona el recurso a la aplicación.

## Sobre la seguridad de los endpoints
En primer lugar, veamos cómo es el flujo de un request asumiendo que ha pasado todos los controles de seguridad.

![](img/m2c9oauth3.png)

Este tipo de diagrama se conoce como diagrama de secuencia UML. Por lo general, las fechas tienen llamadas a métodos. En la práctica, el microservicio no es una caja negra y, además, la base de datos es parte del microservicio. Veamos cómo sería en detalle en la práctica.

![](img/m2c9oauth4.png)

De acuerdo a lo visto en el gráfico, podemos apreciar que el gateway recibe un request a una determinada ruta y este conoce a qué instancia de microservicio redirigir dicho pedido. Este accionar, el gateway, lo realiza a través de un manejador de rutas (RouteHandler), el cual a su vez deriva el pedido a un controlador (Controller) que es quien finalmente invoca al servicio de negocio solicitado.

Luego, como en toda aplicación en capas, la capa de servicios realiza las transformaciones (DTO) y validaciones de negocio necesarias para enviar a la capa de datos solo la información que necesita. La capa de datos traduce la información recibida en sentencias SQL para ser enviada a la base de datos (patrón DAO).

De la misma manera, la respuesta genera el camino inverso. La base de datos responde a la capa de datos la petición generada a través de la sentencia SQL recibida. Luego, esta la envía a la capa de servicios, para finalmente dirigir la información al controlador, quien transforma la información recibida en un response y devuelve esta respuesta al origen que —a los ojos de quien generó la petición— es el gateway.

___

## Seguridad
En el diagrama anterior, vimos con cierto detalle el camino con el que se ejecuta un request. ***Cuanto más se acerque un request a la base de datos, más costoso es en términos de consumo de recursos***.

Si pensamos en la seguridad como un filtro, el punto más claro para aplicar seguridad es el gateway. Esto no es absoluto, pero es el caso típico, si un usuario exterior solo puede acceder por el gateway no tiene sentido aplicar seguridad en los microservicios individuales. No obstante, los riesgos se miden de acuerdo a “probabilidad x impacto”, e incluso existen ciertas regulaciones (SOX, por ejemplo) y buenas prácticas. Esto sugiere que se pueden aplicar controles redundantes de seguridad a fin de mitigar riesgos. Por ejemplo, que un endpoint en el gateway tenga un fallo de seguridad que permita pasar un request a los microservicios sin autenticación. Este tipo de fallo se conoce como “back door”.

Otro caso típico son controles en la base de datos, la **restricción de ciertos comandos e incluso backups y rollbacks de transacciones no autorizadas**. Supongamos un usuario ya logueado que en un campo de texto logra hacer un ataque de SQL injection. El request, al estar autenticado, pasaría el firewall e iría hasta la base de datos, entonces, es necesario aplicar un “DROP ALL TABLES”.

En esta clase vamos a ver cómo aplicar oAuth sobre un gateway de Spring.

## Spring Security
Repasando lo visto en clases anteriores, el mecanismo de seguridad por default de Spring es **Spring Security**. Este provee mecanismos de autenticación (un usuario es quien dice ser), autorización (un usuario determinado puede ejecutar determinadas acciones) y gestión de tokens de sesiones. Existen diferentes maneras de representar esto, pero en la práctica terminan siendo una serie de tablas en la base de datos que contienen la información de los usuarios y sus permisos. La magia de Spring permite gestionar esa información de manera automática, lo que se traduce en logins, permisos, accesos, ABM de usuarios, etc.

### ¿Cómo funciona la autenticación vía web?
Cada vez que usamos el navegador y abrimos una página web, internamente hacemos un request contra esa página. Al hacer esa petición, además de la ruta y los parámetros, el navegador envía información en forma de “headers”, que no son más que valores “key-value” que pueden ser leídos por quien recibe la petición. 
Hay varios tipos de headers: de idioma, de locación, de manejo de caché, pero a nosotros nos interesan los de seguridad.

Entre esos valores tenemos el header <font size="3" color="orange"> “authorization”, </font> el cual posee dos valores (separados por un espacio, recordemos que el valor es un string encodeado): el primero es el tipo de autenticación y el segundo serían las credenciales. Hay muchas implementaciones y estándares, e incluso estas implementaciones se suelen customizar por temas de seguridad y performance.

Antes de avanzar, lo que tenemos que saber es que en cada request mandamos información que debe ser verificada por el server.

El caso más sencillo sería pedir usuario y contraseña, guardarlo del lado del cliente y enviarlo encodeado con cada request sin encriptar. Efectivamente, este tipo de autenticación se llama justamente <font size="2" color="orange">“basic”. Header de ejemplo:

Authorization: Basic AXVubzpwQDU1dzByYM== </font>

Esto es **muy inseguro,** ya que cualquiera que pudiera leer los headers automáticamente tendría el user y el password del usuario.
Entonces, el próximo paso sería que en el server se genere un **token**, el cual por lo general es una función **hash** (si recuerdan de Java, la función **hashCode** genera un string único a partir del valor de las propiedades de un objeto), entonces, cuando el usuario se loguea la primera vez, el server le devuelve el token que debe usar de ahora en adelante. Este tipo de autenticación se llama <font size="2" color="orange">Bearer.

Authorization: Bearer </font>

Supongamos que tenemos varias aplicaciones (o varias APIs) y queremos diferenciar el acceso a una API u otra. Entonces, empezamos a jugar con otro parámetro que entra en combinación con el anterior. Este tipo de autenticación se conoce como autenticación por <font size="2" color="orange"> API Key.

X-API-Key: abcdefgh123456789 </font>

Vamos avanzando un poco más. En los casos anteriores, **había que enviar usuario y contraseña en texto “planos”** (no es estrictamente así, pero a fines ilustrativos supongamos que sí) al menos una vez y almacenarlos de la misma manera. De esta forma, el servidor debería conocer nuestro password. Pero ¿podríamos hacer que el servidor no sepa nuestro password y aun así poder autenticarlo? Volvamos al principio de la función hashCode de Java. Recuerden que —cuando vieron objetos— si sobreescribían la función equals, debían hacer lo mismo con la función hashCode, porque “objetos iguales deben devolver el mismo hashcode”. Entonces, siguiendo ese razonamiento, si yo tengo dos strings y quisiera saber si son iguales, podría hacerlo comparando tan solo sus hashcodes. Por lo que no tendría que conocer las strings originales. Este mecanismo se llama <font size="2" color="orange">digest </font>. Les compartimos un request de ejemplo:

<font size="2" color="orange">
Authorization: Digest username=”admin” Realm=”abcxyz” nonce=”474754847743646”, uri=”/uri” response=”7cffhfr54685gnnfgerg8”
</font>


Este mecanismo es un poco más complejo que los otros, olvidémonos del parámetro URI que es la dirección de retorno. Veamos a continuación cómo funciona.
![](img/m2c9oauthDigest.png)

- Request: el cliente manda un request al server
- El servidor responde con un único "nonce" a modo de identificación de la seseión y un hash (realm) que representa una suerte de "api-key".
- El cliente envía un hash que toma como input el user, password, nonce y realm. Este hash se puede replicar tanto en el server como en el client, sin necesidad de transmitir el password (recuerden que usamos solo el hash, no el password)
- Si el hash del cliente es igual al del server, se autoriza la operación.

### ¿Cómo funciona la autenticación vía web o OAuth 2.0?
OAuth 2.0 se usa para delegar el proceso de login.

 Un usuario se loguea en un sitio de un tercero y ese sitio debería devolver una forma de autenticación. 
 
 Tenemos tres partes: 
 +  el cliente, 
 +  el servidor y 
 +  el proveedor de autenticación. 
  
  El cliente debería loguearse con el proveedor de autenticación y este devolverle una suerte de token tanto al cliente como al servidor para que ellos puedan matchear esos tokens para las próximas requests entre ellos.

![](img/m2c9oauth5.png)

## Cómo implementar OAuth2.0

¿Cómo implementar OAuth?
Conozcamos el paso a paso. 
1. Primero debemos registrarnos en la API del proveedor, el cual nos dará una URL (un clientId y un secret). En el caso de Google es [Google API Console](https://console.developers.google.com/). Al margen de darles esto, deben proveer una dirección de callback a donde Google debe enviar la información de login. Por ejemplo: http://localhost:8081/login/oauth2/code/google. La ruta /login/oauth2/code/NombreDelProveedor es un estándar (la llamada a localhost es para que puedan hacer pruebas locales).
   
2. En la configuración /src/main/resources/application.yml deben poner lo siguiente:

```yml
spring:
 security:
   oauth2:
     client:
       registration:
         google:
           client-id: your-google-client-id
           client-secret: your-google-client-secret
```
En la práctica los secretos se guardan en un servidor de configuración.

3. En la clase SecurityConfig (si cuando generaron el proyecto con Initialzr le pusieron algún módulo de seguridad, ya se las crea, si no deberán crearla). Aplican el siguiente cambio:

```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws         
    http.authorizeRequests()
         .anyRequest().authenticated()
         .and()
         .oauth2Login();
    }
```
Cabe destacar que la última línea oauth2Login() es la que fuerza, por el uso del and(), la autenticación del proveedor.

4. Ahora debemos obtener información del usuario, ya sea para mapearlas con permisos dentro de nuestra aplicación o bien para utilizarla:

```java
@Autowired
private OAuth2AuthorizedClientService authorizedClientService;

@GetMapping("/loginSuccess")
public String getLoginInfo(Model model, OAuth2AuthenticationToken authentication) {
    OAuth2AuthorizedClient client = authorizedClientService
      .loadAuthorizedClient(
        authentication.getAuthorizedClientRegistrationId(), 
          authentication.getName());
    String userInfoEndpointUri = client.getClientRegistration()
  .getProviderDetails().getUserInfoEndpoint().getUri();

if (!StringUtils.isEmpty(userInfoEndpointUri)) {
    RestTemplate restTemplate = new RestTemplate();
    HttpHeaders headers = new HttpHeaders();
    headers.add(HttpHeaders.AUTHORIZATION, "Bearer " + client.getAccessToken()
      .getTokenValue());
    HttpEntity entity = new HttpEntity("", headers);
    ResponseEntity <map>response = restTemplate
      .exchange(userInfoEndpointUri, HttpMethod.GET, entity, Map.class);
    Map userAttributes = response.getBody();
    model.addAttribute("name", userAttributes.get("name"));
}
```
El objeto OAuth2AuthenticationToken viaja con las requests inyectado por Spring, que ya nos trae el getAuthorizedClientRegistrationId(), con eso es suficiente para mapearlo en nuestro esquema de permisos. No obstante, si queremos más información, el objeto nos trae userInfoEndpointUri que a partir de request nos permite traer la info “expuesta” para mapearla y utilizarla según el caso.
5. Una vez terminado lo anterior, procedemos a configurar el SecurityContext (context que contiene toda la info de autenticación). En nuestro caso, como es una aplicación web, debemos controlar la seguridad en cada request recibido. Ya en este punto tenemos configurado OAuth, pero ahora debemos unir nuestro contexto de seguridad a cada request. Considerando que debemos autenticar cada request, el único punto del sistema donde pasan todas las requests que eventualmente serán ruteadas es justamente el gateway y las peticiones se manejan a través de una cadena de filtros. Por lo tanto, es razonable pensar que habrá un filtro que gestione la autenticación también. Repasando la explicación anterior del funcionamiento de la autenticación, vemos que en cada request recibiremos uno o varios headers que tendrán la información necesaria de autenticación para ser procesada.
6. En este punto, se realiza la configuración del SecurityContext. Este context contendrá toda la información del usuario autenticado, información que se utilizará para los chequeos de seguridad. Este context tiene información suficiente para dar soporte de OAuth contra una app desktop o de consola. En nuestro caso, una web con requests, tenemos que validar cada una. Si quisiéramos validar la seguridad en un solo punto —en cada ruta que nos llega—, el punto donde nos llega el lugar a modificar es Spring Cloud Gateway. Repasando un poco, Spring Cloud Gateway manejaba una serie de filtros, entonces, deberíamos configurar algún filtro especial que acepte un request con un determinado token.
   
```yml
spring:
  application:
    name: gateway
  cloud:
    gateway:
      default-filters:
        - TokenRelay
      routes:
        - id: XXXXX
          uri: XXXXXXXX
          predicates:
            - Path=/XXXXXX/**
          filters:
            - RemoveRequestHeader=Cookie
```

Cuando vimos API Gateway, aprendimos cómo agregar rutas, predicados y algunos filtros especiales. Ahora, vamos a agregar un filtro especial que pasa por todas las rutas. Dentro de default-filters agregamos el filtro TokenRelay. Toda la implementación por detrás la genera Spring. Spring Security se encarga de verificar que el token sea válido, pero a nosotros nos interesa que con la información de este token podemos pedirle información al proveedor de seguridad.
Si volvemos hacia el tema “¿Cómo funciona la autenticación web?”, podemos observar que en cada request contra el server se envía un token en el tag Authorization y, del lado del server, este Token se verifica. La implementación de Spring funciona de la siguiente manera: nos llega un request, lo “captura” Spring Security y luego este pasa por una serie de filtros encadenados que terminan determinando las rutas. Estos filtros en definitiva son funciones que reciben el request y, si el mismo es afectado por el filtro, rechaza la operación o se modifica el request; caso contrario, se prosigue con los demás filtros. El filtro del TokenRelay lo que hace es tomar el token de autenticación que llega en el request al gateway (verificado por Spring Security) y lo pasa al resto de la cadena de filtros para poder extraer información del mismo, hacer más chequeos de seguridad o para forwardear el token de autenticación a otros servicios.




