---
title: 'Elija las columnas de distribución en Azure Database for PostgreSQL: hiperescala (Citus) (versión preliminar)'
description: Buenas opciones para las columnas de distribución en escenarios comunes de hiperescala
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: e9fba14b8979f739fd29bc277e32fb544221d08a
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65078991"
---
# <a name="choose-distribution-columns-in-azure-database-for-postgresql--hyperscale-citus-preview"></a>Elija las columnas de distribución en Azure Database for PostgreSQL: hiperescala (Citus) (versión preliminar)

La elección de columna de distribución de cada tabla es **uno de los más importantes** decisiones de modelado. Hiperescala almacena filas en particiones según el valor de columna de distribución de las filas.

Los grupos de la opción correcta relacionadas con datos juntos en los mismos nodos físicos, realizar consultas y agregar soporte técnico para todas las características SQL. Una elección incorrecta hace que sea el sistema se ejecutará con lentitud y no admite todas las características SQL entre nodos.

Esta sección proporciona sugerencias de columna de distribución para los dos escenarios más comunes de gran escala.

### <a name="multi-tenant-apps"></a>Aplicaciones de varios inquilinos

La arquitectura multiempresa usa una forma de modelado para distribuir las consultas en todos los nodos del grupo de servidores de base de datos jerárquica.  La parte superior de la jerarquía de datos se conoce como el *Id. de inquilino*y debe almacenarse en una columna en cada tabla.

Hiperescala inspecciona las consultas para ver qué Id. de inquilino que implican y busca la partición de tabla correspondiente. La consulta enruta a un único nodo de trabajo que contiene la partición. Ejecute una consulta con todos los datos relevantes que se colocan en el mismo nodo se llama a la ubicación compartida.

El siguiente diagrama ilustra la colocación en el modelo de datos de varios inquilinos. Contiene dos tablas, cuentas y campañas, cada uno de ellos distribuido por `account_id`. Las casillas sombreadas representan las particiones, cada uno de cuyo color representa el nodo de trabajo que lo contiene. Particiones verde se almacenan juntos en un nodo de trabajo y el azul en otro. Observe cómo una consulta de combinación entre las cuentas y las campañas tendría todos los datos necesarios juntos en un nodo cuando se restrinja las dos tablas en la misma cuenta\_id.

![colocación de varios inquilinos](media/concepts-hyperscale-choosing-distribution-column/multi-tenant-colocation.png)

Para aplicar este diseño en su propio esquema, identificar qué constituye a un inquilino en la aplicación. Instancias comunes incluyen la compañía, organización, cuenta o cliente. El nombre de columna será algo como `company_id` o `customer_id`. ¿Examine cada una de las consultas y pregúntese: funcionaría si tuviera más cláusulas WHERE para restringir todas las tablas implicadas en filas con el mismo identificador de inquilino?
Las consultas en el modelo de varios inquilinos se limitan a un inquilino, por ejemplo consultas sobre ventas e inventario podrían ampliarse dentro de un almacén determinado.

#### <a name="best-practices"></a>Prácticas recomendadas

-   **Partición distribuye las tablas a un inquilino comunes\_columna de identificador.** Por ejemplo, en una aplicación de SaaS donde los inquilinos son compañías, el inquilino\_identificador probablemente será empresa\_id.
-   **Convertir tablas pequeñas de varios inquilinos a las tablas de referencia.** Cuando varios inquilinos comparten una pequeña tabla de información, puede distribuir como una tabla de referencia.
-   **Restringir el filtro de consulta de todas las aplicaciones por inquilino\_id.** Cada consulta debe solicitar información de un inquilino a la vez.

Leer el [multiempresa tutorial](./tutorial-design-database-hyperscale-multi-tenant.md) para obtener un ejemplo de la creación de este tipo de aplicación.

### <a name="real-time-apps"></a>Aplicaciones en tiempo real

