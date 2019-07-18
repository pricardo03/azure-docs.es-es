---
title: Consulta de datos en Azure Data Lake con Azure Data Explorer
description: Obtenga información sobre cómo consultar datos en Azure Data Lake con Azure Data Explorer.
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/25/2019
ms.openlocfilehash: d6a58d144482e17f7e4b615134115d1da46af6f0
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67454391"
---
# <a name="query-data-in-azure-data-lake-using-azure-data-explorer-preview"></a>Consulta de datos en Azure Data Lake con Azure Data Explorer (versión preliminar)

Azure Data Lake Storage es una solución de lago de datos rentable y muy escalable para el análisis de macrodatos. Combina el potencial de un sistema de archivos de alto rendimiento con escala masiva y economía para ayudarle a obtener conclusiones con rapidez. Data Lake Storage Gen2 amplía la funcionalidad de Azure Blob Storage y está optimizado para cargas de trabajo analíticas.
 
Azure Data Explorer se integra con Azure Blob Storage y Azure Data Lake Storage Gen2, de modo que proporciona un acceso rápido, indexado y almacenado en caché a los datos del lago. Puede analizar y consultar los datos del lago sin una ingesta previa en Azure Data Explorer. También puede consultar al mismo tiempo los datos ingeridos y sin ingerir del lago nativo.  

> [!TIP]
> Para obtener el mejor rendimiento de consultas, se necesita la ingesta de datos en Azure Data Explorer. La funcionalidad para consultar datos en Azure Data Lake Storage Gen2 sin una ingesta previa solo debe usarse para datos históricos o para datos que se consultan con poca frecuencia.
 
## <a name="optimize-query-performance-in-the-lake"></a>Optimización del rendimiento de consultas en el lago 

* Cree particiones de datos para mejorar el rendimiento y optimizar el tiempo de consulta.
* Comprima los datos para potenciar el rendimiento (en formato gzip para una mejor compresión y lz4 para un mejor rendimiento).
* Use Azure Blob Storage o Azure Data Lake Storage Gen2 con la misma región que el clúster de Azure Data Explorer. 

## <a name="create-an-external-table"></a>Creación de una tabla externa

1. Use el comando `.create external table` para crear una tabla externa en Azure Data Explorer. En [Comandos de tabla externa](/azure/kusto/management/externaltables) se documentan otros comandos de este tipo, como `.show`, `.drop` y `.alter`.

    ```Kusto
    .create external table ArchivedProducts(
    Timestamp:datetime,
    ProductId:long, ProductDescription:string) 
    kind=blob
    partition by bin(Timestamp, 1d) 
    dataformat=csv (h@'http://storageaccount.blob.core.windows.net/container1;secretKey') 
    with (compressed = true)  
    ```

    Esta consulta crea particiones diarias *container1/yyyy/MM/dd/all_exported_blobs.csv*. Con una creación de particiones más pormenorizada, se espera un aumento del rendimiento. Por ejemplo, las consultas en tablas externas con particiones diarias (como la anterior) tendrán un mejor rendimiento que las consultas con tablas con particiones mensuales.

    > [!NOTE]
    > Las cuentas de almacenamiento que se admiten actualmente son Azure Blob Storage o Azure Data Lake Storage Gen2, mientras que los formatos de datos compatibles son csv, tsv y txt.

1. La tabla externa está visible en el panel izquierdo de la interfaz de usuario web.

    ![Tabla externa en la interfaz de usuario web](media/data-lake-query-data/external-tables-web-ui.png)
 
### <a name="external-table-permissions"></a>Permisos de tabla externa
 
* El usuario de la base de datos puede crear una tabla externa. El creador de la tabla se convierte automáticamente en el administrador de la tabla.
* El administrador del clúster, la base de datos o la tabla puede editar una tabla existente.
* Cualquier usuario o lector de la base de datos puede consultar una tabla externa.
 
## <a name="query-an-external-table"></a>Consulta de una tabla externa
 
Para consultar una tabla externa, use la función `external_table()` y proporcione el nombre de la tabla como el argumento de función. El resto de la consulta está en lenguaje de consulta Kusto estándar.

```Kusto
external_table("ArchivedProducts") | take 100
```

> [!TIP]
> IntelliSense no se admite actualmente en las consultas de tabla externa.

## <a name="query-external-and-ingested-data-together"></a>Consultar conjuntamente datos externos e ingeridos

Puede usar la misma consulta para consultar tablas externas y tablas de datos ingeridos. Puede usar el operador [`join`](/azure/kusto/query/joinoperator) o [`union`](/azure/kusto/query/unionoperator) para combinar o unir la tabla externa con datos adicionales de Azure Data Explorer, servidores SQL Server u otros orígenes. Use una instrucción de tipo [`let( ) statement`](/azure/kusto/query/letstatement) para asignar un nombre abreviado a una referencia de tabla externa.

En el ejemplo siguiente, *Products* es una tabla de datos ingeridos y *ArchivedProducts* es una tabla externa que contiene datos en Azure Data Lake Storage Gen2:

```kusto
let T1 = external_table("ArchivedProducts") |  where TimeStamp > ago(100d);
let T = Products; //T is an internal table
T1 | join T on ProductId | take 10
```

## <a name="query-taxirides-external-table-in-the-help-cluster"></a>Consulta de la tabla externa *TaxiRides* en el clúster de ayuda

El conjunto de datos de ejemplo *TaxiRides* contiene datos de los taxis de Nueva York tomados de la [Comisión de Taxis y Limusinas de la ciudad de Nueva York](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page).

### <a name="create-external-table-taxirides"></a>Creación de la tabla externa *TaxiRides* 

