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
-- TIPO DE DIRECCION

/*
* Esta Tabla se Crea para Normalizar la Tabla Direccion
* 
*/
CREATE TABLE `address_type` (
  `address_type_id` int NOT NULL,
  `address_type` varchar(45) NOT NULL,
  PRIMARY KEY (`address_type_id`)
);

-- PAIS
/*
* Esta Tabla se Crea para Normalizar la Tabla Direccion
* Se usa para tener mejor control de los paises 
*/

CREATE TABLE `country` (
  `country_id` int NOT NULL,
  `country_prefix` varchar(45) NOT NULL,
  `country_name` varchar(45) NOT NULL,
  `flag_url` varchar(45) DEFAULT NULL,
  CONSTRAINT country_pk PRIMARY KEY (`country_id`),
  UNIQUE KEY `country_unique` (`country_prefix`)
);


-- GAMA DEL PRODUCTO 

CREATE TABLE `gama_product` (
  `gama_product_id` varchar(50) NOT NULL,
  `description_text` text,
  `description_html` text,
  `imagen_url` varchar(250) DEFAULT NULL,
  CONSTRAINT gama_product_pk PRIMARY KEY (`gama_product_id`)
);


-- TIPO DE LEGAL CC, NIT, TI

/*
* Se usa Para Normalizar el tipo de legal id
* Si es Cedula o nit
*/

CREATE TABLE `legal_type` (
  `legal_type_id` int NOT NULL,
  `type` varchar(45) DEFAULT NULL,
  CONSTRAINT legal_type_pk PRIMARY KEY (`legal_type_id`)
);


-- TABLA PARA LAS MEDIDAS DEL PRODUCTO
/*
* Se Normaliza dimensiones la instancia multibalorada a la tabla de las medidas del producto
*/

CREATE TABLE `measurement` (
  `measurement_id` int NOT NULL,
  `height` varchar(45) DEFAULT NULL,
  `width` varchar(45) DEFAULT NULL,
  `Length` varchar(45) DEFAULT NULL,
  `weight` varchar(45) DEFAULT NULL,
   CONSTRAINT measurement_pk PRIMARY KEY (`measurement_id`)
);


-- TIPO DE ORDEN COMPRA, VENTA

/*
* Se Normaliza el tipo de orden para saver si es compra o venta
*/
CREATE TABLE `order_type` (
  `order_type_id` int NOT NULL,
  `type_name` varchar(45) DEFAULT NULL,
  CONSTRAINT order_type_pk PRIMARY KEY (`order_type_id`)
);


-- METODO DE PAGO (EFECTIVO, TARGETA ...)

/*
* Se Normaliza el tipo de metodo de pago
*/
CREATE TABLE `pay_method` (
  `pay_method_id` int NOT NULL,
  `method` varchar(45) DEFAULT NULL,
  CONSTRAINT pay_method_pk PRIMARY KEY (`pay_method_id`)
);


-- ROLES (empleado, cliente, proveedor)
/*
* Se Crea para normalizar la tabla cliente - proveedor -empleado
*/
CREATE TABLE `rol` (
  `rol_id` int NOT NULL,
  `rol_name` varchar(45) DEFAULT NULL,
  CONSTRAINT rol_pk PRIMARY KEY (`rol_id`)
);


-- ESTADO DEL PEDIDO (PEDIENTE, ...)
/*
* Se Normaliza el campo estado del producto
*/
CREATE TABLE `status` (
  `status_id` int NOT NULL,
  `status_name` varchar(45) DEFAULT NULL,
  PRIMARY KEY (`status_id`)
);


-- PRODUCTO PRECIO
/*
* 
*/

CREATE TABLE `product` (
  `product_id` int NOT NULL,
  `product_name` varchar(70) DEFAULT NULL,
  `description` text,
  `stock_amount` smallint DEFAULT NULL,
  `price` decimal(10,0) NOT NULL,
  `price_provider` decimal(10,0) NOT NULL,
  `measurement_id` int NOT NULL,
  `gama_product_id` varchar(50) DEFAULT NULL,
  PRIMARY KEY (`product_id`),
  KEY `fk_product_measurement1_idx` (`measurement_id`),
  KEY `product_gama_product_FK` (`gama_product_id`),
  CONSTRAINT `fk_product_measurement1` FOREIGN KEY (`measurement_id`) REFERENCES `measurement` (`measurement_id`),
  CONSTRAINT `product_gama_product_FK` FOREIGN KEY (`gama_product_id`) REFERENCES `gama_product` (`gama_product_id`)
);


