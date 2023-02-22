TENER EN CUENTA:

Eureka Client:
cuando configuramos el proyecto hay que agregar en el app. properties:

spring.application.name = user-service <br>
server.port = 8082 <br>
eureka.client.service-url.defaultZone = http://localhost:8761/eureka/<br>

Para poder habilitar múltiples instancias de un micro, primero tengo que cambiar la configuración permitiendo múltiples instancias:
![](Docs/img/M2_configCliente.png)

y anrtes de correr la aplicación, tengo que indicar un puerto nuevo para que no genere conflicto:

spring.application.name = user-service <br>
server.port = 8083 <br>
eureka.client.service-url.defaultZone = http://localhost:8761/eureka/<br>

![](Docs/img/M2_dashboard.png)

