---
title: Administración de entornos del servicio de integración en Azure Logic Apps
description: Compruebe el estado de la red y administre las aplicaciones lógicas, las conexiones, los conectores personalizados y las cuentas de integración en el entorno del servicio de integración (ISE) para Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 07/26/2019
ms.openlocfilehash: 8f10e3d3fd7c67d1e803e8f85c9918c91bb81d59
ms.sourcegitcommit: f5cc71cbb9969c681a991aa4a39f1120571a6c2e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68517361"
---
# <a name="manage-your-integration-service-environment-ise-in-azure-logic-apps"></a>Administración del entorno del servicio de integración (ISE) en Azure Logic Apps

Para comprobar el estado de la red en su [entorno del servicio de integración (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) y administrar las aplicaciones lógicas, las conexiones, las cuentas de integración y los conectores personalizados que existan en el ISE, siga los pasos de este tema.

## <a name="view-your-ise"></a>Ver su ISE

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

1. En el cuadro de búsqueda del portal, escriba "entornos de servicio de integración" y, después, seleccione **Entornos de servicio de integración**.

   ![Búsqueda de los entornos de servicio de integración](./media/ise-manage-integration-service-environment/find-integration-service-environment.png)

1. En la lista de resultados, seleccione su entorno del servicio de integración.

   ![Selección del entorno de servicio de integración](./media/ise-manage-integration-service-environment/select-integration-service-environment.png)

1. Continúe con las secciones siguientes para buscar aplicaciones lógicas, conexiones, conectores o cuentas de integración en el ISE.

<a name="check-network-health"></a>

## <a name="check-network-health"></a>Comprobación del estado de la red

En el menú de ISE, en **Configuración**, seleccione **Estado de la red**. Este panel muestra el estado de mantenimiento de las subredes y las dependencias de salida en otros servicios.

![Comprobación del estado de la red](./media/ise-manage-integration-service-environment/ise-check-network-health.png)

<a name="find-logic-apps"></a>

## <a name="manage-your-logic-apps"></a>Administración de las aplicaciones lógicas

1. En el menú de ISE, en **Configuración**, seleccione **Aplicaciones lógicas**.

   ![Búsqueda de aplicaciones lógicas](./media/ise-manage-integration-service-environment/ise-find-logic-apps.png)

1. Para quitar aplicaciones lógicas del ISE cuando ya no las necesite, selecciónelas y, a continuación, seleccione **Eliminar**.

<a name="find-api-connections"></a>

## <a name="manage-api-connections"></a>Administración de las conexiones de API

1. Para ver las conexiones de API creadas por las aplicaciones lógicas que se ejecutan en el ISE, en el menú ISE, debajo de **Configuración**, seleccione **Conexiones de API**.

   ![Búsqueda de las conexiones de API](./media/ise-manage-integration-service-environment/ise-find-api-connections.png)

1. Para quitar las conexiones del ISE cuando ya no las necesite, seleccione esas conexiones y, a continuación, seleccione **Eliminar**.

<a name="find-custom-connectors"></a>

## <a name="manage-custom-connectors"></a>Administración de conectores personalizados

1. Para ver los conectores personalizados que se crearon en el ISE, en el menú de ISE, en **Configuración**, seleccione **Conectores personalizados**.

   ![Buscar conectores personalizados](./media/ise-manage-integration-service-environment/ise-find-custom-connectors.png)

1. Para quitar los conectores personalizados cuando ya no los necesite, seleccione esos conectores y, a continuación, seleccione **Eliminar**.

<a name="find-integration-accounts"></a>

## <a name="manage-integration-accounts"></a>Administración de cuentas de integración

1. En el menú de ISE, en **Configuración**, seleccione **Cuentas de integración**.

   ![Buscar cuentas de integración](./media/ise-manage-integration-service-environment/ise-find-integration-accounts.png)

1. Para quitar las cuentas de integración del ISE cuando ya no las necesite, selecciónelas y, después, seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

* Aprenda a [conectarse a las redes virtuales de Azure desde aplicaciones lógicas aisladas](../logic-apps/connect-virtual-network-vnet-isolated-environment.md).
