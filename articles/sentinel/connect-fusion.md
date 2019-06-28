---
title: Habilitar la fusión en Azure Sentinel, versión preliminar | Microsoft Docs
description: Aprenda a habilitar la fusión en Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 82becf50-6628-47e4-b3d7-18d7d72d505f
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: bf9a78006d13614a73a3fccfc57f28ce850456d9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65204468"
---
# <a name="enable-fusion"></a>Habilitar fusión

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Las capacidades de Machine Learning en Azure Sentinel están integradas directamente desde el principio. Hemos diseñado cuidadosamente el sistema con innovaciones de Machine Learning con el propósito de que los analistas de seguridad, los científicos de datos de seguridad y los ingenieros sean más productivos. Una de estas innovaciones es la fusión de Azure Sentinel, creada concretamente para reducir el exceso de alertas.

La fusión utiliza algoritmos de Machine Learning basados en gráficos para correlacionar millones actividades anómalas de baja fidelidad de diferentes productos (como Azure AD Identity Protection y Microsoft Cloud App Security) a fin de combinarlas en un número asumible de casos de seguridad de interés.

## <a name="enable-fusion"></a>Habilitar fusión

1. En Azure Portal, seleccione el icono para abrir Cloud Shell.
  ![Cloud Shell](./media/connect-fusion/cloud-shell.png)

2.  En la ventana **Welcome to Cloud Shell** (Bienvenido a Cloud Shell) que se abre, seleccione PowerShell.

3.  Elija la suscripción en la que haya implementado Azure Sentinel y seleccione **Create storage** (Crear almacenamiento).

4. Una vez que se haya autenticado y se haya creado la unidad de Azure, ejecute los siguientes comandos en el símbolo del sistema:

            az resource update --ids /subscriptions/{Subscription Guid}/resourceGroups/{Log analytics resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Log analytics workspace Name}/providers/Microsoft.SecurityInsights/settings/Fusion --api-version 2019-01-01-preview --set properties.IsEnabled=true --subscription "{Subscription Guid}"

## <a name="disable-fusion"></a>Deshabilitar la fusión

Realice el mismo procedimiento anterior y ejecute el siguiente comando:

            az resource update --ids /subscriptions/{Subscription Guid}/resourceGroups/{Log analytics resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Log analytics workspace Name}/providers/Microsoft.SecurityInsights/settings/Fusion --api-version 2019-01-01-preview --set properties.IsEnabled=false --subscription "{Subscription Guid}"

## <a name="view-the-status-of-fusion"></a>Ver el estado de la fusión

            az resource show --ids /subscriptions/{Subscription Guid}/resourceGroups/{Log analytics resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Log analytics workspace Name}/providers/Microsoft.SecurityInsights/settings/Fusion --api-version 2019-01-01-preview --subscription "{Subscription Guid}"


## <a name="next-steps"></a>Pasos siguientes

En este documento, ha aprendido a habilitar la fusión en Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats.md).

