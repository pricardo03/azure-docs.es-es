---
title: Plan prémium de Azure Functions
description: Detalles y opciones de configuración (red virtual, arranques no en frío, duración de ejecución ilimitada, etc) del plan Premium de Azure Functions.
author: jeffhollan
ms.topic: conceptual
ms.date: 10/16/2019
ms.author: jehollan
ms.openlocfilehash: b373691a6b9649a43d68c9da93b49fd20536c42b
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024643"
---
# <a name="azure-functions-premium-plan"></a>Plan prémium de Azure Functions

El plan Premium de Azure Functions (a veces denominado plan Elástico Premium) es una opción de hospedaje de las aplicaciones de funciones. El plan Premium cuenta con características como la conectividad de red virtual, arranques no en frío y hardware Premium.  Pueden implementarse varias aplicaciones de funciones en el mismo plan Premium. Este plan permite configurar el tamaño de la instancia de proceso, el tamaño del plan base y el tamaño del plan máximo.  Para ver una comparación del plan Premium y otros tipos de planes y hospedajes, consulte [Escalado y hospedaje de Azure Functions](functions-scale.md).

## <a name="create-a-premium-plan"></a>Creación de un plan Premium

[!INCLUDE [functions-premium-create](../../includes/functions-premium-create.md)]

