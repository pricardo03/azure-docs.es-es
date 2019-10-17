---
title: 'Direcciones IP de entrada/salida: Azure App Service | Microsoft Docs'
description: Describe cómo se usan las direcciones IP de entrada y salida en App Service y cómo encontrar información sobre ellas para su aplicación.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/06/2019
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: de98f972a43f3845d2a01b928d90283732ef4843
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72329790"
---
# <a name="inbound-and-outbound-ip-addresses-in-azure-app-service"></a>Direcciones IP de entrada y salida en Azure App Service

[Azure App Service](overview.md) es un servicio multiinquilino, excepto en los [entornos de App Service](environment/intro.md). Las aplicaciones que no están en un entorno de App Service (no están en el [nivel aislado](https://azure.microsoft.com/pricing/details/app-service/)) comparten la infraestructura de red con otras aplicaciones. Como consecuencia, las direcciones IP de entrada y salida de una aplicación pueden ser diferentes y pueden cambiar incluso en determinadas situaciones. 

Los [entornos de App Service](environment/intro.md) usan infraestructuras de red dedicadas, así que las aplicaciones que se ejecutan en un entorno de App Service obtienen direcciones IP estáticas dedicadas tanto para las conexiones de entrada como de salida.

## <a name="when-inbound-ip-changes"></a>Cuando cambia la dirección IP de entrada

Sin importar el número de instancias escaladas horizontalmente, cada aplicación tiene una única dirección IP de entrada. La dirección IP de entrada puede cambiar al realizar una de las siguientes acciones:

- Eliminar una aplicación y volver a crearla en otro grupo de recursos.
- Eliminar la última aplicación de una combinación de grupo de recursos _y_ región y volver a crearla.
- Eliminar un enlace SSL existente, como durante la renovación de un certificado (consulte [Renew certificates](app-service-web-tutorial-custom-ssl.md#renew-certificates) [Renovación de certificados]).

## <a name="find-the-inbound-ip"></a>Buscar la dirección IP de entrada

Solo ejecute el siguiente comando en un terminal local:

```bash
nslookup <app-name>.azurewebsites.net
```

## <a name="get-a-static-inbound-ip"></a>Obtención de una dirección IP de entrada estática

En ocasiones, podría desear una dirección IP estática dedicada para la aplicación. Para obtener una dirección IP de entrada estática, debe configurar un [enlace SSL basado en IP](app-service-web-tutorial-custom-ssl.md#secure-a-custom-domain). Si realmente no necesita la funcionalidad SSL para proteger la aplicación, puede cargar incluso un certificado autofirmado para este enlace. En un enlace SSL basado en IP, el certificado se enlaza a la dirección IP propiamente dicha, de modo que App Service aprovisiona una dirección IP estática para conseguir que esto ocurra. 

## <a name="when-outbound-ips-change"></a>Cuando cambian las direcciones IP de salida

Sin importar el número de instancias escaladas horizontalmente, cada aplicación tiene un número establecido de direcciones IP de salida en cualquier momento dado. Las conexiones de salida desde la aplicación de App Service, como a una base de datos back-end, usan una de las direcciones IP de salida como dirección IP de origen. Como no puede saber con antelación qué dirección IP usará una instancia de aplicación dada para realizar la conexión de salida, el servicio back-end debe abrir su firewall a todas las direcciones IP de salida de la aplicación.

El conjunto de direcciones IP de salida de la aplicación cambia cuando se escala la aplicación entre los niveles inferiores (**Básica**, **Estándar**, **Premium**) y **Premium V2**.

Puede encontrar el conjunto de todas las posibles direcciones IP de salida que puede utilizar la aplicación, independientemente de los planes de tarifa, buscando la propiedad `possibleOutboundIpAddresses` o en el campo **Direcciones IP salientes adicionales** de la hoja **Propiedades** de Azure Portal. Consulte [Búsqueda de las direcciones IP de salida](#find-outbound-ips).

## <a name="find-outbound-ips"></a>Búsqueda de las direcciones IP de salida

Para buscar las direcciones IP de salida que usa actualmente su aplicación en Azure Portal, haga clic en **Propiedades** en el panel de navegación izquierdo de la aplicación. Se enumeran en el campo **Direcciones IP de salida**.

Puede encontrar la misma información si ejecuta el comando siguiente en [Cloud Shell](../cloud-shell/quickstart.md).

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query outboundIpAddresses --output tsv
```

```azurepowershell
(Get-AzWebApp -ResourceGroup <group_name> -name <app_name>).OutboundIpAddresses
```

Para buscar _todas_ las posibles direcciones IP de salida para la aplicación, con independencia de los planes de tarifa, haga clic en **Propiedades** en el panel de navegación izquierdo de la aplicación. Se enumeran en el campo **Direcciones IP salientes adicionales**.

Puede encontrar la misma información si ejecuta el comando siguiente en [Cloud Shell](../cloud-shell/quickstart.md).

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query possibleOutboundIpAddresses --output tsv
```

```azurepowershell
(Get-AzWebApp -ResourceGroup <group_name> -name <app_name>).PossibleOutboundIpAddresses
```

## <a name="next-steps"></a>Pasos siguientes

Aprenda a restringir el tráfico entrante por dirección IP de origen.

> [!div class="nextstepaction"]
> [Restricciones de IP estática](app-service-ip-restrictions.md)
