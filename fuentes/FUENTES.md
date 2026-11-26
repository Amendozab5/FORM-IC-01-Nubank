# Fuentes del FORM-IC-01

Corpus de artículos científicos y fuentes oficiales del caso Nubank. Todos los PDF están en `fuentes/pdf/` y fueron comprobados contra Crossref, OpenAlex y la primera página del propio archivo. La fecha de verificación es el 25 de noviembre de 2026.

## Tabla general

| # | Archivo | Autores | Año | Título | Revista | Vol. (núm.) | Páginas / artículo | DOI | Acceso |
|---|---|---|---|---|---|---|---|---|---|
| 1 | `01_Soylemez2022_Challenges.pdf` | Söylemez, Tekinerdogan, Kolukısa Tarhan | 2022 | Challenges and Solution Directions of Microservice Architectures: A Systematic Literature Review | Applied Sciences | 12 (11) | 5507 | 10.3390/app12115507 | Gold, CC BY 4.0 |
| 2 | `02_Bogner2021_Evolvability.pdf` | Bogner, Fritzsch, Wagner, Zimmermann | 2021 | Industry practices and challenges for the evolvability assurance of microservices: An interview study and systematic grey literature review | Empirical Software Engineering | 26 (5) | 104 | 10.1007/s10664-021-09999-9 | Hybrid, CC BY 4.0 |
| 3 | `03_Abgaz2023_Decomposition.pdf` | Abgaz, McCarren, Elger, Solan, Lapuz, Bivol, Jackson, Yilmaz, Buckley, Clarke | 2023 | Decomposition of Monolith Applications Into Microservices Architectures: A Systematic Review | IEEE Transactions on Software Engineering | 49 (8) | 4213–4242 | 10.1109/TSE.2023.3287297 | Hybrid, CC BY 4.0 (copia DCU) |
| 4 | `04_Blinowski2022_MonolithVsMicro.pdf` | Blinowski, Ojdowska, Przybyłek | 2022 | Monolithic vs. Microservice Architecture: A Performance and Scalability Evaluation | IEEE Access | 10 | 20357–20374 | 10.1109/ACCESS.2022.3152803 | Gold, CC BY 4.0 |
| 5 | `05_Vural2021_DDD.pdf` | Vural, Koyuncu | 2021 | Does Domain-Driven Design Lead to Finding the Optimal Modularity of a Microservice? | IEEE Access | 9 | 32721–32733 | 10.1109/ACCESS.2021.3060895 | Gold, CC BY 4.0 |
| 6 | `06_Oumoussa2024_Identification.pdf` | Oumoussa, Saidi | 2024 | Evolution of Microservices Identification in Monolith Decomposition: A Systematic Review | IEEE Access | 12 | 23389–23405 | 10.1109/ACCESS.2024.3365079 | Gold, CC BY-NC-ND 4.0 |
| 7 | `07_Henning2022_Scalability.pdf` | Henning, Hasselbring | 2022 | A configurable method for benchmarking scalability of cloud-native applications | Empirical Software Engineering | 27 (6) | 143 | 10.1007/s10664-022-10162-1 | Hybrid, CC BY 4.0 |
| 8 | `08_Ozkan2025_DDD.pdf` | Özkan, Babur, van den Brand | 2025 | Domain-Driven Design in software development: A systematic literature review on implementation, challenges, and effectiveness | Journal of Systems and Software | 230 | 112537 | 10.1016/j.jss.2025.112537 | Hybrid, CC BY-NC-ND 4.0 (copia TU/e) |
| 9 | `09_AlQoran2025_ModularMonolith.pdf` | Al-Qora'n, Al-Said Ahmad | 2025 | Modular Monolith Architecture in Cloud Environments: A Systematic Literature Review | Future Internet | 17 (11) | 496 | 10.3390/fi17110496 | Gold, CC BY 4.0 |
| 10 | `10_Bacao2026_EDA_Consistency.pdf` | Bação, Guerreiro | 2026 | Consistency challenges in event-driven microservices: a literature review on data, process, and system evolution | Computing | 108 (7) | 110 | 10.1007/s00607-026-01701-5 | Hybrid, CC BY 4.0 |

## Fuentes oficiales del caso

**Nubank.** *Building Nubank* (blog técnico oficial). Tres entradas, todas con fecha y autor identificables:

1. **Ferreira, R.** «Microservices at Nubank, an overview». 12 de agosto de 2019. https://building.nu.com/microservices-at-nubank-an-overview/
   - Explica que empezaron con microservicios desde el primer día, el costo de esa decisión y la mensajería asíncrona como mecanismo principal de comunicación.
   - Es la fuente principal para el «porqué» de la EDA.
