---
title: Uso de Azure Data Factory para migrar datos de un clúster de Hadoop local a Azure Storage | Microsoft Docs
description: Use Azure Data Factory para migrar datos de un clúster de Hadoop local a Azure Storage.
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
ms.date: 8/30/2019
ms.openlocfilehash: 1b26b22fa9cdf9f6671702ceb9640aa39a6ecf17
ms.sourcegitcommit: 8fea78b4521921af36e240c8a92f16159294e10a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2019
ms.locfileid: "70211598"
---
# <a name="use-azure-data-factory-to-migrate-data-from-on-premises-hadoop-cluster-to-azure-storage"></a>Uso de Azure Data Factory para migrar datos de un clúster de Hadoop local a Azure Storage 

Azure Data Factory proporciona un mecanismo eficaz, sólido y rentable para migrar datos a escala desde una versión local de HDFS a Azure Blob Storage o Azure Data Lake Storage Gen2. Básicamente, Azure Data Factory contiene dos enfoques para migrar datos desde una versión local de HDFS a Azure. El enfoque que se seleccione dependerá del escenario. 

- Modo DistCp de ADF. ADF admite el uso de [DistCp](https://hadoop.apache.org/docs/current3/hadoop-distcp/DistCp.html) para copiar archivos tal cual están en Azure Blob (incluidas las [copias almacenadas provisionalmente](https://docs.microsoft.com/azure/data-factory/copy-activity-performance#staged-copy)) o en Azure Data Lake Store, en cuyo caso puede aprovechar plenamente la potencia del clúster, en lugar de ejecutarse en el entorno de ejecución de integración autohospedado (IR). De este modo, el rendimiento de la copia será mayor, sobre todo cuando el clúster sea muy potente. En función de la configuración de Azure Data Factory, la actividad de copia crea automáticamente un comando DistCp, lo envía a un clúster de Hadoop y supervisa el estado de la copia. Mediante el uso de ADF integrado con DistCp, el cliente no solo puede aprovechar un potente clúster existente para lograr el mejor rendimiento de la copia, sino también obtener el beneficio de una programación flexible y una experiencia de supervisión unificada desde ADF. De forma predeterminada, el modo DistCp de ADF es la forma recomendada de migrar datos desde un clúster de Hadoop local a Azure.
- Modo IR nativo de ADF. En algunos escenarios, es posible que DistCp no funcione (por ejemplo, en un entorno de red virtual, la herramienta DistCp no admite el emparejamiento privado de ExpressRoute y el punto de conexión de red virtual de Azure Storage). Además, en algunas ocasiones no se desea usar el clúster de Hadoop existente como motor para migrar los datos, ya que se genera pesadas cargas en el clúster, lo que puede afectar al rendimiento de los trabajos ETL existentes. En ese caso, puede usar la funcionalidad nativa de ADF, donde el entorno de ejecución de integración (IR) de ADF puede ser el motor para copiar los datos desde la versión local de HDFS a Azure.

En este artículo se proporciona la siguiente información de ambos enfoques para ingenieros de datos y desarrolladores:
> [!div class="checklist"]
> * Rendimiento 
> * Resistencia de copia
> * Seguridad de las redes
> * Arquitectura de soluciones de alto nivel 
> * Procedimientos recomendados para la implementación  

## <a name="performance"></a>Rendimiento

En el modo DistCp de ADF, el rendimiento es el mismo que si se usa la herramienta DistCp de forma independiente, que aprovecha la funcionalidad del clúster de Hadoop existente. DistCp (copia distribuida) es una herramienta que se usa para la copia de grandes cantidades de datos dentro de clústeres y entre clústeres. Utiliza MapReduce para llevar a cabo la distribución, el control y recuperación de errores, y la generación de informes. Expande una lista de archivos y directorios en la entrada para asignar tareas, cada una de las cuales copiará una partición de los archivos especificados en la lista de origen. Mediante el uso de ADF integrado con DistCp, es posible crear canalizaciones que usen para usar completamente el ancho de banda de red, así como IOPS y ancho de banda de almacenamiento para maximizar el rendimiento del movimiento de datos en su entorno.  

En el modo de IR nativo de ADF, también permite el paralelismo en diferentes niveles, lo que puede usar completamente el ancho de banda de red, así como IOPS y ancho de banda de almacenamiento para maximizar el rendimiento del movimiento de datos mediante el escalado vertical del equipo del entorno de ejecución de integración autohospedado o el escalado horizontal del entorno de ejecución de integración autohospedado.

- Una sola actividad de copia puede aprovechar los recursos de proceso escalables. Con IR autohospedado, puede escalar verticalmente el equipo o realizar el escalado horizontal a varias máquinas ([hasta 4 nodos](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime#high-availability-and-scalability)) y una única actividad de copia creará particiones del conjunto de archivos en todos los nodos. 
- Una única actividad de copia lee y escribe en el almacén de datos mediante varios subprocesos. 
- El flujo de control de ADF puede iniciar varias actividades de copia en paralelo, por ejemplo, mediante un [bucle ForEach](https://docs.microsoft.com/azure/data-factory/control-flow-for-each-activity). 

En [Guía de escalabilidad y rendimiento de la actividad de copia](https://docs.microsoft.com/azure/data-factory/copy-activity-performance) encontrará más información al respecto

## <a name="resilience"></a>Resistencia

En el modo DistCp de ADF, puede usar diferentes parámetros de la línea de comandos de DistCp (por ejemplo,-i, omitir errores;-update, escribir datos cuando el tamaño del archivo de origen no es el mismo que el de destino) para lograr distintos niveles de resistencia.

En el modo de IR nativo de ADF, dentro de una ejecución de una única actividad de copia, ADF tiene un mecanismo de reintento integrado para que pueda controlar cierto nivel de errores transitorios en los almacenes de datos o en la red subyacente. Al realizar copias binarias de la versión local de HDFS a blob y de la versión local de HDFS a ADLS Gen2, ADF incluye automáticamente puntos de control en mayor medida. Si se ha producido un error en la ejecución de la actividad de copia o se ha agotado el tiempo de espera, en un reintento posterior (asegúrese de que el número de reintentos > 1), la copia se reanuda desde el último punto de error en lugar de comenzar desde el principio.

## <a name="network-security"></a>Seguridad de las redes 

De forma predeterminada, ADF transfiere datos desde la versión local de HDFS a Azure Blob Storage o Azure Data Lake Storage Gen2 mediante una conexión cifrada a través del protocolo HTTPS.  HTTPS proporciona cifrado de datos en tránsito y evita la interceptación y ataques de tipo "Man in the middle". 

Como alternativa, si no desea que los datos se transfieran a través de la red pública de Internet, puede lograr una mayor seguridad mediante la transferencia de datos a través de un vínculo de emparejamiento privado vía Azure ExpressRoute. Consulte la arquitectura de la solución siguiente para obtener información sobre cómo lograr esto.

## <a name="solution-architecture"></a>Arquitectura de la solución

Migración de datos a través de la red pública de Internet:

![solución-arquitectura-red-pública](media/data-migration-guidance-hdfs-to-azure-storage/solution-architecture-public-network.png)

- En esta arquitectura, los datos se transfieren de forma segura mediante HTTPS a través de la red pública de Internet. 
- Se recomienda usar el modo DistCp de ADF en el entorno de red pública. Al hacerlo, el cliente no solo puede aprovechar un potente clúster existente para lograr el mejor rendimiento de la copia, sino también obtener el beneficio de una programación flexible y una experiencia de supervisión unificada desde ADF.
- Es preciso instalar un IR autohospedado de ADF en un equipo Windows detrás del firewall corporativo para enviar el comando DistCp al clúster de Hadoop y supervisar el estado de la copia. Como este equipo no será el motor para mover datos (solo se usa con fines de control), su capacidad no afecta al rendimiento del movimiento de datos.
- Se admiten los parámetros existentes del comando DistCp.


Migración de datos a través de un vínculo privado: 

![solución-arquitectura-red-privada](media/data-migration-guidance-hdfs-to-azure-storage/solution-architecture-private-network.png)

- En esta arquitectura, la migración de datos se realiza a través de un vínculo de emparejamiento privado mediante Azure ExpressRoute, de modo que los datos nunca atraviesan la red pública de Internet.
- La herramienta DistCp no admite el emparejamiento privado de ExpressRoute y un punto de conexión de red virtual de Azure Storage, por lo que se recomienda usar la funcionalidad nativa de ADF mediante Integration Runtime para migrar los datos.
- Debe instalar el entorno de ejecución de integración autohospedado de ADF en una máquina virtual de Windows dentro de la red virtual de Azure para lograr esta arquitectura. Puede escalar verticalmente una máquina virtual o escalar horizontalmente a varias máquinas virtuales manualmente para utilizar la totalidad del ancho de banda/IOPS de almacenamiento y de la red.
- La configuración recomendada con la que empezar en cada máquina virtual de Azure (con IR autohospedado de ADF) es Standard_D32s_v3 con 32 vCPU y 128 GB de memoria. Puede seguir supervisando el uso de la CPU y de la memoria de la máquina virtual durante la migración de datos para ver si necesita escalar verticalmente la máquina virtual para mejorar el rendimiento o reducir verticalmente la máquina virtual para ahorrar costos.
- También puede escalar horizontalmente asociando hasta 4 nodos de máquina virtual con un solo entorno de ejecución de integración autohospedado. Un único trabajo de copia que se ejecute en un entorno de ejecución de integración autohospedado particionará automáticamente el conjunto de archivos y aprovechará todos los nodos de máquina virtual para copiar los archivos en paralelo. Para lograr una alta disponibilidad, se recomienda empezar con dos nodos de máquina virtual para evitar que haya un único punto de error durante la migración de datos.
- Tanto la migración de datos de la instantánea inicial como la migración de datos diferencial se pueden realizar con esta arquitectura.


## <a name="implementation-best-practices"></a>Procedimientos recomendados para la implementación 

### <a name="authentication-and-credential-management"></a>Autenticación y administración de identidades 

- Para autenticarse en HDFS, se pueden usar [Windows (Kerberos) o Anónima.](https://docs.microsoft.com/azure/data-factory/connector-hdfs#linked-service-properties) 
- Se admiten varios tipos de autenticación para conectarse a Azure Blob Storage.  Se recomienda encarecidamente el uso de [identidades administradas para recursos de Azure](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#managed-identity): sobre la base de una identidad de ADF administrada automáticamente en Azure AD, permite configurar canalizaciones sin proporcionar credenciales en la definición del servicio vinculado.  Como alternativa, puede autenticarse en Azure Blob Storage mediante la [entidad de servicio](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#service-principal-authentication), la [firma de acceso compartido](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#shared-access-signature-authentication) o la [clave de la cuenta de almacenamiento](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#account-key-authentication). 
- También se admiten varios tipos de autenticación para conectarse a Azure Data Lake Storage Gen2.  Se recomienda encarecidamente el uso de [identidades administradas para recursos de Azure](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#managed-identity), aunque también se puede usar la [entidad de servicio](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#service-principal-authentication) o la [clave de la cuenta de almacenamiento](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#account-key-authentication). 
- Si no usa identidades administradas para recursos de Azure, es muy recomendable [almacenar las credenciales en Azure Key Vault](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault) para facilitar la administración y la rotación de las claves de forma centralizada sin modificar los servicios vinculados de ADF.  Este es también uno de los [procedimientos recomendados para CI/CD](https://docs.microsoft.com/azure/data-factory/continuous-integration-deployment#best-practices-for-cicd). 

### <a name="initial-snapshot-data-migration"></a>Migración de datos de instantánea inicial 

En el modo DistCp de ADF, puede crear una actividad de copia para enviar el comando DistCp con distintos parámetros para controlar el comportamiento inicial de la migración de datos. 

En el modo IR nativo de ADF, se recomienda la partición de datos, sobre todo si se migran más de 10 TB de datos. Para particionar los datos, use los nombres de carpeta de HDFS y, luego, cada trabajo de copia de ADF puede copiar una partición de carpeta a la vez. Puede ejecutar varios trabajos de copia de ADF simultáneamente para mejorar el rendimiento.
Si se produce un error en alguno de los trabajos de copia debido a una incidencia transitoria de la red o del almacén de datos, puede volver a ejecutar el trabajo de copia con errores para volver a cargar esa partición específica de HDFS. El resto de los trabajos de copia que cargan otras particiones no se verán afectados.

### <a name="delta-data-migration"></a>Migración de datos diferencial 

En el modo DistCp de ADF, puede usar diferentes parámetros de la línea de comandos de DistCp "-update, escribir datos cuando el tamaño del archivo de origen no es el mismo que el de destino" para lograr la migración de diferencias de datos.

En el modo IR nativo de ADF, la manera más eficaz de identificar archivos nuevos o modificados de HDFS es mediante la convención de nomenclatura con particiones de tiempo: cuando los datos de HDFS se han particionado en el tiempo con información del intervalo de tiempo en el nombre de archivo o carpeta (por ejemplo,/aaaa/mm/dd/archivo.csv), la canalización puede identificar fácilmente qué archivos o carpetas copiar de forma incremental.
Como alternativa, si los datos de HDFS no tienen particiones de tiempo, ADF puede identificar los archivos nuevos o modificados por el valor de LastModifiedDate (fecha de la última modificación). Para ello, ADF examinará todos los archivos de HDFS y solo copiará el archivo nuevo y actualizado cuya marca de tiempo de última modificación sea mayor que un valor determinado. Tenga en cuenta que si tiene un gran número de archivos en HDFS, el análisis de archivos inicial podría tardar mucho tiempo, independientemente de la cantidad de archivos que cumplan la condición de filtro. En este caso, se recomienda crear primero las particiones de datos y usar la misma partición para la migración de la instantánea inicial, con el fin de que el análisis de archivos pueda realizarse en paralelo.

### <a name="estimating-price"></a>Estimación del precio 

Considere la siguiente canalización creada para migrar datos de HDFS a Azure Blob Storage: 

![precios-canalización](media/data-migration-guidance-hdfs-to-azure-storage/pricing-pipeline.png)

Supongamos lo siguiente: 

- El volumen total de datos es de 1 PB
- Migración de datos mediante una arquitectura de segunda solución (modo IR nativo de ADF)
- 1 PB se divide en particiones de 1000 y cada copia mueve una partición
- Cada actividad de copia se configura con un IR autohospedado asociado a 4 máquinas y logra un rendimiento de 500 MBps.
- La simultaneidad de ForEach se establece en 4 y el rendimiento agregado es de 2 Gbps
- En total, se tardan 146 horas en completar la migración.


Este es el precio estimado en función de las suposiciones anteriores: 

![precios-tabla](media/data-migration-guidance-hdfs-to-azure-storage/pricing-table.png)

> [!NOTE]
> Este es un ejemplo de precios hipotético.  Los precios reales dependen del rendimiento real de su entorno.
> No se incluye el precio de la máquina virtual Windows de Azure (con entorno de ejecución de integración autohospedado instalado).

### <a name="additional-references"></a>Referencias adicionales 
- [Conector de HDFS](https://docs.microsoft.com/azure/data-factory/connector-hdfs)
- [Conector de Azure Blob Storage](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage)
- [Conector de Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage)
- [Guía de optimización y rendimiento de la actividad de copia](https://docs.microsoft.com/azure/data-factory/copy-activity-performance)
- [Creación y configuración de un entorno de ejecución de integración autohospedado](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime)
- [Alta disponibilidad y escalabilidad de un entorno de ejecución de integración autohospedado](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime#high-availability-and-scalability)
- [Consideraciones sobre la seguridad del movimiento de datos](https://docs.microsoft.com/azure/data-factory/data-movement-security-considerations)
- [Almacenamiento de credenciales en Azure Key Vault](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault)
- [Copia incremental de nuevos archivos por el nombre de archivo con particiones de tiempo](https://docs.microsoft.com/azure/data-factory/tutorial-incremental-copy-partitioned-file-name-copy-data-tool)
- [Copia incremental de archivos nuevos y modificados según LastModifiedDate](https://docs.microsoft.com/azure/data-factory/tutorial-incremental-copy-lastmodified-copy-data-tool)
- [Página de precios de ADF](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/)

## <a name="next-steps"></a>Pasos siguientes

- [Copia de archivos de varios contenedores con Azure Data Factory](solution-template-copy-files-multiple-containers.md)