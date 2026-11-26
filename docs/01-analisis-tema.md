# FORM-IC-01 — Análisis del tema y corpus de fuentes

**Estudiante:** Angello Agustín Mendoza Bermello  
**Asignatura:** Aplicaciones Distribuidas — UTEQ, Ingeniería en Software, 7mo nivel B  
**Semana:** 5 (23 al 29 de noviembre de 2026)  
**Etapa:** investigación del tema y descarga de artículos científicos  
**Fecha de esta etapa:** 25 de noviembre de 2026

---

## 1. Qué se buscó y qué se encontró

La consigna pide elegir un estilo de arquitectura distribuida de la Unidad 2, encontrar una empresa que lo use en producción y analizar el caso en una página. Los estilos disponibles son SOA, microservicios, arquitectura orientada a eventos (EDA) y API Gateway.

El primer caso que se trabajó, Uber, ya estaba registrado por otro compañero en el foro del SGA. Lo mismo pasaba con DoorDash, Airbnb y Wix. Había que cambiar de empresa. Después de revisar alternativas, el caso elegido es **Nubank**, el banco digital brasileño, con una **arquitectura orientada a eventos** para la comunicación entre sus microservicios.

Nubank publica en su blog técnico oficial, *Building Nubank*, dos entradas recientes que sirven para sostener el análisis:

- **«Managing Cloud Limits»** (9 de abril de 2025), de Elton Peixoto y Diogo Dantas. Da las cifras de escala: más de 110 millones de clientes, más de 4 000 microservicios, 72 000 millones de eventos diarios por Kafka y millones de peticiones por segundo.
- **«Evolución de la Arquitectura de Nubank Hacia una Plataforma Reflexiva»** (25 de junio de 2025), una recapitulación de la charla de Bel Gonçalves. Explica el paso de una arquitectura simple, de un solo producto y un solo país, a una plataforma con motores reutilizables y servicios por dominio.

Hay una tercera fuente, **«Microservices at Nubank, an overview»** (12 de agosto de 2019), de Rafael Ferreira. Es la más valiosa para entender el porqué: cuenta que Nubank empezó con microservicios desde el primer día, que eso los hizo más lentos al principio y que la mayor parte de la comunicación entre servicios pasa por un broker de mensajería replicado. Esa frase es la que sostiene la elección de EDA.

El caso es provisional hasta que se registre en el foro del SGA. Si Nubank también estuviera ocupado, habría que pasar a otra empresa; el corpus académico y el esquema del análisis se reutilizan.

---

## 2. Los cuatro estilos, sin confundirlos

Los cuatro nombres aparecen juntos en la Unidad 2, pero no responden a la misma pregunta. SOA y microservicios son formas de organizar el sistema en servicios. EDA es una forma de comunicar esos servicios. API Gateway es un patrón de entrada, no una arquitectura completa. Un mismo sistema puede usar microservicios, eventos y un gateway al mismo tiempo, y Nubank hace algo parecido: microservicios como unidad de despliegue, mensajería asíncrona como forma de comunicación y una plataforma interna que hace las veces de fachada.

| Estilo | Unidad de organización | Comunicación típica | Datos | Despliegue | Qué promete | Qué cuesta |
|---|---|---|---|---|---|---|
| SOA | Servicios de alcance empresarial, a veces grandes | Contratos, bus de servicios (ESB), SOAP/REST | Base compartida o por servicio | Por servicio, con gobierno central | Reutilización e integración entre sistemas | Bus central, gobierno pesado, cuello de botella |
| Microservicios | Servicios pequeños, un contexto acotado por servicio | REST, RPC, mensajería | Una base por servicio, sin compartir tablas | Independiente por servicio | Autonomía de equipos, despliegues pequeños, escalado selectivo | Complejidad distribuida, operación, consistencia |
| EDA | Servicios que publican y consumen eventos | Broker o bus de eventos, asíncrono | Estado derivado de eventos | Independiente | Desacoplamiento temporal, reacción a cambios | Consistencia eventual, trazabilidad, idempotencia |
| API Gateway | Una puerta de entrada | HTTP hacia el cliente, ruteo hacia atrás | No es dueño de los datos | Un componente más | Punto único, autenticación, límites, agregación | Punto único de fallo, salto extra, configuración |

