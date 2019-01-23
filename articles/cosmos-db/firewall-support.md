---
title: Firewall de IP para las cuentas de Azure Cosmos
description: Aprenda a proteger los datos de Azure Cosmos DB mediante directivas de control de acceso IP para la compatibilidad con el firewall.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: govindk
ms.openlocfilehash: 2bb3665f0d0ab15f212741a6ce032fcd7ac2620d
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2019
ms.locfileid: "54358120"
---
# <a name="ip-firewall-for-azure-cosmos-accounts"></a>Firewall de IP para las cuentas de Azure Cosmos

Para proteger los datos almacenados en su cuenta, Azure Cosmos DB admite un modelo de autorización basado en secretos que usa código de autentificación de mensajes basado en hash (HMAC). Además, Azure Cosmos DB admite controles de acceso basado en IP para la compatibilidad con el firewall de entrada. Este modelo es muy parecido a las reglas de firewall de un sistema de base de datos tradicional y proporciona un nivel de seguridad adicional a la cuenta. Con los firewalls, puede configurar su cuenta de Azure Cosmos para que sea accesible solo desde un conjunto aprobado de máquinas o servicios en la nube. Para acceder a los datos almacenados en la base de datos de Azure Cosmos DB desde estos conjuntos aprobados de máquinas y servicios, seguirá siendo necesario que el autor de la llamada presente un token de autorización válido.

## <a id="ip-access-control-overview"></a>Introducción al control de acceso IP

De forma predeterminada, la cuenta de Azure Cosmos DB es accesible desde Internet siempre y cuando la solicitud vaya acompañada de un token de autorización válido. Para configurar el control de acceso basado en directivas IP, el usuario debe proporcionar el conjunto de direcciones IP o intervalos de direcciones IP en formato CIDR (Enrutamiento de interdominios sin clases) para incluirlos en la lista de direcciones IP permitidas de cliente para acceder a una cuenta de Azure Cosmos dada. Una vez que se aplica esta configuración, todas las solicitudes que se originan en máquinas que están fuera de esta lista de permitidos reciben la respuesta 403 (Prohibido). Cuando se usa el firewall de IP, se recomienda permitir que Azure Portal acceda a su cuenta. Este acceso es necesario para permitir el uso del explorador de datos, y para recuperar las métricas de la cuenta que se muestran en Azure Portal.

Puede combinar el firewall basado en IP con el control de acceso de red virtual y subred. Al combinarlos, puede limitar el acceso a cualquier origen que tenga una dirección IP pública o desde una subred específica dentro de la red virtual. Para más información sobre el control de acceso de red virtual y subred, consulte [Acceso a los recursos de Azure Cosmos DB desde redes virtuales](vnet-service-endpoint.md).

En resumen, siempre se necesita el token de autorización para acceder a una cuenta de Azure Cosmos. Si el firewall de IP y la lista de control de acceso (ACL) de red virtual no se han configurado, se puede acceder a la cuenta de Azure Cosmos con el token de autorización. Una vez que el firewall de IP o las listas ACL de red virtual, o ambos, están configurados en la cuenta de Azure Cosmos, solo las solicitudes que se originan en los orígenes especificados (y con el token de autorización) obtienen respuestas válidas. 

## <a name="next-steps"></a>Pasos siguientes

A continuación, puede configurar el firewall de IP o el punto de conexión de servicio de red virtual para la cuenta mediante los documentos siguientes:

* [How to configure IP firewall for your Azure Cosmos account](how-to-configure-firewall.md) (Configuración del firewall de IP para la cuenta de Azure Cosmos)
* [Acceso a los recursos de Azure Cosmos DB desde redes virtuales](vnet-service-endpoint.md)
* [How to configure virtual network service endpoint for your Azure Cosmos account](how-to-configure-vnet-service-endpoint.md) (Configuración del punto de conexión de servicio de red virtual para la cuenta de Azure Cosmos)




