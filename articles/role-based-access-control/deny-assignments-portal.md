---
title: Visualización de las asignaciones de denegación mediante Azure Portal | Microsoft Docs
description: Aprenda cómo ver a usuarios, grupos, entidades de servicio e identidades administradas a los que se les ha negado el acceso a acciones específicas en un ámbito determinado mediante Azure Portal.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/30/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: f5870ddbbb8be0ebbeae7656485521a327b86d5b
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "52637730"
---
# <a name="view-deny-assignments-using-the-azure-portal"></a>Visualización de las asignaciones de denegación mediante Azure Portal

[Denegar asignaciones](deny-assignments.md) impide que los usuarios realicen acciones especificadas, incluso si una asignación de roles les concede acceso. Aunque no pueda crear sus propias asignaciones de denegación, debe poder ver las asignaciones de denegación porque pueden afectar a los permisos generales. Para obtener información sobre una asignación de denegación, debe tener el permiso `Microsoft.Authorization/denyAssignments/read`, que se incluye en la mayoría de los [roles integrados](built-in-roles.md).

En este artículo se describe cómo usar Azure Portal para ver las asignaciones de denegación.

> [!NOTE]
> En este momento, las asignaciones de denegación son de solo lectura y únicamente pueden establecerse mediante Azure.

## <a name="view-deny-assignments"></a>Visualización de asignaciones de denegación

Siga estos pasos para ver las asignaciones de denegación en el ámbito de la suscripción o del grupo de administración.

1. En Azure Portal, haga clic en **Todos los servicios** y, después, en **Grupos de administración** o **Suscripciones**.

1. Haga clic en el grupo de administración o suscripción que desee ver.

1. Haga clic en **Control de acceso (IAM)**.

1. Haga clic en la pestaña **Asignaciones de denegación** (o haga clic en el botón **Ver** en el icono Ver asignaciones de denegación).

    Si hay alguna asignación denegada en este ámbito o heredada de este ámbito, se enumerarán.

    ![Control de acceso: pestaña Asignaciones de denegación](./media/deny-assignments-portal/access-control-deny-assignments.png)

1. Para mostrar las columnas adicionales, haga clic en **Editar columnas**.

    ![Asignaciones de denegación: columnas](./media/deny-assignments-portal/deny-assignments-columns.png)

    |  |  |
    | --- | --- |
    | **Nombre** | Nombre de la asignación de denegación. |
    | **Tipo de entidad de seguridad** | Usuario, grupo, grupo definido por el sistema o entidad de servicio. |
    | **Se deniega**  | Nombre de la entidad de seguridad que se incluye en la asignación de denegación. |
    | **Id** | Identificador único para la asignación de denegación. |
    | **Entidades de seguridad excluidas** | Si hay entidades de seguridad que se excluyen de la asignación de denegación. |
    | **No se aplica a los elementos secundarios** | Si la asignación de denegación se hereda en los ámbitos secundarios. |
    | **Protegida por el sistema** | Si Azure administra la asignación de denegación. Actualmente, siempre es sí. |
    | **Ámbito** | Grupo de administración, suscripción, grupo de recursos o identificador de recurso. |

1. Agregue una marca de verificación a cualquiera de los elementos habilitados y, a continuación, haga clic en **Aceptar** para mostrar las columnas seleccionadas.

## <a name="view-details-about-a-deny-assignment"></a>Visualización de detalles sobre una asignación de denegación

Siga estos pasos para ver detalles adicionales sobre una asignación de denegación.

1. Abra el panel **Asignaciones de denegación**, tal como se describe en la sección anterior.

1. Haga clic en el nombre de asignación de denegación para abrir la hoja **Usuarios**.

    ![Asignación de denegación: Usuarios](./media/deny-assignments-portal/deny-assignment-users.png)

    La hoja **Usuarios** incluye las dos secciones siguientes.

    |  |  |
    | --- | --- |
    | **La asignación de denegación se aplica a**  | Entidades de seguridad a las que se aplica la asignación de denegación. |
    | **La asignación de denegación excluye** | Entidades de seguridad que se excluyen de la asignación de denegación. |

    **La entidad de seguridad definida por el sistema** representa a todos los usuarios, grupos, entidades de servicio e identidades administradas de un directorio de Azure AD.

1. Para ver una lista de los permisos que se deniegan, haga clic en **Permisos denegados**.

    ![Asignación de denegación: Permisos denegados](./media/deny-assignments-portal/deny-assignment-denied-permissions.png)

    | Tipo de acción | DESCRIPCIÓN |
    | --- | --- |
    | **Acciones**  | Operaciones de administración denegadas. |
    | **NotActions** | Operaciones de administración que se excluyen de la operación de administración denegada. |
    | **DataActions**  | Operaciones de datos denegadas. |
    | **NotDataActions** | Operaciones de datos que se excluyen de la operación de datos denegada. |

    Para el ejemplo mostrado en la captura de pantalla anterior, los siguientes son los permisos efectivos:

    - Se deniegan todas las operaciones de almacenamiento en el plano de datos, excepto las operaciones de proceso.

1. Para ver las propiedades de una asignación de denegación, haga clic en **Propiedades**.

    ![Asignación de denegación: Propiedades](./media/deny-assignments-portal/deny-assignment-properties.png)

    En la hoja **Propiedades**, puede ver el nombre, el identificador, la descripción y el ámbito de la asignación de denegación. El modificador **No se aplica a los elementos secundarios** indica si la asignación de denegación se hereda a los ámbitos secundarios. El modificador **Protegida por el sistema** indica si Azure administra esta asignación de denegación. Actualmente, es **Sí** en todos los casos.

## <a name="next-steps"></a>Pasos siguientes

* [Descripción de las asignaciones de denegación](deny-assignments.md)
* [List deny assignments using RBAC and the REST API](deny-assignments-rest.md) (Lista de asignaciones de denegación mediante RBAC y la API REST)
