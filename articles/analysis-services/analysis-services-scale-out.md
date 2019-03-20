---
title: Escalabilidad horizontal de Azure Analysis Services | Microsoft Docs
description: Replicación de servidores de Azure Analysis Services con la escalabilidad horizontal
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: eae1569cf6f7ada89f64b96fe81b154b84932a12
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58182853"
---
# <a name="azure-analysis-services-scale-out"></a>Escalabilidad horizontal de Azure Analysis Services

Con escalado horizontal, las consultas de cliente pueden distribuirse entre varios *réplicas de consulta* en un *consultar grupo*, lo que reduce los tiempos de respuesta durante las cargas de trabajo de consulta alta. También puede separar el procesamiento del grupo de consultas, lo que garantiza que las operaciones de procesamiento no afecten de forma negativa a las consultas de cliente. La escalabilidad horizontal puede configurarse en Azure Portal o con el uso de la API de REST de Analysis Services.

La escalabilidad horizontal está disponible para los servidores en el plan de tarifa Estándar. Cada réplica de consulta se factura a la misma tarifa que el servidor. Todas las réplicas de la consulta se crean en la misma región que el servidor. El número de réplicas de consultas que puede configurar está limitado por la región en la que se encuentra el servidor. Para más información, consulte la [disponibilidad por región](analysis-services-overview.md#availability-by-region). La escalabilidad horizontal no aumenta la cantidad de memoria disponible para el servidor. Para aumentar la memoria, debe actualizar el plan. 

## <a name="why-scale-out"></a>¿Por qué escalada?

En una implementación típica de servidor, un servidor actúa como servidor de procesamiento y servidor de consulta. Si el número de consultas de cliente en los modelos en el servidor supera la unidades de procesamiento de consultas (QPU) del plan de su servidor o el procesamiento del modelo se produce al mismo tiempo que las cargas de trabajo con un número elevado de consultas, el rendimiento podría bajar. 

Con escalado horizontal, puede crear un grupo de consultas con hasta siete de los recursos de réplica de consulta adicionales (ocho total, incluida su *principal* server). Puede escalar el número de réplicas en el grupo de consulta para satisfacer las demandas QPU en los momentos críticos, y puede separar un servidor de procesamiento del grupo de consultas en cualquier momento. 

Independientemente del número de réplicas de consultas que tenga en un grupo de consultas, las cargas de trabajo de procesamiento no se distribuyen entre las réplicas de consulta. El servidor principal actúa como el servidor de procesamiento. Las réplicas de consulta sirven solo las consultas en las bases de datos de modelo sincronizados entre el servidor principal y en cada réplica en el grupo de consulta. 

Durante el escalado horizontal, puede tardar hasta cinco minutos para nuevas réplicas de consulta incrementalmente agregarse al grupo de consulta. Cuando todas las nuevas réplicas de consulta están activos y ejecución, nuevas conexiones de cliente tienen la carga equilibrada entre los recursos en el grupo de consulta. Las conexiones de cliente existentes no cambian del recurso al que están conectadas actualmente. En la reducción horizontal, se terminan las conexiones de cliente existentes a un recurso de grupo de consultas que se va a quitar del grupo de consultas. Los clientes pueden volver a conectarse a un recurso de grupo de consultas restantes.

## <a name="how-it-works"></a>Cómo funciona

Al configurar el escalado horizontal de la primera vez, son bases de datos de modelo en el servidor principal *automáticamente* sincronizada con las nuevas réplicas en un nuevo grupo de consulta. La sincronización automática se produce solo una vez. Durante la sincronización automática, se copian los archivos de datos del servidor principal (que se cifran en reposo en el almacenamiento de blobs) en una segunda ubicación, que también se cifran en reposo en el almacenamiento de blobs. Las réplicas en el grupo de consulta son, a continuación, *hidrata* con datos desde el segundo conjunto de archivos. 

Mientras se realiza una sincronización automática sólo cuando se escala horizontalmente un servidor por primera vez, también puede realizar una sincronización manual. Sincronizar garantiza que coinciden con los datos en las réplicas en el grupo de consultas del servidor principal. Al procesar los modelos (actualizar) en el servidor principal, se debe realizar una sincronización *después* se completan las operaciones de procesamiento. Esta sincronización copia datos actualizados desde los archivos del servidor principal en blob storage en el segundo conjunto de archivos. Las réplicas en el grupo de consulta, a continuación, se hidrata con los datos actualizados desde el segundo conjunto de archivos en blob storage. 

Al realizar una operación de escalado posterior, por ejemplo, aumentar el número de réplicas en el grupo de consulta de dos a cinco, las nuevas réplicas se hidrata con datos desde el segundo conjunto de archivos en blob storage. No hay ninguna sincronización. Si tuviera que, a continuación, realice una sincronización después de escalado horizontal, las nuevas réplicas en el grupo de consulta sería hidrata dos veces: una hidratación redundante. Al realizar una operación de escalado posterior, es importante tener en cuenta:

* Realizar una sincronización *antes de la operación de escalado horizontal* para evitar la hidratación redundante de las réplicas se ha agregado.

* Cuando se automatiza ambos procesamiento *y* las operaciones de escalado horizontal, es importante procesar primero los datos en el servidor principal, a continuación, realice una sincronización y, a continuación, realizar la operación de escalado horizontal. Esta secuencia garantiza un impacto mínimo en los recursos de memoria y QPU.

* Se permite la sincronización incluso cuando no hay ninguna réplica en el grupo de consulta. Si va a escalar desde cero a uno o más réplicas con nuevos datos de una operación de procesamiento en el servidor principal, realice la sincronización en primer lugar sin réplicas en el grupo de consulta y, a continuación, escalar horizontalmente. La sincronización antes de escalar horizontalmente evita hidratación redundante de las réplicas recién agregadas.

* Al eliminar una base de datos de modelo desde el servidor principal, lo que no se automáticamente eliminan de las réplicas en el grupo de consulta. Debe realizar una operación de sincronización que quita el archivo/s para esa base de datos de ubicación de almacenamiento de blobs compartida de la réplica y, a continuación, elimina la base de datos de modelo en las réplicas en el grupo de consulta.

### <a name="separate-processing-from-query-pool"></a>Separar el procesamiento del grupo de consultas

Para obtener un rendimiento máximo para las operaciones de procesamiento y consulta, puede elegir separar el servidor de procesamiento del grupo de consultas. Cuando están separados, las conexiones de cliente nuevas y existentes se asignan a réplicas de consultas en el grupo de consultas únicamente. Si las operaciones de procesamiento solo ocupan un breve período de tiempo, puede elegir separar el servidor de procesamiento del grupo de consultas solo durante el tiempo que se tarda en realizar las operaciones de procesamiento y sincronización y, a continuación, volver a incluirlo en el grupo de consultas. 

## <a name="monitor-qpu-usage"></a>Supervisión del uso de QPU

Para determinar si la escalabilidad horizontal del servidor es necesaria, supervise el servidor en Azure Portal con Métricas. Si se supera el nivel máximo de QPU con regularidad, significa que el número de consultas en los modelos excede el límite de QPU del plan. La métrica de longitud de la cola de trabajos del grupo de consultas también aumenta si el número de consultas de la cola del grupo de subprocesos de consulta excede el número de QPU disponibles. 

Otra buena métrica para inspeccionar es QPU promedio por ServerResourceType. Esta métrica compara QPU promedio para el servidor principal con la el grupo de consulta. 

### <a name="to-configure-qpu-by-serverresourcetype"></a>Para configurar QPU por ServerResourceType
1. En un gráfico de líneas de las métricas, haga clic en **agregar métrica**. 
2. En **recursos**, seleccione el servidor, a continuación, en **espacio de nombres de MÉTRICA**, seleccione **métricas estándar de Analysis Services**, a continuación, en **MÉTRICA**, Seleccione **QPU**y, a continuación, en **agregación**, seleccione **Avg**. 
3. Haga clic en **aplicar dividir**. 
4. En **valores**, seleccione **ServerResourceType**.  

Para más información, vea [Supervisión de las métricas del servidor](analysis-services-monitor.md).

## <a name="configure-scale-out"></a>Configuración de la escalabilidad horizontal

### <a name="in-azure-portal"></a>En Azure Portal

1. En Azure Portal, haga clic en **Escalabilidad horizontal**. Utilice el control deslizante para seleccionar el número de servidores de réplica de consulta. El número de réplicas elegido se suma al servidor existente.

2. En **Separar el servidor de procesamiento del grupo de consultas**, seleccione Sí para excluir el servidor de procesamiento de los servidores de consulta. Cliente [conexiones](#connections) utilizando la cadena de conexión predeterminada (sin `:rw`) se redirigen a las réplicas en el grupo de consulta. 

   ![Control deslizante de escalabilidad horizontal](media/analysis-services-scale-out/aas-scale-out-slider.png)

3. Haga clic en **Guardar** para aprovisionar los nuevos servidores de réplica de consulta. 

Al configurar el escalado horizontal para un servidor de la primera vez, los modelos en el servidor principal se sincronizan automáticamente con réplicas en el grupo de consulta. La sincronización automática sólo se produce una vez, cuando se configura escalada a una o más réplicas. Los cambios posteriores en el número de réplicas en el mismo servidor *no desencadenará otra sincronización automática*. No se producirá la sincronización automática nuevo incluso si configura el servidor de réplicas de cero y, a continuación, volver a escalar horizontalmente a cualquier número de réplicas. 

## <a name="synchronize"></a>Sincronizar 

Las operaciones de sincronización deben realizarse manualmente o mediante la API de REST.

### <a name="in-azure-portal"></a>En Azure Portal

En **Información general** > modelo > **Sincronizar el modelo**.

![Control deslizante de escalabilidad horizontal](media/analysis-services-scale-out/aas-scale-out-sync.png)

### <a name="rest-api"></a>API DE REST

Use la operación **sync**.

#### <a name="synchronize-a-model"></a>Sincronización de un modelo   

`POST https://<region>.asazure.windows.net/servers/<servername>:rw/models/<modelname>/sync`

#### <a name="get-sync-status"></a>Obtención del estado de sincronización  

`GET https://<region>.asazure.windows.net/servers/<servername>/models/<modelname>/sync`

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Antes de usar PowerShell, [instalar o actualizar el módulo Azure PowerShell más reciente](/powershell/azure/install-az-ps). 

Para ejecutar la sincronización, utilice [sincronización AzAnalysisServicesInstance](https://docs.microsoft.com/powershell/module/az.analysisservices/sync-AzAnalysisServicesinstance).

Para establecer el número de réplicas de consulta, use [conjunto AzAnalysisServicesServer](https://docs.microsoft.com/powershell/module/az.analysisservices/set-azanalysisservicesserver). Especifique el parámetro opcional `-ReadonlyReplicaCount`.

## <a name="connections"></a>Conexiones

En la página de información general del servidor, hay dos nombres de servidor. Si aún no ha configurado la escalabilidad horizontal de un servidor, los dos nombres de servidor funcionan igual. Una vez configurada la escalabilidad horizontal de un servidor, debe especificar el nombre de servidor adecuado según el tipo de conexión. 

Para las conexiones de cliente para el usuario final como Power BI Desktop, Excel y aplicaciones personalizadas, utilice **Nombre del servidor**. 

Para SSMS, SSDT y cadenas de conexión en PowerShell, las aplicaciones de Azure Functions y AMO usan el **nombre del servidor de administración**. El nombre del servidor de administración incluye un calificador especial `:rw` (lectura y escritura). Todas las operaciones de procesamiento se producen en el servidor de administración (principal).

![Nombres de servidor](media/analysis-services-scale-out/aas-scale-out-name.png)

## <a name="troubleshoot"></a>Solución de problemas

**Problema:** los usuarios ven la instancia de error **No se puede encontrar el servidor "\<Nombre del servidor >" en el modo de conexión "ReadOnly".**

**Solución:** Al seleccionar el **separar el servidor de procesamiento del grupo consultando** opción conexiones de cliente mediante la cadena de conexión predeterminada (sin `:rw`) se redirigen a las réplicas del grupo de consulta. Si las réplicas en el grupo de consultas no están en línea porque la sincronización aún no se ha completado, es posible que se produzca un error en las conexiones de cliente redirigidas. Para evitar errores de conexión, debe haber al menos dos servidores en el grupo de consultas al realizar una sincronización. Cada servidor se sincroniza individualmente mientras que otros siguen en línea. Si decide no incluir el servidor de procesamiento en el grupo de consultas durante el procesamiento, puede quitarlo del grupo para el procesamiento y agregarlo de nuevo una vez completado el procesamiento, pero antes de la sincronización. Use las métricas de memoria y QPU para supervisar el estado de sincronización.

## <a name="related-information"></a>Información relacionada

[Supervisión de las métricas del servidor](analysis-services-monitor.md)   
[Administración de Azure Analysis Services](analysis-services-manage.md) 
