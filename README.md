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
/*
* Esta Tabla se Crea para Normalizar la Tabla Direccion
* 
*/
CREATE TABLE `address_type` (
  `address_type_id` int NOT NULL,
  `address_type` varchar(45) NOT NULL,
  PRIMARY KEY (`address_type_id`)
)ENGINE=InnoDB;

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
  PRIMARY KEY (`country_id`),
  UNIQUE KEY `country_unique` (`country_prefix`)
)ENGINE=InnoDB;


-- GAMA DEL PRODUCTO 

CREATE TABLE `gama_product` (
  `gama_product_id` varchar(50) NOT NULL,
  `description_text` text,
  `description_html` text,
  `imagen_url` varchar(250) DEFAULT NULL,
  PRIMARY KEY (`gama_product_id`)
)ENGINE=InnoDB;


-- TIPO DE LEGAL CC, NIT, TI

/*
* Se usa Para Normalizar el tipo de legal id
* Si es Cedula o nit
*/

CREATE TABLE `legal_type` (
  `legal_type_id` int NOT NULL,
  `type` varchar(45) DEFAULT NULL,
  PRIMARY KEY (`legal_type_id`)
)ENGINE=InnoDB;


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
  PRIMARY KEY (`measurement_id`)
)ENGINE=InnoDB;


-- TIPO DE ORDEN COMPRA, VENTA

/*
* Se Normaliza el tipo de orden para saver si es compra o venta
*/
CREATE TABLE `order_type` (
  `order_type_id` int NOT NULL,
  `type_name` varchar(45) DEFAULT NULL,
  PRIMARY KEY (`order_type_id`)
)ENGINE=InnoDB;


-- METODO DE PAGO (EFECTIVO, TARGETA ...)

/*
* Se Normaliza el tipo de metodo de pago
*/
CREATE TABLE `pay_method` (
  `pay_method_id` int NOT NULL,
  `method` varchar(45) DEFAULT NULL,
  PRIMARY KEY (`pay_method_id`)
)ENGINE=InnoDB;


-- ROLES (empleado, cliente, proveedor)
/*
* Se Crea para normalizar la tabla cliente - proveedor -empleado
*/
CREATE TABLE `rol` (
  `rol_id` int NOT NULL,
  `rol_name` varchar(45) DEFAULT NULL,
  PRIMARY KEY (`rol_id`)
)ENGINE=InnoDB;


-- ESTADO DEL PEDIDO (PEDIENTE, ...)
/*
* Se Normaliza el campo estado del producto
*/
CREATE TABLE `order_status` (
  `order_status_id` int NOT NULL,
  `order_status_name` varchar(45) DEFAULT NULL,
  PRIMARY KEY (`order_status_id`)
)ENGINE=InnoDB;


-- PRODUCTO
/*
* Se normaliza los tados de dimensiones y gama
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
  KEY `product_gama_product_FK` (`gama_product_id`)
)ENGINE=InnoDB;


-- REGION

CREATE TABLE `region` (
  `region_id` int NOT NULL,
  `region_name` varchar(45) DEFAULT NULL,
  `country_id` int NOT NULL,
  PRIMARY KEY (`region_id`),
  KEY `fk_region_country1_idx` (`country_id`)
)ENGINE=InnoDB;


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
  KEY `fk_user_meta_legal_type1_idx` (`legal_type_id`)
)ENGINE=InnoDB;


-- CIUDAD

CREATE TABLE `city` (
  `city_id` int NOT NULL,
  `city_name` varchar(45) DEFAULT NULL,
  `postal_code` varchar(45) DEFAULT NULL,
  `region_id` int NOT NULL,
  PRIMARY KEY (`city_id`),
  KEY `fk_city_region1_idx` (`region_id`)
)ENGINE=InnoDB;


-- DIRECCION
/*
* Se normaliza la cciudad y el tipo de direccion
* Envio o Facturacion
*/

CREATE TABLE `address` (
  `address_id` int NOT NULL,
  `address_line_1` varchar(45) DEFAULT NULL,
  `address_line_2` varchar(45) DEFAULT NULL,
  `user_id` int DEFAULT NULL,
  `city_id` int DEFAULT NULL,
  `office_id` int NULL,
  `adreess_type_id` int NOT NULL,
  PRIMARY KEY (`address_id`),
  KEY `fk_adreess_user1_idx` (`user_id`),
  KEY `fk_adreess_city1_idx` (`city_id`),
  KEY `fk_adreess_adreess_type1_idx` (`adreess_type_id`)
)ENGINE=InnoDB;


