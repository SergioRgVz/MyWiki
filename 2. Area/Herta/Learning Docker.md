Docker es una plataforma que permite empaquetar aplicaciones y sus dependencias en contenedores. Estos conetenedores son unidades estandarizadas de software que incluyen lo necesario para que una aplicación se ejecute.

# Conceptos clave
## Imagen
Una plantilla de solo lectura que contiene el código, las dependencias y la configuración de una aplicación. Es como la snapshot de un sistema. Contiene:
- El sistema operativo base
- Las dependencias de la aplicación
- El código de la aplicación
- Configuraciones necesarias
Las imágenes son inmutables, si necesitas hacer modificaciones, necesitas crear una nueva imagen. Se construyen por capas, lo que permite reutilizar partes comunes entre diferentes imágenes.
##  Contenedor
Una instancia ejecutable de una imagen. Es un proceso aislado que:
- Tiene su propio sistema de archivos (proporcionado por la imagen)
- Puede tener su propia red
- Corre de forma aislada de otros contenedores y del sistema host
- Es efímero (puede ser detenido, iniciado o eliminado fácilmente)
## Dockerfile
Un archivo de texto que contiene las instrucciones para construir una imagen Docker. Algunas instrucciones básicas son:
- FROM: especifica la imagen base
- COPY o ADD: copia archivos al contenedor
- RUN: ejecuta comandos durante la construcción
- CMD o ENTRYPOINT: especifica que comando se ejecutará cuando el contenedor se inicie
## Docker Hub
Un repositorio en línea para compartir y obtener imágenes de Docker. Proporciona un registro público de imágenes Docker, capacidad para almacenar y compartir tus propias imágenes, integración con Github y Bitbucket para construcción automática de imágenes.

# Primeros pasos
a) Listar imágenes
```bash
docker images
```

b) Listar contenedores en ejecución
```shell
docker ps
```

c) Listar todos los contenedores (incluyendo los detenidos):
```bash
docker ps -a
```

d) Detener un contenedor:
```bash
docker stop [ID_DEL_CONTENEDOR]
```

e) Eliminar un contenedor:
```bash
docker rm [ID_DEL_CONTENEDOR]
```

f) Eliminar una imagen:
```bash
docker rmi [NOMBRE_DE_LA_IMAGEN]
```

# Primera imagen de Docker

Creemos un nuevo directorio para el proyecto llamado `test`: 
```bash
mkdir mi_app_docker
cd mi_app_docker
```

Creamos el fichero app.py:
```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return "¡Hola desde Docker!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

Creamos un fichero requirements.txt:
```
Flask==2.0.1
```

Y creamos nuestro Dockerfile:
```bash
# Usa una imagen base de Python
FROM python:3.9-slim

# Establece el directorio de trabajo en el contenedor
WORKDIR /app

# Copia los archivos de requisitos y los instala
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copia el resto de los archivos de la aplicación
COPY . .

# Expone el puerto en el que se ejecutará la aplicación
EXPOSE 5000

# Define el comando para ejecutar la aplicación
CMD ["python", "app.py"]
```

Contstruimos la imagen con: 
`docker build -t mi-app-flask .`

Y lanzamos la imagen con:
`docker run -p 5000:5000 mi-app-flask`


1. Gestión de contenedores:

a) Ejecutar un contenedor en modo detach (en segundo plano):
```
docker run -d -p 5000:5000 mi-app-flask
```
El flag -d ejecuta el contenedor en modo detach

b) Para ver los logs de un contenedor
```
docker logs [ID_DEL_CONTENEDOR]
```

c) Ejecutar un comando dentro de un contenedor en ejecución:
```
docker exec -it [ID_DEL_CONTENEDOR] /bin/bash
```

2. Docker volumes: los volúmenes permiten persistir datos y compartirlos entre contenedores.
a) Crear un volumen
```
docker volume create mi-volumen
```

b) Ejecutar un contenedor con un volumen
```
docker run -v mi-volumen:/data mi-app-flask
```

3. Docker Network: las redes Docker permiten la comunicación entre contenedores
a) Crear una red:
```
docker network create mi-red
```

b) Ejecutar un contenedor en una red específica:
```
docker run --network mi-red mi-app-flask
```

4. Docker Compose: es una herramienta para definir y ejecutar aplicaciones Docker multi-contenedor
Creamos un archivo docker-compose.yml:
```yml
version: '3'
services:
  web:
    build: .
    ports:
      - "5000:5000"
  redis:
    image: "redis:alpine"
```

Ejecutamos la aplicación con Docker Compose:
```
docker-compose up
```
