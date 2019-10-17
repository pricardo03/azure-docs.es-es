---
title: Adición o eliminación de una delegación de subred en una red virtual de Azure
titlesuffix: Azure Virtual Network
description: Aprenda a agregar o quitar una subred delegada de un servicio en Azure.
services: virtual-network
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/01/2019
ms.author: kumud
ms.openlocfilehash: 5fa340fc3c839d74f292f551b73184ea4df1c0f1
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72175946"
---
# <a name="add-or-remove-a-subnet-delegation"></a>Adición o eliminación de una delegación de subred

La delegación de subred proporciona permisos explícitos al servicio para crear los recursos específicos del servicio en la subred con un identificador único al implementar el servicio. En este artículo se describe cómo agregar o quitar una subred delegada en un servicio de Azure.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="create-the-virtual-network"></a>Crear la red virtual

En esta sección, creará una red virtual y la subred que posteriormente delegará en un servicio de Azure.

1. En la parte superior izquierda de la pantalla, seleccione **Crear un recurso** > **Redes** > **Red virtual**.
1. En **Creación de una red virtual**, escriba o seleccione esta información:

    | Configuración | Valor |
    | ------- | ----- |
    | NOMBRE | Escriba *MyVirtualNetwork*. |
    | Espacio de direcciones | Escriba *10.0.0.0/16*. |
    | Subscription | Seleccione su suscripción.|
    | Resource group | Seleccione **Crear nuevo**, escriba *myResourceGroup* y, después, seleccione **Aceptar**. |
    | Location | Seleccione **EastUS**.|
    | Subred: nombre | Escriba *mySubnet*. |
    | Subred: intervalo de direcciones | Escriba *10.0.0.0/24*. |
    |||
1. Deje el resto de opciones con el valor predeterminado y seleccione **Create** (Crear).

## <a name="permissons"></a>Permisos

Si no ha creado la subred que desea delegar en un servicio de Azure, necesita el siguiente permiso: `Microsoft.Network/virtualNetworks/subnets/write`.

El rol [Colaborador de la red ](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) integrado también contiene los permisos necesarios.

## <a name="delegate-a-subnet-to-an-azure-service"></a>Delegación de una subred a un servicio de Azure

En esta sección, delegará la subred que creó en la sección anterior en un servicio de Azure.

1. En la barra de búsqueda del portal, escriba *myVirtualNetwork*. Cuando aparezca la opción **myVirtualNetwork** en los resultados de la búsqueda, selecciónela.
2. En los resultados de la búsqueda, seleccione *myVirtualNetwork*.
3. Seleccione **Subredes** en **CONFIGURACIÓN** y, después, seleccione **mySubnet**.
4. En la página *mySubnet*, en la lista **Subnet delegation** (Delegación de subred), seleccione uno de los servicios enumerados en **Delegate subnet to a service** (Delegar subred en un servicio) (por ejemplo, **Microsoft. DBforPostgreSQL/serversv2**).  

## <a name="remove-subnet-delegation-from-an-azure-service"></a>Eliminación de la delegación de subred de un servicio de Azure

1. En la barra de búsqueda del portal, escriba *myVirtualNetwork*. Cuando aparezca la opción **myVirtualNetwork** en los resultados de la búsqueda, selecciónela.
2. En los resultados de la búsqueda, seleccione *myVirtualNetwork*.
3. Seleccione **Subredes** en **CONFIGURACIÓN** y, después, seleccione **mySubnet**.
4. En la página *mySubnet*, en la lista **Subnet delegation** (Delegación de subred), seleccione **None** (Ninguno) en los servicios enumerados en **Delegate subnet to a service** (Delegar subred en un servicio). 

## <a name="next-steps"></a>Pasos siguientes
- Aprenda a [administrar subredes en Azure](virtual-network-manage-subnet.md).