-- OFFICINAS
/*
* Se normaliza los datos de direccion 
* los datos de telefono
*/
CREATE TABLE `office` (
  `office_id` int NOT NULL,
  `office_name` varchar(45) DEFAULT NULL,
  PRIMARY KEY (`office_id`)
)ENGINE=InnoDB;


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
  KEY `fk_order_order_type1_idx` (`order_type_id`)
)ENGINE=InnoDB;


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
  KEY `fk_detail_order_order1_idx` (`order_id`)
)ENGINE=InnoDB;


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
  KEY `fk_pay_pay_method1_idx` (`pay_method_id`)
)ENGINE=InnoDB;


-- NUMEROS DE TELEFONOS
/*
* Esta Tabla nos ayuda a normalizar la duplicidad
*
*/

CREATE TABLE `phone_number` (
  `phone_number_id` int NOT NULL,
  `phone_number` varchar(45) NOT NULL,
  `phone_prefix` varchar(45) DEFAULT NULL,
  `description` varchar(45) DEFAULT NULL,
  `user_id` int DEFAULT NULL,
  `office_id` int DEFAULT NULL,
  `phone_extension` varchar(50) DEFAULT NULL,
  PRIMARY KEY (`phone_number_id`),
  KEY `fk_phone_number_user1_idx` (`user_id`),
  KEY `fk_phone_number_office1_idx` (`office_id`),
  KEY `phone_number_country_FK` (`phone_prefix`)
)ENGINE=InnoDB;


-- USUARIOS
/*
* Esta Tabla nos ayuda a normalizar la duplicidad
*
*/

CREATE TABLE `users` (
  `user_id` int NOT NULL,
  `created_at` datetime DEFAULT NULL,
  `updated_at` datetime DEFAULT NULL,
  `usermeta_id` int DEFAULT NULL,
  `office_id` int DEFAULT NULL,
  `rol_id` int NOT NULL,
  `employee_id` int NULL,
  `boss_id` int NULL,
  `loan_limit` decimal(15,0) DEFAULT NULL,
  PRIMARY KEY (`user_id`),
  KEY `fk_user_user_meta1_idx` (`usermeta_id`),
  KEY `fk_user_office1_idx` (`office_id`),
  KEY `fk_user_rol1_idx` (`rol_id`),
  KEY `fk_user_boss_idx` (`boss_id`),
  KEY `fk_user_user1_idx` (`employee_id`)
)ENGINE=InnoDB;

-- Constraints para la tabla `address`
ALTER TABLE `address`
ADD CONSTRAINT `fk_adreess_user1` FOREIGN KEY (`user_id`) REFERENCES `users` (`user_id`),
ADD CONSTRAINT `fk_adreess_office1` FOREIGN KEY (`office_id`) REFERENCES `office` (`office_id`),
ADD CONSTRAINT `fk_adreess_city1` FOREIGN KEY (`city_id`) REFERENCES `city` (`city_id`),
ADD CONSTRAINT `fk_adreess_adreess_type1` FOREIGN KEY (`adreess_type_id`) REFERENCES `address_type` (`address_type_id`);


-- Constraints para la tabla `order`
ALTER TABLE `order`
ADD CONSTRAINT `fk_order_user1` FOREIGN KEY (`user_id`) REFERENCES `users` (`user_id`),
ADD CONSTRAINT `fk_order_status1` FOREIGN KEY (`status_id`) REFERENCES `order_status` (`order_status_id`),
ADD CONSTRAINT `fk_order_order_type1` FOREIGN KEY (`order_type_id`) REFERENCES `order_type` (`order_type_id`);

-- Constraints para la tabla `order_detail`
ALTER TABLE `order_detail`
ADD CONSTRAINT `fk_detail_order_order1` FOREIGN KEY (`order_id`) REFERENCES `order` (`order_id`),
ADD CONSTRAINT `fk_detail_order_product1` FOREIGN KEY (`product_id`) REFERENCES `product` (`product_id`);

-- Constraints para la tabla `pay`
ALTER TABLE `pay`
ADD CONSTRAINT `fk_pay_user1` FOREIGN KEY (`user_id`) REFERENCES `users` (`user_id`),
ADD CONSTRAINT `fk_pay_pay_method1` FOREIGN KEY (`pay_method_id`) REFERENCES `pay_method` (`pay_method_id`);

