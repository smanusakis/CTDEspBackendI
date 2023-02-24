TENER EN CUENTA:

Eureka Server:

en Application.properties:

server.port = 8761 <br>
eureka.client.register-with-eureka = false<br>
eureka.client.fetch-registry = false<br>

y en main agregar la anotación:<span style=color:yellow> @EnableEurekaServer </span>


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

OJO TENGO QUE AGREGARLE UNA DEPENDENCIA DE JDK 11 que es un plug ing que no volvió a estar dps de la verisón 11

2[.9. JDK 11 Support](https://docs.spring.io/spring-cloud-netflix/docs/current/reference/html/#jdk-11-support)
The JAXB modules which the Eureka server depends upon were removed in JDK 11. If you intend to use JDK 11 when running a Eureka server you must include these dependencies in your POM or Gradle file.

<dependency >
    <groupId >org.glassfish.jaxb</groupId >
    <artifactId >jaxb-runtime</artifactId >
</dependency >
