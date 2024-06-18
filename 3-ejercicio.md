### Crear contenedor de Postgres sin que exponga los puertos. Usar la imagen: postgres:11.21-alpine3.17
```
docker run -d --name postgres-container postgres:11.21-alpine3.17
```

### Crear un cliente de postgres. Usar la imagen: dpage/pgadmin4

```
docker run -p 80:80 --name pgadmin-container -e PGADMIN_DEFAULT_EMAIL=myemail@example.com -e PGADMIN_DEFAULT_PASSWORD=mypassword -d dpage/pgadmin4
```

La figura presenta el esquema creado en donde los puertos son:
- a: la interfaz o puerto de comunicación entre el cliente y el servidor PostgreSQL
- b: la interfaz o puerto de comunicación del cliente con PostgreSQL
- c: el puerto expuesto por el contenedor de PostgreSQL

![Imagen](imagenes/esquema-ejercicio3.PNG)

## Desde el cliente
### Acceder desde el cliente al servidor postgres creado.

![login PgAdmin](imagenes/loginPgAdmin.png)

### Crear la base de datos info, y dentro de esa base la tabla personas, con id (serial) y nombre (varchar), agregar un par de registros en la tabla, obligatorio incluir su nombre.

![select PostgreSQL](imagenes/selectPg.png)

## Desde el servidor postgresl
### Acceder al servidor
```
docker exec -it postgres-container psql -U postgres
```
### Conectarse a la base de datos info

Dentro de la sesión interactiva de psql ejectutar:

```
\c info
```


### Realizar un select *from personas

![selectPgDesdeServer](imagenes/selectPgDesdeServer.png)
