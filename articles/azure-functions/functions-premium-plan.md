---
title: Plan Premium de Azure Functions (versión preliminar) | Microsoft Docs
description: Detalles y opciones de configuración (red virtual, arranques no en frío, duración de ejecución ilimitada, etc) del plan Premium de Azure Functions.
services: functions
author: jeffhollan
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 4/11/2019
ms.author: jehollan
ms.openlocfilehash: 8ad09550e572c98931346b44a6c6f84da29a85e4
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443981"
---
# <a name="azure-functions-premium-plan-preview"></a>Plan Premium de Azure Functions (versión preliminar)

El plan Premium de Azure Functions es una opción de hospedaje de las aplicaciones de funciones. El plan Premium cuenta con características como la conectividad de red virtual, arranques no en frío y hardware Premium.  Pueden implementarse varias aplicaciones de funciones en el mismo plan Premium. Este plan permite configurar el tamaño de la instancia de proceso, el tamaño del plan base y el tamaño del plan máximo.  Para ver una comparación del plan Premium y otros tipos de planes y hospedajes, consulte [Escalado y hospedaje de Azure Functions](functions-scale.md).

## <a name="create-a-premium-plan"></a>Creación de un plan Premium

[!INCLUDE [functions-premium-create](../../includes/functions-premium-create.md)]