La diferencia entre SOA y microservicios no es solo el tamaño. SOA nació para integrar sistemas heterogéneos dentro de una empresa, y por eso suele apoyarse en un bus y en contratos amplios. Los microservicios nacen de la idea de que cada servicio tenga un límite de negocio claro, su propio equipo y su propio ciclo de despliegue. Esa independencia es la que se busca, y también la que crea el problema: cuando hay cientos o miles de servicios, nadie tiene el mapa completo.

EDA no es lo mismo que microservicios. Se puede tener microservicios que se llaman por HTTP y no publican eventos, y se puede tener un monolito que publica eventos. Lo que cambia con EDA es el acoplamiento temporal: el productor no espera al consumidor. Eso ayuda a que un fallo no se propague de inmediato, pero traslada el problema a la consistencia y a la observabilidad. En Nubank la comunicación principal es asíncrona: el servicio que inicia una operación publica un mensaje en un broker replicado y el siguiente servicio lo consume después. No es una llamada que espera respuesta; es un evento que queda en una cola y se procesa cuando el consumidor puede.

API Gateway es una pieza concreta. Resuelve la entrada: un solo dominio, autenticación, límites de tasa, ruteo, a veces agregación de respuestas. No resuelve la división del sistema ni la consistencia. Confundirlo con una arquitectura lleva a poner un gateway delante de un monolito y creer que ya se tienen microservicios. Nubank tiene una plataforma interna y servicios por dominio, pero no se apoya en un gateway central para todo; su problema principal no es la entrada, sino la comunicación entre servicios y el aislamiento de fallos.

---

## 3. El caso: Nubank y su arquitectura orientada a eventos

### 3.1 Lo que había antes

Nubank nació en 2013 como un banco de un solo producto: tarjeta de crédito, en un solo país, Brasil. La arquitectura inicial era simple, «como la de muchas startups», según la entrada de 2025. No había un monolito gigantesco que se hubiera roto: había una empresa pequeña con un producto acotado.

La decisión llamativa es que **empezaron con microservicios desde el primer día**, en contra del consejo habitual de arrancar con un monolito. Rafael Ferreira lo dice sin adornos: «we started with microservices from day one, defying the standard advice of starting with a monolith». Y también cuenta el costo: «starting with microservices, particularly in 2013, made us slower in the beginning. Putting together a complex provisioning infrastructure while building the product from scratch was a lot for a small team».

Ese es un dato importante para el análisis. No se trata de una empresa que migró de un monolito a microservicios porque el monolito se le rompió. Se trata de una empresa que eligió el camino difícil desde el principio, aceptó que sería más lenta al inicio y lo hizo porque su negocio no era de cambios rápidos de modelo: prefería invertir en una base sólida antes que rehacerla después.

La otra parte del «antes» es la infraestructura. En 2025, el blog describe **«Pangeia»**: la infraestructura agrupada en unas pocas cuentas enormes de AWS. Un incidente menor podía provocar una caída mayor, porque todo estaba atado a un conjunto limitado de cuentas. Aislar ambientes como staging y producción era difícil, y eso frenaba los despliegues.

### 3.2 Por qué EDA

La razón que da el blog de 2019 es la integridad de los datos. Nubank maneja dinero; perder un mensaje no es una opción. En un sistema distribuido, las fallas parciales y las particiones de red son la norma. La respuesta fue apoyarse en la mensajería asíncrona:

> «Most service-to-service interactions are intermediated by a reliable replicated messaging broker. Instead of having the client service wait for the server to finish processing and respond — subject to all sorts of failures due to load imbalances, network glitches, and the like — the initiator service will reliably publish a message to be later consumed by the next service in the flow».

Eso es EDA en el sentido de la consigna: los servicios se comunican publicando y consumiendo eventos. El broker guarda el mensaje, el consumidor lo procesa cuando puede y, si algo falla, el mensaje se puede reencaminar a una *dead letter topic* en lugar de perderse. La consecuencia es que el productor no queda esperando la respuesta del consumidor, y un fallo en el consumidor no se convierte automáticamente en un fallo en cadena.

La otra razón es organizacional. Con 4 000 microservicios, los equipos necesitan autonomía. La mensajería asíncrona permite que un equipo publique un evento y siga trabajando, mientras otro lo consume a su ritmo. Eso reduce el acoplamiento temporal, aunque no elimina el acoplamiento de datos: el contrato del evento sigue siendo un contrato.

