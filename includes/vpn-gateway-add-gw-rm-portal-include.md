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
ms.openlocfilehash: 72e61a36b58c0bc666f3e19b71fb1abe842208f5
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53111723"
---
1. Inicie sesión en Azure Portal y seleccione **Crear un recurso**. Se abre la página **Nuevos**.

2. En el campo **Buscar en Marketplace**, escriba *puerta de enlace de red virtual* y seleccione **Puerta de enlace de red virtual** en la lista de búsqueda. 

3. En la página **Puerta de enlace de red virtual**, seleccione **Crear** para abrir la página **Crear puerta de enlace de red virtual**.

   ![Campos de la página Crear puerta de enlace de red virtual](./media/vpn-gateway-add-gw-rm-portal-include/gw.png "Campos de la página Crear puerta de enlace de red virtual")

4. En la página **Crear puerta de enlace de red virtual**, rellene los valores de la puerta de enlace de red virtual:

   - **Nombre**: escriba un nombre para el objeto de puerta de enlace que está creando. Este nombre es distinto del nombre de la subred de puerta de enlace. 

   - **Tipo de puerta de enlace**: seleccione **VPN** para las puertas de enlace de VPN. 

   - **Tipo de VPN**: seleccione el tipo de VPN que se especifica para la configuración. La mayoría de las configuraciones requieren un tipo de VPN **basada en enrutamiento**.

   - **SKU**: seleccione la SKU de puerta de enlace en la lista desplegable. Las SKU que aparecen en la lista desplegable dependen del tipo de VPN que seleccione. Para más información acerca de las SKU de puerta de enlace, consulte [SKU de puerta de enlace](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).

      Seleccione **Habilitar el modo activo/activo** solo si va a crear una configuración de puerta de enlace activa/activa. En caso contrario, deje este valor sin seleccionar.
  
   - **Ubicación**: puede que necesite desplazarse para ver la **ubicación**. Establezca el campo **Ubicación** en la ubicación en la que se encuentra la red virtual. Por ejemplo, **Oeste de EE. UU.** Si no establece la ubicación en la región donde se encuentra la red virtual, no aparecerá en la lista desplegable cuando seleccione una red virtual.

   - **Red virtual**: Elija la red virtual a la que quiera agregar esta puerta de enlace. Seleccione **Red virtual** para abrir la página **Elegir red virtual** y seleccione la red virtual. Si no ve la red virtual, asegúrese de que el campo **Ubicación** esté establecido en la región en la que esta se encuentra.

   - **Intervalo de direcciones de subred de puerta de enlace**: solo verá esta opción si anteriormente no creó una subred de puerta de enlace para la red virtual. Si creó una subred de puerta de enlace válida, esta opción no aparecerá.

   - **Dirección IP pública**: esta configuración especifica el objeto de dirección IP asociado con la puerta de enlace de VPN. La dirección IP pública se asigna dinámicamente a este objeto cuando se crea la puerta de enlace de VPN. Actualmente, VPN Gateway solo admite la asignación de direcciones IP públicas *dinámicas*. Sin embargo, la asignación dinámica no significa que la dirección IP cambie después de que se haya asignado a la puerta de enlace VPN. La única vez que la dirección IP pública cambia es cuando la puerta de enlace se elimina y se vuelve a crear. No cambia cuando se cambia el tamaño, se restablece o se realizan actualizaciones u otras operaciones de mantenimiento interno de una puerta de enlace VPN.
    
      - Mantenga la opción **Crear nueva** seleccionada.

      - En el cuadro de texto, escriba un nombre para la dirección IP pública.

   - **Configure BGP ASN** (Configuración de ASN de BGP): desactive esta opción, a menos que la configuración la requiera específicamente. Si es así, el ASN predeterminado es *65515*, un valor que se puede cambiar.
     
5. Compruebe la configuración y seleccione **Crear** para empezar a crear la puerta de enlace de VPN. Se validará la configuración y verá el icono **Implementación de la puerta de enlace de red virtual** en el panel. La creación de una puerta de enlace puede tardar hasta 45 minutos. Es posible que tenga que actualizar la página de portal para ver el estado completado.

6. Después de crear la puerta de enlace, compruebe la dirección IP que tiene asignada mediante la visualización de la red virtual en el portal. La puerta de enlace aparece como un dispositivo conectado. Para más información, puede seleccionar el dispositivo conectado (la puerta de enlace de red virtual).