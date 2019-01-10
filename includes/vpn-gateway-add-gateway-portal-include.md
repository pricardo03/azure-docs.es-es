---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/24/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 35f987f26ce47c19e3d5eb1ca5d2bb32d0c7ad1b
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/15/2018
ms.locfileid: "53444379"
---
1. A la izquierda de la página del portal, haga clic en **+** y escriba "Puerta de enlace de red virtual" en la búsqueda. En **Resultados**, busque y haga clic en **Puerta de enlace de red virtual**.
2. En la parte inferior de la página "Puerta de enlace de red virtual", haga clic en **Crear**. Se abre la página **Crear puerta de enlace de red virtual**.
3. En la página **Crear puerta de enlace de red virtual**, especifique los valores de la puerta de enlace de red virtual.

    ![Campos de la página Crear puerta de enlace de red virtual](./media/vpn-gateway-add-gateway-portal-include/create-gateway.png "Nueva puerta de enlace")

   - **Nombre**: Asigne un nombre a la puerta de enlace. Esta acción no es igual a la de asignación de un nombre a una subred de puerta de enlace. Este es el nombre del objeto de puerta de enlace que va a crear.
   - **Tipo de puerta de enlace**: Seleccione **VPN**. Las puertas de enlace VPN usan el tipo de puerta de enlace de red virtual **VPN**. 
   - **Tipo de VPN**: seleccione el tipo de VPN que se especifica para la configuración. La mayoría de las configuraciones requieren un tipo de VPN basada en enrutamiento.
   - **SKU**: seleccione la SKU de puerta de enlace en la lista desplegable. Las SKU que aparecen en la lista desplegable dependen del tipo de VPN que seleccione. Para más información acerca de las SKU de puerta de enlace, consulte [SKU de puerta de enlace](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).

     Seleccione **Habilitar el modo activo/activo** solo si va a crear una configuración de puerta de enlace activa/activa. En caso contrario, deje este valor sin seleccionar.
   - **Ubicación**: puede que necesite desplazarse para ver la ubicación. Ajuste el campo **Ubicación** para que apunte a la ubicación en la que se encuentra la red virtual. Por ejemplo, Oeste de EE. UU. Si la ubicación no apunta a la región en que reside la red virtual, al seleccionar una red virtual en el paso siguiente, esta no aparecerá en la lista desplegable.
   - **Red virtual**: Elija la red virtual a la que quiera agregar esta puerta de enlace. Haga clic en **Red virtual** para abrir la página "Elegir una red virtual". Seleccione la red virtual. Si no ve la red virtual, asegúrese de que el campo Ubicación apunta a la región en la que esta se encuentra.
   - **Intervalo de direcciones de subred de puerta de enlace**: solo verá esta opción si anteriormente no creó una subred de puerta de enlace para la red virtual. Si creó una subred de puerta de enlace válida, esta opción no aparecerá.
   - **Dirección IP pública**: esta configuración especifica el objeto de dirección IP pública que se asocia a la puerta de enlace de VPN. La dirección IP pública se asigna dinámicamente a este objeto cuando se crea la puerta de enlace de VPN. Actualmente, VPN Gateway solo admite la asignación de direcciones IP públicas *dinámicas*. Sin embargo, esto no significa que la dirección IP cambia después de que se ha asignado a una puerta de enlace VPN. La única vez que la dirección IP pública cambia es cuando la puerta de enlace se elimina y se vuelve a crear. No cambia cuando se cambia el tamaño, se restablece o se realizan actualizaciones u otras operaciones de mantenimiento interno de una puerta de enlace VPN.

     - Mantenga la opción **Crear nueva** seleccionada.
     - En el cuadro de texto, escriba un **nombre** para la dirección IP pública.

4. Deje la opción **Configurar ASN BGP** sin seleccionar, a menos que su configuración requiera específicamente este valor. Si necesita esta configuración, el valor predeterminado del ASN es 65515, aunque esto se puede cambiar.
5. Compruebe la configuración. Si desea que la puerta de enlace aparezca en el panel, puede seleccionar **Anclar al panel** en la parte inferior de la página. 
6. Haga clic en **Crear** para empezar a crear la puerta de enlace de VPN. Se validará la configuración y verá el icono "Implementación de la puerta de enlace de red virtual" en el panel. La creación de una puerta de enlace puede tardar hasta 45 minutos. Es posible que tenga que actualizar la página de portal para ver el estado completado.
   puerta de enlace.

Una vez creada la puerta de enlace, puede ver la dirección IP que se le ha asignado consultando la red virtual en el portal. La puerta de enlace aparece como un dispositivo conectado. Puede hacer clic en el dispositivo conectado (la puerta de enlace de red virtual) para más información.