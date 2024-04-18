![HMTL](https://raw.githubusercontent.com/David-Albarracin/README_MATERIALS/main/sql.png)

# Taller SQL # 3

```sql
-- CREAR BASE DE DATOS

CREATE DATABASE garden;
USE garden;

CREATE TABLE garden.products (
	`code_product` INT AUTO_INCREMENT NOT NULL,
	`name_product` VARCHAR(100) NULL,
	`gama_id` VARCHAR(50) NULL,
	`dimensions` VARCHAR(25) NULL,
	`provider` VARCHAR(100) NULL,
	`description` TEXT NULL,
	`stock_amount` SMALLINT NOT NULL,
	`price_sell` DECIMAL(15.2) NOT NULL,
	`price_provider` DOUBLE(15.2) NULL,
	CONSTRAINT products_pk PRIMARY KEY (code_product),
    CONSTRAINT gama_fk FOREIGN KEY (gama_id) REFERENCES gama(gama_id)
);




```

