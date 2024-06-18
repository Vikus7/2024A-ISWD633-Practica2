# Variables de Entorno
### ¿Qué son las variables de entorno?

Las variables de entorno son configuraciones dinámicas que afectan cómo se ejecutan las aplicaciones dentro de contenedores. Estas variables proporcionan información específica al entorno de ejecución sobre aspectos como la configuración del sistema, la conexión a bases de datos, las credenciales de servicios externos, y otros parámetros que pueden variar según el entorno de despliegue (desarrollo, prueba, producción, etc.)

### Para crear un contenedor con variables de entorno

```
docker run -d --name <nombre contenedor> -e <nombre variable1>=<valor1> -e <nombre variable2>=<valor2>
```

### Crear un contenedor a partir de la imagen de nginx:alpine con las siguientes variables de entorno: username y role. Para la variable de entorno rol asignar el valor admin.

```
docker run -d --name nginx-container -e username=<valor_username> -e role=admin nginx:alpine
```

Para comprobar las variables de entorno y en funcion a su definicion podemos acceder al shell y mostrarlo desde adentro:
```
docker exec -it nginx-container /bin/sh
```

Usando el comando

```
env
```

![Variables de entorno](imagenes/variablesDeEntorno.png)

### Crear un contenedor con mysql:8 , mapear todos los puertos
```
docker run -d --name mysql-container -p 3306:3306 -p 33060:33060 mysql:8
```

### ¿El contenedor se está ejecutando?
No esta ejecutando al ingresar la instrucción 
```
docker ps
```

### Identificar el problema
El error se puede identificar accediendo al registro del contenedor, mediante `docker logs mysql-container`
Al ejecurse indica que MySQL se está iniciando, pero no se puede configurar correctamente debido a que no se ha especificado una contraseña para el usuario root de MySQL.

### Eliminar el contenedor creado con mysql:8 
```
docker rm mysql-container
```

### Para crear un contenedor con variables de entorno especificadas
- Portabilidad: Las aplicaciones se vuelven más portátiles y pueden ser desplegadas en diferentes entornos (desarrollo, pruebas, producción) simplemente cambiando el archivo de variables de entorno.
- Centralización: Todas las configuraciones importantes se centralizan en un solo lugar, lo que facilita la gestión y auditoría de las configuraciones.
- Consistencia: Asegura que todos los miembros del equipo de desarrollo o los entornos de despliegue utilicen las mismas configuraciones.
- Evitar Exposición en el Código: Mantener variables sensibles como contraseñas, claves API, y tokens fuera del código fuente reduce el riesgo de exposición accidental a través del control de versiones.
- Control de Acceso: Los archivos de variables de entorno pueden ser gestionados con permisos específicos, limitando quién puede ver o modificar la configuración sensible.

Previo a esto es necesario crear el archivo y colocar las variables en un archivo, **.env** se ha convertido en una convención estándar, pero también es posible usar cualquier extensión como **.txt**.
```
docker run -d --name <nombre contenedor> --env-file=<nombreArchivo>.<extensión> <nombre imagen>
```
**Considerar**
Es necesario especificar la ruta absoluta del archivo si este se encuentra en una ubicación diferente a la que estás ejecutando el comando docker run.

### Crear un contenedor con mysql:8 , mapear todos los puertos y configurar las variables de entorno mediante un archivo

El primer paso sera crear el archivo **.env**:

![Archivo .env](imagenes/envMySql.png)

Ejecutar el contenedor MySQL con docker run:

```
docker run -d --name mysql-container -p 3306:3306 -p 33060:33060 --env-file=./variablesMySQL.env mysql:8
```

![Variables de entorno env MySql Comprobacion](imagenes/envMySqlComprobacion.png)

### ¿Qué bases de datos existen en el contenedor creado?

Una vez dentro del shell del contenedor, puedes iniciar sesión en MySQL como usuario root.

```
mysql -uroot -p
```

Ingresar la password creada en en archivo **.env**
y ejecutar la consulta:

```
SHOW DATABASES;
```

![mySqlDatabases](imagenes/mySqlDatabases/png)
