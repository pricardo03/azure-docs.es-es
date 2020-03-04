---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/25/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 872ba86c9e43b1f6642331908eb829605f6c19bd
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2020
ms.locfileid: "77619710"
---
Para crear una red virtual con el modelo de implementación de Resource Manager y Azure Portal, siga los pasos que se indican a continuación. Para más información acerca de redes virtuales, consulte [Introducción a las redes virtuales](../articles/virtual-network/virtual-networks-overview.md).

>[!NOTE]
>Para que esta red virtual se conecte a una ubicación local, coordínese con el administrador de la red local para delimitar un intervalo de direcciones IP que pueda usar específicamente para esta red virtual. Si existe un intervalo de direcciones duplicado en ambos lados de la conexión VPN, el tráfico se enrutará de forma inesperada. Además, si desea conectar esta red virtual a otra red virtual, el espacio de direcciones no se puede superponer con otra red virtual. Planee la configuración de red en consecuencia.
>
>

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. En **Buscar recursos, servicios y documentos (G +/)** , escriba *red virtual*.

   ![Página de recursos Buscar red virtual](./media/vpn-gateway-basic-vnet-rm-portal-include/marketplace.png "Página de recursos Buscar red virtual")
1. Seleccione **Red virtual** en los resultados de **Marketplace**.

   ![Selección de red virtual](./media/vpn-gateway-basic-vnet-rm-portal-include/marketplace-results.png "Página de recursos Buscar red virtual")
1. En la página **Red virtual**, haga clic en **Crear**.

   ![Página de red virtual](./media/vpn-gateway-basic-vnet-rm-portal-include/vnet-click-create.png "Haga clic en Crear.")
1. Una vez que haga clic en Crear, se abre la página **Crear red virtual**.

   ![Página Crear red virtual](./media/vpn-gateway-basic-vnet-rm-portal-include/create-virtual-network-page.png "Crear la página de la red virtual")
1. En la página **Crear red virtual**, configure los valores de la red virtual. Al rellenar los campos, el signo de exclamación rojo se convierte en una marca de verificación verde cuando los caracteres escritos en el campo sean válidos. Algunos valores se rellenan automáticamente, que puede sustituir por sus propios valores:

   - **Name**: escriba el nombre de la red virtual.
   - **Espacio de direcciones**: escriba el espacio de direcciones. Si tiene varios espacios de direcciones que agregar, indique aquí su primer espacio de direcciones. Podrá agregar más espacios de direcciones posteriormente, tras crear la red virtual. Si la configuración requiere un espacio de direcciones IPv6, active la casilla para especificar esa información.
   - **Suscripción**: compruebe que la suscripción que aparece en la lista es la correcta. Puede cambiar las suscripciones mediante la lista desplegable.
   - **Grupo de recursos**: seleccione un grupo de recursos existente, o bien cree uno nuevo y escriba su nombre. Si va a crear un nuevo grupo, dé un nombre al grupo de recursos según los valores de configuración planeados. Para más información sobre los grupos de recursos, consulte [Información general de Azure Resource Manager](../articles/azure-resource-manager/management/overview.md#resource-groups).
   - **Ubicación**: seleccione la ubicación de la red virtual. La ubicación determina dónde van a residir los recursos que se implementen en esta red virtual.
   - **Subred**: agregue el **nombre** y el **intervalo de direcciones** de la subred. Podrá agregar más subredes posteriormente, tras crear la red virtual.
   - **Protección contra DDos**: Seleccione **Básico**, a menos que desee usar el servicio Estándar.
   - **Puntos de conexión de servicio**: Puede dejar esta configuración como **Deshabilitado**, a menos que su configuración especifique este valor.
   - **Firewall**: Puede dejar esta configuración como **Deshabilitado**, a menos que su configuración especifique este valor.
1. Haga clic en **Crear** para comenzar la implementación de la red virtual.
