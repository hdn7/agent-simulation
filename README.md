# Proyectos de Simulaci´on Agentes
Colectivo de Simulacio´n
Orientaciones Generales
Cada estudiante debe resolver el proyecto de forma individual. La implementaci´on se debe realizar en Prolog y debe entregarse por correo electr´onico al profesor de conferencia de Simulaci´on (yudy@matcom.uh.cu) con copia a la profesora de conferencia de Programaci´on Declarativa (dafne@matcom.uh.cu) antes del viernes 26 de abril a las 12:00 de la noche. Junto a la implementaci´on se debe enviar un informe de trabajo (un documento en formato pdf). El informe de trabajo debe contener los siguientes elementos:
- Datos generales del estudiante.
- Principales Ideas seguidas para la soluci´on del problema
- Modelos de Agentes considerados (por cada agente se deben presentar dos modelos diferentes)
- Ideas seguidas para la implementaci´on
- Consideraciones obtenidas a partir de la ejecuci´on de las simulaciones del problema (determinar a partir de la experimentaci´on cu´ales fueron los modelos de agentes que mejor funcionaron)
## 1. Marco General
El ambiente en el cual intervienen los agentes es discreto y tiene la forma de un rect´angulo de N ×M. Ningu´n agente u objeto del ambiente puede salir del terreno (chocan contra las paredes que lo rodean). El ambiente es de informaci´on completa, por tanto todos los agentes conocen toda la informaci´on sobre el ambiente. El ambiente var´ıa aleatoriamente cada t unidades de tiempo. El valor de t es conocido. En una unidad de tiempo se realizan las acciones de los agentes por turno. Todos los agentes tienen un identiﬁcador diferente (nu´meros desde 1 hasta n). Inicialmente cada agente toma una decisi´on de la acci´on que va a realizar. Se ordenan todos los agentes segu´n sus identiﬁcadors. Luego uno a uno, los agentes realizan la acci´on determinada (esta puede modiﬁcar el ambiente). Despu´es de que todos los agentes realicen sus acciones pero antes de ﬁnalizar la unidad de tiempo k, si k es multiplo de t se produce un cambio aleatorio del ambiente.

Los elementos que pueden existir en el ambiente son obst´aculos, suciedad, nin˜os, el corral y los agentes que son llamados Robots de Casa. A continuaci´on se precisan las caracter´ısticas de los elementos del ambiente:
- Obst´aculos: estos ocupan una u´nica casilla en el ambiente. Ellos pueden ser movidos, empuj´andolos, por los nin˜os, una u´nica casilla. El Robot de Casa sin embargo no puede moverlo. Solo pueden ser movidos hacia casillas vac´ıas.
Suciedad: la suciedad es por cada casilla del ambiente. Solo puede aparecer en casillas que previamente estuvieron vac´ıas. Esta aparece en el estado inicial o es creada por los nin˜os. Los nin˜os y los Robots de casa pueden caminar sobre la suciedad. La suciedad puede aparecer en las casillas donde est´en los nin˜os o los Robots de Casas pero no en la misma posici´on que los obst´aculos.
- Corral: Las casillas del corral forman una regi´on conexa en el ambiente. (Para dos casillas cuales quiera que sean corral existe un camino moviendose en las cuatro direcciones (arriba, abajo, izquierda, derecha) entre ambas casillas tales que todas las casillas intermedias son corral). Hay la misma cantidad de corrales que de nin˜os presentes en el ambiente. El corral no puede moverse. En una casilla del corral solo puede coexistir un nin˜o. En una casilla del corral, que est´e vac´ıa, puede entrar un robot. En una misma casilla del corral pueden coexistir un nin˜o y un robot solo si el robot lo carga, o si acaba de dejar al nin˜o.
- Nin˜o: Son agentes del ambiente. Los nin˜os ocupan solo una casilla. Un nin˜o selecciona de forma aleatoria una de las 8 casillas adyacentes (no importa que est´e en un borde del terreno). El nin˜o intenta moverse en esa direcci´on. Si en la casilla a la que intenta moverse contiene un obst´aculo, el obst´aculo se mueve en esa misma direcci´on, si hay m´as obst´aculos en dicha direcci´on todos se mueven. Si un obst´aculo est´a en una casilla donde no puede ser empujado se considera como un intento fallido del nin˜o y se mantiene en su misma posici´on. Si intenta moverse fuera del terreno tambi´en se considera como un intento fallido. Los nin˜os son los responsables de que aparezca la suciedad. Antes de realizar el movimiento en la casilla de 3x3 que contiene al nin˜o como centro se ensucia segu´n la siguiente regla: Si hay un nin˜o en la cuadr´ıcula de 3x3 se elige una casilla de forma aleatoria en dicha cuadr´ıcula y si se puede ensuciar se ensucia. Si hay dos nin˜os en la cuadr´ıcula de 3x3 se eligen dos casillas de forma aleatoria en dicha cuadr´ıcula y si se pueden ensuciar se ensucian. Si hay tres o m´as nin˜os en la cuadr´ıcula de 3x3 se eligen seis casillas de forma aleatoria en dicha cuadr´ıcula y si se pueden ensuciar se ensucian. Una casilla se puede ensuciar si est´a dentro de los l´ımites del ambiente (en el rect´angulo de N ×M), no est´a sucia y no contiene un obst´aculo. Los nin˜os cuando est´an en una casilla del corral, ni se mueven ni ensucian. Si un nin˜o es capturado por un Robot de Casa tampoco se mueve ni ensucia.


