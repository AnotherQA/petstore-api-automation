# PetStore API Automation – Postman & Newman

Proyecto de **automatización de pruebas de API REST** utilizando **Postman** para el diseño de casos y **Newman** para la ejecución por línea de comandos.  
La suite valida un flujo **CRUD end-to-end**, incluye **data-driven testing con CSV** y genera **reportes HTML** como evidencia de ejecución.

**Autor:** Wilder Carranza  
**Rol:** QA Analyst (Automation – nivel básico/intermedio)

---

## Tecnologías utilizadas

- Postman (Collections & Environments)
- Newman (CLI Runner)
- Newman Reporter Htmlextra (HTML Report)
- CSV (Data Driven Testing)
- Node.js

---

## Estructura del proyecto

petstore-api-automation/
├── collections/ # Colecciones Postman exportadas (JSON)
├── environments/ # Environments Postman exportados (JSON)
├── data/ # Archivos CSV para data-driven testing
├── reports/ # Reportes HTML de ejecución
├── newman/ # Carpeta generada por Newman
├── .gitignore
├── package.json
└── README.md


---

## Alcance de pruebas

### 00_Auth
- Validación básica de sesión (login)

### 01_Smoke_Tests
- Health check de la API (`findByStatus`)
- Validación de tiempo de respuesta

### 02_Functional_Regression (CRUD End-to-End)
- **Create Pet** (POST)
- **Get Pet by ID** (GET)
- **Update Pet** (PUT)
- **Delete Pet** (DELETE)
- Manejo dinámico de variables (`pet_id`) entre requests

### 03_Negative_Testing
- Creación con datos inválidos
- Body vacío
- ID inexistente

---

## ⚙️ Requisitos previos

- Node.js (versión LTS recomendada)
- Newman

Instalación global (opcional):

```bash
npm install -g newman
npm install -g newman-reporter-htmlextra

```
O instalación local en el proyecto:

npm install

### Ejecución de pruebas
Ejecutar pruebas funcionales CRUD con CSV

newman run collections/PetStore_Collection.json ^
 -e environments/PetStore_Environment.json ^
 -d data/mascotas.csv ^
 --folder "02_Functional_Regression"

También puede ejecutarse en una sola línea:

newman run collections/PetStore_Collection.json -e environments/PetStore_Environment.json -d data/mascotas.csv --folder "02_Functional_Regression"

### Generación de reporte HTML
newman run collections/PetStore_Collection.json ^
 -e environments/PetStore_Environment.json ^
 -d data/mascotas.csv ^
 --folder "02_Functional_Regression" ^
 -r cli,htmlextra ^
 --reporter-htmlextra-export reports/PetStore_Report.html

El reporte se genera en:

reports/PetStore_Report.html

### Data Driven Testing
Las pruebas utilizan un archivo CSV (data/mascotas.csv) para ejecutar múltiples iteraciones.
El identificador pet_id se genera dinámicamente por iteración mediante scripts en Postman, evitando colisiones de datos.

### Notas técnicas
*Se implementó manejo defensivo de respuestas para evitar errores cuando la API devuelve formatos distintos a JSON.
*Las variables de entorno permiten encadenar requests en un flujo end-to-end.
*El proyecto está preparado para integrarse a pipelines CI/CD.

### Evidencia de ejecución
Reporte HTML disponible en la carpeta reports/