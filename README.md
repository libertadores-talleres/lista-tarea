[![AWS EC2](https://img.shields.io/badge/AWS-EC2-orange?logo=amazon-aws&logoColor=white)](http://18.225.9.200:8080)
[![CI](https://github.com/Danielsuarez1-ops/mi-lista-tareas/actions/workflows/ci.yml/badge.svg)](https://github.com/Danielsuarez1-ops/mi-lista-tareas/actions/workflows/ci.yml)
[![Docker Ready](https://img.shields.io/badge/Docker-Ready-blue?logo=docker&logoColor=white)](https://www.docker.com/)





# 📌 Guia de Desarrollo Mi Lista de Tareas   EC2 [![AWS EC2](https://img.shields.io/badge/AWS-EC2-orange?logo=amazon-aws&logoColor=white)](http://18.225.9.200:8080)

# 👥 Equipo de desarrollo




# 💡 Descripción aplicativo

Este proyecto es una aplicación web sencilla tipo **To-Do List**, desarrollada con **HTML, CSS y JavaScript**.  
Tiene tres funcionalidades principales:

---

## ✨ Funcionalidades
- ➕ Agregar nuevas tareas.
- ✅ Marcar tareas como completadas.
- 🗑️ Eliminar tareas de la lista.

---

## URL de la aplicación desplegada
👉 [http://3.139.90.170:8080](http://3.139.90.170:8080)

---

## 🚀 Demo en línea

Puedes probar la aplicación aquí:  
👉 [Mi Lista de Tareas en GitHub Pages](https://danielsuarez1-ops.github.io/mi-lista-tareas/)


---

## 🛠️ Tecnologías utilizadas
- **HTML5** → estructura de la página.  
- **CSS3** → estilos y diseño responsivo.  
- **JavaScript (Vanilla JS)** → lógica de la aplicación.
- **AWS EC2** (Ubuntu 24.04 LTS)

---

## Requicitos previos

 - Cuenta en AWS (Free Tier).
- Instancia EC2 con Ubuntu 24.04.
- Configuración de clave SSH para conectarse.
- Puertos abiertos en **Security Groups**: 22 (SSH) y 8080 (HTTP para la app).
- Git y Node.js instalados en la instancia.

---

## 🔧 Paso 1: 🛡️ Configuración del Security Group
Asegúrate de que tu Security Group tenga estas reglas:


SSH   	22	  Tu IP o 0.0.0.0/0	   Acceso SSH

TCP Personalizado	  8080	   0.0.0.0/0	  Servidor desarrollo

---

## 🔧 Paso 2: Conectarse a la Instancia EC2

   1. Ve a **AWS Console** 	➟ EC2 ➟ **Instancias**
   2. Selecciona tu instancia
   3. Clic en **"Connect"**
   4. Selecciona **"EC2 Instance Connect"**
   5. Click en **Connect**


---

## 🔧 Paso 3: Preparar el Entorno

**Actualizar el sistema e instalar dependencias**

   1. sudo apt update && sudo apt upgrade -y 
   2. sudo apt update && sudo apt upgrade -y && sudo apt install -y nodejs npm
   3. node -v
   npm -v
   4. sudo apt install -y git

---

## 🔧 Paso 4: Clonar el Repositorio

   5. git clone https://github.com/danielsuarez1-ops/mi-lista-tareas.git
   6. cd mi-lista-tareas


---

## 🔧 Paso 5: Instalar Dependencias

   7. sudo npm install -g http-server


---

## 🔧 Paso 6: Ejecutar el Servidor del Desarrollo

   8. cd ~/mi-lista-tareas && http-server -p 8080

<img width="1050" height="524" alt="image" src="https://github.com/user-attachments/assets/f7ebe54c-0983-4426-a9ed-336c0fab317b" />


---

## 🔧 Paso 7: Accerde a la Aplicacion

Ir al navegador: http://3.139.90.170:8080

![alt text](/Imagenes/image-1.png)
---

## 🔧 Paso 8: Detener el Servidor 

Presiona **Ctrl + C**  en la terminal

---
## Problemas encontrados y soluciones

❌ ~sudo → daba error porque no existe el comando, lo corregí a sudo.

❌ npm install fallaba → la app no tenía package.json, descubrí que no lo necesitaba al ser HTML estático.

❌ cd ~/mi-lista-tareas http-server -p 8080 → error de “too many arguments”, lo solucioné ejecutando comandos en líneas separadas.


---

## Consejos y mejores prácticas

- Siempre separar comandos con && o en líneas distintas.

- Usar http-server para proyectos estáticos.

- No exponer más puertos de los necesarios en AWS.

- Verificar con node -v y npm -v que las herramientas se instalen correctamente.


---
## ✅ Integración Continua (CI con GitHub Actions) [![CI](https://github.com/Danielsuarez1-ops/mi-lista-tareas/actions/workflows/ci.yml/badge.svg)](https://github.com/Danielsuarez1-ops/mi-lista-tareas/actions/workflows/ci.yml)


## 🔄¿Qué se automatiza?

- ➕ Tests automáticos: Se ejecutan cada vez que se hace push
- ➕ Checks: Valida configuración del proyecto
- ➕ Verificación de migraciones: Asegura que no falten migraciones

## 🔄 ¿Cuándo se ejecuta?


- Cada push a la rama main
- Cada Pull Request
- Manualmente desde GitHub

Para que aparezca la etiqueta passing configuré GitHub Actions:

---

## 1) Creación del Workflow

Se creó el archivo .github/workflows/ci.yml con este contenido:

name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repo
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test

---

## 2) Creación de pruebas con Jest

Agregué un archivo en tests/basic.test.js:

// tests/basic.test.js
describe('Basic project checks', () => {
  test('arithmetic works', () => {
    expect(1 + 1).toBe(2);
  });

  test('index.html exists', () => {
    const fs = require('fs');
    expect(fs.existsSync('index.html')).toBe(true);
  });
});


En package.json añadí el script de test:

"scripts": {
  "test": "jest"
}

---


## 3) Resultado

Cuando hago un commit a main, GitHub Actions ejecuta los tests.

Si todo pasa, el badge PASSING aparece en el README, cuando yo hacia algun comando mal aparecia FALLING.

Los test se pueden verificar en **"Actions"**.


---

##  ⚠️ Problemas encontrados y soluciones

❌ GitHub Actions no corría tests → agregué un archivo basic.test.js mínimo para validar la app.

❌ Cuando queria guardar cambios en el archivo ci.yml, GitHub no me dejaba hacer commit directo a la rama main. Solucion: en lugar de intentar forzar el commit a main, seleccione la opción:
Create a new branch for this commit and start a pull request, luego hice un Pull Request desde add-ci hacia main.

❌ Error “No tests found” en GitHub Actions, Jest no detectaba ningún archivo de pruebas. Solución: guardé mis pruebas en la carpeta tests/ y con el sufijo .test.js (por ejemplo: basic.test.js).

✅ Finalmente logré que el workflow se ejecute y aparezca el badge passing.


---

# 📦 Dockerización de la aplicación [![Docker Ready](https://img.shields.io/badge/Docker-Ready-blue?logo=docker&logoColor=white)](https://www.docker.com/)


Para desplegar esta aplicación en contenedores, se utilizó **Docker** con **nginx** como servidor web.  
Esto permitió empaquetar el proyecto en una imagen ligera y replicable en cualquier entorno.

---

## 🛠️ Dockerfile

El archivo `Dockerfile` se creó en la raíz del proyecto con el siguiente contenido:

 Usar una imagen base ligera de nginx
FROM nginx:alpine

 Borrar contenido default de nginx
RUN rm -rf /usr/share/nginx/html/*

 Copiar los archivos de la app
COPY . /usr/share/nginx/html

 Exponer el puerto 80
EXPOSE 80

 Comando por defecto de nginx
CMD ["nginx", "-g", "daemon off;"]

---

## 🚫 .dockerignore

Se creó un archivo .dockerignore para evitar copiar archivos innecesarios a la imagen:

README.md
Dockerfile
.dockerignore
.git
.github

---

## 🚀 Construcción y ejecución con Docker

1. Construir la imagen
sudo docker build -t mi-lista-tareas:1.0 .

2. Ejecutar el contenedor
sudo docker run --name mi-lista-tareas -p 8080:80 -d mi-lista-tareas:1.0


-p 8080:80 → expone el puerto 8080 del servidor hacia el 80 del contenedor.

-d → lo corre en segundo plano.

<img width="1862" height="570" alt="image" src="https://github.com/user-attachments/assets/d4b40c73-f337-4bf0-b83b-816a46109f7b" />

---

## 🔧 Docker Compose

Para simplificar el despliegue se creó un archivo docker-compose.yml:

version: "3.9"

services:
  mi-lista-tareas:
    build: .
    container_name: mi-lista-tareas
    ports:
      - "8080:80"
    restart: always

Comandos principales:

Levantar el contenedor:

sudo docker compose up -d


Reiniciar (cuando se hacen cambios):

sudo docker compose down && sudo docker compose up -d

<img width="1857" height="265" alt="image" src="https://github.com/user-attachments/assets/8b7b6c88-69cc-43d2-920b-aca5e3a024c1" />


---

## ⚠️ Problemas encontrados y soluciones


❌ Docker no estaba instalado en la instancia EC2
👉 Solución: se creó un script install-docker.sh que instala y habilita Docker en Ubuntu 24.04.

❌ Error: no se encontraba el Dockerfile
👉 Solución: crear el archivo en la carpeta correcta (~/mi-lista-tareas) usando nano Dockerfile.

❌ docker: Error response from daemon: Conflict. The container name "/mi-lista-tareas" is already in use
👉 Solución: detener y eliminar el contenedor viejo con:

sudo docker stop mi-lista-tareas
sudo docker rm mi-lista-tareas

---

## 🚀 Conclusión

Gracias a Docker, mi aplicación ahora:
- Se despliega de forma rápida y replicable en cualquier servidor.
- No depende de configuraciones locales, todo está dentro del contenedor.
- Puede escalar fácilmente usando Docker Compose o Kubernetes en el futuro.

Esto garantiza que la aplicación sea **portable, ligera y lista para producción**.

![alt text](/Imagenes/image-1.png)

# Metodologia Kanban

Se realiza seguimiento y asignacion de tareas utilizando la metodologia Kanban, en esta se encuentran cuatro columnas 
📋 Backlog → 🚀 Ready → 👥 In Progress → 🔍 Review → ✅ Done

Donde:
-	Backlog (Pendientes o ideas)
Aquí se almacenan todas las tareas que aún no se han empezado.

-	Ready (Preparadas para iniciar)
Son las tareas que ya están listas para comenzar porque tienen toda la información necesaria.
-	In Progress (En progreso)
Aquí se colocan las tareas que ya se están trabajando actualmente.
-	Review (En revisión o prueba)
La tarea está terminada, pero necesita revisión o validación.

![alt text](/Imagenes/image.png)

# Historias de usuarios

Se elaboran historias de usuario que incluyen los criterios mínimos de aceptación, la categoría correspondiente y su tipo de estimación, con el fin de definir claramente el alcance y el esfuerzo requerido para su desarrollo.

![alt text](/Imagenes/image2.png)

La estructura utilizada para la redacción de cada historia es la siguiente:

- Como: [persona o rol] que realiza la acción.

- Quiero: [acción o funcionalidad] que el usuario desea ejecutar.

- Para: [objetivo o beneficio] que se busca alcanzar con dicha acción.

![alt text](/Imagenes/image3.png)
---
