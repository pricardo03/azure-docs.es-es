---
title: Escalado y hospedaje de Azure Functions | Microsoft Docs
description: Aprenda a elegir entre el plan de consumo y el plan de App Service de Azure Functions.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure Functions, funciones, plan de consumo, plan de App Service, procesamiento de eventos, webhooks, proceso dinámico, arquitectura sin servidor
ms.assetid: 5b63649c-ec7f-4564-b168-e0a74cb7e0f3
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 08/09/2018
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 08897b2085c2a8f0eafb90b77486d60a0edce190
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2019
ms.locfileid: "54359874"
---
# <a name="azure-functions-scale-and-hosting"></a>Escalado y hospedaje de Azure Functions

Azure Functions se ejecuta en dos modos diferentes: plan de consumo y plan de Azure App Service. El plan de consumo asigna automáticamente potencia de proceso cuando se ejecuta el código. La aplicación se escala horizontalmente cuando es necesario para administrar la carga, y se reduce verticalmente si no se está ejecutando el código. No tiene que pagar por VM inactivas ni reservar capacidad de antemano.

> [!NOTE]  
> El plan de consumo para Linux está [ahora en versión preliminar pública](https://azure.microsoft.com/updates/azure-functions-consumption-plan-for-linux-preview/).

Si todavía no está familiarizado con Azure Functions, vea la [Información general sobre Azure Functions](functions-overview.md).

Cuando crea una aplicación de función, elige un plan de hospedaje para las funciones en la aplicación. En ambos planes, las funciones se ejecutan en una instancia del *host de Azure Functions*. El tipo de plan controla:

* Cómo se escalan horizontalmente las instancias de host.
* Los recursos que están disponibles para cada host.

> [!IMPORTANT]
> Es necesario elegir el tipo de plan de hospedaje durante la creación de la aplicación de función. No se puede cambiar después.

En un plan App Service, puede escalar entre los niveles para asignar distintas cantidades de recursos. En el plan de consumo, Azure Functions controla automáticamente toda la asignación de recursos. 

## <a name="consumption-plan"></a>Plan de consumo

Cuando se usa un plan de consumo, las instancias del host de Azure Functions se agregan y quitan de forma dinámica según el número de eventos de entrada. Este plan sin servidor se escala automáticamente y solo se le cobra por los recursos de proceso cuando se ejecutan las funciones. En un plan de consumo, se agota el tiempo de espera de una ejecución de función tras un período de tiempo configurable.

> [!NOTE]
> El tiempo de espera predeterminado para las funciones en un plan de consumo es de cinco minutos. El valor se puede aumentar para Function App hasta un máximo de 10 minutos si se cambia la propiedad `functionTimeout` en el archivo de proyecto [host.json](functions-host-json.md#functiontimeout).

La facturación se basa en el número de ejecuciones, el tiempo de ejecución y el uso de la memoria. La facturación es un agregado de todas las funciones dentro de la aplicación de función. Para más información, consulte la [página de precios de Azure Functions].

El plan de consumo es el plan de hospedaje predeterminado y ofrece las siguientes ventajas:

* Pague solo cuando se ejecutan las funciones.
* Escale horizontalmente de forma automática, incluso durante períodos de gran carga.

## <a name="app-service-plan"></a>Plan de App Service

En el plan de App Service dedicado, las aplicaciones de función se ejecutan en VM dedicadas de los niveles Básico, Estándar y Premium, y en las SKU aisladas, al igual que otras aplicaciones de App Service. Se asignan VM dedicadas a la aplicación de función, lo que significa que el host de Functions puede estar [siempre en ejecución](#always-on). Los planes de App Service admiten Linux.

Considere el plan de App Service en los casos siguientes:

* Tiene máquinas virtuales infrautilizadas que ya ejecutan otras instancias de App Service.
* La aplicación de función se ejecuta de forma continua, o casi continua. En este caso, un plan de App Service puede ser más rentable.
* Necesita más opciones de CPU o memoria que las que proporciona el plan de consumo.
* El código debe ejecutarse por más tiempo que máximo de ejecución máximo permitido en el plan de consumo, que son 10 minutos.
* Necesita características que solo están disponibles en un plan de App Service, como la compatibilidad con el entorno de App Service, la conectividad VNET/VPN y mayores tamaños de máquina virtual.
* Quiere ejecutar la aplicación de función en Linux o quiere proporcionar una imagen personalizada en la cual ejecutar las funciones.

Una máquina virtual separa el coste del número de ejecuciones, el tiempo de ejecución y el uso de la memoria. Como resultado, no tendrá que pagar más que el coste de la instancia de máquina virtual que asigna. Para más información acerca del funcionamiento del plan de App Service, consulte [Introducción detallada sobre los planes de Azure App Service](../app-service/overview-hosting-plans.md). 

Con un plan de App Service, se puede escalar horizontalmente de forma manual mediante la incorporación de más instancias de máquina virtual o se puede habilitar el escalado automático. Para obtener más información, consulte [Escalación del recuento de instancias de forma manual o automática](../azure-monitor/platform/autoscale-get-started.md?toc=%2fazure%2fapp-service%2ftoc.json). También puede escalar verticalmente eligiendo un plan de App Service diferente. Vea [Escalado vertical de aplicaciones en Azure](../app-service/web-sites-scale.md) para obtener más información. 

Al ejecutar funciones de JavaScript en un plan de App Service, debe elegir un plan con menos vCPU. Para obtener más información, consulte [Elección de los planes de App Service de un solo núcleo](functions-reference-node.md#considerations-for-javascript-functions).  

<!-- Note: the portal links to this section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 
<a name="always-on"></a>
### <a name="always-on"></a>Always On

Si se ejecuta en un plan de App Service, debe habilitar la configuración **Always On** para que la aplicación de función se ejecute correctamente. En un plan de App Service, el tiempo de ejecución de las funciones queda inactivo después de unos minutos de inactividad, por lo que solo los desencadenadores HTTP podrán "reactivar" las funciones. Always On solo está disponible en un plan de App Service. En un plan de consumo, la plataforma activa automáticamente las aplicaciones de función.

## <a name="what-is-my-hosting-plan"></a>¿Cuál es mi plan de hospedaje?

Para determinar el plan de hospedaje utilizado por la aplicación de función, consulte **Plan de App Service/plan de tarifa** en la pestaña **Introducción** para la aplicación de función en [Azure Portal](https://portal.azure.com). Los planes de App Service, también se indica el plan de tarifa. 

![Vista del plan de escalado en el portal](./media/functions-scale/function-app-overview-portal.png)

También puede usar la CLI de Azure para determinar el plan, como sigue:

```azurecli-interactive
appServicePlanId=$(az functionapp show --name <my_function_app_name> --resource-group <my_resource_group> --query appServicePlanId --output tsv)
az appservice plan list --query "[?id=='$appServicePlanId'].sku.tier" --output tsv
```  

Cuando el resultado de este comando es `dynamic`, la aplicación de función está en el plan de consumo. Todos los demás valores indican los niveles de un plan de App Service.

Incluso con Always On habilitado, el tiempo de espera de ejecución para las funciones individuales se controla mediante la opción `functionTimeout` en el archivo de proyecto [host.json](functions-host-json.md#functiontimeout).

## <a name="storage-account-requirements"></a>Requisitos de la cuenta de almacenamiento

Tanto en el plan de consumo como en el plan de App Service, una aplicación de función requiere una cuenta de Azure Storage general que admita almacenamiento de Azure en blobs, colas, archivos y tablas. Esto es porque las Functions basa en Azure Storage para operaciones como la administración de desencadenadores y el registro de las ejecuciones de funciones, pero algunas cuentas de almacenamiento no admiten colas y tablas. Estas cuentas, que incluyen las cuentas de almacenamiento solo para blobs (incluido almacenamiento Premium) y las cuentas de almacenamiento de uso general con replicación de almacenamiento con redundancia de zona, se filtran horizontalmente de las selecciones de **Cuenta de almacenamiento** existentes cuando se crea una aplicación de función.

<!-- JH: Does using a Premium Storage account improve perf? -->

Para obtener más información sobre los tipos de cuenta de almacenamiento, vea [Introducción de los servicios Azure Storage](../storage/common/storage-introduction.md#azure-storage-services).

## <a name="how-the-consumption-plan-works"></a>Funcionamiento del plan de consumo

En el plan de consumo, el controlador de escalado escala automáticamente los recursos de CPU y memoria mediante la incorporación de instancias del host de Functions, según el número de eventos desencadenados por las funciones. Cada instancia del host de Functions se limita a 1,5 GB de memoria.  Una instancia del host es la aplicación de función, lo que significa que todas las funciones de una aplicación de función comparten recursos al mismo tiempo en una instancia y escala determinadas. Las aplicaciones de función que comparten el mismo plan de consumo se escalan de manera independiente.  

Al usar el plan de hospedaje de consumo, los archivos de código de función se almacenan en recursos compartidos de Azure Files en la cuenta de almacenamiento principal de la función. Al eliminarse la cuenta de almacenamiento principal de la aplicación de función, los archivos de código de función también se eliminan y no se pueden recuperar.

> [!NOTE]
> Al usar un desencadenador de blobs en un plan de consumo, puede haber un retraso de hasta 10 minutos en el procesamiento de nuevos blobs. Este retraso se produce cuando una aplicación de función ha quedado inactiva. Después de que se ejecute la aplicación de función, los blobs se procesan inmediatamente. Para evitar este retraso por un arranque en frío, use un plan de App Service con **Always On** habilitado, o use un desencadenador de Event Grid. Para más información, consulte [el artículo de referencia sobre los enlaces del desencadenador de blob](functions-bindings-storage-blob.md#trigger).

### <a name="runtime-scaling"></a>Escalado del entorno de tiempo de ejecución

Azure Functions usa un componente denominado *controlador de escala* para supervisar la tasa de eventos y determinar si se debe escalar o reducir horizontalmente. El controlador de escala usa la heurística para cada tipo de desencadenador. Por ejemplo, al usar un desencadenador de Azure Queue Storage, se escala en función de la longitud de la cola y la antigüedad del mensaje más antiguo de la cola.

La unidad de escalado es la aplicación de función. Al escalar horizontalmente la aplicación de función, se asignan recursos adicionales para ejecutar varias instancias del host de Azure Functions. Por el contrario, si la demanda se reduce, el controlador de escala elimina instancias del host de la función. El número de instancias se reduce verticalmente hasta cero cuando no se ejecuta ninguna función en la aplicación de función.

![Controlador de escala que supervisa los eventos y la creación de instancias](./media/functions-scale/central-listener.png)

### <a name="understanding-scaling-behaviors"></a>Descripción de los comportamientos de escalado

El escalado puede variar en función de varios factores, y realizarse de forma diferente según el desencadenador y el idioma seleccionados. No obstante, hay algunos aspectos del escalado que existen actualmente en el sistema:

* Una aplicación de función única solo se escala verticalmente hasta un máximo de 200 instancias. Una única instancia puede procesar más de un mensaje o solicitud a la vez, por lo que no hay un límite establecido en el número de ejecuciones simultáneas.
* Solo se asignarán nuevas instancias como máximo una vez cada 10 segundos.

Diferentes desencadenadores pueden también tener distintos límites de escalado como se describe a continuación:

* [Event Hubs](functions-bindings-event-hubs.md#trigger---scaling)

### <a name="best-practices-and-patterns-for-scalable-apps"></a>Procedimientos recomendados y patrones para aplicaciones escalables

Hay muchos aspectos de una aplicación de función que afectarán a cómo se escala esta, incluida la configuración del host, la superficie del sistema de tiempo de ejecución y la eficacia de los recursos.  Para obtener más información, consulte la [sección de escalabilidad del artículo sobre consideraciones de rendimiento](functions-best-practices.md#scalability-best-practices). También debe tener en cuenta cómo se comportan las conexiones a medida que la aplicación de función se escala. Para más información, consulte [How to manage connections in Azure Functions](manage-connections.md) (Administración de conexiones en Azure Functions).

### <a name="billing-model"></a>Modelo de facturación

La facturación del plan de consumo se describe de manera detallada en la [página de precios de Azure Functions]. El uso se agrega en el nivel de la aplicación de función, y solo se cuenta el tiempo que el código de la función está en ejecución. Estas son las unidades de facturación:

* **Consumo de recursos en gigabytes-segundo (GB-s)**. Se calcula como una combinación del tamaño de la memoria y el tiempo de ejecución de todas las funciones de una aplicación de función. 
* **Ejecuciones**. Se cuenta cada vez que se ejecuta una función en respuesta a un desencadenador de eventos.

[Página de precios de Azure Functions]: https://azure.microsoft.com/pricing/details/functions