### 3.3 La escala

Las cifras de 2025 son las siguientes:

- **Más de 110 millones de clientes** en Brasil, Colombia y México.
- **Más de 4 000 microservicios** orquestados con Kubernetes.
- **72 000 millones de eventos diarios** procesados por Kafka.
- **Millones de peticiones por segundo**.
- **Decenas de miles de pods**.

No son cifras de una maqueta. Son la operación de un banco que mueve dinero y que tiene que cumplir regulaciones en tres países. Eso explica por qué la arquitectura no es un experimento: es la infraestructura sobre la que corre el negocio.

### 3.4 Beneficios que reporta la empresa

El blog de 2025 no presenta una tabla de resultados financieros, pero sí describe mejoras concretas:

- **Reducción del radio de fallo.** Con la estrategia de múltiples cuentas de AWS, que llaman «continental drift», cada dominio y cada equipo puede tener su propia cuenta. Un problema en una cuenta no arrastra a todas las demás.
- **Aislamiento de ambientes.** Staging y producción dejaron de compartir el mismo espacio, y los despliegues se volvieron más rápidos.
- **Evolución independiente.** Cada dominio puede cambiar a su ritmo, sin coordinar con todos los demás.
- **Mejor capacidad de pronóstico.** Al dividir en shards, el uso de un shard sirve para proyectar el comportamiento de los otros.
- **Plataformización.** La plataforma de tarjetas pasó de un servicio único (`CreditCardAccounts`) a servicios más flexibles, capaces de atender la tarjeta combo en Brasil y las tarjetas separadas de débito y crédito en México. El motor de autorización (`Authorizer`) se migró en parte del datacenter físico a la nube, con baja latencia y alta disponibilidad para hablar con Mastercard.
- **Estandarización.** Todos los equipos usan Clojure, microservicios y comunicación asíncrona, lo que reduce el costo cognitivo y facilita la reutilización.

Los beneficios son, sobre todo, de operación y de organización: aislamiento, autonomía, velocidad de despliegue. No son un aumento automático del rendimiento. De hecho, la propia empresa dice que empezar con microservicios la hizo más lenta al principio. Esa honestidad es útil para el análisis.

### 3.5 Desafíos y cómo los resolvió

Los problemas que aparecen en las fuentes son estos:

1. **Redibujar los límites de los servicios.** A medida que entendían mejor el dominio, tenían que cambiar abstracciones centrales. «At any given time, one squad or another will be working on splitting up a service or merging two services», dice el blog de 2019. No es un problema que se resuelva una vez; es parte del trabajo.
2. **Complejidad de aprovisionamiento.** Montar la infraestructura para microservicios con un equipo pequeño fue costoso. Pasaron por Chef, CoreOS Fleet, ECS, una infraestructura propia con EC2 y CloudFormation, y finalmente Kubernetes. La automatización fue la respuesta: todos los recursos se aprovisionan por proceso automático, incluso los casos aislados.
3. **Fallos parciales y pérdida de mensajes.** La respuesta fue el broker replicado y las *dead letter topics*. Si un consumidor falla, el mensaje no se pierde: se desvía y se puede reprocesar.
4. **Consistencia de datos.** Al ser asíncrono, el sistema no tiene una única verdad instantánea. La revisión de Bação y Guerreiro (2026) muestra que la consistencia en microservicios orientados a eventos se divide en tres problemas: consistencia de datos, de proceso y de evolución del sistema. Nubank lo aborda con contratos estables para los datos analíticos y con una capa de `contracts` que evita romper los modelos de los consumidores.
5. **Límites de la nube.** Aunque la nube parezca infinita, cada proveedor impone cuotas. Nubank construyó un **AWS Limit Smoke Detector**: un pipeline que recoge uso, cuotas y costos de todas las cuentas, los compara con los límites y avisa antes de que se conviertan en un incidente.
6. **Radio de fallo.** La respuesta fue «continental drift»: separar por cuentas y por dominios, no tener todo en una sola cuenta gigante.
7. **Sobreingeniería.** La entrada de 2025 lo dice de forma explícita: al extraer partes de sistemas legados y reescribir componentes, hay que evitar la sobreingeniería y buscar el equilibrio entre generalidad y simplicidad. Es una advertencia que vale para cualquier equipo.

### 3.6 Límites de las fuentes