También puede crear un plan Premium mediante [az functionapp plan create](/cli/azure/functionapp/plan#az-functionapp-plan-create) en la CLI de Azure. En el ejemplo siguiente se crea un plan de nivel _Elastic Premium 1_:

```azurecli-interactive
az functionapp plan create --resource-group <RESOURCE_GROUP> --name <PLAN_NAME> \
--location <REGION> --sku EP1
```

En este ejemplo, reemplace `<RESOURCE_GROUP>` por el grupo de recursos y `<PLAN_NAME>` por un nombre para el plan que sea único en el grupo de recursos. Especifique una [compatible `<REGION>`](#regions). Para crear un plan Premium que admita Linux, incluya la opción `--is-linux`.

Con el plan creado, puede usar [az functionapp create](/cli/azure/functionapp#az-functionapp-create) para crear la aplicación de funciones. En el portal, el plan y la aplicación se crean al mismo tiempo. 

## <a name="features"></a>Características

Las siguientes características están disponibles para las aplicaciones de funciones implementadas en planes Premium.

### <a name="pre-warmed-instances"></a>Instancias activadas previamente

Si hay un día en que no tiene lugar ningún evento ni ejecución en el plan de consumo, la aplicación puede reducirse verticalmente a cero instancias. Cuando se produzcan nuevos eventos, será necesario especializar una nueva instancia con la aplicación que se ejecute en ella.  La especialización de nuevas instancias puede tardar algún tiempo dependiendo de la aplicación.  Esta latencia adicional de la primera llamada también suele denominarse "arranque en frío basado en la aplicación".

En el plan Premium, puede tener la aplicación previamente activada en un número concreto de instancias hasta alcanzar el tamaño mínimo del plan.  Las instancias activadas previamente también permiten escalar una aplicación de forma anticipada antes de que llegue una carga elevada. Cuando la aplicación realiza el escalado horizontal, primero escala las instancias activadas previamente. Otras instancias seguirán preparándose y calentando motores en previsión de la nueva operación de escalado. Al contar con una serie de instancias activadas previamente, se pueden evitar eficazmente las latencias de los arranques en frío.  Las instancias activadas previamente son una característica del plan Premium que requiere que haya al menos una instancia en ejecución disponible siempre que el plan esté activo.

Puede configurar el número de instancias activadas previamente en Azure Portal. Para ello, seleccione una aplicación de funciones en **Function App**, vaya a la pestaña **Características de la plataforma** y seleccione las opciones de **Escalar horizontalmente**. En la ventana de edición de la aplicación de funciones, las instancias activadas previamente que aparecen son específicas de esa aplicación, pero el número mínimo y máximo de instancias se aplica a todo el plan.

![Configuración del escalado elástico](./media/functions-premium-plan/scale-out.png)

Las instancias activadas previamente para una aplicación también se pueden configurar con la CLI de Azure.

```azurecli-interactive
az resource update -g <resource_group> -n <function_app_name>/config/web --set properties.preWarmedInstanceCount=<desired_prewarmed_count> --resource-type Microsoft.Web/sites
```

### <a name="private-network-connectivity"></a>Conectividad de red privada

Las instancias de Azure Functions implementadas en un plan Premium tienen la ventaja de disfrutar de la [nueva integración con red virtual para aplicaciones web](../app-service/web-sites-integrate-with-vnet.md).  Cuando se configura, la aplicación puede comunicarse con los recursos de la red virtual o protegerse mediante puntos de conexión de servicio.  Las restricciones de IP también están disponibles en la aplicación para restringir el tráfico entrante.

Cuando se asigne una subred a la aplicación de funciones en un plan Premium, necesitará una subred con suficientes direcciones IP para cada posible instancia. Aunque el número máximo de instancias puede variar durante la vigencia de la versión preliminar, necesitamos un bloque de IP que tenga al menos 100 direcciones disponibles.

Para más información, consulte este artículo sobre la [integración de una aplicación de funciones con una red virtual](functions-create-vnet.md).

### <a name="rapid-elastic-scale"></a>Escalado elástico rápido

Otras instancias de proceso se agregan automáticamente a la aplicación utilizando la misma lógica de escalado rápido que el plan de consumo.  Para más información sobre el funcionamiento del escalado, consulte este artículo sobre el [escalado y hospedaje de funciones](./functions-scale.md#how-the-consumption-and-premium-plans-work).

### <a name="unbounded-run-duration"></a>Duración ilimitada de la ejecución

Azure Functions en un plan de consumo que impone un límite de 10 minutos en cada ejecución.  En el plan Premium, la duración de ejecución predeterminada es de 30 minutos para evitar ejecuciones descontroladas. Sin embargo, puede [modificar la configuración de host.json](./functions-host-json.md#functiontimeout) para que sea ilimitada en las aplicaciones del plan Premium.

En la versión preliminar, la duración no puede garantizarse pasados 12 minutos. Para tener más posibilidades de que la ejecución sobrepase los 30 minutos, es mejor que, al escalar la aplicación, no se sobrepase el número mínimo de instancias de trabajo.

## <a name="plan-and-sku-settings"></a>Configuración del plan y la SKU

Cuando se crea un plan, hay que configurar dos opciones: el número mínimo de instancias (o tamaño de plan) y el límite máximo de ráfaga.  El número mínimo de instancias de un plan Premium es 1, mientras que la ráfaga máxima durante la versión preliminar es 20.  Las instancias mínimas están reservadas y siempre en ejecución.

> [!IMPORTANT]
> Se le cobrará por cada instancia especificada en el número mínimo de instancias independientemente de si las funciones se ejecutan o no.

Si la aplicación necesita instancias que superan el tamaño del plan, puede seguir realizando el escalado horizontal hasta que el número de instancias alcance el límite máximo de ráfaga.  Las instancias que superen el tamaño del plan solo se cobrarán cuando estén en ejecución y las tenga alquiladas.  Haremos todo lo posible para escalar horizontalmente la aplicación con arreglo al límite máximo definido y garantizaremos siempre que el uso de las instancias mínimas del plan para la aplicación.

Puede configurar el tamaño del plan y establecer valores máximos en Azure Portal en función de las opciones de **Escalar horizontalmente** seleccionadas o de una aplicación de funciones implementada en el plan (en **Características de la plataforma**).

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

A continuación se muestran las regiones admitidas actualmente en la versión preliminar pública de cada sistema operativo.

|Region| Windows | Linux |
|--| -- | -- |
|Este de Australia| ✔ | |
|Sudeste de Australia | ✔ | ✔ |
|Centro de Canadá| ✔ |  |
|Centro de EE. UU.| ✔ |  |
|Asia oriental| ✔ |  |
|East US | | ✔ |
|Este de EE. UU. 2| ✔ |  |
|Centro de Francia| ✔ |  |
|Este de Japón|  | ✔ |
|Oeste de Japón| ✔ | |
|Corea Central| ✔ |  |
|Centro-Norte de EE. UU| ✔ |  |
|Europa del Norte| ✔ | ✔ |
|Centro-Sur de EE. UU| ✔ |  |
|Sur de la India | ✔ | |
|Sudeste asiático| ✔ | ✔ |
|Oeste de Reino Unido| ✔ |  |
|Europa occidental| ✔ | ✔ |
|Oeste de la India| ✔ |  |
|Oeste de EE. UU.| ✔ | ✔ |

## <a name="known-issues"></a>Problemas conocidos

Puede realizar un seguimiento del estado de los problemas conocidos de la [versión preliminar pública en GitHub](https://github.com/Azure/Azure-Functions/wiki/Premium-plan-known-issues).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Información sobre las opciones de escalado y hospedaje de Azure Functions](functions-scale.md)