-- Constraints para la tabla `phone_number`
ALTER TABLE `phone_number`
ADD CONSTRAINT `fk_phone_number_office1` FOREIGN KEY (`office_id`) REFERENCES `office` (`office_id`),
ADD CONSTRAINT `fk_phone_number_user1` FOREIGN KEY (`user_id`) REFERENCES `users` (`user_id`),
ADD CONSTRAINT `phone_number_country_FK` FOREIGN KEY (`phone_prefix`) REFERENCES `country` (`country_prefix`);

-- Constraints para la tabla `user`
ALTER TABLE `users`
ADD CONSTRAINT `fk_user_user_meta1` FOREIGN KEY (`usermeta_id`) REFERENCES `user_meta` (`usermeta_id`),
ADD CONSTRAINT `fk_user_office1` FOREIGN KEY (`office_id`) REFERENCES `office` (`office_id`),
ADD CONSTRAINT `fk_user_rol1` FOREIGN KEY (`rol_id`) REFERENCES `rol` (`rol_id`),
ADD CONSTRAINT `fk_user_user1` FOREIGN KEY (`employee_id`) REFERENCES `users` (`user_id`);

-- Constraints para la tabla `product`
ALTER TABLE `product` 
ADD CONSTRAINT product_gama_product_FK FOREIGN KEY (gama_product_id) REFERENCES gama_product(gama_product_id),
ADD CONSTRAINT product_measurement_FK FOREIGN KEY (measurement_id) REFERENCES measurement(measurement_id);

-- Constraints para la tabla `user_meta`
ALTER TABLE `user_meta` ADD CONSTRAINT user_meta_legal_type_FK FOREIGN KEY (legal_type_id) REFERENCES legal_type(legal_type_id);

-- Constraints para la tabla `city`
ALTER TABLE `city` ADD CONSTRAINT city_region_FK FOREIGN KEY (region_id) REFERENCES region(region_id);

-- Constraints para la tabla `region`
ALTER TABLE `region` ADD CONSTRAINT region_country_FK FOREIGN KEY (country_id) REFERENCES country(country_id);

```



## INSERTAR DATOS DE PRUEBA

```sql
-- Insertando datos en la tabla `address_type`
INSERT INTO `address_type` (`address_type_id`, `address_type`)
VALUES 
(1, 'Envío'),
(2, 'Facturación'),
(3, 'Otro'),
(4, 'Trabajo');


-- Insertando datos de prueba en la tabla `country`
-- Insertando datos en la tabla `country`
INSERT INTO `country` (`country_id`, `country_prefix`, `country_name`, `flag_url`)
VALUES 
(1, '+34', 'Spain', 'https://example.com/flag_spain.png'),
(2, '+57', 'Colombia', 'https://example.com/flag_colombia.png'),
(3, '+1', 'United States', 'https://example.com/flag_usa.png'),
(4, '+44', 'United Kingdom', 'https://example.com/flag_uk.png'),
(5, '+49', 'Germany', 'https://example.com/flag_germany.png');

-- Insertando datos de prueba en la tabla `gama_product`
INSERT INTO `gama_product` (`gama_product_id`, `description_text`, `description_html`, `imagen_url`)
VALUES 
('Herbaceas', 'Plantas para jardín decorativas', NULL, NULL),
('Herramientas', 'Herramientas para todo tipo de acción', NULL, NULL),
('Aromáticas', 'Plantas aromáticas', NULL, NULL),
('Frutales', 'Árboles pequeños de producción frutal', NULL, NULL),
('Ornamentales', 'Plantas vistosas para la decoración del jardín', NULL, NULL);


-- Insertando datos de prueba en la tabla `legal_type`
INSERT INTO `legal_type` (`legal_type_id`, `type`)
VALUES 
(1, 'CC'),
(2, 'NIT'),
(3, 'TI');

-- Insertando datos de prueba en la tabla `measurement`
INSERT INTO `measurement` (`measurement_id`, `height`, `width`, `Length`, `weight`)
VALUES 
(1, '10 cm', '5 cm', '20 cm', '500 g'),
(2, '15 cm', '8 cm', '25 cm', '750 g');

-- Insertando datos de prueba en la tabla `order_type`
INSERT INTO `order_type` (`order_type_id`, `type_name`)
VALUES 
(1, 'Compra'),
(2, 'Venta');

-- Insertando datos de prueba en la tabla `pay_method`
INSERT INTO `pay_method` (`pay_method_id`, `method`)
VALUES 
(1, 'Efectivo'),
(2, 'Tarjeta de crédito'),
(3, 'Transferencia bancaria'),
(4, 'PayPal');

