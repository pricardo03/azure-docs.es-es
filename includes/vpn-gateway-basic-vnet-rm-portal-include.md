---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 835f23f98ebe56e0b19081f07dc3302ef93b27b9
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66171588"
---
Para crear una red virtual con el modelo de implementación de Resource Manager y Azure Portal, siga los pasos que se indican a continuación. Para más información acerca de redes virtuales, consulte [Introducción a las redes virtuales](../articles/virtual-network/virtual-networks-overview.md).

>[!NOTE]
>Para que esta red virtual se conecte a una ubicación local, coordínese con el administrador de la red local para delimitar un intervalo de direcciones IP que pueda usar específicamente para esta red virtual. Si existe un intervalo de direcciones duplicado en ambos lados de la conexión VPN, el tráfico se enrutará de forma inesperada. Además, si desea conectar esta red virtual a otra red virtual, el espacio de direcciones no se puede superponer con otra red virtual. Planee la configuración de red en consecuencia.
>
>

1. Inicie sesión en [Azure Portal](http://portal.azure.com) y después seleccione **Crear un recurso**. Se abre la página **Nuevos**.

2. En el campo **Buscar en Marketplace**, escriba *red virtual* y seleccione **Red virtual** en la lista de resultados. Se abre la página **Red virtual**.

   ![Busque la página de recursos de red virtual](./media/vpn-gateway-basic-vnet-rm-portal-include/newvnetportal700.png "Busque la página de recursos de red virtual")

3. En la lista **Seleccionar un modelo de implementación** cerca de la parte inferior de la página, seleccione **Resource Manager** y, luego, haga clic en **Crear**. Se abre la página **Crear red virtual**.

   ![Página Crear red virtual](./media/vpn-gateway-basic-vnet-rm-portal-include/vnet.png "Página Crear red virtual")

4. En la página **Crear red virtual**, configure los valores de la red virtual. Al rellenar los campos, el signo de exclamación rojo se convierte en una marca de verificación verde cuando los caracteres escritos en el campo sean válidos. Algunos valores se rellenan automáticamente, que puede sustituir por sus propios valores:

   - **Nombre**: escriba el nombre de la red virtual.

   - **Espacio de direcciones**: escriba el espacio de direcciones. Si tiene varios espacios de direcciones que agregar, indique aquí su primer espacio de direcciones. Podrá agregar más espacios de direcciones posteriormente, tras crear la red virtual.

   - **Suscripción**: compruebe que la suscripción que aparece en la lista es la correcta. Puede cambiar las suscripciones mediante la lista desplegable.

   - **Grupo de recursos**: seleccione un grupo de recursos existente, o bien cree uno nuevo y escriba su nombre. Si va a crear un nuevo grupo, dé un nombre al grupo de recursos según los valores de configuración planeados. Para más información sobre los grupos de recursos, consulte [Información general de Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md#resource-groups).

   - **Ubicación**: seleccione la ubicación de la red virtual. La ubicación determina dónde van a residir los recursos que se implementen en esta red virtual.

   - **Subred**: agregue el **nombre** y el **intervalo de direcciones** de la subred. Podrá agregar más subredes posteriormente, tras crear la red virtual. 
     
5. Seleccione **Crear**.
