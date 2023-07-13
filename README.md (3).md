

# Práctica a desarrollar

Se tiene que desarrollar una ontología para una posible aplicación de agentes que puedan desarrollar los siguientes juegos:

1.  [Juego del Parchís](https://www.ludoteka.com/juegos/parchis/reglas).
2.  [Juego del Backgammon](https://www.ludoteka.com/juegos/backgammon/reglas).
3.  Juego de la Escalera:
    -   El juego de la escalera con casillas, serpientes y escaleras es un juego de mesa clásico que consiste en avanzar por el tablero desde el inicio hasta el final. Cada jugador tira un dado y mueve su ficha el número de casillas que le salga. Si cae en una casilla con el inicio de una escalera, sube por ella hasta la casilla que indique el otro extremo. Si cae en una casilla con la cola de una serpiente, baja por ella hasta la casilla que indique la cabeza. El primer jugador que llegue a la última casilla del tablero gana el juego.
4.  [Juego de los gatos y el ratón](http://www.victorblasco.es/games/GatoRaton/instructions.php):

-   Se utiliza el tablero de ajedrez con 4 fichas que representan a los gatos y una ficha que representa al ratón.
-   El ratón y los gatos se colocan inicialmente en lados opuestos del tablero y en una casilla del mismo color, por ejemplo la negra.
-   El ratón puede moverse a cualquier casilla vacía en diagonal, pero los gatos solo pueden moverse hacia adelante en diagonal.
-   No se puede capturar ninguna pieza.
-   Los turnos de movimiento son alternos entre el ratón y uno de los gatos. El ratón empieza moviendo primero.
-   Gana el ratón si llega a la última fila del tablero y ganan los gatos si el ratón no puede moverse más.  
    

Para el desarrollo de la ontología hay que tener en cuenta lo siguiente:

-   Hay que definir la ontología de dominio con todos los elementos comunes a los cuatro juegos. También deberá tenerse presente la posibilidad de añadir nuevos juegos de tablero. Por tanto hay que diseñar la ontología pensando en esta eventualidad.
-   Hay que definir una ontología para cada uno de los juegos con los elementos que que añaden a la ontología de dominio que permiten desarrollar el juego en cuestión.
    -   Como son cuatro juegos, cada miembro del equipo deberá ocuparse del diseño de dos ontologías de juego.

Además de la ontología de juego hay que definir el esquema de comunicación y protocolos FIPA para resolver una partida para cada uno de los juegos.

**ACLARACIÓN**: La ontología de dominio ha sido diseñado por los dos componentes del grupo simultáneamente, para la parte específica de cada juego (diseño de la Ontología del juego y Protocolos FIPA utilizados) se ha desarrollado de la siguiente forma: 
- Alejandro Ramos Gallego (Juego del Parchís y Juego de la Escalera).
- Ángel Barrero García (Juego BackGammon y Juego de los Gatos y el Ratón)
# Análisis de la ontología

Nuestra ontología debe responder apropiadamente a las siguientes preguntas:

1.  ¿Cómo diferenciar a los diferentes agentes especializados?
2.  ¿Cómo proponer a los diferentes jugadores que participen en un juego?
3.  ¿Cómo debe completarse un juego ?
4.  ¿Cómo obtener el resultado del juego propuesto?
5.  ¿Cómo generar las partidas que componen un juego?
6.  ¿Cómo completar un turno de una partida? ¿Cómo completar la partida?
7.  ¿Cómo informar del resultado final de la partida?

## 1.1 ¿Cómo diferenciar a los diferentes agentes especializados? 
Para resolver esta pregunta utilizaremos la utilidad del servicio de páginas amarillas que nos proporciona la plataforma de agentes-FIPA. De esta forma no será necesario tener elementos en la ontología para poder resolver el problema de comunicación pero sí será necesario un elemento en el vocabulario para que los agentes puedan subscribirse en el servicio de páginas amarillas de forma homogénea:

***TipoServicio***: Los agentes especializados tendrán asociado un tipo de servicio y utilizan este elemento del vocabulario. Los agentes especializados serán:

-   **JUGADOR**: Representa el tipo de servicio para cualquier agente jugador de alguno de los tipos de juegos representados en la ontología. 
Este agente representa a un jugador en los juegos. 
	- En el caso del Parchís y Backgammon, este agente manejaría la lógica de juego para un jugador individual, tomando decisiones basadas en el estado actual del tablero. 
	- En el Juego de la Escalera y el Juego de los Gatos y el Ratón, este agente también manejaría la lógica de movimiento y estrategia.
-   **TABLERO**: Representa el tipo de servicio para cualquier agente que completa una partida para alguno de los tipos de juegos representados en la ontología.
	- En el Parchís y Backgammon, este agente mantendría el estado actual del tablero, incluyendo la posición de todas las fichas. 
	- En el Juego de la Escalera y el Juego de los Gatos y el Ratón, este agente también manejaría la lógica de las serpientes, escaleras, y el movimiento de los gatos.

-   **ORGANIZADOR**: Representa el tipo de servicio que proporcionan los agentes que se encargarán de la organización de las partidas que representan un juego, de alguno de los tipos de la ontología, y computarán el resultado.

***TipoJuego***: Representa a los tipos de juegos representados en la ontología y que formará parte del nombre del servicio del agente y también forma parte del vocabulario.

-   PARCHIS
-   BACKGAMMON
-   JUEGO_ESCALERA
-   GATOS_Y_RATON


## 1.2 ¿Cómo proponer a los diferentes jugadores que participen en un juego?
El **AgenteMonitor** será el encargado de proponer un juego, de los tipos que tiene conocimiento, a los AgentesJugador correspondientes a ese tipo de juego. Cada juego debe tener una identificación única y los jugadores pueden tomar su decisión de participar en el juego basándose en su estado actual.

Los elementos de la ontología que formarán parte del contenido del mensaje enviado al agente son los siguientes:

-   **ProponerJuego**: Contiene la información necesaria para que los agentes puedan tomar su decisión.
	-   Juego: Representa el juego en el que se espera que participe el jugador.
		-   idJuego: Identificará de manera única el juego.
		-   TipoJuego: Uno de los tipos de juegos disponibles en la ontología, que pueden ser Parchís, Backgammon, Juego de la Escalera, Juego de los Gatos y el Ratón.
	-   Modo: Define el desarrollo del juego, lo que determina el número de partidas que cada jugador deberá completar en el juego. Puede ser UNICO, ELIMINATORIA o TORNEO.
	-   InfoJuego: Es un concepto abstracto que permite representar los datos necesarios de un juego, permitiendo extender la ontología con nuevos tipos de juegos.

Además, se incluyen las especificaciones para cada juego, como Parchís, Backgammon, Juego de la Escalera, Juego de los Gatos y el Ratón, que contienen los atributos necesarios para cada juego.

**1.  Parchís:**
    
   -   Tablero: dimensiones del tablero para un juego de Parchís.
   -   numJugadores: número de jugadores para el juego, normalmente 4.
   -   numFichas: número de fichas que dispone un jugador, normalmente 4.
   
**2.  Backgammon:**
    
   -   Tablero: dimensiones del tablero para un juego de Backgammon.
   -   numJugadores: número de jugadores para el juego, normalmente 2.
   -   numFichas: número de fichas que dispone un jugador, normalmente 15.
   
**3.  Juego de la Escalera:**
    
   -   Tablero: dimensiones del tablero para un juego de la Escalera, normalmente 10x10.
   -   numJugadores: número de jugadores para el juego, puede variar.
   -   numEscaleras: número de escaleras en el tablero.
   -   numSerpientes: número de serpientes en el tablero.
   
**4.  Juego de los gatos y el ratón:**
    
   -   Tablero: dimensiones del tablero para un juego de los gatos y el ratón, normalmente 8x8.
   -   numGatos: número de gatos en el juego, normalmente 4.
   -   numRatones: número de ratones en el juego, normalmente 1.

Estos atributos son necesarios para definir las reglas básicas y el estado inicial de cada juego. Dependiendo de la complejidad de los agentes y las estrategias de juego que se quieran implementar,  se podría necesitar atributos adicionales.

![Diagrama de clases UML](https://showme.redstarplugin.com/d/oueE3I4R)

-   **Justificacion**: Es un elemento del vocabulario y contendrá las posibilidades que tiene el agente para indicar el rechazo para participar en el juego.
-   **JuegoAceptado**: Permite al agente indicar que desea participar en el juego, ya sea como jugador u organizador.

Estos elementos permiten una comunicación efectiva entre los agentes para la propuesta y aceptación de juegos.



## 1.3 ¿Cómo debe completarse un juego ?
Una vez que el AgenteMonitor ha identificado a los jugadores dispuestos a participar en un juego, es necesario localizar a un AgenteOrganizador que se encargará de generar las partidas necesarias para completar ese juego.

![Diagrama de secuencia](https://showme.redstarplugin.com/d/slHKPbn5)
Los elementos de la ontología que formarán parte del contenido del mensaje enviado al agente son los siguientes:
-   **CompletarJuego**: Contiene la información necesaria para generar las partidas individuales que permitirán completar el juego propuesto.
-   **Juego**: Representa el juego que se debe completar.
-   **Modo**: Define el desarrollo del juego, lo que determina el número de partidas que cada jugador deberá completar en el juego. Puede ser UNICO, ELIMINATORIA o TORNEO.
-   **InfoJuego**:  Permite representar los datos necesarios de un juego, permitiendo extender la ontología con nuevos tipos de juegos.
-   **ListaJugadores**: Es una colección de elementos Jugador, y deben ser al menos dos, que participarán en las partidas que definen el juego.

	-   **Parchís**: En este juego, cada jugador tiene cuatro fichas que deben completar un recorrido alrededor del tablero y llegar a la meta. El AgenteOrganizador debe generar las partidas de manera que cada jugador tenga oportunidad de mover sus fichas basándose en los resultados de los dados. El juego se completa cuando todas las fichas de un jugador han llegado a la meta.
    
	-   **Backgammon**: Este es un juego para dos jugadores, donde cada uno tiene quince fichas que deben moverse alrededor del tablero en direcciones opuestas, según los resultados de los dados. El AgenteOrganizador debe generar las partidas de manera que cada jugador tenga oportunidad de mover sus fichas. El juego se completa cuando un jugador ha sacado todas sus fichas del tablero.
    
	-   **Juego de la Escalera**: En este juego, los jugadores avanzan en el tablero basándose en los resultados de los dados, y pueden subir o bajar dependiendo de si caen en una escalera o una serpiente. El AgenteOrganizador debe generar las partidas de manera que cada jugador tenga oportunidad de mover sus fichas. El juego se completa cuando un jugador llega a la última casilla del tablero.
    
	-   **Juego de los Gatos y el Ratón**: En este juego, se utilizan cinco jugadores: un ratón y cuatro gatos. Los movimientos son alternos entre el ratón y los gatos, y el juego termina cuando el ratón llega a la última fila del tablero o no puede moverse más. El AgenteOrganizador debe generar las partidas de manera que cada jugador tenga oportunidad de mover sus fichas.

## 1.4 ¿Cómo obtener el resultado del juego propuesto?
Para que el AgenteMonitor pueda obtener la información del resultado de los juegos que ha propuesto a varios AgentesOrganizadores, se requiere el siguiente flujo de mensajes:
**Estos componentes serían generales a todos los juegos.**
Los componentes de la ontología que se incorporarán en el contenido del mensaje que se envía al agente son los siguientes:

-   **InformarResultado**: Este componente representa la información que el agente desea recibir sobre el resultado del juego.
-   **AgenteJuego**: Este componente representa al agente especializado que está interesado en recibir la información. Es un concepto abstracto que permite representar a los agentes especializados y ofrece la posibilidad de extender la ontología para acomodar la adición de más agentes especializados en el futuro.
-   **SubInform**: Este es un componente abstracto que representa las diferentes tipos de información que el agente especializado puede recibir. En este caso, los valores pueden ser ClasificacionJuego o IncidenciaJuego, que abarcan las posibles formas de finalización de un juego.
-   **ClasificacionJuego**: Si el juego ha concluido correctamente, se envía la información relacionada con la clasificación del juego y los jugadores involucrados.
	-   Juego: Este componente representa el juego que ha concluido.
	-   ListaJugadores: Esta es una colección de elementos Jugador que han participado en el juego, ordenados desde el ganador en adelante.
	-   ListaPuntuacion: Esta es una colección con los puntos obtenidos por cada uno de los jugadores de la lista anterior.
-   **IncidenciaJuego**: Si el juego no finaliza de manera normal, este componente indicará la razón. Los posibles valores recogidos en el vocabulario para una finalización incompleta del juego son CANCELADO y JUGADORES_INSUFICIENTES.

![Diagrama de secuencia](https://showme.redstarplugin.com/d/zVPNgQlN)
1.  El AgenteMonitor envía un mensaje al AgenteOrganizador correspondiente, solicitando la información del resultado del juego propuesto.
2.  El AgenteOrganizador recibe el mensaje y analiza la solicitud.
3.  Si el juego ha finalizado correctamente, el AgenteOrganizador envía un mensaje de respuesta al AgenteMonitor con el componente ClasificacionJuego, que incluye la información sobre la clasificación del juego, los jugadores involucrados y sus puntuaciones.
4.  Si el juego no ha finalizado de manera normal, el AgenteOrganizador envía un mensaje de respuesta al AgenteMonitor con el componente IncidenciaJuego, indicando la razón de la finalización incompleta del juego (CANCELADO o JUGADORES_INSUFICIENTES).
5.  En caso de que se solicite el resultado de una partida individual, el AgenteOrganizador enviará un mensaje de respuesta al AgenteMonitor con el componente ResultadoPartida, que contiene la información sobre el resultado de esa partida específica.
## 1.5 ¿Cómo generar las partidas que componen un juego?
El AgenteOrganizador tiene la tarea de crear las rondas necesarias para finalizar el juego. El número total de rondas se determinará por el atributo Modo del elemento CompletarJuego. En cada ronda, se crean varias partidas que deben ser gestionadas por el AgenteTablero.

![Diagrama](https://showme.redstarplugin.com/d/W9nFHgGr)


El diagrama ilustra los componentes de la ontología que deben ser incluidos en el contenido del mensaje enviado al agente. Estos componentes de la ontología tendrán los siguientes atributos:

-   **CompletarPartida**: Incluye la información requerida de la partida que se va a jugar.
-   **Partida**: Información de la partida que se ha completado.
-   **idPartida**: Proporciona una identificación única a la partida que pertenece a un juego específico.
-   **Juego**: El juego al que pertenece la partida.
-   **ronda**: Señala la ronda de la partida dentro del juego.
-   **maxRondas**: Indica el número máximo de rondas que el juego tendrá.
-   **InfoJuego**: Es un concepto abstracto que permite representar la información necesaria de un juego. Esto permite la extensión de la ontología para incluir nuevos tipos de juegos.
-   **ListaJugadores**: Es un conjunto de elementos Jugador, y debe haber al menos dos, que participarán en la partida. Este atributo permite la inclusión de más jugadores en la partida, es decir, que el número de jugadores sea mayor que 2.

## 1.6 ¿Cómo completar un turno de una partida? ¿Cómo completar la partida?
El **AgenteTablero** es el encargado de organizar los turnos que componen una partida. Para completar la partida, se deben generar los turnos necesarios hasta obtener un ganador o declarar un empate.

Para cada uno de los juegos, los elementos de la ontología necesarios para completar un turno de juego son los siguientes:


## **Juego del Parchís:**

1.  **PedirMovimientoParchis**: Contiene la información necesaria para completar el turno de juego.
    
    -   **Partida**: Identifica la partida a la que corresponde el turno.
    -   **Jugador**: Indica el jugador activo que debe realizar el movimiento.
2.  **EstadoPartidaParchis**: Proporciona información sobre posibles contingencias que pueden ocurrir durante la partida.
    
    -   **Partida**: Identifica la partida a la que corresponde el turno.
    -   **Estado**: Representa el estado actual de la partida. Los posibles valores pueden incluir: GANADOR, ABANDONO, SEGUIR_JUGANDO, FIN_PARTIDA, JUGADOR_NO_ACTIVO.
3.  **MovimientoEntregadoParchis**: Contiene la información del movimiento realizado por el jugador activo.
    
    -   **Partida**: Identifica la partida a la que corresponde el turno.
    -   **Movimiento**: Representa el movimiento válido realizado por el jugador.
4.  **FichaJuegoParchis**: Representa las fichas utilizadas en el juego del Parchís.
    
    -   **Posicion**: Indica la posición actual de la ficha en el tablero (casilla).

## **Juego del Backgammon:**

1.  **PedirMovimientoBackgammon**: Contiene la información necesaria para completar el turno de juego.
    
    -   **Partida**: Identifica la partida a la que corresponde el turno.
    -   **Jugador**: Indica el jugador activo que debe realizar el movimiento.
2.  **EstadoPartidaBackgammon**: Proporciona información sobre posibles contingencias que pueden ocurrir durante la partida.
    
    -   **Partida**: Identifica la partida a la que corresponde el turno.
    -   **Estado**: Representa el estado actual de la partida. Los posibles valores pueden incluir: GANADOR, ABANDONO, SEGUIR_JUGANDO, FIN_PARTIDA, JUGADOR_NO_ACTIVO.
3.  **MovimientoEntregadoBackgammon**: Contiene la información del movimiento realizado por el jugador activo.
    
    -   **Partida**: Identifica la partida a la que corresponde el turno.
    -   **Movimiento**: Representa el movimiento válido realizado por el jugador. Puede incluir el número de casillas a mover y la dirección (avance o retroceso).
4.  **FichaJuegoBackgammon**: Representa las fichas utilizadas en el juego del Backgammon.
    
    -   **Posicion**: Indica la posición actual de la ficha en el tablero (casilla).
    

## **Juego de la Escalera:**

1.  **PedirMovimientoEscalera**: Contiene la información necesaria para completar el turno de juego.
    
    -   **Partida**: Identifica la partida a la que corresponde el turno.
    -   **Jugador**: Indica el jugador activo que debe realizar el movimiento.
2.  **EstadoPartidaEscalera**: Proporciona información sobre posibles contingencias que pueden ocurrir durante la partida.
    
    -   **Partida**: Identifica la partida a la que corresponde el turno.
    -   **Estado**: Representa el estado actual de la partida. Los posibles valores pueden incluir: GANADOR, ABANDONO, SEGUIR_JUGANDO, FIN_PARTIDA, JUGADOR_NO_ACTIVO.
3.  **MovimientoEntregadoEscalera**: Contiene la información del movimiento realizado por el jugador activo.
    
    -   **Partida**: Identifica la partida a la que corresponde el turno.
    -   **Movimiento**: Representa el resultado del lanzamiento de un dado, es decir, el número de casillas que el jugador debe mover su ficha.
4.  **FichaJuegoEscalera**: Representa las fichas utilizadas en el juego de la Escalera.
    
    -   **Posicion**: Indica la posición actual de la ficha en el tablero (casilla).
5.  **CasillaEscalera**: Representa una casilla especial en el tablero que puede contener el inicio de una escalera o la cola de una serpiente.
    
    -   **InicioEscalera**: Indica la casilla en la que comienza una escalera.
    -   **FinEscalera**: Indica la casilla a la que se llega al subir por una escalera.
    -   **ColaSerpiente**: Indica la casilla en la que se encuentra la cola de una serpiente.
    -   **CabezaSerpiente**: Indica la casilla a la que se llega al bajar por una serpiente.

## **Juego de los Gatos y el Ratón:**

1.  **PedirMovimientoGatosRaton**: Contiene la información necesaria para completar el turno de juego.
    
    -   **Partida**: Identifica la partida a la que corresponde el turno.
    -   **Jugador**: Indica el jugador activo que debe realizar el movimiento.
2.  **EstadoPartidaGatosRaton**: Proporciona información sobre posibles contingencias que pueden ocurrir durante la partida.
    
    -   **Partida**: Identifica la partida a la que corresponde el turno.
    -   **Estado**: Representa el estado actual de la partida. Los posibles valores pueden incluir: GANADOR_RATON, GANADOR_GATOS, SEGUIR_JUGANDO, FIN_PARTIDA.
3.  **MovimientoEntregadoGatosRaton**: Contiene la información del movimiento realizado por el jugador activo.
    
    -   **Partida**: Identifica la partida a la que corresponde el turno.
    -   **Movimiento**: Representa la casilla a la que el jugador mueve su ficha en el tablero.
4.  **FichaJuegoGatosRaton**: Representa las fichas utilizadas en el juego de los gatos y el ratón.
    
    -   **Posicion**: Indica la posición actual de la ficha en el tablero de ajedrez.
5.  **TableroGatosRaton**: Representa el tablero de ajedrez utilizado en el juego.
    
    -   **Casilla**: Indica el estado de una casilla del tablero, que puede estar vacía, ocupada por el ratón o por uno de los gatos.

## 1.7 ¿Cómo informar del resultado final de la partida?
Un AgenteOrganizador o AgenteJugador que esté involucrado en partidas supervisadas por un AgenteTablero necesita estar al tanto de los resultados de las partidas que el AgenteTablero finaliza. La información que el AgenteTablero envía debe identificar al agente solicitante para asegurar que solo reciba los resultados de las partidas en las que ha participado.

![Diagram](https://showme.redstarplugin.com/d/mGgOL1ov)


Para cada uno de los juegos, los componentes de la ontología que se incluirán en el contenido del mensaje enviado al agente son los siguientes:

-   **SubInform**: Componente que representa las diferentes formas de información que el agente especializado puede recibir. En este caso, los valores pueden ser ResultadoPartida o IncidenciaJuego, que abarcan las posibles formas en que puede finalizar una partida.
    
-   **ResultadoPartida**: Este componente representa el desenlace de la partida. Incluye los detalles de la partida que ha concluido y al ganador de la partida. Si no hay un ganador, se incluirá un Jugador vacío.
   
*Para cada juego:*

- **Parchís:** El desenlace de la partida en el juego del Parchís se determina por el jugador que ha conseguido llevar todas sus fichas a la meta antes que los demás. En este juego, cada jugador debe mover sus fichas por el tablero en base a los resultados de los dados. El objetivo es avanzar por el tablero y llevar todas las fichas al área de meta. El desenlace de la partida incluirá al jugador que ha logrado llevar todas sus fichas a la meta antes que los demás jugadores. Una vez que un jugador ha movido todas sus fichas al área de meta, se considera el ganador de la partida.

- **Backgammon:** En el juego del Backgammon, el desenlace de la partida se determina por el jugador que ha conseguido retirar todas sus fichas del tablero antes que su contrincante. En este juego, los jugadores mueven sus fichas según los resultados de los dados y tratan de llevarlas a su área de salida y luego fuera del tablero. El desenlace de la partida incluirá al jugador que ha logrado retirar todas sus fichas del tablero antes que su oponente. Una vez que un jugador ha retirado todas sus fichas, se considera el ganador de la partida.

- **Escalera:** En el juego de la Escalera, el desenlace de la partida se determina por el jugador que ha conseguido llegar a la última casilla del tablero primero. Cada jugador tira un dado y mueve su ficha el número de casillas que le salga. El objetivo es avanzar por el tablero desde el inicio hasta el final. El desenlace de la partida incluirá al jugador que ha logrado llegar a la última casilla del tablero primero. Una vez que un jugador ha alcanzado la última casilla, se considera el ganador de la partida.

	**Gatos y el Ratón:** En el juego de los Gatos y el Ratón, el desenlace de la partida se determina de la siguiente manera: si el ratón ha conseguido llegar a la última fila del tablero, el ratón gana la partida. Por otro lado, si el ratón no puede moverse más y queda bloqueado, los gatos ganan la partida. El desenlace de la partida incluirá si el ratón ha logrado llegar a la última fila del tablero (en cuyo caso el ratón gana) o si el ratón no puede moverse más y queda bloqueado (en cuyo caso los gatos ganan).

# Diseño de la Ontología 
En la sección previa, hemos introducido todos los componentes y conexiones que forman parte de la ontología requerida para llevar a cabo los juegos propuestos. Al diseñar la ontología, debemos tener en cuenta su implementación utilizando las funcionalidades que nos ofrece la biblioteca de agentes Jade. Por lo tanto, se mostrarán diagramas de clases que ilustran las relaciones entre estos componentes. Se presentarán tres diagramas, cada uno centrado en un tipo de elemento:

-   Concept: Estos elementos representan la información necesaria para describir los diferentes juegos incluidos en la ontología.
    ![Diagrama de clases](https://showme.redstarplugin.com/d/wQYp2bsn)
    
-   AgentAction: Estos elementos representan los eventos a los que los agentes reaccionan para llevar a cabo los juegos en la ontología.
![Diagrama de Clase](https://showme.redstarplugin.com/d/cjiCTtlh)
    
-   Predicate: Estos elementos representan las respuestas a los eventos que permiten completar los juegos en la ontología.
 ![Diagrama de clases](https://showme.redstarplugin.com/d/X8EBtMsJ)

## 2.4 Protocolos FIPA para las tareas de comunicación. 
### Juego del Parchís

-   **Protocolo FIPA-Request**: Este protocolo se puede utilizar para que un agente solicite información o realice una acción a otro agente. En el juego del Parchís, un agente podría utilizar este protocolo para enviar una solicitud de movimiento a otro agente que represente a un jugador. El agente emisor de la solicitud enviaría los detalles del movimiento deseado, como la ficha a mover y la posición de destino. El agente receptor procesaría la solicitud y respondería con la confirmación del movimiento o informando que el movimiento no es válido.
    
-   **Protocolo FIPA-Subscribe**: Este protocolo puede utilizarse para establecer una suscripción a eventos relevantes en el juego. Por ejemplo, los agentes podrían suscribirse a eventos como "ficha movida", "cambio de turno" o "partida finalizada". Cuando se produce un evento suscrito, el agente emisor enviaría una notificación a los agentes suscritos con los detalles del evento. De esta manera, los agentes estarían informados sobre los cambios en el estado del juego y podrían tomar decisiones en función de esta información.
    
-  **Protocolo FIPA-Contract-Net**: Este protocolo es adecuado para situaciones en las que se requiere la asignación de tareas o la solicitud de propuestas de agentes interesados. En el juego del Parchís, se podría utilizar este protocolo para asignar tareas específicas a los agentes, como la gestión de las reglas del juego, el seguimiento de las fichas o el cálculo de las puntuaciones. Un agente emisor enviaría una solicitud de propuestas a los agentes interesados, quienes responderían con propuestas detallando cómo pueden cumplir la tarea solicitada. El agente emisor evaluaría las propuestas recibidas y seleccionaría la más adecuada para llevar a cabo la tarea.


### Juego del Backgammon
![Diagrama de secuencia](https://showme.redstarplugin.com/d/TvLmIFRE)

### Juego de la Escalera 
![Diagrama de Comunicación](https://showme.redstarplugin.com/d/mtOKXGT2)
### Juego de los gatos y el ratón 
![Diagrama de secuencia](https://showme.redstarplugin.com/d/4KCvfiFI)
