¿Dnd estoy?


¿Qué tipo de proyectos quiero hacer?
¿A que me quiero dedicar?

A la inteligencia artificial, porque me llama muchísimo la cantidad de cosas diferentes que se pueden hacer en este campo de la informática, la cantidad de aplicaciones diferentes que tenemos y como nos ayudan a tener una vida mejor y más cómoda. En concreto, en la inteligencia artificial según los divide Hugging Face, hay diferentes ramas: modelos multimodales, modelos de computer vision, modelos de natural language processing, modelos para audio, modelos de tabular, de aprendizaje por refuerzo. Quiero aportar en aplicaciones que de verdad ayuden a la gente, lo cual mi empresa ahora mismo no casa con esto (Herta), cuando nuestra única fuente de ingresos fiable va a ser Kuwait que a saber para que lo van a usar ahí.

Tengo que buscar mi hueco y a lo que me quiero dedicar, ya que pensaba que iba a ser Data Scientist pero creo que no encaja con mi puesto, lo mío sería más como Ingeniero de Inteligencia Artificial/Machine Learning Engineer.

- ¿Qué tan cómodo te sientes con:
    - Contenedores y Docker?
    - Me siento cómodo trabajando con contenedores de Docker, estoy acostumbrado a trabajar con .devcontainer y a crear entornos mediante Dockerfiles para tenerlos aislados y poder trabajar mejor
    - CI/CD específicamente para modelos ML?
    - En este caso, tengo poca experiencia con CI/CD para modelos ML ya que los modelos que he desplegado han sido en sistemas embebidos que no requerían de estas tecnologías
    - Cloud platforms (AWS, GCP, Azure)?
    - Despliegue básico de webs en AWS, he usado instancias EC2 y Amazon RDS
- 
- En cuanto a MLOps:
    - ¿Has trabajado con herramientas de experiment tracking (MLflow, Weights & Biases)?
    - He usado solo MLFlow para que me pusiera las gráficas de entrenamiento y poder compararlas (Loss y mAP)
    - ¿Tienes experiencia con feature stores?
    - No
    - ¿Has implementado monitoring de modelos en producción?
    - En producción no, solo a nivel de entrenamiento
- Respecto a ML:
    - ¿Qué tan profundo es tu conocimiento de matemáticas (cálculo, álgebra lineal, estadística)?
    - Tengo una carrera en ingeniería informática y tengo la base que se da en esos estudios
    - ¿Has trabajado con distributed training?
    - No
    - ¿Has optimizado modelos para producción (cuantización, pruning, etc.)?
    - He optimizado modelos para tensorrt para despliegues en producción más óptimos en gráficas de NVidia
- Sobre datos:
    - ¿Tienes experiencia con bases de datos NoSQL?
    - No
    - ¿Has trabajado con procesamiento distribuido (Spark)?
    - No
    - ¿Qué experiencia tienes con data versioning?
    - No tengo experiencia

# Roadmap para ML Engineer

## Prioridad Alta (1-3 meses)

### MLOps Fundamentals

- Profundizar en CI/CD para ML
    - GitHub Actions o Jenkins para automatizar training pipelines
    - Automatización de tests para modelos ML
    - Integración de quality gates para modelos
- Experiment Tracking Avanzado
    - Weights & Biases (más usado en la industria que MLflow)
    - Gestión de hyperparámetros
    - Colaboración en equipo y reproducibilidad
- Model Monitoring
    - Prometheus + Grafana para métricas de modelos
    - Monitorización de data drift
    - Alerting systems

### Cloud & Infrastructure

- AWS para ML (ampliar conocimientos actuales)
    - SageMaker para training y deployment
    - AWS Lambda para serverless inference
    - S3 para model storage
    - ECR para container registry
- Kubernetes basics
    - Deployments y Services
    - Resource management
    - Kubeflow introduction

## Prioridad Media (3-6 meses)

### Data Engineering

- Bases de datos NoSQL
    - MongoDB para datos no estructurados
    - Redis para caching
- Data Versioning
    - DVC (Data Version Control)
    - Git LFS
- Feature Stores
    - Feast
    - Feature computation y serving

### ML Scaling

- Distributed Training
    - PyTorch DDP (DistributedDataParallel)
    - Horovod
    - Multi-GPU training
- Model Optimization (ampliar conocimientos actuales)
    - Pruning techniques
    - Knowledge distillation
    - Mixed precision training

## Prioridad Baja (6+ meses)

