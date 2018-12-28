---
title: 'Flujos de trabajo de configuración de circuito en ExpressRoute: Azure | Microsoft Docs'
description: En esta página se muestran los flujos de trabajo para configurar el circuito ExpressRoute y las configuraciones entre pares
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: 3ffcc5ac2193e607573ceb93717258f5349d1f15
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53101694"
---
# <a name="expressroute-workflows-for-circuit-provisioning-and-circuit-states"></a>Flujos de trabajo de ExpressRoute para aprovisionamiento de circuitos y estados de circuitos de ExpressRoute
Esta página le guiará a través del aprovisionamiento de servicios y de los flujos de trabajo de configuración del enrutamiento a alto nivel.

![flujo de trabajo de circuito](./media/expressroute-workflows/expressroute-circuit-workflow.png)

Tanto la ilustración como los pasos correspondientes siguientes muestran las tareas que se deben realizar para aprovisionar un circuito ExpressRoute de un extremo a otro. 

1. Use PowerShell para configurar un circuito ExpressRoute. Para obtener más información, siga las instrucciones del artículo [Creación y modificación de circuitos de ExpressRoute](expressroute-howto-circuit-classic.md) .
2. Solicite conectividad al proveedor de servicios. Este proceso varía. Para obtener más información sobre cómo solicitar conectividad, póngase en contacto con su proveedor de conectividad.
3. Asegúrese de que el circuito se ha aprovisionado correctamente, para lo que debe comprobar el estado de aprovisionamiento del circuito ExpressRoute a través de PowerShell. 
4. Configure los dominios de enrutamiento. Si el proveedor de conectividad administra el nivel 3, configurará el enrutamiento del circuito. Si el proveedor de conectividad solo ofrece servicios de nivel 2, debe configurar el enrutamiento según las instrucciones que se describen en las páginas de [requisitos de enrutamiento](expressroute-routing.md) y [configuración de enrutamiento](expressroute-howto-routing-classic.md).
   
   * Habilitar el emparejamiento privado en Azure: habilite este emparejamiento para conectarse a VM/servicios en la nube implementados en redes virtuales.

   * Habilitar el emparejamiento de Microsoft: habilítelo para acceder a Office 365 y Dynamics 365. Además, se puede acceder a todos los servicios de PaaS de Azure a través del emparejamiento de Microsoft.
     
     > [!IMPORTANT]
     > Debe asegurarse de que usa un proxy o borde independientes para conectarse a Microsoft distinto del que usa para Internet. Si usa el mismo borde para ExpressRoute e Internet se producirá un enrutamiento asimétrico y causará interrupciones en la conectividad de la red.
     > 
     > 
     
     ![flujos de trabajo de enrutamiento](./media/expressroute-workflows/routing-workflow.png)
5. Vinculación de redes virtuales a circuitos de ExpressRoute: puede vincular redes virtuales a un circuito ExpressRoute. Siga las instrucciones [para vincular redes virtuales](expressroute-howto-linkvnet-arm.md) al circuito. Dichas redes virtuales pueden estar en la misma suscripción de Azure que el circuito ExpressRoute, o bien pueden estar en una suscripción diferente.

## <a name="expressroute-circuit-provisioning-states"></a>Estados de aprovisionamiento de circuitos de ExpressRoute
Cada circuito ExpressRoute tiene dos estados:

* Estado de aprovisionamiento de proveedor de servicio
* Status

Estado representa el estado de aprovisionamiento de Microsoft. Esta propiedad se establece en Habilitado al crear un circuito Expressroute.

El estado de aprovisionamiento del proveedor de conectividad representa el estado del lado del proveedor de conectividad. Puede ser *No aprovisionado*, *Aprovisionando* o *Aprovisionado*. El circuito ExpressRoute debe estar en estado Aprovisionado para poder usarlo.

### <a name="possible-states-of-an-expressroute-circuit"></a>Posibles estados de un circuito ExpressRoute
En esta sección se enumeran los posibles estados de un circuito ExpressRoute.

**En tiempo de creación**

Verá el circuito ExpressRoute en el estado siguiente en cuanto ejecute el cmdlet de PowerShell para crear el circuito ExpressRoute.

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


**Cuando el proveedor de conectividad está en el proceso de aprovisionamiento del circuito**

Verá el circuito ExpressRoute en el estado siguiente en cuanto pase la clave de servicio al proveedor de conectividad y hayan iniciado el proceso de aprovisionamiento.

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled


**Cuando el proveedor de conectividad haya completado el proceso de aprovisionamiento**

Verá el circuito ExpressRoute en el estado siguiente en cuanto el proveedor de conectividad haya completado el proceso de aprovisionamiento.

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled

Aprovisionado y habilitado es el único estado en que puede estar el circuito para poder usarlo. Si usa un proveedor de nivel 2, puede configurar el enrutamiento para el circuito solo cuando se encuentre en este estado.

**Cuando el proveedor de conectividad está desaprovisionando el circuito**

Si solicitó al proveedor de servicios que desaprovisione el circuito ExpressRoute, verá el circuito establecido en el estado siguiente una vez que el proveedor de servicios haya completado el proceso de desaprovisionamiento.

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


Si es necesario, puede volver a habilitarlo, o bien ejecutar los cmdlets de PowerShell para eliminar el circuito.  

> [!IMPORTANT]
> Si ejecuta el cmdlet de PowerShell para eliminar el circuito cuando el estado de ServiceProviderProvisioningState es Aprovisionamiento o Aprovisionado, se producirá un error en la operación. Colabore con el proveedor de conectividad para desaprovisionar el circuito ExpressRoute primero y, después, eliminar el circuito. Microsoft seguirá facturando el circuito hasta que se ejecute el cmdlet de PowerShell para eliminar el circuito.
> 
> 

## <a name="routing-session-configuration-state"></a>Estado de configuración de sesión de enrutamiento
El estado de aprovisionamiento de BGP permite saber si la sesión BGP se ha habilitado en el borde de Microsoft. El estado debe habilitarse para poder usar la configuración entre pares.

Es importante comprobar el estado de sesión de BGP, especialmente para la configuración entre pares de Microsoft. Además del estado de aprovisionamiento de BGP, hay otro estado denominado *estado de prefijos públicos anunciados*. El estado de los prefijos públicos anunciados debe estar en estado *configurado* , tanto para que la sesión BGP esté activa como para que el enrutamiento funcione de un extremo a otro. 

Si el estado de los prefijos públicos anunciados se establece en el estado de *validación necesaria* , la sesión BGP no se habilita, ya que los prefijos anunciados no coincidían con el número AS en ninguno de los registros de enrutamiento. 

> [!IMPORTANT]
> Si el estado de los prefijos públicos anunciados es *validación manual* , debe abrir una incidencia de soporte técnico en el [servicio de soporte técnico de Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) y aportar una evidencia de que posee la dirección IP anunciada, junto con el número del sistema autónomo asociado.
> 
> 

## <a name="next-steps"></a>Pasos siguientes
* Configure su conexión ExpressRoute.
  
  * [Creación de un circuito ExpressRoute](expressroute-howto-circuit-arm.md)
  * [Configuración del enrutamiento](expressroute-howto-routing-arm.md)
  * [Vinculación de redes virtuales a circuitos ExpressRoute](expressroute-howto-linkvnet-arm.md)

