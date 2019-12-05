---
title: Administración de entornos del servicio de integración en Azure Logic Apps
description: Compruebe el estado de la red y administre las aplicaciones lógicas, las conexiones, los conectores personalizados y las cuentas de integración en el entorno del servicio de integración (ISE) para Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: 1d91813e0f39207bcf7768de89600a6bdee0fc53
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74792630"
---
# <a name="manage-your-integration-service-environment-ise-in-azure-logic-apps"></a>Administración del entorno del servicio de integración (ISE) en Azure Logic Apps

Para comprobar el estado de la red en su [entorno del servicio de integración (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) y administrar las aplicaciones lógicas, las conexiones, las cuentas de integración y los conectores que existan en el ISE, siga los pasos de este tema. Para agregar estos artefactos al ISE, consulte [Adición de artefactos al entorno del servicio de integración](../logic-apps/add-artifacts-integration-service-environment-ise.md).

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

Puede ver y administrar las aplicaciones lógicas que se encuentran en el ISE.

1. En el menú de ISE, en **Configuración**, seleccione **Aplicaciones lógicas**.

   ![Ver aplicaciones lógicas](./media/ise-manage-integration-service-environment/ise-find-logic-apps.png)

1. Para eliminar aplicaciones lógicas del ISE cuando ya no las necesite, selecciónelas y, a continuación, seleccione **Eliminar**. Seleccione **Sí** para confirmar que desea eliminarlas.

<a name="find-api-connections"></a>

## <a name="manage-api-connections"></a>Administración de las conexiones de API

Puede ver y administrar las conexiones creadas por las aplicaciones lógicas que se ejecutan en el ISE.

1. En el menú de ISE, en **Configuración**, seleccione **Conexiones de API**.

   ![Ver conexiones de API](./media/ise-manage-integration-service-environment/ise-find-api-connections.png)

1. Para eliminar las conexiones del ISE cuando ya no las necesite, seleccione esas conexiones y, a continuación, seleccione **Eliminar**. Seleccione **Sí** para confirmar que desea eliminarlas.

<a name="manage-api-connectors"></a>

## <a name="manage-ise-connectors"></a>Administración de los conectores de ISE

Puede ver y administrar los conectores de API que se han implementado en el ISE.

1. En el menú de ISE, en **Configuración**, seleccione **Conectores administrados**.

   ![Ver conectores administrados](./media/ise-manage-integration-service-environment/ise-view-managed-connectors.png)

1. Para eliminar los conectores del ISE cuando ya no los necesite, seleccione esos conectores y, a continuación, seleccione **Eliminar**. Seleccione **Sí** para confirmar que desea eliminarlas.

<a name="find-custom-connectors"></a>

## <a name="manage-custom-connectors"></a>Administración de conectores personalizados

Puede ver y administrar los conectores personalizados que implementó en el ISE.

1. En el menú de ISE, en **Configuración**, seleccione **Conectores personalizados**.

   ![Buscar conectores personalizados](./media/ise-manage-integration-service-environment/ise-find-custom-connectors.png)

1. Para eliminar los conectores personalizados del ISE cuando ya no los necesite, seleccione esos conectores y, a continuación, seleccione **Eliminar**. Seleccione **Sí** para confirmar que desea eliminarlas.

<a name="find-integration-accounts"></a>

## <a name="manage-integration-accounts"></a>Administración de cuentas de integración

1. En el menú de ISE, en **Configuración**, seleccione **Cuentas de integración**.

   ![Buscar cuentas de integración](./media/ise-manage-integration-service-environment/ise-find-integration-accounts.png)

1. Para quitar las cuentas de integración del ISE cuando ya no las necesite, selecciónelas y, después, seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

* Aprenda a [conectarse a las redes virtuales de Azure desde aplicaciones lógicas aisladas](../logic-apps/connect-virtual-network-vnet-isolated-environment.md).