Las tres fuentes son de la propia empresa. No hay una auditoría externa que confirme las cifras, ni tasas de error, ni percentiles de latencia, ni costo absoluto. La entrada de 2019 es una descripción de arquitectura; las de 2025 son recapitulaciones de meetups y de gestión de límites. Sirven para entender decisiones y mecanismos, no para concluir que EDA «es mejor» en abstracto.

También hay que decir que Nubank no es un caso de migración de monolito a microservicios. Empezó con microservicios y luego tuvo que ordenar la plataforma. Eso lo hace distinto de Uber o de Netflix, y obliga a leerlo como una historia de crecimiento, no de rescate.

---

## 4. Lo que dice la literatura

Se descargaron diez artículos de acceso abierto, publicados entre 2021 y 2026. Todos tienen DOI verificado y PDF completo. La ficha está en `fuentes/FUENTES.md` y el registro de comprobaciones en `fuentes/VERIFICACION.md`.

### 4.1 Söylemez, Tekinerdogan y Kolukısa Tarhan (2022)

**«Challenges and Solution Directions of Microservice Architectures: A Systematic Literature Review»**, *Applied Sciences*, 12(11):5507.

Revisión sistemática de 3 842 trabajos, de los cuales seleccionaron 85. Identifican **nueve categorías de desafíos**: descubrimiento de servicios, gestión y consistencia de datos, pruebas, predicción y medición del rendimiento, comunicación e integración, orquestación, seguridad, monitoreo/trazabilidad/logs y descomposición. Cada categoría se abre en 40 subcategorías con posibles soluciones.

Uso: es el mapa para clasificar los problemas de Nubank. La consistencia de datos, la comunicación, el monitoreo y la descomposición aparecen en el caso. También advierte que los desafíos pueden anular los beneficios si no se atienden.

Límite: revisión de literatura, no medición en una empresa.

### 4.2 Bogner, Fritzsch, Wagner y Zimmermann (2021)

**«Industry practices and challenges for the evolvability assurance of microservices»**, *Empirical Software Engineering*, 26(5):104.

Entrevistaron a 17 profesionales de 10 empresas sobre 14 sistemas, y revisaron 295 recursos de práctica. La conclusión principal es que la calidad de cada servicio no era el problema; **el problema estaba en la arquitectura del conjunto**. Los equipos con poca experiencia sufrían con el corte de servicios y la integración, y faltaban herramientas y métricas para evaluar la arquitectura a nivel macro.

Uso: explica por qué Nubank necesitó estandarizar (Clojure, comunicación asíncrona, contratos) y por qué la plataforma no se resuelve solo con servicios pequeños.

### 4.3 Abgaz y coautores (2023)

**«Decomposition of Monolith Applications Into Microservices Architectures: A Systematic Review»**, *IEEE Transactions on Software Engineering*, 49(8):4213–4242.

Revisan 35 trabajos sobre cómo partir un monolito. La conclusión es incómoda: la descomposición **sigue en una etapa temprana**. No hay métodos que combinen datos estáticos, dinámicos y de evolución; faltan herramientas; y no existen métricas, conjuntos de datos ni líneas base estandarizadas.

Uso: aunque Nubank no venga de un monolito, el problema de los límites es el mismo. Redibujar servicios es una tarea sin método estándar, y la literatura lo confirma.

### 4.4 Blinowski, Ojdowska y Przybyłek (2022)

**«Monolithic vs. Microservice Architecture: A Performance and Scalability Evaluation»**, *IEEE Access*, 10:20357–20374.

Comparan cuatro versiones de una aplicación (monolito y microservicios, en Java y .NET) en tres entornos. Los resultados que importan:

- En una sola máquina, **el monolito rinde mejor** que su equivalente en microservicios.
- El escalado vertical resulta **más rentable** que el horizontal en Azure.
- Escalar horizontalmente más allá de cierto número de instancias **degrada** el rendimiento.
- La tecnología de implementación (Java o .NET) no tiene un impacto notable en la escalabilidad.

La conclusión de los autores es directa: «A monolithic architecture seems to be a better choice for simple, small-sized systems that do not have to support a large number of concurrent users». Es la cita más útil para la sección del PFC.

### 4.5 Vural y Koyuncu (2021)

**«Does Domain-Driven Design Lead to Finding the Optimal Modularity of a Microservice?»**, *IEEE Access*, 9:32721–32733.

