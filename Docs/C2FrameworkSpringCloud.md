SPRING CLOUD NETFLIX (2018)

Frameworks y librerías para resolver problemas de sistemas distribuidos a gran escala como:
- descubrimiento de servicios
- balanceo de cargas
- tolerancia a fallos
- ruteo
- configuración

Eureka es un servicio REST que registra y localiza microservicios existentes, informar de su localización, estado y datos relevantes de cada uno.

Durante su arranque, cada microservicio se comunicará con el servidor  EUREKA para notificar si está disponible, su ubicación y metadatos de esta forma, Eureka mantendrá en su registro la info de todos los micros del sistema.
Los micros informan su estado cada 30'' en lo que se denomina **HEARTBEAT**. Si después de 3 períodos (90'') Eureka no recibe notificación, se elimina del registro. Del mismo modo, si recibe 3 notificaciones seguidas, considerará al servicio disponible de nuevo.

Además, EUREKA permite aumentar o disminuir las instancias de nuestro microservicio de manera dinámica y transparente para el resto de los microservicios.

Es decir, si tenemos 2 microservicios, A y B, y A consume un endpoint que expone la Api de B, ubicada en una URL formada por una dirección y un puerto. ¿Qué pasa si deployamos una nueva instancia de B o eliminamos la instancia actual y creamos otra?, tendríamos que buscar en el código y modificar la URL. Gracias a EUREKA podemos ubicar los servicios usando el nombre x el que se registrarion y no su IP (x en https://servicio-b). En este sentido es transparente, porque una nueva instancia de B será registrada por EUREKA con el mismo nombre.

Además, podemos integrar EUREKA con un **LOAD BALANCER** que permitirá enviar la petición al servidor que mejor prepardo esté para ejecutarla es decir, al microservicio A le es indiferente cuántas instancias de, micro B haya y su ubicación, ya que siempre se comunicará de la misma manera. 

¿Cómo funciona Eureka?

Hay dos grandes componentes
- EUREKA Server
- EUREKA Client

por un lado, vamos a tener un servicio donde se va a ejecutar EUREKA SERVER y por otro, cada microservicio de nuestro ecosistema deberá configurarse como un cliente de EUREKA, de esta manera, todos los cientes tendrán comunicación con el servidor de Eureka.

En conclusión, más allá de la tencología, tener un service registry, nos permite abstraernos de la ubicación física del microservicio y abstraernos y conocer el estado del ecosistema de microservicios, aumentando la tolerancia a fallos y permitiéndonos escalar dinámicamente.

