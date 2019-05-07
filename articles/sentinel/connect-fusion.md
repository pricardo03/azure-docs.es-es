---
title: Habilitar fusión en versión preliminar de Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo habilitar fusión en Centinela de Azure.
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
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65204468"
---
# <a name="enable-fusion"></a>Habilitar fusión

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Machine Learning en Azure Sentinel está integrada directamente desde el principio. Hemos diseñado cuidadosamente el sistema con las innovaciones de Machine Learning destinadas a productivo de analistas de seguridad, los científicos de datos de seguridad e ingenieros. Una la innovación es que Azure Sentinel Fusion creado especialmente para reducir la fatiga de la alerta.

Fusión utiliza algoritmos de aprendizaje automático de gráfico con la tecnología para correlacionar entre millones de menor actividad anómala de fidelidad de diferentes productos como Azure AD Identity Protection y Microsoft Cloud App Security, combinarlos en un número manejable de casos de seguridad interesantes.

## <a name="enable-fusion"></a>Habilitar fusión

1. En el portal de Azure, seleccione el icono para abrir Cloud Shell.
  ![Cloud Shell](./media/connect-fusion/cloud-shell.png)

2.  En el **Bienvenido a Cloud Shell** windows que se abre a continuación, seleccione PowerShell.

3.  Elija la suscripción en el que ha implementado Azure Sentinel, y **crear almacenamiento**.

4. Una vez se haya autenticado y se ha creado la unidad de Azure, en el símbolo del sistema, ejecute los siguientes comandos:

            az resource update --ids /subscriptions/{Subscription Guid}/resourceGroups/{Log analytics resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Log analytics workspace Name}/providers/Microsoft.SecurityInsights/settings/Fusion --api-version 2019-01-01-preview --set properties.IsEnabled=true --subscription "{Subscription Guid}"

## <a name="disable-fusion"></a>Deshabilitar la fusión

Siga el procedimiento anterior y ejecute el siguiente comando:

            az resource update --ids /subscriptions/{Subscription Guid}/resourceGroups/{Log analytics resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Log analytics workspace Name}/providers/Microsoft.SecurityInsights/settings/Fusion --api-version 2019-01-01-preview --set properties.IsEnabled=false --subscription "{Subscription Guid}"

## <a name="view-the-status-of-fusion"></a>Ver el estado de fusión

            az resource show --ids /subscriptions/{Subscription Guid}/resourceGroups/{Log analytics resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Log analytics workspace Name}/providers/Microsoft.SecurityInsights/settings/Fusion --api-version 2019-01-01-preview --subscription "{Subscription Guid}"


## <a name="next-steps"></a>Pasos siguientes

En este documento, aprendió a habilitar fusión en Centinela de Azure. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Obtenga información sobre cómo [Obtenga visibilidad sobre sus datos y las posibles amenazas](quickstart-get-visibility.md).
- Introducción a [detección de amenazas con Azure Sentinel](tutorial-detect-threats.md).