### Big Data

- Apache Spark
    - PySpark para procesamiento distribuido
    - Spark MLlib
- Data Lakes
    - Arquitectura y mejores prácticas
    - Delta Lake

### Advanced MLOps

- Feature Stores avanzados
    - Offline vs Online serving
    - Backfilling
- A/B Testing
    - Diseño de experimentos
    - Statistical significance
- Advanced Monitoring
    - Concept drift detection
    - Automated retraining pipelines

## Proyectos Prácticos Recomendados

1. **ML Pipeline End-to-End**
    - Crear un pipeline completo usando GitHub Actions
    - Implementar tracking con W&B
    - Desplegar en AWS SageMaker
    - Incluir monitoring básico
2. **Distributed Training Project**
    - Entrenar un modelo grande en múltiples GPUs
    - Implementar diferentes estrategias de paralelización
    - Comparar rendimiento y escalabilidad
3. **Feature Store Implementation**
    - Construir un feature store simple
    - Implementar offline y online serving
    - Integrar con un pipeline de ML existente

## Recursos Recomendados

### Cursos

- "Machine Learning Engineering for Production (MLOps)" en Coursera
- "Full Stack Deep Learning" bootcamp
- "Distributed Training" en Fast.ai

### Libros

- "Designing Machine Learning Systems" por Chip Huyen
- "Machine Learning Design Patterns" por Lakshmanan et al.
- "Building Machine Learning Powered Applications" por Emmanuel Ameisen

### Blogs/Newsletters

- Neptune.ai blog
- Chip Huyen's blog
- Eugene Yan's blog
- MLOps Community newsletter


# ML Engineering Portfolio Projects

## 1. Sistema de Recomendación de Productos (2-3 meses)

**Objetivo:** Crear un sistema de recomendación end-to-end con enfoque en MLOps y deployment.

### Características técnicas:

- FastAPI para crear la API del modelo
- Redis para caching de recomendaciones
- MongoDB para almacenar datos de usuarios/productos
- Docker Compose para orquestar servicios
- GitHub Actions para CI/CD
- Monitoring con Prometheus + Grafana
- Deployment en AWS usando ECS o Kubernetes

### Aprendizaje:

- Arquitectura de sistemas ML en producción
- Bases de datos NoSQL
- Monitorización de modelos
- CI/CD práctico
- Caching y optimización

## 2. Sistema de Detección de Anomalías en Tiempo Real (2-3 meses)

**Objetivo:** Procesar y analizar streams de datos en tiempo real.

### Características técnicas:

- Kafka para streaming de datos
- Feature Store con Feast
- Modelo de detección de anomalías (isolation forest/autoencoder)
- Dashboard en tiempo real con Streamlit
- AWS Kinesis/Lambda para procesamiento
- Sistema de alertas

### Aprendizaje:

- Procesamiento en tiempo real
- Feature stores
- Sistemas distribuidos
- Visualización de datos en tiempo real
- Serverless computing

## 3. Sistema de Clasificación de Imágenes Distribuido (2-3 meses)

**Objetivo:** Crear un sistema de clasificación que maneje grandes volúmenes de datos y entrenamiento distribuido.

### Características técnicas:

- PyTorch DDP para entrenamiento distribuido
- Weights & Biases para experiment tracking
- DVC para versionado de datos
- Model serving con TorchServe
- A/B testing framework simple
- Optimización con TensorRT (tu experiencia previa)

### Aprendizaje:

- Entrenamiento distribuido
- Gestión y versionado de datos
- A/B testing
- Model serving
- Optimización de modelos

## Bonus: MLOps Platform (Proyecto Integrador)

**Objetivo:** Integrar los proyectos anteriores en una plataforma unificada.

### Características técnicas:

- UI central con React/Next.js
- API Gateway para gestionar servicios
- Kubernetes para orquestación
- Monitoring centralizado
- Sistema de logging unificado
- Dashboard de métricas

### Aprendizaje:

- Arquitectura de sistemas complejos
- Microservicios
- Orquestación de contenedores
- Integración de sistemas

## Consideraciones para cada proyecto:

1. Comenzar pequeño y añadir funcionalidades incrementalmente
2. Documentar bien el proceso y decisiones técnicas (excelente para el portfolio)
3. Escribir posts técnicos sobre cada proyecto
4. Crear diagramas de arquitectura
5. Incluir tests y best practices
6. Mantener el código en GitHub con buena documentación