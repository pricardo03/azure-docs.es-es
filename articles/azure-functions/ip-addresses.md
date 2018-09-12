---
title: Direcciones IP en Azure Functions
description: Aprenda a buscar las direcciones IP entrantes y salientes de aplicaciones de función y descubra qué es lo que hace que cambien.
services: functions
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: glenga
ms.openlocfilehash: 0fcda59add346d60f37273625fbbcf41faab0e15
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44091210"
---
# <a name="ip-addresses-in-azure-functions"></a>Direcciones IP en Azure Functions

En este artículo se explican los siguientes temas relacionados con las direcciones IP de las aplicaciones de función:

* Cómo buscar las direcciones IP que usa actualmente una aplicación de función.
* ¿Qué provoca el cambio de las direcciones IP de una aplicación de función?
* Cómo restringir las direcciones IP que pueden acceder a una aplicación de función.
* Cómo obtener las direcciones IP dedicadas para una aplicación de función.

Las direcciones IP se asocian a aplicaciones de función, no a funciones individuales. Las solicitudes HTTP entrantes no pueden usar la dirección IP de entrada para llamar a funciones individuales; deben usar el nombre de dominio predeterminado (functionappname.azurewebsites.net) o un nombre de dominio personalizado.

## <a name="function-app-inbound-ip-address"></a>Dirección IP de entrada de una aplicación de función

Cada aplicación de función tiene una única dirección IP de entrada. Para encontrar dicha dirección IP:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Vaya a la aplicación de función.
3. Seleccione **Características de la plataforma**.
4. Seleccione **Propiedades** y la dirección IP de entrada aparece en **Dirección IP virtual**.

## <a name="find-outbound-ip-addresses"></a>Direcciones IP de salida de una aplicación de función

Cada aplicación de función tiene un conjunto de direcciones IP de salida disponibles. Las conexiones de salida desde cualquier función, como a una base de datos back-end, usan una de las direcciones IP de salida disponibles como dirección IP de origen. No se puede saber con antelación qué dirección IP usará una conexión determinada. Por este motivo, el servicio back-end debe abrir su firewall a todas las direcciones IP de salida de la aplicación de función.

Para buscar las direcciones IP de salida disponibles para una aplicación de función:

1. Inicie sesión en [Azure Resource Explorer](https://resources.azure.com).
2. Seleccione **suscriptions (suscripciones) > {su suscripción} > providers (proveedores) > Microsoft.Web > sites (sitios)**.
3. En el panel JSON, busque el sitio con una propiedad `id` que acabe en el nombre de la aplicación de función.
4. Vea `outboundIpAddresses` y `possibleOutboundIpAddresses`. 

El conjunto de `outboundIpAddresses` está actualmente disponible para la aplicación de función. El conjunto de `possibleOutboundIpAddresses` incluye las direcciones IP que estarán disponibles solo si la aplicación de función [se escala a otros planes de tarifa](#outbound-ip-address-changes).

Una forma alternativa de encontrar las direcciones IP de salida disponibles es mediante [Cloud Shell](../cloud-shell/quickstart.md):

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query outboundIpAddresses --output tsv
az webapp show --resource-group <group_name> --name <app_name> --query possibleOutboundIpAddresses --output tsv
```

## <a name="data-center-outbound-ip-addresses"></a>Direcciones IP de salida del centro de datos

Si necesita incluir en la lista blanca las direcciones IP de salida que utilizan sus aplicaciones de función, otra opción es incluir en la lista blanca el centro de datos de las aplicaciones de función (región de Azure). Puede [descargar un archivo XML que contenga las direcciones IP de todos los centros de datos de Azure](https://www.microsoft.com/en-us/download/details.aspx?id=41653). Luego, busque el elemento XML que se aplique a la región en que se ejecuta la aplicación de función.

Por ejemplo, así sería el elemento XML de Europa occidental:

```
  <Region Name="europewest">
    <IpRange Subnet="13.69.0.0/17" />
    <IpRange Subnet="13.73.128.0/18" />
    <!-- Some IP addresses not shown here -->
    <IpRange Subnet="213.199.180.192/27" />
    <IpRange Subnet="213.199.183.0/24" />
  </Region>
```

 Para obtener información acerca de cuándo se actualiza este archivo y cuando cambian las direcciones IP, expanda la sección **Details** (Detalles) de la [página del Centro de descarga](https://www.microsoft.com/en-us/download/details.aspx?id=41653).

## <a name="inbound-ip-address-changes"></a>Cambios en la dirección IP de entrada

 La dirección IP de entrada **puede** cambiar cuándo se:

- Elimina una aplicación de función y se vuelve a crearla en otro grupo de recursos.
- Elimina la última aplicación de función de una combinación de grupo de recursos y región, y se vuelve a crear.
- Elimina un enlace SSL, como por ejemplo, cuando se [renuevan certificados](../app-service/app-service-web-tutorial-custom-ssl.md#renew-certificates)).

La dirección IP de entrada también puede cambiar cuando no ha realizado ninguna acción como las que se muestran.

## <a name="outbound-ip-address-changes"></a>Cambios en la dirección IP de salida

El conjunto de direcciones IP de salida disponibles para una aplicación de función puede cambiar cuando se:

* Realiza alguna acción que puede cambiar la dirección IP de entrada.
* Cambia el plan de tarifa del plan de App Service. La lista de todas las posibles direcciones IP de salida que puede utilizar una aplicación, para todos los planes de tarifa, está en la propiedad `possibleOutboundIPAddresses`. Consulte [Búsqueda de las direcciones IP de salida](#find-outbound-ip-addresses).

La dirección IP de entrada también puede cambiar cuando no ha realizado ninguna acción como las que se muestran.

Para forzar deliberadamente el cambio de la dirección IP de salida:

1. Escale el plan de App Service o redúzcalo verticalmente entre los planes de tarifa Estándar y Premium v2.
2. Espere 10 minutos.
3. Vuelva a escalar al plan inicial.

## <a name="ip-address-restrictions"></a>Restricciones de las direcciones IP

Puede configurar una lista de direcciones IP a las que desea permitir o denegar el acceso a una aplicación de función. Para más información, consulte [Restricciones de IP estáticas de Azure App Service](../app-service/app-service-ip-restrictions.md).

## <a name="dedicated-ip-addresses"></a>Direcciones IP dedicadas

Si necesita direcciones IP estáticas dedicadas, es aconsejable usar [App Service Environment](../app-service/environment/intro.md) (el [nivel Aislado](https://azure.microsoft.com/pricing/details/app-service/) de los planes de App Service). Para más información, consulte [Direcciones IP de App Service Environment](../app-service/environment/network-info.md#ase-ip-addresses) y [Control del tráfico de entrada a App Service Environment](../app-service/environment/app-service-app-service-environment-control-inbound-traffic.md).

Para averiguar si una aplicación de función se ejecuta en App Service Environment:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Vaya a la aplicación de función.
3. Seleccione la pestaña **Información general**.
4. El nivel del plan de App Service aparece en **Plan de App Service/plan de tarifa**. El plan de tarifa de App Service Environment es **Aislado**.
 
Como alternativa, puede usar [Cloud Shell](../cloud-shell/quickstart.md):

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query sku --output tsv
```

El App Service Environment `sku` es `Isolated`.

## <a name="next-steps"></a>Pasos siguientes

Una causa común de los cambios IP son los cambios de escala de las aplicaciones de función. [Más información acerca del escalado de las aplicaciones de función](functions-scale.md).