Toman dos ejemplos diseñados con DDD y los modifican para tener más y menos granularidad. Miden acoplamiento y cohesión. Concluyen que DDD **sirvió para encontrar una granularidad razonable** en esos dos casos, aunque reconocen que la muestra es pequeña.

Uso: apoya la idea de que los límites de dominio son una buena guía, pero no una fórmula. Nubank redibuja límites todo el tiempo; la literatura dice que eso es normal y difícil.

### 4.6 Oumoussa y Saidi (2024)

**«Evolution of Microservices Identification in Monolith Decomposition: A Systematic Review»**, *IEEE Access*, 12:23389–23405.

Revisan la literatura sobre cómo identificar microservicios candidatos. Encuentran métricas inconsistentes, falta de conjuntos de datos y benchmarks, y factores humanos. Su conclusión es que la comparación entre métodos es difícil por la falta de estándares.

Uso: refuerza el punto de Abgaz. No basta con «querer microservicios»; hay que identificar qué se separa y con qué criterio.

### 4.7 Henning y Hasselbring (2022)

**«A configurable method for benchmarking scalability of cloud-native applications»**, *Empirical Software Engineering*, 27(6):143.

Proponen un método para medir escalabilidad con experimentos aislados, objetivos de nivel de servicio (SLO) y pocas repeticiones. Lo prueban con Kafka Streams y Flink en dos nubes públicas y una privada. Con cinco repeticiones o menos y ejecuciones de cinco minutos o menos, alcanza para evaluar si se cumplen los SLO.

Uso: si el PFC algún día necesita justificar una separación de servicios, tiene que medir. Este artículo da una idea de cómo hacerlo sin montar un laboratorio enorme.

### 4.8 Özkan, Babur y van den Brand (2025)

**«Domain-Driven Design in software development: A systematic literature review on implementation, challenges, and effectiveness»**, *Journal of Systems and Software*, 230:112537.

Revisión sistemática de DDD. Encuentran que el interés creció junto con los microservicios desde 2017, que el lenguaje ubicuo y los contextos acotados son centrales, y que la implementación **depende mucho de la experiencia de las personas**. También señalan que falta investigación empírica.

Uso: modera el entusiasmo por DDD. Nubank pudo pagar expertos y tiempo; un equipo de estudiantes no tiene ese lujo. Se puede tomar la idea de límites, no el aparato completo.

### 4.9 Al-Qora'n y Al-Said Ahmad (2025)

**«Modular Monolith Architecture in Cloud Environments: A Systematic Literature Review»**, *Future Internet*, 17(11):496.

Primera revisión sistemática del monolito modular. Revisan 15 estudios entre 2020 y mayo de 2025. Los motivos de adopción son despliegue simplificado, mantenibilidad y menos sobrecarga de orquestación. La conclusión es que el monolito modular es una opción intermedia: tiene la simplicidad operativa del monolito y parte de la modularidad de los microservicios, pero su escalabilidad y resiliencia son más limitadas.

Uso: es el argumento central para la recomendación del PFC. No es «no usar microservicios»; es «empezar por un monolito modular y separar solo cuando haya una razón medida».

### 4.10 Bação y Guerreiro (2026)

**«Consistency challenges in event-driven microservices: a literature review on data, process, and system evolution»**, *Computing*, 108(7):110.

Revisión sistemática sobre la consistencia en microservicios orientados a eventos. Distinguen tres tipos: consistencia de datos, de proceso y de evolución del sistema. Señalan que los enfoques se tratan por separado y que el panorama está fragmentado.

Uso: es la referencia específica de EDA. Permite hablar con precisión del costo central de la mensajería asíncrona: no hay una verdad instantánea, y hay que decidir qué se hace mientras los eventos se propagan.

### 4.11 Síntesis

Juntando las fuentes, quedan cinco ideas:

1. **Los beneficios de Nubank son de operación y organización.** Aislamiento, autonomía, velocidad de despliegue. No son un aumento automático del rendimiento; Blinowski muestra que el rendimiento puede ser peor.
2. **La complejidad no desaparece: se muda.** Pasa del código a la arquitectura, la operación y la organización. Söylemez y Bogner la describen; Nubank la vivió.
3. **Lo difícil es el límite, no la tecnología.** Abgaz y Oumoussa muestran que la descomposición sigue sin estándares. Vural y Özkan muestran que DDD ayuda, pero depende de expertos.
4. **La consistencia es el precio de EDA.** Bação y Guerreiro la ordenan en tres tipos; Nubank la maneja con contratos, brokers y *dead letters*.
5. **Para un sistema pequeño, el monolito modular es una opción racional.** Al-Qora'n lo respalda; Blinowski lo demuestra con mediciones.

