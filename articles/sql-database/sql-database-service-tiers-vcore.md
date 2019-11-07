---
title: 'Información general del servicio Azure SQL Database: núcleo virtual | Microsoft Docs'
description: El modelo de compra de núcleo virtual permite escalar los recursos de proceso y de almacenamiento de manera independiente, igualar el rendimiento local y optimizar el precio.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
ms.date: 11/04/2019
ms.openlocfilehash: 2bbdd565a861004014ca4161856bba83ec0be511
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73496049"
---
# <a name="vcore-model-overview"></a>Introducción al modelo de núcleos virtuales

El modelo de núcleo virtual proporciona varias ventajas:

- Mayores límites de proceso, memoria, E/S y almacenamiento.
- Control sobre la generación de hardware para satisfacer mejor los requisitos de proceso y memoria de la carga de trabajo.
- Descuentos en los precios de [Ventaja híbrida de Azure (AHB)](sql-database-azure-hybrid-benefit.md) e [Instancia reservada (RI)](sql-database-reserved-capacity.md).
- Mayor transparencia en los detalles de hardware que potencian el proceso; facilita la planeación de las migraciones desde implementaciones locales.

## <a name="service-tiers"></a>Niveles de servicio

Entre las opciones de nivel de servicio del modelo núcleo virtual se incluyen Uso general, Crítico para la empresa e Hiperescala. El nivel de servicio define generalmente la arquitectura de almacenamiento, los límites de espacio y de E/S y las opciones de continuidad empresarial relacionadas con la disponibilidad y la recuperación ante desastres.

||**Uso general**|**Crítico para la empresa**|**Hiperescala**|
|---|---|---|---|
|Más adecuado para|La mayoría de las cargas de trabajo empresariales. Ofrece opciones de proceso y almacenamiento equilibradas y escalables pensando en el presupuesto. |Ofrece a las aplicaciones empresariales la mayor resistencia a los errores mediante el uso de varias réplicas aisladas y proporciona el mayor rendimiento de E/S por réplica de base de datos.|La mayoría de las cargas de trabajo de una empresa que tengan requisitos altamente escalables de almacenamiento y escalado de lectura.  Ofrece mayor resistencia a los errores al permitir la configuración de más de una réplica de base de datos aislada. |
|Storage|Usa el almacenamiento remoto.<br/>**Proceso aprovisionado de base de datos única y de grupo elástico**:<br/>5 GB – 4 TB<br/>**Proceso sin servidor**:<br/>5 GB - 3 TB<br/>**Instancia administrada**: 32 GB - 8 TB |Usa almacenamiento local de SSD.<br/>**Proceso aprovisionado de base de datos única y de grupo elástico**:<br/>5 GB - 8 TB<br/>**Instancia administrada**:<br/>32 GB - 4 TB |Crecimiento automático flexible de almacenamiento según sea necesario. Admite hasta 100 TB de almacenamiento. Utiliza almacenamiento SSD local para la caché del grupo de búferes local y almacenamiento de datos local. Utiliza almacenamiento remoto de Azure como almacén de datos final a largo plazo. |
|Rendimiento de E/S (aproximado)|**Grupo elástico y base de datos única**: 500 IOPS por núcleo virtual, hasta 40000 IOPS como máximo.<br/>**Instancia administrada**: Depende del [tamaño del archivo](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes).|5000 IOPS por núcleo virtual, hasta 320 000 IOPS como máximo|Hiperescala es una arquitectura de varios niveles con almacenamiento en caché en varios niveles. Los IOPS efectivos dependen de la carga de trabajo.|
|Disponibilidad|1 réplica, sin réplicas de escalado de lectura|3 réplicas, 1 [réplica de escalado de lectura](sql-database-read-scale-out.md),<br/>Alta disponibilidad (HA) con redundancia de zona|1 réplica de lectura y escritura, además de 0 a 4 [réplicas de escalado de lectura](sql-database-read-scale-out.md)|
|Copias de seguridad|[Almacenamiento con redundancia geográfica con acceso de lectura (RA-GRS)](../storage/common/storage-designing-ha-apps-with-ragrs.md), de 7 a 35 días (7 días de forma predeterminada)|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), de 7 a 35 días (7 días de forma predeterminada)|Copias de seguridad basadas en instantáneas en el almacenamiento remoto de Azure. Los procesos de restauración usan estas instantáneas para conseguir una recuperación rápida. Las copias de seguridad son instantáneas y no afectan al rendimiento de E/S del proceso. Las restauraciones son rápidas y no son operaciones relacionadas con el tamaño de los datos (tardan minutos en lugar de horas o días).|
|En memoria|No compatible|Compatible|No compatible|
|||


