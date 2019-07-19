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
ms.date: 05/06/2019
ms.openlocfilehash: f981452b06ec06f2be1b8fe0319cd49a678ccfe0
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67441577"
---
# <a name="access-to-azure-virtual-network-resources-from-azure-logic-apps-by-using-integration-service-environments-ises"></a>Acceso a recursos de Azure Virtual Network desde Azure Logic Apps mediante entornos de servicio de integración (ISE)

A veces, sus cuentas de integración y aplicaciones lógicas necesitan tener acceso a recursos protegidos, como máquinas virtuales y otros sistemas o servicios dentro de una red de [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Para configurar este acceso, puede [crear un *Entorno del servicio de integración* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) en el que ejecutar las aplicaciones lógicas y crear las cuentas de integración.

Al crear una instancia de ISE, Azure implementa una instancia aislada y privada del servicio Logic Apps en su red de Azure Virtual Network. La instancia privada usa recursos dedicados, como almacenamiento, y se ejecuta aparte del servicio Logic Apps público "global". El hecho de separar la instancia privada aislada y la instancia global pública también ayuda a reducir el impacto que podrían tener otros inquilinos de Azure en el rendimiento de la aplicación, también conocido como el [efecto "vecinos ruidosos"](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors).

Después de crear el ISE, cuando vaya a crear la aplicación lógica o la cuenta de integración, puede seleccionar el ISE como ubicación de la aplicación lógica o la cuenta de integración:

![Selección del entorno de servicio de integración](./media/connect-virtual-network-vnet-isolated-environment-overview/select-logic-app-integration-service-environment.png)

Ahora, la aplicación lógica puede acceder directamente a los sistemas que están dentro de la red virtual o conectados a esta mediante cualquiera de estos elementos:

* Un conector con la etiqueta **ISE** para ese sistema, como SQL Server.
* Un desencadenador o acción integrados con la etiqueta **Core**, como la acción o el desencadenador HTTP.
* Un conector personalizado.

En esta introducción se describen más detalles sobre cómo un ISE proporciona a las aplicaciones lógicas y las cuentas de integración acceso directo a la red de Azure Virtual Network y se comparan las diferencias entre un ISE y el servicio Logic Apps global.

> [!NOTE]
> Las aplicaciones lógicas, los desencadenadores integrados, las acciones integradas y los conectores que se ejecutan en el ISE usan un plan de tarifa diferente al plan de tarifa basado en el consumo. Para obtener más información, consulte [Precios de Logic Apps](../logic-apps/logic-apps-pricing.md). El ISE también ha aumentado los límites de duración de ejecución, retención de almacenamiento, rendimiento, solicitud HTTP y tiempos de espera de respuesta, tamaños de mensaje y solicitudes del conector personalizado. Para más información, consulte el artículo de [límites y configuración para Azure Logic Apps](logic-apps-limits-and-config.md).

<a name="difference"></a>

## <a name="isolated-versus-global"></a>Diferencias entre aislamiento y servicio global

Al crear un Entorno del servicio integrado (ISE) en Azure, puede seleccionar la red de Azure Virtual Network donde quiera *insertar* el ISE. Después, Azure inserta (o implementa) una instancia privada del servicio Logic Apps en la red virtual. Esta acción crea un entorno aislado donde puede crear y ejecutar las aplicaciones lógicas en recursos dedicados. Cuando cree una aplicación lógica, seleccione el ISE como ubicación de la aplicación, lo que proporciona a la aplicación lógica acceso directo a la red virtual y a los recursos contenidos en ella.

Las aplicaciones lógicas de una instancia de ISE proporcionan las mismas experiencias de usuario y funcionalidades similares que el servicio global de Logic Apps. No solo puede usar los mismos conectores integrados, acciones integradas y conectores del servicio Logic Apps global, sino que también puede usar conectores específicos del ISE. Por ejemplo, a continuación se muestran algunos conectores estándar que ofrecen versiones que se ejecutan en un ISE:

* Azure Blob Storage, File Storage y Table Storage
* Azure Queues, Azure Service Bus, Azure Event Hubs e IBM MQ
* FTP y SFTP-SSH
* SQL Server, SQL Data Warehouse, Azure Cosmos DB
* AS2, X12 y EDIFACT

La diferencia entre los conectores de ISE y de otro tipo radica en las ubicaciones donde se ejecutan los desencadenadores y las acciones:

* En el ISE, los desencadenadores y acciones integrados, como HTTP, siempre se ejecutan en el mismo ISE que la aplicación lógica y muestran la etiqueta **Core**.

  ![Selección de desencadenadores y acciones integrados "Core"](./media/connect-virtual-network-vnet-isolated-environment-overview/select-core-built-in-actions-triggers.png)

* Los conectores que se ejecutan en un ISE también tienen versiones hospedadas públicamente en el servicio Logic Apps global. En el caso de los conectores que ofrecen dos versiones, los que tienen la etiqueta **ISE** siempre se ejecutan en el mismo ISE que la aplicación lógica. Los conectores sin la etiqueta **ISE** se ejecutan en el servicio global de Logic Apps.

  ![Selección de los conectores de ISE](./media/connect-virtual-network-vnet-isolated-environment-overview/select-ise-connectors.png)

Un ISE también proporciona mayores límites de duración de ejecución, retención de almacenamiento, rendimiento, solicitud HTTP y tiempos de espera de respuesta, tamaños de mensaje y solicitudes del conector personalizado. Para más información, consulte el artículo de [límites y configuración para Azure Logic Apps](logic-apps-limits-and-config.md).

### <a name="access-to-on-premises-data-sources"></a>Acceso a orígenes de datos locales

En lo que respecta a los sistemas locales conectados a una red de Azure Virtual Network, inserte un ISE en esa red para que las aplicaciones lógicas puedan acceder directamente a esos sistemas mediante cualquiera de estos elementos:

* Conector con versión ISE para ese sistema (por ejemplo, SQL Server)
* Acción HTTP
* Conector personalizado

  * Si tiene conectores personalizados que requieren la puerta de enlace de datos local y los ha creado fuera de un ISE, las aplicaciones lógicas en un ISE también pueden usar esos conectores.
  
  * Los conectores personalizados creados en un ISE no funcionan con la puerta de enlace de datos local. Aun así, estos conectores pueden acceder directamente a orígenes de datos locales que están conectados a la red virtual en la que se hospeda el ISE. Por tanto, es muy probable que las aplicaciones lógicas en un ISE no necesiten la puerta de enlace de datos cuando se comuniquen con esos recursos.

En el caso de los sistemas locales que no están conectados a una red virtual o que no tienen conectores con versión ISE, primero debe [configurar la puerta de enlace de datos local](../logic-apps/logic-apps-gateway-install.md) para que las aplicaciones lógicas puedan conectarse a esos sistemas.

<a name="create-integration-account-environment"></a>

## <a name="integration-accounts-with-ise"></a>Cuentas de integración con ISE

Puede usar cuentas de integración con aplicaciones lógicas dentro de un entorno de servicio de integración (ISE). Sin embargo, esas cuentas de integración deben usar el *mismo ISE* que las aplicaciones lógicas vinculadas. Las aplicaciones lógicas de una instancia de ISE solo pueden hacer referencia a aquellas cuentas de integración que se encuentran en la misma instancia de ISE. Cuando se crea una cuenta de integración, puede seleccionar la instancia de ISE como la ubicación de la cuenta de integración.

## <a name="next-steps"></a>Pasos siguientes

* Aprenda a [conectarse a las redes virtuales de Azure desde aplicaciones lógicas aisladas](../logic-apps/connect-virtual-network-vnet-isolated-environment.md).
* Consulte más información sobre [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).
* Obtenga información sobre la [integración de redes virtuales para los servicios de Azure](../virtual-network/virtual-network-for-azure-services.md).