---

## 5. Aplicación al PFC

El PFC del equipo es el **Sistema de Pre-Sustentaciones**. Es una aplicación web para gestionar solicitudes de pre-sustentación, documentos, revisiones, aprobaciones y notas. La escala es pequeña: un equipo de estudiantes, un servidor, una base PostgreSQL y pocos usuarios concurrentes. Compararlo con Nubank no es comparar tamaños; es comparar decisiones.

### 5.1 Qué tomaría del caso

- **Límites de dominio antes que servicios.** Aunque todo viva en un solo proceso, separar solicitudes, documentos, revisión, notas y notificaciones en módulos con interfaces claras evita el monolito desordenado. Es la idea de la plataformización de Nubank sin el costo de la red.
- **Contratos estables.** Nubank usa una capa de `contracts` para que los consumidores no dependan de los modelos internos. En el PFC, eso se traduce en esquemas de API versionados y eventos con campos bien definidos.
- **Asincronía solo donde aporta.** La mensajería tiene sentido para notificaciones, auditoría o tareas que pueden esperar. No para el flujo principal de aprobación, donde el usuario necesita una respuesta inmediata.
- **Límites y alertas.** Aunque el PFC no tenga 4 000 servicios, puede tener límites de almacenamiento, de tamaño de archivos o de conexiones. Medirlos y avisar antes de que fallen es una buena práctica que Nubank lleva al extremo.
- **Medir antes de separar.** Si un módulo se vuelve lento o necesita escalar aparte, primero se mide. Henning y Hasselbring muestran que no hace falta un experimento enorme para saber si se cumplen los SLO.

### 5.2 Qué no tomaría

- **4 000 microservicios y 72 000 millones de eventos diarios.** Es la escala de Nubank, no la del PFC. Aplicarla sería copiar la solución sin el problema.
- **Un clúster de Kafka y múltiples cuentas de AWS.** Sin equipo de plataforma, multiplica el trabajo de operación.
- **Una base de datos por servicio.** Obliga a transacciones distribuidas, sagas y reconciliación. Para solicitudes, documentos y notas, una sola base con buenas tablas y transacciones es más simple y más segura.
- **EDA para todo.** La consistencia eventual complica la aprobación de una solicitud o el registro de una nota. Bação y Guerreiro muestran que la consistencia de proceso es un problema aparte, no un detalle.
- **DDD completo sin expertos.** Özkan muestra que DDD depende de la experiencia. Se pueden tomar los contextos acotados y el lenguaje ubicuo, sin aggregates, domain events y toda la parafernalia.
- **Clojure y Datomic.** Son decisiones coherentes con la cultura de Nubank, pero no con el stack del PFC ni con el tiempo del equipo.

### 5.3 Por qué

La razón es la misma que da Blinowski: en una sola máquina, el monolito rinde mejor; escalar horizontalmente de más degrada. La razón de Al-Qora'n es complementaria: el monolito modular encaja cuando los microservicios introducen más complejidad y costo que beneficio. Y la razón de Bação y Guerreiro es la advertencia: la consistencia en EDA es un problema de diseño, no un detalle de implementación. Un equipo pequeño no debería gastar su tiempo en resolver un problema que todavía no tiene.

### 5.4 Evolución propuesta

1. **Monolito modular** con módulos claros, pruebas y una sola base.
2. **Medición**: latencia p95, tasa de error, tiempo de despliegue, contención de la base.
3. **Extracción selectiva** solo si un módulo tiene una necesidad distinta de escalado o de aislamiento.
4. **Si se extrae**: contrato explícito, base propia y un patrón de consistencia (outbox o saga) para no romper el flujo de aprobación.
5. **Si se usa mensajería**: solo para notificaciones, auditoría o tareas diferibles; con reintentos, *dead letter* y trazabilidad.

Eso es lo que Nubank hizo a su escala: no partió por partir, sino que puso límites, contratos y automatización cuando el costo de no tenerlos se volvió insoportable. La lección para el PFC es la misma, con otro tamaño.

