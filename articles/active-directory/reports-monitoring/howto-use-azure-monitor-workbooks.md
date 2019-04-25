---
title: Utilizar los libros de Azure Monitor para los informes de Azure Active Directory | Microsoft Docs
description: Obtenga información sobre cómo usar los libros de Azure Monitor para los informes de Azure Active Directory
services: active-directory
author: MarkusVi
manager: daveba
ms.assetid: 4066725c-c430-42b8-a75b-fe2360699b82
ms.service: active-directory
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 2c9b3d0ef110fea0629af345a71d0d7b7cce7313
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60287326"
---
# <a name="how-to-use-azure-monitor-workbooks-for-azure-active-directory-reports"></a>Procedimientos para: Utilizar los libros de Azure Monitor para los informes de Azure Active Directory

Si desea:

- ¿Comprender el impacto de su [directivas de acceso condicional](../conditional-access/overview.md) en iniciar sesión sus usuarios?

- ¿Solucionar errores de inicio de sesión para obtener una vista mejor de su estado de inicio de sesión de organización así como solucionar los problemas rápidamente?

- ¿Saber quién está utilizando las autenticaciones heredadas para iniciar sesión su entorno? Por [bloqueo de la autenticación heredada](../conditional-access/block-legacy-authentication.md), puede mejorar la protección de su inquilino.


[Libros de Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks) combinar texto, consultas de análisis, métricas de Azure y los parámetros en informes interactivos enriquecidos. Azure Active Directory proporciona para la supervisión de los libros que le ayudarán a encontrar respuestas a las preguntas anteriores.

Este artículo:

- Se da por supuesto que está familiarizado con cómo [cree informes interactivos con libros de Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks).

- Explica cómo puede usar los libros de Azure Monitor sobre la supervisión de responder a las preguntas anteriores.
 


## <a name="prerequisites"></a>Requisitos previos

Para usar esta característica, necesita:

- Un inquilino de Azure Active Directory, con una licencia premium (P1 y P2). Obtenga información sobre cómo [obtener una licencia premium](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-get-started-premium).

- Un [área de trabajo de Log Analytics](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

## <a name="access-workbooks"></a>Libros de acceso 

Para obtener acceso a los libros:

1. Inicie sesión en su [portal Azure](https://portal.azure.com).

2. En la barra de navegación izquierda, haga clic en **Azure Active Directory**.

3. En el **supervisión** sección, haga clic en **Insights**. 

    ![Información detallada](./media/howto-use-azure-monitor-workbooks/41.png)

4. Haga clic en un informe o una plantilla, o haga clic en **abierto** en la barra de herramientas. 

    ![Gallery](./media/howto-use-azure-monitor-workbooks/42.png)


## <a name="sign-in-analysis"></a>Inicio de sesión de análisis

Para acceder al documento de inicio de sesión de análisis, haga clic en **inicios de sesión** en el **uso** sección. 

Este libro muestra las siguientes tendencias:

- Todos los inicios de sesión

- Correcto

- Acción del usuario pendiente

- Error

Puede filtrar cada tendencia por:

- Intervalo de tiempo

- Aplicaciones

- Usuarios

![Gallery](./media/howto-use-azure-monitor-workbooks/43.png)


Para cada tendencia, obtener un desglose:

- Location

    ![Gallery](./media/howto-use-azure-monitor-workbooks/45.png)

- Dispositivo

    ![Gallery](./media/howto-use-azure-monitor-workbooks/46.png)


## <a name="sign-ins-using-legacy-authentication"></a>Inicios de sesión mediante autenticación heredados 


Para obtener acceso a los inicios de sesión mediante [autenticación heredados](../conditional-access/block-legacy-authentication.md) libro, haga clic en **inicios de sesión mediante la autenticación heredada** en el **uso** sección. 

Este libro muestra las siguientes tendencias:

- Todos los inicios de sesión

- Correcto


Puede filtrar cada tendencia por:

- Intervalo de tiempo

- Aplicaciones

- Usuarios

- Protocolos 

![Gallery](./media/howto-use-azure-monitor-workbooks/47.png)


Para cada tendencia, obtener un desglose por aplicación y protocolo.

![Gallery](./media/howto-use-azure-monitor-workbooks/48.png)



## <a name="sign-ins-by-conditional-access"></a>Inicios de sesión de acceso condicional 


Para obtener acceso a los inicios de sesión por [directivas de acceso condicional](../conditional-access/overview.md) libro, haga clic en **inicios de sesión de acceso condicional** en el **acceso condicional** sección. 

Este libro muestra la tendencia para inicios de sesión deshabilitados.

Puede filtrar cada tendencia por:

- Intervalo de tiempo

- Aplicaciones

- Usuarios

![Gallery](./media/howto-use-azure-monitor-workbooks/49.png)


Para los inicios de sesión deshabilitados, obtener un desglose por el estado de acceso condicional.

![Estado de acceso condicional](./media/howto-use-azure-monitor-workbooks/conditional-access-status.png)








## <a name="sign-ins-by-grant-controls"></a>Inicios de sesión por los controles de concesión

Para obtener acceso a los inicios de sesión por [controles de concesión](../conditional-access/controls.md) libro, haga clic en **inicios de sesión por los controles de concesión** en el **acceso condicional** sección. 

Este libro muestra la siguientes deshabilitadas inicio de sesión de tendencias:

- Requerir MFA
 
- Requerir a los términos de uso

- Requerir la declaración de privacidad

- Otros


Puede filtrar cada tendencia por:

- Intervalo de tiempo

- Aplicaciones

- Usuarios

![Gallery](./media/howto-use-azure-monitor-workbooks/50.png)


Para cada tendencia, obtener un desglose por aplicación y protocolo.

![Gallery](./media/howto-use-azure-monitor-workbooks/51.png)




## <a name="sign-ins-failure-analysis"></a>Análisis de errores de inicios de sesión

Use la **análisis de errores de inicios de sesión** libro para solucionar los errores con:

- Inicios de sesión
- Directivas de acceso condicional
- Autenticación heredados. 


Para obtener acceso a los inicios de sesión por los datos de acceso condicional, haga clic en **inicios de sesión mediante la autenticación heredada** en el **solucionar** sección. 

Este libro muestra las siguientes tendencias:

- Todos los inicios de sesión

- Correcto

- Acción pendiente

- Error


Puede filtrar cada tendencia por:

- Intervalo de tiempo

- Aplicaciones

- Usuarios

![Gallery](./media/howto-use-azure-monitor-workbooks/52.png)


Para solucionar problemas de inicios de sesión, obtener un desglose:

- Errores principales

    ![Gallery](./media/howto-use-azure-monitor-workbooks/53.png)

- Inicios de sesión a la espera en la acción del usuario

    ![Gallery](./media/howto-use-azure-monitor-workbooks/54.png)






## <a name="next-steps"></a>Pasos siguientes

* [Cree informes interactivos con libros de Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks)