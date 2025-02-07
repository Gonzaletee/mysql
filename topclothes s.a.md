### Análisis Descriptivo de la Empresa "TopClothes S.A."

### Introducción
En este proyecto se realizará un análisis detallado de la empresa TopClothes S.A., una compañía dedicada a la venta de ropa. A través del uso de consultas en MySQL, se explorará el comportamiento de las ventas, los productos más vendidos, los clientes con mayor gasto y otros factores clave que permitirán comprender el rendimiento del negocio.

### Objetivo
El objetivo principal de este análisis es extraer insights valiosos sobre las ventas de la empresa, así como demostrar un dominio sólido de SQL, utilizando consultas avanzadas con SELECT, GROUP BY, HAVING, JOINs y funciones agregadas.

### Diagrama


### Consultas
#### Ventas total del año 2025
Para conocer el rendimiento global de la empresa durante el año 2025, se calcularon las ventas totales en este periodo:

Consulta SQL:
```sql 
SELECT 
    YEAR(SaleDate) AS Año,
    round(sum(TotalAmount),2) AS ventas_total
FROM 
    sales
GROUP BY 
    YEAR(SaleDate)
```
Resultado

| Year  | Sales    |
|-------|----------|
| 2025  |  6739.27 |


#### Ventas Totales por Mes (Ventas Brutas y Margen Absoluto)

Para comprender mejor la distribución de las ventas, analizamos los ingresos mensuales dentro del año 2025

Consulta SQL:
```sql
SELECT 
    YEAR(SaleDate) AS Año,
    MONTHNAME(SaleDate) AS Mes,
    SUM(TotalAmount) AS ventas_total
FROM 
    sales
GROUP BY 
    YEAR(SaleDate), MONTH(SaleDate),  MONTHNAME(SaleDate)
ORDER BY 
    ventas_total DESC;
```
Resultado: 
| Year  | Month      | Sales    |
|-------|------------|----------|
| 2025  | June       | 1086.05  |
| 2025  | March      | 812      |
| 2025  | September  | 687.5    |
| 2025  | May        | 636.99   |
| 2025  | January    | 636.24   |


#### Promedio Ventas Brutas Mensuales (Top 5)

Se analizan los meses con mayor promedio de ventas.

Consulta SQL:

```sql
    MONTHNAME(SaleDate) AS Mes,
    avg(TotalAmount) AS ventas_total
FROM 
    sales
GROUP BY 
    YEAR(SaleDate), MONTH(SaleDate),  MONTHNAME(SaleDate)
ORDER BY 
    ventas_total DESC
LIMIT 5;
```
Resultado:
| Year  | Month     | Sales     |
|-------|-----------|-----------| 
|2025   |April      |147.81     |
|2025   |October    |147.62     |
|2025   |November   |142.25     |
|2025   |December   |138.02     |
|2025   |September  |137.5      |


#### Productos Más Vendidos

Consulta SQL:
``` sql
SELECT 
    p.ProductName,
    SUM(sd.Quantity) AS TotalVendido
FROM 
    salesdetails sd
INNER JOIN 
    products p ON sd.ProductID = p.ProductID
GROUP BY 
    p.ProductName
ORDER BY 
    TotalVendido DESC
LIMIT 5;
 ```
Resultado:
|ProductName    |TotalVendido   |
|---------------|---------------|
|Denim Jeans    |	15          |
|Summer Dress   |	12          |
|Maxi Skirt     |	12          |
|Leather Jacket |	10          |
|Classic T-Shirt|	9           |

Esta consulta nos permite conocer los productos con mayor rotación dentro del inventario.

#### Clientes que Más Gastaron

Consulta SQL:
```sql
SELECT 
    c.first_name, 
    c.last_name,
    SUM(o.TotalAmount) AS GastoTotal
FROM 
    customers c
INNER JOIN 
    sales o ON c.CustomerID = o.CustomerID
GROUP BY 
    c.CustomerID
ORDER BY 
    GastoTotal DESC
LIMIT 5;

```

Resultado:
|first_name     |last_name  |GastoTotal |
|---------------|-----------|-----------|
|Mead           |Pask       |494.27     |
|usy           |Scourfield	|421.25     |
|Che            |Conboy	    |411.25     |
|Godard         |Essery	    |341.25     |
|Aubrey         |Umpleby    |330.25     |

Con esta información podemos identificar a los clientes de mayor valor y analizar su comportamiento de compra.

#### Productos Comprados por el Cliente que Más Gastó
Consulta SQL:
```sql
SELECT DISTINCT 
    p.ProductName
FROM 
    sales s
INNER JOIN 
    customers c ON s.CustomerID = c.CustomerID
INNER JOIN 
    salesdetails sd ON s.SaleID = sd.SaleID
INNER JOIN 
    products p ON sd.ProductID = p.ProductID
WHERE 
    c.first_name = 'Mead' AND c.last_name = 'Pask'
ORDER BY 
    p.ProductName;

```
Resultado:
|ProductName    |
----------------
|Bomber Jacket  |
|Leather Jacket |
|Summer Dress   |

Este análisis nos permite ver qué productos han sido los favoritos del cliente con mayor gasto.

#### Clientes que Gastaron Menos de $200
quien gasto meenos de  200 dlrs

Consulta SQL:
```sql
SELECT 
    c.first_name,
    c.last_name,
    SUM(s.TotalAmount) AS TotalGastado
FROM 
    customers c
INNER JOIN 
    sales s ON c.CustomerID = s.CustomerID
GROUP BY 
    c.CustomerID
HAVING 
    SUM(s.TotalAmount) < 200
ORDER BY 
    TotalGastado DESC;
```
Resultado:
|first_name |last_name  |TotalGasto |
|-----------|-----------|-----------|
|Gasper     |Keelinge   |195.99     |
|Wang       |Pellett    |195.25     |
|Walker     |Sabine     |194        |
|Johnathon  |Tremathick |175.99     |
|Becki      |Mulholland |140        |
|Sascha     |Overshott  |135        |

Este análisis permite identificar clientes de bajo consumo, lo que puede ser útil para estrategias de retención y promociones.

### Resultados finales

El total de ventas del año 2025 asciende a \$ 6,739.27 USD. proporcionando una referencia clara sobre el volumen de ingresos generado durante este periodo. Este dato permite establecer un punto de partida para el análisis de tendencias y estrategias comerciales futuras.

En cuanto a la distribución mensual de las ventas, junio se posiciona como el mes con mayor volumen de transacciones, seguido por marzo y septiembre. Esto sugiere una posible estacionalidad en la demanda, que podría ser aprovechada a través de estrategias de marketing y gestión de inventarios.

Por otro lado, abril y octubre presentan las mayores ventas promedio mensuales. Estos meses representan oportunidades clave para la implementación de promociones y estrategias de fidelización que maximicen el impacto en las ventas.

El análisis de los productos más vendidos revela que los artículos con mayor rotación en el inventario son Denim Jeans (15 unidades), Summer Dress (12 unidades) y Maxi Skirt (12 unidades). Este hallazgo permite a la empresa optimizar su inventario y reforzar la disponibilidad de estos productos en futuras temporadas.

Respecto al comportamiento de los clientes, se identificó que Mead Pask fue el comprador que realizó el mayor gasto, con un total de \$ 494.27, seguido por Usy Scourfield con \$ 421.25. El análisis detallado de las compras de Mead Pask muestra que sus productos favoritos fueron Bomber Jacket, Leather Jacket y Summer Dress, lo que sugiere una preferencia por prendas de moda y versátiles.

Asimismo, se identificaron los clientes con menor nivel de gasto, siendo Becki Mulholland (\$140) y Sascha Overshott (\$135 ) los de consumo más bajo. Estos datos pueden ser útiles para la implementación de estrategias de retención de clientes y promociones personalizadas con el fin de aumentar su participación en las ventas.

#### Conclusiones

Este análisis proporciona una visión detallada sobre el comportamiento de las ventas de TopClothes S.A., permitiendo identificar patrones clave, productos más vendidos y clientes de alto y bajo valor. La información obtenida puede ser utilizada para diseñar estrategias de marketing más efectivas, optimizar la gestión del inventario y fortalecer la fidelización de clientes.

El estudio también destaca la importancia de la segmentación de clientes y la planificación de promociones en meses estratégicos como abril y octubre. Además, permite a la empresa anticiparse a la demanda estacional, ajustando sus niveles de stock y estrategias comerciales según los hallazgos obtenidos.