También puede crear un plan Premium mediante [az functionapp plan create](/cli/azure/functionapp/plan#az-functionapp-plan-create) en la CLI de Azure. En el ejemplo siguiente se crea un plan de nivel _Elastic Premium 1_:

```azurecli-interactive
az functionapp plan create --resource-group <RESOURCE_GROUP> --name <PLAN_NAME> \
--location <REGION> --sku EP1
```

En este ejemplo, reemplace `<RESOURCE_GROUP>` por el grupo de recursos y `<PLAN_NAME>` por un nombre para el plan que sea único en el grupo de recursos. Especifique una [compatible `<REGION>`](#regions). Para crear un plan Premium que admita Linux, incluya la opción `--is-linux`.

Con el plan creado, puede usar [az functionapp create](/cli/azure/functionapp#az-functionapp-create) para crear la aplicación de funciones. En el portal, el plan y la aplicación se crean al mismo tiempo. Para ver un ejemplo de un script de la CLI de Azure completo, consulte [Creación de una aplicación de funciones en un plan Premium](scripts/functions-cli-create-premium-plan.md).

## <a name="features"></a>Características

Las siguientes características están disponibles para las aplicaciones de funciones implementadas en planes Premium.

### <a name="pre-warmed-instances"></a>Instancias activadas previamente

Si hay un día en que no tiene lugar ningún evento ni ejecución en el plan de consumo, la aplicación puede reducirse horizontalmente a cero instancias. Cuando se produzcan nuevos eventos, será necesario especializar una nueva instancia con la aplicación que se ejecute en ella.  La especialización de nuevas instancias puede tardar algún tiempo dependiendo de la aplicación.  Esta latencia adicional de la primera llamada también suele denominarse "arranque en frío basado en la aplicación".

En el plan Premium, puede tener la aplicación previamente activada en un número concreto de instancias hasta alcanzar el tamaño mínimo del plan.  Las instancias activadas previamente también permiten escalar una aplicación de forma anticipada antes de que llegue una carga elevada. Cuando la aplicación realiza el escalado horizontal, primero escala las instancias activadas previamente. Otras instancias seguirán preparándose y calentando motores en previsión de la nueva operación de escalado. Al contar con una serie de instancias activadas previamente, se pueden evitar eficazmente las latencias de los arranques en frío.  Las instancias activadas previamente son una característica del plan Premium que requiere que haya al menos una instancia en ejecución disponible siempre que el plan esté activo.

Puede configurar el número de instancias activadas previamente en Azure Portal. Para ello, seleccione una aplicación de funciones en **Function App**, vaya a la pestaña **Características de la plataforma** y seleccione las opciones de **Escalar horizontalmente**. En la ventana de edición de la aplicación de funciones, las instancias activadas previamente que aparecen son específicas de esa aplicación, pero el número mínimo y máximo de instancias se aplica a todo el plan.

![Configuración del escalado elástico](./media/functions-premium-plan/scale-out.png)

Las instancias activadas previamente para una aplicación también se pueden configurar con la CLI de Azure.

```azurecli-interactive
az resource update -g <resource_group> -n <function_app_name>/config/web --set properties.preWarmedInstanceCount=<desired_prewarmed_count> --resource-type Microsoft.Web/sites
```

### <a name="private-network-connectivity"></a>Conectividad de red privada

Las instancias de Azure Functions implementadas en un plan Premium tienen la ventaja de disfrutar de la [nueva integración con red virtual para aplicaciones web](../app-service/web-sites-integrate-with-vnet.md).  Cuando se configura, la aplicación puede comunicarse con los recursos de la red virtual o protegerse mediante puntos de conexión de servicio.  Las restricciones de IP también están disponibles en la aplicación para restringir el tráfico entrante.

Cuando se asigne una subred a la aplicación de funciones en un plan Premium, necesitará una subred con suficientes direcciones IP para cada posible instancia. Se requiere un bloque de direcciones IP con al menos 100 direcciones disponibles.

Para más información, consulte este artículo sobre la [integración de una aplicación de funciones con una red virtual](functions-create-vnet.md).

### <a name="rapid-elastic-scale"></a>Escalado elástico rápido

Otras instancias de proceso se agregan automáticamente a la aplicación utilizando la misma lógica de escalado rápido que el plan de consumo.  Para más información sobre el funcionamiento del escalado, consulte este artículo sobre el [escalado y hospedaje de funciones](./functions-scale.md#how-the-consumption-and-premium-plans-work).

### <a name="longer-run-duration"></a>Duración de la ejecución más larga

Azure Functions en un plan de consumo que impone un límite de 10 minutos en cada ejecución.  En el plan Premium, la duración de ejecución predeterminada es de 30 minutos para evitar ejecuciones descontroladas. Sin embargo, puede [modificar la configuración de host.json](./functions-host-json.md#functiontimeout) para que sea de 60 minutos en las aplicaciones del plan Premium.

## <a name="plan-and-sku-settings"></a>Configuración del plan y la SKU

Cuando se crea un plan, hay que configurar dos opciones: el número mínimo de instancias (o tamaño de plan) y el límite máximo de ráfaga.  Las instancias mínimas están reservadas y siempre en ejecución.

> [!IMPORTANT]
> Se le cobrará por cada instancia especificada en el número mínimo de instancias independientemente de si las funciones se ejecutan o no.

Si la aplicación necesita instancias que superan el tamaño del plan, puede seguir realizando el escalado horizontal hasta que el número de instancias alcance el límite máximo de ráfaga.  Las instancias que superen el tamaño del plan solo se cobrarán cuando estén en ejecución y las tenga alquiladas.  Haremos todo lo posible para escalar horizontalmente la aplicación con arreglo al límite máximo definido y garantizaremos siempre que el uso de las instancias mínimas del plan para la aplicación.

Puede configurar el tamaño del plan y establecer valores máximos en Azure Portal seleccionando las opciones **Escalar horizontalmente** en el plan o una aplicación de funciones implementada en el plan (en **Características de la plataforma**).

También puede aumentar el límite máximo de ráfaga mediante la CLI de Azure:

```azurecli-interactive
az resource update -g <resource_group> -n <premium_plan_name> --set properties.maximumElasticWorkerCount=<desired_max_burst> --resource-type Microsoft.Web/serverfarms 
```

### <a name="available-instance-skus"></a>SKU de instancias disponibles

Cuando cree o escale un plan, podrá elegir entre tres tamaños de instancia.  Se le facturará el número total de núcleos y de memoria consumida por segundo.  La aplicación puede escalar horizontalmente de forma automática en varias instancias cuando sea necesario.  

|SKU|Núcleos|Memoria|Storage|
|--|--|--|--|
|EP1|1|3,5 GB|250 GB|
|EP2|2|7 GB|250 GB|
|EP3|4|14 GB|250 GB|

## <a name="regions"></a>Regions

A continuación se muestran las regiones admitidas actualmente para cada SO.

|Region| Windows | Linux |
|--| -- | -- |
|Centro de Australia| ✔<sup>1</sup> | |
|Centro de Australia 2| ✔<sup>1</sup> | |
|Este de Australia| ✔ | |
|Sudeste de Australia | ✔ | ✔<sup>1</sup> |
|Sur de Brasil| ✔<sup>2</sup> |  |
|Centro de Canadá| ✔ |  |
|Centro de EE. UU.| ✔ |  |
|Asia oriental| ✔ |  |
|Este de EE. UU. | ✔ | ✔<sup>1</sup> |
|Este de EE. UU. 2| ✔ |  |
|Centro de Francia| ✔ |  |
|Centro-oeste de Alemania| ✔ | |
|Japón Oriental| ✔ | ✔<sup>1</sup> |
|Japón Occidental| ✔ | |
|Centro de Corea del Sur| ✔ |  |
|Centro-Norte de EE. UU| ✔ |  |
|Norte de Europa| ✔ | ✔<sup>1</sup> |
|Centro-sur de EE. UU.| ✔ | ✔<sup>1</sup> |
|India del Sur | ✔ | |
|Sudeste asiático| ✔ | ✔<sup>1</sup> |
|Sur de Reino Unido 2| ✔ | |
|Oeste de Reino Unido| ✔ |  |
|Oeste de Europa| ✔ | ✔<sup>1</sup> |
|Oeste de la India| ✔ |  |
|Oeste de EE. UU.| ✔ | ✔<sup>1</sup> |
|Oeste de EE. UU. 2| ✔ |  |

<sup>1</sup>Escalado horizontal máximo limitado a 20 instancias.  
<sup>2</sup>Escalado horizontal máximo limitado a 60 instancias.


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Información sobre las opciones de escalado y hospedaje de Azure Functions](functions-scale.md)