2. **Peixoto, E.; Dantas, D.** «Managing Cloud Limits». 9 de abril de 2025. https://building.nu.com/managing-cloud-limits/
   - Da las cifras de escala: 110 millones de clientes, 4 000 microservicios, 72 000 millones de eventos diarios por Kafka, millones de peticiones por segundo.
   - Describe «Pangeia», la estrategia multi-cuenta y el AWS Limit Smoke Detector.
3. **Nubank.** «Evolución de la Arquitectura de Nubank Hacia una Plataforma Reflexiva». 25 de junio de 2025. https://building.nu.com/es/engineering-meetup-arquitectura-plataformas-crecimiento-profesional/
   - Recapitula la charla de Bel Gonçalves sobre el paso de un producto y un país a una plataforma con servicios reutilizables.
   - Explica la plataforma de tarjetas, el `Authorizer` y la estandarización con Clojure y comunicación asíncrona.

Las tres son fuentes de la propia empresa. Se guardó el texto completo en `_tmp/` para trabajar; la captura anotada irá en `capturas/`.

## Enlaces de descarga

Los PDF ya están en `fuentes/pdf/`. Estos son los enlaces directos:

1. Söylemez et al. (2022): https://mdpi-res.com/d_attachment/applsci/applsci-12-05507/article_deploy/applsci-12-05507.pdf
2. Bogner et al. (2021): https://link.springer.com/content/pdf/10.1007/s10664-021-09999-9.pdf
3. Abgaz et al. (2023): https://doras.dcu.ie/28925/2/Decomposition_of_Monolith_Applications_Into_Microservices_Architectures_A_Systematic_Review.pdf
4. Blinowski et al. (2022): https://ieeexplore.ieee.org/ielx7/6287639/9668973/09717259.pdf
5. Vural y Koyuncu (2021): https://ieeexplore.ieee.org/ielx7/6287639/9312710/09359794.pdf
6. Oumoussa y Saidi (2024): https://ieeexplore.ieee.org/ielx7/6287639/6514899/10431792.pdf
7. Henning y Hasselbring (2022): https://link.springer.com/content/pdf/10.1007/s10664-022-10162-1.pdf
8. Özkan et al. (2025): https://research.tue.nl/p/file/document/362673836/1-s2.0-S0164121225002055-main.pdf
9. Al-Qora'n y Al-Said Ahmad (2025): https://mdpi-res.com/d_attachment/futureinternet/futureinternet-17-00496/article_deploy/futureinternet-17-00496.pdf
10. Bação y Guerreiro (2026): https://link.springer.com/content/pdf/10.1007/s00607-026-01701-5.pdf

Fuentes oficiales del caso:

- https://building.nu.com/microservices-at-nubank-an-overview/
- https://building.nu.com/managing-cloud-limits/
- https://building.nu.com/es/engineering-meetup-arquitectura-plataformas-crecimiento-profesional/

## Ficha de cada artículo

### 1. Söylemez et al. (2022)

- **Tipo:** revisión sistemática de literatura (SLR).
- **Método:** 3 842 trabajos encontrados, 85 primarios, 9 categorías de desafíos y 40 subcategorías.
- **Aporte:** mapa de problemas de la arquitectura de microservicios: descubrimiento de servicios, datos y consistencia, pruebas, rendimiento, comunicación e integración, orquestación, seguridad, monitoreo/trazabilidad/logs y descomposición.
- **Uso previsto:** clasificar los problemas de Nubank y sostener que los desafíos pueden anular los beneficios.
- **Límite:** es literatura, no medición de campo.
- **PDF:** descargado de `mdpi-res.com` (MDPI bloquea el HTML con 403; el PDF directo funciona).

### 2. Bogner et al. (2021)

- **Tipo:** estudio empírico cualitativo + revisión de literatura gris.
- **Método:** 17 entrevistas, 10 empresas, 14 sistemas; 295 recursos de práctica.
- **Aporte:** la calidad de cada servicio no era el problema; el problema era la arquitectura del conjunto. Importancia de equilibrar descentralización y estandarización. Falta de herramientas y métricas para evaluar la arquitectura a nivel macro.
- **Uso previsto:** explicar por qué Nubank necesitó estandarizar (Clojure, contratos, comunicación asíncrona); advertir sobre el costo de evolución.
- **Límite:** muestra cualitativa, no representativa de toda la industria.
- **PDF:** Springer, acceso abierto.

### 3. Abgaz et al. (2023)

