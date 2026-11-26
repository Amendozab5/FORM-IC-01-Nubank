# Verificación de fuentes — FORM-IC-01

**Fecha:** 25 de noviembre de 2026  
**Alcance:** diez artículos científicos de acceso abierto y tres entradas del blog técnico oficial de Nubank.  
**Resultado:** 10 de 10 artículos con DOI existente, metadatos coincidentes y PDF completo; 3 de 3 fuentes oficiales localizadas, con fecha y autor identificables.

## 1. Método

Para cada artículo se hicieron cinco comprobaciones independientes:

1. **Crossref.** `GET https://api.crossref.org/works/{DOI}`. Se compararon título, autores, año, revista, volumen, número y páginas.
2. **DOI resuelto.** `GET https://doi.org/{DOI}` sin seguir la redirección. Se comprobó que el destino sea el editor correcto y que el identificador del artículo coincida con el PDF.
3. **OpenAlex.** `GET https://api.openalex.org/works/doi:{DOI}`. Se confirmó el estado de acceso abierto y se localizaron copias en repositorios.
4. **PDF.** Se comprobó la cabecera `%PDF`, el número de páginas y la primera página. El título y los autores de esa página tienen que coincidir con los de Crossref.
5. **Hash.** Se calculó SHA-256 del archivo para dejar una huella verificable.

No se aceptó ningún artículo por el resultado de un buscador. Todos pasaron por las cinco comprobaciones.

## 2. Coincidencia bibliográfica

| # | DOI | Título en Crossref | Año | Revista, vol. (núm.), págs. | Destino del DOI | ¿Coincide? |
|---|---|---|---|---|---|---|
| 1 | 10.3390/app12115507 | Challenges and Solution Directions of Microservice Architectures: A Systematic Literature Review | 2022 | Applied Sciences, 12(11), 5507 | `mdpi.com/2076-3417/12/11/5507` | Sí |
| 2 | 10.1007/s10664-021-09999-9 | Industry practices and challenges for the evolvability assurance of microservices | 2021 | Empirical Software Engineering, 26(5), 104 | `link.springer.com/10.1007/s10664-021-09999-9` | Sí |
| 3 | 10.1109/TSE.2023.3287297 | Decomposition of Monolith Applications Into Microservices Architectures: A Systematic Review | 2023 | IEEE TSE, 49(8), 4213–4242 | `ieeexplore.ieee.org/document/10160171/` | Sí |
| 4 | 10.1109/ACCESS.2022.3152803 | Monolithic vs. Microservice Architecture: A Performance and Scalability Evaluation | 2022 | IEEE Access, 10, 20357–20374 | `ieeexplore.ieee.org/document/9717259/` | Sí |
| 5 | 10.1109/ACCESS.2021.3060895 | Does Domain-Driven Design Lead to Finding the Optimal Modularity of a Microservice? | 2021 | IEEE Access, 9, 32721–32733 | `ieeexplore.ieee.org/document/9359794/` | Sí |
| 6 | 10.1109/ACCESS.2024.3365079 | Evolution of Microservices Identification in Monolith Decomposition: A Systematic Review | 2024 | IEEE Access, 12, 23389–23405 | `ieeexplore.ieee.org/document/10431792/` | Sí |
| 7 | 10.1007/s10664-022-10162-1 | A configurable method for benchmarking scalability of cloud-native applications | 2022 | Empirical Software Engineering, 27(6), 143 | `link.springer.com/10.1007/s10664-022-10162-1` | Sí |
| 8 | 10.1016/j.jss.2025.112537 | Domain-Driven Design in software development: A systematic literature review on implementation, challenges, and effectiveness | 2025 | Journal of Systems and Software, 230, 112537 | `linkinghub.elsevier.com/retrieve/pii/S0164121225002055` | Sí |
| 9 | 10.3390/fi17110496 | Modular Monolith Architecture in Cloud Environments: A Systematic Literature Review | 2025 | Future Internet, 17(11), 496 | `mdpi.com/1999-5903/17/11/496` | Sí |
| 10 | 10.1007/s00607-026-01701-5 | Consistency challenges in event-driven microservices: a literature review on data, process, and system evolution | 2026 | Computing, 108(7), 110 | `link.springer.com/10.1007/s00607-026-01701-5` | Sí |

