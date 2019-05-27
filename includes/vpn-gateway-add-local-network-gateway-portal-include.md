---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 12/19/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 03a4005da7794a989166f914e4ba4d03f93ae8b9
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66170126"
---
1. En el portal, haga clic en **+Crear un recurso**.
2. En el cuadro de búsqueda, escriba **puerta de enlace de red local**, a continuación, presione **Entrar** para buscar. Se devolverá una lista de resultados. Haga clic en **Puerta de enlace de red local** y haga clic en el botón **Crear** para abrir la página **Crear puerta de enlace de red local**.

   ![Creación de la puerta de enlace de red local](./media/vpn-gateway-add-local-network-gateway-portal-include/create-local-network-gateway.png "Create the local network gateway")

3. En la página **Crear puerta de enlace de red local**, especifique los valores de la puerta de enlace de red local.

   - **Nombre:** especifique el nombre del objeto de puerta de enlace de red local.
   - **Dirección IP**: es la dirección IP pública del dispositivo VPN al que desea que Azure se conecte. Especifique una dirección IP pública válida. Si no tiene la dirección IP en este momento, puede usar los valores que se muestran en el ejemplo, pero deberá volver y reemplazar la dirección IP del marcador de posición por la dirección IP pública de su dispositivo VPN. Si no lo hace, Azure no podrá conectarse.
   - **Espacio de direcciones** hace referencia a los intervalos de direcciones de la red que representa esta red local. Puede agregar varios intervalos de espacios de direcciones. Asegúrese de que los intervalos que especifique aquí no se superpongan con los de otras redes a las que quiera conectarse. Azure enrutará el intervalo de direcciones que especifique a la dirección IP del dispositivo VPN local. *Use sus propios valores aquí, y no los mostrados en el ejemplo, si quiere conectarse a su sitio local*.
   - **Configurar BGP:** usar solo al configurar BGP. En caso contrario, no seleccione esta opción.
   - **Subscription** (Suscripción): compruebe que se muestra la suscripción correcta.
   - **Grupos de recursos:** seleccione el grupo de recursos que quiere usar. Puede crear un grupo de recursos nuevo o seleccionar uno ya creado.
   - **Ubicación:** seleccione la ubicación en la que se creará este objeto. Puede seleccionar la misma ubicación en la que reside la red, pero no es obligatorio.

4. Cuando haya terminado de especificar los valores, haga clic en el botón **Crear** en la parte inferior de la página para crear la puerta de enlace de red local.
