---
title: Integración de los registros de Azure Active Directory con Log Analytics mediante Azure Monitor (versión preliminar) | Microsoft Docs
description: Obtenga información sobre cómo integrar los registros de Azure Active Directory con Log Analytics mediante Azure Monitor (versión preliminar).
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: 2c3db9a8-50fa-475a-97d8-f31082af6593
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 51db96523a96015822f4507731bad2a398521530
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56165848"
---
# <a name="integrate-azure-ad-logs-with-log-analytics-using-azure-monitor-preview"></a>Integración de registros de Azure AD con Log Analytics mediante Azure Monitor (versión preliminar)

Log Analytics le permite consultar los datos para buscar eventos concretos, analizar tendencias y realizar la correlación entre varios orígenes de datos. Con la integración de los registros de actividad de Azure AD en Log Analytics, ahora puede realizar tareas como:

 * Comparar los registros de inicio de sesión de Azure AD con los registros de seguridad publicados por Azure Security Center.

 * Solucionar problemas de cuellos de botella de rendimiento en la página de inicio de sesión de la aplicación mediante la correlación de datos de rendimiento de la aplicación de Azure Application Insights.  

El vídeo siguiente de una sesión de Ignite muestra las ventajas del uso de Log Analytics para los registros de Azure AD en escenarios de usuario prácticos.

> [!VIDEO https://www.youtube.com/embed/MP5IaCTwkQg?start=1894]

En este artículo, se ofrece información sobre cómo integrar los registros de Azure Active Directory (Azure AD) con Log Analytics mediante Azure Monitor.

## <a name="supported-reports"></a>Informes admitidos

Puede enrutar los registros de actividad de auditoría y los registros de actividad de inicio de sesión a Log Analytics para su posterior análisis. 

* **Registros de auditoría**: el [informe de actividad de registros de auditoría](concept-audit-logs.md) le proporciona acceso al historial de todas las tareas llevadas a cabo en el inquilino.
* **Registros de inicio de sesión**: Con el [informe de actividad de inicios de sesión](concept-sign-ins.md), puede determinar quién ha realizado las tareas notificadas en el informe de registros de auditoría.

> [!NOTE]
> En este momento no se admiten los registros de actividad de auditoría e inicio de sesión relacionados con B2C.
>

## <a name="prerequisites"></a>Requisitos previos 

Para usar esta característica, necesita:

* Una suscripción de Azure. Si no tiene ninguna suscripción de Azure, puede [registrarse para obtener una evaluación gratuita](https://azure.microsoft.com/free/).
* Un inquilino de Azure AD.
* Un usuario que sea *administrador global* o *administrador de seguridad* para el inquilino de Azure AD.
* Un área de trabajo de Log Analytics en la suscripción a Azure. Aprenda a [crear un área de trabajo de Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-create-workspace).

## <a name="send-logs-to-log-analytics"></a>Envío de registros a Log Analytics

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). 

2. Seleccione **Azure Active Directory** > **Configuración de diagnóstico** -> **Agregar configuración de diagnóstico**. También puede seleccionar **Exportar configuraciones** desde la página **Registros de auditoría** o **Inicios de sesión** para ir a la página de configuración de diagnóstico.  
    
3. En el menú **Configuración de diagnóstico**, seleccione la casilla **Enviar a Log Analytics** y, a continuación, seleccione **Configurar**.

4. Seleccione el área de trabajo de Log Analytics a la que quiere enviar los registros o cree una nueva área de trabajo en el cuadro de diálogo proporcionado.  

5. Realice alguna de las siguientes acciones o ambas:
    * Para enviar los registros de auditoría al área de trabajo de Log Analytics, seleccione la casilla **AuditLogs**. 
    * Para enviar los registros de inicio de sesión al área de trabajo de Log Analytics, seleccione la casilla **SignInLogs**.

6. Seleccione **Guardar** para guardar la configuración.

    ![Configuración de diagnóstico](./media/howto-integrate-activity-logs-with-log-analytics/Configure.png)

7. Después de unos 15 minutos, compruebe que los eventos se transmiten al área de trabajo de Log Analytics.

## <a name="next-steps"></a>Pasos siguientes

* [Analyze Azure AD activity logs in Log Analytics](howto-analyze-activity-logs-log-analytics.md) (Análisis de registros de actividad de Azure AD en Log Analytics)
* [Install and use the Log Analytics views for Azure Active Directory](howto-install-use-log-analytics-views.md) (Instalación y uso de las vistas de Log Analytics para Azure Active Directory)
