---
title: Eliminación y restauración de un área de trabajo de Azure Log Analytics | Microsoft Docs
description: Obtenga información para eliminar el área de trabajo de Log Analytics si creó uno en una suscripción personal o reestructure el modelo de área de trabajo.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: magoedte
ms.openlocfilehash: f8dcab1a7a46d518b752e48f9886b60a37d8ec4c
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2019
ms.locfileid: "71299537"
---
# <a name="delete-and-restore-azure-log-analytics-workspace"></a>Eliminación y restauración de un área de trabajo de Azure Log Analytics
En este artículo se explica el concepto de eliminación temporal de áreas de trabajo de Azure Log Analytics y cómo recuperarlas. 

## <a name="considerations-when-deleting-a-workspace"></a>Consideraciones al eliminar áreas de trabajo
Al eliminar un área de trabajo de Log Analytics, se realiza una operación de eliminación temporal para que se pueda recuperar el área de trabajo, incluidos los datos y los agentes conectados en un plazo de 14 días, tanto si la eliminación fue accidental como intencionada. Después del período de eliminación temporal, el área de trabajo y sus datos no podrán recuperarse y se pondrán en cola para su eliminación permanente en un plazo de 30 días.

Debe tener cuidado al eliminar un área de trabajo porque puede haber datos importantes y una configuración que puede afectar negativamente a las operaciones de servicio. Revise qué agentes, soluciones y otros servicios y orígenes de Azure almacenan sus datos en Log Analytics. Por ejemplo:
* Soluciones de administración
* Azure Automation
* Agentes que se ejecutan en máquinas virtuales Windows y Linux
* Agentes que se ejecutan en equipos Windows y Linux en su entorno
* System Center Operations Manager

La operación de eliminación temporal quita el recurso del área de trabajo y los permisos de los usuarios asociados. Si los usuarios están asociados a otras áreas de trabajo, podrán seguir utilizando Log Analytics con esas otras áreas de trabajo.

## <a name="soft-delete-behavior"></a>Comportamiento de eliminación temporal
La operación de eliminación del área de trabajo quita el recurso de Resource Manager del área de trabajo, pero su configuración y sus datos se conservan durante 14 días (y da la sensación de que el área de trabajo se ha eliminado). Los agentes y grupos de administración de System Center Operations Manager configurados para enviar notificaciones al área de trabajo continúan en un estado huérfano durante el período de eliminación temporal. El servicio proporciona además un mecanismo para recuperar el área de trabajo eliminada (incluidos sus datos y recursos conectados), básicamente, deshaciendo la eliminación.

> [!NOTE] 
> Las soluciones instaladas y los servicios vinculados, como la cuenta de Automation, se quitan permanentemente del área de trabajo en el momento de la eliminación y no se pueden recuperar. Deben volver a configurarse después de la operación de recuperación para devolver al área de trabajo su funcionalidad anterior. 

Puede eliminar un área de trabajo con [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/remove-azurermoperationalinsightsworkspace?view=azurermps-6.13.0), [API](https://docs.microsoft.com/rest/api/loganalytics/workspaces/delete) o en [Azure Portal](https://portal.azure.com).

### <a name="delete-workspace-in-azure-portal"></a>Eliminación de áreas de trabajo en Azure Portal
1. Para iniciar sesión, vaya a [Azure Portal](https://portal.azure.com). 
2. En Azure Portal, seleccione **Todos los servicios**. En la lista de recursos, escriba **Log Analytics**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Seleccione **Áreas de trabajo de Log Analytics**.
3. En la lista de áreas de trabajo de Log Analytics, seleccione un área de trabajo y, a continuación, haga clic en **Eliminar** en la parte superior del panel intermedio.
   ![Opción Eliminar desde el panel de propiedades del área de trabajo](media/delete-workspace/log-analytics-delete-workspace.png)
4. Cuando aparezca de la ventana de mensaje de confirmación que le solicite que confirme la eliminación del área de trabajo, haga clic en **Sí**.
   ![Confirmación de la eliminación del área de trabajo](media/delete-workspace/log-analytics-delete-workspace-confirm.png)

## <a name="recover-workspace"></a>Recuperación de áreas de trabajo
Si tiene permisos de Colaborador en la suscripción y el grupo de recursos al que se asoció el área de trabajo antes de la operación de eliminación temporal, puede recuperarlos durante dicho período de eliminación temporal, incluidos los datos, la configuración y los agentes conectados. Después del período de eliminación temporal, el área de trabajo no podrá recuperarse y se pondrá en cola para su eliminación permanente.

Para recuperar un área de trabajo, vuelva a crearla con cualquiera de los métodos de creación admitidos: PowerShell, la CLI de Azure o desde Azure Portal, siempre que estas propiedades se rellenen con los detalles del área de trabajo eliminada. Por ejemplo:
1.  Id. de suscripción
2.  Nombre del grupo de recursos
3.  Nombre del área de trabajo
4.  Region

> [!NOTE]
> Los nombres de las áreas de trabajo eliminadas se mantienen en el período de eliminación temporal y no se pueden usar al crear un área de trabajo. Los nombres de las áreas de trabajo se *liberan* y pueden usarse para crear áreas de trabajo después de que haya expirado el período de eliminación temporal.
