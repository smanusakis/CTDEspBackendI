1) crear un repo público.
2) copiar y pegar config data dentro del repo privado para la entrega pero para probarlo nosotros, tiene que estar en un público (que no se entrega). Luego la carpeta la copio y pego en el repo privado al nivel del resto de las carpetas de las apis
3) El parcial pide 2 micros de negocio, solo película accede a la base de datos. CAtálogo se expone al api gateway es el **único** movies no tiene que estar en el api gateway el customer solo le pega al apigateway y esto solo a catálogo ((todo se prueba desde api gateway))
4) El response es una lista de películas. catálogo y película van a tener elmismo response solo que uno consume a otro.
5) buscar x g´nero tiene que estar en película y va a devolver esa lista. catálogo va a forwardear esa respuesta, va a invocar el feing de película.
6) No agregar el test, esto se evalúa en el final.
7) Los 2 microservicis con sus application yml en el config. el eureka
8) ruteo correcto y con balanceador de carga. usar el feing par consumir pelicula x catálogo
9) que estén los 5 microservicios.


El microservicio api-movie es el único que tiene conexión con la base de datos.
La base de datos en un mysql. Pueden usar docker para levantar la instancia, o una base de datos mysql con un servidor local. (esto como prefieran) 
El microservicio api-catalog es el único que se comunica con api-movie.
Los temas que se evalúan y deben implementar son: EurekaServer, ConfigServer, ApiGateway(sin seguridad), LoadBalance, Feign. 
Si bien el enunciado dice hacer un test, en esta instancia de evaluación no se va considerar a la hora de puntuar, ya que testing lo evaluamos en el final. 
Cualquier duda que tengan por favor utilizar el canal general de discord. No se responden preguntas por privado.
El parcial debe estar subido completo al github de ustedes.
Para hacer oficial la entrega del parcial deben completar el siguiente Google Forms, indicando apellido y nombre, y url del repositorio de github. Deben loguearse con la misma cuenta que está registrada en el sistema de Digital, caso contrario no será considerada esa entrega. 