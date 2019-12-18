---
title: Visualización de la actividad del proveedor de servicios
description: Los clientes pueden visualizar la actividad registrada con el fin de ver las acciones realizadas por los proveedores de servicios mediante la administración de recursos delegados de Azure.
ms.date: 12/6/2019
ms.topic: conceptual
ms.openlocfilehash: 69517e942aa9f82be16fa3d0e7d6f9252de44d4c
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2019
ms.locfileid: "74932249"
---
# <a name="view-service-provider-activity"></a>Visualización de la actividad del proveedor de servicios

Los clientes que tengan suscripciones delegadas para la administración de recursos delegados de Azure pueden [visualizar los dados del registro de actividad de Azure](https://docs.microsoft.com/azure/azure-monitor/platform/activity-logs-overview) para ver todas las acciones realizadas. Esto proporciona a los clientes una visibilidad completa de las operaciones que los proveedores de servicios realizan mediante la administración de recursos delegados de Azure, junto con las realizadas por los usuarios en el propio inquilino del cliente de Azure Active Directory (Azure AD).

## <a name="view-activity-log-data"></a>Visualización de los datos del registro de actividad

Puede [visualizar el registro de actividad](https://docs.microsoft.com/azure/azure-monitor/platform/activity-logs-overview#view-the-activity-log) en el menú **Supervisión** de Azure Portal. Para limitar los resultados a una suscripción específica, puede usar los filtros y seleccionar la suscripción que desee. También puede [visualizar y recuperar eventos del registro de actividad](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view) mediante programación.

> [!NOTE]
> Los usuarios de un inquilino de proveedor de servicios pueden visualizar los resultados del registro de actividad de una suscripción delegada en un inquilino de cliente, si al incorporar la suscripción a la administración de recursos delegados de Azure se les concedió el rol de [Lector](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#reader) (u otro rol integrado que incluya acceso de lectura).

En el registro de actividad, verá el nombre de la operación y su estado, junto con la fecha y la hora en la que se realizó. La columna **Evento iniciado por** muestra el usuario que ha realizado la operación, tanto si se trata de un usuario del inquilino de un proveedor de servicios que actúa mediante la administración de recursos delegados de Azure, o un usuario en el propio inquilino del cliente. Tenga en cuenta que se muestra el nombre del usuario, no el del inquilino o el rol que se ha asignado al usuario para esa suscripción.

La actividad registrada está disponible en Azure Portal para los últimos 90 días. Si desea información sobre cómo almacenar estos datos durante más de 90 días, consulte [Recopilación y análisis de los registros de actividad de Azure en un área de trabajo de Log Analytics en Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-collect)

## <a name="set-alerts-for-critical-operations"></a>Establecimiento de alertas para las operaciones críticas

Para mantenerse informado sobre las operaciones críticas que realizan los proveedores de servicios (o los usuarios de su propio inquilino), se recomienda crear [alertas de registro de actividad](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-alerts). Por ejemplo, puede que desee realizar el seguimiento de todas las acciones administrativas de una suscripción, o recibir una notificación cuando se elimine una máquina virtual de un grupo de recursos determinado. Las alertas que cree incluirán las acciones realizadas por los usuarios en el propio inquilino del cliente, así como en cualquier inquilino de administración.

Para más información consulte la sección [Creación y administración de alertas del registro de actividades](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log).

## <a name="create-log-queries"></a>Creación de consultas de registro

Puede crear consultas para analizar la actividad registrada o centrarse en elementos concretos. Por ejemplo, es posible que una auditoría le pida que informe sobre todas las acciones de nivel administrativo realizadas en una suscripción. Puede crear una consulta para filtrar solo estas acciones y ordenar los resultados por usuario, fecha u otro valor.

Para más información consulte [Introducción a las consultas de registro en Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview).

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/).
- Obtenga información sobre la [Visualización y administración de proveedores de servicios](view-manage-service-providers.md) en Azure Portal.