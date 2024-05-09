# bbdJardineria
![image](https://github.com/nataliagiraldo/bbdJardineria/assets/131258170/85593847-5381-443c-abb8-9362df5fb077)

```mysql
DDL
CREATE TABLE IF NOT EXISTS empleado (
    id INT NOT NULL PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    apellido1 VARCHAR(100) NOT NULL,
    apellido2 VARCHAR(100),
    extension INT NOT NULL,
    email VARCHAR(100) NOT NULL,
    jefe INT,
    id_oficina INT,
    id_puesto INT,
    CONSTRAINT FK_id_jefe FOREIGN KEY(jefe) REFERENCES empleado(id),
    CONSTRAINT FK_id_oficina_empleado FOREIGN KEY(id_oficina) REFERENCES oficina(id),
    CONSTRAINT FK_id_puesto FOREIGN KEY(id_puesto) REFERENCES puesto(id) 
);

CREATE TABLE IF NOT EXISTS pais(
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL
);

CREATE TABLE IF NOT EXISTS region(
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    id_pais INT NOT NULL,
    CONSTRAINT FK_id_pais FOREIGN KEY(id_pais) REFERENCES pais(id)
);

CREATE TABLE IF NOT EXISTS ciudad(
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    id_region INT NOT NULL,
    CONSTRAINT FK_id_region FOREIGN KEY(id_region) REFERENCES region(id)
);

CREATE TABLE IF NOT EXISTS direccion(
    id INT AUTO_INCREMENT PRIMARY KEY,
    calle VARCHAR(100),
    carrera VARCHAR(100),
    numero VARCHAR(100),
    postal VARCHAR(10),
    id_ciudad INT NOT NULL,
    CONSTRAINT FK_id_ciudad FOREIGN KEY(id_ciudad) REFERENCES ciudad(id)
);

CREATE TABLE IF NOT EXISTS oficina(
    id INT AUTO_INCREMENT PRIMARY KEY,
    id_direccion INT,
    CONSTRAINT FK_id_direccion_oficina FOREIGN KEY(id_direccion) REFERENCES direccion(id)
);

CREATE TABLE IF NOT EXISTS puesto(
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL
);

CREATE TABLE IF NOT EXISTS cliente(
    id INT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    fax VARCHAR(100) NOT NULL,
    id_direccion INT,
    id_empleado INT,
    CONSTRAINT FK_id_direccion_cliente FOREIGN KEY(id_direccion) REFERENCES direccion(id),
    CONSTRAINT FK_id_empleado FOREIGN KEY(id_empleado) REFERENCES empleado(id)
);

CREATE TABLE IF NOT EXISTS proveedor(
    nit INT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL
);

CREATE TABLE IF NOT EXISTS tipo_telefono(
    id INT AUTO_INCREMENT PRIMARY KEY,
    descripcion VARCHAR(100) NOT NULL
);

CREATE TABLE IF NOT EXISTS telefono(
    id INT AUTO_INCREMENT PRIMARY KEY,
    numero INT NOT NULL,
    prefijo INT NOT NULL,
    id_tipo_telefono INT NOT NULL,
    id_cliente INT NOT NULL,
    CONSTRAINT FK_id_tipo_telefono FOREIGN KEY(id_tipo_telefono) REFERENCES tipo_telefono(id),
    CONSTRAINT FK_id_cliente FOREIGN KEY(id_cliente) REFERENCES cliente(id)
);

CREATE TABLE IF NOT EXISTS tipo_pago(
    id INT AUTO_INCREMENT PRIMARY KEY,
    descripcion VARCHAR(100) NOT NULL
);

CREATE TABLE IF NOT EXISTS pago(
    id INT AUTO_INCREMENT PRIMARY KEY,
    fecha DATE NOT NULL,
    total DOUBLE NOT NULL,
    id_cliente INT NOT NULL,
    id_tipo_pago INT NOT NULL,
    CONSTRAINT FK_id_cliente_pago FOREIGN KEY(id_cliente) REFERENCES cliente(id),
    CONSTRAINT FK_id_tipo_pago_pago FOREIGN KEY(id_tipo_pago) REFERENCES tipo_pago(id)
);

CREATE TABLE IF NOT EXISTS contacto(
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    apellido VARCHAR(100) NOT NULL,
    id_cliente INT,
    CONSTRAINT FK_id_cliente_contacto FOREIGN KEY(id_cliente) REFERENCES cliente(id)
);

CREATE TABLE IF NOT EXISTS estado_pedido(
    id INT AUTO_INCREMENT PRIMARY KEY,
    descripcion VARCHAR(100) NOT NULL
);

CREATE TABLE IF NOT EXISTS gama_producto(
    id INT AUTO_INCREMENT PRIMARY KEY,
    descripcion_txt VARCHAR(200) NOT NULL,
    descripcion_html VARCHAR(200) NOT NULL,
    imagen VARCHAR(100) NOT NULL
);

CREATE TABLE IF NOT EXISTS dimensiones(
    id INT AUTO_INCREMENT PRIMARY KEY,
    largo INT NOT NULL,
    ancho INT NOT NULL,
    alto INT
);

CREATE TABLE IF NOT EXISTS producto(
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    descripcion VARCHAR(200),
    precio_venta DOUBLE NOT NULL,
    precio_compra DOUBLE,
    id_gama_producto INT NOT NULL,
    id_dimensiones INT,
    nit_proveedor INT,
    CONSTRAINT FK_id_gama_producto FOREIGN KEY(id_gama_producto) REFERENCES gama_producto(id),
    CONSTRAINT FK_id_dimensiones FOREIGN KEY(id_dimensiones) REFERENCES dimensiones(id),
    CONSTRAINT FK_nit_proveedor_producto FOREIGN KEY(nit_proveedor) REFERENCES proveedor(nit)
);

CREATE TABLE IF NOT EXISTS inventario(
    id INT AUTO_INCREMENT PRIMARY KEY,
    stock INT NOT NULL,
    id_producto INT NOT NULL,
    CONSTRAINT FK_id_producto_inventario FOREIGN KEY(id_producto) REFERENCES producto(id)
);

CREATE TABLE IF NOT EXISTS pedido(
    id INT AUTO_INCREMENT PRIMARY KEY,
    fecha_pedido DATE NOT NULL,
    fecha_esperada DATE NOT NULL,
    fecha_entrega DATE,
    id_estado_pedido INT NOT NULL,
    id_cliente INT NOT NULL,
    CONSTRAINT FK_id_estado_pedido_pedido FOREIGN KEY(id_estado_pedido) REFERENCES estado_pedido(id),
    CONSTRAINT FK_id_cliente_pedido FOREIGN KEY(id_cliente) REFERENCES cliente(id)
);

CREATE TABLE IF NOT EXISTS detalle_pedido(
    id_producto INT NOT NULL,
    id_pedido INT NOT NULL,
    cantidad INT NOT NULL,
    precio_unidad DOUBLE NOT NULL,
    numero_linea INT NOT NULL,
    PRIMARY KEY(id_producto, id_pedido),
    CONSTRAINT FK_id_producto_detalle_pedido FOREIGN KEY(id_producto) REFERENCES producto(id),
    CONSTRAINT FK_id_pedido_detalle_pedido FOREIGN KEY(id_pedido) REFERENCES pedido(id)
);

CREATE TABLE IF NOT EXISTS comentarios(
    id INT AUTO_INCREMENT PRIMARY KEY,
    id_pedido INT NOT NULL,
    contenido VARCHAR(50),
    CONSTRAINT FK_id_pedido_comentarios FOREIGN KEY(id_pedido) REFERENCES pedido(id)
);

DML

-- Inserts para la tabla pais
INSERT INTO pais (nombre) 
VALUES 
    ('España'), 
    ('Francia'), 
    ('Italia');

-- Inserts para la tabla region
INSERT INTO region (nombre, id_pais) 
VALUES 
    ('Madrid', 1), 
    ('Barcelona', 1), 
    ('París', 2), 
    ('Marsella', 2), 
    ('Roma', 3), 
    ('Milán', 3);

-- Inserts para la tabla ciudad
INSERT INTO ciudad (nombre, id_region) 
VALUES 
    ('Madrid', 1), 
    ('Barcelona', 2), 
    ('París', 3), 
    ('Marsella', 4), 
    ('Roma', 5), 
    ('Milán', 6);

-- Inserts para la tabla direccion
INSERT INTO direccion (calle, carrera, numero, postal, id_ciudad) 
VALUES 
    ('Calle Mayor', 'Avinguda Diagonal', '123', '28001', 1), 
    ('Calle de Paris', 'Rue de Rivoli', '45', '75001', 3), 
    ('Via Roma', 'Corso Buenos Aires', '10', '00100', 5);

-- Inserts para la tabla oficina
INSERT INTO oficina (id_direccion) 
VALUES 
    (1), 
    (2), 
    (3);

-- Inserts para la tabla empleado
INSERT INTO empleado (id, nombre, apellido1, apellido2, extension, email, jefe, id_oficina) 
VALUES 
    (1, 'Juan', 'Pérez', 'García', 101, 'juan.perez@example.com', NULL, 1), 
    (2, 'María', 'López', 'Martínez', 102, 'maria.lopez@example.com', 1, 2), 
    (3, 'Carlos', 'González', 'Fernández', 103, 'carlos.gonzalez@example.com', 1, 1);

-- Inserts para la tabla puesto
INSERT INTO puesto (nombre) 
VALUES 
    ('Gerente'), 
    ('Asistente'), 
    ('Recepcionista');

-- Inserts para la tabla cliente
INSERT INTO cliente (id, nombre, fax, id_direccion, id_empleado) 
VALUES 
    (1, 'Cliente 1', '123456789', 1, 2), 
    (2, 'Cliente 2', '987654321', 2, 3), 
    (3, 'Cliente 3', '555555555', 3, 1);

-- Inserts para la tabla proveedor
INSERT INTO proveedor (nit, nombre) 
VALUES 
    (123456789, 'Proveedor 1'), 
    (987654321, 'Proveedor 2');

-- Inserts para la tabla tipo_telefono
INSERT INTO tipo_telefono (descripcion) 
VALUES 
    ('Móvil'), 
    ('Fijo');

-- Inserts para la tabla telefono
INSERT INTO telefono (numero, prefijo, id_tipo_telefono, id_cliente) 
VALUES 
    (666666666, 34, 1, 1), 
    (555555555, 34, 2, 2), 
    (444444444, 34, 1, 3);

-- Inserts para la tabla tipo_pago
INSERT INTO tipo_pago (descripcion) 
VALUES 
    ('Tarjeta de crédito'), 
    ('Transferencia bancaria');

-- Inserts para la tabla pago
INSERT INTO pago (fecha, total, id_cliente, id_tipo_pago) 
VALUES 
    ('2023-01-15', 100.50, 1, 1), 
    ('2023-02-20', 200.75, 2, 2), 
    ('2023-03-10', 150.25, 3, 1);

-- Inserts para la tabla contacto
INSERT INTO contacto (nombre, apellido, id_cliente) 
VALUES 
    ('Contacto 1', 'Apellido 1', 1), 
    ('Contacto 2', 'Apellido 2', 2), 
    ('Contacto 3', 'Apellido 3', 3);

-- Inserts para la tabla estado_pedido
INSERT INTO estado_pedido (descripcion) 
VALUES 
    ('En proceso'), 
    ('Entregado'), 
    ('Cancelado');

-- Inserts para la tabla gama_producto
INSERT INTO gama_producto (descripcion_txt, descripcion_html, imagen) 
VALUES 
    ('Electrodomésticos', 'Electrodomésticos', 'electrodomesticos.jpg'), 
    ('Muebles', 'Muebles', 'muebles.jpg'), 
    ('Electrónica', 'Electrónica', 'electronica.jpg');

-- Inserts para la tabla dimensiones
INSERT INTO dimensiones (largo, ancho, alto) 
VALUES 
    (100, 50, 70), 
    (80, 40, 60), 
    (120, 60, 80);

-- Inserts para la tabla producto
INSERT INTO producto (nombre, descripcion, precio_venta, precio_compra, id_gama_producto, id_dimensiones, nit_proveedor) 
VALUES 
    ('Lavadora', 'Lavadora de carga frontal', 500.00, 400.00, 1, 1, 123456789), 
    ('Sofá', 'Sofá de tres plazas', 600.00, 450.00, 2, 2, 987654321), 
    ('Smartphone', 'Teléfono inteligente', 700.00, 550.00, 3, 3, 123456789);

-- Inserts para la tabla inventario
INSERT INTO inventario (stock, id_producto) 
VALUES 
    (50, 1), 
    (30, 2), 
    (20, 3);

-- Inserts para la tabla pedido
INSERT INTO pedido (fecha_pedido, fecha_esperada, fecha_entrega, id_estado_pedido, id_cliente) 
VALUES 
    ('2023-01-01', '2023-01-10', NULL, 1, 1), 
    ('2023-02-01', '2023-02-10', NULL, 1, 2), 
    ('2023-03-01', '2023-03-10', '2023-03-05', 2, 3);

-- Inserts para la tabla detalle_pedido
INSERT INTO detalle_pedido (id_producto, id_pedido, cantidad, precio_unidad, numero_linea) 
VALUES 
    (1, 1, 2, 250.00, 1), 
    (2, 2, 1, 600.00, 1), 
    (3, 3, 3, 700.00, 1);

-- Inserts para la tabla comentarios
INSERT INTO comentarios (id_pedido, contenido) 
VALUES 
    (1, 'Pedido en espera'), 
    (3, 'Pedido entregado con éxito');
    
    
Consultas sobre una tabla

1. Devuelve un listado con el código de oficina y la ciudad donde hay oficinas.

mysql> SELECT oficina.id, ciudad.nombre 
    -> FROM oficina
    -> INNER JOIN direccion ON oficina.id_direccion = direccion.id
    -> INNER JOIN ciudad ON direccion.id_ciudad = ciudad.id;
+----+--------+
| id | nombre |
+----+--------+
|  1 | Madrid |
|  2 | París  |
|  3 | Roma   |
+----+--------+

2. Devuelve un listado con la ciudad y el teléfono de las oficinas de España.

mysql>  SELECT oficina.id, ciudad.nombre, oficina.telefono
    -> FROM oficina
    ->     INNER JOIN direccion ON oficina.id_direccion = direccion.id
    ->     INNER JOIN ciudad ON direccion.id_ciudad = ciudad.id
    -> INNER JOIN region ON ciudad.id_region = region.id
    -> INNER JOIN pais ON region.id_pais = pais.id
    -> WHERE pais.nombre = 'España';
+----+--------+----------+
| id | nombre | telefono |
+----+--------+----------+
|  1 | Madrid |     NULL |
+----+--------+----------+

3. Devuelve un listado con el nombre, apellidos y email de los empleados cuyo
   jefe tiene un código de jefe igual a 7.

mysql> SELECT e.nombre, e.apellido1, e.apellido2, e.email 
    -> FROM empleado AS e
    -> WHERE jefe = 7;
+--------+-----------+------------+-----------------------------+
| nombre | apellido1 | apellido2  | email                       |
+--------+-----------+------------+-----------------------------+
| María  | López     | Martínez   | maria.lopez@example.com     |
| Carlos | González  | Fernández  | carlos.gonzalez@example.com |
+--------+-----------+------------+-----------------------------+

4. Devuelve el nombre del puesto, nombre, apellidos y email del jefe de la
   empresa.

mysql> SELECT puesto.nombre AS puesto, empleado.nombre, empleado.apellido1, empleado.apellido2, empleado.email 
    -> FROM empleado
    -> JOIN puesto ON empleado.id = puesto.id
    -> WHERE empleado.jefe IS NULL;
+---------+--------+-----------+-----------+------------------------+
| puesto  | nombre | apellido1 | apellido2 | email                  |
+---------+--------+-----------+-----------+------------------------+
| Gerente | Juan   | Pérez     | García    | juan.perez@example.com |
+---------+--------+-----------+-----------+------------------------+

5. Devuelve un listado con el nombre, apellidos y puesto de aquellos
   empleados que no sean representantes de ventas.

mysql> SELECT puesto.nombre AS puesto, empleado.nombre, empleado.apellido1, empleado.apellido2
    -> FROM empleado
    -> JOIN puesto ON empleado.id = puesto.id
    -> WHERE puesto.nombre NOT IN ('representante de ventas');
+---------------+--------+-----------+------------+
| puesto        | nombre | apellido1 | apellido2  |
+---------------+--------+-----------+------------+
| Gerente       | Juan   | Pérez     | García     |
| Asistente     | María  | López     | Martínez   |
| Recepcionista | Carlos | González  | Fernández  |
+---------------+--------+-----------+------------+

6. Devuelve un listado con el nombre de los todos los clientes españoles.
   mysql> SELECT cliente.nombre
    -> FROM cliente 
    -> INNER JOIN direccion ON cliente.id_direccion = direccion.id
    -> INNER JOIN ciudad ON direccion.id_ciudad = ciudad.id
    -> INNER JOIN region ON ciudad.id_region = region.id
    -> INNER JOIN pais ON region.id_pais = pais.id
    -> WHERE pais.nombre = 'España';
   +-----------+
   | nombre    |
   +-----------+
   | Cliente 1 |
   +-----------+

7. Devuelve un listado con los distintos estados por los que puede pasar un
   pedido.

mysql> SELECT descripcion
    -> FROM estado_pedido;
+-------------+
| descripcion |
+-------------+
| En proceso  |
| Entregado   |
| Cancelado   |
+-------------+


8. Devuelve un listado con el código de cliente de aquellos clientes que
   realizaron algún pago en 2008. Tenga en cuenta que deberá eliminar
   aquellos códigos de cliente que aparezcan repetidos. Resuelva la consulta:
   • Utilizando la función YEAR de MySQL.
   • Utilizando la función DATE_FORMAT de MySQL.
   • Sin utilizar ninguna de las funciones anteriores.

mysql> SELECT DISTINCT cliente.id
    -> FROM cliente
    -> INNER JOIN pago ON cliente.id = pago.id_cliente
    -> WHERE YEAR(pago.fecha) = 2008;
Empty set (0.01 sec)

mysql> 
mysql> SELECT DISTINCT cliente.id
    -> FROM cliente
    -> INNER JOIN pago ON cliente.id = pago.id_cliente
    -> WHERE DATE_FORMAT(pago.fecha, '%Y') = '2008';
Empty set (0.00 sec)

mysql> SELECT DISTINCT cliente.id
    -> FROM cliente
    -> INNER JOIN pago ON cliente.id = pago.id_cliente
    -> WHERE pago.fecha >= '2008-01-01' AND pago.fecha <= '2008-12-31';
Empty set (0.00 sec)

9. Devuelve un listado con el código de pedido, código de cliente, fecha
   esperada y fecha de entrega de los pedidos que no han sido entregados a
   tiempo.
   mysql> SELECT id AS 'Código de Pedido', id_cliente AS 'Código de Cliente', fecha_esperada AS 'Fecha Esperada', fecha_entrega AS 'Fecha de Entrega'
    -> FROM pedido
    -> WHERE fecha_entrega > fecha_esperada;
   Empty set (0.06 sec)

10. Devuelve un listado con el código de pedido, código de cliente, fecha
    esperada y fecha de entrega de los pedidos cuya fecha de entrega ha sido al
    menos dos días antes de la fecha esperada.
    • Utilizando la función ADDDATE de MySQL.
    • Utilizando la función DATEDIFF de MySQL.
    • ¿Sería posible resolver esta consulta utilizando el operador de suma + o
    resta -?

SELECT id AS 'Código de Pedido', id_cliente AS 'Código de Cliente', fecha_esperada AS 'Fecha Esperada', fecha_entrega AS 'Fecha de Entrega'
FROM pedido
WHERE fecha_entrega < ADDDATE(fecha_esperada, -2);

SELECT id AS 'Código de Pedido', id_cliente AS 'Código de Cliente', fecha_esperada AS 'Fecha Esperada', fecha_entrega AS 'Fecha de Entrega'
FROM pedido
WHERE DATEDIFF(fecha_esperada, fecha_entrega) >= 2;

SELECT id AS 'Código de Pedido', id_cliente AS 'Código de Cliente', fecha_esperada AS 'Fecha Esperada', fecha_entrega AS 'Fecha de Entrega'
FROM pedido
WHERE fecha_esperada - fecha_entrega >= INTERVAL 2 DAY;


11. Devuelve un listado de todos los pedidos que fueron rechazados en 2009.

SELECT pedido.id AS 'Código de Pedido', pedido.id_cliente AS 'Código de Cliente', pedido.fecha_pedido AS 'Fecha de Pedido', pedido.fecha_entrega AS 'Fecha de Entrega'
FROM pedido
JOIN estado_pedido ON pedido.id_estado_pedido = estado_pedido.id
WHERE estado_pedido.descripcion = 'rechazado' AND YEAR(pedido.fecha_pedido) = 2009;

12. Devuelve un listado de todos los pedidos que han sido entregados en el
    mes de enero de cualquier año.

SELECT id AS 'Código de Pedido', id_cliente AS 'Código de Cliente', fecha_esperada AS 'Fecha Esperada', fecha_entrega AS 'Fecha de Entrega'
FROM pedido
WHERE MONTH(fecha_entrega) = 1;


13. Devuelve un listado con todos los pagos que se realizaron en el
    año 2008 mediante Paypal. Ordene el resultado de mayor a menor.

SELECT *
FROM pago
WHERE YEAR(fecha) = 2008 AND id_tipo_pago = (
    SELECT id
    FROM tipo_pago
    WHERE descripcion = 'Paypal'
)
ORDER BY total DESC;

14. Devuelve un listado con todas las formas de pago que aparecen en la
    tabla pago. Tenga en cuenta que no deben aparecer formas de pago
    repetidas.

mysql> SELECT DISTINCT descripcion
    -> FROM tipo_pago
    -> INNER JOIN pago ON tipo_pago.id = pago.id_tipo_pago
    -> ;
+------------------------+
| descripcion            |
+------------------------+
| Tarjeta de crédito     |
| Transferencia bancaria |
+------------------------+


15. Devuelve un listado con todos los productos que pertenecen a la
    gama Ornamentales y que tienen más de 100 unidades en stock. El listado
    deberá estar ordenado por su precio de venta, mostrando en primer lugar
    los de mayor precio.

SELECT producto.id, producto.nombre, producto.precio_venta, inventario.stock
FROM producto
INNER JOIN inventario ON producto.id = inventario.id_producto
INNER JOIN gama_producto ON producto.id_gama_producto = gama_producto.id
WHERE gama_producto.descripcion_txt = 'Ornamentales' AND inventario.stock > 100
ORDER BY producto.precio_venta DESC;


16. Devuelve un listado con todos los clientes que sean de la ciudad de Madrid y
    cuyo representante de ventas tenga el código de empleado 11 o 30.


mysql> SELECT cliente.id, cliente.nombre
    -> FROM cliente
    -> INNER JOIN direccion ON cliente.id_direccion = direccion.id
    -> INNER JOIN ciudad ON direccion.id_ciudad = ciudad.id
    -> INNER JOIN empleado ON cliente.id_empleado = empleado.id
    -> WHERE ciudad.nombre = 'Madrid' AND (empleado.id = 11 OR empleado.id = 30);
+----+-----------+
| id | nombre    |
+----+-----------+
|  1 | Cliente 1 |
+----+-----------+

Consultas multitabla (Composición interna)
Resuelva todas las consultas utilizando la sintaxis de SQL1 y SQL2. Las consultas con
sintaxis de SQL2 se deben resolver con INNER JOIN y NATURAL JOIN.

1. Obtén un listado con el nombre de cada cliente y el nombre y apellido de su
   representante de ventas.

mysql> SELECT c.nombre AS 'Nombre Cliente', e.nombre AS 'Nombre Representante', e.apellido1 AS 'Apellido Representante'
    -> FROM cliente c
    -> JOIN empleado r ON c.id_empleado = r.id
    -> JOIN empleado e ON r.jefe = e.id;
+----------------+----------------------+------------------------+
| Nombre Cliente | Nombre Representante | Apellido Representante |
+----------------+----------------------+------------------------+
| Cliente 1      | Juan                 | Pérez                  |
| Cliente 2      | Juan                 | Pérez                  |
+----------------+----------------------+------------------------+

2. Muestra el nombre de los clientes que hayan realizado pagos junto con el
   nombre de sus representantes de ventas.

mysql> SELECT DISTINCT c.nombre AS 'Nombre Cliente', e.nombre AS 'Nombre Representante'
    -> FROM cliente c
    -> JOIN pago p ON c.id = p.id_cliente
    -> JOIN empleado r ON c.id_empleado = r.id
    -> JOIN empleado e ON r.jefe = e.id;
+----------------+----------------------+
| Nombre Cliente | Nombre Representante |
+----------------+----------------------+
| Cliente 1      | Juan                 |
| Cliente 2      | Juan                 |
+----------------+----------------------+

3. Muestra el nombre de los clientes que no hayan realizado pagos junto con
   el nombre de sus representantes de ventas.

SELECT c.nombre AS 'Nombre Cliente', e.nombre AS 'Nombre Representante'
FROM cliente c
LEFT JOIN pago p ON c.id = p.id_cliente
JOIN empleado r ON c.id_empleado = r.id
JOIN empleado e ON r.jefe = e.id
WHERE p.id IS NULL;

4. Devuelve el nombre de los clientes que han hecho pagos y el nombre de sus
   representantes junto con la ciudad de la oficina a la que pertenece el
   representante.

mysql> SELECT c.nombre AS 'Nombre Cliente', e.nombre AS 'Nombre Representante', ciu.nombre AS 'Ciudad Representante'
    -> FROM cliente c
    -> JOIN pago p ON c.id = p.id_cliente
    -> JOIN empleado r ON c.id_empleado = r.id
    -> JOIN empleado e ON r.jefe = e.id
    -> JOIN oficina o ON e.id_oficina = o.id
    -> JOIN direccion d ON o.id_direccion = d.id
    -> JOIN ciudad ciu ON d.id_ciudad = ciu.id;
+----------------+----------------------+----------------------+
| Nombre Cliente | Nombre Representante | Ciudad Representante |
+----------------+----------------------+----------------------+
| Cliente 1      | Juan                 | Madrid               |
| Cliente 2      | Juan                 | Madrid               |
+----------------+----------------------+----------------------+

5. Devuelve el nombre de los clientes que no hayan hecho pagos y el nombre
   de sus representantes junto con la ciudad de la oficina a la que pertenece el
   representante.

SELECT c.nombre AS 'Nombre Cliente', e.nombre AS 'Nombre Representante', ciu.nombre AS 'Ciudad Representante'
FROM cliente c
LEFT JOIN pago p ON c.id = p.id_cliente
JOIN empleado r ON c.id_empleado = r.id
JOIN empleado e ON r.jefe = e.id
JOIN oficina o ON e.id_oficina = o.id
JOIN direccion d ON o.id_direccion = d.id
JOIN ciudad ciu ON d.id_ciudad = ciu.id
WHERE p.id IS NULL;

6. Lista la dirección de las oficinas que tengan clientes en Fuenlabrada.

SELECT d.calle, d.carrera, d.numero, d.postal
FROM direccion d
JOIN ciudad c ON d.id_ciudad = c.id
JOIN cliente cl ON d.id = cl.id_direccion
WHERE c.nombre = 'Fuenlabrada';

7. Devuelve el nombre de los clientes y el nombre de sus representantes junto
   con la ciudad de la oficina a la que pertenece el representante.

mysql> SELECT c.nombre AS 'Nombre Cliente', e.nombre AS 'Nombre Representante', ciu.nombre AS 'Ciudad Representante'
    -> FROM cliente c
    -> JOIN empleado r ON c.id_empleado = r.id
    -> JOIN empleado e ON r.jefe = e.id
    -> JOIN oficina o ON e.id_oficina = o.id
    -> JOIN direccion d ON o.id_direccion = d.id
    -> JOIN ciudad ciu ON d.id_ciudad = ciu.id;
+----------------+----------------------+----------------------+
| Nombre Cliente | Nombre Representante | Ciudad Representante |
+----------------+----------------------+----------------------+
| Cliente 1      | Juan                 | Madrid               |
| Cliente 2      | Juan                 | Madrid               |
+----------------+----------------------+----------------------+

8. Devuelve un listado con el nombre de los empleados junto con el nombre
   de sus jefes.

mysql> SELECT e1.nombre AS 'Nombre Empleado', e2.nombre AS 'Nombre Jefe'
    -> FROM empleado e1
    -> LEFT JOIN empleado e2 ON e1.jefe = e2.id;
+-----------------+-------------+
| Nombre Empleado | Nombre Jefe |
+-----------------+-------------+
| Juan            | NULL        |
| María           | Juan        |
| Carlos          | Juan        |
+-----------------+-------------+

9. Devuelve un listado que muestre el nombre de cada empleados, el nombre
   de su jefe y el nombre del jefe de sus jefe.

mysql> SELECT e1.nombre AS 'Nombre Empleado', e2.nombre AS 'Nombre Jefe', e3.nombre AS 'Nombre Jefe del Jefe'
    -> FROM empleado e1
    -> LEFT JOIN empleado e2 ON e1.jefe = e2.id
    -> LEFT JOIN empleado e3 ON e2.jefe = e3.id;
+-----------------+-------------+----------------------+
| Nombre Empleado | Nombre Jefe | Nombre Jefe del Jefe |
+-----------------+-------------+----------------------+
| Juan            | NULL        | NULL                 |
| María           | Juan        | NULL                 |
| Carlos          | Juan        | NULL                 |
+-----------------+-------------+----------------------+

10. Devuelve el nombre de los clientes a los que no se les ha entregado a
    tiempo un pedido.


mysql> SELECT DISTINCT c.nombre AS 'Nombre Cliente'
    -> FROM cliente c
    -> JOIN pedido p ON c.id = p.id_cliente
    -> JOIN estado_pedido ep ON p.id_estado_pedido = ep.id
    -> WHERE ep.descripcion != 'Entregado' OR (ep.descripcion = 'Entregado' AND p.fecha_entrega > p.fecha_esperada);
+----------------+
| Nombre Cliente |
+----------------+
| Cliente 1      |
| Cliente 2      |
+----------------+


11. Devuelve un listado de las diferentes gamas de producto que ha comprado
    cada cliente.

mysql> SELECT c.nombre AS 'Nombre Cliente', GROUP_CONCAT(DISTINCT gp.descripcion_txt SEPARATOR ', ') AS 'Gamas de Producto Compradas'
    -> FROM cliente c
    -> JOIN pedido p ON c.id = p.id_cliente
    -> JOIN detalle_pedido dp ON p.id = dp.id_pedido
    -> JOIN producto prod ON dp.id_producto = prod.id
    -> JOIN gama_producto gp ON prod.id_gama_producto = gp.id
    -> GROUP BY c.id;
+----------------+-----------------------------+
| Nombre Cliente | Gamas de Producto Compradas |
+----------------+-----------------------------+
| Cliente 1      | Electrodomésticos           |
| Cliente 2      | Muebles                     |
| Cliente 3      | Electrónica                 |
+----------------+-----------------------------+
Consultas multitabla (Composición externa)
Resuelva todas las consultas utilizando las cláusulas LEFT JOIN, RIGHT JOIN, NATURAL
LEFT JOIN y NATURAL RIGHT JOIN.

1. Devuelve un listado que muestre solamente los clientes que no han
   realizado ningún pago.

SELECT c.nombre AS 'Nombre Cliente'
FROM cliente c
LEFT JOIN pago p ON c.id = p.id_cliente
WHERE p.id IS NULL;

2. Devuelve un listado que muestre solamente los clientes que no han
   realizado ningún pedido.

SELECT c.nombre AS 'Nombre Cliente'
FROM cliente c
LEFT JOIN pedido p ON c.id = p.id_cliente
WHERE p.id IS NULL;

3. Devuelve un listado que muestre los clientes que no han realizado ningún
   pago y los que no han realizado ningún pedido.


SELECT c.nombre AS 'Nombre Cliente', 'Sin Pagos' AS 'Estado'
FROM cliente c
LEFT JOIN pago p ON c.id = p.id_cliente
WHERE p.id IS NULL

UNION

SELECT c.nombre AS 'Nombre Cliente', 'Sin Pedidos' AS 'Estado'
FROM cliente c
LEFT JOIN pedido pd ON c.id = pd.id_cliente
WHERE pd.id IS NULL;

4. Devuelve un listado que muestre solamente los empleados que no tienen
   una oficina asociada.

SELECT nombre AS 'Nombre Empleado'
FROM empleado
WHERE id_oficina IS NULL;

5. Devuelve un listado que muestre solamente los empleados que no tienen un
   cliente asociado.

SELECT nombre AS 'Nombre Empleado'
FROM empleado
WHERE id NOT IN (SELECT id_empleado FROM cliente);

6. Devuelve un listado que muestre solamente los empleados que no tienen un
   cliente asociado junto con los datos de la oficina donde trabajan.

SELECT e.nombre AS 'Nombre Empleado', o.*
FROM empleado e
JOIN oficina o ON e.id_oficina = o.id
WHERE e.id NOT IN (SELECT id_empleado FROM cliente);

7. Devuelve un listado que muestre los empleados que no tienen una oficina
   asociada y los que no tienen un cliente asociado.


SELECT nombre AS 'Nombre Empleado', 'Sin Oficina' AS 'Estado'
FROM empleado
WHERE id_oficina IS NULL

UNION


SELECT nombre AS 'Nombre Empleado', 'Sin Cliente' AS 'Estado'
FROM empleado
WHERE id NOT IN (SELECT id_empleado FROM cliente);

8. Devuelve un listado de los productos que nunca han aparecido en un
   pedido.

SELECT *
FROM producto
WHERE id NOT IN (SELECT id_producto FROM detalle_pedido);

9. Devuelve un listado de los productos que nunca han aparecido en un
   pedido. El resultado debe mostrar el nombre, la descripción y la imagen del
   producto.

SELECT nombre, descripcion, imagen
FROM producto
WHERE id NOT IN (SELECT id_producto FROM detalle_pedido);


10. Devuelve las oficinas donde no trabajan ninguno de los empleados que
    hayan sido los representantes de ventas de algún cliente que haya realizado
    la compra de algún producto de la gama Frutales.

mysql> SELECT DISTINCT o.*
    -> FROM oficina o
    -> LEFT JOIN empleado e ON o.id = e.id_oficina
    -> WHERE e.id IS NULL OR e.id NOT IN (
    ->     SELECT r.id
    ->     FROM empleado r
    ->     JOIN cliente c ON r.id = c.id_empleado
    ->     JOIN pedido p ON c.id = p.id_cliente
    ->     JOIN detalle_pedido dp ON p.id = dp.id_pedido
    ->     JOIN producto prod ON dp.id_producto = prod.id
    ->     JOIN gama_producto gp ON prod.id_gama_producto = gp.id
    ->     WHERE gp.descripcion_txt = 'Frutales'
    -> );
+----+--------------+----------+
| id | id_direccion | telefono |
+----+--------------+----------+
|  1 |            1 |     NULL |
|  2 |            2 |     NULL |
|  3 |            3 |     NULL |
+----+--------------+----------+

11. Devuelve un listado con los clientes que han realizado algún pedido pero no
    han realizado ningún pago.

SELECT c.nombre AS 'Nombre Cliente'
FROM cliente c
WHERE c.id IN (SELECT id_cliente FROM pedido)
  AND c.id NOT IN (SELECT id_cliente FROM pago);

12. Devuelve un listado con los datos de los empleados que no tienen clientes
    asociados y el nombre de su jefe asociado.

SELECT e.nombre AS 'Nombre Empleado', e.apellido1 AS 'Apellido Empleado', e.apellido2 AS 'Segundo Apellido Empleado', j.nombre AS 'Nombre Jefe', j.apellido1 AS 'Apellido Jefe', j.apellido2 AS 'Segundo Apellido Jefe'
FROM empleado e
LEFT JOIN empleado j ON e.jefe = j.id
WHERE e.id NOT IN (SELECT id_empleado FROM cliente);

Consultas resumen

1. ¿Cuántos empleados hay en la compañía?

mysql> SELECT COUNT(*) AS 'Total Empleados'
    -> FROM empleado;
+-----------------+
| Total Empleados |
+-----------------+
|               3 |
+-----------------+

2. ¿Cuántos clientes tiene cada país?

mysql> SELECT p.nombre AS 'País', COUNT(c.id) AS 'Total Clientes'
    -> FROM pais p
    -> LEFT JOIN region r ON p.id = r.id_pais
    -> LEFT JOIN ciudad ci ON r.id = ci.id_region
    -> LEFT JOIN direccion d ON ci.id = d.id_ciudad
    -> LEFT JOIN cliente c ON d.id = c.id_direccion
    -> GROUP BY p.nombre;
+---------+----------------+
| País    | Total Clientes |
+---------+----------------+
| España  |              1 |
| Francia |              1 |
| Italia  |              1 |
+---------+----------------+

3. ¿Cuál fue el pago medio en 2009?

SELECT AVG(total) AS 'Pago Medio 2009'
FROM pago
WHERE YEAR(fecha) = 2009;

4. ¿Cuántos pedidos hay en cada estado? Ordena el resultado de forma
   descendente por el número de pedidos.

mysql> SELECT estado_pedido.descripcion AS 'Estado', COUNT(pedido.id) AS 'Número de Pedidos'
    -> FROM estado_pedido
    -> LEFT JOIN pedido ON estado_pedido.id = pedido.id_estado_pedido
    -> GROUP BY estado_pedido.descripcion
    -> ORDER BY COUNT(pedido.id) DESC;
+------------+--------------------+
| Estado     | Número de Pedidos  |
+------------+--------------------+
| En proceso |                  2 |
| Entregado  |                  1 |
| Cancelado  |                  0 |
+------------+--------------------+

5. Calcula el precio de venta del producto más caro y más barato en una
   misma consulta.

mysql> SELECT 
    ->     MAX(precio_venta) AS 'Precio Más Caro',
    ->     MIN(precio_venta) AS 'Precio Más Barato'
    -> FROM producto;
+------------------+--------------------+
| Precio Más Caro  | Precio Más Barato  |
+------------------+--------------------+
|              700 |                500 |
+------------------+--------------------+

6. Calcula el número de clientes que tiene la empresa.

mysql> SELECT COUNT(*) AS 'Número de Clientes'
    -> FROM cliente;
+---------------------+
| Número de Clientes  |
+---------------------+
|                   3 |
+---------------------+

7. ¿Cuántos clientes existen con domicilio en la ciudad de Madrid?


mysql> SELECT COUNT(*) AS 'Clientes en Madrid'
    -> FROM cliente c
    -> JOIN direccion d ON c.id_direccion = d.id
    -> JOIN ciudad ci ON d.id_ciudad = ci.id
    -> WHERE ci.nombre = 'Madrid';
+--------------------+
| Clientes en Madrid |
+--------------------+
|                  1 |
+--------------------+

8. ¿Calcula cuántos clientes tiene cada una de las ciudades que empiezan
   por M?


mysql> SELECT ci.nombre AS 'Ciudad', COUNT(c.id) AS 'Total Clientes'
    -> FROM ciudad ci
    -> JOIN direccion d ON ci.id = d.id_ciudad
    -> JOIN cliente c ON d.id = c.id_direccion
    -> WHERE ci.nombre LIKE 'M%'
    -> GROUP BY ci.nombre;
+--------+----------------+
| Ciudad | Total Clientes |
+--------+----------------+
| Madrid |              1 |
+--------+----------------+

9. Devuelve el nombre de los representantes de ventas y el número de clientes
   al que atiende cada uno.

mysql> SELECT e.nombre AS 'Representante de Ventas', COUNT(c.id) AS 'Número de Clientes'
    -> FROM empleado e
    -> LEFT JOIN cliente c ON e.id = c.id_empleado
    -> WHERE e.jefe IS NULL
    -> GROUP BY e.nombre;
+-------------------------+---------------------+
| Representante de Ventas | Número de Clientes  |
+-------------------------+---------------------+
| Juan                    |                   1 |
+-------------------------+---------------------+

10. Calcula el número de clientes que no tiene asignado representante de
    ventas.

mysql> SELECT COUNT(*) AS 'Clientes sin Representante'
    -> FROM cliente
    -> WHERE id_empleado IS NULL;
+----------------------------+
| Clientes sin Representante |
+----------------------------+
|                          0 |
+----------------------------+

11. Calcula la fecha del primer y último pago realizado por cada uno de los
    clientes. El listado deberá mostrar el nombre y los apellidos de cada cliente.

mysql> SELECT 
    ->     c.nombre AS 'Nombre Cliente',
    ->     MIN(p.fecha) AS 'Primer Pago',
    ->     MAX(p.fecha) AS 'Último Pago'
    -> FROM 
    ->     cliente c
    -> LEFT JOIN 
    ->     pago p ON c.id = p.id_cliente
    -> GROUP BY 
    ->     c.nombre;
+----------------+-------------+--------------+
| Nombre Cliente | Primer Pago | Último Pago  |
+----------------+-------------+--------------+
| Cliente 1      | 2023-01-15  | 2023-01-15   |
| Cliente 2      | 2023-02-20  | 2023-02-20   |
| Cliente 3      | 2023-03-10  | 2023-03-10   |
+----------------+-------------+--------------+

12. Calcula el número de productos diferentes que hay en cada uno de los
    pedidos.

mysql> SELECT id_pedido, COUNT(DISTINCT id_producto) AS 'Número de Productos'
    -> FROM detalle_pedido
    -> GROUP BY id_pedido;
+-----------+----------------------+
| id_pedido | Número de Productos  |
+-----------+----------------------+
|         1 |                    1 |
|         2 |                    1 |
|         3 |                    1 |
+-----------+----------------------+

13. Calcula la suma de la cantidad total de todos los productos que aparecen en
    cada uno de los pedidos.

mysql> SELECT id_pedido, SUM(cantidad) AS 'Cantidad Total'
    -> FROM detalle_pedido
    -> GROUP BY id_pedido;
+-----------+----------------+
| id_pedido | Cantidad Total |
+-----------+----------------+
|         1 |              2 |
|         2 |              1 |
|         3 |              3 |
+-----------+----------------+

14. Devuelve un listado de los 20 productos más vendidos y el número total de
    unidades que se han vendido de cada uno. El listado deberá estar ordenado
    por el número total de unidades vendidas.

mysql> SELECT 
    ->     dp.id_producto,
    ->     p.nombre AS 'Nombre Producto',
    ->     SUM(dp.cantidad) AS 'Total Unidades Vendidas'
    -> FROM 
    ->     detalle_pedido dp
    -> JOIN 
    ->     producto p ON dp.id_producto = p.id
    -> GROUP BY 
    ->     dp.id_producto, p.nombre
    -> ORDER BY 
    ->     SUM(dp.cantidad) DESC
    -> LIMIT 20;
+-------------+-----------------+-------------------------+
| id_producto | Nombre Producto | Total Unidades Vendidas |
+-------------+-----------------+-------------------------+
|           3 | Smartphone      |                       3 |
|           1 | Lavadora        |                       2 |
|           2 | Sofá            |                       1 |
+-------------+-----------------+-------------------------+

15. La facturación que ha tenido la empresa en toda la historia, indicando la
    base imponible, el IVA y el total facturado. La base imponible se calcula
    sumando el coste del producto por el número de unidades vendidas de la
    tabla detalle_pedido. El IVA es el 21 % de la base imponible, y el total la
    suma de los dos campos anteriores.

mysql> SELECT 
    ->     SUM(dp.precio_unidad * dp.cantidad) AS 'Base Imponible',
    ->     SUM(dp.precio_unidad * dp.cantidad * 0.21) AS 'IVA',
    ->     SUM(dp.precio_unidad * dp.cantidad) + SUM(dp.precio_unidad * dp.cantidad * 0.21) AS 'Total Facturado'
    -> FROM 
    ->     detalle_pedido dp;
+----------------+------+-----------------+
| Base Imponible | IVA  | Total Facturado |
+----------------+------+-----------------+
|           3200 |  672 |            3872 |
+----------------+------+-----------------+

16. La misma información que en la pregunta anterior, pero agrupada por
    código de producto.

mysql> SELECT 
    ->     dp.id_producto AS 'Código de Producto',
    ->     SUM(dp.precio_unidad * dp.cantidad) AS 'Base Imponible',
    ->     SUM(dp.precio_unidad * dp.cantidad * 0.21) AS 'IVA',
    ->     SUM(dp.precio_unidad * dp.cantidad) + SUM(dp.precio_unidad * dp.cantidad * 0.21) AS 'Total Facturado'
    -> FROM 
    ->     detalle_pedido dp
    -> GROUP BY 
    ->     dp.id_producto;
+---------------------+----------------+------+-----------------+
| Código de Producto  | Base Imponible | IVA  | Total Facturado |
+---------------------+----------------+------+-----------------+
|                   1 |            500 |  105 |             605 |
|                   2 |            600 |  126 |             726 |
|                   3 |           2100 |  441 |            2541 |
+---------------------+----------------+------+-----------------+

17. La misma información que en la pregunta anterior, pero agrupada por
    código de producto filtrada por los códigos que empiecen por OR

mysql> SELECT 
    ->     dp.id_producto AS 'Código de Producto',
    ->     SUM(dp.precio_unidad * dp.cantidad) AS 'Base Imponible',
    ->     SUM(dp.precio_unidad * dp.cantidad * 0.21) AS 'IVA',
    ->     SUM(dp.precio_unidad * dp.cantidad) + SUM(dp.precio_unidad * dp.cantidad * 0.21) AS 'Total Facturado'
    -> FROM 
    ->     detalle_pedido dp
    -> JOIN
    ->     producto p ON dp.id_producto = p.id
    -> JOIN
    ->     gama_producto gp ON p.id_gama_producto = gp.id
    -> WHERE
    ->     gp.descripcion_txt LIKE 'OR%'
    -> GROUP BY 
    ->     dp.id_producto;
+---------------------+----------------+------+-----------------+
| Código de Producto  | Base Imponible | IVA  | Total Facturado |
+---------------------+----------------+------+-----------------+
|                   1 |            500 |  105 |             605 |
|                   2 |            600 |  126 |             726 |
|                   3 |           2100 |  441 |            2541 |
+---------------------+----------------+------+-----------------+

18. Lista las ventas totales de los productos que hayan facturado más de 3000
    euros. Se mostrará el nombre, unidades vendidas, total facturado y total
    facturado con impuestos (21% IVA).

SELECT 
    p.nombre AS 'Nombre del Producto',
    SUM(dp.cantidad) AS 'Unidades Vendidas',
    SUM(dp.precio_unidad * dp.cantidad) AS 'Total Facturado',
    SUM(dp.precio_unidad * dp.cantidad) * 1.21 AS 'Total Facturado con Impuestos'
FROM 
    detalle_pedido dp
JOIN 
    producto p ON dp.id_producto = p.id
GROUP BY 
    p.nombre
HAVING 
    SUM(dp.precio_unidad * dp.cantidad) > 3000;

19. Muestre la suma total de todos los pagos que se realizaron para cada uno
    de los años que aparecen en la tabla pagos.

mysql> SELECT YEAR(fecha) AS 'Año', SUM(total) AS 'Total Pagado'
    -> FROM pago
    -> GROUP BY YEAR(fecha);
+------+--------------+
| Año  | Total Pagado |
+------+--------------+
| 2023 |        451.5 |
+------+--------------+

------------------------------------------------------------

Subconsultas
Con operadores básicos de comparación
1. Devuelve el nombre del cliente con mayor límite de crédito.
SELECT nombre
FROM cliente
WHERE limite_credito = (
    SELECT MAX(limite_credito) 
    FROM cliente
);

2. Devuelve el nombre del producto que tenga el precio de venta más caro.
SELECT nombre 
FROM producto 
WHERE precio_venta = (
    SELECT MAX(precio_venta) 
    FROM producto
);

3. Devuelve el nombre del producto del que se han vendido más unidades.
(Tenga en cuenta que tendrá que calcular cuál es el número total de
unidades que se han vendido de cada producto a partir de los datos de la
tabla detalle_pedido)

SELECT nombre_producto
FROM (
    SELECT p.nombre AS nombre_producto, SUM(dp.cantidad) AS total_vendido
    FROM producto p
    JOIN detalle_pedido dp ON p.id = dp.id_producto
    GROUP BY p.nombre
) AS ventas_por_producto
ORDER BY total_vendido DESC
LIMIT 1;

+-----------------+
| nombre_producto |
+-----------------+
| Smartphone      |
+-----------------+

4. Los clientes cuyo límite de crédito sea mayor que los pagos que haya
realizado. (Sin utilizar INNER JOIN).

SELECT id, nombre
FROM cliente
WHERE limite_credito > (
    SELECT IFNULL(SUM(total), 0)
    FROM pago
    WHERE pago.id_cliente = cliente.id
);

5. Devuelve el producto que más unidades tiene en stock.

SELECT nombre
FROM producto
WHERE id = (
    SELECT id_producto
    FROM inventario
    ORDER BY stock DESC
    LIMIT 1
);
+----------+
| nombre   |
+----------+
| Lavadora |
+----------+

6. Devuelve el producto que menos unidades tiene en stock.
SELECT nombre
FROM producto
WHERE id = (
    SELECT id_producto
    FROM inventario
    ORDER BY stock ASC
    LIMIT 1
);

+------------+
| nombre     |
+------------+
| Smartphone |
+------------+
7. Devuelve el nombre, los apellidos y el email de los empleados que están a
cargo de Alberto Soria.

SELECT e.nombre, e.apellido1, e.apellido2, e.email
FROM empleado AS e
JOIN empleado AS jefe ON e.jefe = jefe.id
WHERE jefe.nombre = 'Alberto' AND jefe.apellido1 = 'Soria';

Subconsultas con ALL y ANY
8. Devuelve el nombre del cliente con mayor límite de crédito.
SELECT nombre
FROM cliente
WHERE limite_credito >= ALL (
    SELECT limite_credito 
    FROM cliente
);

9. Devuelve el nombre del producto que tenga el precio de venta más caro.
SELECT nombre
FROM producto
WHERE precio_venta >= ALL (
    SELECT precio_venta 
    FROM producto
);

+------------+
| nombre     |
+------------+
| Smartphone |
+------------+

10. Devuelve el producto que menos unidades tiene en stock.
SELECT nombre
FROM producto
WHERE id = ANY (
    SELECT id_producto
    FROM inventario
    WHERE stock = ANY (SELECT MIN(stock) FROM inventario)
);
Subconsultas con IN y NOT IN
11. Devuelve el nombre, apellido1 y cargo de los empleados que no
representen a ningún cliente.

SELECT empleado.nombre, empleado.apellido1, empleado.apellido2, puesto.nombre
FROM empleado
LEFT JOIN puesto ON empleado.puesto_id = puesto.id
WHERE empleado.id NOT IN (
  SELECT id_empleado
  FROM cliente
  
);
12. Devuelve un listado que muestre solamente los clientes que no han
realizado ningún pago.

SELECT nombre 
FROM cliente 
WHERE id NOT IN(
  SELECT id_cliente
  FROM pago
);
13. Devuelve un listado que muestre solamente los clientes que sí han realizado
algún pago.
SELECT nombre 
FROM cliente 
WHERE id IN(
  SELECT id_cliente
  FROM pago
);

+-----------+
| nombre    |
+-----------+
| Cliente 1 |
| Cliente 2 |
| Cliente 3 |
+-----------+
14. Devuelve un listado de los productos que nunca han aparecido en un
pedido.
SELECT nombre
FROM producto
WHERE id NOT IN (
    SELECT id_producto
    FROM detalle_pedido
);
15. Devuelve el nombre, apellidos y puesto de aquellos
empleados que no sean representante de ventas de ningún cliente.


SELECT empleado.nombre, empleado.apellido1, empleado.apellido2, puesto.nombre
FROM empleado
LEFT JOIN puesto ON empleado.puesto_id = puesto.id
WHERE empleado.id NOT IN (
  SELECT id_empleado
  FROM cliente
  );
16. Devuelve las oficinas donde no trabajan ninguno de los empleados que
hayan sido los representantes de ventas de algún cliente que haya realizado
la compra de algún producto de la gama Frutales.
SELECT oficina.id
FROM oficina
WHERE id NOT IN (
    SELECT id_oficina
    FROM empleado
    WHERE id IN (
        SELECT DISTINCT id_empleado
        FROM cliente
        WHERE id IN (
            SELECT DISTINCT id_cliente
            FROM pedido
            JOIN detalle_pedido ON pedido.id = detalle_pedido.id_pedido
            JOIN producto ON detalle_pedido.id_producto = producto.id
            JOIN gama_producto ON producto.id_gama_producto = gama_producto.id
            WHERE gama_producto.descripcion_txt = 'Frutales'
        )
       
    )
);

+----+
| id |
+----+
|  1 |
|  2 |
|  3 |
+----+

17. Devuelve un listado con los clientes que han realizado algún pedido pero no
han realizado ningún pago.
SELECT nombre
FROM cliente
WHERE id IN (
    SELECT id_cliente
    FROM pedido
) AND id NOT IN (
    SELECT id_cliente
    FROM pago
);


Subconsultas con EXISTS y NOT EXISTS
18. Devuelve un listado que muestre solamente los clientes que no han
realizado ningún pago.
SELECT nombre, fax
FROM cliente c
WHERE EXISTS (
    SELECT 1
    FROM pedido p
    WHERE p.id_cliente = c.id
) AND NOT EXISTS (
    SELECT 1
    FROM pago pa
    WHERE pa.id_cliente = c.id
);

19. Devuelve un listado que muestre solamente los clientes que sí han realizado
algún pago.

SELECT nombre, fax
FROM cliente c
WHERE EXISTS (
    SELECT 1
    FROM pago pa
    WHERE pa.id_cliente = c.id
);
+-----------+-----------+
| nombre    | fax       |
+-----------+-----------+
| Cliente 1 | 123456789 |
| Cliente 2 | 987654321 |
| Cliente 3 | 555555555 |
+-----------+-----------+
20. Devuelve un listado de los productos que nunca han aparecido en un
pedido.

SELECT nombre
FROM producto p
WHERE NOT EXISTS (
    SELECT 1
    FROM detalle_pedido dp
    WHERE dp.id_producto = p.id
);
21. Devuelve un listado de los productos que han aparecido en un pedido
alguna vez.
SELECT nombre
FROM producto p
WHERE EXISTS (
    SELECT 1
    FROM detalle_pedido dp
    WHERE dp.id_producto = p.id
);

+------------+
| nombre     |
+------------+
| Lavadora   |
| Sofá       |
| Smartphone |
+------------+

Consultas variadas

1. Devuelve el listado de clientes indicando el nombre del cliente y cuántos
   pedidos ha realizado. Tenga en cuenta que pueden existir clientes que no
   han realizado ningún pedido.

mysql> SELECT 
    ->     c.nombre AS 'Nombre del Cliente',
    ->     COUNT(p.id) AS 'Cantidad de Pedidos'
    -> FROM 
    ->     cliente c
    -> LEFT JOIN 
    ->     pedido p ON c.id = p.id_cliente
    -> GROUP BY 
    ->     c.nombre;
+--------------------+---------------------+
| Nombre del Cliente | Cantidad de Pedidos |
+--------------------+---------------------+
| Cliente 1          |                   1 |
| Cliente 2          |                   1 |
| Cliente 3          |                   1 |
+--------------------+---------------------+

2. Devuelve un listado con los nombres de los clientes y el total pagado por
   cada uno de ellos. Tenga en cuenta que pueden existir clientes que no han
   realizado ningún pago.

mysql> SELECT 
    ->     c.nombre AS 'Nombre del Cliente',
    ->     COALESCE(SUM(p.total), 0) AS 'Total Pagado'
    -> FROM 
    ->     cliente c
    -> LEFT JOIN 
    ->     pago p ON c.id = p.id_cliente
    -> GROUP BY 
    ->     c.nombre;
+--------------------+--------------+
| Nombre del Cliente | Total Pagado |
+--------------------+--------------+
| Cliente 1          |        100.5 |
| Cliente 2          |       200.75 |
| Cliente 3          |       150.25 |
+--------------------+--------------+

3. Devuelve el nombre de los clientes que hayan hecho pedidos en 2008
   ordenados alfabéticamente de menor a mayor.

SELECT DISTINCT
    c.nombre AS 'Nombre del Cliente'
FROM 
    cliente c
JOIN 
    pedido p ON c.id = p.id_cliente
WHERE 
    YEAR(p.fecha_pedido) = 2008
ORDER BY 
    c.nombre ASC;

4. Devuelve el nombre del cliente, el nombre y primer apellido de su
   representante de ventas y el número de teléfono de la oficina del
   representante de ventas, de aquellos clientes que no hayan realizado ningún
   pago.

SELECT 
    c.nombre AS 'Nombre del Cliente',
    CONCAT(e.nombre, ' ', e.apellido1) AS 'Nombre del Representante',
    t.numero AS 'Número de Teléfono de la Oficina'
FROM 
    cliente c
JOIN 
    empleado e ON c.id_empleado = e.id
JOIN 
    oficina o ON e.id_oficina = o.id
JOIN 
    telefono t ON o.telefono = t.id
LEFT JOIN 
    pago p ON c.id = p.id_cliente
WHERE 
    p.id IS NULL;

5. Devuelve el listado de clientes donde aparezca el nombre del cliente, el
   nombre y primer apellido de su representante de ventas y la ciudad donde
   está su oficina.

mysql> SELECT 
    ->     c.nombre AS 'Nombre del Cliente',
    ->     CONCAT(e.nombre, ' ', e.apellido1) AS 'Nombre del Representante',
    ->     ci.nombre AS 'Ciudad de la Oficina'
    -> FROM 
    ->     cliente c
    -> JOIN 
    ->     empleado e ON c.id_empleado = e.id
    -> JOIN 
    ->     oficina o ON e.id_oficina = o.id
    -> JOIN 
    ->     direccion d ON o.id_direccion = d.id
    -> JOIN 
    ->     ciudad ci ON d.id_ciudad = ci.id;
+--------------------+--------------------------+----------------------+
| Nombre del Cliente | Nombre del Representante | Ciudad de la Oficina |
+--------------------+--------------------------+----------------------+
| Cliente 1          | María López              | París                |
| Cliente 2          | Carlos González          | Madrid               |
| Cliente 3          | Juan Pérez               | Madrid               |
+--------------------+--------------------------+----------------------+

6. Devuelve el nombre, apellidos, puesto y teléfono de la oficina de aquellos
   empleados que no sean representante de ventas de ningún cliente.

SELECT 
    e.nombre AS 'Nombre del Empleado',
    CONCAT(e.apellido1, ' ', COALESCE(e.apellido2, '')) AS 'Apellidos',
    p.nombre AS 'Puesto',
    t.numero AS 'Teléfono de la Oficina'
FROM 
    empleado e
JOIN 
    oficina o ON e.id_oficina = o.id
JOIN 
    puesto p ON e.id = p.id
JOIN 
    telefono t ON o.telefono = t.id
LEFT JOIN 
    cliente c ON e.id = c.id_empleado
WHERE 
    c.id_empleado IS NULL
    OR c.id_empleado = '';

7. Devuelve un listado indicando todas las ciudades donde hay oficinas y el
   número de empleados que tiene.

mysql> SELECT 
    ->     ci.nombre AS 'Ciudad',
    ->     COUNT(e.id) AS 'Número de Empleados'
    -> FROM 
    ->     ciudad ci
    -> JOIN 
    ->     direccion d ON ci.id = d.id_ciudad
    -> JOIN 
    ->     oficina o ON d.id = o.id_direccion
    -> JOIN 
    ->     empleado e ON o.id = e.id_oficina
    -> GROUP BY 
    ->     ci.nombre;
+--------+----------------------+
| Ciudad | Número de Empleados  |
+--------+----------------------+
| Madrid |                    2 |
| París  |                    1 |
+--------+----------------------+

VISTAS



1. Vista de Empleados:

CREATE VIEW vista_empleados AS
SELECT id, CONCAT(nombre, ' ', apellido1, ' ', COALESCE(apellido2, '')) AS nombre_completo, email, extension
FROM empleado;

Llamada:

SELECT * FROM vista_empleados;


2. Vista de Clientes:

CREATE VIEW vista_clientes AS
SELECT id, nombre, email
FROM cliente;

Llamada:

SELECT * FROM vista_clientes;


3. Vista de Productos Disponibles:

CREATE VIEW vista_productos_disponibles AS
SELECT p.id, p.nombre, p.precio_venta, i.stock
FROM producto p
JOIN inventario i ON p.id = i.id_producto;

Llamada:

SELECT * FROM vista_productos_disponibles;


4. Vista de Pedidos Recientes:

CREATE VIEW vista_pedidos_recientes AS
SELECT id, fecha_pedido, fecha_esperada, id_estado_pedido
FROM pedido
ORDER BY fecha_pedido DESC
LIMIT 10;

Llamada:

SELECT * FROM vista_pedidos_recientes;


5. Vista de Proveedores:

CREATE VIEW vista_proveedores AS
SELECT nit, nombre
FROM proveedor;

Llamada:

SELECT * FROM vista_proveedores;


6. Vista de Direcciones de Clientes:

CREATE VIEW vista_direcciones_clientes AS
SELECT c.id, d.calle, d.carrera, d.numero, d.postal, ci.nombre AS ciudad, r.nombre AS region, p.nombre AS pais
FROM cliente c
JOIN direccion d ON c.id_direccion = d.id
JOIN ciudad ci ON d.id_ciudad = ci.id
JOIN region r ON ci.id_region = r.id
JOIN pais p ON r.id_pais = p.id;

Llamada:

SELECT * FROM vista_direcciones_clientes;


7. Vista de Comentarios de Pedidos:

CREATE VIEW vista_comentarios_pedidos AS
SELECT id_pedido, contenido
FROM comentarios;

Llamada:

SELECT * FROM vista_comentarios_pedidos;


8. Vista de Detalles de Pedidos:

CREATE VIEW vista_detalles_pedidos AS
SELECT id_pedido, id_producto, cantidad, precio_unidad
FROM detalle_pedido;

Llamada:

SELECT * FROM vista_detalles_pedidos;


9. Vista de Pagos de Clientes:

CREATE VIEW vista_pagos_clientes AS
SELECT id_cliente, fecha, total

Llamada:

SELECT * FROM vista_pagos_clientes;


10. Vista de Gamas de Productos:

CREATE VIEW vista_gamas_productos AS
SELECT id, descripcion_txt, imagen
FROM gama_producto;

Llamada:

SELECT * FROM vista_gamas_productos;

PROCEDIMIENTOS ALMACENADOS

DELIMITER $$

-- Procedimiento para crear un empleado
CREATE PROCEDURE CrearEmpleado (
    IN p_nombre VARCHAR(100),
    IN p_apellido1 VARCHAR(100),
    IN p_apellido2 VARCHAR(100),
    IN p_extension INT,
    IN p_email VARCHAR(100),
    IN p_jefe INT,
    IN p_id_oficina INT,
    IN p_id_puesto INT
)
BEGIN
    INSERT INTO empleado (nombre, apellido1, apellido2, extension, email, jefe, id_oficina, id_puesto) 
    VALUES (p_nombre, p_apellido1, p_apellido2, p_extension, p_email, p_jefe, p_id_oficina, p_id_puesto);
END$$

-- Llamado al procedimiento para crear un empleado
CALL CrearEmpleado('Juan', 'Perez', 'Gomez', 101, 'juan@example.com', NULL, 1, 1)$$

-- Procedimiento para buscar un empleado por ID
CREATE PROCEDURE BuscarEmpleadoPorID (
    IN p_id INT
)
BEGIN
    SELECT * FROM empleado WHERE id = p_id;
END$$

-- Llamado al procedimiento para buscar un empleado por ID
CALL BuscarEmpleadoPorID(1)$$

-- Procedimiento para actualizar el email de un empleado
CREATE PROCEDURE ActualizarEmailEmpleado (
    IN p_id INT,
    IN p_nuevo_email VARCHAR(100)
)
BEGIN
    UPDATE empleado SET email = p_nuevo_email WHERE id = p_id;
END$$

-- Llamado al procedimiento para actualizar el email de un empleado
CALL ActualizarEmailEmpleado(1, 'nuevo_email@example.com')$$

-- Procedimiento para eliminar un empleado por ID
CREATE PROCEDURE EliminarEmpleadoPorID (
    IN p_id INT
)
BEGIN
    DELETE FROM empleado WHERE id = p_id;
END$$

-- Llamado al procedimiento para eliminar un empleado por ID
CALL EliminarEmpleadoPorID(1)$$

-- Procedimiento para crear un cliente
CREATE PROCEDURE CrearCliente (
    IN p_id INT,
    IN p_nombre VARCHAR(100),
    IN p_fax VARCHAR(100),
    IN p_id_direccion INT,
    IN p_id_empleado INT
)
BEGIN
    INSERT INTO cliente (id, nombre, fax, id_direccion, id_empleado) 
    VALUES (p_id, p_nombre, p_fax, p_id_direccion, p_id_empleado);
END$$

-- Llamado al procedimiento para crear un cliente
CALL CrearCliente(1, 'Cliente1', '123456', 1, 1)$$

-- Procedimiento para buscar un cliente por ID
CREATE PROCEDURE BuscarClientePorID (
    IN p_id INT
)
BEGIN
    SELECT * FROM cliente WHERE id = p_id;
END$$

-- Llamado al procedimiento para buscar un cliente por ID
CALL BuscarClientePorID(1)$$

-- Procedimiento para actualizar el nombre de un cliente
CREATE PROCEDURE ActualizarNombreCliente (
    IN p_id INT,
    IN p_nuevo_nombre VARCHAR(100)
)
BEGIN
    UPDATE cliente SET nombre = p_nuevo_nombre WHERE id = p_id;
END$$

-- Llamado al procedimiento para actualizar el nombre de un cliente
CALL ActualizarNombreCliente(1, 'Nuevo Nombre')$$

-- Procedimiento para eliminar un cliente por ID
CREATE PROCEDURE EliminarClientePorID (
    IN p_id INT
)
BEGIN
    DELETE FROM cliente WHERE id = p_id;
END$$

-- Llamado al procedimiento para eliminar un cliente por ID
CALL EliminarClientePorID(1)$$

-- Procedimiento para crear un producto
CREATE PROCEDURE CrearProducto (
    IN p_nombre VARCHAR(100),
    IN p_descripcion VARCHAR(200),
    IN p_precio_venta DOUBLE,
    IN p_precio_compra DOUBLE,
    IN p_id_gama_producto INT,
    IN p_id_dimensiones INT,
    IN p_nit_proveedor INT
)
BEGIN
    INSERT INTO producto (nombre, descripcion, precio_venta, precio_compra, id_gama_producto, id_dimensiones, nit_proveedor) 
    VALUES (p_nombre, p_descripcion, p_precio_venta, p_precio_compra, p_id_gama_producto, p_id_dimensiones, p_nit_proveedor);
END$$

-- Llamado al procedimiento para crear un producto
CALL CrearProducto('Producto1', 'Descripción del producto 1', 100.0, 50.0, 1, 1, 123456)$$

-- Procedimiento para buscar un producto por ID
CREATE PROCEDURE BuscarProductoPorID (
    IN p_id INT
)
BEGIN
    SELECT * FROM producto WHERE id = p_id;
END$$

-- Llamado al procedimiento para buscar un producto por ID
CALL BuscarProductoPorID(1)$$

DELIMITER ;



```
