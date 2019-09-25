---
title: Uso de los libros de Azure Monitor en informes de Azure Active Directory | Microsoft Docs
description: Aprenda a usar los libros de Azure Monitor en informes de Azure Active Directory.
services: active-directory
author: cawrites
manager: daveba
ms.assetid: 4066725c-c430-42b8-a75b-fe2360699b82
ms.service: active-directory
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: chadam
ms.reviewer: dhanyahk
ms.openlocfilehash: 288fa54a1a6dd4eb05f953a4490bf7736d6d7ff8
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2019
ms.locfileid: "70931247"
---
# <a name="how-to-use-azure-monitor-workbooks-for-azure-active-directory-reports"></a>Uso de los libros de Azure Monitor en informes de Azure Active Directory

Quiere:

- Comprender el efecto de las [directivas de acceso condicional](../conditional-access/overview.md) en la experiencia de inicio de sesión de los usuarios.

- Solucionar problemas de inicio de sesión para obtener una mejor visión del estado de inicio de sesión de la organización y resolver problemas rápidamente.

- Saber quién está utilizando las autenticaciones heredadas para iniciar sesión en su entorno. (Al [bloquear la autenticación heredada](../conditional-access/block-legacy-authentication.md), puede mejorar la protección del inquilino).

Para ayudarle a resolver estas cuestiones, Active Directory proporciona los libros para la supervisión. Los [libros de Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks) combinan texto, consultas de análisis, métricas de Azure y parámetros en informes interactivos avanzados. 

Este artículo:

- Supone que está familiarizado con cómo [crear informes interactivos con libros de Monitor](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks).

- Explica cómo utilizar los libros de Monitor para comprender el efecto de las directivas de acceso condicional, para solucionar los problemas de inicio de sesión e identificar las autenticaciones heredadas.
 


## <a name="prerequisites"></a>Requisitos previos

Para utilizar los libros de Monitor, necesita:

- Un inquilino de Active Directory con una licencia premium (P1 o P2). Aprenda cómo [obtener una licencia prémium](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-get-started-premium).

- Un [área de trabajo de Log Analytics.](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace)

## <a name="roles"></a>Roles
Debe estar en uno de los roles siguientes y tener [acceso al área de trabajo subyacente de Log Analytics](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/manage-access#manage-access-using-azure-permissions) para administrar los libros:
-   Administrador global
-   Administrador de seguridad
-   Lector de seguridad
-   Lector de informes
-   Administrador de aplicaciones


## <a name="workbook-access"></a>Acceso a los libros 

Para acceder a los libros:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. En el menú de navegación de la izquierda, seleccione **Azure Active Directory**.

3. En la sección **Supervisión**, seleccione **Workbooks**. 

    ![Seleccionar Insights](./media/howto-use-azure-monitor-workbooks/41.png)

4. Seleccione un informe o una plantilla o, en la barra de herramientas, seleccione **Abrir**. 

    ![Seleccionar Abrir](./media/howto-use-azure-monitor-workbooks/42.png)


## <a name="sign-in-analysis"></a>Análisis de inicio de sesión

Para tener acceso al libro de análisis de inicio de sesión, en la sección **Uso**, seleccione **Inicios de sesión**. 

Este libro muestra las siguientes tendencias de inicio de sesión:

- Todos los inicios de sesión

- Correcto

- Acción de usuario pendiente

- Error

Puede filtrar cada tendencia por las siguientes categorías:

- Intervalo de tiempo

- Aplicaciones

- Usuarios

![Análisis de inicio de sesión](./media/howto-use-azure-monitor-workbooks/43.png)


Para cada tendencia, puede obtener un desglose por las siguientes categorías:

- Location

    ![Inicios de sesión por ubicación](./media/howto-use-azure-monitor-workbooks/45.png)

- Dispositivo

    ![Inicios de sesión por dispositivo](./media/howto-use-azure-monitor-workbooks/46.png)


## <a name="sign-ins-using-legacy-authentication"></a>Inicios de sesión que utilizan una autenticación heredada 


Para acceder al libro para los inicios de sesión que utilizan la [autenticación heredada](../conditional-access/block-legacy-authentication.md), en la sección **Usage** (Uso), seleccione **Sign-ins using Legacy Authentication** (Inicios de sesión que utilizan una autenticación heredada). 

Este libro muestra las siguientes tendencias de inicio de sesión:

- Todos los inicios de sesión

- Correcto


Puede filtrar cada tendencia por las siguientes categorías:

- Intervalo de tiempo

- Aplicaciones

- Usuarios

- Protocolos

![Inicios de sesión por autenticación heredada](./media/howto-use-azure-monitor-workbooks/47.png)


Para cada tendencia, va a obtener un desglose por aplicación y protocolo.

![Inicios de sesión de autenticación heredada por aplicación y protocolo](./media/howto-use-azure-monitor-workbooks/48.png)



## <a name="sign-ins-by-conditional-access"></a>Inicios de sesión por acceso condicional 


Para tener acceso al libro para los inicios de sesión por [directivas de acceso condicional](../conditional-access/overview.md), en la sección **Conditional Access** (Acceso condicional), seleccione **Sign-ins by Conditional Access** (Inicios de sesión por acceso condicional). 

Este libro muestra las tendencias para los inicios de sesión deshabilitados. Puede filtrar cada tendencia por las siguientes categorías:

- Intervalo de tiempo

- Aplicaciones

- Usuarios

![Inicios de sesión con acceso condicional](./media/howto-use-azure-monitor-workbooks/49.png)


Para los inicios de sesión deshabilitados, obtendrá un desglose por el estado de acceso condicional.

![Estado de acceso condicional](./media/howto-use-azure-monitor-workbooks/conditional-access-status.png)








## <a name="sign-ins-by-grant-controls"></a>Inicios de sesión por controles de concesión

Para tener acceso al libro para los inicios de sesión por [controles de concesión](../conditional-access/controls.md), en la sección **Conditional Access** (Acceso condicional), seleccione **Sign-ins by Grant Controls** (Inicios de sesión por controles de concesión). 

Este libro muestra las siguientes tendencias de inicios de sesión deshabilitados:

- Requerir MFA
 
- Requerir condiciones de uso

- Requerir la declaración de privacidad

- Otros


Puede filtrar cada tendencia por las siguientes categorías:

- Intervalo de tiempo

- Aplicaciones

- Usuarios

![Inicios de sesión por controles de concesión](./media/howto-use-azure-monitor-workbooks/50.png)


Para cada tendencia, va a obtener un desglose por aplicación y protocolo.

![Desglose de los inicios de sesión recientes](./media/howto-use-azure-monitor-workbooks/51.png)




## <a name="sign-ins-failure-analysis"></a>Análisis de errores de inicio de sesión

Use el libro **Análisis de errores de inicios de sesión** para solucionar los errores de lo siguiente:

- Inicios de sesión
- Directivas de acceso condicional
- Autenticación heredada 


Para acceder a los inicios de sesión por datos de acceso condicional, en la sección **Solución de problemas**, seleccione **Sign-ins using Legacy Authentication** (Inicios de sesión que utilizan una autenticación heredada). 

Este libro muestra las siguientes tendencias de inicio de sesión:

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

- Principales errores

    ![Resumen de los errores principales](./media/howto-use-azure-monitor-workbooks/53.png)

- Inicios de sesión a la espera de una acción del usuario

    ![Resumen de los inicios de sesión a la espera de una acción del usuario](./media/howto-use-azure-monitor-workbooks/54.png)






## <a name="next-steps"></a>Pasos siguientes

[Creación de informes interactivos con los libros de Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks).
