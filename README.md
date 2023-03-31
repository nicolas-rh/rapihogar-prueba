# Rapihogar - Prueba técnica #

## Instalar Docker

* Mac, Windows y Ubuntu: [Docker Desktop](https://www.docker.com/products/docker-desktop)
* [Docker Desktop Documentation](https://docs.docker.com/desktop/)

### Revisar si el puerto 80 está ocupado. 

Debian:
```bash
netstat -tulpn | grep 80
```
Mac:
```bash
sudo lsof -i :80
```
```bash
docker-compose build
docker-compose up
docker-compose ps

    Name                  Command               State                    Ports                  
------------------------------------------------------------------------------------------------
test_nginx_1   /docker-entrypoint.sh ngin ...   Up      0.0.0.0:80->80/tcp,:::80->80/tcp        
test_web_1     python manage.py runserver ...   Up      0.0.0.0:8000->8000/tcp,:::8000->8000/tcp
```

```bash
docker exec test_web_1  python manage.py  makemigrations rapihogar
docker exec test_web_1  python manage.py  migrate
```
### Cargar datos de pruebas
```bash
docker exec test_web_1 python manage.py loaddata rapihogar/fixtures/user.json --app rapihogar.user
docker exec test_web_1 python manage.py loaddata rapihogar/fixtures/company.json --app rapihogar.company
docker exec test_web_1 python manage.py loaddata rapihogar/fixtures/scheme.json --app rapihogar.scheme
docker exec test_web_1 python manage.py loaddata rapihogar/fixtures/pedido.json --app rapihogar.pedido
```

```bash
docker exec -it test_web_1 python manage.py createsuperuser
```
### Run tests ###

```bash
docker exec -it test_web_1 python manage.py test
```
# Tarea a realizar #
Rapihogar necesita cargar las horas trabajadas por los técnicos para poder realizar la liquidación. Se pide:

### 1. Base de datos (Opcional) ###
La base de datos por defecto es SQLite, opcionalmente, añadir una base de datos PostgreSQL corriendo en un contenedor de Docker y conectarla con Django

### 2. Modelo de técnico ###
Crear el modelo para cargar técnicos y crear al menos 5 técnicos

### 3. Django Admin ###
Registrar todos los modelos en el admin

### 4. Comando para generar pedidos ###
Realizar un Comando que genere N pedidos  (N, será el número de pedidos a cargar, que se deberá ingresar)

* N solo puede contener valores entre 1 (inclusive) y 100 (inclusive)
* Seleccionar un Técnico aleatoriamente
* Seleccionar un Cliente  aleatoriamente
* Asigne horas trabajadas entre 1 y 10

### 5. Endpoint listado de técnicos ###
Luego de cargar todos los datos crear un servicio web que liste todos los técnicos y calcule el pago según las horas trabajadas 

Cálculo de Pago según la siguiente tabla:

| Cantidad De Horas | Valor Hora  | Porcentaje de descuento  |
| --------   | -------- | -------- |
|  0-14 | 200 | 15% |
| 15-28 | 250 | 16% |
| 29-47 | 300 | 17% |
|  >48 | 350 | 18% |

	
	Por ejemplo: Trabajador “Larusso Daniel”, Horas trabajadas = 20
		total = (20 * 250) – (20 * 250 * 0.16)
		total = (5.000) – (800)
		total = 4.200
		
El listado completo de técnicos debería ser algo así:

* Nombre completo 
* Horas Trabajadas 
* Total a Cobrar
* Cantidad de pedidos en los que trabajo 

```
 El listado se debe poder filtrar por parte del nombre 
```
### 6. Endpoint informe ###
Luego realizar un servicio que muestre un informe que contenga:

* Monto promedio cobrado por todos los técnicos
* Datos de todos los técnicos que cobraron menos que el promedio
* El último trabajador ingresado que cobró el monto más bajo
* El último trabajador ingresado que cobró el monto más alto

### Nota. ###
Para la implementación de las API's utilizar solo las clases necesarias, no exponer métodos públicos que no se necesitan.

### 7. Endpoint UPDATE (Opcional) ###
Realizar un servicio que permita modificar **solo los pedidos**.

### 8. Tests ###
Crear tests para los servicios nuevos

---
### Requisitos para la entrega (Importante) ###
1 - Crear un repositorio **privado** en Github, subir el código y compartirlo con los emails indicados.

2 - Para la implementación de las API's utilizar solo las clases necesarias, **no exponer métodos públicos que no se necesitan.**