- **Tipo:** SLR.
- **Método:** 35 trabajos sobre descomposición de monolitos.
- **Aporte:** la descomposición sigue en etapa temprana; no hay métodos que combinen datos estáticos, dinámicos y evolutivos; faltan herramientas, métricas, datasets y líneas base.
- **Uso previsto:** advertir que redibujar límites no es un trámite; sostener la cautela del PFC.
- **Límite:** revisión, no experiencia propia.
- **PDF:** copia aceptada del repositorio de Dublin City University (DCU), con la misma paginación de IEEE TSE.

### 4. Blinowski et al. (2022)

- **Tipo:** evaluación experimental de rendimiento y escalabilidad.
- **Método:** cuatro versiones (monolito/microservicios, Java/.NET) en tres entornos (local, Azure Spring Cloud, Azure App Service).
- **Aporte:** en una sola máquina el monolito rinde mejor; el escalado vertical es más rentable que el horizontal; escalar de más degrada el rendimiento; la tecnología no incide de forma notable en la escalabilidad.
- **Uso previsto:** argumento central para no adoptar microservicios en el PFC sin necesidad.
- **Límite:** aplicación de referencia, no un sistema real de producción.
- **PDF:** IEEE Access, acceso abierto.

### 5. Vural y Koyuncu (2021)

- **Tipo:** estudio empírico con dos casos y métricas de acoplamiento/cohesión.
- **Aporte:** DDD ayudó a encontrar una granularidad razonable en los dos casos; el problema de granularidad es en realidad un problema de modularidad.
- **Uso previsto:** respaldar la idea de límites de dominio, con cautela por la muestra pequeña.
- **Límite:** dos ejemplos, no generalizable.
- **PDF:** IEEE Access, acceso abierto.

### 6. Oumoussa y Saidi (2024)

- **Tipo:** SLR.
- **Aporte:** la identificación de microservicios candidatos sigue siendo un problema abierto; métricas inconsistentes, falta de benchmarks y datasets, factores humanos.
- **Uso previsto:** reforzar la dificultad de la descomposición desde el ángulo de la identificación.
- **Límite:** revisión, no validación empírica propia.
- **PDF:** IEEE Access, acceso abierto (CC BY-NC-ND).

### 7. Henning y Hasselbring (2022)

- **Tipo:** método de benchmarking + evaluación experimental.
- **Aporte:** método configurable para medir escalabilidad de aplicaciones cloud-native con SLO; pocas repeticiones y ejecuciones cortas alcanzan para evaluar.
- **Uso previsto:** si el PFC justifica separar un servicio, medir con un método reproducible.
- **Límite:** se centra en escalabilidad, no en costo total ni en organización.
- **PDF:** Springer, acceso abierto.

### 8. Özkan et al. (2025)

- **Tipo:** SLR.
- **Aporte:** DDD creció junto con microservicios desde 2017; el lenguaje ubicuo y los contextos acotados son centrales; la implementación depende de la experiencia de las personas; falta investigación empírica.
- **Uso previsto:** moderar la adopción de DDD en el PFC; tomar los límites, no el aparato completo.
- **Límite:** revisión; la muestra de estudios todavía es heterogénea.
- **PDF:** versión del editor (version of record) descargada del repositorio de la TU/e; 22 páginas, CC BY-NC-ND 4.0.

### 9. Al-Qora'n y Al-Said Ahmad (2025)

- **Tipo:** SLR.
- **Método:** 15 estudios, 6 bibliotecas digitales, 2020 a mayo de 2025.
- **Aporte:** el monolito modular es una opción intermedia; sus motivos son despliegue simplificado, mantenibilidad y menos orquestación; su escalabilidad y resiliencia son limitadas frente a microservicios.
- **Uso previsto:** recomendación central para el PFC.
- **Límite:** literatura aún escasa y sin benchmarks homogéneos.
- **PDF:** MDPI, acceso abierto.

### 10. Bação y Guerreiro (2026)

- **Tipo:** revisión sistemática de literatura.
- **Aporte:** ordena la consistencia en microservicios orientados a eventos en tres tipos: datos, proceso y evolución del sistema. Señala que los enfoques se tratan por separado y que el panorama está fragmentado.
- **Uso previsto:** referencia específica de EDA; explicar el costo de la consistencia eventual y por qué el PFC no debe usar eventos para el flujo principal.
- **Límite:** revisión; no mide un sistema real.
- **PDF:** Springer, acceso abierto (CC BY 4.0).

## Qué falta

- La captura anotada de la fuente principal (blog de Nubank).
- La evidencia de un sistema de primera mano.
- La declaración de uso de IA.
- El documento LaTeX de una página.
- El PDF del SGA y el README definitivo.
