---
title: 'Tutorial: Streaming de los registros de Azure Active Directory a Azure Event Hubs (versión preliminar) | Microsoft Docs'
description: Aprenda a configurar Azure Diagnostics para insertar los registros de Azure Active Directory en Azure Event Hubs (versión preliminar)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 045f94b3-6f12-407a-8e9c-ed13ae7b43a3
ms.service: active-directory
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: c4f5bf1b49d7f59b2bb938a7ddc53b96c1d354ef
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2018
ms.locfileid: "39240271"
---
# <a name="tutorial-stream-azure-active-directory-logs-to-an-azure-event-hub-preview"></a>Tutorial: Streaming de los registros de Azure Active Directory a Azure Event Hubs (versión preliminar)

En este tutorial, aprenderá a configurar los valores de diagnóstico de Azure Monitor para enviar por streaming los registros de Azure Active Directory a Azure Event Hubs. Este mecanismo se usa para integrar los registros con herramientas SIEM de terceros como Splunk y QRadar.

## <a name="prerequisites"></a>Requisitos previos 

Necesita:

* Una suscripción de Azure. Si no tiene ninguna suscripción de Azure, puede [registrarse para obtener una evaluación gratuita](https://azure.microsoft.com/free/).
* Un inquilino de Azure Active Directory
* Un usuario, que sea administrador global o administrador de seguridad en dicho inquilino
* Un espacio de nombres de Event Hubs y un centro de eventos en la suscripción de Azure. Aprenda a [crearlo aquí](https://docs.microsoft.com/azure/event-hubs/event-hubs-create.md).

## <a name="archive-logs-to-event-hub"></a>Archivo de registros en un centro de eventos

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). 
2. Haga clic en **Azure Active Directory** -> **Actividad** -> **Registros de auditoría**. 
3. Haga clic en **Exportar configuración** para abrir la hoja Configuración de diagnóstico. Haga clic en **Editar configuración** si desea cambiar la configuración existente o en **Add diagnostic setting** (Agregar configuración de diagnóstico) si lo que desea es agregar una nueva. Puede tener un máximo de tres configuraciones. 
    ![Exportar configuración](./media/reporting-azure-monitor-diagnostics-azure-event-hub/ExportSettings.png "Export settings")

4. Seleccione la casilla **Transmitir a un centro de eventos** y haga clic en **Centro de eventos/Configurar**.
5. Seleccione una suscripción de Azure y el espacio de nombres de Event Hubs al que desea enrutar los registros. Tanto la suscripción como el espacio de nombres de Event Hubs deben estar asociados al inquilino de Active Directory desde el que se envían los registros por streaming. También se puede especificar un centro de eventos en el espacio de nombres de Event Hubs al que se deben enviar los registros. Si no se especifica ningún centro de eventos, se creará uno en el espacio de nombres con el nombre predeterminado **insights-logs-audit**.
6. Haga clic en **Aceptar** para salir de la configuración del centro de eventos.
7. Seleccione la casilla **AuditLogs** para enviar los registros de auditoría a la cuenta de almacenamiento. 
8. Seleccione la casilla **SignInLogs** para enviar los registros de inicio de sesión a la cuenta de almacenamiento.
9. Haga clic en **Guardar** para guardar el valor.
    ![Configuración de diagnóstico](./media/reporting-azure-monitor-diagnostics-azure-event-hub/DiagnosticSettings.png "Diagnostic settings")

10. Quince minutos después, compruebe que aparecen eventos en el centro de eventos. Para ello, vaya al centro de eventos desde el portal y compruebe que el número de **mensajes entrantes** es mayor que cero. 
    ![Registros de auditoría](./media/reporting-azure-monitor-diagnostics-azure-event-hub/InsightsLogsAudit.png "Registros de auditoría")


## <a name="access-data-from-event-hubs"></a>Acceso a datos desde Event Hubs

Una vez que los datos aparecen en el centro de eventos, se puede acceder a ellos de dos formas.

* **Configurar una herramienta SIEM compatible para leer los datos**: la mayoría de las herramientas requieren la cadena de conexión al centro de eventos y determinados permisos para la suscripción de Azure para leer datos desde el centro de eventos. Esta es una lista incompleta de herramientas con la integración de Azure Monitor:
    1. **Splunk**: para más información acerca de cómo integrar los registros de Azure AD con Splunk, consulte [Integración de registros de Azure Active Directory con Splunk mediante Azure Monitor](reporting-azure-monitor-diagnostics-splunk-integration.md).
    
    2. **IBM QRadar**: tanto el módulo específico del dispositivo de Microsoft Azure como el protocolo del centro de eventos de Microsoft Azure se pueden para descargar del [sitio web de soporte técnico de IBM](http://www.ibm.com/support). [Más información sobre la integración con Azure aquí](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/c_dsm_guide_microsoft_azure_overview.html?cp=SS42VS_7.3.0).
    
    3. **SumoLogic**: siga [estas instrucciones](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure-Audit/02Collect-Logs-for-Azure-Audit-from-Event-Hub) para configurar SumoLogic para que consuma datos de un centro de eventos. 

* **Configurar herramientas personalizadas para leer los datos**: si el SIEM actual aún no se admite en Azure Monitor, las herramientas personalizadas se pueden configurar mediante las API de Event Hubs. Para más información, consulte las [API de Event Hubs](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph).


## <a name="next-steps"></a>Pasos siguientes

* [Integración de los registros de Azure Active Directory con Splunk mediante Azure Monitor Diagnostics](reporting-azure-monitor-diagnostics-splunk-integration.md)
* [Interpretación de un esquema de registros de auditoría en los diagnósticos de Azure Monitor](reporting-azure-monitor-diagnostics-audit-log-schema.md)
* [Interpretación de un esquema de registros de inicio de sesión en los diagnósticos de Azure Monitor](reporting-azure-monitor-diagnostics-sign-in-log-schema.md)