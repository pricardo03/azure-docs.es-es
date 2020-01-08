---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 5975f334eae543ea0f6ddc182170ae185ac5397a
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75468171"
---
Para crear una red virtual con el modelo de implementación de Resource Manager mediante Azure Portal, siga los pasos que se indican a continuación. Las capturas de pantalla se proporcionan a modo de ejemplos. Asegúrese de reemplazar los valores por los suyos. Para más información sobre redes virtuales, consulte [Información general sobre redes virtuales](../articles/virtual-network/virtual-networks-overview.md).

>[!NOTE]
>Si desea que esta red virtual se conecte a una ubicación local (además de crear una configuración de P2S), debe coordinarse con el administrador de la red local para delimitar un intervalo de direcciones IP que pueda usar específicamente para esta red virtual. Si existe un intervalo de direcciones duplicado en ambos lados de la conexión VPN, el tráfico no se enrutará como cabría esperar. Además, si desea conectar esta red virtual a otra red virtual, el espacio de direcciones no se puede superponer con otra red virtual. Por consiguiente, tenga cuidado al planear la configuración de red.
>
>

1. Inicie sesión en [Azure Portal](https://portal.azure.com).  En el menú de Azure Portal o en la página **Inicio**, seleccione **Crear un recurso**. Se abre la página **Nuevos**.

2. En **Buscar en Marketplace**, escriba *virtual network* y seleccione **Virtual Network** en los resultados.

   ![Página de recursos Buscar red virtual](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/search-marketplace-for-virtual-network.png "Página de recursos Buscar red virtual")

3. En la parte inferior de la página Virtual Network, en la lista **Seleccionar un modelo de implementación**, seleccione **Resource Manager** y, finalmente, haga clic en **Crear**.

   ![Selección de Resource Manager](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/resourcemanager250.png "Selección del Administrador de recursos")
4. En la página **Crear red virtual**, configure los valores de la red virtual. Al rellenar los campos, el signo de exclamación rojo se convierte en una marca de verificación verde cuando los caracteres escritos en el campo sean válidos. Es posible que algunos valores se rellenen automáticamente. En tal caso, reemplace esos valores por los que desee. La página **Crear red virtual** es similar a la del ejemplo siguiente:

   ![Validación de campos](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/vnetp2s.png "Validación de campos")
5. **Name**: escriba el nombre de la red virtual.
6. **Espacio de direcciones**: escriba el espacio de direcciones. Si tiene varios espacios de direcciones que agregar, agregue su primer espacio de direcciones. Podrá agregar más espacios de direcciones posteriormente, tras crear la red virtual.
7. **Suscripción**: verifique que la suscripción que aparece en la lista es la correcta. Puede cambiar las suscripciones mediante la lista desplegable.
8. **Grupo de recursos**: seleccione un grupo de recursos existente, o bien cree uno nuevo y escriba su nombre. Si va a crear un nuevo grupo, dé un nombre al grupo de recursos según los valores de configuración planeados. Para obtener más información sobre los grupos de recursos, visite [Información general del Administrador de recursos de Azure](../articles/azure-resource-manager/management/overview.md#resource-groups).
9. **Ubicación**: seleccione la ubicación de la red virtual. La ubicación determina dónde van a residir los recursos que se implementen en esta red virtual.
10. **Subred**: agregue el nombre y el intervalo de direcciones de la subred. Podrá agregar más subredes posteriormente, tras crear la red virtual.
11. Seleccione **Anclar al panel** si desea encontrar la red virtual fácilmente en el panel y, luego, haga clic en **Crear**.

    ![Anclar al panel](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/pintodashboard150.png "Anclar al panel")
12. Después de hacer clic en **Crear**, verá un icono en el panel que reflejará el progreso de la red virtual. El icono cambiará a medida que se vaya creando la red virtual.

    ![Creación de un icono de red virtual](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/deploying150.png "Icono de Crear red virtual")
