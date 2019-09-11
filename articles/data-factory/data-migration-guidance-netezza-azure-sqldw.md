---
title: Uso de Azure Data Factory para migrar datos de un servidor de Netezza local a Azure| Microsoft Docs
description: Use Azure Data Factory para migrar datos de un servidor de Netezza local a Azure.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 9/03/2019
ms.openlocfilehash: 4690fd81247035267861b06c204c6db7a052eba5
ms.sourcegitcommit: 267a9f62af9795698e1958a038feb7ff79e77909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70259460"
---
# <a name="use-azure-data-factory-to-migrate-data-from-on-premises-netezza-server-to-azure"></a>Uso de Azure Data Factory para migrar datos de un servidor de Netezza local a Azure 

Azure Data Factory proporciona un mecanismo eficaz, sólido y rentable para migrar datos a escala desde un servidor de Netezza local a Azure Storage o Azure SQL Data Warehouse. En este artículo se proporciona la siguiente información para ingenieros de datos y desarrolladores:

> [!div class="checklist"]
> * Rendimiento 
> * Resistencia de copia
> * Seguridad de las redes
> * Arquitectura de soluciones de alto nivel 
> * Procedimientos recomendados para la implementación  

## <a name="performance"></a>Rendimiento

Azure Data Factory ofrece una arquitectura sin servidor que permite paralelismo en diferentes niveles, lo que permite a los desarrolladores crear canalizaciones para aprovechar al máximo el ancho de banda de red, así como el ancho de banda de la base de datos para maximizar el rendimiento del movimiento de datos para su entorno.

![rendimiento](media/data-migration-guidance-netezza-azure-sqldw/performance.png)

