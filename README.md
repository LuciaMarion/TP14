# TP14: Threagile Modelado de amenazas
## ¿Qué es Threagile y por qué lo integramos en el pipeline CI/CD?
Threagile es una herramienta de código abierto que nos permite hacer Modelado de Amenazas Ágil de forma declarativa. Esto significa que, en lugar de dibujar diagramas en pizarrones que se olvidan, describimos la arquitectura de nuestra aplicación en un archivo de texto llamado threagile.yaml directamente en nuestro editor de código.
Al integrar Threagile en nuestro pipeline de GitHub Actions (CI/CD), logramos que cada vez que cambie nuestra infraestructura o la forma en que los contenedores se comunican, el pipeline ejecute automáticamente un análisis de riesgos de seguridad. Threagile evaluará si hay fallas en el diseño y generará reportes en PDF y diagramas de flujo de datos actualizados sin que tengamos que hacerlo a mano.

## Paso 1: Preparar entorno de trabajo local
Primero, ingresaremos a la carpeta de nuestro proyecto de la app de notas para trabajar desde ahí.
Integrar todos los cambios hasta el TP12. Lo que incluye los 7 dockers: 3 de aplicacion Notes App mas 4 de monitoreo en Kubernetes con ingress, helm, y terraform.

## Paso 2: Generar un modelo de amenazas inicial (Stub Model)
docker run --rm -it -v "$(pwd)":/app/work threagile/threagile --create-stub-model --output /app/work
Este comando descarga y ejecuta el contenedor oficial de Threagile, creando un archivo de plantilla básico llamado threagile-stub-model.yaml en la carpeta actual.

## Paso 3: Renombrar la plantilla para que sea tu modelo oficial
Para que Threagile y las GitHub Actions reconozcan el modelo por defecto, debemos renombrar el archivo generado
Para eso se utiliza: mv threagile-stub-model.yaml threagile.yaml

## Paso 4: Adaptar el modelo a la arquitectura de la Notes App
Se configuró el archivo threagile.yaml con la arquitectura completa de la aplicación para realizar el modelado de amenazas con Threagile. 
Se definieron los 7 contenedores/activos del ecosistema y sus interacciones en las 5 secciones principales del archive

## Paso 5: Validar la sintaxis de tu modelo localmente
Antes de subir los cambios al repositorio, se ejecutó una prueba local utilizando el contenedor oficial de Threagile mediante Docker para validar la sintaxis YAML, la coherencia de la arquitectura y detectar posibles errores de indentación.

## Paso 6: Integrar Threagile en tu Pipeline (cicd.yml)
Se modificó el pipeline de CI/CD (.github/workflows/cicd.yml) para incorporar el modelado automático de amenazas como parte de las prácticas de DevSecOps. Cada vez que se realiza un cambio en la arquitectura o en el código (git push), el pipeline evalúa los riesgos de seguridad sin intervención manual.

## Paso 7: Agregar los archivos al control de versiones de Git
Se agregaron al control de versiones los archivos de configuración modificados:
-	git add threagile.yaml
-	git add .github/workflows/cicd.yml

## Paso 8: Crear un commit con los cambios
Se creó un commit en el repositorio local para registrar la integración de Threagile en el proyecto:

## Paso 9: Subir los cambios a tu repositorio en GitHub
Para activar el pipeline en los servidores de GitHub y ver la automatización en ejecución, se envían los cambios a la rama principal.

## Paso 10: Verificar que el pipeline funciona correctamente
Monitoreo en Actions: Se navegó a la pestaña Actions en GitHub y se seleccionó la corrida correspondiente al commit del TP14.
Resultado Exitoso: Se confirmó que el job Threat Model Analysis finalizó correctamente en verde.
Descarga de Artefactos: En la sección Artifacts al pie de la ejecución, se descargó el archivo comprimido threagile-report

## Mapa de carpetas

```text
raíz-del-repositorio/             # ejecutar Git y editar .github aquí
├── .github/workflows/cicd.yml
├── .yamllint.yml                 # reglas usadas por el job de lint
├── devops-tp12/                 # proyecto integrado y modelo TP14
│   ├── app/ chart/ scripts/
│   └── threagile.yaml           # lo crearás; no viene resuelto
├── devops-TP06/                 # copia histórica usada por jobs previos
├── guia-06/                     # Docker Compose
├── guia-08/                     # Prometheus y Grafana
├── guia-09/                     # Kubernetes
├── guia-10/                     # Helm e Ingress
├── guia-11/                     # Terraform
└── guia-12/                     # portfolio integrado
```
