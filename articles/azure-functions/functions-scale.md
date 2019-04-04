---
title: Escalado y hospedaje de Azure Functions | Microsoft Docs
description: Obtenga información sobre cómo elegir entre el plan de consumo de Azure Functions y Premium.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure functions, funciones, plan de consumo, plan premium, procesamiento de eventos, webhooks, proceso dinámico, arquitectura sin servidor
ms.assetid: 5b63649c-ec7f-4564-b168-e0a74cb7e0f3
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 03/27/2019
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f09fded38e384126a8dfdbe567ce4a3ebd5b1af4
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58893595"
---
# <a name="azure-functions-scale-and-hosting"></a>Escalado y hospedaje de Azure Functions

Las funciones de Azure se ejecuta en dos planes distintos: Plan de consumo y plan Premium (versión preliminar pública). El plan de consumo agrega automáticamente la capacidad de proceso cuando se ejecuta el código. La aplicación se escala horizontalmente cuando es necesario para administrar la carga y reducir verticalmente cuando se detiene la ejecución de código. No tiene que pagar por VM inactivas ni reservar capacidad de antemano.  El plan Premium también automáticamente escalar y agregar capacidad de proceso adicional cuando se ejecuta el código.  El plan Premium incluye características adicionales, como instancias de proceso premium, la capacidad de conservar las instancias semiactiva indefinidamente y conectividad de red virtual.  Si tiene un Plan de App Service, también puede ejecutar las aplicaciones de función dentro de ellos.