-- REGION

CREATE TABLE `region` (
  `region_id` int NOT NULL,
  `region_name` varchar(45) DEFAULT NULL,
  `country_id` int NOT NULL,
  PRIMARY KEY (`region_id`),
  KEY `fk_region_country1_idx` (`country_id`),
  CONSTRAINT `fk_region_country1` FOREIGN KEY (`country_id`) REFERENCES `country` (`country_id`)
);


-- INFORMACION DEL USUARIO

CREATE TABLE `user_meta` (
  `usermeta_id` int NOT NULL,
  `first_name` varchar(45) DEFAULT NULL,
  `last_names` varchar(250) DEFAULT NULL,
  `first_surname` varchar(45) DEFAULT NULL,
  `last_surname` varchar(45) DEFAULT NULL,
  `email` varchar(45) DEFAULT NULL,
  `legal_id` varchar(45) DEFAULT NULL,
  `legal_type_id` int NOT NULL,
  PRIMARY KEY (`usermeta_id`),
  KEY `fk_user_meta_legal_type1_idx` (`legal_type_id`),
  CONSTRAINT `fk_user_meta_legal_type1` FOREIGN KEY (`legal_type_id`) REFERENCES `legal_type` (`legal_type_id`)
);


-- CIUDAD

CREATE TABLE `city` (
  `city_id` int NOT NULL,
  `city_name` varchar(45) DEFAULT NULL,
  `postal_code` varchar(45) DEFAULT NULL,
  `region_id` int NOT NULL,
  PRIMARY KEY (`city_id`),
  KEY `fk_city_region1_idx` (`region_id`),
  CONSTRAINT `fk_city_region1` FOREIGN KEY (`region_id`) REFERENCES `region` (`region_id`)
);


-- DIRECCION
/*
* Se normaliza la cciudad y el tipo de direccion
* Envio o Facturacion
*/

CREATE TABLE `address` (
  `address_id` int NOT NULL,
  `address_line_1` varchar(45) DEFAULT NULL,
  `address_line_2` varchar(45) DEFAULT NULL,
  `user_id` int NOT NULL,
  `city_id` int NOT NULL,
  `adreess_type_id` int NOT NULL,
  PRIMARY KEY (`address_id`),
  KEY `fk_adreess_user1_idx` (`user_id`),
  KEY `fk_adreess_city1_idx` (`city_id`),
  KEY `fk_adreess_adreess_type1_idx` (`adreess_type_id`),
  CONSTRAINT `fk_adreess_adreess_type1` FOREIGN KEY (`adreess_type_id`) REFERENCES `address_type` (`address_type_id`),
  CONSTRAINT `fk_adreess_city1` FOREIGN KEY (`city_id`) REFERENCES `city` (`city_id`),
  CONSTRAINT `fk_adreess_user1` FOREIGN KEY (`user_id`) REFERENCES `user` (`user_id`)
);


-- OFFICINAS
/*
* Se normaliza los datos de direccion 
* los datos de telefono
*/
CREATE TABLE `office` (
  `office_id` int NOT NULL,
  `address_id` int NOT NULL,
  `office_name` varchar(45) DEFAULT NULL,
  PRIMARY KEY (`office_id`),
  KEY `fk_office_adreess1_idx` (`address_id`),
  CONSTRAINT `fk_office_adreess1` FOREIGN KEY (`address_id`) REFERENCES `address` (`address_id`)
);


-- ORDEN
/*
* Se normaliza el estado de la orden
* Se normaliza los detalles de la orden
*/

CREATE TABLE `order` (
  `order_id` int NOT NULL,
  `user_id` int NOT NULL,
  `order_date` date DEFAULT NULL,
  `waiting_date` date DEFAULT NULL,
  `deliver_date` date DEFAULT NULL,
  `comments` varchar(45) DEFAULT NULL,
  `status_id` int NOT NULL,
  `order_type_id` int NOT NULL,
  PRIMARY KEY (`order_id`),
  KEY `fk_order_user1_idx` (`user_id`),
  KEY `fk_order_status1_idx` (`status_id`),
  KEY `fk_order_order_type1_idx` (`order_type_id`),
  CONSTRAINT `fk_order_order_type1` FOREIGN KEY (`order_type_id`) REFERENCES `order_type` (`order_type_id`),
  CONSTRAINT `fk_order_status1` FOREIGN KEY (`status_id`) REFERENCES `status` (`status_id`),
  CONSTRAINT `fk_order_user1` FOREIGN KEY (`user_id`) REFERENCES `user` (`user_id`)
);