La arquitectura multiempresa presenta una estructura jerárquica y utiliza la colocación de datos para enrutar consultas por inquilino. Por el contrario, arquitecturas en tiempo real dependen de las propiedades específicas de distribución de sus datos para lograr un procesamiento paralelo.

Usamos "Id. de entidad" como un término para las columnas de distribución en el modelo en tiempo real. Entidades típicas son los usuarios, hosts o dispositivos.

Consultas en tiempo real normalmente incluyen numéricos agregados agrupados por fecha o categoría. Hiperescala envía estas consultas para cada partición para resultados parciales y ensambla la respuesta final en el nodo coordinador. Las consultas se ejecutan más rápido cuando como contribuir con muchos nodos como sea posible, y cuando ningún nodo solo debe hacer una cantidad desproporcionada de trabajo.

#### <a name="best-practices"></a>Prácticas recomendadas

-   **Elija una columna con cardinalidad elevada como columna de distribución.** Para la comparación, un \"estado\" campo en una tabla de pedidos con los valores "nuevos", "pago" y "enviado" es una mala elección de columna de distribución. Se supone que solo esos pocos valores, lo que limita el número de particiones que puede contener los datos y el número de nodos que pueda procesarlo. Entre las columnas con cardinalidad elevada, es buena para además de elegirlas que se utilizan con frecuencia en las cláusulas group by o como claves de combinación.
-   **Elija una columna con una distribución uniforme.** Si distribuye una tabla en una columna de sesgado a ciertos valores comunes, datos de la tabla tenderán a acumular en ciertas particiones. Los nodos que contienen esas particiones terminará haciendo más trabajo que otros nodos.
-   **Distribuir las tablas de hechos y dimensiones en sus columnas comunes.**
    La tabla de hechos puede tener sólo una clave de distribución. Las tablas que participe en la otra clave no se comparte ubicación con la tabla de hechos. Elija una dimensión colocar según el tamaño de las filas de combinación y con qué frecuencia se ha unido.
-   **Cambiar algunas tablas de dimensiones en las tablas de referencia.** Si una tabla de dimensiones no puede estar colocada con la tabla de hechos, puede mejorar el rendimiento mediante la distribución de copias de la tabla de dimensiones para todos los nodos en forma de una tabla de referencia.

Leer el [tutorial del panel en tiempo real](./tutorial-design-database-hyperscale-realtime.md) para obtener un ejemplo de la creación de este tipo de aplicación.

### <a name="timeseries-data"></a>Datos de serie temporal

En una carga de trabajo de la serie temporal, las aplicaciones consultar información reciente al archivar la información antigua.

Modela la información de serie temporal de hiperescala el error más común es utilizar la marca de tiempo como una columna de distribución. Una distribución hash según la hora distribuirá veces aparentemente de forma aleatoria en diferentes particiones en lugar de mantener los intervalos de tiempo juntos en las particiones. Las consultas que implican tiempo generalmente hacen referencia a rangos de tiempo (por ejemplo los datos más recientes), por lo que esta distribución hash daría lugar a la red sobrecarga.

#### <a name="best-practices"></a>Prácticas recomendadas

-   **No elija una marca de tiempo como columna de distribución.** Elija una columna de distribución diferente. En una aplicación de varios inquilinos, use el identificador del inquilino o en una aplicación en tiempo real, usa el identificador de entidad.
-   **Use PostgreSQL particiones de tabla para el tiempo en su lugar.** Utilice las particiones de tablas para dividir una tabla grande de datos ordenadas en el tiempo en varias tablas heredadas con cada una con diferentes intervalos de tiempo.  Distribución de una tabla con particiones de Postgres en hiperescala crea particiones para las tablas heredadas.

Leer el [timeseries tutorial](https://aka.ms/hyperscale-tutorial-timeseries) para obtener un ejemplo de la creación de este tipo de aplicación.

## <a name="next-steps"></a>Pasos siguientes
- Obtenga información sobre cómo [coubicación](concepts-hyperscale-colocation.md) entre ayuda a los datos distribuidos que las consultas se ejecutan rápidamente
