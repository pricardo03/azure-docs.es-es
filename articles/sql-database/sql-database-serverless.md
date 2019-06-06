---
title: Azure SQL Database sin servidor (versión preliminar)| Microsoft Docs
description: En este artículo se describe el nuevo nivel de proceso sin servidor y se compara con el nivel de proceso aprovisionado existente.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: sstein, carlrab
manager: craigg
ms.date: 06/05/2019
ms.openlocfilehash: b39d2c839444e3cad60d5ff08e117282ecc04d7a
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734770"
---
# <a name="sql-database-serverless-preview"></a>SQL Database sin servidor (versión preliminar)

## <a name="serverless-compute-tier"></a>Nivel de servicio de informática sin servidor

Base de datos SQL sin servidor (versión preliminar) es un nivel de proceso de base de datos única que escala automáticamente de proceso y se les factura para la cantidad de proceso utilizado por segundo. 

Una base de datos en el nivel de proceso sin servidor se parametriza con el intervalo de proceso que puede usar y una demora de pausa automática.

![facturación sin servidor](./media/sql-database-serverless/serverless-billing.png)

### <a name="performance"></a>Rendimiento

- El número de núcleos virtuales mínimos y núcleos virtuales es parámetros configurables que definen el rango de capacidad de proceso disponible para la base de datos. Los límites de memoria y E/S son proporcionales al intervalo de núcleos virtuales especificado.  
- La demora de pausa automática es un parámetro configurable que define el período de tiempo de que la base de datos debe estar inactiva antes de detenerse automáticamente. La base de datos se reanuda automáticamente con el siguiente inicio de sesión.

### <a name="pricing"></a>Precios

- La factura total de una base de datos sin servidor es la suma de la factura de proceso y la factura de almacenamiento.
La facturación de proceso se basa en la cantidad de núcleos virtuales usados y memoria usada por segundo.
- El mínimo de recursos facturado se basa en el mínimo de núcleos virtuales y el mínimo de memoria.
- Mientras la base de datos está en pausa, solo se factura el almacenamiento.

## <a name="scenarios"></a>Escenarios

Este nivel de proceso sin servidor ofrece una relación entre precio y rendimiento optimizada para bases de datos únicas con patrones de uso intermitentes e impredecibles, que pueden permitirse alguna demora en el calentamiento de los recursos de proceso después de períodos de inactividad. En cambio, el nivel de proceso aprovisionada es precio / rendimiento optimizado para las bases de datos únicas o varias bases de datos en grupos elásticos con un mayor uso medio que no puede permitirse cualquier retraso en la preparación del proceso.

### <a name="scenarios-well-suited-for-serverless-compute"></a>Escenarios adecuados para el proceso sin servidor

- Bases de datos únicas con patrones de uso impredecible, intermitente intercalan con períodos de inactividad y la utilización de proceso promedio inferior con el tiempo.
- Bases de datos únicas en el nivel de proceso aprovisionada que se escalan con frecuencia y los clientes que prefieren para delegar el proceso cambia la escala para el servicio.
- Nuevas bases de datos únicas sin historial de uso donde el tamaño de proceso es difícil o no es posible calcular antes de la implementación en la base de datos SQL.

### <a name="scenarios-well-suited-for-provisioned-compute"></a>Escenarios adecuados para el proceso aprovisionado

- Bases de datos únicas con patrones de uso más regular y predecible y promedio mayor uso con el tiempo de proceso.
- Bases de datos que no pueden tolerar compensaciones de rendimiento resultantes de recortes de memoria más frecuentes o de una demora en la reanudación automática desde un estado de pausa.
- Varias bases de datos con patrones de uso impredecible, intermitente que se pueden consolidar en grupos elásticos para una mejor optimización de precio / rendimiento.

## <a name="comparison-with-provisioned-compute-tier"></a>Comparación con el nivel de proceso aprovisionado

La tabla siguiente resume las diferencias entre el nivel de proceso sin servidor y el nivel de proceso aprovisionado:

| | **Proceso sin servidor** | **Proceso aprovisionado** |
|:---|:---|:---|
|**Patrón de uso de la base de datos**| Uso con el tiempo de proceso de uso impredecible, intermitente con un promedio inferior. |  Patrones de uso más normales con Media mayor uso en el tiempo o varias bases de datos con grupos elásticos de proceso.|
| **Trabajo de administración del rendimiento** |Inferior|Superior|
|**Escalado de proceso**|Automático|Manual|
|**Capacidad de respuesta del proceso**|Menor después de períodos de inactividad|Inmediata|
|**Granularidad de facturación**|Por segundo|Por hora|

## <a name="purchasing-model-and-service-tier"></a>Modelo de compra y nivel de servicio

SQL Database sin servidor solo se admite actualmente en el nivel de uso general en hardware de generación 5 en el modelo de compra de núcleos virtuales.

## <a name="autoscale"></a>Escalado automático

### <a name="scaling-responsiveness"></a>Escalado de la capacidad de respuesta

En general, las bases de datos sin servidor se ejecutan en un equipo con capacidad suficiente para satisfacer la demanda de recursos sin interrupciones para cualquier cantidad de proceso solicitado dentro de los límites establecidos por el valor del número de núcleos virtuales. En ocasiones, se produce automáticamente un equilibrio de carga si la máquina no puede satisfacer la demanda de recursos en cuestión de minutos. Por ejemplo, si la demanda de recursos es de 4 núcleos virtuales, pero sólo 2 núcleos virtuales están disponibles, a continuación, esta puede tardar unos minutos para equilibrar la carga antes de que se proporcionan 4 núcleos virtuales. La base de datos permanece en línea durante el equilibrio de carga excepto durante un breve período al final de la operación cuando se deshabilitan las conexiones.

### <a name="memory-management"></a>Administración de memoria

Se reclame la memoria para bases de datos sin servidor con mayor frecuencia que las bases de datos de proceso aprovisionada. Este comportamiento es importante para controlar los costos en sin servidor y puede afectar al rendimiento.

#### <a name="cache-reclamation"></a>Recuperación de la memoria caché

A diferencia de las bases de datos de proceso aprovisionada, se reclama memoria desde la caché SQL desde una base de datos sin servidor cuando el uso de CPU o memoria caché es baja.

- Utilización de la caché se considera baja cuando el tamaño total de las más usados recientemente caché entradas cae por debajo del umbral durante un período de tiempo.
- Cuando se desencadena la recuperación de la memoria caché, el tamaño de caché de destino se reduce gradualmente a una fracción del tamaño anterior y reclamar solo continúa si uso sigue siendo bajo.
- Cuando se produce la recuperación de caché, la directiva para seleccionar las entradas de caché se va a expulsar es la misma directiva de selección en cuanto a las bases de datos de proceso aprovisionada cuando la presión de memoria es alta.
- El tamaño de caché nunca se reduce por debajo del límite de memoria mínima como se define en núcleos virtuales mínimos que se pueden configurar.

Sin servidor y aprovisionado proceso bases de datos, caché entradas pueden expulsarse si se usa toda la memoria disponible.

#### <a name="cache-hydration"></a>Hidratación de caché

La caché de SQL crece a medida que se capturan datos desde el disco de la misma manera y con la misma velocidad que las bases de datos aprovisionados. Cuando la base de datos está ocupado, la memoria caché puede crecer sin restricciones hasta el límite máximo de memoria.

## <a name="autopause-and-autoresume"></a>Pausa y reanudación automáticas

### <a name="autopause"></a>Pausa automática

Autopausing se desencadena si todas las condiciones siguientes son verdaderas para la duración del retraso pausarautomáticamente:

- Número de sesiones = 0
- CPU = 0 para la carga de trabajo de usuario que se ejecuta en el grupo de usuario

Se proporciona una opción para deshabilitar autopausing si lo desea.

Las características siguientes no admiten autopausing.  Es decir, si se utiliza cualquiera de las siguientes características, la base de datos permanece en línea, independientemente de la duración de inactividad de la base de datos:

- Replicación geográfica (grupos de conmutación por error automática y replicación geográfica activos).
- Retención de copia de seguridad a largo plazo (LTR).
- La base de datos de sincronización utilizada en la sincronización de datos SQL.

Autopausing se evita temporalmente durante la implementación de algunas actualizaciones de servicio que requieren que la base de datos esté en línea.  En tales casos, autopausing se convierte en permite a intentarlo una vez completada la actualización del servicio.

### <a name="autoresume"></a>Reanudación automática

Autoresuming se desencadena si alguna de las siguientes condiciones es verdadera en cualquier momento:

|Característica|Desencadenamiento de reanudación automática|
|---|---|
|Autenticación y autorización|Inicio de sesión|
|Detección de amenazas|Habilitación o deshabilitación de la configuración de detección de amenazas en el nivel de base de datos o servidor.<br>Modificar configuración de detección de amenazas en el nivel de base de datos o servidor.|
|Detección y clasificación de datos|Adición, modificación, eliminación o visualización de las etiquetas de confidencialidad|
|Auditoría|Visualización de registros de auditoría<br>Actualizar o ver la directiva de auditoría.|
|Enmascaramiento de datos|Adición, modificación, eliminación o visualización de reglas de enmascaramiento de datos|
|Cifrado de datos transparente|Visualización del estado de cifrado de datos transparente|
|Almacén de datos de consulta (rendimiento)|Modificación o visualización de la configuración del almacén de consulta, ajuste automático|
|Ajuste automático|Aplicación y comprobación de recomendaciones de ajuste automático, como la indexación automática|
|Copia de base de datos|Crear base de datos como copia.<br>Exportar a un archivo BACPAC.|
|Sincronización de datos SQL|Sincronización entre la base de datos central y las bases de datos miembro que se ejecutan según una programación configurable o bien de forma manual|
|Modificación de algunos metadatos de base de datos|Agregar nuevas etiquetas de la base de datos.<br>Cambiar el número de núcleos virtuales, núcleos virtuales mínimos o retraso pausarautomáticamente.|
|SQL Server Management Studio (SSMS)|Al usar SSMS versión 18 y abrir una nueva ventana de consulta para cualquier base de datos en el servidor se reanudará cualquier base de datos en pausa automática en el mismo servidor. Este comportamiento no se produce si se usa SSMS versión 17.9.1 con IntelliSense deshabilitado.|

Autoresuming también se desencadena durante la implementación de algunas actualizaciones de servicio que requieren que la base de datos esté en línea.

### <a name="connectivity"></a>Conectividad

Si está en pausa una base de datos sin servidor, el primer inicio de sesión se reanude la base de datos y devolver un error que indica que la base de datos no está disponible con el código de error 40613. Una vez que se reanude la base de datos, será necesario intentar iniciar sesión de nuevo para establecer la conectividad. No es necesario modificar los clientes de la base de datos con lógica de reintento de conexión.

### <a name="latency"></a>Latencia

La latencia a autoresume y pausarautomáticamente una base de datos sin servidor suele ser el orden de 1 minuto para autoresume y 1-10 minutos para pausarautomáticamente.

## <a name="onboarding-into-serverless-compute-tier"></a>Incorporación a nivel de proceso sin servidor

La creación de una nueva base de datos o el cambio de una base de datos existente a un nivel de proceso sin servidor siguen el mismo patrón que la creación de una nueva base de datos en el nivel de proceso aprovisionado y constan de los dos pasos siguientes:

1. Especifique el nombre del objetivo de servicio. El objetivo de servicio prescribe el nivel de servicio, la generación de hardware y el número de núcleos virtuales. La siguiente tabla muestra las opciones de objetivo de servicio:

   |Nombre del objetivo de servicio|Nivel de servicio|Generación de hardware|Máximo de núcleos virtuales|
   |---|---|---|---|
   |GP_S_Gen5_1|Uso general|Gen5|1|
   |GP_S_Gen5_2|Uso general|Gen5|2|
   |GP_S_Gen5_4|Uso general|Gen5|4|

2. Opcionalmente, especifique el retraso mínimo de núcleos virtuales y pausarautomáticamente para cambiar sus valores predeterminados. En la siguiente tabla se muestran los valores disponibles para estos parámetros.

   |Parámetro|Opciones de valores|Valor predeterminado|
   |---|---|---|---|
   |Núcleos virtuales mínimos|Cualquier valor entre {0,5, 1, 2, 4} que no supere el máximo de núcleos virtuales|0,5 núcleos virtuales|
   |Demora de pausa automática|Mín.: 360 minutos (6 horas)<br>Máx.: 10 080 minutos (7 días)<br>Incrementos: 60 minutos<br>Deshabilitar pausa automática: -1|360 minutos|

> [!NOTE]
> Actualmente no es posible usar T-SQL para mover una base de datos existente al nivel de proceso sin servidor o cambiar su tamaño de proceso, pero esto sí puede realizarse mediante Azure Portal o PowerShell.

### <a name="create-new-serverless-database-using-azure-portal"></a>Crear nueva base de datos sin servidor mediante Azure portal

Consulte [Quickstart: Creación de una base de datos única en Azure SQL Database con Azure Portal](sql-database-single-database-get-started.md).

### <a name="create-new-serverless-database-using-powershell"></a>Crear nueva base de datos sin servidor mediante PowerShell

En el ejemplo siguiente se crea una nueva base de datos en el nivel de proceso sin servidor definida por el objetivo de servicio denominado GP_S_Gen5_4 con los valores predeterminados para el mínimo de núcleos virtuales y la demora de pausa automática.

```powershell
New-AzSqlDatabase `
  -ResourceGroupName $resourceGroupName `
  -ServerName $serverName `
  -DatabaseName $databaseName `
  -ComputeModel Serverless `
  -Edition GeneralPurpose `
  -ComputeGeneration Gen5 `
  -MinVcore 0.5 `
  -MaxVcore 2 `
  -AutoPauseDelayInMinutes 720
```

### <a name="move-provisioned-compute-database-into-serverless-compute-tier"></a>Mover la base de datos de proceso aprovisionado en el nivel de proceso sin servidor

En el siguiente ejemplo se mueve una base de datos única existente desde el nivel de proceso aprovisionado al nivel de proceso sin servidor. En este ejemplo se especifica explícitamente el mínimo de núcleos virtuales, el máximo de núcleos virtuales y la demora de pausa automática.

```powershell
Set-AzSqlDatabase
  -ResourceGroupName $resourceGroupName `
  -ServerName $serverName `
  -DatabaseName $databaseName `
  -Edition GeneralPurpose `
  -ComputeModel Serverless `
  -ComputeGeneration Gen5 `
  -MinVcore 1 `
  -MaxVcore 4 `
  -AutoPauseDelayInMinutes 1440
