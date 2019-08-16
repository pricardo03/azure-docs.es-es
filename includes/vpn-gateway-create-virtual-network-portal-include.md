---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 08/02/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 8b585a47d3950d232eb3e8047c12ee8949030c95
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780197"
---
Para crear una red virtual con el modelo de implementación de Resource Manager mediante Azure Portal, siga los pasos que se indican a continuación. Si va a utilizar estos pasos como tutorial, use los **valores de ejemplo**. Si no va a hacerlo, asegúrese de reemplazar los valores por los suyos. Para más información sobre redes virtuales, consulte [Información general sobre redes virtuales](../articles/virtual-network/virtual-networks-overview.md).

>[!NOTE]
>Para que esta red virtual se conecte a una ubicación local, debe coordinarse con el administrador de la red local para delimitar un intervalo de direcciones IP que pueda usar específicamente para esta red virtual. Si existe un intervalo de direcciones duplicado en ambos lados de la conexión VPN, el tráfico no se enrutará como cabría esperar. Además, si desea conectar esta red virtual a otra red virtual, el espacio de direcciones no se puede superponer con otra red virtual. Por consiguiente, tenga cuidado al planear la configuración de red.
>

1. Desde un explorador, navegue al [Portal de Azure](https://portal.azure.com) e inicie sesión con su cuenta de Azure.
2. Haga clic en **Crear un recurso**. En el campo **Buscar en el Marketplace**, escriba "Red virtual". En la lista de resultados, busque **Virtual Network** y haga clic para abrir la página **Virtual Network**.
3. Haga clic en **Create**(Crear). Se abre la página **Crear red virtual**.
4. En la página **Crear red virtual**, configure los valores de la red virtual. Al rellenar los campos, el signo de exclamación rojo se convierte en una marca de verificación verde cuando los caracteres escritos en el campo sean válidos. Use los valores siguientes:

   - **Nombre**: VNet1
   - **Espacio de direcciones**: 10.1.0.0/16
   - **Suscripción**: verifique que la suscripción que aparece en la lista es la que desea usar. Puede cambiar las suscripciones mediante la lista desplegable.
   - **Grupo de recursos**: TestRG1 (haga clic en **Crear nuevo** para crear un grupo)
   - **Ubicación**: East US
   - **Subred**: Front-end
   - **Intervalo de direcciones**: 10.1.0.0/24

   ![Crear la página de la red virtual](./media/vpn-gateway-create-virtual-network-portal-include/create-virtual-network1.png)
5. Deje DDoS como Básico, Puntos de conexión de servicio como Deshabilitados y Firewall, como deshabilitado.
6. Haga clic en **Crear** para crear la red virtual.