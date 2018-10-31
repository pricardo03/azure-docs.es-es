---
title: Acceso a instancias de Azure Virtual Network desde Azure Logic Apps mediante entornos de servicio de integración (ISE)
description: En esta introducción se describe cómo los entornos de servicio de integración (ISE) facilitan el acceso de las aplicaciones lógicas a las redes virtuales de Azure.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 09/24/2018
ms.openlocfilehash: f21af23cf0b7b121441b1433f382db60ef7f13fe
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2018
ms.locfileid: "49408666"
---
# <a name="access-to-azure-virtual-network-resources-from-azure-logic-apps-by-using-integration-service-environments-ises"></a>Acceso a recursos de Azure Virtual Network desde Azure Logic Apps mediante entornos de servicio de integración (ISE)

> [!NOTE]
> Esta funcionalidad está en *versión preliminar privada*. Para solicitar acceso, [cree su solicitud para unirse aquí](https://aka.ms/iseprivatepreview).

A veces, sus cuentas de integración y aplicaciones lógicas necesitan obtener acceso a recursos protegidos, como máquinas virtuales y otros sistemas o servicios dentro de una red de [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Para configurar este acceso, puede [crear un *entorno de servicio de integración* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) que utilice como ubicación de las aplicaciones lógicas y las cuentas de integración. 

![Selección del entorno de servicio de integración](./media/connect-virtual-network-vnet-isolated-environment-overview/select-logic-app-integration-service-environment.png)

Al crear una instancia de ISE, se implementa una instancia de Logic Apps aislada y privada en la red virtual de Azure. La instancia privada usa recursos dedicados, como almacenamiento, y se ejecuta por separado desde el servicio público de Logic Apps "global". Esta separación también ayuda a reducir el impacto que podrían tener otros inquilinos de Azure en el rendimiento de la aplicación; también conocido como el [efecto "vecinos ruidosos"](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors). 

En esta introducción se describe cómo crear una instancia de ISE que ayude a las aplicaciones lógicas y a las cuentas de integración a obtener acceso directamente a los recursos de la red virtual de Azure. Además, se comparan las diferencias entre una instancia de ISE y el servicio global de Logic Apps.

<a name="difference"></a>

## <a name="isolated-versus-global"></a>Diferencias entre aislamiento y servicio global

Cuando crea un entorno del servicio integrado (ISE) de Azure, puede seleccionar una red virtual de Azure como un *elemento del mismo nivel* para el entorno. Azure implementa una instancia privada del servicio Logic Apps en la red virtual de Azure, lo que genera un entorno aislado donde puede crear y ejecutar las aplicaciones lógicas en recursos dedicados. Cuando crea una aplicación lógica, puede seleccionar este entorno como ubicación de la aplicación, que también permite a la aplicación lógica acceder directamente a los recursos de la red virtual.  

Las aplicaciones lógicas de una instancia de ISE proporcionan las mismas experiencias de usuario y funcionalidades similares que el servicio global de Logic Apps. No solo puede usar los mismos elementos integrados y los conectores que proporciona el servicio global de Logic Apps, sino que también puede elegir entre los conectores que ofrecen versiones de ISE. Por ejemplo, a continuación, se muestran algunos conectores estándares que ofrecen versiones que se ejecutan en una instancia de ISE:
 
* Azure Blob Storage, File Storage y Table Storage
* Colas de Azure
* Azure Service Bus
* FTP
* SSH FTP (SFTP)
* SQL Server
* AS2, X12 y EDIFACT

La diferencia entre los conectores de ISE y de otro tipo radica en las ubicaciones donde se ejecutan los desencadenadores y las acciones:

* Si utiliza acciones y desencadenadores integrados, como HTTP en la instancia de ISE, estos desencadenadores y acciones siempre se ejecutan en la misma instancia de ISE que la aplicación lógica. 

* En lo que respecta a los conectores que ofrecen dos versiones: una versión se ejecuta en una instancia de ISE, y la otra lo hace en el servicio global de Logic Apps.  

  Los conectores que tienen la etiqueta **ISE** siempre se ejecutan en la misma instancia de ISE que la aplicación lógica. Los conectores sin la etiqueta **ISE** se ejecutan en el servicio global de Logic Apps. 

  ![Selección de los conectores de ISE](./media/connect-virtual-network-vnet-isolated-environment-overview/select-ise-connectors.png)

* Los conectores que se configuran en la instancia de ISE también están disponibles para usarse en el servicio global de Logic Apps. 

> [!IMPORTANT]
> Las aplicaciones lógicas, las acciones integradas y los conectores que se ejecutan en la instancia de ISE utilizan un plan de tarifa diferente, no uno basado en el consumo. Para obtener más información, consulte [Precios de Logic Apps](../logic-apps/logic-apps-pricing.md).

<a name="vnet-access"></a>

## <a name="permissions-for-virtual-network-access"></a>Permisos de acceso para la red virtual

Cuando crea un entorno del servicio de integración (ISE), puede seleccionar una red virtual de Azure como un *elemento del mismo nivel* para el entorno. Sin embargo, *solo* puede crear esta relación, o *emparejamiento*, al crear la instancia de ISE. Esta relación proporciona a la instancia de ISE acceso a los recursos de la red virtual, lo que permite a las aplicaciones lógicas de esa instancia de ISE conectarse directamente a los recursos de la red virtual. En lo que respecta a los sistemas locales de una red virtual que está vinculada a un ISE, las aplicaciones lógicas pueden acceder directamente a esos sistemas mediante cualquiera de estos elementos: 

* Conector de ISE para ese sistema (por ejemplo, SQL Server)

* Acción HTTP 

* Conector personalizado

Con respecto a los sistemas locales que no están en ninguna red virtual o no tienen conectores de ISE, todavía pueden conectarse después de [configurar y usar la puerta de enlace de datos local](../logic-apps/logic-apps-gateway-install.md).

Antes de poder seleccionar una red virtual de Azure como un elemento del mismo nivel para el entorno, debe configurar los permisos de control de acceso basado en rol (RBAC) en la red virtual de Azure para el servicio Azure Logic Apps. Esta tarea requiere asignar los roles **Colaborador de la red** y **Colaborador clásico** en el servicio Azure Logic Apps. Para obtener más información sobre los permisos de rol requeridos para el emparejamiento, consulte la [sección Permisos de Crear, cambiar o eliminar un emparejamiento de red virtual](../virtual-network/virtual-network-manage-peering.md#permissions).

<a name="create-integration-account-environment"></a>

## <a name="integration-accounts-with-ise"></a>Cuentas de integración con ISE

Puede usar cuentas de integración con aplicaciones lógicas que se ejecutan en un entorno de servicio de integración (ISE), pero esas cuentas de integración también deben usar la *misma instancia de ISE* que las aplicaciones lógicas vinculadas. Las aplicaciones lógicas de una instancia de ISE solo pueden hacer referencia a aquellas cuentas de integración que se encuentran en la misma instancia de ISE. Cuando se crea una cuenta de integración, puede seleccionar la instancia de ISE como la ubicación de la cuenta de integración.

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">foro de Azure Logic Apps</a>.
* Para enviar ideas sobre características o votar sobre ellas, visite el <a href="http://aka.ms/logicapps-wish" target="_blank">sitio de comentarios de los usuarios de Logic Apps</a>.

## <a name="next-steps"></a>Pasos siguientes

* Aprenda a [conectarse a las redes virtuales de Azure desde aplicaciones lógicas aisladas](../logic-apps/connect-virtual-network-vnet-isolated-environment.md).
* Consulte más información sobre [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).
* Obtenga información sobre la [integración de redes virtuales para los servicios de Azure](../virtual-network/virtual-network-for-azure-services.md).
