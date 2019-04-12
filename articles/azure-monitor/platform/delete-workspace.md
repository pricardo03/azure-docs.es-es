---
title: Eliminación de un área de trabajo de Azure Log Analytics | Microsoft Docs
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
ms.openlocfilehash: a6542838acba3143123dc90d96746179a2b4469b
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2019
ms.locfileid: "55989175"
---
# <a name="delete-an-azure-log-analytics-workspace-with-the-azure-portal"></a>Eliminación de una área de trabajo de Azure Log Analytics con Azure Portal
En este tema se muestra cómo usar Azure Portal para eliminar un área de trabajo de Log Analytics que ya no necesite. 

## <a name="to-delete-a-workspace"></a>Para eliminar un área de trabajo 
Cuando elimina un área de trabajo de Log Analytics, todos los datos relacionados con el área de trabajo se eliminan del servicio en un plazo máximo de 30 días.  Debe tener cuidado al eliminar un área de trabajo porque puede haber datos importantes y una configuración que puede afectar negativamente a las operaciones del servicio. Tenga en cuenta los demás servicios y orígenes de Azure que almacenan sus datos en Log Analytics, como:

* Application Insights
* Azure Security Center
* Azure Automation
* Agentes que se ejecutan en máquinas virtuales Windows y Linux
* Agentes que se ejecutan en equipos Windows y Linux en su entorno
* System Center Operations Manager
* Soluciones de administración 

Los agentes y grupos de administración de System Center Operations Manager configurados para notificar al área de trabajo que continúe en un estado huérfano.  Inventario con el que se integran los agentes, las soluciones y otros servicios de Azure antes de continuar.   
 
Si es administrador y hay varios usuarios asociados al área de trabajo, se interrumpirá la asociación entre los usuarios y el área de trabajo. Si los usuarios están asociados a otras áreas de trabajo, podrán seguir utilizando Log Analytics con esas otras áreas de trabajo. Sin embargo, si no están asociados a otras áreas de trabajo, deberán crear un área de trabajo para poder usar Log Analytics. 

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com). 
2. En Azure Portal, haga clic en **Más servicios**, en la esquina inferior izquierda. En la lista de recursos, escriba **Log Analytics**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Seleccione **Áreas de trabajo de Log Analytics**.
3. En el panel de suscripciones de Log Analytics, seleccione un área de trabajo y, a continuación, haga clic en **Eliminar** desde la parte superior del panel intermedio.<br><br> ![Opción Eliminar desde el panel de propiedades del área de trabajo](media/delete-workspace/log-analytics-delete-workspace.png)<br>  
4. Cuando aparezca de la ventana de mensaje de confirmación que le solicite que confirme la eliminación del área de trabajo, haga clic en **Sí**.<br><br> ![Confirmación de la eliminación del área de trabajo](media/delete-workspace/log-analytics-delete-workspace-confirm.png)