-- Insertando datos de prueba en la tabla `rol`
INSERT INTO `rol` (`rol_id`, `rol_name`)
VALUES 
(1, 'Empleado'),
(2, 'Cliente'),
(3, 'Proveedor'),
(4, 'Jefe');

-- Insertando datos de prueba en la tabla `order_status`
INSERT INTO `order_status` (`order_status_id`, `order_status_name`)
VALUES 
(1, 'Pendiente'),
(2, 'En proceso'),
(3, 'Entregado');

-- Insertando datos de prueba en la tabla `region`
INSERT INTO `region` (`region_id`, `region_name`, `country_id`)
VALUES 
(1, 'Madrid', 1),
(2, 'Barcelona', 1);

-- Insertando datos de prueba en la tabla `city`
INSERT INTO `city` (`city_id`, `city_name`, `postal_code`, `region_id`)
VALUES 
(1, 'Madrid', '28001', 1),
(2, 'Barcelona', '08001', 2);

-- Insertando datos de prueba en la tabla `user_meta`
INSERT INTO `user_meta` (`usermeta_id`, `first_name`, `last_names`, `first_surname`, `last_surname`, `email`, `legal_id`, `legal_type_id`)
VALUES 
(1, 'Juan', 'García Pérez', 'García', 'Pérez', 'juan@example.com', '123456789', 1),
(2, 'María', 'López García', 'López', 'García', 'maria@example.com', '987654321', 2),
(3, 'David', 'Jose', 'Castellanos', 'Torres', 'pepe@example.com', '1231551515', 1),
(7, 'Pepe', 'carlos', 'López', 'García', 'pepe@example.com', '98765432131', 1);

-- Insertando datos de prueba en la tabla `product`
INSERT INTO `product` (`product_id`, `product_name`, `description`, `stock_amount`, `price`, `price_provider`, `measurement_id`, `gama_product_id`)
VALUES 
(1, 'Rosa roja', 'Descripción de la rosa roja', 100, 10, 8, 1, 'Herbaceas'),
(2, 'Tulipán blanco', 'Descripción del tulipán blanco', 50, 8, 6, 2, 'Herramientas'),
(3, 'Margarita amarilla', 'Descripción de la margarita amarilla', 80, 12, 10, 1, 'Ornamentales'),
(4, 'Pala grande', 'Descripción de la pala grande', 30, 15, 14, 2, 'Herramientas'),
(5, 'Azada de metal', 'Descripción de la azada de metal', 40, 18, 16, 2, 'Herramientas'),
(6, 'Tomate', 'Descripción del tomate', 120, 2, 1, 1, 'Frutales'),
(7, 'Cebolla', 'Descripción de la cebolla', 90, 1, 1, 1, 'Herbaceas'),
(8, 'Clavel rojo', 'Descripción del clavel rojo', 70, 5, 4, 1, 'Ornamentales'),
(9, 'Llave inglesa', 'Descripción de la llave inglesa', 25, 20, 18, 2, 'Herramientas'),
(10, 'Semillas de lechuga', 'Descripción de las semillas de lechuga', 150, 3, 2, 1, 'Herbaceas'),
(11, 'Mango', 'Descripción del mango', 80, 4, 2, 1, 'Frutales'),
(12, 'Pimiento rojo', 'Descripción del pimiento rojo', 100, 3, 2, 1, 'Frutales'),
(13, 'Martillo', 'Descripción del martillo', 35, 25, 22, 2, 'Herramientas'),
(14, 'Tijeras de podar', 'Descripción de las tijeras de podar', 60, 12, 10, 2, 'Herramientas'),
(15, 'Fresas', 'Descripción de las fresas', 110, 6, 4, 1, 'Frutales'),
(16, 'Cactus', 'Descripción del cactus', 200, 8, 6, 1, 'Ornamentales'),
(17, 'Perro de plástico para jardín', 'Descripción del perro de plástico para jardín', 20, 30, 28, 1, 'Ornamentales'),
(18, 'Escoba de jardín', 'Descripción de la escoba de jardín', 45, 10, 8, 2, 'Herramientas'),
(19, 'Cinta métrica', 'Descripción de la cinta métrica', 55, 8, 6, 2, 'Herramientas'),
(20, 'Planta de lavanda', 'Descripción de la planta de lavanda', 85, 7, 5, 1, 'Aromáticas');

-- Insertando datos de prueba en la tabla `office`
INSERT INTO `office` (`office_id`, `office_name`)
VALUES 
(1, 'Oficina Central'),
(2, 'Oficina de Ventas');

