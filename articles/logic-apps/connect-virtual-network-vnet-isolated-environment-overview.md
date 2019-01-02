---
title: Acceso a instancias de Azure Virtual Network desde Azure Logic Apps mediante entornos de servicio de integración (ISE)
description: En esta introducción se describe cómo los entornos de servicio de integración (ISE) facilitan el acceso de las aplicaciones lógicas a las redes virtuales (VNET) de Azure.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 12/03/2018
ms.openlocfilehash: 5046b1012e0074e9548cad050c16eef25c00cee0
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52845202"
---
# <a name="access-to-azure-virtual-network-resources-from-azure-logic-apps-by-using-integration-service-environments-ises"></a>Acceso a recursos de Azure Virtual Network desde Azure Logic Apps mediante entornos de servicio de integración (ISE)

> [!NOTE]
> Esta funcionalidad está en *versión preliminar privada*. Para solicitar acceso, [cree su solicitud para unirse aquí](https://aka.ms/iseprivatepreview).

A veces, sus cuentas de integración y aplicaciones lógicas necesitan acceder a recursos protegidos, como máquinas virtuales y otros sistemas o servicios dentro de una [red virtual de Azure](../virtual-network/virtual-networks-overview.md). Para configurar este acceso, puede [crear un *entorno de servicio de integración* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) para ejecutar las aplicaciones lógicas y las cuentas de integración. 

![Selección del entorno de servicio de integración](./media/connect-virtual-network-vnet-isolated-environment-overview/select-logic-app-integration-service-environment.png)

Al crear una instancia de ISE, se implementa una instancia de Logic Apps aislada y privada en la red virtual de Azure. La instancia privada usa recursos dedicados, como almacenamiento, y se ejecuta aparte del servicio Logic Apps público "global". Esta separación también ayuda a reducir el impacto que podrían tener otros inquilinos de Azure en el rendimiento de la aplicación; también conocido como el [efecto "vecinos ruidosos"](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors). 

En esta introducción se describe cómo un ISE proporciona a las aplicaciones lógicas y las cuentas de integración acceso directo a la red virtual de Azure y se comparan las diferencias entre un ISE y el servicio Logic Apps global.

<a name="difference"></a>

## <a name="isolated-versus-global"></a>Diferencias entre aislamiento y servicio global

Cuando crea un entorno de servicio integrado (ISE) de Azure, puede seleccionar una red virtual de Azure como entorno de *inserción*. Azure implementa una instancia privada del servicio Logic Apps en la red virtual. Esta acción crea un entorno aislado donde puede crear y ejecutar las aplicaciones lógicas en recursos dedicados. Cuando crea una aplicación lógica, puede seleccionar este entorno como ubicación de la aplicación, lo que proporciona a su aplicación lógica acceso directo a los recursos de la red virtual. 

Las aplicaciones lógicas de una instancia de ISE proporcionan las mismas experiencias de usuario y funcionalidades similares que el servicio global de Logic Apps. No solo puede usar las mismas acciones y conectores integrados del servicio Logic Apps global, sino que también puede usar conectores específicos de ISE. Por ejemplo, a continuación, se muestran algunos conectores estándares que ofrecen versiones que se ejecutan en una instancia de ISE:
 
* Azure Blob Storage, File Storage y Table Storage
* Azure Queues, Azure Service Bus, Azure Event Hubs e IBM MQ
* FTP y SFTP-SSH
* SQL Server, SQL Data Warehouse, Azure Cosmos DB
* AS2, X12 y EDIFACT

La diferencia entre los conectores de ISE y de otro tipo radica en las ubicaciones donde se ejecutan los desencadenadores y las acciones:

* En el ISE, los desencadenadores y acciones integrados, como HTTP, siempre se ejecutan en el mismo ISE que la aplicación lógica. 

* En el caso de conectores que ofrecen dos versiones, una versión se ejecuta en un ISE y la otra lo hace en el servicio Logic Apps global.  

  Los conectores que tienen la etiqueta **ISE** siempre se ejecutan en la misma instancia de ISE que la aplicación lógica. Los conectores sin la etiqueta **ISE** se ejecutan en el servicio global de Logic Apps. 

  ![Selección de los conectores de ISE](./media/connect-virtual-network-vnet-isolated-environment-overview/select-ise-connectors.png)

* Los conectores que se ejecutan en un ISE también están disponibles en el servicio Logic Apps global. 

> [!IMPORTANT]
> Las aplicaciones lógicas, las acciones integradas y los conectores que se ejecutan en la instancia de ISE utilizan un plan de tarifa diferente, no uno basado en el consumo. Para obtener más información, consulte [Precios de Logic Apps](../logic-apps/logic-apps-pricing.md).

<a name="vnet-access"></a>

## <a name="permissions-for-virtual-network-access"></a>Permisos de acceso para la red virtual

Cuando crea un entorno del servicio de integración (ISE), selecciona una red virtual de Azure donde *insertar* su entorno. La inserción implementa una instancia privada del servicio Logic Apps en la red virtual. Esta acción da como resultado un entorno aislado donde puede crear y ejecutar las aplicaciones lógicas en recursos dedicados. Al crear las aplicaciones lógicas, seleccione su ISE como ubicación de la aplicación. Estas aplicaciones lógicas pueden acceder directamente a la red virtual y conectarse a los recursos de esa red. 

Para los sistemas que están conectados a una red virtual, puede insertar un ISE en esa red para que las aplicaciones lógicas puedan acceder directamente a esos sistemas mediante cualquiera de estos elementos: 

* Conector de ISE para ese sistema (por ejemplo, SQL Server)

* Acción HTTP 

* Conector personalizado

Para los sistemas locales que no están conectados a una red virtual o que no tienen conectores ISE, puede conectarse a esos sistemas mediante la [configuración y el uso de la puerta de enlace de datos local](../logic-apps/logic-apps-gateway-install.md).

Antes de seleccionar una red virtual de Azure para insertar su entorno, debe configurar los permisos de control de acceso basado en rol (RBAC) en la red virtual para el servicio Azure Logic Apps. Esta tarea requiere asignar los roles **Colaborador de la red** y **Colaborador clásico** en el servicio Azure Logic Apps.
Para configurar estos permisos, consulte [Conexión a redes virtuales de Azure desde aplicaciones lógicas](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#vnet-access).

<a name="create-integration-account-environment"></a>

## <a name="integration-accounts-with-ise"></a>Cuentas de integración con ISE

Puede usar cuentas de integración con aplicaciones lógicas dentro de un entorno de servicio de integración (ISE). Sin embargo, esas cuentas de integración deben usar el *mismo ISE* que las aplicaciones lógicas vinculadas. Las aplicaciones lógicas de una instancia de ISE solo pueden hacer referencia a aquellas cuentas de integración que se encuentran en la misma instancia de ISE. Cuando se crea una cuenta de integración, puede seleccionar la instancia de ISE como la ubicación de la cuenta de integración.

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">foro de Azure Logic Apps</a>.
* Para enviar ideas sobre características o votar sobre ellas, visite el <a href="https://aka.ms/logicapps-wish" target="_blank">sitio de comentarios de los usuarios de Logic Apps</a>.

## <a name="next-steps"></a>Pasos siguientes

* Aprenda a [conectarse a las redes virtuales de Azure desde aplicaciones lógicas aisladas](../logic-apps/connect-virtual-network-vnet-isolated-environment.md).
* Consulte más información sobre [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).
* Obtenga información sobre la [integración de redes virtuales para los servicios de Azure](../virtual-network/virtual-network-for-azure-services.md).