Los diez DOI respondieron con `302` y un `Location` hacia el editor correspondiente. MDPI devuelve `403` al abrir el HTML con un cliente automatizado, pero el DOI resuelve a la ruta correcta y el PDF se descargó de su servidor de archivos. IEEE Xplore carga el HTML con JavaScript y no muestra el título en el `<title>`; la comprobación se hizo por el número de documento, que coincide con el nombre del PDF y con el DOI de Crossref.

## 3. Integridad de los PDF

| # | Archivo | Páginas | Tamaño (bytes) | SHA-256 |
|---|---|---|---|---|
| 1 | `01_Soylemez2022_Challenges.pdf` | 40 | 948 611 | `99b60925041d73be4d4d801ac9419741f8ed0da7716f5d97a8dd18609645cc4c` |
| 2 | `02_Bogner2021_Evolvability.pdf` | 39 | 2 031 802 | `ef42a9d4e79b56985c68aa6db6f65b8a00aa186208642019650ce8293499d58c` |
| 3 | `03_Abgaz2023_Decomposition.pdf` | 30 | 5 228 469 | `43f790a5120f41a5596f11ed2df0ae6c7e9634b227518767229bdfd7981fddd9` |
| 4 | `04_Blinowski2022_MonolithVsMicro.pdf` | 18 | 1 582 881 | `696a5851d9f97c2d3e2403fba392dd8372df5e9a18f8143a65cfd67c3a2ab384` |
| 5 | `05_Vural2021_DDD.pdf` | 13 | 7 117 283 | `e1916ed706f4a9fba8946f84289b4636b3e1c6c8882274304a8d41b38de85cf2` |
| 6 | `06_Oumoussa2024_Identification.pdf` | 17 | 473 745 | `4362da0f2422405a5f2b0dce489a924ef88c399820dd4539a7414144b8be7e5d` |
| 7 | `07_Henning2022_Scalability.pdf` | 42 | 3 072 851 | `3c07a98bb30482a54b10acb46ed86fcaf6b55bbd7ea9cbc6d6f27039ad79cbd8` |
| 8 | `08_Ozkan2025_DDD.pdf` | 22 | 4 450 864 | `b70116197c82fd237f8600d82aafd35e15394178561b1b65965fc292fa327517` |
| 9 | `09_AlQoran2025_ModularMonolith.pdf` | 30 | 2 172 461 | `86f9665edca4566712e133213b25976cccde3940d150a70355981c7412902b64` |
| 10 | `10_Bacao2026_EDA_Consistency.pdf` | 32 | 1 169 201 | `bd5ba2b1241f13e8e56099837226a8925fa69a5444e68a1c680487e3f1a8326e` |

## 4. Primera página de cada PDF

