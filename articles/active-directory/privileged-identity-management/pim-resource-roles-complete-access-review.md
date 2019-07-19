---
title: 'Realización de una revisión de acceso de roles de recursos de Azure en PIM: Azure Active Directory | Microsoft Docs'
description: Aprenda a completar una revisión de acceso para los roles de recursos de Azure en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9903bb82a82291febf571829fb9874ba66d2eab2
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476362"
---
# <a name="complete-an-access-review-of-azure-resource-roles-in-pim"></a>Realización de una revisión de acceso de los roles de recursos de Azure en PIM
Los administradores de roles con privilegios pueden revisar el acceso con privilegios una vez se [ha iniciado una revisión del acceso](pim-resource-roles-start-access-review.md). Azure Active Directory (Azure AD) Privileged Identity Management (PIM) envía automáticamente un correo electrónico para pedir a los usuarios que revisen su acceso. Si un usuario no recibe un correo electrónico, puede enviarle las instrucciones necesarias para [realizar una revisión del acceso](pim-resource-roles-perform-access-review.md).

Cuando acabe el período de la revisión del acceso o una vez que todos los usuarios hayan finalizado su autorrevisión, siga los pasos de este artículo para administrar la revisión y ver los resultados.

## <a name="manage-access-reviews"></a>Administración de revisiones del acceso
1. Vaya a [Azure Portal](https://portal.azure.com/). A continuación, en el panel, seleccione la aplicación **Recursos de Azure**.

2. Seleccione el recurso.

3. Seleccione la sección **Revisiones de acceso** del panel.

    ![Recursos de Azure: lista de revisiones de acceso que muestra el rol, el propietario, la fecha de inicio, la fecha de finalización y el estado](media/pim-resource-roles-complete-access-review/rbac-access-review-home-list.png)

4. Seleccione la revisión de acceso que quiere administrar.

En la hoja de detalles de la revisión de acceso, hay varias opciones para administrar dicha revisión. Las opciones son las siguientes:

![Opciones para administrar una revisión: Detener, Restablecer, Aplicar, Eliminar](media/pim-resource-roles-complete-access-review/rbac-access-review-menu.png)

### <a name="stop"></a>Stop
Todas las revisiones de acceso tienen una fecha de finalización, pero puede usar el botón **Detener** para terminarlas antes. Todos los usuarios que no hayan finalizado la revisión para entonces no podrán finalizarla después de detener la revisión. No se puede reiniciar una revisión después de detenerla.

### <a name="reset"></a>Reset
Puede restablecer una revisión de acceso para quitar todas las decisiones que se tomaron en ella. Una vez que haya restablecido una revisión de acceso, todos los usuarios se marcan de nuevo como no revisados. 

### <a name="apply"></a>Aplicar
Una vez completada una revisión de acceso, use el botón **Aplicar** para implementar el resultado de la revisión. Si se denegó el acceso de un usuario en la revisión, este paso quitará su asignación de rol.  

### <a name="delete"></a>Eliminar
Si ya no le interesa más la revisión, elimínela. El botón **Eliminar** quita la revisión de la aplicación PIM.

## <a name="results"></a>Results
En la página **Resultados**, consulte y descargue una lista de los resultados de la revisión. 

![Página de resultados que muestra los usuarios, el resultado, quién realizó la revisión, quién la aplicó y la aplicación del resultado](media/pim-resource-roles-complete-access-review/rbac-access-review-results.png)

## <a name="reviewers"></a>Revisores
Vea y agregue revisores a la revisión de acceso existente. Recuerde a los revisores que realicen sus revisiones.

![Página de revisores que muestra el nombre y el nombre principal de usuario](media/pim-resource-roles-complete-access-review/rbac-access-review-reviewers.png)

## <a name="next-steps"></a>Pasos siguientes

- [Inicio de una revisión de acceso para los roles de recurso de Azure en PIM](pim-resource-roles-start-access-review.md)
- [Realización de una revisión de acceso para los roles de recurso de Azure en PIM](pim-resource-roles-perform-access-review.md)