Robot de Casa: El Robot de Casa se encarga de limpiar y de controlar a los nin˜os. El Robot se mueve a una de las casillas adyacentes, las que decida. Solo se mueve una casilla sino carga un nin˜o. Si carga un nin˜o pude moverse hasta dos casillas consecutivas. Tambi´en puede realizar las acciones de limpiar y cargar nin˜os. Si se mueve a una casilla con suciedad, en el pr´oximo turno puede decidir limpiar o moverse. Si se mueve a una casilla donde est´a un nin˜o, inmediatamente lo carga. En ese momento, coexisten en la casilla Robot y nin˜o. Si se mueve a una casilla del corral que est´a vac´ıa, y carga un nin˜o, puede decidir si lo deja esta casilla o se sigue moviendo. El Robot puede dejar al nin˜o que carga en cualquier casilla. En ese momento cesa el movimiento del Robot en el turno, y coexisten hasta el pr´oximo turno, en la misma casilla, Robot y nin˜o.
## 2. Objetivos
El objetivo del Robot de Casa es mantener la casa (a.k.a el ambiente) limpia. Se considera la casa limpia si el 60% de las casillas vac´ıas no est´an sucias. Se sabe que si la casa llega al 60% de casillas sucias el Robot es despedido e inmediatamente cesa la simulaci´on. Si el Robot ubica a todos los nin˜os en el corral y el 100% de las casillas est´an limpias tambi´en cesa la simulaci´on. Estos son llamados estados ﬁnales. Debe programar el comportamiento del robot por cada turno as´ı como las posibles variaciones del ambiente.
## 3. Ambiente inicial
Como ambiente inicial se especiﬁca el taman˜o del ambiente, el porciento de casillas que aparecen sucias, el porciento de obst´aculos y el nu´mero de nin˜os. El Robot de Casa parte de un posici´on aleatoria y es´el quien realiza el primer turno. Igual, se especiﬁca el valor del tiempo de unidades de cambio (t). Con estos datos se genera un ambiente inicial que cumpla las restricciones previamente planteadas en el Marco General. El ambiente inicial debe ser factible. En caso de que no se logre uno de los estados ﬁnales del ambiente, la simulaci´on debe detenerse cuando hayan transcurrido 100 veces t.
## 4. Experimentos
Para complementar el trabajo, deben realizarse un conjunto de simulaciones (30 por cada ambiente) partiendo de distintos escenarios iniciales (no menos de 10). Se debe reportar, por ambiente generado, el porciento de casillas sucias medio, el nu´mero de veces que el Robot fue despedido y el nu´mero de veces que ubic´o a los nin˜os en el corral y limpi´o toda la casa. Estos datos deben reportarse en el informe.