-- Insertando datos de prueba en la tabla `user`
INSERT INTO `users` (`user_id`, `created_at`, `updated_at`, `usermeta_id`, `office_id`, `rol_id`, `employee_id`, `boss_id`, `loan_limit`)
VALUES 
(1, '2024-04-19 12:00:00', '2024-04-19 12:00:00', 1, 2, 1, NULL, 7, 1000),
(2, '2024-04-19 12:00:00', '2024-04-19 12:00:00', 2, 2, 2, NULL, NULL, NULL),
(3, '2024-04-19 12:00:00', '2024-04-19 12:00:00', 3, 2, 2, 1, NULL, 1200),
(7, '2024-04-19 12:00:00', '2024-04-19 12:00:00', 7, 2, 4, NULL, NULL, NULL);

-- Insertando datos de prueba en la tabla `order`
INSERT INTO `order` (`order_id`, `user_id`, `order_date`, `waiting_date`, `deliver_date`, `comments`, `status_id`, `order_type_id`)
VALUES 
(1, 1, '2008-11-10', NULL, NULL, 'Pedido de prueba', 1, 1),
(2, 1, '2008-12-10', NULL, NULL, 'Otro pedido de prueba', 1, 1),
(3, 3, '2009-01-16', NULL, NULL, 'Pedido de prueba', 1, 1),
(4, 3, '2009-02-16', NULL, NULL, 'Otro pedido de prueba', 1, 1),
(5, 3, '2009-02-19', NULL, NULL, 'Pedido de prueba', 1, 1),
(6, 2, '2007-01-08', NULL, NULL, 'Otro pedido de prueba', 1, 1),
(7, 2, '2007-01-08', NULL, NULL, 'Pedido de prueba', 1, 1),
(8, 2, '2007-01-08', NULL, NULL, 'Otro pedido de prueba', 1, 1),
(9, 2, '2007-01-08', NULL, NULL, 'Pedido de prueba', 1, 1),
(10, 3, '2007-01-08', NULL, NULL, 'Otro pedido de prueba', 1, 1),
(11, 3, '2006-01-18', NULL, NULL, 'Pedido de prueba', 1, 1),
(12, 7, '2009-01-13', NULL, NULL, 'Otro pedido de prueba', 1, 1),
(13, 7, '2009-01-06', NULL, NULL, 'Pedido de prueba', 1, 1);

-- Insertando datos de prueba en la tabla `order_detail`
INSERT INTO `order_detail` (`product_id`, `order_id`, `amount`, `unit_price`, `line_number`)
VALUES 
(1, 1, '1', '10', 1),
(2, 2, '2', '8', 1);

-- Insertando datos de prueba en la tabla `pay`
INSERT INTO `pay` (`id_transaction`, `pay_date`, `total`, `user_id`, `pay_method_id`)
VALUES 
('TXN001', '2024-04-19 12:00:00', 10, 1, 1),
('TXN002', '2024-04-19 12:00:00', 10, 1, 4),
('TXN003', '2024-04-18 12:00:00', 16, 2, 4);

-- Insertando datos de prueba en la tabla `phone_number`
INSERT INTO `phone_number` (`phone_number_id`, `phone_number`, `phone_prefix`, `description`, `user_id`, `office_id`, `phone_extension`)
VALUES 
(1, '123456789', '+34', 'Personal', 1, NULL, NULL),
(2, '987654321', '+34', 'Trabajo', 2, 2, NULL);

-- Insertando datos de prueba en la tabla `address`
INSERT INTO `address` (`address_id`, `address_line_1`, `address_line_2`, `user_id`, `office_id`, `city_id`, `adreess_type_id`)
VALUES 
(1, 'Calle Principal 123', 'Piso 2, Puerta B', 1, 1, 1, 1),
(2, 'Avenida Secundaria 456', NULL, 2, 2, 2, 2),
(3, 'Avenida Secundaria 456', NULL, 3, 2, 2, 2),
(4, 'Avenida Secundaria 456', NULL, 7, 2, 2, 2);


