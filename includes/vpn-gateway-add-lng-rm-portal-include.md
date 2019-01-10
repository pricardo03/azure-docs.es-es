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
ms.openlocfilehash: d9825ea41937dc9436fe8b465b48b378e13407c1
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/15/2018
ms.locfileid: "53444363"
---
1. En el portal, en **Todos los recursos**, haga clic en **+Agregar**.
2. En el cuadro de búsqueda de la página **Todo**, escriba **Puerta de enlace de red local** y después haga clic para devolver una lista de recursos. Haga clic en **Puerta de enlace de red local** para abrir la página y, a continuación, haga clic en **Crear** para abrir la página **Crear puerta de enlace de red local**.

   ![crear una puerta de enlace de red local](./media/vpn-gateway-add-lng-rm-portal-include/lng.png)
3. En la página **Crear puerta de enlace de red local**, especifique los valores de la puerta de enlace de red local.

   - **Nombre:** especifique el nombre del objeto de puerta de enlace de red local. Si es posible, use algo intuitivo, como **ClassicVNetLocal** o **TestVNet1Local**. Esto facilita la identificación de la puerta de enlace de red local en el portal.
   - **Dirección IP**: Especifique una **dirección IP** pública válida para el dispositivo VPN o la puerta de enlace de red virtual a la que desea conectarse.

     * **Si esta red local representa una ubicación local:** Especifique la dirección IP pública del dispositivo VPN al que quiere conectarse. No puede encontrarse detrás de NAT y debe estar al alcance de Azure.
     * **Si esta red local representa otra red virtual:** especifique la dirección IP pública que se asignó a la puerta de enlace de red virtual de esa red virtual.
     * **Si aún no tiene la dirección IP:** puede crear una dirección IP de marcador de posición válida y luego volver y modificar esta configuración antes de la conexión.
   - **Espacio de direcciones** hace referencia a los intervalos de direcciones de la red que representa esta red local. Puede agregar varios intervalos de espacios de direcciones. Asegúrese de que los intervalos que especifique aquí no se superpongan con los de otras redes a las que quiere conectarse.
   - **Configurar BGP:** usar solo al configurar BGP. En caso contrario, no seleccione esta opción.
   - **Subscription** (Suscripción): compruebe que se muestra la suscripción correcta.
   - **Grupos de recursos:** seleccione el grupo de recursos que quiere usar. Puede crear un grupo de recursos nuevo o seleccionar uno ya creado.
   - **Ubicación:** seleccione la ubicación en la que se creará este objeto. Puede seleccionar la misma ubicación en la que reside la red, pero no es obligatorio.

4. Haga clic en **Crear** para crear la puerta de enlace de red local.
