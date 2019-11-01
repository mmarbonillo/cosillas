## COMANDOS DOCKER

### PARA NUESTRA PRÁCTICA:

Nos vamos a la carpeta donde queramos tener nuestra app en nuestro equipo y ejecutamos el siguiente comando:

    curl -LO https://raw.githubusercontent.com/bitnami/bitnami-docker-laravel/master/docker-compose.yml    

Este comando descarga un archivo .yml que contiene la información necesaria para levantar dos contenedores docker (mariaDB y laravel) con toda la configuración de puertos hecha para que coexistan entre sí. Para levantarlos haremos uso del siguiente comando:

    docker-compose up   

Necesitaremos abrir otra terminal de docker ya que al ejecutar este comando, la consola se queda en ejecución de contenedores.
Ya tenemos nuestro entorno de base de datos y laravel y podemos acceder a este a través de 'localhost:3000'.

- Si queremos detener los contenedores sin eliminarlos: `docker-compose stop`.
- Si queremos detener los contenedores eliminandolos: `docker-compose down`.
- Si queremos arrancar los contenedores parados: `docker-compose start`.


### 1. Docker search

Busca imagenes en los repositorios de contenedores docker (docker hub).

    docker search <imagen>  

### 2. Docker pull

Cuando sepamos que imagen queremos, la descargamos con el comanndo `docker pull`.

    docker pull <imagen>    

### 3. Docker images

Muestra todas las imágenes que tenemos en el sistema.

    docker images   

### 4. Docker ps

Con este comando podemos ver los contenedores que están corriendo. Si le añadimos la opción -a también veremos los contenedores que hemos detenido.

    docker ps   
    docker ps -a    

### 5. Docker start

Reinicia un contenedor detenido. Lo podemos hacer por id o por nombre.

### 6. Docker run

Sirve para lanzar un contenedor.

    docker run [opciones] <imagen> [comando] [parametro]

#### 6.1 Restringir tamaño de memoria

Podemos limitar la memoria que nuestro contenedor va a usar.

    docker run -m "xxxmb" <imagen>  

#### 6.2 Redirección de puertos

Cada servicio tiene un puerto predefinido y el contenedor de docker tendrá abiertos exclusivamente los puertos de los servicios que esté ejecutando.
Podemos asignar un puerto concreto de nuestra máquina al puerto del servicio que se esté ejecutando en docker.

    docker run -p <puerto máquina>:<puerto servicio>    

#### 6.3 Interaccionar con el contenedor

Para poder conectarnos con el contenedor usaremos los parametros `-it`.

Con el parámetro `--name` le asignamos un nombre al contenedor.

Usaremos `--rm` para que, cuando salgamos del contenedor, este se elimine y no se quede consumiendo memoria.

Para ejecutar el contenedor en background usaremos `-d`.

Para no perder todo lo que hagamos en nuestro contenedor necesitaremos mapear el volumen del contenedor a una carpeta de nuestra máquina local con el parámetro `-v`.
Podemos usar una ruta o `$(pwd)` que representa el directorio actual.

Nuestro comando final quedaría algo así:

    docker run -d -it --name laravel -v C:\Users\Bonillo\Documents\DAW\segundo\dwes\laravel:/var/www -p 80:80 eboraas/laravel    

Para no perder nada de lo que hagamos en nuestro contenedor sin maquetar volúmenes basta con ahorrarnos `--rm` y `-v` y acordarnos de hacer `docker stop <id>` a nuestro contenedor para detenerlo.

    docker run -d -it --name laravel -p 80:80 eboraas/laravel   

Ahora que tenemos el contenedor arrancado lo siguiente es: ¿cómo me conecto a él?. Para esto necesitamos hacer uso del comando `exec` que sirve para lanzar comandos a un contenedor activo.

    docker exec -it <nombre-del-contenedor> bash   
 
### Docker rmi

Eliminar imagenes docker

### Docker rm

Eliminar contenedores docker
