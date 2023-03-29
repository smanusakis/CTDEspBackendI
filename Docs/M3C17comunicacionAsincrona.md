### Comunicación sincrónica vs. asincrónica

Como hemos visto, la arquitectura de microservicios es la más utilizada en el mercado. Es una alternativa a los monolitos (una única aplicación que contiene todas las funcionalidades juntas), que se basa en dividir las funcionalidades en servicios separados para aportarnos numerosos beneficios. Pero, dado que todos los servicios de la aplicación están divididos por sus respectivas funciones, ¿cómo podemos integrarlos? ¿Cómo se comunican? En esta clase, veremos cómo hacerlo.

## ¿Qué es RabbitMQ?
RabbitMQ es un servidor de mensajes de código abierto (open-source) desarrollado con el lenguaje de programación Erlang. También se lo conoce como un Message Broker, en referencia al concepto de un sistema de servidor de mensajes. Su función es servir de “intermediario” entre quien produce el mensaje y quien lo consumirá. Para ejemplificar esto, podemos hacer una asociación con algo de nuestro día a día, como un agente inmobiliario: esta persona, con este rol, intermediará la negociación entre el cliente (la persona que quiere comprar) y la persona vendiendo la propiedad.

## Spring Cloud Stream
Tolerancia a fallas, desacoplamiento, escalabilidad, arquitecturas dirigidas por eventos, alta disponibilidad… Todos estos términos y conceptos implican el conjunto de requisitos no funcionales que los sistemas modernos de gran demanda y gran escala buscan cumplir. En las aplicaciones modernas, la arquitectura está compuesta —de forma distribuida— por varios elementos que se encargan de la orquestación y comunicación entre los microservicios. Los Message Brokers son una parte sumamente importante dentro de estas arquitecturas, ya que permiten garantizar una mayor escalabilidad para la aplicación y también posibilitan la comunicación entre servicios.

Con el crecimiento y popularidad de los Message Brokers, surgieron algunas opciones que ganaron mayor popularidad por su robustez y simplicidad de uso. Entre los más populares se encuentran: **Apache Kafka y RabbitMQ**. A pesar de que la documentación de ambos es bastante completa, usar estos Message Brokers no es nada sencillo, entenderlos y usarlos tiene una curva de aprendizaje significativa.

Ante la necesidad de utilizar Message Brokers por parte de las arquitecturas modernas y la existencia de diferentes proveedores siendo adoptados por la comunidad, Pivotal (mantenedora de proyectos Spring) creó un nuevo módulo donde agrupa a todas estas tecnologías de mensajería asincrónica, llamado **Spring Cloud Stream**, haciendo lo que Spring Framework sabe hacer como pocos: ¡hacer la vida más fácil a los desarrolladores!