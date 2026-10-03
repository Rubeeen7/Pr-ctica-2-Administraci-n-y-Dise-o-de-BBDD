# Práctica 2 - Modelo Entidad/Relación: Tajinaste S.A.

## Descripción

La empresa Tajinaste S.A. se dedica a la venta de plantas, productos de jardinería y decoración mediante una red de viveros.

El objetivo de este modelo entidad/relación es representar la información necesaria para controlar:

- Los viveros y sus zonas.
- El stock de productos disponible en cada zona.
- Los empleados y sus destinos a lo largo del tiempo.
- El histórico de puestos desempeñados por los empleados.
- La productividad de los empleados en las distintas zonas.
- Los clientes pertenecientes al programa Tajinaste Plus.
- Los pedidos realizados por dichos clientes.
- Los empleados responsables de gestionar cada pedido.


# Entidades

## VIVERO

Representa cada uno de los viveros pertenecientes a Tajinaste S.A.

### Atributos

| Atributo | Descripción | Dominio | Ejemplo |
|---|---|---|---|
| id_vivero | Identificador único del vivero | Entero positivo | 1 |
| latitud | Latitud de la georreferenciación del vivero | Número real | 28.4636 |
| longitud | Longitud de la georreferenciación del vivero | Número real | -16.2518 |

`id_vivero` es el atributo identificador de la entidad.


## ZONA

Representa cada una de las zonas existentes dentro de un vivero, como pueden ser una zona exterior, un almacén o una zona interior.

### Atributos

| Atributo | Descripción | Dominio | Ejemplo |
|---|---|---|---|
| id_zona | Identificador único de la zona | Entero positivo | 4 |
| nombre | Nombre de la zona | Cadena de caracteres | "Zona exterior" |
| georreferenciación | Localización geográfica de la zona | Atributo compuesto | — |
| latitud | Latitud de la zona | Número real | 28.4637 |
| longitud | Longitud de la zona | Número real | -16.2520 |

`id_zona` es el atributo identificador.

La georreferenciación es un atributo compuesto formado por `latitud` y `longitud`.


## PRODUCTO

Representa los diferentes productos comercializados por Tajinaste S.A., como plantas, productos de jardinería o productos de decoración.

### Atributos

| Atributo | Descripción | Dominio | Ejemplo |
|---|---|---|---|
| id_producto | Identificador único del producto | Entero positivo | 27 |
| nombre | Nombre del producto | Cadena de caracteres | "Maceta de barro" |

`id_producto` es el atributo identificador.


## EMPLEADO

Representa a los empleados de Tajinaste S.A.

### Atributos

| Atributo | Descripción | Dominio | Ejemplo |
|---|---|---|---|
| DNI | Documento identificativo del empleado | Cadena de caracteres | "12345678A" |
| nombre | Nombre del empleado | Cadena de caracteres | "Pedro" |
| apellidos | Apellidos del empleado | Cadena de caracteres | "García Pérez" |

`DNI` es el atributo identificador.


## PUESTO

Representa los distintos puestos de trabajo que pueden desempeñar los empleados.

### Atributos

| Atributo | Descripción | Dominio | Ejemplo |
|---|---|---|---|
| id_puesto | Identificador único del puesto | Entero positivo | 3 |
| nombre | Nombre del puesto | Cadena de caracteres | "Vendedor" |

`id_puesto` es el atributo identificador.


## CLIENTE

Representa a los clientes pertenecientes al programa de fidelización Tajinaste Plus.

### Atributos

| Atributo | Descripción | Dominio | Ejemplo |
|---|---|---|---|
| id_cliente | Identificador único del cliente | Entero positivo | 15 |
| nombre | Nombre del cliente | Cadena de caracteres | "Ana Pérez" |
| fecha_ingreso_plus | Fecha de ingreso en Tajinaste Plus | Fecha | 2026-03-15 |
| bonificación | Bonificación actualmente asociada al cliente | Número/porcentaje | 10% |

`id_cliente` es el atributo identificador.

La bonificación se asigna en función del volumen de compras realizado por el cliente.


## PEDIDO

Representa los pedidos realizados por los clientes Tajinaste Plus.

### Atributos

| Atributo | Descripción | Dominio | Ejemplo |
|---|---|---|---|
| id_pedido | Identificador único del pedido | Entero positivo | 1024 |
| fecha | Fecha en la que se realiza el pedido | Fecha | 2026-05-20 |
| importe_total | Importe total del pedido | Número real positivo | 84.50 |

`id_pedido` es el atributo identificador.


# Relaciones

## TIENE

Relaciona las entidades `VIVERO` y `ZONA`.

Un vivero tiene una o varias zonas y cada zona pertenece a un único vivero.

### Cardinalidad

- Un `VIVERO` tiene entre 1 y N zonas.
- Una `ZONA` pertenece exactamente a 1 vivero.

