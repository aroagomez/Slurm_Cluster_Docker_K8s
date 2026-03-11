# Orquestación de Clúster HPC con Slurm, Docker y Kubernetes

Este proyecto despliega una infraestructura funcional para procesamiento masivo basada en **Slurm Workload Manager**, empaquetada como un conjunto de servicios orquestados. El objetivo es simular un entorno HPC (High Performance Computing) realista para la ejecución de trabajos (Job Arrays, MPI) con gestión de recursos y contabilidad.

> **Créditos y Repositorio Base:** > Este proyecto toma como punto de partida el excelente trabajo de `giovtorres/slurm-docker-cluster`. Sobre esta base, se ha extendido la arquitectura, se ha integrado una interfaz de monitorización de base de datos, se han configurado particiones personalizadas y se ha desarrollado una Prueba de Concepto (PoC) para su despliegue en Kubernetes.

## Arquitectura y Servicios (Docker Compose)
La infraestructura centralizada despliega los siguientes contenedores interconectados:
* **slurmctld:** Controlador principal del clúster.
* **slurmdbd / MariaDB:** Demonio de contabilidad y base de datos para el registro histórico de jobs.
* **slurmrestd:** Servicio API REST para interactuar con Slurm.
* **Adminer:** WebUI añadida para la inspección y gestión directa de la base de datos de contabilidad.
* **Nodos de cómputo (c1-c4):** 4 workers escalables para la ejecución de cargas de trabajo.

## Configuración de Particiones y Políticas
Para simular un entorno de producción, el clúster se ha segmentado en diferentes colas de trabajo:
* `normal`: Partición por defecto para trabajos de duración indefinida.
* `rapida`: Partición especializada con límite de tiempo (2:00:00) para testeo ágil y debugging.
* Implementación de **políticas QoS** y limitaciones de recursos evaluadas mediante `sacct`.

## Prueba de Concepto en Kubernetes (K8s)
Como extensión a la orquestación con Docker Compose, el proyecto incluye una exploración de despliegue declarativo en Kubernetes. Esta PoC replica la arquitectura básica como pods dentro de un clúster K8s, sentando las bases para una futura implementación con persistencia estable (PVC), gestión de secretos y políticas de seguridad avanzadas.