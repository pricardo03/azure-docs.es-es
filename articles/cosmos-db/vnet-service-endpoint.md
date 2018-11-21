---
title: Protección del acceso a una cuenta de Azure Cosmos DB con el uso de un punto de conexión de servicio de Azure Virtual Network
description: En este documento se describe el control de acceso a redes virtuales y subredes para una cuenta de Azure Cosmos.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: govindk
ms.openlocfilehash: 5f3ee4e5af32ff0ea90d8807c65d14ddce417e7b
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51626089"
---
# <a name="access-azure-cosmos-db-resources-from-virtual-networks"></a>Acceso a los recursos de Azure Cosmos DB desde redes virtuales

Puede configurar la cuenta de Azure Cosmos para permitir el acceso solo desde una subred específica de la red virtual (VNET). Si habilita el [punto de conexión de servicio](../virtual-network/virtual-network-service-endpoints-overview.md) para acceder a Azure Cosmos DB en la subred de una red virtual, el tráfico de esa subred se envía a Azure Cosmos DB con la identidad de la subred y la red virtual. Una vez habilitado el punto de conexión de servicio de Azure Cosmos DB, puede limitar el acceso a la subred agregándola a la cuenta de Azure Cosmos.

De forma predeterminada, la cuenta de Azure Cosmos DB es accesible desde Internet siempre y cuando la solicitud vaya acompañada de un token de autorización válido. Cuando agrega una o varias subredes en las redes virtuales, solo aquellas solicitudes que se originan en esas subredes recibirán una respuesta válida. Las solicitudes procedentes de cualquier otro origen recibirán una respuesta 404 (No encontrado). 

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

Estas son algunas preguntas frecuentes acerca de cómo configurar el acceso desde las redes virtuales:

### <a name="can-i-specify-both-virtual-network-service-endpoint-and-ip-access-control-policy-on-an-azure-cosmos-account"></a>¿Puedo especificar un punto de conexión de servicio de red virtual y una directiva de control de acceso de IP en una cuenta de Azure Cosmos? 

Puede habilitar el punto de conexión de servicio de red virtual y una directiva de control de acceso de IP (anteriormente conocida como firewall) en la cuenta de Azure Cosmos. Estas dos características son complementarias y aseguran de forma colectiva el aislamiento y seguridad de la cuenta de Azure Cosmos. El uso del firewall para las direcciones IP garantiza que las direcciones IP estáticas puedan acceder a la cuenta. 

### <a name="how-do-i-limit-access-to-subnet-within-a-virtual-network"></a>¿Cómo se puede limitar el acceso a la subred dentro de una red virtual? 

Hay dos pasos necesarios para limitar el acceso a la cuenta de Azure Cosmos desde una subred. En primer lugar, permita que el tráfico de la subred lleve la identidad de su subred y su red virtual a Azure Cosmos DB. Para ello, habilite el punto de conexión de servicio para Azure Cosmos DB en la subred. A continuación, agregue una regla en la cuenta de Azure Cosmos que especifique esta subred como origen desde el cual se puede acceder a la cuenta.

### <a name="will-virtual-network-acls-and-ip-firewall-reject-requests-or-connections"></a>¿Rechazarán las ACL de red virtual y el firewall para las direcciones IP las solicitudes o las conexiones? 

Cuando se agregan reglas de acceso al firewall para direcciones IP o a la red virtual, solo aquellas solicitudes procedentes de orígenes permitidos obtendrán respuestas válidas. Las demás solicitudes se rechazan con un error 404 (No encontrado). Es importante distinguir entre el firewall de la cuenta de Azure Cosmos y un firewall de un nivel de conexión. Todavía se puede conectar el origen al servicio y no se rechazan las conexiones.

### <a name="my-requests-started-getting-blocked-when-i-enabled-service-endpoint-to-azure-cosmos-db-on-the-subnet-what-happened"></a>Mis solicitudes se empezaron a bloquear cuando habilité el punto de conexión de servicio para Azure Cosmos DB en la subred. ¿Qué ha ocurrido?

Una vez que se habilita el punto de conexión de servicio para Azure Cosmos DB en una subred, el origen del tráfico que llega a Azure Cosmos DB cambia de una dirección IP pública a la red virtual y la subred. Si la cuenta de Azure Cosmos tiene solo un firewall basado en IP, el tráfico de la subred habilitada para el servicio ya no coincidirá con las reglas de firewall para direcciones IP y, por tanto, se rechazará. Recorra cada uno de los pasos para migrar fácilmente desde un firewall basado en IP a un control de acceso basado en redes virtuales.

### <a name="do-the-peered-virtual-networks-also-have-access-to-azure-cosmos-account"></a>¿Tienen las redes virtuales del mismo nivel acceso a la cuenta de Azure Cosmos? 
Solo la red virtual y las subredes que se agregaron a la cuenta de Azure Cosmos tienen acceso. Las redes virtuales del mismo nivel no podrán acceder a la cuenta hasta que las subredes dentro de ellas se agreguen a la cuenta.

### <a name="what-is-the-maximum-number-of-subnets-allowed-to-access-a-single-cosmos-account"></a>¿Cuál es el número máximo de subredes a las que se permite acceder a una sola cuenta de Cosmos? 
Actualmente, se permite un máximo de 64 subredes para una cuenta de Azure Cosmos

### <a name="can-i-enable-access-from-vpn-and-express-route"></a>¿Puedo habilitar el acceso desde VPN y ExpressRoute? 
Para acceder a una cuenta de Azure Cosmos a través de ExpressRoute desde una ubicación local, necesita habilitar el emparejamiento de Microsoft. Una vez que pone reglas de acceso para el firewall de direcciones IP o la red virtual, puede agregar las direcciones IP públicas usadas para el emparejamiento de Microsoft al firewall de direcciones IP de la cuenta de Azure Cosmos para permitir que los servicios locales accedan a esta. 

### <a name="do-i-need-to-update-the-network-security-groups-nsg-rules"></a>¿Es necesario actualizar las reglas de los grupos de seguridad de red? 
Las reglas de los grupos de seguridad de red se usan para limitar la conectividad hacia una subred y desde esta con una red virtual. Cuando agrega el punto de conexión de servicio de Azure Cosmos DB a la subred, no es necesario abrir una conectividad de salida en el grupo de seguridad de red para la cuenta de Azure Cosmos. 

### <a name="are-service-endpoints-available-for-all-vnets"></a>¿Están disponibles los puntos de conexión de servicio para todas las redes virtuales?
No, solo las redes virtuales de Azure Resource Manager pueden tener el punto de conexión de servicio habilitado. Las redes virtuales clásicas no admiten puntos de conexión de servicio.


## <a name="next-steps"></a>Pasos siguientes

* [Cómo limitar el acceso de la cuenta de Azure Cosmos a las subredes de redes virtuales](how-to-configure-vnet-service-endpoint.md)
* [How to configure IP firewall for your Azure Cosmos account](how-to-configure-firewall.md) (Configuración del firewall de IP para la cuenta de Azure Cosmos)

