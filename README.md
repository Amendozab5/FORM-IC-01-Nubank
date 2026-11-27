# FORM-IC-01 — Investigación individual: arquitecturas distribuidas en la industria

**Caso:** Nubank y su arquitectura orientada a eventos (EDA), con microservicios como unidad de despliegue.  
**Estudiante:** Angello Agustín Mendoza Bermello  
**Asignatura:** Aplicaciones Distribuidas — UTEQ, Facultad de Ciencias de la Computación y Diseño Digital  
**Carrera:** Ingeniería en Software, 7mo nivel, paralelo B  
**Docente:** Ing. Gleiston Cicerón Guerrero Ulloa  
**Semana:** 5 (23 al 29 de noviembre de 2026)  
**Repositorio:** https://github.com/Amendozab5/FORM-IC-01-Nubank

## De qué trata

El trabajo investiga un caso real de arquitectura distribuida en producción y lo analiza en una página. El caso elegido es **Nubank**, el banco digital brasileño. La empresa atiende a más de 110 millones de clientes en Brasil, Colombia y México, opera más de 4 000 microservicios sobre Kubernetes y procesa 72 000 millones de eventos diarios a través de Kafka. La comunicación principal entre servicios es asíncrona: los servicios publican mensajes en un broker replicado y los consumen después.

Las fuentes principales son tres entradas del blog técnico oficial *Building Nubank*: la descripción de microservicios de 2019 (Rafael Ferreira), la gestión de límites de nube de 2025 (Elton Peixoto y Diogo Dantas) y la evolución de la arquitectura de 2025 (charla de Bel Gonçalves).

El análisis se apoya en diez artículos científicos de acceso abierto, publicados entre 2021 y 2026, con DOI verificado. La lista y las comprobaciones están en `fuentes/`.

## Compilación

> **Documento:** el repositorio ya incluye `informe/main.tex` y `informe/main.pdf`. El PDF tiene cinco páginas: carátula, análisis de una página, referencias, Anexo A (captura) y una página con los anexos B y C (sistema de primera mano y declaración de IA).

- **Compilador:** pdfLaTeX (MiKTeX 26.5 o TeX Live 2024 actualizado) con Biber.
- **Archivo principal:** `informe/main.tex`.
- **Orden de comandos:**

```bash
cd informe
pdflatex main.tex
biber main
pdflatex main.tex
pdflatex main.tex
```

- **Dependencias:** `biblatex` con estilo `ieee`, `biber`, `babel` (español), `csquotes`, `geometry`, `graphicx`, `booktabs`, `array`, `caption`, `float`, `tikz`, `hyperref`, `fancyhdr`, `microtype`, `parskip`, `xcolor`, `lmodern`.
- **Rutas:** `main.tex` usa `../referencias.bib` y `../capturas/`. Hay que compilar desde `informe/` o subir el repositorio completo a Overleaf y fijar `informe/main.tex` como documento principal.

## Estructura del repositorio

```
.
├── README.md
├── BITACORA.md              # registro fechado del trabajo
├── referencias.bib          # fuentes en BibLaTeX, estilo IEEE
├── docs/
│   └── 01-analisis-tema.md  # análisis extenso de esta etapa
├── fuentes/
│   ├── FUENTES.md           # ficha bibliográfica de cada artículo
│   ├── VERIFICACION.md      # DOI, metadatos, hashes y licencias
│   └── pdf/                 # PDFs de los artículos (no versionados)
├── capturas/                # evidencia: fuente anotada, sistema propio
├── informe/                 # main.tex y PDF compilado
└── sga/                     # PDF de una página para el SGA
```

## Fuentes

- **Caso:** *Building Nubank* (blog técnico oficial):
  - R. Ferreira, «Microservices at Nubank, an overview», 12 ago. 2019. https://building.nu.com/microservices-at-nubank-an-overview/
  - E. Peixoto y D. Dantas, «Managing Cloud Limits», 9 abr. 2025. https://building.nu.com/managing-cloud-limits/
  - Nubank, «Evolución de la Arquitectura de Nubank Hacia una Plataforma Reflexiva», 25 jun. 2025. https://building.nu.com/es/engineering-meetup-arquitectura-plataformas-crecimiento-profesional/
- **Artículos:** diez referencias académicas de acceso abierto, entre 2021 y 2026, con DOI. El detalle está en `fuentes/FUENTES.md` y la verificación en `fuentes/VERIFICACION.md`.

## Estado del trabajo

- [x] Caso elegido y delimitado (Nubank, EDA, microservicios).
- [x] Fuentes oficiales localizadas, con autor, fecha y cifras.
- [x] Corpus de diez artículos descargado y verificado.
- [x] Metadatos, DOI, licencias y hashes registrados.
- [x] Captura anotada de la fuente principal.
- [x] Evidencia de un sistema de primera mano.
- [x] Redacción del análisis de una página en LaTeX.
- [x] Compilación y revisión del PDF.
- [ ] PDF de una página para el SGA.
- [x] Repositorio publicado y primer commit subido.
- [x] Segundo commit: documento, captura y PDF compilado.
- [ ] Tercer commit: PDF del SGA y cierre.

## Declaración de uso de inteligencia artificial

La declaración completa irá en el documento final. En resumen: se usó un asistente de programación (DeepSeek Harness, modelo DeepSeek Flash) para buscar candidatos de fuentes, ordenar el repositorio, revisar el formato LaTeX y proponer una estructura de redacción. Ninguna cifra del caso se tomó de texto generado: todas salen del blog de Nubank y de los artículos descargados. Los datos bibliográficos se comprobaron uno por uno en Crossref y en la primera página de cada PDF. La redacción final y las decisiones de análisis son del estudiante.