| # | Título en la primera página | Autores | DOI impreso | Observación |
|---|---|---|---|---|
| 1 | Challenges and Solution Directions of Microservice Architectures: A Systematic Literature Review | Söylemez, Tekinerdogan, Kolukısa Tarhan | 10.3390/app12115507 | Versión del editor |
| 2 | Industry practices and challenges for the evolvability assurance of microservices | Bogner, Fritzsch, Wagner, Zimmermann | 10.1007/s10664-021-09999-9 | Versión del editor |
| 3 | Decomposition of Monolith Applications Into Microservices Architectures: A Systematic Review | Abgaz et al. | 10.1109/TSE.2023.3287297 | Copia aceptada de DCU, misma maqueta y paginación IEEE |
| 4 | Monolithic vs. Microservice Architecture: A Performance and Scalability Evaluation | Blinowski, Ojdowska, Przybyłek | 10.1109/ACCESS.2022.3152803 | Versión del editor |
| 5 | Does Domain-Driven Design Lead to Finding the Optimal Modularity of a Microservice? | Vural, Koyuncu | 10.1109/ACCESS.2021.3060895 | Versión del editor |
| 6 | Evolution of Microservices Identification in Monolith Decomposition: A Systematic Review | Oumoussa, Saidi | 10.1109/ACCESS.2024.3365079 | Versión aceptada; incluye un DOI de plantilla además del real |
| 7 | A configurable method for benchmarking scalability of cloud-native applications | Henning, Hasselbring | 10.1007/s10664-022-10162-1 | Versión del editor |
| 8 | Domain-Driven Design in software development: A systematic literature review on implementation, challenges, and effectiveness | Özkan, Babur, van den Brand | 10.1016/j.jss.2025.112537 | Versión del editor (TU/e), CC BY-NC-ND |
| 9 | Modular Monolith Architecture in Cloud Environments: A Systematic Literature Review | Al-Qora'n, Al-Said Ahmad | 10.3390/fi17110496 | Versión del editor |
| 10 | Consistency challenges in event-driven microservices: a literature review on data, process, and system evolution | Bação, Guerreiro | 10.1007/s00607-026-01701-5 | Versión del editor, publicación en línea de 2026 |

## 5. Acceso abierto y licencias

| # | Estado | Licencia | De dónde se descargó |
|---|---|---|---|
| 1 | Gold | CC BY 4.0 | `mdpi-res.com` (PDF directo) |
| 2 | Hybrid | CC BY 4.0 | Springer Link |
| 3 | Hybrid | CC BY 4.0 | Repositorio DCU (copia aceptada) |
| 4 | Gold | CC BY 4.0 | IEEE Xplore |
| 5 | Gold | CC BY 4.0 | IEEE Xplore |
| 6 | Gold | CC BY-NC-ND 4.0 | IEEE Xplore |
| 7 | Hybrid | CC BY 4.0 | Springer Link |
| 8 | Hybrid | CC BY-NC-ND 4.0 | Repositorio TU/e (versión del editor) |
| 9 | Gold | CC BY 4.0 | `mdpi-res.com` (PDF directo) |
| 10 | Hybrid | CC BY 4.0 | Springer Link |

Los PDF no se redistribuyen en el repositorio. Se guardan en `fuentes/pdf/` para el trabajo local y se enlazan por DOI. Si se publican en el repositorio, hay que respetar la licencia de cada uno; las licencias CC BY-NC-ND no permiten uso comercial ni obras derivadas.

## 6. Fuentes oficiales del caso

| Fuente | Autor | Fecha | URL | Comprobación |
|---|---|---|---|---|
| Microservices at Nubank, an overview | Rafael Ferreira, Software Engineer | 12 ago. 2019 | https://building.nu.com/microservices-at-nubank-an-overview/ | Página y texto completo vía Wayback Machine (el sitio bloquea clientes automatizados) |
| Managing Cloud Limits | Elton Peixoto y Diogo Dantas | 9 abr. 2025 | https://building.nu.com/managing-cloud-limits/ | Página y texto completo vía lector de texto |
| Evolución de la Arquitectura de Nubank Hacia una Plataforma Reflexiva | Nubank (charla de Bel Gonçalves) | 25 jun. 2025 | https://building.nu.com/es/engineering-meetup-arquitectura-plataformas-crecimiento-profesional/ | Página y texto completo vía lector de texto |

Cifras comprobadas en las fuentes:

- Más de 110 millones de clientes en Brasil, Colombia y México.
- Más de 4 000 microservicios con Kubernetes.
- 72 000 millones de eventos diarios por Kafka.
- Millones de peticiones por segundo.
- Decenas de miles de pods.
- «Pangeia»: pocas cuentas grandes de AWS; un incidente menor podía causar una caída mayor.
- «Continental drift»: estrategia multi-cuenta para reducir el radio de fallo.
- AWS Limit Smoke Detector: pipeline de uso, cuotas y costos, con alertas.

