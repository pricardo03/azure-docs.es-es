---
title: Modelos de compra de Azure SQL Database | Microsoft Docs
description: Información sobre el modelo de compra de Azure SQL Database.
services: sql-database
author: CarlRabeler
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: 1a5424b69fc70f69359b12beac86060f4e23ff27
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2018
ms.locfileid: "37083998"
---
# <a name="azure-sql-database-purchasing-models-and-resources"></a>Modelos de compra y recursos de Azure SQL Database 

Los servidores lógicos de [Azure SQL Database](sql-database-technical-overview.md) ofrecen dos modelos de compra para recursos de proceso, almacenamiento y E/S: uno basado en DTU y el otro en núcleos virtuales (que se encuentra en versión preliminar). 

> [!NOTE]
> Las [Instancias administradas](sql-database-managed-instance.md) de Azure SQL Database solo ofrecen el modelo de compra basado en núcleos virtuales.

En la tabla y el gráfico siguientes se comparan y contrastan estos dos modelos de compra.

> [!IMPORTANT]
> Consulte este artículo sobre el [modelo de compra basado en núcleos virtuales (versión preliminar)](sql-database-service-tiers-vcore.md) si desea obtener más información al respecto.

|**Modelo de compra**|**Descripción**|**Más adecuado para**|
|---|---|---|
|Modelo basado en DTU|Este modelo se basa en una medida agrupada de recursos de proceso, almacenamiento y E/S. Los niveles de rendimiento se expresan como unidades de transmisión de datos (DTU) para las bases de datos únicas y como unidades de transmisión de datos elásticas (eDTU) para los grupos de bases de datos elásticas. Para más información sobre las DTU y las eDTU, consulte [¿Qué son las DTU y las eDTU?](sql-database-service-tiers.md#what-are-database-transaction-units-dtus)|Recomendado para los clientes que desean opciones de recursos simples y configuradas previamente.| 
|Modelo basado en núcleos virtuales|Este modelo le permite escalar los recursos de proceso y almacenamiento de manera independiente. También permite usar Ventaja híbrida de Azure para SQL Server para ahorrar en los costos.|Recomendado para los clientes que valoran la flexibilidad, el control y la transparencia.|
||||  

![Modelo de precios](./media/sql-database-service-tiers/pricing-model.png)

## <a name="vcore-based-purchasing-model--preview"></a>Modelo de compra basado en núcleos virtuales (versión preliminar)

Un núcleo virtual representa una CPU lógica que cuenta con una opción para elegir entre varias generaciones de hardware. El modelo de compra basado en núcleos virtuales (versión preliminar) le ofrece flexibilidad, control, transparencia de consumo de recursos individuales y una manera sencilla de trasladar los requisitos de carga de trabajo locales a la nube. Este modelo le permite escalar los recursos de proceso, memoria y almacenamiento en función de las necesidades de carga de trabajo. En el modelo de compra basado en núcleos virtuales (versión preliminar), los clientes pueden elegir entre los niveles de servicio De uso general y Crítico para la empresa (versión preliminar) tanto para [bases de datos únicas](sql-database-single-database-scale.md) como para [grupos elásticos](sql-database-elastic-pool.md). 

El modelo de compra basado en núcleos virtuales (versión preliminar) permite escalar los recursos de proceso y almacenamiento de manera independiente, igualar el rendimiento local y optimizar el precio. Si la base de datos o el grupo elástico consumen más de 300 DTU, la conversión a núcleos virtuales puede reducir los costos. Puede realizar la conversión mediante la API de su elección o usar Azure Portal, sin experimentar tiempo de inactividad. Sin embargo, la conversión no es necesaria. Si el modelo de compra de DTU satisface sus requisitos de rendimiento y empresariales, debe seguir usándolo. Si decide pasar del modelo de DTU al modelo de núcleos virtuales, debe seleccionar el nivel de rendimiento mediante la siguiente regla general: cada 100 DTU del nivel Estándar requieren al menos 1 núcleo virtual en el nivel De uso general, y cada 125 DTU del nivel Premium requieren como mínimo 1 núcleo virtual en el nivel Crítico para la empresa.

En un modelo de compra basado en núcleos virtuales (versión preliminar), los clientes pagan por:
- Proceso (nivel de servicio + número de núcleos virtuales + generación de hardware)*
- Tipo y cantidad de almacenamiento de datos y registros 
- Número de E/S**
- Almacenamiento de copia de seguridad (RA-GRS)** 

\* En la versión preliminar pública inicial, las CPU lógicas de Gen 4 se basan en procesadores Intel E5-2673 v3 (Haswell) a 2,4 GHz.

\*\* Durante la versión preliminar, las copias de seguridad y las E/S son gratuitas durante 7 días.

> [!IMPORTANT]
> Los recursos de proceso, E/S y almacenamiento de datos y registros se cobran por base de datos o grupo elástico. El almacenamiento de copia de seguridad se cobra por cada base de datos. Para más información sobre los precios de Instancia administrada, consulte [Instancia administrada de Azure SQL Database](sql-database-managed-instance.md).
> **Limitaciones de región:** el modelo de compra basado en núcleos virtuales aún no está disponible en las regiones siguientes: Europa Occidental, Centro de Francia, Sur de Reino Unido, Oeste de Reino Unido y Sudeste de Australia.

## <a name="dtu-based-purchasing-model"></a>Modelo de compra basado en DTU

La unidad de rendimiento de base de datos (DTU) representa una medida combinada de CPU, memoria, lecturas y escrituras. El modelo de compra basado en DTU ofrece un conjunto de agrupaciones preconfiguradas de recursos de proceso y almacenamiento incluido para impulsar diferentes niveles de rendimiento de la aplicación. Los clientes que prefieren la simplicidad de una agrupación preconfigurada y pagos fijos cada mes pueden encontrar el modelo basado en DTU más adecuado para sus necesidades. En el modelo de compra basado en DTU, los clientes pueden elegir entre los niveles de servicio **Básico**, **Estándar** y **Premium** tanto para [bases de datos únicas](sql-database-single-database-scale.md) como para [grupos elásticos](sql-database-elastic-pool.md). 

### <a name="what-are-database-transaction-units-dtus"></a>¿Qué son las unidades de transacción de base de datos?
Para una sola base de datos de Azure SQL en un nivel de rendimiento específico dentro de un [nivel de servicio](sql-database-single-database-scale.md), Microsoft garantiza un determinado nivel de recursos para esa base de datos (independiente de cualquier otra base de datos en la nube de Azure), de forma que ofrece un nivel de rendimiento predecible. La cantidad de recursos se calcula como un número de unidades de transacción de base de datos o DTU y es una medida combinada de recursos de proceso, almacenamiento y E/S. La relación entre estos recursos se determinó originalmente mediante una [carga de trabajo OLTP de prueba comparativa](sql-database-benchmark-overview.md) diseñada para representar las típicas cargas de trabajo OLTP reales. Cuando la carga de trabajo supera la cantidad de cualquiera de estos recursos, se limita el rendimiento, con lo que se obtiene un rendimiento y tiempos de espera más lentos. Los recursos usados por la carga de trabajo no afectan a los recursos disponibles para otras bases de datos SQL en la nube de Azure, y los recursos usados por otras cargas de trabajo no afectan a los recursos disponibles para la base de datos SQL.

![rectángulo de selección](./media/sql-database-what-is-a-dtu/bounding-box.png)

Las DTU son especialmente útiles para comprender la cantidad relativa de recursos entre bases de datos de Azure SQL Database en diferentes niveles de rendimiento y niveles de servicio. Por ejemplo, duplicar el número de DTU al aumentar el nivel de rendimiento de una base de datos equivale a duplicar el conjunto de recursos disponibles para esa base de datos. Por ejemplo, una base de datos Premium P11 con 1750 DTU proporciona una potencia de proceso de DTU 350 veces mayor que una base de datos básica con 5 DTU.  

Para obtener información más detallada sobre el consumo de recursos (DTU) de la carga de trabajo, use [Información de rendimiento de consultas de Azure SQL Database](sql-database-query-performance.md) para:

- Identificar las consultas principales por CPU, duración y recuento de ejecuciones, que se pueden ajustar para mejorar el rendimiento. Por ejemplo, una consulta de uso intensivo de E/S puede beneficiarse del uso de [técnicas de optimización en memoria](sql-database-in-memory.md) para hacer un mejor uso de la memoria disponible en un cierto nivel de rendimiento y de nivel de servicio.
- Profundizar en los detalles de una consulta, ver su texto e historial de uso de recursos.
- Tener acceso a recomendaciones de ajuste del rendimiento que muestran las acciones realizadas por el [SQL Database Advisor](sql-database-advisor.md).

Puede [cambiar los niveles de servicio de DTU](sql-database-service-tiers-dtu.md) en cualquier momento con un tiempo de inactividad mínimo para la aplicación (normalmente una media de menos de cuatro segundos). Para muchas empresas y aplicaciones, poder crear bases de datos y aumentar o reducir el rendimiento a petición es suficiente, especialmente si los patrones de uso son relativamente predecibles. Pero si dichos patrones son impredecibles, pueden dificultar la administración de los costos y del modelo de negocio. Para este escenario, se usa un grupo elástico con un determinado número de eDTU que se comparten entre varias bases de datos del grupo.

![Introducción a SQL Database: DTU de bases de datos únicas por nivel](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

### <a name="what-are-elastic-database-transaction-units-edtus"></a>¿Qué son las unidades de transacción de base de datos elásticas (eDTU)?
En lugar de proporcionar un conjunto exclusivo de recursos (DTU) que puede que no siempre sea necesario para una SQL Database que está siempre disponible, puede colocar las bases de datos en un [grupo elástico](sql-database-elastic-pool.md) en un servidor de SQL Database que comparte un grupo de recursos entre esas bases de datos. Los recursos compartidos de un grupo elástico se miden mediante unidades de transacción de bases de datos elásticas o eDTU. Los grupos elásticos proporcionan una solución sencilla y rentable para administrar los objetivos de rendimiento de varias bases de datos que tienen patrones de uso muy diferentes e imprevisibles. Un grupo elástico garantiza que una base de datos del grupo no puede consumir los recursos, mientras que asegura que cada base de datos del grupo tenga disponible siempre una cantidad mínima de recursos necesarios. 

![Introducción a SQL Database: eDTU por servicio y nivel](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

A un grupo se le asigna un número fijo de eDTU, por un precio fijo. Dentro del grupo elástico, a las bases de datos individuales se les proporciona la flexibilidad de escalarse automáticamente dentro de los límites configurados. Una base de datos con cargas más elevadas consumirá más eDTU para satisfacer la demanda. Las bases de datos con cargas más bajas consumirán menos eDTU. Las bases de datos sin cargas no consumirán ningún eDTU. Mediante el aprovisionamiento de recursos para todo el grupo, en lugar de por base de datos, se simplifican las tareas de administración, de forma que se proporciona un presupuesto del grupo predecible.

Se pueden agregar eDTU adicionales a un grupo existente sin que la base de datos experimente tiempo de inactividad ni que las bases de datos del grupo resulten afectadas. De igual forma, si las eDTU adicionales dejan de necesitarse, se pueden quitar de cualquier grupo existente en cualquier momento. Puede agregar o quitar bases de datos del grupo, o limitar la cantidad de eDTU que puede usar una base de datos cuando está sobrecargada para reservar eDTU para otras bases de datos. Si una base de datos infrautiliza recursos de forma predecible, es posible sacarla del grupo y configurarla como una base de datos única con una cantidad predecible de recursos necesarios.

### <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>¿Cómo se puede determinar el número de DTU necesarias para la carga de trabajo?
Si desea migrar una carga de trabajo de máquina virtual existente local o de SQL Server en Azure SQL Database, puede usar la [calculadora de DTU](http://dtucalculator.azurewebsites.net/) para hacer una estimación del número aproximado de DTU que se necesitan. Para una carga de trabajo existente de Azure SQL Database, puede usar la [información de rendimiento de consultas de SQL Database](sql-database-query-performance.md) para comprender el consumo de recursos de la base de datos (DTU) y obtener información más detallada para optimizar la carga de trabajo. También puede usar la DMV [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) para ver el consumo de recursos de la última hora. Como alternativa, la vista de catálogo [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) muestra el consumo de recursos de los últimos 14 días, aunque con una fidelidad inferior media de cinco minutos.

### <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>¿Cómo se puede saber si podría beneficiarme de un grupo elástico de recursos?
Los grupos son apropiados para un amplio número de bases de datos con patrones de utilización específicos. Para una base de datos determinada, este patrón está caracterizado por una utilización media baja con picos de utilización relativamente poco frecuentes. SQL Database evalúa automáticamente el historial de uso de recursos de bases de datos en un servidor de SQL Database existente y recomienda la configuración de grupo apropiada en Azure Portal. Para más información, consulte [¿Cuándo se debe utilizar un grupo elástico?](sql-database-elastic-pool.md)

### <a name="what-happens-when-i-hit-my-maximum-dtus"></a>¿Qué ocurre cuando se alcanza el número máximo de DTU?
Los niveles de rendimiento se calibran y rigen para proporcionar los recursos necesarios para ejecutar la carga de trabajo de la base de datos hasta el máximo permitido para el nivel de rendimiento o de servicio seleccionado. Si la carga de trabajo alcanza uno de los límites de CPU, E/S de datos o E/S de registro, seguirá recibiendo el nivel máximo de recursos permitido, pero es probable que perciba latencias de consultas aumentadas. Alcanzar uno de estos límites no provocará errores, sino una ralentización de la carga de trabajo, a menos que la ralentización sea tan severa que las consultas empiecen a agotar el tiempo de espera. Si alcanza las sesiones y solicitudes de usuario simultáneas máximas permitidas (subprocesos de trabajo), verá errores explícitos. Consulte [Límites de recursos de Azure SQL Database]( sql-database-resource-limits.md#what-happens-when-database-resource-limits-are-reached) para obtener información sobre los límites de recursos no relacionados con la CPU, la memoria, la E/S de datos o la E/S del registro de transacciones.

### <a name="correlating-benchmark-results-to-real-world-database-performance"></a>Correlación de los resultados de la prueba comparativa con el rendimiento real de la base de datos
Es importante comprender que todas las pruebas comparativas solo son representativas e indicativas. Las velocidades de transacción logradas con la aplicación de la prueba comparativa no serán iguales que las que se podrían lograr con otras aplicaciones. La prueba comparativa comprende un conjunto de diferentes tipos de transacción ejecutados en un esquema que contiene una variedad de tipos de datos y tablas. Si bien la prueba comparativa ejerce las mismas operaciones básicas que son comunes para todas las cargas de trabajo OLTP, no representa ninguna clase específica de base de datos o aplicación. El objetivo de la prueba comparativa es proporcionar una orientación razonable del rendimiento relativo de una base de datos que se puede esperar al aumentar o reducir el nivel de rendimiento. En realidad, las bases de datos son de distintos tamaños y complejidad, tienen distintas combinaciones de cargas de trabajo y responden de maneras diferentes. Por ejemplo, una aplicación que haga un uso intensivo de ES podría alcanzar antes el umbral de ES, o una que haga un uso intensivo de la CPU podría alcanzar antes los límites de CPU. No se garantiza que una base de datos concreta se escale de la misma manera que la prueba comparativa bajo una carga creciente.

La prueba comparativa y su metodología se describen a continuación de forma más detallada.

### <a name="benchmark-summary"></a>Resumen de la prueba comparativa
El ASDB mide el rendimiento de una combinación de operaciones de bases de datos básicas que se producen con mayor frecuencia en las cargas de trabajo de procesamiento de transacciones en línea (OLTP). Aunque la prueba comparativa está diseñada teniendo en cuenta la computación en la nube, el esquema de la base de datos, el rellenado de datos y las transacciones se diseñaron para representar ampliamente los elementos básicos usados con mayor frecuencia en las cargas de trabajo OLTP.

### <a name="schema"></a>Esquema
El esquema se ha diseñado para que presente una variedad y complejidad suficientes como para permitir una amplia gama de operaciones. La prueba comparativa se ejecuta en una base de datos formada por seis tablas. Las tablas pertenecen a tres categorías: de tamaño fijo, de escalado y de crecimiento. Existen dos tablas de tamaño fijo, tres tablas de escalado y una tabla de crecimiento. Las tablas de tamaño fijo tienen un número de filas constante. Las tablas de escalado presentan una cardinalidad proporcional al rendimiento de la base de datos, pero no cambian durante la prueba comparativa. La tabla de crecimiento tiene un tamaño igual que la tabla de escalado en la carga inicial, pero después la cardinalidad cambia durante el transcurso de la prueba comparativa según se van insertando y eliminando filas.

El esquema incluye una combinación de tipos de datos que incluyen valores enteros, numéricos, caracteres y fecha/hora. El esquema incluye claves principales y secundarias, pero no claves externas; es decir, no hay restricciones de integridad referenciales entre las tablas.

Un programa de generación de datos genera los datos para la base de datos inicial. Los datos enteros y numéricos se generan con diversas estrategias. En algunos casos, los valores se distribuyen al azar a lo largo de un intervalo. En otros casos, se permuta al azar un conjunto de valores para asegurarse de que se mantiene una distribución específica. Los campos de texto se generan a partir de una lista ponderada de palabras para producir datos con aspecto real.

La base de datos se dimensiona basándose en un “factor de escala”. El factor de escala (abreviado SF) determina la cardinalidad de las tablas de escalado y de crecimiento. Como se describe a continuación en la sección Usuarios y velocidad, el tamaño de la base de datos, el número de usuarios y el rendimiento máximo se escalan de modo proporcional entre sí.

### <a name="transactions"></a>Transacciones
La carga de trabajo consta de nueve tipos de transacciones, como se muestra en la tabla siguiente. Cada transacción se diseño para destacar un conjunto determinado de características del sistema en el motor de la base de datos y en el hardware del sistema, con un elevado contraste con respecto a las otras transacciones. Este enfoque facilita la evaluación del impacto de diferentes componentes sobre el rendimiento global. Por ejemplo, la transacción “Lectura intensa” produce un número significativo de operaciones de lectura de disco.

| Tipo de transacción | DESCRIPCIÓN |
| --- | --- |
| Lectura ligera |SELECT; en memoria; solo lectura |
| Lectura mediana |SELECT; principalmente en memoria; solo lectura |
| Lectura intensa |SELECT; principalmente no en memoria; solo lectura |
| Actualización ligera |UPDATE; en memoria; solo escritura |
| Actualización intensa |UPDATE; principalmente no en memoria; solo escritura |
| Inserción ligera |INSERT; en memoria; solo escritura |
| Inserción intensa |INSERT; principalmente no en memoria; solo escritura |
| Eliminar |DELETE; combinación de en memoria y no en memoria; solo lectura |
| CPU intensa |SELECT; en memoria; carga en CPU relativamente intensa; solo lectura |

### <a name="workload-mix"></a>Combinación de cargas de trabajo
Las transacciones se seleccionan aleatoriamente de una distribución ponderada con la siguiente combinación global. La combinación global presenta una relación de lectura/escritura aproximadamente de 2:1.

| Tipo de transacción | % de combinación |
| --- | --- |
| Lectura ligera |35 |
| Lectura mediana |20 |
| Lectura intensa |5 |
| Actualización ligera |20 |
| Actualización intensa |3 |
| Inserción ligera |3 |
| Inserción intensa |2 |
| Eliminar |2 |
| CPU intensa |10 |

### <a name="users-and-pacing"></a>Usuarios y velocidad
La carga de trabajo de la prueba comparativa está dirigida a partir de una herramienta que envía transacciones a través de un conjunto de conexiones para simular el comportamiento de numerosos usuarios simultáneos. Aunque todas las conexiones y transacciones son generadas a máquina, para simplificar nos referiremos a estas conexiones como “usuarios”. Aunque cada usuario opera independientemente de todos los demás usuarios, todos los usuarios realizan el mismo ciclo de pasos mostrado a continuación:

1. Establecer una conexión de base de datos.
2. Repetir hasta que se señale la salida:
   * Seleccionar una transacción aleatoriamente (a partir de una distribución ponderada).
   * Realizar la transacción seleccionada y medir el tiempo de respuesta.
   * Esperar un retraso de velocidad.
3. Cerrar la conexión de la base de datos.
4. Salir.

El retraso de velocidad (en el paso 2c) se selecciona aleatoriamente, pero con una distribución que tenga un promedio de 1,0 segundos. De este modo, cada usuario puede, en promedio, generar como máximo una transacción por segundo.

### <a name="scaling-rules"></a>Reglas de escalado
El número de usuarios viene determinado por el tamaño de la base de datos (en unidades de factor de escala). Hay un usuario por cada cinco unidades de factor de escala. Debido al retraso de velocidad, un usuario puede, en promedio, generar como máximo una transacción por segundo.

Por ejemplo, una base de datos que tenga un factor de escala 500 (SF=500) tendrá 100 usuarios y podrá alcanzar una velocidad máxima de 100 TPS. Para generar una velocidad de TPS mayor, son necesarios más usuarios y una base de datos mayor.

La tabla siguiente muestra el número de usuarios sostenidos realmente para cada nivel de servicio y nivel de rendimiento.

| Nivel de servicio (nivel de rendimiento) | Usuarios | Tamaño de base de datos |
| --- | --- | --- |
| Básica |5 |720 MB |
| Estándar (S0) |10 |1 GB |
| Estándar (S1) |20 |2,1 GB |
| Estándar (S2) |50 |7,1 GB |
| Premium (P1) |100 |14 GB |
| Premium (P2) |200 |28 GB |
| Premium (P6) |800 |114 GB |

### <a name="measurement-duration"></a>Duración de la medición
Una ejecución válida de la prueba comparativa precisa una duración de medición en estado fijo de al menos una hora.

### <a name="metrics"></a>Métricas
Las métricas clave de la prueba comparativa son rendimiento y tiempo de respuesta.

* El rendimiento es la medición de rendimiento esencial en la prueba comparativa. El rendimiento se indica en transacciones por unidad de tiempo, contando todos los tipos de transacciones.
* El tiempo de respuesta es una medición de la previsibilidad del rendimiento. La restricción del tiempo de respuesta varía con la clase de servicio, presentando las clases de servicio mayores un requisito de tiempo de respuesta más estricto, como se muestra a continuación.

| Clase de servicio | Medición del rendimiento | Requisito del tiempo de respuesta |
| --- | --- | --- |
| Premium |Transacciones por segundo |Percentil 95 en 0,5 segundos |
| Estándar |Transacciones por minuto |Percentil 90 en 1,0 segundo |
| Básica |Transacciones por hora |Percentil 80 en 2,0 segundos |

## <a name="next-steps"></a>Pasos siguientes

- Consulte este artículo sobre el [modelo de compra basado en núcleos virtuales (versión preliminar)](sql-database-service-tiers-vcore.md) si desea obtener más información al respecto.
- Con respecto al modelo de compra basado en DTU, consulte [Modelo de compra basado en DTU](sql-database-service-tiers-dtu.md).
