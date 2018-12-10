---
title: 'Guía de inicio rápido: Visualización de los roles asignados a un usuario mediante Azure Portal | Microsoft Docs'
description: Obtenga información sobre cómo ver los permisos del control de acceso basado en rol (RBAC) asignados a un usuario, grupo, entidad de servicio o identidad administrada mediante Azure Portal.
services: role-based-access-control
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: quickstart
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 11/30/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: b755dd6223c21084cafea82a1c8857f9f54b03b5
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "52641872"
---
# <a name="quickstart-view-roles-assigned-to-a-user-using-the-azure-portal"></a>Guía de inicio rápido: Visualización de los roles asignados a un usuario mediante Azure Portal

Puede usar la hoja **Control de acceso (IAM)** de [Control de acceso basado en rol (RBAC)](overview.md) para ver las asignaciones de roles para varios usuarios, grupos, entidades de servicio y identidades administradas, pero a veces basta con ver rápidamente las asignaciones de roles para un solo usuario, grupo, entidad de servicio o identidad administrada. La manera más fácil de hacerlo es usar la característica **Comprobar acceso** en Azure Portal.

## <a name="view-role-assignments"></a>Visualización de asignaciones de roles

Siga estos pasos para ver las asignaciones de roles de un solo usuario, grupo, entidad de servicio o identidad administrada en el ámbito de la suscripción.

1. En Azure Portal, haga clic en **Todos los servicios** y luego en **Suscripciones**.

1. Haga clic en la suscripción.

1. Haga clic en **Control de acceso (IAM)**.

1. Haga clic en la pestaña **Comprobar acceso**.

    ![Control de acceso: pestaña Comprobar acceso](./media/check-access/access-control-check-access.png)

1. En la lista **Buscar**, seleccione el tipo de entidad de seguridad cuyo acceso quiere comprobar.

1. En el cuadro de búsqueda, escriba una cadena para buscar nombres para mostrar, direcciones de correo electrónico o identificadores de objeto en el directorio.

    ![Lista de selección de Comprobar acceso](./media/check-access/check-access-select.png)

1. Haga clic en la entidad de seguridad para abrir el panel **Asignaciones**.

    ![Panel de asignaciones](./media/check-access/check-access-assignments.png)

    En este panel puede ver los roles asignados a la entidad de seguridad seleccionada y el ámbito. Si hay asignaciones denegadas en este ámbito o heredadas en este ámbito, se mostrarán.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial: Concesión de acceso a un usuario mediante RBAC y Azure Portal](quickstart-assign-role-user-portal.md)
