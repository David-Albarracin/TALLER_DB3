![HMTL](https://raw.githubusercontent.com/David-Albarracin/README_MATERIALS/main/sql.png)

# Taller SQL # 3

#### Diagrama Entidad Relación

![diagrama](https://raw.githubusercontent.com/David-Albarracin/TALLER_DB3/main/garden.png)

#### Comandos DDL y DML

```sql
-- CREAR BASE DE DATOS

CREATE DATABASE garden;
USE garden;
```

```sql
-- TABLA PRODUCTO
/*
* Normalizacion Nª1 (INTEGRIDAD)
* Normalizacion Nª2 (DUPLICIDAD)
* Normalizacion Nª3 (DEPENDENCIAS)
* Normalizacion Nª4 (MULTI DEPENDENCIAS)
*/
CREATE TABLE garden.product (
	`code_product` INT AUTO_INCREMENT NOT NULL,
	`name_product` VARCHAR(100) NULL,
	`gama_id` VARCHAR(50) NULL,
	`dimensions` VARCHAR(25) NULL,
	`provider` VARCHAR(100) NULL,
	`description` TEXT NULL,
	`stock_amount` SMALLINT NOT NULL,
	`price_sell` DECIMAL(15.2) NOT NULL,
	`price_provider` DECIMAL(15.2) NULL,
	CONSTRAINT product_pk PRIMARY KEY (code_product),
    CONSTRAINT gama_fk FOREIGN KEY (gama_id) REFERENCES gama_product(gama_id)
);
```

```sql
-- TABLA GAMA PRODUCTO

/*
* Normalizacion Nª1 (INTEGRIDAD)
* Normalizacion Nª2 (DUPLICIDAD)
* Normalizacion Nª3 (DEPENDENCIAS)
* Normalizacion Nª4 (MULTI DEPENDENCIAS)
*/
CREATE TABLE garden.gama_product (
	`gama_id` VARCHAR(50) NOT NULL,
	`description_txt` TEXT NULL,
	`description_html` TEXT NULL,
	`imagen` VARCHAR(250) NULL,
	CONSTRAINT gama_product_pk PRIMARY KEY (gama_id)
)
```

```sql
-- TABLA DETALLE PEDIDO
/*
* Normalizacion Nª1 (INTEGRIDAD)
* Normalizacion Nª2 (DUPLICIDAD)
* Normalizacion Nª3 (DEPENDENCIAS)
* Normalizacion Nª4 (MULTI DEPENDENCIAS)
*/
CREATE TABLE garden.detail_order (
	`code_order` VARCHAR(50) NOT NULL,
	`code_product` TEXT NULL,
	`description_html` TEXT NULL,
	`imagen varchar`(250) NULL,
	CONSTRAINT gama_product_pk PRIMARY KEY (gama_id)
)
```

```sql
-- TABLA PEDIDO
/*
* Normalizacion Nª1 (INTEGRIDAD)
* Normalizacion Nª2 (DUPLICIDAD)
* Normalizacion Nª3 (DEPENDENCIAS)
* Normalizacion Nª4 (MULTI DEPENDENCIAS)
*/
CREATE TABLE garden.detail_order (
	`code_order` VARCHAR(50) NOT NULL,
	`code_product` TEXT NULL,
	`description_html` TEXT NULL,
	`imagen varchar`(250) NULL,
	CONSTRAINT gama_product_pk PRIMARY KEY (gama_id)
)
```

```sql
-- TABLA CLIENTE
/*
* Normalizacion Nª1 (INTEGRIDAD)
* Normalizacion Nª2 (DUPLICIDAD)
* Normalizacion Nª3 (DEPENDENCIAS)
* Normalizacion Nª4 (MULTI DEPENDENCIAS)
*/
CREATE TABLE garden.detail_order (
	`code_order` VARCHAR(50) NOT NULL,
	`code_product` TEXT NULL,
	`description_html` TEXT NULL,
	`imagen varchar`(250) NULL,
	CONSTRAINT gama_product_pk PRIMARY KEY (gama_id)
)
```

```sql
-- TABLA PAGO
/*
* Normalizacion Nª1 (INTEGRIDAD)
* Normalizacion Nª2 (DUPLICIDAD)
* Normalizacion Nª3 (DEPENDENCIAS)
* Normalizacion Nª4 (MULTI DEPENDENCIAS)
*/
CREATE TABLE garden.detail_order (
	`code_order` VARCHAR(50) NOT NULL,
	`code_product` TEXT NULL,
	`description_html` TEXT NULL,
	`imagen varchar`(250) NULL,
	CONSTRAINT gama_product_pk PRIMARY KEY (gama_id)
)
```

```sql
-- TABLA EMPLEADO
/*
* Normalizacion Nª1 (INTEGRIDAD)
* Normalizacion Nª2 (DUPLICIDAD)
* Normalizacion Nª3 (DEPENDENCIAS)
* Normalizacion Nª4 (MULTI DEPENDENCIAS)
*/
CREATE TABLE garden.detail_order (
	`code_order` VARCHAR(50) NOT NULL,
	`code_product` TEXT NULL,
	`description_html` TEXT NULL,
	`imagen varchar`(250) NULL,
	CONSTRAINT gama_product_pk PRIMARY KEY (gama_id)
)
```

```sql
-- TABLA OFICINA
/*
* Normalizacion Nª1 (INTEGRIDAD)
* Normalizacion Nª2 (DUPLICIDAD)
* Normalizacion Nª3 (DEPENDENCIAS)
* Normalizacion Nª4 (MULTI DEPENDENCIAS)
*/
CREATE TABLE garden.detail_order (
	`code_order` VARCHAR(50) NOT NULL,
	`code_product` TEXT NULL,
	`description_html` TEXT NULL,
	`imagen varchar`(250) NULL,
	CONSTRAINT gama_product_pk PRIMARY KEY (gama_id)
)
```

## NUEVAS TABLAS

```sql
-- TABLA OFICINA
/*
* Normalizacion Nª1 (INTEGRIDAD)
* Normalizacion Nª2 (DUPLICIDAD)
* Normalizacion Nª3 (DEPENDENCIAS)
* Normalizacion Nª4 (MULTI DEPENDENCIAS)
*/
CREATE TABLE garden.detail_order (
	`code_order` VARCHAR(50) NOT NULL,
	`code_product` TEXT NULL,
	`description_html` TEXT NULL,
	`imagen varchar`(250) NULL,
	CONSTRAINT gama_product_pk PRIMARY KEY (gama_id)
)
```







## Consultas sobre una tabla

1. Devuelve un listado con el código de oficina y la ciudad donde hay oficinas.

   ```sql
   
   ```

2. Devuelve un listado con la ciudad y el teléfono de las oficinas de España.

   ```sql
   
   ```

3. Devuelve un listado con el nombre, apellidos y email de los empleados cuyo
   jefe tiene un código de jefe igual a 7.

   ```sql
   
   ```

4. Devuelve el nombre del puesto, nombre, apellidos y email del jefe de la
   empresa.

   ```sql
   
   ```

5. Devuelve un listado con el nombre, apellidos y puesto de aquellos
   empleados que no sean representantes de ventas.

   ```sql
   
   ```

6. Devuelve un listado con el nombre de los todos los clientes españoles.

   ```sql
   
   ```

7. Devuelve un listado con los distintos estados por los que puede pasar un
   pedido.

   ```sql
   
   ```

8. Devuelve un listado con el código de cliente de aquellos clientes que
   realizaron algún pago en 2008. Tenga en cuenta que deberá eliminar
   aquellos códigos de cliente que aparezcan repetidos. Resuelva la consulta:

   ​	• Utilizando la función YEAR de MySQL.
   ​	• Utilizando la función DATE_FORMAT de MySQL.
   ​	• Sin utilizar ninguna de las funciones anteriores.

   ```sql
   
   ```

9. Devuelve un listado con el código de pedido, código de cliente, fecha
   esperada y fecha de entrega de los pedidos que no han sido entregados a
   tiempo.

   ```sql
   
   ```

10. Devuelve un listado con el código de pedido, código de cliente, fecha
    esperada y fecha de entrega de los pedidos cuya fecha de entrega ha sido al
    menos dos días antes de la fecha esperada.

    ​	• Utilizando la función ADDDATE de MySQL.
    ​	• Utilizando la función DATEDIFF de MySQL.
    ​	• ¿Sería posible resolver esta consulta utilizando el operador de suma + o resta -?

    ```sql
    
    ```

11. Devuelve un listado de todos los pedidos que fueron rechazados en 2009.

    ```sql
    
    ```

12. Devuelve un listado de todos los pedidos que han sido entregados en el
    mes de enero de cualquier año.

    ```sql
    
    ```

13. Devuelve un listado con todos los pagos que se realizaron en el
    año 2008 mediante Paypal. Ordene el resultado de mayor a menor.

    ```sql
    
    ```

14. Devuelve un listado con todas las formas de pago que aparecen en la
    tabla pago. Tenga en cuenta que no deben aparecer formas de pago
    repetidas.

    ```sql
    
    ```

15. Devuelve un listado con todos los productos que pertenecen a la
    gama Ornamentales y que tienen más de 100 unidades en stock. El listado
    deberá estar ordenado por su precio de venta, mostrando en primer lugar
    los de mayor precio.

    ```sql
    
    ```

16. Devuelve un listado con todos los clientes que sean de la ciudad de Madrid y
    cuyo representante de ventas tenga el código de empleado 11 o 30.

    ```sql
    
    ```



## Consultas multitabla (Composición interna)

Resuelva todas las consultas utilizando la sintaxis de SQL1 y SQL2. Las consultas con
sintaxis de SQL2 se deben resolver con INNER JOIN y NATURAL JOIN.

1. Obtén un listado con el nombre de cada cliente y el nombre y apellido de su
   representante de ventas.

   ```sql
   
   ```

2. Muestra el nombre de los clientes que hayan realizado pagos junto con el
   nombre de sus representantes de ventas.

   ```sql
   
   ```

3. Muestra el nombre de los clientes que no hayan realizado pagos junto con
   el nombre de sus representantes de ventas.

   ```sql
   
   ```

4. Devuelve el nombre de los clientes que han hecho pagos y el nombre de sus
   representantes junto con la ciudad de la oficina a la que pertenece el
   representante.

   ```sql
   
   ```

5. Devuelve el nombre de los clientes que no hayan hecho pagos y el nombre
   de sus representantes junto con la ciudad de la oficina a la que pertenece el
   representante.

   ```sql
   
   ```

6. Lista la dirección de las oficinas que tengan clientes en Fuenlabrada.

   ```sql
   
   ```

7. Devuelve el nombre de los clientes y el nombre de sus representantes junto
   con la ciudad de la oficina a la que pertenece el representante.

   ```sql
   
   ```

8. Devuelve un listado con el nombre de los empleados junto con el nombre
   de sus jefes.

   ```sql
   
   ```

9. Devuelve un listado que muestre el nombre de cada empleados, el nombre
   de su jefe y el nombre del jefe de sus jefe.

   ```sql
   
   ```

10. Devuelve el nombre de los clientes a los que no se les ha entregado a
    tiempo un pedido.

    ```sql
    
    ```

11. Devuelve un listado de las diferentes gamas de producto que ha comprado
    cada cliente.

    ```sql
    
    ```



## Consultas multitabla (Composición externa)

Resuelva todas las consultas utilizando las cláusulas LEFT JOIN, RIGHT JOIN, NATURAL
LEFT JOIN y NATURAL RIGHT JOIN.

1. Devuelve un listado que muestre solamente los clientes que no han
   realizado ningún pago.

   ```sql
   
   ```

2. Devuelve un listado que muestre solamente los clientes que no han
   realizado ningún pedido.

   ```sql
   
   ```

3. Devuelve un listado que muestre los clientes que no han realizado ningún
   pago y los que no han realizado ningún pedido.

   ```sq
   
   ```

4. Devuelve un listado que muestre solamente los empleados que no tienen
   una oficina asociada.

   ```sql
   
   ```

5. Devuelve un listado que muestre solamente los empleados que no tienen un
   cliente asociado.

   ```sql
   
   ```

6. Devuelve un listado que muestre solamente los empleados que no tienen un
   cliente asociado junto con los datos de la oficina donde trabajan.

   ```sq
   
   ```

7. Devuelve un listado que muestre los empleados que no tienen una oficina
   asociada y los que no tienen un cliente asociado.

   ```sql
   
   ```

8. Devuelve un listado de los productos que nunca han aparecido en un
   pedido.

   ```sql
   
   ```

9. Devuelve un listado de los productos que nunca han aparecido en un
   pedido. El resultado debe mostrar el nombre, la descripción y la imagen del
   producto.

   ```sql
   
   ```

10. Devuelve las oficinas donde no trabajan ninguno de los empleados que
    hayan sido los representantes de ventas de algún cliente que haya realizado
    la compra de algún producto de la gama Frutales.

    ```sql
    
    ```

11. Devuelve un listado con los clientes que han realizado algún pedido pero no
    han realizado ningún pago.

    ```sql
    
    ```

12. Devuelve un listado con los datos de los empleados que no tienen clientes
    asociados y el nombre de su jefe asociado.

    ```sql
    
    ```



## Consultas resumen

1. ¿Cuántos empleados hay en la compañía?

   ```sql
   
   ```

2. ¿Cuántos clientes tiene cada país?

   ```sql
   
   ```

3. ¿Cuál fue el pago medio en 2009?

   ```sql
   
   ```

4. ¿Cuántos pedidos hay en cada estado? Ordena el resultado de forma
   descendente por el número de pedidos.

   ```sql
   
   ```

5. Calcula el precio de venta del producto más caro y más barato en una
   misma consulta.

   ```sql
   
   ```

6. Calcula el número de clientes que tiene la empresa.

   ```sql
   
   ```

7. ¿Cuántos clientes existen con domicilio en la ciudad de Madrid?

   ```sql
   
   ```

8. ¿Calcula cuántos clientes tiene cada una de las ciudades que empiezan
   por M?

   ```sql
   
   ```

9. Devuelve el nombre de los representantes de ventas y el número de clientes
   al que atiende cada uno.

   ```sql
   
   ```

10. Calcula el número de clientes que no tiene asignado representante de
    ventas.

    ```sql
    
    ```

11. Calcula la fecha del primer y último pago realizado por cada uno de los
    clientes. El listado deberá mostrar el nombre y los apellidos de cada cliente.

    ```sql
    
    ```

12. Calcula el número de productos diferentes que hay en cada uno de los
    pedidos.

    ```sql
    
    ```

13. Calcula la suma de la cantidad total de todos los productos que aparecen en
    cada uno de los pedidos.

    ```sql
    
    ```

14. Devuelve un listado de los 20 productos más vendidos y el número total de
    unidades que se han vendido de cada uno. El listado deberá estar ordenado
    por el número total de unidades vendidas.

    ```sql
    
    ```

15. La facturación que ha tenido la empresa en toda la historia, indicando la
    base imponible, el IVA y el total facturado. La base imponible se calcula
    sumando el coste del producto por el número de unidades vendidas de la
    tabla detalle_pedido. El IVA es el 21 % de la base imponible, y el total la
    suma de los dos campos anteriores.

    ```sql
    
    ```

16. La misma información que en la pregunta anterior, pero agrupada por
    código de producto.

    ```sql
    
    ```

17. La misma información que en la pregunta anterior, pero agrupada por
    código de producto filtrada por los códigos que empiecen por OR.

    ```sql
    
    ```

18. Lista las ventas totales de los productos que hayan facturado más de 3000
    euros. Se mostrará el nombre, unidades vendidas, total facturado y total
    facturado con impuestos (21% IVA).

    ```sql
    
    ```

19. Muestre la suma total de todos los pagos que se realizaron para cada uno
    de los años que aparecen en la tabla pagos.

    ```sql
    
    ```



## Consultas variadas

1. Devuelve el listado de clientes indicando el nombre del cliente y cuántos
   pedidos ha realizado. Tenga en cuenta que pueden existir clientes que no
   han realizado ningún pedido.

   ```sql
   
   ```

2. Devuelve un listado con los nombres de los clientes y el total pagado por
   cada uno de ellos. Tenga en cuenta que pueden existir clientes que no han
   realizado ningún pago.

   ```sql
   
   ```

3. Devuelve el nombre de los clientes que hayan hecho pedidos en 2008
   ordenados alfabéticamente de menor a mayor.

   ```sql
   
   ```

4. Devuelve el nombre del cliente, el nombre y primer apellido de su
   representante de ventas y el número de teléfono de la oficina del representante de ventas, de aquellos clientes que no hayan realizado ningún
   pago.

   ```sql
   
   ```

5. Devuelve el listado de clientes donde aparezca el nombre del cliente, el
   nombre y primer apellido de su representante de ventas y la ciudad donde
   está su oficina.

   ```sql
   
   ```

6. Devuelve el nombre, apellidos, puesto y teléfono de la oficina de aquellos
   empleados que no sean representante de ventas de ningún cliente.

   ```sql
   
   ```

7. Devuelve un listado indicando todas las ciudades donde hay oficinas y el
   número de empleados que tiene.