> [!NOTE]  
> Ambos [plan Premium](https://azure.microsoft.com/blog/uncompromised-serverless-scale-for-enterprise-workloads-with-the-azure-functions-premium-plan/preview/) y [plan de consumo para Linux](https://azure.microsoft.com/updates/azure-functions-consumption-plan-for-linux-preview/) están actualmente en versión preliminar.

Si todavía no está familiarizado con Azure Functions, vea la [Información general sobre Azure Functions](functions-overview.md).

Cuando crea una aplicación de función, elige un plan de hospedaje para las funciones en la aplicación. En ambos planes, las funciones se ejecutan en una instancia del *host de Azure Functions*. El tipo de plan controla:

* Cómo se escalan horizontalmente las instancias de host.
* Los recursos que están disponibles para cada host.
* Características de instancia como conectividad de red virtual.

> [!NOTE]
> Puede cambiar entre los planes de consumo y Premium cambiando la propiedad del plan del recurso de aplicación de función.

## <a name="consumption-plan"></a>Plan de consumo

Cuando se usa el plan de consumo, las instancias del host de Azure Functions se agregan dinámicamente y se quitan en función del número de eventos de entrada. Este plan sin servidor se escala automáticamente y solo se le cobra por los recursos de proceso cuando se ejecutan las funciones. En un plan de consumo, se agota el tiempo de espera de una ejecución de función tras un período de tiempo configurable.

La facturación se basa en el número de ejecuciones, el tiempo de ejecución y el uso de la memoria. La facturación es un agregado de todas las funciones dentro de la aplicación de función. Para más información, consulte la [página de precios de Azure Functions].

El plan de consumo es el plan de hospedaje predeterminado y ofrece las siguientes ventajas:

* Pague solo cuando se ejecutan las funciones.
* Escale horizontalmente de forma automática, incluso durante períodos de gran carga.

## <a name="premium-plan-public-preview"></a>Plan Premium (versión preliminar)

Cuando se usa el plan Premium, las instancias del host de Azure Functions se agregan rápidamente y se quitan en función del número de eventos de entrada al igual que el plan de consumo.  Sin embargo, el plan Premium también ofrece:

* Instancias siempre activos para evitar cualquier inicio en frío.
* Conectividad de red virtual.
* Duración de ejecución ilimitado.
* Tamaños de instancia Premium (un núcleo, cuatro instancias de núcleo y dos núcleos).
* Opciones de precios de predicción.
* Asignación de aplicación de alta densidad para los planes con varias aplicaciones de función.

Puede encontrar información sobre cómo configurar estas opciones en el [plan premium de Azure Functions](functions-premium-plan.md).

En lugar de facturación por ejecución y memoria utilizada, la facturación se basa en el número de núcleos por segundo y GB-segundos usar entre las instancias reservadas y necesarias.  Al menos una instancia es necesaria que sea caliente en todo momento, existe un costo mensual fijo por plan que está activo (independientemente del número de ejecuciones).

Tenga en cuenta el plan premium de Azure Functions en los casos siguientes:
* La aplicación de función se ejecuta de forma continua, o casi continua. En este caso, un plan de App Service puede ser más rentable.
* Necesita más opciones de CPU o memoria que las que proporciona el plan de consumo.
* El código necesita ejecuciones que superan el [tiempo de ejecución máximo permitido](#timeout) en el plan de consumo.
* Necesita características que solo están disponibles en un plan de App Service, como la compatibilidad con el entorno de App Service, la conectividad VNET/VPN y mayores tamaños de máquina virtual.

> [!NOTE]
> La vista previa del plan premium admite actualmente las funciones que se ejecutan en. NET, nodo o Java a través de la infraestructura de Windows.

Al ejecutar las funciones de JavaScript en un plan Premium, debe elegir una instancia con menos vCPU. Para obtener más información, consulte el [elija los planes Premium de núcleo único](functions-reference-node.md#considerations-for-javascript-functions).  

## <a name="app-service-plan"></a>Plan de App Service

También pueden ejecutar las aplicaciones de función en las mismas de máquinas virtuales dedicadas como otras aplicaciones de App Service (Basic, Standard, Premium y las SKU aisladas). Los planes de App Service admiten Linux.

Considere el plan de App Service en los casos siguientes:

* Tiene máquinas virtuales infrautilizadas que ya ejecutan otras instancias de App Service.
* Quiere ejecutar la aplicación de función en Linux o quiere proporcionar una imagen personalizada en la cual ejecutar las funciones.

Se paga el mismo para las aplicaciones de función en un Plan de App Service como lo haría para otros recursos de App Service, como aplicaciones web. Para más información acerca del funcionamiento del plan de App Service, consulte [Introducción detallada sobre los planes de Azure App Service](../app-service/overview-hosting-plans.md). 

Con un plan de App Service, se puede escalar horizontalmente de forma manual mediante la incorporación de más instancias de máquina virtual o se puede habilitar el escalado automático. Para obtener más información, consulte [Escalación del recuento de instancias de forma manual o automática](../azure-monitor/platform/autoscale-get-started.md?toc=%2fazure%2fapp-service%2ftoc.json). También puede escalar verticalmente eligiendo un plan de App Service diferente. Vea [Escalado vertical de aplicaciones en Azure](../app-service/web-sites-scale.md) para obtener más información. 

Al ejecutar funciones de JavaScript en un plan de App Service, debe elegir un plan con menos vCPU. Para obtener más información, consulte [elija planes de App Service solo núcleo](functions-reference-node.md#choose-single-vcpu-app-service-plans). 
<!-- Note: the portal links to this section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 

### <a name="always-on"></a> Siempre activado

Si se ejecuta en un plan de App Service, debe habilitar la configuración **Always On** para que la aplicación de función se ejecute correctamente. En un plan de App Service, el tiempo de ejecución de las funciones queda inactivo después de unos minutos de inactividad, por lo que solo los desencadenadores HTTP podrán "reactivar" las funciones. Always On solo está disponible en un plan de App Service. En un plan de consumo, la plataforma activa automáticamente las aplicaciones de función.

[!INCLUDE [Timeout Duration section](../../includes/functions-timeout-duration.md)]

## <a name="what-is-my-hosting-plan"></a>¿Cuál es mi plan de hospedaje?

Para determinar el plan de hospedaje utilizado por la aplicación de función, consulte **Plan de App Service/plan de tarifa** en la pestaña **Introducción** para la aplicación de función en [Azure Portal](https://portal.azure.com). Los planes de App Service, también se indica el plan de tarifa. 

![Vista del plan de escalado en el portal](./media/functions-scale/function-app-overview-portal.png)

También puede usar la CLI de Azure para determinar el plan, como sigue:

```azurecli-interactive
appServicePlanId=$(az functionapp show --name <my_function_app_name> --resource-group <my_resource_group> --query appServicePlanId --output tsv)
az appservice plan list --query "[?id=='$appServicePlanId'].sku.tier" --output tsv
```  

Cuando el resultado de este comando es `dynamic`, la aplicación de función está en el plan de consumo. Cuando el resultado de este comando es `ElasticPremium`, la aplicación de función está en el plan Premium.  Todos los demás valores indican los niveles de un plan de App Service.

Incluso con Always On habilitado, el tiempo de espera de ejecución para las funciones individuales se controla mediante la opción `functionTimeout` en el archivo de proyecto [host.json](functions-host-json.md#functiontimeout).

## <a name="storage-account-requirements"></a>Requisitos de la cuenta de almacenamiento

En cualquier plan, una aplicación de función requiere una cuenta de Azure Storage general que admite el almacenamiento de Azure Blob, cola, tabla y archivos. Esto es porque las funciones basan en el almacenamiento de Azure para operaciones como la administración de desencadenadores y las ejecuciones de la función de registro, pero algunas cuentas de almacenamiento no admiten las colas y tablas. Estas cuentas, que incluyen las cuentas de almacenamiento solo para blobs (incluido almacenamiento Premium) y las cuentas de almacenamiento de uso general con replicación de almacenamiento con redundancia de zona, se filtran horizontalmente de las selecciones de **Cuenta de almacenamiento** existentes cuando se crea una aplicación de función.

<!-- JH: Does using a Premium Storage account improve perf? -->

Para obtener más información sobre los tipos de cuenta de almacenamiento, vea [Introducción de los servicios Azure Storage](../storage/common/storage-introduction.md#azure-storage-services).

## <a name="how-the-consumption-and-premium-plans-work"></a>Cómo funcionan los planes de consumo y premium

En el consumo y los planes premium, el controlador de escalado escala automáticamente recursos de CPU y memoria mediante la adición de instancias adicionales de lo host de Functions, en función del número de eventos desencadenados por sus funciones. Cada instancia del host de funciones en el plan de consumo se limita a 1,5 GB de memoria y 1 CPU.  Una instancia del host es la aplicación de función completa, lo que significa que todas las funciones dentro de un recurso compartido de aplicación de función en una instancia y escala al mismo tiempo. Aplicaciones de función que comparten el mismo plan de consumo se escalan de forma independiente.  En el plan premium, el tamaño de plan determinará la memoria disponible y la CPU para todas las aplicaciones de ese plan en esa instancia.  

Los archivos de código de función se almacenan en recursos compartidos de archivos de Azure en la cuenta de almacenamiento principal de la función. Al eliminarse la cuenta de almacenamiento principal de la aplicación de función, los archivos de código de función también se eliminan y no se pueden recuperar.

> [!NOTE]
> Al usar un desencadenador de blobs en un plan de consumo, puede haber un retraso de hasta 10 minutos en el procesamiento de nuevos blobs. Este retraso se produce cuando una aplicación de función ha quedado inactiva. Después de que se ejecute la aplicación de función, los blobs se procesan inmediatamente. Para evitar este retraso de inicio en frío, use el plan Premium, o usar el [desencadenador de Event Grid](functions-bindings-event-grid.md). Para más información, consulte [el artículo de referencia sobre los enlaces del desencadenador de blob](functions-bindings-storage-blob.md#trigger).

### <a name="runtime-scaling"></a>Escalado del entorno de tiempo de ejecución

Azure Functions usa un componente denominado *controlador de escala* para supervisar la tasa de eventos y determinar si se debe escalar o reducir horizontalmente. El controlador de escala usa la heurística para cada tipo de desencadenador. Por ejemplo, al usar un desencadenador de Azure Queue Storage, se escala en función de la longitud de la cola y la antigüedad del mensaje más antiguo de la cola.

La unidad de escalado es la aplicación de función. Al escalar horizontalmente la aplicación de función, se asignan recursos adicionales para ejecutar varias instancias del host de Azure Functions. Por el contrario, si la demanda se reduce, el controlador de escala elimina instancias del host de la función. El número de instancias se reduce verticalmente hasta cero cuando no se ejecuta ninguna función en la aplicación de función.

![Controlador de escala que supervisa los eventos y la creación de instancias](./media/functions-scale/central-listener.png)

### <a name="understanding-scaling-behaviors"></a>Descripción de los comportamientos de escalado

El escalado puede variar en función de varios factores, y realizarse de forma diferente según el desencadenador y el idioma seleccionados. No obstante, hay algunos aspectos del escalado que existen actualmente en el sistema:

* Una aplicación de función única solo se escala verticalmente hasta un máximo de 200 instancias. Una única instancia puede procesar más de un mensaje o solicitud a la vez, por lo que no hay un límite establecido en el número de ejecuciones simultáneas.
* Para los desencadenadores HTTP, nuevas instancias solo se asignarán a lo sumo una vez cada segundo.
* Para los desencadenadores que no son HTTP, nuevas instancias solo se asignarán a lo sumo una vez cada 30 segundos.

Diferentes desencadenadores pueden también tener distintos límites de escalado como se describe a continuación:

* [Centro de eventos](functions-bindings-event-hubs.md#trigger---scaling)

### <a name="best-practices-and-patterns-for-scalable-apps"></a>Procedimientos recomendados y patrones para aplicaciones escalables

Hay muchos aspectos de una aplicación de función que afectarán a cómo se escala esta, incluida la configuración del host, la superficie del sistema de tiempo de ejecución y la eficacia de los recursos.  Para obtener más información, consulte la [sección de escalabilidad del artículo sobre consideraciones de rendimiento](functions-best-practices.md#scalability-best-practices). También debe tener en cuenta cómo se comportan las conexiones a medida que la aplicación de función se escala. Para más información, consulte [How to manage connections in Azure Functions](manage-connections.md) (Administración de conexiones en Azure Functions).

### <a name="billing-model"></a>Modelo de facturación

La facturación del plan de consumo se describe de manera detallada en la [página de precios de Azure Functions]. El uso se agrega en el nivel de la aplicación de función, y solo se cuenta el tiempo que el código de la función está en ejecución. Estas son las unidades de facturación:

* **Consumo de recursos en gigabytes-segundo (GB-s)**. Se calcula como una combinación del tamaño de la memoria y el tiempo de ejecución de todas las funciones de una aplicación de función. 
* **Ejecuciones**. Se cuenta cada vez que se ejecuta una función en respuesta a un desencadenador de eventos.

Puede encontrar consultas útiles y obtener información sobre cómo comprender la factura de consumo [en las preguntas más frecuentes de facturación](https://github.com/Azure/Azure-Functions/wiki/Consumption-Plan-Cost-Billing-FAQ).

[Página de precios de Azure Functions]: https://azure.microsoft.com/pricing/details/functions
