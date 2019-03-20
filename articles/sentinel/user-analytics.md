---
title: Investigar los usuarios con análisis de usuarios en la versión preliminar de Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo investigar a los usuarios con análisis de usuarios en Azure Sentinel.
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 5b06aef0-16be-4ca0-83e4-a33795a82353
ms.service: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: ee871f90838b32be3c950ab23a0f01568c309269
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2019
ms.locfileid: "57246328"
---
# <a name="investigate-users-with-user-analytics"></a>Investigar los usuarios con análisis de usuarios

> [!IMPORTANT]
> Sentinel Azure está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Los usuarios son entidades que desea supervisar de cerca. Para ayudarle a obtener información sobre los usuarios, Centinela de Azure se integra perfectamente con protección contra amenazas avanzada de Azure. Esta integración le permite analizar el comportamiento de usuario y dar prioridad a los usuarios que debe investigar en primer lugar, en función de sus alertas y los patrones de actividad sospechosa en Centinela de Azure y Microsoft 365.

Azure centinela enriquece los datos de usuario con los análisis de Microsoft 365 para habilitar el análisis de análisis y el riesgo de usuario completa para todos los usuarios en Azure Active Directory. 

## <a name="how-it-works"></a>Cómo funciona

1. En función de la la seguridad de las reglas de análisis, cuando una coincidencia se detecta, Azure Sentinel envía las alertas de ATP de Azure.
1. ATP de Azure comprueba que las entidades de usuario están relacionados con las alertas y calcula la prioridad de investigación para esos usuarios.
3. ATP de Azure, a continuación, vuelve a calcular la puntuación de los usuarios después de que se ha enriquecido con los datos de las reglas de análisis de Azure Sentinel.


## <a name="prerequisites"></a>Requisitos previos

- Una licencia de protección contra amenazas avanzada de Azure 
- Para habilitar la característica, necesita permisos de administrador global en el inquilino en la que instaló Centinela de Azure

> [!NOTE]
> - Para cada inquilino de ATP de Azure, puede conectar una instancia de Azure Sentinel.
> - Análisis del usuario actualmente solo está disponible para los usuarios de Azure Active Directory. 

## <a name="enable-user-analytics"></a>Habilitar el análisis de usuario

1. Para habilitar el análisis de usuario en Azure Sentinel, en el portal, vaya a la **analytics usuario** página y haga clic en **habilitar**. Esto envía información sobre el área de trabajo de ATP de Azure.

1. A continuación, le lleva a ATP de Azure. En **extensiones de seguridad** en el **Centinela de Microsoft Azure** pestaña, haga clic en el **+ plus** para agregar y, a continuación, seleccione el área de trabajo. 
1. Haga clic en **Conectar**.

## <a name="investigate-a-user"></a>Investigar un usuario

1. En el menú de Azure Sentinel en **analytics usuario**, revise la lista de los usuarios según su prioridad de investigación. La puntuación se basa en Azure Sentinel alertas y Microsoft 365.

2. Buscar un usuario que desea investigar. Haga clic en el usuario para acceder a la página de usuario. Revise el usuario las actividades y alertas, con el tiempo, en la escala de tiempo. Puede ver todas las actividades de Microsoft 365 y Azure Sentinel. También puede comunicarse con la página de usuario mediante la obtención de detalles de los usuarios desde dentro de un caso.
      
    ![Usuarios](./media/user-analytics/user-investigation.png)

 
3. Para que funcione bien, se debe configurar correctamente la [regla de alerta personalizada](tutorial-detect-threats.md) para asignar los identificadores de usuario correcto para el **cuenta** entidad.

    - Para los eventos de Windows, se asignan **cuenta** a la **SID**
    - Los datos de Azure AD (que pueden encontrarse en muchos registros de actividad de Azure) o datos de Office 365, asignar el **cuenta** propiedad a la **GUID**, o el **nombre Principal de usuario**. 

   ![Consultar](./media/user-analytics/query-window.png)



Por ejemplo:  
> [!NOTE]
> En los registros de actividad de la actividad de Azure, la entidad de autor de llamada incluye el UPN.

1. Esta consulta busca la creación de un número anómalo de recursos o las actividades de implementación en el registro de actividad de Azure.

        AzureActivity      
        | where TimeGenerated >= startofday(ago(7d))        
        | where OperationName == "Create or Update Virtual Machine" or OperationName == "Create Deployment"        
        | where ActivityStatus == "Succeeded"        
        | make-series dResourceCount=dcount(ResourceId)  default=0 on EventSubmissionTimestamp in range(startofday(ago(7d)), now(), 1d) by Caller        
        | extend (RSquare,Slope,Variance,RVariance,Interception,LineFit)=series_fit_line(dResourceCount)        
        | where Slope > 0.2        
        | join kind=leftsemi (        
        // Last day's activity is anomalous        
        AzureActivity        
        | where TimeGenerated >= startofday(ago(1d))        
        | where OperationName == "Create or Update Virtual Machine" or OperationName == "Create Deployment"        
        | where ActivityStatus == "Succeeded"        
        | make-series dResourceCount=dcount(ResourceId)  default=0 on EventSubmissionTimestamp in range(startofday(ago(1d)), now(), 1d) by Caller        
        | extend (RSquare,Slope,Variance,RVariance,Interception,LineFit)=series_fit_line(dResourceCount)        
        | where Slope >0.2        
        ) on Caller        
        | extend AccountCustomEntity = Caller
 
2. En la regla de alerta personalizada, asignar el **cuenta** propiedad **llamador**.
   
   ![Consultar](./media/user-analytics/query-window.png)

3. Investigue el usuario en la ventana de investigación de usuario. Para obtener consejos acerca de cómo investigar a los usuarios, consulte [Tutorial: Investigar un usuario](https://docs.microsoft.com/azure-advanced-threat-protection/investigate-a-user).



## <a name="next-steps"></a>Pasos siguientes

En este documento, ha aprendido cómo conectar el proveedor de inteligencia de amenazas a Centinela de Azure. Para obtener más información acerca de Centinela de Azure, consulte los siguientes artículos.

- Para empezar a trabajar con Azure Sentinel, necesita una suscripción a Microsoft Azure. Si no tiene una suscripción, puede registrarse para obtener una [evaluación gratuita](https://azure.microsoft.com/free/).
- Obtenga información sobre cómo [incorporar los datos a Azure Sentinel](quickstart-onboard.md), y [Obtenga visibilidad sobre sus datos y las posibles amenazas](quickstart-get-visibility.md).
