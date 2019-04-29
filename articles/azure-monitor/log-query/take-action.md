---
title: Acción de runbook de Azure Automation iniciada por el usuario en Log Analytics | Microsoft Docs
description: En este artículo se describe cómo ejecutar un runbook de Automation desde un resultado de búsqueda de Log Analytics a petición.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/06/2019
ms.author: magoedte
ms.openlocfilehash: 9194d5fe6553607ac5a0bb4e133da97f53790984
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61424761"
---
# <a name="take-action-with-an-automation-runbook-from-a-log-analytics-log-search-result"></a>Realizar acciones con un runbook de Automation desde un resultado de búsqueda de registros de Log Analytics

> [!NOTE]
> Iniciar un runbook desde los resultados de búsqueda es una característica del portal clásico de búsqueda de registros que dejará de usarse el 15 de febrero de 2019. Puede configurar un grupo de acciones que pueden iniciar un runbook, además de otras acciones desde una [regla de alerta](../platform/alerts-log.md) de Azure Monitor.

Desde un resultado de búsqueda de registros de Log Analytics de Azure, ahora puede seleccionar **Take action** (Realizar acción) para ejecutar un runbook de Automation.  El runbook se puede usar para corregir un problema o realizar otra acción, como recopilar información para solucionar problemas, enviar un correo electrónico o crear una solicitud de servicio. 


## <a name="components-and-features-used"></a>Componentes y características usados
* [Cuenta de Azure Automation](../../automation/automation-quickstart-create-account.md)
* [Área de trabajo de Log Analytics](../../azure-monitor/log-query/log-query-overview.md)

## <a name="to-initiate-runbook-from-log-search"></a>Para iniciar el runbook desde una búsqueda de registros

Para realizar acciones en un evento e iniciar un runbook desde los resultados de búsqueda de registros, primero debe crear una búsqueda de registros y, desde los resultados, puede invocar un runbook a petición. Esto se puede conseguir con la característica de búsqueda de registros clasica de [Azure Portal](../../azure-monitor/log-query/log-query-overview.md). En este ejemplo, llevamos a cabo una búsqueda de registros desde Azure Portal con una demostración básica de esta característica.

1. En Azure Portal, en el menú central, haga clic en **Todos los servicios** y seleccione **Log Analytics**.  
2. Seleccione el área de trabajo de Log Analytics.
3. En el área de trabajo, seleccione **Registros (clásico)**.  
4. En la hoja Búsqueda de registros, realice una búsqueda de registros.  
5. Desde los resultados de la búsqueda de registros, haga clic en los puntos suspensivos situados a la izquierda de uno de los campos y, en la ventana emergente, seleccione **Take action on** (Realizar acción en).<br><br> ![Seleccionar Realizar acción a partir del resultado de búsqueda](./media/take-action/log-search-takeaction-menuoption.png) 
6. Seleccione **Ejecutar un runbook** y seleccione un runbook para ejecutar.  Puede seleccionar cualquier runbook de la cuenta de Automation que esté vinculado al área de trabajo de Log Analytics.  Tenga en cuenta lo siguiente:

    * Los runbooks se organizan por etiquetas.
    * Puede seleccionar los valores de los parámetros de entrada del runbook directamente desde los campos del resultado de la búsqueda.  Aparecerá una lista desplegable en la que se muestran todos los campos disponibles en el resultado para que realice una selección.  
    * Puede elegir ejecutar el runbook en un [Hybrid Runbook Worker](../../automation/automation-hybrid-runbook-worker.md) instalado en el equipo que tenga el problema si dispone de un grupo Hybrid Runbook Worker correspondiente que solo contenga este equipo como miembro.  Si el nombre del grupo Hybrid Worker coincide con el nombre del equipo que se encuentra en el resultado de la búsqueda de registros, dicho grupo se seleccionará automáticamente.    

6. Tras hacer clic en **Ejecutar**, se abrirá la hoja del trabajo de runbook para que pueda revisar el estado del trabajo.   

Si selecciona un runbook que estaba configurado para que [se llame desde una alerta de Log Analytics](../../automation/automation-create-alert-triggered-runbook.md), tendrá un parámetro de entrada denominado **WebhookData** de tipo **Object**.  Si el parámetro de entrada es obligatorio, debe pasar los resultados de la búsqueda al runbook para que pueda convertir la cadena con formato JSON en un tipo de objeto, lo que le permitirá filtrar los elementos específicos a los que hará referencia en las actividades de runbook.  Para ello, seleccione **Search result (Object)** (Resultado de búsqueda (objeto)) en la lista desplegable.<br><br> ![Seleccionar el objeto de datos de webhook para el parámetro de runbook](media/take-action/select-runbook-and-properties.png)   
    
## <a name="next-steps"></a>Pasos siguientes

* Revise la [Referencia sobre búsqueda de registros de Log Analytics](../../azure-monitor/log-query/log-query-overview.md) para ver todos los campos de búsqueda y las facetas disponibles en Log Analytics.
* Para obtener información sobre cómo invocar un runbook de Automation de manera automática, vea [Llamada a un runbook de Azure Automation desde una alerta de Log Analytics](../../automation/automation-create-alert-triggered-runbook.md).  