### <a name="choosing-a-service-tier"></a>Selección de un nivel de servicio

Para obtener información sobre cómo seleccionar un nivel de servicio para una carga de trabajo determinada, consulte los siguientes artículos:

- [Cuándo elegir el nivel de servicio Uso general](sql-database-service-tier-general-purpose.md#when-to-choose-this-service-tier)
- [Cuándo elegir el nivel de servicio Crítico para la empresa](sql-database-service-tier-business-critical.md#when-to-choose-this-service-tier)
- [Cuándo elegir el nivel de servicio Hiperescala](sql-database-service-tier-hyperscale.md#who-should-consider-the-hyperscale-service-tier)


## <a name="compute-tiers"></a>Niveles de proceso

Entre las opciones de nivel de proceso del modelo de núcleo virtual se incluyen los niveles de proceso aprovisionados y sin servidor.


### <a name="provisioned-compute"></a>Proceso aprovisionado

El nivel de proceso aprovisionado proporciona una cantidad específica de recursos de proceso que se aprovisionan continuamente con independencia de la actividad de carga de trabajo y factura la cantidad de proceso aprovisionado a un precio fijo por hora.


### <a name="serverless-compute"></a>Proceso sin servidor

El [nivel de proceso sin servidor](sql-database-serverless.md) escala automáticamente los recursos de proceso en función de la actividad de la carga de trabajo y se factura según la cantidad de proceso usado por segundo.



## <a name="hardware-generations"></a>Generaciones de hardware

Entre las opciones de generación de hardware del modelo de núcleo virtual se incluyen Gen 4/5, la serie M (versión preliminar) y la serie Fsv2 (versión preliminar). En general, la generación de hardware define los límites de proceso y de memoria, así como otras características que afectan al rendimiento de la carga de trabajo.

### <a name="gen4gen5"></a>Gen4/Gen5

- El hardware Gen4/Gen5 proporciona recursos equilibrados de proceso y memoria, y es adecuado para la mayoría de las cargas de trabajo de base de datos que no tienen una memoria mayor, un núcleo virtual superior o unos requisitos de núcleo virtual único más rápidos como los proporcionados por la serie Fsv2 o la serie M.

Para ver las regiones en las que Gen4/Gen5 está disponible, vea la [disponibilidad de Gen4/Gen5](#gen4gen5-1).

### <a name="fsv2-seriespreview"></a>Serie Fsv2 (versión preliminar)

- La serie Fsv2 es una opción de hardware optimizado para proceso que ofrece una latencia de CPU baja y una velocidad de reloj elevada para las cargas de trabajo más exigentes de CPU.
- En función de la carga de trabajo, la serie Fsv2 puede ofrecer más rendimiento de CPU por núcleo virtual que Gen5, y el tamaño de 72 núcleos virtuales puede proporcionar más rendimiento de CPU con menos costos que 80 núcleos virtuales en Gen5. 
- Fsv2 proporciona menos memoria y tempdb por núcleo virtual que otro hardware, por lo que se pueden tomar en consideración las series Gen5 y M para las cargas de trabajo sensibles a esos límites.  

Para las regiones en las que está disponible la serie Fsv2, consulte la [disponibilidad de la serie Fsv2](#fsv2-series).


### <a name="m-seriespreview"></a>Serie M (versión preliminar)

- La serie M es una opción de hardware optimizado para memoria para las cargas de trabajo que exigen más memoria y mayores límites de proceso que los proporcionados por Gen5.
- La serie M proporciona 29 GB por núcleo virtual y 128 núcleos virtuales, lo que aumenta el límite de memoria con respecto a Gen5 por 8x hasta casi 4 TB.

Para habilitar el hardware de la serie M para una suscripción y región, se debe abrir una solicitud de soporte técnico. Si se aprueba la solicitud de soporte técnico, la experiencia de selección y aprovisionamiento de la serie M sigue el mismo patrón que para otras generaciones de hardware. Para ver las regiones en las que la serie M está disponible, consulte la [disponibilidad de la serie M](#m-series).


### <a name="compute-and-memory-specifications"></a>Especificaciones de memoria y proceso


|Generación de hardware  |Proceso  |Memoria  |
|:---------|:---------|:---------|
|Gen4     |- Procesadores Intel E5-2673 v3 (Haswell) 2,4 GHz<br>- Aprovisionamiento de hasta 24 núcleos virtuales (1 núcleo virtual = 1 núcleo físico)  |- 7 GB por núcleo virtual<br>- Aprovisionamiento de hasta 168 GB|
|Gen5     |**Proceso aprovisionado**<br>- Procesadores Intel E5-2673 v4 (Broadwell) 2,3 GHz<br>- Aprovisionamiento de hasta 80 núcleos virtuales (1 núcleo virtual = 1 hiperproceso)<br><br>**Proceso sin servidor**<br>- Procesadores Intel E5-2673 v4 (Broadwell) 2,3 GHz<br>- Escalado vertical automático de hasta 16 núcleos virtuales (1 núcleo virtual = 1 hiperproceso)|**Proceso aprovisionado**<br>- 5,1 GB por núcleo virtual<br>- Aprovisionamiento de hasta 408 GB<br><br>**Proceso sin servidor**<br>- Escalado vertical automático de hasta 24 GB por núcleo virtual<br>- Escalado vertical automático de hasta 48 GB máx.|
|Serie Fsv2     |- Procesadores Intel Xeon Platinum 8168 (SkyLake)<br>- Presentación de una velocidad de reloj turbo sostenida de todos los núcleos de hasta 3,4 GHz y una velocidad de reloj turbo de un solo núcleo máxima de 3,7 GHz.<br>- Aprovisionamiento de hasta 72 núcleos virtuales (1 núcleo virtual = 1 hiperproceso)|1,9 GB por núcleo virtual<br>- Aprovisionamiento de 136 GB|
|Serie M     |- Procesadores Intel Xeon E7-8890 v3 de 2,5 GHz<br>- Aprovisionamiento de 128 núcleos virtuales (1 núcleo virtual = 1 hiperproceso)|29 GB por núcleo virtual<br>- Aprovisionamiento de 3,7 TB|


Para obtener más información sobre los límites de recursos, vea [Límites de recursos para bases de datos únicas (núcleo virtual)](sql-database-vcore-resource-limits-single-databases.md) o [Límites de recursos para grupos elásticos (núcleo virtual)](sql-database-vcore-resource-limits-elastic-pools.md).

### <a name="selecting-a-hardware-generation"></a>Selección de una generación de hardware

En Azure Portal, puede seleccionar la generación de hardware para una base de datos o grupo SQL en el momento de la creación o puede cambiar la generación de hardware de una base de datos o grupo SQL existente.

**Seleccionar una generación de hardware al crear una base de datos o un grupo SQL**

Para obtener información detallada, consulte [Creación de una base de datos SQL](sql-database-single-database-get-started.md).

En la pestaña **Básico**, seleccione el vínculo **Configurar base de datos** en la sección **Compute + storage** (Proceso + almacenamiento) y, a continuación, seleccione el vínculo **Cambiar configuración**:

  ![configurar base de datos](media/sql-database-service-tiers-vcore/configure-sql-database.png)

Seleccione la generación de hardware deseada:

  ![seleccionar hardware](media/sql-database-service-tiers-vcore/select-hardware.png)


**Cambiar la generación de hardware de una base de datos o grupo SQL existente**

En el caso de una base de datos, en la página de información general, seleccione el vínculo **Plan de tarifa**:

  ![cambiar hardware](media/sql-database-service-tiers-vcore/change-hardware.png)

En la página de información general, seleccione **Configurar**.

Siga los pasos para cambiar la configuración y seleccione la generación de hardware como se describe en los pasos anteriores.

### <a name="hardware-availability"></a>Disponibilidad de hardware

#### <a name="gen4gen5"></a>Gen4/Gen5

Las nuevas bases de datos de Gen4 ya no se admiten en las regiones Este de Australia o Sur de Brasil. 

Gen5 está disponible en la mayoría de las regiones de todo el mundo.

#### <a name="fsv2-series"></a>Serie Fsv2

La serie Fsv2 está disponible en las siguientes regiones: Centro de Australia, Centro de Australia 2, Este de Australia, Sudeste de Australia, Sur de Brasil, Centro de Canadá, Asia Oriental, Asia Pacífico, Centro de Francia, Centro de la India, India occidental, Centro de Corea del Sur, Sur de Corea del Sur, Norte de Europa, Norte de Sudáfrica, Asia Suroriental, Sur de Reino Unido, Oeste de Reino Unido, Oeste de Europa, Oeste de Europa 2.


#### <a name="m-series"></a>Serie M

La serie M está disponible en las siguientes regiones: Este de EE. UU., Norte de Europa, Oeste de Europa, Oeste de EE. UU. 2.
La serie M también puede tener una disponibilidad limitada en otras regiones. Puede solicitar una región distinta de la que se muestra aquí, pero puede que no sea posible la ejecución en otra región.

Para habilitar la disponibilidad de la serie M en una suscripción, debe solicitarse el acceso mediante la [presentación de una nueva solicitud de soporte técnico](#create-a-support-request-to-enable-m-series).


##### <a name="create-a-support-request-to-enable-m-series"></a>Cree una solicitud de soporte técnico para habilitar la serie M: 

1. Seleccione **Ayuda y soporte técnico** en el portal.
2. Seleccione **Nueva solicitud de soporte técnico**.

En la página **Datos básicos**, proporcione los valores siguientes:

1. En **Tipo de problema**, seleccione **Límites de servicio y suscripción (cuotas)** .
2. En **Suscripción**, seleccione la suscripción para habilitar la serie M.
3. En **Tipo de cuota**, seleccione **Base de datos SQL**.
4. Seleccione **Siguiente** para ir a la página **Detalles**.

En la página **Detalles**, proporcione lo siguiente:

5. En la sección **Detalles del problema**, seleccione el vínculo **Proporcionar detalles**. 
6. En **Tipo de cuota de base de datos SQL**, seleccione **Serie M**.
7. En **Región**, seleccione la región para habilitar la serie M.
    Para ver las regiones en las que la serie M está disponible, consulte la [disponibilidad de la serie M](#m-series).

Normalmente, las solicitudes de soporte técnico aprobadas se cumplimentan en un plazo de 5 días laborables.


## <a name="next-steps"></a>Pasos siguientes

- Para crear una base de datos SQL, consulte la [creación de una base de datos SQL mediante Azure Portal](sql-database-single-database-get-started.md).
- Para más información sobre las opciones de tamaños de proceso y de almacenamiento específicas que hay disponibles para las bases de datos únicas, consulte [Límites de recursos basados en núcleos virtuales de SQL Database para bases de datos únicas](sql-database-vcore-resource-limits-single-databases.md).
- Para más información sobre las opciones de tamaño de proceso y de almacenamiento específicas que hay disponibles para los grupos elásticos, consulte [Límites de recursos basados en núcleos virtuales de Azure SQL Database para grupos elásticos](sql-database-vcore-resource-limits-elastic-pools.md).
- Para obtener información detallada acerca de los precios, consulte la [página de precios de Azure SQL Database](https://azure.microsoft.com/pricing/details/sql-database/single/).
