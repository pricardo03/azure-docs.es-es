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
ms.openlocfilehash: 0206fd2b2ea0a7cfaf79aaf19052e0174645780b
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65143157"
---
# <a name="access-to-azure-virtual-network-resources-from-azure-logic-apps-by-using-integration-service-environments-ises"></a>Acceso a recursos de Azure Virtual Network desde Azure Logic Apps mediante entornos de servicio de integración (ISE)

A veces, sus cuentas de integración y aplicaciones lógicas necesitan acceder a recursos protegidos, como máquinas virtuales y otros sistemas o servicios dentro de una [red virtual de Azure](../virtual-network/virtual-networks-overview.md). Para configurar este acceso, puede [crear un *entorno de servicio de integración* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) para ejecutar las aplicaciones lógicas y las cuentas de integración. Cuando se crea una instancia de ISE, Azure implementa una instancia privada y aislada del servicio Logic Apps en su red virtual de Azure. La instancia privada usa recursos dedicados, como almacenamiento, y se ejecuta aparte del servicio Logic Apps público "global". La separación de la instancia privada aislada y la instancia global pública también ayuda a reducir el impacto que podrían tener otros inquilinos de Azure en el rendimiento de su aplicación, que también se conoce como el [efecto "vecinos ruidosos"](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors).

Después de crear el ISE, cuando vaya a crear la cuenta de integración o la aplicación lógica, puede seleccionar el ISE como ubicación de logic app o la integración de su cuenta:

![Selección del entorno de servicio de integración](./media/connect-virtual-network-vnet-isolated-environment-overview/select-logic-app-integration-service-environment.png)

La aplicación lógica ahora puede acceder directamente a los sistemas que están dentro de o conectan a la red virtual mediante cualquiera de estos elementos:

* Un conector con control de versiones ISE para ese sistema, por ejemplo, SQL Server
* Un desencadenador integrada o la acción, como la acción o el desencadenador HTTP
* Un conector personalizado

En esta introducción se describe más detalles acerca de cómo las aplicaciones lógicas no proporciona un ISE e integración de cuentas de acceso directo a la red virtual de Azure y compara las diferencias entre una instancia de ISE y el servicio global de aplicaciones lógicas.
Para los sistemas locales que no están conectados a una red virtual o no tienen los conectores de la versión de ISE, puede conectarse a esos sistemas por [cómo configurar y usar la puerta de enlace de datos local](../logic-apps/logic-apps-gateway-install.md).

> [!IMPORTANT]
> Las aplicaciones lógicas, las acciones integradas y los conectores que se ejecutan en la instancia de ISE utilizan un plan de tarifa diferente, no uno basado en el consumo. Para obtener más información, consulte [Precios de Logic Apps](../logic-apps/logic-apps-pricing.md).

<a name="difference"></a>

## <a name="isolated-versus-global"></a>Diferencias entre aislamiento y servicio global

Cuando se crea un entorno de servicio integrado (ISE) de Azure, puede seleccionar la red virtual de Azure donde desea *insertar* el ISE. Azure, a continuación, inserta o implementa una instancia privada del servicio Logic Apps en la red virtual. Esta acción crea un entorno aislado donde puede crear y ejecutar las aplicaciones lógicas en recursos dedicados. Cuando se crea la aplicación lógica, seleccione el ISE como ubicación de la aplicación, que proporciona el acceso directo de aplicación lógica a la red virtual y los recursos de esa red.

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

<a name="create-integration-account-environment"></a>

## <a name="integration-accounts-with-ise"></a>Cuentas de integración con ISE

Puede usar cuentas de integración con aplicaciones lógicas dentro de un entorno de servicio de integración (ISE). Sin embargo, esas cuentas de integración deben usar el *mismo ISE* que las aplicaciones lógicas vinculadas. Las aplicaciones lógicas de una instancia de ISE solo pueden hacer referencia a aquellas cuentas de integración que se encuentran en la misma instancia de ISE. Cuando se crea una cuenta de integración, puede seleccionar la instancia de ISE como la ubicación de la cuenta de integración.

## <a name="next-steps"></a>Pasos siguientes

* Aprenda a [conectarse a las redes virtuales de Azure desde aplicaciones lógicas aisladas](../logic-apps/connect-virtual-network-vnet-isolated-environment.md).
* Consulte más información sobre [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).
* Obtenga información sobre la [integración de redes virtuales para los servicios de Azure](../virtual-network/virtual-network-for-azure-services.md).