```





## Consultas sobre una tabla

1. Devuelve un listado con el código de oficina y la ciudad donde hay oficinas.

   ```sql
   SELECT 
   	o.office_id,
   	c.city_name
   FROM
   	office AS o
   INNER JOIN
   	address AS a ON a.office_id = o.office_id
   INNER JOIN
   	city AS c ON c.city_id = a.city_id
   where
   	a.user_id IS null;
   	
   +-----------+-----------+
   | office_id | city_name |
   +-----------+-----------+
   |         2 | Barcelona |
   +-----------+-----------+
   1 row in set (0.00 sec)
   ```

2. Devuelve un listado con la ciudad y el teléfono de las oficinas de España.

   ```sql
   SELECT 
   	o.office_name,
   	c.city_name,
   	p.phone_number
   FROM
   	office AS o
   INNER JOIN
   	address AS a ON o.office_id = a.office_id
   INNER JOIN
   	city AS c ON a.city_id = c.city_id
   INNER JOIN
   	region AS r ON r.region_id  = c.region_id 
   INNER JOIN
   	country AS co ON r.country_id = co.country_id
   INNER JOIN
   	phone_number AS p ON a.office_id = p.office_id
   WHERE
   	co.country_name = 'Spain';
   	
   +-------------------+-----------+--------------+
   | office_name       | city_name | phone_number |
   +-------------------+-----------+--------------+
   | Oficina de Ventas | Barcelona | 987654321    |
   +-------------------+-----------+--------------+
   1 row in set (0.00 sec)
   
   ```

3. Devuelve un listado con el nombre, apellidos y email de los empleados cuyo
   jefe tiene un código de jefe igual a 7.

   ```sql
   SELECT 
   	um.first_name
   FROM
   	users AS u
   INNER JOIN
   	user_meta AS um ON um.usermeta_id = u.usermeta_id 
   WHERE 
   	u.boss_id = 7;
   
   +------------+
   | first_name |
   +------------+
   | Juan       |
   +------------+
   1 row in set (0.00 sec)
   ```
   
4. Devuelve el nombre del puesto, nombre, apellidos y email del jefe de la
   empresa.

   ```sql
   SELECT 
   	um.first_name,
   	um.first_surname,
   	um.email,
   	r.rol_name 
   FROM
   	users AS u
   INNER JOIN
   	user_meta AS um ON um.usermeta_id = u.usermeta_id 
   INNER JOIN
   	rol AS r ON r.rol_id = u.rol_id
   WHERE
   	r.rol_name = 'Jefe';
   	
   +------------+---------------+------------------+----------+
   | first_name | first_surname | email            | rol_name |
   +------------+---------------+------------------+----------+
   | Pepe       | López         | pepe@example.com | Jefe     |
   +------------+---------------+------------------+----------+
   1 row in set (0.00 sec)
   ```
   
5. Devuelve un listado con el nombre, apellidos y puesto de aquellos
   empleados que no sean representantes de ventas.

   ```sql
   SELECT 
   	um.first_name,
   	um.first_surname,
   	um.email,
   	r.rol_name 
   FROM
   	users AS u
   INNER JOIN
   	user_meta AS um ON um.usermeta_id = u.usermeta_id 
   INNER JOIN
   	rol AS r ON r.rol_id = u.rol_id
   WHERE
   	r.rol_name = 'Empleado'
   	AND u.employee_id NOT IN (SELECT employee_id FROM users WHERE employee_id IS NOT NULL);
   	
   +------------+---------------+------------------+----------+
   | first_name | first_surname | email            | rol_name |
   +------------+---------------+------------------+----------+
   | Juan       | García        | juan@example.com | Empleado |
   +------------+---------------+------------------+----------+
   1 row in set (0.00 sec)
   ```
   
6. Devuelve un listado con el nombre de los todos los clientes españoles.

   ```sql
   SELECT 
       um.first_name,
       um.last_names
   FROM 
       users AS u
   JOIN 
       user_meta AS um ON u.usermeta_id = um.usermeta_id
   JOIN 
       address AS a ON u.user_id = a.user_id
   JOIN 
       city AS c ON a.city_id = c.city_id
   JOIN 
       region AS r ON c.region_id = r.region_id
   JOIN 
       country AS co ON r.country_id = co.country_id
   WHERE 
       u.rol_id = 2
       AND co.country_name = 'Spain';
       
   +------------+--------------+
   | first_name | last_names   |
   +------------+--------------+
   | María      | López García |
   | David      | Jose         |
   +------------+--------------+
   2 rows in set (0.00 sec)
   
   ```

7. Devuelve un listado con los distintos estados por los que puede pasar un
   pedido.

   ```sql
   SELECT
   	os.order_status_name
   FROM
   	order_status AS os;
   	
   +-------------------+
   | order_status_name |
   +-------------------+
   | Pendiente         |
   | En proceso        |
   | Entregado         |
   +-------------------+
   3 rows in set (0.00 sec)
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
