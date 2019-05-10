---
title: Utilizar los libros de Azure Monitor para los informes de Azure Active Directory | Microsoft Docs
description: Obtenga información sobre cómo utilizar los libros de Azure Monitor para los informes de Azure Active Directory.
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
ms.openlocfilehash: 6ae14ec152975717af5d55780bcc39aa87c4b01a
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65406590"
---
# <a name="how-to-use-azure-monitor-workbooks-for-azure-active-directory-reports"></a>Cómo usar los libros de Azure Monitor para los informes de Azure Active Directory

Si desea:

- ¿Entender el efecto de su [directivas de acceso condicional](../conditional-access/overview.md) en la experiencia de inicio de sesión de usuarios?

- ¿Solucionar errores de inicio de sesión para obtener una mejor visión del estado de inicio de sesión de su organización y resuelva problemas rápidamente?

- ¿Saber quién está utilizando las autenticaciones heredadas para iniciar sesión en su entorno? (Por [bloqueo de la autenticación heredada](../conditional-access/block-legacy-authentication.md), puede mejorar la protección de su inquilino.)

Para ayudarle a resolver estas cuestiones, Active Directory proporciona los libros para la supervisión. [Libros de Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks) combinar texto, las consultas de análisis, métricas y los parámetros en informes interactivos enriquecidos. 

Este artículo:

- Se supone que está familiarizado con cómo [crear informes interactivos con libros Monitor](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks).

- Explica cómo utilizar los libros de Monitor para comprender el efecto de las directivas de acceso condicional, para solucionar problemas de errores de inicio de sesión y para identificar las autenticaciones heredadas.
 


## <a name="prerequisites"></a>Requisitos previos

Para utilizar los libros de Monitor, necesita:

- Un inquilino de Active Directory con una licencia de premium (P1 o P2). Obtenga información sobre cómo [obtener una licencia premium](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-get-started-premium).

- Un [área de trabajo de Log Analytics](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

## <a name="workbook-access"></a>Acceso a los libros 

Para obtener acceso a los libros:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. En el panel de navegación izquierdo, seleccione **Azure Active Directory**.

3. En el **supervisión** sección, seleccione **Insights**. 

    ![Seleccione Insights](./media/howto-use-azure-monitor-workbooks/41.png)

4. Seleccione un informe o una plantilla o en la barra de herramientas **abierto**. 

    ![Seleccione Abrir](./media/howto-use-azure-monitor-workbooks/42.png)


## <a name="sign-in-analysis"></a>Inicio de sesión de análisis

Para tener acceso al libro de inicio de sesión de análisis, en el **uso** sección, seleccione **inicios de sesión**. 

Este libro muestra las siguientes tendencias:

- Todos los inicios de sesión

- Correcto

- Acción del usuario pendiente

- Error

Puede filtrar cada tendencia por las siguientes categorías:

- Intervalo de tiempo

- Aplicaciones

- Usuarios

![Inicio de sesión de análisis](./media/howto-use-azure-monitor-workbooks/43.png)


Para cada tendencia, obtener un desglose por las siguientes categorías:

- Ubicación

    ![Inicios de sesión por ubicación](./media/howto-use-azure-monitor-workbooks/45.png)

- Dispositivo

    ![Inicios de sesión por dispositivo](./media/howto-use-azure-monitor-workbooks/46.png)


## <a name="sign-ins-using-legacy-authentication"></a>Inicios de sesión mediante autenticación heredados 


Para tener acceso al libro para inicios de sesión que usan [autenticación heredados](../conditional-access/block-legacy-authentication.md), en el **uso** sección, seleccione **inicios de sesión mediante la autenticación heredada**. 

Este libro muestra las siguientes tendencias:

- Todos los inicios de sesión

- Correcto


Puede filtrar cada tendencia por las siguientes categorías:

- Intervalo de tiempo

- Aplicaciones

- Usuarios

- Protocolos

![Inicios de sesión mediante autenticación heredados](./media/howto-use-azure-monitor-workbooks/47.png)


Para cada tendencia, obtener un desglose por aplicación y protocolo.

![Inicios de sesión de autenticación heredado por aplicación y protocolo](./media/howto-use-azure-monitor-workbooks/48.png)



## <a name="sign-ins-by-conditional-access"></a>Inicios de sesión de acceso condicional 


Para tener acceso al libro para inicios de sesión por [directivas de acceso condicional](../conditional-access/overview.md), en el **acceso condicional** sección, seleccione **inicios de sesión de acceso condicional**. 

Este libro muestra las tendencias para inicios de sesión deshabilitados. Puede filtrar cada tendencia por las siguientes categorías:

- Intervalo de tiempo

- Aplicaciones

- Usuarios

![Inicios de sesión con acceso condicional](./media/howto-use-azure-monitor-workbooks/49.png)


Para inicios de sesión deshabilitados, obtener un desglose por el estado de acceso condicional.

![Estado de acceso condicional](./media/howto-use-azure-monitor-workbooks/conditional-access-status.png)








## <a name="sign-ins-by-grant-controls"></a>Inicios de sesión por los controles de concesión

Para tener acceso al libro para inicios de sesión por [controles de concesión](../conditional-access/controls.md), en el **acceso condicional** sección, seleccione **inicios de sesión por los controles de concesión**. 

Este libro muestra la siguientes deshabilitadas inicio de sesión de tendencias:

- Requerir MFA
 
- Requerir a los términos de uso

- Requerir la declaración de privacidad

- Otros


Puede filtrar cada tendencia por las siguientes categorías:

- Intervalo de tiempo

- Aplicaciones

- Usuarios

![Inicios de sesión por los controles de concesión](./media/howto-use-azure-monitor-workbooks/50.png)


Para cada tendencia, obtener un desglose por aplicación y protocolo.

![Desglose de los inicios de sesión recientes](./media/howto-use-azure-monitor-workbooks/51.png)




## <a name="sign-ins-failure-analysis"></a>Análisis de errores de inicios de sesión

Use la **análisis de errores de inicios de sesión** libro para solucionar los errores con los siguientes:

- Inicios de sesión
- Directivas de acceso condicional
- Autenticación heredada 


Para obtener acceso a los inicios de sesión por los datos de acceso condicional, en el **solucionar** sección, seleccione **inicios de sesión mediante la autenticación heredada**. 

Este libro muestra las siguientes tendencias:

- Todos los inicios de sesión

- Correcto

- Acción pendiente

- Error


Puede filtrar cada tendencia por las siguientes categorías:

- Intervalo de tiempo

- Aplicaciones

- Usuarios

![Solución de problemas de inicios de sesión](./media/howto-use-azure-monitor-workbooks/52.png)


Para ayudarle a solucionar problemas de inicios de sesión, Azure Monitor proporciona un desglose por las siguientes categorías:

- Errores principales

    ![Resumen de errores principales](./media/howto-use-azure-monitor-workbooks/53.png)

- Inicios de sesión a la espera en la acción del usuario

    ![Resumen de inicios de sesión a la espera en la acción del usuario](./media/howto-use-azure-monitor-workbooks/54.png)






## <a name="next-steps"></a>Pasos siguientes

[Cree informes interactivos mediante el uso de los libros de Monitor](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks).