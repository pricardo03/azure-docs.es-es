---
title: Azure SQL Database sin servidor (versión preliminar) | Microsoft Docs
description: En este artículo se describe el nuevo nivel de proceso sin servidor y lo compara con el nivel de proceso aprovisionada existente
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
ms.date: 05/07/2019
ms.openlocfilehash: 2ab8f272fc264f153144803be772d381c1780512
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65143267"
---
# <a name="sql-database-serverless-preview"></a>Sin servidor de SQL Database (versión preliminar)

## <a name="what-is-the-serverless-compute-tier"></a>¿Qué es el nivel de proceso sin servidor

Base de datos SQL sin servidor (versión preliminar) es un nivel de proceso que se factura por el importe de proceso utilizado por una sola base de datos por segundo. Sin servidor está optimizada para bases de datos únicas con patrones de uso por ráfagas que pueden permitirse algún retraso en el calentamiento de proceso después de períodos de inactividad de uso de rendimiento de precio.
En cambio, de ofertas disponibles públicamente en la factura de hoy de base de datos SQL para la cantidad de proceso aprovisionada cada hora. Este nivel de proceso aprovisionada está optimizado para las bases de datos únicas o elástica de rendimiento de precio de los grupos con un mayor uso medio que no puede permitirse cualquier retraso en la preparación del proceso.

Una base de datos en el nivel de equipo sin servidor se parametriza por el intervalo de proceso que puede usar y un retraso pausarautomáticamente.

![facturación sin servidor](./media/sql-database-serverless/serverless-billing.png)

### <a name="performance"></a>Rendimiento

- `MinVcore` y `MaxVcore` son parámetros configurables que definen el rango de capacidad de proceso disponible para la base de datos. Límites de memoria y E/S son proporcionales al intervalo de memoria con núcleo virtual especificado.  
- El retraso de pausarautomáticamente es un parámetro configurable que define el período de tiempo de que la base de datos debe estar inactivo antes de se detiene automáticamente. La base de datos se reanuda automáticamente cuando se produce el siguiente inicio de sesión.

### <a name="pricing"></a>Precios

- La factura total para una base de datos sin servidor es la suma de la factura de proceso y la factura de almacenamiento.
La facturación de proceso se basa en la cantidad de memoria usada por segundo y de núcleos virtuales que usa.
- El proceso mínimos facturado se basa en núcleos virtuales mínimos y mínima de memoria.
- Mientras está en pausa la base de datos, solo el almacenamiento se factura.

## <a name="scenarios"></a>Escenarios

Sin servidor es el precio / rendimiento optimizado para las bases de datos únicas con patrones de uso por ráfagas que pueden permitirse algún retraso en el calentamiento de proceso después de períodos de inactividad de uso. El nivel de proceso aprovisionada es precio / rendimiento optimizado para las bases de datos única o agrupadas con mayor uso medio que no puede permitirse cualquier retraso en la preparación del proceso.

En la tabla siguiente se compara niveles de proceso sin servidor con el nivel de proceso aprovisionada:

||Informática sin servidor|Proceso aprovisionado|
|---|---|---|
|**Escenario de uso típico**|Las bases de datos con el uso por ráfagas, imprevisible intercalan con períodos inactivos|Las bases de datos o grupos elásticos con el uso más habitual|
|**Esfuerzo de administración del rendimiento**|inferior|Superior|
|**Proceso de escalado**|Automático|Manual|
|**Calcular la capacidad de respuesta**|Inferior después de períodos inactivos|Inmediata|
|**Granularidad de facturación**|por segundo|Por hora|
|

### <a name="scenarios-well-suited-for-serverless-compute"></a>Escenarios adecuados para el proceso sin servidor

- Bases de datos únicas con patrones de uso por ráfagas intercalados con períodos de inactividad pueden beneficiarse de ahorro de precios basado en la facturación por segundo para la cantidad de proceso utilizado.
- Bases de datos únicas con la demanda de recursos que es difícil de predecir y los clientes que prefieren para delegar el ajuste de tamaño de proceso para el servicio.
- Bases de datos únicas en el nivel de proceso aprovisionada que cambian con frecuencia los niveles de rendimiento.

### <a name="scenarios-well-suited-for-provisioned-compute"></a>Escenarios adecuados para el proceso aprovisionado

- Uso con el tiempo de proceso de bases de datos únicas con normal más y más importantes.
- Las bases de datos que no pueden tolerar los inconvenientes de rendimiento resultante de más frecuentan de recorte de memoria o retraso en autoresuming desde un estado de pausa.
- Varias bases de datos con patrones de uso por ráfagas que se pueden consolidar en un único servidor y usan los grupos elásticos para una mejor optimización de precios.


## <a name="purchasing-model-and-service-tier"></a>Nivel de servicio y modelo de compra

SQL Database sin servidor no está actualmente solo se admite en el nivel de uso General en 5 generación de hardware en el modelo de compra de núcleos virtuales.

## <a name="autoscaling"></a>Escalado automático

### <a name="scaling-responsiveness"></a>Escalar la capacidad de respuesta

En general, las bases de datos se ejecutan en un equipo con capacidad suficiente para satisfacer la demanda de recursos sin interrupciones para cualquier cantidad de proceso solicitado dentro de los límites establecidos por el `maxVcores` valor. En ocasiones, se produce automáticamente Equilibrio de carga si la máquina no puede satisfacer la demanda de recursos en cuestión de minutos. La base de datos permanece en línea durante el equilibrio de carga excepto para un período breve al final de la operación cuando se quitan conexiones.

### <a name="memory-management"></a>Administración de memoria

Se reclame la memoria para bases de datos sin servidor con mayor frecuencia que las bases de datos aprovisionado. Este comportamiento es importante para controlar los costos en sin servidor. A diferencia del proceso aprovisionado, se reclama memoria desde la caché SQL desde una base de datos sin servidor cuando el uso de CPU o memoria caché es baja.

## <a name="autopause-and-autoresume"></a>Pausarautomáticamente y autoresume

### <a name="autopause"></a>Pausarautomáticamente

Pausarautomáticamente se desencadena si todas las condiciones siguientes son verdaderas para la duración del retraso pausarautomáticamente:

- Sesiones número = 0
- CPU = 0 (para la carga de trabajo de usuario que se ejecuta en el grupo de usuario)

Se proporciona una opción para deshabilitar pausarautomáticamente si lo desea.

### <a name="autoresume"></a>Autoresume

Autoresume se desencadena si alguna de las siguientes condiciones es verdadera en cualquier momento:

|Característica|Desencadenador Autoresume|
|---|---|
|Autenticación y autorización|Inicio de sesión|
|Detección de amenazas|Habilitación o deshabilitación de la configuración de detección de amenazas en el nivel de base de datos o servidor<br>Modificar la configuración de detección de amenazas en el nivel de base de datos o servidor|
|Detección y clasificación de datos|Agregar, modificar, eliminar o ver las etiquetas de confidencialidad|
|Auditoría|Ver registros de auditoría.<br>Actualizar o ver la directiva de auditoría|
|Enmascaramiento de datos|Agregar, modificar, eliminar o ver las reglas de enmascaramiento de datos|
|Cifrado de datos transparente|Estado de vista o el estado de cifrado de datos transparente|
|Almacén de datos de consulta (rendimiento)|Modificar o ver la configuración de almacén de consultas; el ajuste automático|
|Autoajuste|La aplicación y comprobación de recomendaciones de autoajuste como la indexación automática|
|Copiar base de datos|Crear base de datos como copia<br>Exportar a un archivo BACPAC|
|Sincronización de datos SQL|Sincronización entre las bases de datos de concentrador y miembro que se ejecutan según una programación configurable o se realizan de forma manual|
|Modificar algunos metadatos de la base de datos|Agregar nuevas etiquetas de la base de datos<br>Cambiar el número de núcleos virtuales, núcleos virtuales mínimos, retraso pausarautomáticamente|
|SQL Server Management Studio (SSMS)|Con SSMS versión 18 y abrir una nueva ventana de consulta para cualquier base de datos en el servidor se reanudará cualquier base de datos en pausa automática en el mismo servidor. Este comportamiento no se produce si mediante SSMS versión 17.9.1 con IntelliSense-deshabilitada.|

### <a name="connectivity"></a>Conectividad

Si está en pausa una base de datos sin servidor, el primer inicio de sesión se reanude la base de datos y devolver un error que indica que la base de datos no está disponible. Una vez que se reanuda la base de datos, debe volver a intentar el inicio de sesión para establecer la conectividad. Los clientes de la base de datos con lógica de reintento de conexión no es necesario modificar.

### <a name="latency"></a>Latencia

La latencia a pausarautomáticamente o autoresume una base de datos sin servidor es por lo general del orden de 1 minuto.

### <a name="feature-support"></a>Compatibilidad con las características

Las características siguientes no admiten autopausing y autoresuming. Es decir, si se utiliza cualquiera de las siguientes características, la base de datos permanece en línea, independientemente de la duración de inactividad de la base de datos:

- Replicación geográfica (active automática y replicación geográfica conmutación por error grupos)
- Retención de copia de seguridad a largo plazo (LTR)
- La base de datos de sincronización utilizado en SQL data sync.


## <a name="on-boarding-into-the-serverless-compute-tier"></a>Incorporación en el nivel de proceso sin servidor

Crear una nueva base de datos o mover que una base de datos existente a un nivel de proceso sin servidor sigue el mismo patrón que crear una nueva base de datos aprovisionado de nivel de proceso e implica los dos pasos siguientes:

1. Especifique el nombre del objetivo de servicio. En la tabla siguiente se muestra el nivel de servicio disponible y los tamaños de proceso actualmente disponibles en la versión preliminar pública.

   |Nivel de servicio|Tamaño de proceso|
   |---|---|
   |Uso general|GP_S_Gen5_1|
   |Uso general|GP_S_Gen5_2|
   |Uso general|GP_S_Gen5_4|

2. Opcionalmente, especifique el retraso mínimo de núcleos virtuales y pausarautomáticamente para cambiar sus valores predeterminados. En la tabla siguiente se muestra los valores disponibles para estos parámetros.

   |Parámetro|Opciones de valor|Valor predeterminado|
   |---|---|---|---|
   |Mínimo de núcleos virtuales|Cualquiera de {0,5, 1, 2, 4} no supera el número de núcleos virtuales|0,5 núcleos de virtuales|
   |Pausarautomáticamente retraso|Mín.: 360 minutos (6 horas)<br>Máx.: 10080 minutos (7 días)<br>Incrementos: 60 minutos<br>Deshabilitar pausarautomáticamente: -1|360 minutos|

> [!NOTE]
> No se admite actualmente con Transact-SQL para mover una base de datos existente a sin servidor o cambiar su tamaño de proceso, pero puede realizarse a través del portal de Azure o PowerShell.

### <a name="create-new-database-using-the-azure-portal"></a>Crear nueva base de datos mediante el portal de Azure

Consulte [Quickstart: Crear una base de datos en Azure SQL Database mediante Azure portal](sql-database-single-database-get-started.md).

### <a name="create-new-database-using-powershell"></a>Crear nueva base de datos mediante PowerShell

El ejemplo siguiente crea una nueva base de datos en el nivel de proceso sin servidor definido por el objetivo de servicio denominado GP_S_Gen5_4 con los valores predeterminados para el retraso mínimo de núcleos virtuales y pausarautomáticamente.

Sin servidor requiere una versión más reciente de PowerShell que está actualmente en la galería, así que ejecute `Update-Module Az.Sql` para obtener los últimos cmdlets sin servidor habilitado.

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
  -AutoPauseDelay 720
```

### <a name="move-existing-database-into-the-serverless-compute-tier"></a>Mover la base de datos existente en el nivel de proceso sin servidor

El siguiente ejemplo mueve una sola base de datos existente desde el nivel de proceso aprovisionada en el nivel de proceso sin servidor. Este ejemplo utiliza los valores predeterminados para los núcleos virtuales mínimos, número de núcleos virtuales y pausarautomáticamente retraso.

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
  -AutoPauseDelay 1440
```

### <a name="move-a-database-out-of-the-serverless-compute-tier"></a>Mover una base de datos fuera del nivel de proceso sin servidor

Una base de datos sin servidor se puede mover a un nivel de proceso aprovisionado en la misma manera que mover una base de datos de proceso aprovisionado en un nivel de proceso sin servidor.

## <a name="modify-serverless-configuration-parameters"></a>Modificar los parámetros de configuración sin servidor

### <a name="maximum-vcores"></a>Máximo de núcleos virtuales

Modificar el máximo de núcleos virtuales se realiza mediante el [conjunto AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) comando en PowerShell mediante el `MaxVcore` argumento.

### <a name="minimum-vcores"></a>Mínimo de núcleos virtuales

Modificar el máximo de núcleos virtuales se realiza mediante el [conjunto AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) comando en PowerShell mediante el `MinVcore` argumento.

### <a name="autopause-delay"></a>Pausarautomáticamente retraso

Modificar el máximo de núcleos virtuales se realiza mediante el [conjunto AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) comando en PowerShell mediante el `AutoPauseDelay` argumento.

## <a name="monitor-serverless-database"></a>Base de datos sin servidor del Monitor

### <a name="resources-used-and-billed"></a>Recursos utilizados y factura

Los recursos de una base de datos sin servidor están encapsulados en las siguientes entidades:

#### <a name="app-package"></a>Paquete de aplicación

El paquete de aplicación es el límite de administración de recursos más externo para una base de datos, independientemente de la base de datos esté en un nivel de proceso sin servidor o aprovisionado. El paquete de aplicación contiene la instancia de SQL y servicios externos ese ámbito junto todos los recursos de usuario y del sistema utilizados por una base de datos en la base de datos SQL. R y búsqueda de texto completo son ejemplos de servicios externos. Generalmente, la instancia de SQL domina el uso de recursos global en el paquete de aplicación.

#### <a name="user-resource-pool"></a>Grupo de recursos de usuario

El grupo de recursos de usuario es interno mayoría límite de administración de recursos para una base de datos, independientemente de la base de datos esté en un nivel de proceso sin servidor o aprovisionado. El grupo de recursos de usuario establece el ámbito de CPU y E/S para cargas de trabajo de usuario generados por consultas de DDL (por ejemplo, CREATE, ALTER, etc.) y DML (por ejemplo, SELECT, INSERT, UPDATE, DELETE, etcetera). Estas consultas suelen representan la proporción de uso dentro del paquete de aplicación más importante.

### <a name="metrics"></a>Métricas

|Entidad|Métrica|DESCRIPCIÓN|Unidades|
|---|---|---|---|
|Paquete de aplicación|app_cpu_percent|Porcentaje de núcleos virtuales utilizados por la aplicación con respecto al número de núcleos virtuales permitido para la aplicación.|Porcentaje|
|Paquete de aplicación|app_cpu_billed|La cantidad de procesos que se facturan de la aplicación durante el período de notificación. El importe pagado durante este período es el producto de esta métrica y el precio de unidad de memoria con núcleo virtual.<br>Los valores de esta métrica se determinan al agregar con el tiempo que se usa el número máximo de CPU y memoria usada por segundo.<br>Si la cantidad utilizada es menor que la cantidad mínima que se aprovisiona como conjunto por los núcleos virtuales mínimos y la cantidad mínima de memoria, se factura la cantidad mínima aprovisionada.  Para comparar la CPU con la memoria para la facturación, memoria se normaliza en unidades de núcleos virtuales por cambiar la escala de la cantidad de memoria en GB/3 GB por núcleo virtual.|segundos de la memoria con núcleo virtual|
|Paquete de aplicación|app_memory_percent|Porcentaje de memoria utilizada por la aplicación en relación con la máxima memoria permitida para la aplicación.|Porcentaje|
|Grupo de usuario|cpu_percent|Porcentaje de núcleos virtuales utilizados por la carga de trabajo de usuario con respecto al número de núcleos virtuales permitido para la carga de trabajo de usuario.|Porcentaje|
|Grupo de usuario|data_IO_percent|Porcentaje de datos utilizado por la carga de trabajo de usuario en relación con la E/S de datos máximo de IOPS permitido para la carga de trabajo de usuario.|Porcentaje|
|Grupo de usuario|log_IO_percent|Porcentaje de registro utilizado por la carga de trabajo de usuario en relación con MB/s de registro máximo en MB/s. permitidos para la carga de trabajo de usuario.|Porcentaje|
|Grupo de usuario|workers_percent|Porcentaje de trabajos usados por la carga de trabajo de usuario en relación con el número máximo de trabajos permitido para la carga de trabajo de usuario.|Porcentaje|
|Grupo de usuario|sessions_percent|Porcentaje de sesiones utilizadas por la carga de trabajo de usuario en relación con el número máximo de sesiones permitido para la carga de trabajo de usuario.|Porcentaje|
____

> [!NOTE]
> Las métricas en el portal de Azure están disponibles en el panel de la base de datos para una sola base de datos en **supervisión**.

### <a name="pause-and-resume-status"></a>Estado de pausa y reanudación

En el portal de Azure, se muestra el estado de la base de datos en el panel de información general del servidor que se enumera las bases de datos que contiene. El estado de la base de datos también se muestra en el panel de información general de la base de datos.

Con el siguiente comando de PowerShell para consultar la pausa y reanudar el estado de una base de datos:

```powershell
Get-AzSqlDatabase `
  -ResourceGroupName $resourcegroupname `
  -ServerName $servername `
  -DatabaseName $databasename `
  | Select -ExpandProperty "Status"
```

## <a name="resource-limits"></a>Límites de recursos

Para los límites de recursos, consulte [nivel de proceso sin servidor](sql-database-vCore-resource-limits-single-databases.md#serverless-compute-tier)

## <a name="billing"></a>Facturación

La cantidad de procesos que se facturan por segundo es el número máximo de uso de CPU y memoria que usa cada segundo. Si usa la cantidad de CPU y memoria utilizada es menor que la cantidad mínima aprovisionada para cada uno, se factura el importe aprovisionado. Para comparar la CPU con la memoria para la facturación, memoria se normaliza en unidades de núcleos virtuales por cambiar la escala de la cantidad de memoria en GB/3 GB por núcleo virtual.

- **Recurso facturada**: CPU y memoria
- **Importe facturado ($)**: precio unitario de memoria con núcleo virtual * max (núcleos virtuales mínimos, núcleos virtuales que se usa, GB de memoria mínima * 1/3, memoria GB usados * 1/3) 
- **Frecuencia de facturación**: por segundo

La cantidad de proceso de factura se expone mediante la métrica siguiente:

- **Métrica**: app_cpu_billed (núcleos virtuales segundos)
- **Definición**: max (núcleos virtuales mínimos, núcleos virtuales que se usa, cantidad mínima de memoria GB * 1/3, memoria GB usados * 1/3) *
- **Frecuencia de informes**: Por minuto

> [!NOTE]
> \* Esta cantidad se calcula cada segundo y se agregan durante 1 minuto.

**Ejemplo**: Considere la posibilidad de una base de datos mediante GP_S_Gen5_4 con el siguiente uso durante un período de una hora:

|Tiempo (horas: minutos)|app_cpu_billed (vCore seconds)|
|---|---|
|0:01|63|
|0:02|123|
|0:03|95|
|0:04|54|
|0:05|41|
|0:06 - 1:00|1255|
||Total: 1631|

Suponga que el precio de unidad de proceso es $0.2609/vCore/hour. A continuación, el proceso se facturan durante este período de una hora se determina mediante la siguiente fórmula: **$0.2609/vCore/hour * 1631 segundos de memoria con núcleo virtual * 3600 segundos o 1 hora = $0.1232**

## <a name="available-regions"></a>Regiones disponibles

El nivel de proceso sin servidor está disponible en todas las regiones excepto las siguientes regiones: Este de Australia Central, China, Norte de China, sur de Francia, noreste de Alemania, Alemania Central, India occidental, Corea del Sur, oeste de Sudáfrica, Norte de Reino Unido, sur de Reino Unido, oeste de Reino Unido y centro occidental de EE.UU

## <a name="next-steps"></a>Pasos siguientes

Para los límites de recursos, consulte [los límites de recursos de nivel de proceso sin servidor](sql-database-vCore-resource-limits-single-databases.md#serverless-compute-tier).