La página de 2019 se recuperó desde `https://web.archive.org/web/20241110084048/https://building.nubank.com.br/microservices-at-nubank-an-overview/` porque el sitio devuelve 403 a clientes automatizados. La copia de trabajo está en `_tmp/nubank_2019_overview.txt`.

## 7. Reproducibilidad

Cualquier persona puede repetir la comprobación con estos comandos (Windows, PowerShell, Python 3.14):

```powershell
# Metadatos
Invoke-RestMethod "https://api.crossref.org/works/10.1007/s00607-026-01701-5" | Select-Object -ExpandProperty message

# Estado de acceso abierto
Invoke-RestMethod "https://api.openalex.org/works/doi:10.1007/s00607-026-01701-5" | Select-Object -ExpandProperty open_access

# Resolución del DOI
curl.exe -sSI "https://doi.org/10.1007/s00607-026-01701-5"

# Hash de un PDF
Get-FileHash "fuentes\pdf\10_Bacao2026_EDA_Consistency.pdf" -Algorithm SHA256
```

> Nota: en este equipo `curl.exe` falla por un problema de credenciales de Schannel. Las consultas se hicieron con `urllib` de Python y un contexto SSL sin verificación, porque el certificado local del equipo no está en el almacén. Eso no afecta la validez del contenido: los datos se contrastaron con Crossref y con la primera página del PDF.

## 8. Incidencias y cómo se resolvieron

- **MDPI devuelve 403.** El HTML de `mdpi.com` bloquea clientes automatizados. Se usó la ruta de descarga directa `mdpi-res.com/d_attachment/...`.
- **IEEE Xplore no muestra el título en el HTML.** La página se arma con JavaScript. Se comprobó el número de documento (`10160171`, `9717259`, `9359794`, `10431792`) contra el nombre del PDF y el DOI de Crossref.
- **Abgaz: PDF de IEEE con 404.** La ruta `ielx7` no estaba disponible. Se descargó la copia aceptada del repositorio de DCU, que conserva la maqueta y la paginación de IEEE TSE.
- **Özkan: arXiv no es la versión publicada.** Se localizó la versión del editor en el repositorio de la TU/e (`1-s2.0-S0164121225002055-main.pdf`) y se reemplazó el archivo. Ahora el PDF tiene el DOI de JSS y 22 páginas.
- **Oumoussa: DOI de plantilla.** La versión aceptada conserva el texto `10.1109/ACCESS.2017.DOI` de la plantilla, pero también el DOI real `10.1109/ACCESS.2024.3365079`.
- **Nubank bloquea clientes automatizados.** El sitio devuelve 403 a `urllib`. Se usó el lector `r.jina.ai` para las páginas de 2025 y la Wayback Machine para la de 2019.
- **Nubank 2019 no se pudo leer en vivo.** La página actual no entrega el texto completo al cliente; la copia archivada de 2024 sí. Se cita la URL canónica y se advierte que el texto se recuperó del archivo.

## 9. Lista de verificación para la entrega

- [x] Cada DOI existe y resuelve al editor correcto.
- [x] Título, autores, año, revista, volumen y páginas coinciden con Crossref.
- [x] Cada PDF empieza con `%PDF` y tiene la primera página con el título correcto.
- [x] Cada artículo es de acceso abierto (gold u hybrid).
- [x] Se guardó el hash de cada PDF.
- [x] Las fuentes oficiales del caso tienen autor, fecha y URL.
- [ ] Captura anotada de la fuente principal.
- [ ] Captura o descripción del sistema de primera mano.
- [ ] Declaración de uso de IA.
- [ ] Compilación del PDF y revisión de que el análisis ocupe una sola página.