-- ORDEN DETALLES
/*
* Se normaliza los detalles de la orden
*/

CREATE TABLE `order_detail` (
  `product_id` int NOT NULL,
  `order_id` int NOT NULL,
  `amount` varchar(45) DEFAULT NULL,
  `unit_price` varchar(45) DEFAULT NULL,
  `line_number` smallint DEFAULT NULL,
  KEY `fk_detail_order_product1_idx` (`product_id`),
  KEY `fk_detail_order_order1_idx` (`order_id`),
  CONSTRAINT `fk_detail_order_order1` FOREIGN KEY (`order_id`) REFERENCES `order` (`order_id`),
  CONSTRAINT `fk_detail_order_product1` FOREIGN KEY (`product_id`) REFERENCES `product` (`product_id`)
);


-- PAGOS
/*
* Se normaliza los metodos de pago que podria usar el cliente
*
*/

CREATE TABLE `pay` (
  `id_transaction` varchar(250) NOT NULL,
  `pay_date` datetime DEFAULT NULL,
  `total` decimal(15,0) DEFAULT NULL,
  `user_id` int NOT NULL,
  `pay_method_id` int NOT NULL,
  PRIMARY KEY (`id_transaction`),
  KEY `fk_pay_user1_idx` (`user_id`),
  KEY `fk_pay_pay_method1_idx` (`pay_method_id`),
  CONSTRAINT `fk_pay_pay_method1` FOREIGN KEY (`pay_method_id`) REFERENCES `pay_method` (`pay_method_id`),
  CONSTRAINT `fk_pay_user1` FOREIGN KEY (`user_id`) REFERENCES `user` (`user_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;


-- NUMEROS DE TELEFONOS
/*
* Esta Tabla nos ayuda a normalizar la duplicidad
*
*/

CREATE TABLE `phone_number` (
  `phone_number_id` int NOT NULL,
  `phone_number` varchar(45) DEFAULT NULL,
  `phone_prefix` varchar(45) DEFAULT NULL,
  `description` varchar(45) DEFAULT NULL,
  `user_id` int DEFAULT NULL,
  `office_id` int DEFAULT NULL,
  `phone_extension` varchar(50) DEFAULT NULL,
  PRIMARY KEY (`phone_number_id`),
  KEY `fk_phone_number_user1_idx` (`user_id`),
  KEY `fk_phone_number_office1_idx` (`office_id`),
  KEY `phone_number_country_FK` (`phone_prefix`),
  CONSTRAINT `fk_phone_number_office1` FOREIGN KEY (`office_id`) REFERENCES `office` (`office_id`),
  CONSTRAINT `fk_phone_number_user1` FOREIGN KEY (`user_id`) REFERENCES `user` (`user_id`),
  CONSTRAINT `phone_number_country_FK` FOREIGN KEY (`phone_prefix`) REFERENCES `country` (`country_prefix`)
);


-- USUARIOS
/*
* Esta Tabla nos ayuda a normalizar la duplicidad
*
*/

CREATE TABLE `user` (
  `user_id` int NOT NULL,
  `created_at` datetime DEFAULT NULL,
  `updated_at` datetime DEFAULT NULL,
  `usermeta_id` int NOT NULL,
  `office_id` int NOT NULL,
  `rol_id` int NOT NULL,
  `employee_id` int NOT NULL,
  `loan_limit` decimal(15,0) DEFAULT NULL,
  PRIMARY KEY (`user_id`),
  KEY `fk_user_user_meta1_idx` (`usermeta_id`),
  KEY `fk_user_office1_idx` (`office_id`),
  KEY `fk_user_rol1_idx` (`rol_id`),
  KEY `fk_user_user1_idx` (`employee_id`),
  CONSTRAINT `fk_user_office1` FOREIGN KEY (`office_id`) REFERENCES `office` (`office_id`),
  CONSTRAINT `fk_user_rol1` FOREIGN KEY (`rol_id`) REFERENCES `rol` (`rol_id`),
  CONSTRAINT `fk_user_user1` FOREIGN KEY (`employee_id`) REFERENCES `user` (`user_id`),
  CONSTRAINT `fk_user_user_meta1` FOREIGN KEY (`usermeta_id`) REFERENCES `user_meta` (`usermeta_id`)
);
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