```

### <a name="move-serverless-database-into-provisioned-compute-tier"></a>Mover la base de datos sin servidor en el nivel de proceso aprovisionada

Una base de datos sin servidor se puede mover a un nivel de proceso aprovisionado de la misma manera que se mueve una base de datos de proceso aprovisionado a un nivel de proceso sin servidor.

## <a name="modifying-serverless-configuration"></a>Modificación de la configuración sin servidor

### <a name="maximum-vcores"></a>Máximo de núcleos virtuales

Modificar el número de núcleos virtuales se realiza mediante el [conjunto AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) comando en PowerShell mediante el `MaxVcore` argumento.

### <a name="minimum-vcores"></a>Mínimo de núcleos virtuales

Modificación de los núcleos virtuales mínimos se realiza mediante el [conjunto AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) comando en PowerShell mediante el `MinVcore` argumento.

### <a name="autopause-delay"></a>Demora de pausa automática

Modificar el retraso pausarautomáticamente se realiza mediante el [conjunto AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) comando en PowerShell mediante el `AutoPauseDelayInMinutes` argumento.

## <a name="monitoring"></a>Supervisión

### <a name="resources-used-and-billed"></a>Recursos utilizados y facturados

Los recursos de una base de datos sin servidor están encapsulados con las siguientes entidades:

#### <a name="app-package"></a>Paquete de aplicaciones

El paquete de aplicaciones es el límite de administración de recursos más externo de una base de datos, independientemente de si la base de datos se encuentra en un nivel de proceso sin servidor o aprovisionado. El paquete de aplicaciones contiene la instancia de SQL y los servicios externos que en conjunto abarcan todos los recursos de usuario y del sistema que utiliza una base de datos en SQL Database. Entre los ejemplos de servicios externos se incluyen R y la búsqueda de texto completo. Generalmente, la instancia de SQL domina el uso de recursos global en el paquete de aplicaciones.

#### <a name="user-resource-pool"></a>Grupo de recursos de usuario

El grupo de recursos de usuario es el límite de administración de recursos más interno de una base de datos, independientemente de si la base de datos se encuentra en un nivel de proceso sin servidor o aprovisionado. El usuario resource pool ámbitos CPU y E/S para cargas de trabajo de usuario generados por consultas DDL como CREATE y ALTER y DML de las consultas, como seleccionar, insertar, actualizar y eliminar. Por lo general, estas consultas representan la proporción de uso dentro del paquete de aplicaciones más importante.

### <a name="metrics"></a>metrics

|Entidad|Métrica|DESCRIPCIÓN|Unidades|
|---|---|---|---|
|Paquete de aplicaciones|app_cpu_percent|Porcentaje de núcleos virtuales utilizado por la aplicación con respecto al máximo de núcleos virtuales permitido para la aplicación.|Porcentaje|
|Paquete de aplicaciones|app_cpu_billed|La cantidad de procesos que se facturan para la aplicación durante el período de informe. El importe pagado durante este período es el producto de esta métrica por el precio de la unidad de núcleo virtual. <br><br>Los valores de esta métrica se determinan al agregar en el tiempo el máximo de CPU utilizada y la memoria usada por segundo. Si la cantidad utilizada es menor que la cantidad mínima aprovisionada definida por el mínimo de núcleos virtuales y la memoria mínima, se factura la cantidad mínima aprovisionada. Para comparar la CPU y la memoria con fines de facturación, la memoria se normaliza en unidades de núcleos virtuales cambiando la escala de la cantidad de GB de memoria en 3 GB por núcleo virtual.|Segundos de núcleo virtual|
|Paquete de aplicaciones|app_memory_percent|Porcentaje de memoria utilizada por la aplicación con respecto a la memoria máxima permitida para la aplicación.|Porcentaje|
|Grupo de usuarios|cpu_percent|Porcentaje de núcleos virtuales utilizado por la carga de trabajo de usuario con respecto al máximo de núcleos virtuales permitido para la carga de trabajo de usuario.|Porcentaje|
|Grupo de usuarios|data_IO_percent|Porcentaje de IOPS de datos utilizado por la carga de trabajo de usuario con respecto al máximo de IOPS de datos permitido para la carga de trabajo de usuario.|Porcentaje|
|Grupo de usuarios|log_IO_percent|Porcentaje de MB/s de registro utilizado por la carga de trabajo de usuario con respecto al máximo de MB/s de registro permitido para la carga de trabajo de usuario.|Porcentaje|
|Grupo de usuarios|workers_percent|Porcentaje de trabajos utilizado por la carga de trabajo de usuario con respecto al máximo de trabajos permitido para la carga de trabajo de usuario.|Porcentaje|
|Grupo de usuarios|sessions_percent|Porcentaje de sesiones utilizado por la carga de trabajo de usuario con respecto al máximo de sesiones permitido para la carga de trabajo de usuario.|Porcentaje|
____

> [!NOTE]
> Las métricas en Azure Portal están disponibles en el panel de base de datos para una base de datos única en **Supervisión**.

### <a name="pause-and-resume-status"></a>Estado de pausa y reanudación

En Azure Portal, se muestra el estado de la base de datos en el panel de información general del servidor que enumera las bases de datos que contiene. El estado de la base de datos también se muestra en el panel de información general de la base de datos.

Con el siguiente comando de PowerShell puede consultar el estado de pausa y reanudación de una base de datos:

```powershell
Get-AzSqlDatabase `
  -ResourceGroupName $resourcegroupname `
  -ServerName $servername `
  -DatabaseName $databasename `
  | Select -ExpandProperty "Status"
```

## <a name="resource-limits"></a>Límites de recursos

Para ver los límites de recursos, consulte [Nivel de proceso sin servidor](sql-database-vCore-resource-limits-single-databases.md#serverless-compute-tier).

## <a name="billing"></a>Facturación

La cantidad de proceso que se factura es el máximo de CPU y memoria usado en cada segundo. Si la cantidad de CPU y memoria usadas es inferior a la cantidad mínima aprovisionada para cada una, se factura la cantidad aprovisionada. Para comparar la CPU y la memoria con fines de facturación, la memoria se normaliza en unidades de núcleos virtuales cambiando la escala de la cantidad de GB de memoria en 3 GB por núcleo virtual.

- **Recurso facturado**: CPU y memoria
- **Importe facturado**: precio unitario de memoria con núcleo virtual * max (núcleos virtuales mínimos, núcleos virtuales que se usa, GB de memoria mínima * 1/3, memoria GB usados * 1/3) 
- **Frecuencia de facturación**: Por segundo

El precio de unidad de memoria con núcleo virtual en el costo por núcleo virtual por segundo. Consulte la [página de precios de Azure SQL Database](https://azure.microsoft.com/pricing/details/sql-database/single/) para conocer los precios de unidad específicos de una región determinada.

La cantidad de proceso facturada se expone mediante la métrica siguiente:

- **Métrica**: app_cpu_billed (segundos de núcleo virtual)
- **Definición**: máx. (mínimo de núcleos virtuales, núcleos virtuales usados, GB de memoria mínima * 1/3, GB de memoria usada * 1/3)
- **Frecuencia de informes**: Por minuto

Esta cantidad se calcula cada segundo y se agrega en un intervalo de 1 minuto.

Considere la posibilidad de una base de datos sin servidor configurado con núcleo virtual de 1 minuto y número de 4 núcleos virtuales.  Esto corresponde a aproximadamente 3 GB de memoria de mínimo y máximo de 12 GB de memoria.  Supongamos que el retraso de pausa automática se establece en 6 horas y la carga de trabajo de la base de datos está activa durante las primeras horas 2 de un período de 24 horas e inactiva en caso contrario.    

En este caso, la base de datos se factura por proceso y almacenamiento durante las primeras 8 horas.  Aunque la base de datos está iniciando inactiva después de la segunda hora, todavía se factura para el cálculo en las subsiguientes 6 horas, según el proceso mínimos aprovisionado mientras la base de datos está en línea.  Sólo almacenamiento se factura durante el resto del período de 24 horas mientras está en pausa la base de datos.

Más concretamente, la factura de proceso en este ejemplo se calcula como sigue:

|Intervalo de tiempo|núcleos virtuales que usa cada segundo.|Utilizar GB por segundo|Calcular la factura de dimensión|segundos de la memoria con núcleo virtual se factura en un intervalo de tiempo|
|---|---|---|---|---|
|0:00-1:00|4|9|núcleos virtuales que se usa|4 núcleos virtuales * 3600 segundos = 14400 segundos de memoria con núcleo virtual|
|1:00-2:00|1|12|Memoria usada|12 GB * 1/3 * 3600 segundos = 14400 segundos de memoria con núcleo virtual|
|2:00-8:00|0|0|Memoria mínima aprovisionado|3 GB * 1/3 * 21600 segundos = 21600 segundos de la memoria con núcleo virtual|
|8:00-24:00|0|0|No hay ningún proceso de factura mientras está en pausa|núcleo virtual 0 segundos|
|Segundos de núcleo virtual total facturadas más de 24 horas||||núcleo virtual 50400 segundos|

Suponga que el precio de la unidad de proceso es 0,000073 $/núcleo virtual/segundo.  A continuación, la factura para este período de 24 horas de proceso es el producto de los segundos de precio y núcleo virtual de unidad proceso facturadas: $0.000073/vCore/second * 50400 segundos de la memoria con núcleo virtual = $3,68

## <a name="available-regions"></a>Regiones disponibles

El nivel de proceso sin servidor está disponible en todo el mundo, excepto las siguientes regiones: Este de Australia Central, China, Norte de China, sur de Francia, noreste de Alemania, Alemania Central, India occidental, Corea del Sur, oeste de Sudáfrica, Norte de Reino Unido, sur de Reino Unido, oeste de Reino Unido y centro occidental de EE.UU.

## <a name="next-steps"></a>Pasos siguientes

- Para comenzar, consulte [Inicio rápido: Creación de una base de datos única en Azure SQL Database con Azure Portal](sql-database-single-database-get-started.md).
- Para ver los límites de recursos, consulte [Límites de recursos del nivel de proceso sin servidor](sql-database-vCore-resource-limits-single-databases.md#serverless-compute-tier).
