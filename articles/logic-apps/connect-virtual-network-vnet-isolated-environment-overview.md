---
title: Acceso a redes virtuales de Azure
description: Introducción sobre la forma en que los entornos de servicio de integración (ISE) facilitan el acceso de las aplicaciones lógicas a las redes virtuales (VNET) de Azure.
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 02/10/2020
ms.openlocfilehash: 1f743384f467e4559412fa1a46d48011b568d249
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2020
ms.locfileid: "77191565"
---
# <a name="access-to-azure-virtual-network-resources-from-azure-logic-apps-by-using-integration-service-environments-ises"></a>Acceso a recursos de Azure Virtual Network desde Azure Logic Apps mediante entornos de servicio de integración (ISE)

A veces, sus cuentas de integración y aplicaciones lógicas necesitan tener acceso a recursos protegidos, como máquinas virtuales y otros sistemas o servicios dentro de una red de [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Para configurar este acceso, puede [crear un *Entorno del servicio de integración* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) en el que ejecutar las aplicaciones lógicas y crear las cuentas de integración.

Cuando crea un ISE, Azure *inserta* ese ISE en la red virtual de Azure que, luego, implementa una instancia privada y aislada del servicio Logic Apps en la red virtual de Azure. La instancia privada usa recursos dedicados, como almacenamiento, y se ejecuta aparte del servicio Logic Apps público "global" multiinquilino. El hecho de separar la instancia privada aislada y la instancia global pública también ayuda a reducir el impacto que podrían tener otros inquilinos de Azure en el rendimiento de la aplicación, también conocido como el [efecto "vecinos ruidosos"](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors). Un ISE también le proporciona sus propias direcciones IP estáticas. Estas direcciones IP son independientes de las direcciones IP estáticas que comparten las aplicaciones lógicas en el servicio multiinquilino público.

Después de crear el ISE, cuando vaya a crear la aplicación lógica o la cuenta de integración, puede seleccionar el ISE como ubicación de la aplicación lógica o la cuenta de integración:

![Selección del entorno de servicio de integración](./media/connect-virtual-network-vnet-isolated-environment-overview/select-logic-app-integration-service-environment.png)

Ahora, la aplicación lógica puede acceder directamente a los sistemas que están dentro de la red virtual o conectados a esta mediante cualquiera de estos elementos, que se ejecutan dentro del mismo ISE que la aplicación lógica:

* Un conector con la etiqueta **ISE** para ese sistema
* Una acción o desencadenador integrado con la etiqueta **CORE**, como la acción o el desencadenador HTTP.
* Un conector personalizado.

En esta introducción se describen más detalles sobre cómo un ISE proporciona a las aplicaciones lógicas y las cuentas de integración acceso directo a la red de Azure Virtual Network y se comparan las diferencias entre un ISE y el servicio Logic Apps global.

> [!IMPORTANT]
> Las aplicaciones lógicas, los desencadenadores integrados, las acciones integradas y los conectores que se ejecutan en el ISE usan un plan de tarifa diferente al plan de tarifa basado en el consumo. Para saber cómo funcionan los precios y la facturación para los ISE, consulte [Modelo de precios de Logic Apps](../logic-apps/logic-apps-pricing.md#fixed-pricing). Para ver las tarifas de precios, consulte los [precios de Logic Apps](../logic-apps/logic-apps-pricing.md).
>
> El ISE también ha aumentado los límites de duración de ejecución, retención de almacenamiento, rendimiento, solicitud HTTP y tiempos de espera de respuesta, tamaños de mensaje y solicitudes del conector personalizado. 
> Para más información, consulte el artículo de [límites y configuración para Azure Logic Apps](logic-apps-limits-and-config.md).

<a name="difference"></a>

## <a name="isolated-versus-global"></a>Diferencias entre aislamiento y servicio global

Al crear un Entorno del servicio integrado (ISE) en Azure, puede seleccionar la red de Azure Virtual Network donde quiera *insertar* el ISE. Después, Azure inserta (o implementa) una instancia privada del servicio Logic Apps en la red virtual. Esta acción crea un entorno aislado donde puede crear y ejecutar las aplicaciones lógicas en recursos dedicados. Cuando cree una aplicación lógica, seleccione el ISE como ubicación de la aplicación, lo que proporciona a la aplicación lógica acceso directo a la red virtual y a los recursos contenidos en ella.

Las aplicaciones lógicas de una instancia de ISE proporcionan las mismas experiencias de usuario y funcionalidades similares que el servicio público global de Logic Apps. Puede usar los mismos desencadenadores, acciones y conectores administrados integrados que están disponibles en el servicio global de Logic Apps. Algunos conectores administrados ofrecen versiones de ISE adicionales. La diferencia existe en el lugar donde se ejecutan y las etiquetas que se muestran en el diseñador de aplicación lógica cuando se trabaja en un ISE.

![Conectores con y sin etiquetas en un ISE](./media/connect-virtual-network-vnet-isolated-environment-overview/labeled-trigger-actions-integration-service-environment.png)

* Las acciones y los desencadenadores integrados muestran la etiqueta **CORE** y se ejecutan en el mismo ISE que la aplicación lógica. Los conectores administrados en los que se muestra la etiqueta **ISE** también se ejecutan en el mismo ISE que la aplicación lógica.

  Por ejemplo, estos son algunos conectores que ofrecen versiones de ISE:

  * Azure Blob Storage, File Storage y Table Storage
  * Azure Queues, Azure Service Bus, Azure Event Hubs e IBM MQ
  * FTP y SFTP-SSH
  * SQL Server, Azure SQL Data Warehouse, Azure Cosmos DB
  * AS2, X12 y EDIFACT

* Los conectores administrados que no muestran etiquetas adicionales siempre se ejecutan en el servicio público global de Logic Apps, pero puede seguir usando estos conectores en una aplicación lógica basada en ISE.

Un ISE también proporciona mayores límites de duración de ejecución, retención de almacenamiento, rendimiento, solicitud HTTP y tiempos de espera de respuesta, tamaños de mensaje y solicitudes del conector personalizado. Para más información, consulte el artículo de [límites y configuración para Azure Logic Apps](logic-apps-limits-and-config.md).

<a name="ise-level"></a>

## <a name="ise-skus"></a>SKU de ISE

Al crear el ISE, puede seleccionar la SKU de desarrollador o SKU Premium. Las diferencias entre estas SKU son las siguientes:

* **Developer**

  Proporciona un ISE de menor costo que puede usar para experimentación, desarrollo y pruebas, pero no para producción ni para pruebas de rendimiento. La SKU de desarrollador incluye desencadenadores y acciones integrados, conectores estándar, conectores empresariales y una sola cuenta de integración de [nivel Gratis](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits) por un precio mensual fijo. Sin embargo, esta SKU no incluye ningún contrato de nivel de servicio (SLA), opciones para escalar verticalmente la capacidad o redundancia durante el reciclaje, lo que significa que puede experimentar retrasos o tiempos de inactividad.

* **Premium**

  Proporciona un ISE que puede usar para producción y que incluye soporte de SLA, desencadenadores y acciones integrados, conectores estándar, conectores empresariales, una sola cuenta de integración de [nivel Estándar](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits), opciones para el escalado vertical de la capacidad y redundancia durante el reciclaje por un precio fijo al mes.

> [!IMPORTANT]
> La opción del SKU solo está disponible durante la creación del ISE y no se puede cambiar más adelante.

Para ver las tarifas de precios, consulte los [precios de Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/). Para saber cómo funcionan los precios y la facturación para los ISE, consulte [Modelo de precios de Logic Apps](../logic-apps/logic-apps-pricing.md#fixed-pricing).

<a name="endpoint-access"></a>

## <a name="ise-endpoint-access"></a>Acceso al punto de conexión del ISE

Al crear el ISE, puede optar por usar puntos de conexión de acceso internos o externos. La selección determina si los desencadenadores de solicitudes o de webhooks de las aplicaciones lógicas del ISE pueden recibir llamadas desde fuera de la red virtual o no.

Estos puntos de conexión también afectan a la manera en que puede acceder a las entradas y salidas en el historial de ejecución de las aplicaciones lógicas.

* **Internas**: puntos de conexión privados que permiten llamadas a aplicaciones lógicas del ISE donde puede ver las entradas y salidas de las aplicaciones lógicas del historial de ejecuciones, así como acceder a dichas entradas y salidas, *solo desde dentro de la red virtual*.

* **Externas**: puntos de conexión públicos que permiten llamadas a aplicaciones lógicas del ISE donde puede ver las entradas y salidas de las aplicaciones lógicas del historial de ejecuciones, así como acceder a dichas entradas y salidas, *desde fuera de la red virtual*. Si usa grupos de seguridad de red (NSG), asegúrese de que están configurados con reglas de entrada para permitir el acceso a las entradas y salidas del historial de ejecución. Para más información, consulte [Habilitar el acceso para el ISE](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#enable-access).

> [!IMPORTANT]
> La opción del punto de conexión de acceso solo está disponible durante la creación del ISE y no se puede cambiar más adelante.

<a name="on-premises"></a>

## <a name="access-to-on-premises-data-sources"></a>Acceso a orígenes de datos locales

En lo que respecta a los sistemas locales conectados a una red de Azure Virtual Network, inserte un ISE en esa red para que las aplicaciones lógicas puedan acceder directamente a esos sistemas mediante cualquiera de estos elementos:

* Acción HTTP

* Un conector con la etiqueta ISE para ese sistema

  > [!NOTE]
  > Para usar autenticación de Windows con el conector de SQL Server en un [entorno de servicio de integración (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md), use la versión no ISE del conector con la [puerta de enlace de datos local](../logic-apps/logic-apps-gateway-install.md). La versión con etiqueta ISE no admite autenticación de Windows.

* Conector personalizado

  * Si tiene conectores personalizados que requieren la puerta de enlace de datos local y los ha creado fuera de un ISE, las aplicaciones lógicas en un ISE también pueden usar esos conectores.

  * Los conectores personalizados creados en un ISE no funcionan con la puerta de enlace de datos local. Aun así, estos conectores pueden acceder directamente a orígenes de datos locales que están conectados a la red virtual en la que se hospeda el ISE. Por tanto, es muy probable que las aplicaciones lógicas en un ISE no necesiten la puerta de enlace de datos cuando se comuniquen con esos recursos.

En el caso de sistemas locales que no están conectados a una red virtual o que no tienen conectores con etiqueta ISE, primero debe [configurar la puerta de enlace de datos local](../logic-apps/logic-apps-gateway-install.md) para que las aplicaciones lógicas puedan conectarse a esos sistemas.

<a name="create-integration-account-environment"></a>

## <a name="integration-accounts-with-ise"></a>Cuentas de integración con ISE

Puede usar cuentas de integración con aplicaciones lógicas dentro de un entorno de servicio de integración (ISE). Sin embargo, esas cuentas de integración deben usar el *mismo ISE* que las aplicaciones lógicas vinculadas. Las aplicaciones lógicas de una instancia de ISE solo pueden hacer referencia a aquellas cuentas de integración que se encuentran en la misma instancia de ISE. Cuando se crea una cuenta de integración, puede seleccionar la instancia de ISE como la ubicación de la cuenta de integración. Para saber cómo funcionan los precios y la facturación de las cuentas de integración con un ISE, consulte [Modelo de precios de Logic Apps](../logic-apps/logic-apps-pricing.md#fixed-pricing). Para ver las tarifas de precios, consulte los [precios de Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/).

## <a name="next-steps"></a>Pasos siguientes

* [Conexión a redes virtuales de Azure desde aplicaciones lógicas aisladas](../logic-apps/connect-virtual-network-vnet-isolated-environment.md).
* [Incorporación de artefactos a entornos del servicio de integración](../logic-apps/add-artifacts-integration-service-environment-ise.md)
* [Administración de entornos del servicio de integración](../logic-apps/ise-manage-integration-service-environment.md)
* Consulte más información sobre [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).
* Obtenga información sobre la [integración de redes virtuales para los servicios de Azure](../virtual-network/virtual-network-for-azure-services.md).