- Una única actividad de copia puede aprovechar recursos de proceso escalables: si usa Azure Integration Runtime, puede especificar [hasta 256 DIU](https://docs.microsoft.com/azure/data-factory/copy-activity-performance#data-integration-units) para cada actividad de copia en modo sin servidor; si usa un entorno de ejecución de integración autohospedado, puede escalar verticalmente la máquina o escalar horizontalmente a varias máquinas ([hasta 4 nodos](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime#high-availability-and-scalability)) de manera manual y una única actividad de copia distribuirá su partición en todos los nodos. 
- Una única actividad de copia lee y escribe en el almacén de datos mediante varios subprocesos. 
- El flujo de control de Azure Data Factory puede iniciar varias actividades de copia en paralelo, por ejemplo, mediante un [bucle ForEach](https://docs.microsoft.com/azure/data-factory/control-flow-for-each-activity). 

Puede obtener más detalles en la [guía de rendimiento de la actividad de copia](https://docs.microsoft.com/azure/data-factory/copy-activity-performance).

## <a name="resilience"></a>Resistencia

Dentro de una ejecución de una única actividad de copia, Azure Data Factory tiene un mecanismo de reintento integrado para que pueda controlar cierto nivel de errores transitorios en los almacenes de datos o en la red subyacente.

La actividad de copia de Azure Data Factory también ofrece dos maneras para tratar con filas incompatibles cuando se copian datos entre almacenes de datos de origen y de receptor. Puede anular y suspender la actividad de copia cuando se encuentren datos incompatibles o continuar copiando los datos en reposo omitiendo las filas de datos incompatibles. Además, puede registrar las filas incompatibles en Azure Blob Storage o Azure Data Lake Store para conocer la causa del error, corregir los datos en el origen de datos y reintentar la actividad de copia.

## <a name="network-security"></a>Seguridad de las redes 

De manera predeterminada, Azure Data Factory transfiere datos del servidor local de Netezza a Azure Storage o Azure SQL Data Warehouse mediante una conexión cifrada a través del protocolo HTTPS. Proporciona cifrado de datos en tránsito y evita la interceptación y ataques de tipo "Man in the middle".

Como alternativa, si no desea que los datos se transfieran a través de la red pública de Internet, puede lograr una mayor seguridad mediante la transferencia de datos a través de un vínculo de emparejamiento privado vía Azure Express Route. Consulte la arquitectura de la solución siguiente para obtener información sobre cómo lograr esto.

## <a name="solution-architecture"></a>Arquitectura de la solución

Migración de datos a través de la red pública de Internet:

![solución-arquitectura-red-pública](media/data-migration-guidance-netezza-azure-sqldw/solution-architecture-public-network.png)

- En esta arquitectura, los datos se transfieren de forma segura mediante HTTPS a través de la red pública de Internet.
- Debe instalar el entorno de ejecución de integración autohospedado de Azure Data Factory en una máquina Windows detrás del firewall corporativo para lograr esta arquitectura. Asegúrese de que el entorno de ejecución de integración autohospedado de Azure Data Factory en una máquina Windows puede obtener acceso directamente a su servidor de Netezza. Puede escalar verticalmente la máquina o escalar horizontalmente a varias máquinas para usar totalmente la red y el ancho de banda de los almacenes de datos para copiar los datos.
- Tanto la migración de datos de la instantánea inicial como la migración de datos diferencial se pueden realizar con esta arquitectura.

Migración de datos a través de un vínculo privado: 

![solución-arquitectura-red-privada](media/data-migration-guidance-netezza-azure-sqldw/solution-architecture-private-network.png)

- En esta arquitectura, la migración de datos se realiza a través de un vínculo de emparejamiento privado vía Azure Express Route, de modo que los datos nunca atraviesan la red pública de Internet. 
- Debe instalar el entorno de ejecución de integración autohospedado de Azure Data Factory en una máquina virtual de Windows dentro de la red virtual de Azure para lograr esta arquitectura. Puede escalar verticalmente las máquinas virtuales o escalar horizontalmente a varias máquinas virtuales para usar totalmente la red y el ancho de banda de los almacenes de datos para copiar los datos.
- Tanto la migración de datos de la instantánea inicial como la migración de datos diferencial se pueden realizar con esta arquitectura.

## <a name="implementation-best-practices"></a>Procedimientos recomendados para la implementación 

### <a name="authentication-and-credential-management"></a>Autenticación y administración de identidades 

- Para autenticarse en Netezza, puede usar [la autenticación ODBC a través de la cadena de conexión](https://docs.microsoft.com/azure/data-factory/connector-netezza#linked-service-properties). 
- Se admiten varios tipos de autenticación para conectarse a Azure Blob Storage.  Se recomienda encarecidamente el uso de [identidades administradas para recursos de Azure](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#managed-identity): sobre la base de una identidad de Azure Data Factory administrada de forma automática en Azure AD, permite configurar canalizaciones sin proporcionar credenciales en la definición del servicio vinculado.  Como alternativa, puede autenticarse en Azure Blob Storage mediante la [entidad de servicio](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#service-principal-authentication), la [firma de acceso compartido](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#shared-access-signature-authentication) o la [clave de la cuenta de almacenamiento](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#account-key-authentication). 
- También se admiten varios tipos de autenticación para conectarse a Azure Data Lake Storage Gen2.  Se recomienda encarecidamente el uso de [identidades administradas para recursos de Azure](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#managed-identity), aunque también se puede usar la [entidad de servicio](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#service-principal-authentication) o la [clave de la cuenta de almacenamiento](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#account-key-authentication). 
- También se admiten varios tipos de autenticación para conectarse a Azure SQL Data Warehouse. Se recomienda encarecidamente el uso de [identidades administradas para recursos de Azure](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-data-warehouse#managed-identity), aunque también se puede usar la [entidad de servicio](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-data-warehouse#service-principal-authentication) o la [autenticación de SQL](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-data-warehouse#sql-authentication).
- Si no usa identidades administradas para recursos de Azure, es muy recomendable [almacenar las credenciales en Azure Key Vault](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault) para facilitar la administración y la rotación de las claves de forma centralizada sin modificar los servicios vinculados de Azure Data Factory.  Este es también uno de los [procedimientos recomendados para CI/CD](https://docs.microsoft.com/azure/data-factory/continuous-integration-deployment#best-practices-for-cicd). 

### <a name="initial-snapshot-data-migration"></a>Migración de datos de instantánea inicial 

En el caso de las tablas pequeñas en las que su tamaño de volumen es inferior a 100 GB, o que se pueden migrar a Azure en un período de 2 horas, puede hacer que cada trabajo de copia cargue los datos por tabla. Puede ejecutar varios trabajos de copia de Azure Data Factory para cargar distintas tablas de manera simultánea para mejorar el rendimiento. 

Dentro de cada trabajo de copia, también puede alcanzar algún nivel de paralelismo mediante el uso de la [configuración parallelCopies](https://docs.microsoft.com/azure/data-factory/copy-activity-performance#parallel-copy) con la opción de partición de datos para ejecutar consultas paralelas y copiar datos por particiones. Hay dos opciones de partición de datos que se van a elegir con los detalles siguientes.
- Se recomienda comenzar desde el segmento de datos, ya que es más eficaz.  Asegúrese de que el número de paralelismo de la configuración parallelCopies esté por debajo del número total de particiones de segmentos de datos de la tabla en el servidor de Netezza.  
- Si el tamaño del volumen de cada partición de segmento de datos sigue siendo grande (por ejemplo, mayor de 10 GB), se recomienda cambiar a la partición dinámica por rango, donde tendrá más flexibilidad para definir el número de particiones y el tamaño del volumen de cada partición por columna de partición, límite superior y límite inferior.

En el caso de las tablas grandes en las que el tamaño de volumen es superior a 100 GB, o que no se pueden migrar a Azure en un período de 2 horas, se recomienda particionar los datos por consulta personalizada y, luego, hacer que cada trabajo de copia copie una partición a la vez. Puede ejecutar varios trabajos de copia de Azure Data Factory simultáneamente para mejorar el rendimiento. Tenga en cuenta que, para que cada destino de trabajo de copia cargue una partición por consulta personalizada, todavía puede habilitar el paralelismo a través de un segmento de datos o de un intervalo dinámico para aumentar el rendimiento. 

Si se produce un error en alguno de los trabajos de copia debido a una incidencia transitoria de la red o del almacén de datos, puede volver a ejecutar el trabajo de copia con errores para volver a cargar esa partición específica desde la tabla. El resto de los trabajos de copia que cargan otras particiones no se verán afectados.

Al cargar datos en Azure SQL Data Warehouse, se sugiere que Polybase esté habilitado en el trabajo de copia con una instancia de Azure Blob Storage como almacenamiento provisional.

### <a name="delta-data-migration"></a>Migración de datos diferencial 

La manera de identificar las filas nuevas o actualizadas de la tabla es usar una columna de marca de tiempo o una clave de incremento dentro del esquema y, a continuación, almacenar el valor más reciente como límite máximo de una tabla externa, que se puede usar para filtrar los datos diferenciales para la próxima carga de datos. 

Las distintas tablas pueden usar una columna de marca de agua diferente para identificar las filas nuevas o actualizadas. Se recomienda crear una tabla de control externa en la que cada fila represente una tabla en el servidor de Netezza con su nombre de columna de marca de agua específico y el valor de límite máximo. 

### <a name="self-hosted-integration-runtime-configuration-on-azure-vm-or-machine"></a>Entorno de ejecución de integración autohospedado en un equipo o una máquina virtual de Azure

Dado que va a migrar datos desde un servidor de Netezza a Azure, independientemente de que el servidor de Netezza esté en el entorno local detrás del firewall corporativo o dentro de un entorno de red virtual, debe instalar el entorno de ejecución de integración autohospedado en el equipo Windows o en la máquina virtual, que es el motor para mover los datos.

- La configuración recomendada con la cual empezar por cada equipo o máquina virtual es de 32 vCPU y 128 GB de memoria. Puede seguir supervisando el uso de la CPU y la memoria de la máquina del entorno de ejecución de integración durante la migración de datos para ver si necesita escalar verticalmente la máquina para mejorar el rendimiento o reducir verticalmente la máquina para ahorrar costos.
- También puede escalar horizontalmente asociando hasta 4 nodos con un solo entorno de ejecución de integración autohospedado. Un único trabajo de copia que se ejecute en un entorno de ejecución de integración autohospedado aprovechará automáticamente todos los nodos de máquina virtual para copiar los datos en paralelo. Para lograr una alta disponibilidad, se recomienda empezar con 2 nodos de máquina virtual para evitar un único punto de error durante la migración de datos.

### <a name="rate-limiting"></a>Limitación de frecuencia

Como procedimiento recomendado, lleve a cabo una prueba de concepto de rendimiento con un conjunto de datos de ejemplo representativo, a fin de determinar el tamaño de partición adecuado para cada actividad de copia. Se recomienda que cada partición se cargue en Azure dentro de un período de 2 horas.  

Comience con una actividad de copia única con una sola máquina de entorno de ejecución de integración autohospedado para copiar una tabla. Aumente gradualmente el valor de parallelCopies en función del número de particiones de segmentos de datos de la tabla y compruebe si toda la tabla se puede cargar en Azure en un plazo de 2 horas, según el rendimiento que vea en el trabajo de copia. 

Si no se puede lograr y, al mismo tiempo, la capacidad del nodo del entorno de ejecución de integración autohospedado y del almacén de datos no se usan por completo, aumente de manera gradual el número de actividades de copia simultáneas hasta alcanzar los límites de la red o el límite de ancho de banda de los almacenes de datos. 

Siga supervisando el uso de la CPU y la memoria en la máquina del entorno de ejecución de integración autohospedado y prepárese para escalar verticalmente la máquina o escalar horizontalmente a varias máquinas cuando vea que la CPU o la memoria se usan por completo. 

Si encuentra errores de limitación notificados por la actividad de copia de Azure Data Factory, reduzca el valor de simultaneidad o parallelCopies en Azure Data Factory o considere la posibilidad de aumentar los límites de ancho de banda/IOPS de la red y los almacenes de datos. 


### <a name="estimating-price"></a>Estimación del precio 

Considere la canalización siguiente construida para migrar datos de un servidor local de Netezza a Azure SQL Data Warehouse:

![precios-canalización](media/data-migration-guidance-netezza-azure-sqldw/pricing-pipeline.png)

Supongamos lo siguiente: 

- El volumen total de datos es de 50 TB. 
- Migración de los datos con la primera arquitectura de la solución (el servidor de Netezza está en el entorno local detrás del firewall)
- Los 50 TB se dividen en 500 particiones y cada actividad de copia mueve una partición.
- Cada actividad de copia se configura con un IR autohospedado en 4 máquinas y logra un rendimiento de 20 MBps. (Dentro de la actividad de copia, parallelCopies se establece en 4 y cada subproceso para cargar datos de la tabla alcanza un rendimiento de 5 MBps).
- La simultaneidad de ForEach se establece en 3 y el rendimiento agregado es de 60 MBps.
- En total, la migración tarda 243 horas en completarse.

Este es el precio estimado en función de las suposiciones anteriores: 

![precios-tabla](media/data-migration-guidance-netezza-azure-sqldw/pricing-table.png)

> [!NOTE]
> Este es un ejemplo de precios hipotético. Los precios reales dependen del rendimiento real de su entorno. No se incluye el precio de la máquina Windows (con entorno de ejecución de integración autohospedado instalado). 

### <a name="additional-references"></a>Referencias adicionales 
- [Migración de datos del almacenamiento de datos relacional local a Azure mediante Azure Data Factory](https://azure.microsoft.com/mediahandler/files/resourcefiles/data-migration-from-on-premise-relational-data-warehouse-to-azure-data-lake-using-azure-data-factory/Data_migration_from_on-prem_RDW_to_ADLS_using_ADF.pdf)
- [Conector de Netezza](https://docs.microsoft.com/azure/data-factory/connector-netezza)
- [Conector ODBC](https://docs.microsoft.com/azure/data-factory/connector-odbc)
- [Conector de Azure Blob Storage](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage)
- [Conector de Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage)
- [Copia de datos con Azure SQL Data Warehouse como origen o destino mediante Azure Data Factory](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-data-warehouse)
- [Guía de optimización y rendimiento de la actividad de copia](https://docs.microsoft.com/azure/data-factory/copy-activity-performance)
- [Creación y configuración de un entorno de ejecución de integración autohospedado](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime)
- [Alta disponibilidad y escalabilidad de un entorno de ejecución de integración autohospedado](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime#high-availability-and-scalability)
- [Consideraciones sobre la seguridad del movimiento de datos](https://docs.microsoft.com/azure/data-factory/data-movement-security-considerations)
- [Almacenamiento de credenciales en Azure Key Vault](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault)
- [Copia de datos incremental desde una tabla](https://docs.microsoft.com/azure/data-factory/tutorial-incremental-copy-portal)
- [Copia de datos incremental desde varias tablas](https://docs.microsoft.com/azure/data-factory/tutorial-incremental-copy-multiple-tables-portal)
- [Página de precios de Azure Data Factory](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/)

## <a name="next-steps"></a>Pasos siguientes

- [Copia de archivos de varios contenedores con Azure Data Factory](solution-template-copy-files-multiple-containers.md)