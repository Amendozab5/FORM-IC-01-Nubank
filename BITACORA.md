# Bitácora — FORM-IC-01

Registro de lo que se hizo, en orden. Las fechas son las del trabajo real.

## 2026-11-25 — Investigación del tema y corpus de fuentes

**Objetivo de la sesión:** entender los cuatro estilos de la Unidad 2, elegir un caso real y reunir artículos científicos de acceso abierto.

**Cambio de caso:** el primer intento fue Uber y DOMA, pero Uber ya estaba registrado en el foro del SGA, junto con DoorDash, Airbnb y Wix. Se descartó Uber y se pasó a **Nubank**, que no aparece en la lista de ocupados.

**Decisiones:**

- Caso: Nubank, banco digital brasileño, con una arquitectura orientada a eventos para la comunicación entre microservicios.
- Estilo: EDA. Los servicios publican mensajes en un broker replicado y los consumen después; la mayor parte de las interacciones entre servicios es asíncrona. Los microservicios son la unidad de despliegue, pero el estilo que se analiza es la comunicación por eventos.
- Fuentes oficiales: tres entradas de *Building Nubank* (2019, 2025 y 2025). La de 2019 explica el porqué y el costo inicial; las de 2025 dan la escala y las soluciones de infraestructura.
- Se descartaron Netflix (muy probable duplicado), Amazon (pocas cifras verificables), Mercado Libre (la fuente principal es un caso de AWS, no el blog de la empresa) y Zalando (fuentes más antiguas).

**Trabajo hecho:**

- Se leyeron y resumieron los cuatro estilos: SOA, microservicios, EDA y API Gateway.
- Se extrajo el texto completo de las tres entradas de Nubank. La de 2019 se recuperó desde Wayback Machine porque el sitio bloquea clientes automatizados.
- Se guardaron las citas que se van a usar: «we started with microservices from day one», «made us slower in the beginning», «most service-to-service interactions are intermediated by a reliable replicated messaging broker», «dead letter topic».
- Se buscaron artículos en OpenAlex con filtros de acceso abierto y fecha (2021–2026).
- Se seleccionaron diez artículos por pertinencia, calidad de la revista y disponibilidad del PDF.
- Se descargaron los diez PDF y se comprobó la cabecera, el número de páginas y la primera página.
- Se verificaron los diez DOI en Crossref y se comprobó a qué editor redirigen.
- Se calculó el SHA-256 de cada PDF.
- Se escribieron `docs/01-analisis-tema.md`, `fuentes/FUENTES.md`, `fuentes/VERIFICACION.md`, `referencias.bib`, `README.md` y `.gitignore`.
- Se creó la estructura de carpetas: `docs/`, `fuentes/pdf/`, `capturas/`, `informe/`, `sga/`.

**Incidencias:**

- MDPI bloquea el HTML con 403. Se resolvió con la ruta directa de `mdpi-res.com`.
- IEEE Xplore no expone el título en el HTML. Se comprobó por número de documento.
- El PDF de IEEE TSE de Abgaz daba 404. Se usó la copia aceptada del repositorio de DCU.
- El PDF de Özkan en arXiv no era la versión publicada. Se reemplazó por la versión del editor del repositorio de la TU/e.
- Nubank devuelve 403 a `urllib`. Las páginas de 2025 se leyeron con `r.jina.ai`; la de 2019, con Wayback Machine.

**Pendiente:**

- Confirmar Nubank en el foro del SGA.
- Elegir el sistema de primera mano.
- Redactar el análisis de una página.
- Preparar la captura anotada.
- Compilar el PDF.
- Publicar el repositorio.

## 2026-11-26 — Pendiente

- Redacción del análisis de una página en LaTeX.
- Carátula y referencias.
- Captura anotada de la fuente principal.

## 2026-11-27 — Pendiente

- Compilación, revisión de una página, PDF del SGA.
- README definitivo y publicación.