> [!NOTE]
> En esta sección se describe la consulta empleada para crear la tabla externa *TaxiRides* en el clúster *ayuda*. Puesto que esta tabla ya se ha creado, puede omitir esta sección y realizar la [Consulta de los datos de la tabla externa *TaxiRides*](#query-taxirides-external-table-data). 

1. La siguiente consulta se usó para crear la tabla externa *TaxiRides* en el clúster de ayuda. 

    ```kusto
    .create external table TaxiRides
    (
    trip_id: long,
    vendor_id: string, 
    pickup_datetime: datetime,
    dropoff_datetime: datetime,
    store_and_fwd_flag: string,
    rate_code_id: int,
    pickup_longitude: real,
    pickup_latitude: real,
    dropoff_longitude: real,
    dropoff_latitude: real,
    passenger_count: int,
    trip_distance: real,
    fare_amount: real,
    extra: real,
    mta_tax: real,
    tip_amount: real,
    tolls_amount: real,
    ehail_fee: real,
    improvement_surcharge: real,
    total_amount: real,
    payment_type: string,
    trip_type: int,
    pickup: string,
    dropoff: string,
    cab_type: string,
    precipitation: int,
    snow_depth: int,
    snowfall: int,
    max_temperature: int,
    min_temperature: int,
    average_wind_speed: int,
    pickup_nyct2010_gid: int,
    pickup_ctlabel: string,
    pickup_borocode: int,
    pickup_boroname: string,
    pickup_ct2010: string,
    pickup_boroct2010: string,
    pickup_cdeligibil: string,
    pickup_ntacode: string,
    pickup_ntaname: string,
    pickup_puma: string,
    dropoff_nyct2010_gid: int,
    dropoff_ctlabel: string,
    dropoff_borocode: int,
    dropoff_boroname: string,
    dropoff_ct2010: string,
    dropoff_boroct2010: string,
    dropoff_cdeligibil: string,
    dropoff_ntacode: string,
    dropoff_ntaname: string,
    dropoff_puma: string
    )
    kind=blob 
    partition by bin(pickup_datetime, 1d)
    dataformat=csv
    ( 
    h@'https://externalkustosamples.blob.core.windows.net/taxiridesbyday?st=2019-06-18T14%3A59%3A00Z&se=2029-06-19T14%3A59%3A00Z&sp=rl&sv=2016-05-31&sr=c&sig=yEaO%2BrzFHzAq7lvd4d9PeQ%2BTi3AWnho8Rn8hGU0X30M%3D'
    )
    ```
1. La tabla resultante se creó en el clúster *ayuda*:

    ![Tabla externa TaxiRides](media/data-lake-query-data/taxirides-external-table.png) 

### <a name="query-taxirides-external-table-data"></a>Consulta de los datos de la tabla externa *TaxiRides* 

Inicie sesión en [https://dataexplorer.azure.com/clusters/help/databases/Samples](https://dataexplorer.azure.com/clusters/help/databases/Samples) para consultar la tabla externa *TaxiRides*. 

#### <a name="query-taxirides-external-table-without-partitioning"></a>Consulta de la tabla externa *TaxiRides* sin particiones

[Ejecute esta consulta](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAAx3LSwqAMAwFwL3gHYKreh1xL7F9YrCtElP84OEV9zM4DZo5DsZjhGt6PqWTgL1p6+qhvaTEKjeI/FqyuZbGiwJf63QAi9vEL2UbAhtMEv6jyAH6+VhS9jOr1dULfUgAm2cAAAA=) en la tabla externa *TaxiRides* para mostrar las carreras de los taxis por día de la semana en todo el conjunto de datos. 

```kusto
external_table("TaxiRides")
| summarize count() by dayofweek(pickup_datetime)
| render columnchart
```

Esta consulta muestra el día más ajetreado de la semana. Dado que no se han creado particiones de los datos, esta consulta puede tardar mucho tiempo en devolver resultados (hasta varios minutos).

![Representación de una consulta sin particiones](media/data-lake-query-data/taxirides-no-partition.png)

#### <a name="query-taxirides-external-table-with-partitioning"></a>Consulta de la tabla externa TaxiRides con particiones 

[Ejecute esta consulta](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA13NQQqDMBQE0L3gHT6ukkVF3fQepXv5SQYMNWmIP6ilh68WuinM6jHMYBPkyPMobGao5s6bv3mHpdF19aZ1QgYlbx8ljY4F4gPIQFYgkvqJGrr+eun6I5ralv58OP27t5QQOPsXiOyzRFGazE6WzSh7wtnIiA75uISdOEtdfQDLWmP+ogAAAA==) en la tabla externa *TaxiRides* que muestra los tipos de taxis (amarillos o verdes) que se usaron en enero de 2017. 

```kusto
external_table("TaxiRides")
| where pickup_datetime between (datetime(2017-01-01) .. datetime(2017-02-01))
| summarize count() by cab_type
| render piechart
```

Esta consulta usa particiones, lo que optimiza el rendimiento y el tiempo de consulta. La consulta filtra en función de una columna con particiones (pickup_datetime) y devuelve los resultados en pocos segundos.

![Representación de una consulta con particiones](media/data-lake-query-data/taxirides-with-partition.png)
  
Puede escribir otras consultas para ejecutarlas en la tabla externa *TaxiRides* y obtener más información sobre los datos. 

## <a name="next-steps"></a>Pasos siguientes

Consulte sus datos en Azure Data Lake con Azure Data Explorer. Aprenda a [escribir consultas](write-queries.md) y obtener más información de los datos.