Por tanto, la relación es **1:N**.

Ejemplo:

Un vivero puede disponer de las zonas "Exterior", "Almacén" e "Interior", pero cada una de estas zonas pertenece únicamente a ese vivero.


## STOCK

Relaciona las entidades `ZONA` y `PRODUCTO`.

Permite conocer qué productos se encuentran en cada zona y cuántas unidades hay disponibles.

### Cardinalidad

- Una `ZONA` puede contener entre 0 y N productos.
- Un `PRODUCTO` puede encontrarse en entre 0 y N zonas.

Por tanto, la relación es **N:M**.

### Atributos de la relación

| Atributo | Descripción | Dominio | Ejemplo |
|---|---|---|---|
| cantidad_disponible | Cantidad del producto disponible en esa zona | Entero no negativo | 35 |

`cantidad_disponible` es un atributo propio de la relación porque depende tanto del producto como de la zona.

Por ejemplo, el producto "Maceta de barro" puede tener 20 unidades en la zona exterior y 100 unidades en el almacén.


## DESTINO

Relaciona las entidades `EMPLEADO` y `ZONA`.

Permite conocer las distintas zonas a las que un empleado ha sido destinado a lo largo del tiempo.

### Cardinalidad

- Un `EMPLEADO` puede haber tenido entre 0 y N destinos.
- Una `ZONA` puede haber recibido entre 0 y N empleados.

Por tanto, desde el punto de vista histórico, la relación es **N:M**.

### Atributos de la relación

| Atributo | Descripción | Dominio | Ejemplo |
|---|---|---|---|
| fecha_inicio | Fecha en la que comienza el destino | Fecha | 2026-01-01 |
| fecha_fin | Fecha en la que finaliza el destino | Fecha o valor nulo | 2026-06-30 |
| productividad | Productividad del empleado durante ese destino | Valor numérico | 85 |

Las fechas permiten conservar el histórico de destinos de cada empleado.


## DESEMPEÑA

Relaciona las entidades `EMPLEADO` y `PUESTO`.

Permite almacenar el histórico de puestos que ha desempeñado un empleado.

### Cardinalidad

- Un `EMPLEADO` puede desempeñar entre 0 y N puestos a lo largo del tiempo.
- Un mismo `PUESTO` puede ser desempeñado por entre 0 y N empleados.

La relación es **N:M** desde el punto de vista histórico.

### Atributos de la relación

| Atributo | Descripción | Dominio | Ejemplo |
|---|---|---|---|
| fecha_inicio | Fecha de comienzo en el puesto | Fecha | 2025-01-01 |
| fecha_fin | Fecha de finalización en el puesto | Fecha o valor nulo | 2025-12-31 |

Por ejemplo, un empleado puede haber trabajado primero como vendedor y posteriormente como responsable de zona.


## REALIZA

Relaciona las entidades `CLIENTE` y `PEDIDO`.

Representa los pedidos realizados por los clientes Tajinaste Plus.

### Cardinalidad

- Un `CLIENTE` puede realizar entre 0 y N pedidos.
- Cada `PEDIDO` pertenece exactamente a 1 cliente.

Por tanto, la relación es **1:N**.


## GESTIONA

Relaciona las entidades `EMPLEADO` y `PEDIDO`.

Permite conocer qué empleado ha sido responsable de gestionar cada pedido.

### Cardinalidad

- Un `EMPLEADO` puede gestionar entre 0 y N pedidos.
- Cada `PEDIDO` es gestionado exactamente por 1 empleado.

Por tanto, la relación es **1:N**.

Cada pedido tiene un único empleado responsable.


# Restricciones semánticas

Además de las cardinalidades representadas en el modelo, se establecen las siguientes restricciones:

1. Un empleado no puede tener dos destinos simultáneamente.

   Los intervalos definidos por `fecha_inicio` y `fecha_fin` de dos destinos de un mismo empleado no pueden solaparse.

2. La fecha de finalización de un destino debe ser posterior o igual a su fecha de inicio.

3. La fecha de finalización de un puesto desempeñado debe ser posterior o igual a su fecha de inicio.

4. La cantidad disponible de un producto en una zona no puede ser negativa.

5. Cada zona pertenece a un único vivero.

6. Cada pedido pertenece a un único cliente Tajinaste Plus.

7. Cada pedido tiene un único empleado responsable.

8. Los pedidos considerados para el programa Tajinaste Plus deben haber sido realizados después de la fecha de ingreso del cliente en el programa.

9. El valor de `importe_total` de un pedido no puede ser negativo.

10. La bonificación de un cliente depende del volumen de compras realizado mensualmente.


# Ficheros entregados

- `tajinaste.drawio`: modelo entidad/relación editable en Draw.io.
- `tajinaste.png`: imagen exportada del modelo entidad/relación.
- `README.md`: documentación del modelo.