---

## 6. Plan del documento de una página

**Título provisional:** «Nubank: 72 000 millones de eventos diarios en una arquitectura orientada a eventos».

| Apartado | Extensión aproximada | Qué debe decir | Citas |
|---|---|---|---|
| 1. Descripción del caso | 100 palabras | Nubank, banco digital, 110 millones de clientes, 4 000 microservicios, 72 000 millones de eventos diarios, EDA con Kafka | Nubank 2025 |
| 2. Por qué y qué había antes | 110 palabras | 2013: un producto, un país, arquitectura simple; empezaron con microservicios y les costó; comunicación asíncrona por broker para no perder datos | Nubank 2019, Nubank 2025 |
| 3. Beneficios | 110 palabras | 110 millones de clientes; 4 000 servicios; 72 000 millones de eventos; multi-cuenta reduce radio de fallo; plataformización | Nubank 2025, Söylemez |
| 4. Desafíos y soluciones | 140 palabras | Límites redibujados, aprovisionamiento, fallos parciales, consistencia, cuotas; broker + dead letters, contratos, multi-cuenta, sharding, smoke detector | Nubank 2019/2025, Bação, Bogner |
| 5. Aplicación al PFC | 130 palabras | Tomar límites, contratos, asincronía solo para notificaciones, medir; no tomar 4 000 servicios, Kafka, base por servicio; monolito modular | Blinowski, Al-Qora'n, Bação, Abgaz |

Total: unas 590 palabras. Con 11 pt y márgenes de 2,3 cm entra en una página. Si no entra, se recorta la sección 4 o se bajan las citas a las mínimas.

**Referencias previstas:** las diez fuentes académicas y las tres entradas de Nubank. En el texto se citan las que se usen; si alguna queda sin uso, se retira del `.bib` para no inflar la lista.

---

## 7. Verificación de fuentes

El método fue este:

1. **Crossref.** Para cada DOI se consultó `api.crossref.org/works/{doi}` y se compararon autores, año, título, revista, volumen, número y páginas.
2. **OpenAlex.** Se usó para confirmar el estado de acceso abierto y localizar copias en repositorios.
3. **Resolución del DOI.** Se abrió `https://doi.org/{doi}` y se comprobó que redirige al editor correcto.
4. **Descarga.** Se verificó que el archivo empiece con `%PDF`, se contaron las páginas y se extrajo la primera página para comparar título y autores.
5. **Hash.** Se guardó el SHA-256 de cada PDF.

Resultado: **10 de 10 PDFs** descargados, con título y autores coincidentes. Las tres páginas de Nubank se abrieron y se guardó su texto; la de 2019 se recuperó también desde Wayback Machine porque el sitio bloquea clientes automatizados. El detalle está en `fuentes/VERIFICACION.md`.

---

## 8. Decisiones pendientes

1. **Confirmar Nubank como caso** y registrarlo en el foro del SGA. Si también está ocupado, pasar a otra empresa; el corpus académico se reutiliza.
2. **Elegir el sistema de primera mano** para la evidencia de autoría. Opciones: el SGA de la UTEQ, la app de un banco, el proyecto de control de acceso del equipo o el propio PFC. La más honesta es el PFC, porque se conoce por dentro; la más cercana a «sistema en producción» es el SGA.
3. **Definir la URL del repositorio** y la cuenta de GitHub. El usuario de los trabajos anteriores es `Amendozab5`; el repo nuevo podría llamarse `form-ic-01-arquitecturas-distribuidas`.
4. **Plan de commits.** La consigna pide al menos tres commits en tres días distintos. Hoy se puede hacer el primero (investigación y corpus). Los otros dos tendrían que hacerse el 26 y el 27, o fecharse, lo que falsearía el historial. Hay que decidir con el usuario.
5. **Declaración de IA.** Se redactará al final, con la herramienta usada, para qué se usó y cómo se verificó.

---

## 9. Archivos de esta etapa

- `fuentes/pdf/` — los diez artículos, en PDF.
- `fuentes/FUENTES.md` — ficha bibliográfica de cada uno.
- `fuentes/VERIFICACION.md` — registro de comprobaciones.
- `referencias.bib` — entradas para BibLaTeX.
- `docs/01-analisis-tema.md` — este documento.
