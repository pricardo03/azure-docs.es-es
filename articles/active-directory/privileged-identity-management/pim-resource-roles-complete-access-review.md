---
title: Completar una revisión de acceso para recursos de Azure mediante Privileged Identity Management | Microsoft Docs
description: Aquí se describe cómo completar una revisión de acceso para recursos de Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: f1fc23afb41753bfe358cb05376a5ee9a07e34fd
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2018
ms.locfileid: "39620049"
---
# <a name="complete-an-access-review-for-azure-resources-by-using-privileged-identity-management"></a>Completar una revisión de acceso para recursos de Azure mediante Privileged Identity Management
Los administradores de roles con privilegios pueden revisar el acceso con privilegios una vez se [ha iniciado una revisión del acceso](pim-resource-roles-start-access-review.md). Privileged Identity Management (PIM) para recursos de Azure envía automáticamente un correo electrónico para pedir a los usuarios que revisen su acceso. Si un usuario no recibe un correo electrónico, puede enviarle las instrucciones necesarias para [realizar una revisión del acceso](pim-resource-roles-perform-access-review.md).

Cuando acabe el período de la revisión del acceso o una vez que todos los usuarios hayan finalizado su autorrevisión, siga los pasos de este artículo para administrar la revisión y ver los resultados.

## <a name="manage-access-reviews"></a>Administración de revisiones del acceso
1. Vaya a [Azure Portal](https://portal.azure.com/). A continuación, en el panel, seleccione la aplicación **Recursos de Azure**.

2. Seleccione el recurso.

3. Seleccione la sección **Revisiones de acceso** del panel.
![Revisiones de acceso](media/azure-pim-resource-rbac/rbac-access-review-home-list.png)

4. Seleccione la revisión de acceso que quiere administrar.

En la hoja de detalles de la revisión de acceso, hay varias opciones para administrar dicha revisión. Las opciones son las siguientes:

![Opciones para administrar una revisión](media/azure-pim-resource-rbac/rbac-access-review-menu.png)

### <a name="stop"></a>Stop
Todas las revisiones de acceso tienen una fecha de finalización, pero puede usar el botón **Detener** para terminarlas antes. Todos los usuarios que no hayan finalizado la revisión para entonces no podrán finalizarla después de detener la revisión. No se puede reiniciar una revisión después de detenerla.

### <a name="reset"></a>Reset
Puede restablecer una revisión de acceso para quitar todas las decisiones que se tomaron en ella. Una vez que haya restablecido una revisión de acceso, todos los usuarios se marcan de nuevo como no revisados. 

### <a name="apply"></a>Aplicar
Una vez completada una revisión de acceso, use el botón **Aplicar** para implementar el resultado de la revisión. Si se denegó el acceso de un usuario en la revisión, este paso quitará su asignación de rol.  

### <a name="delete"></a>Eliminar
Si ya no le interesa más la revisión, elimínela. El botón **Eliminar** quita la revisión de la aplicación PIM.

## <a name="results"></a>Results
Consulte y descargue una lista de los resultados de la revisión en la pestaña **Resultados**. 
![Pestaña de resultados](media/azure-pim-resource-rbac/rbac-access-review-results.png)

## <a name="reviewers"></a>Revisores
Vea y agregue revisores a la revisión de acceso existente. Recuerde a los revisores que realicen sus revisiones.
![Agregar revisores](media/azure-pim-resource-rbac/rbac-access-review-reviewers.png)



