---
title: Integración de los registros de Azure Active Directory con SumoLogic mediante Azure Monitor (versión preliminar) | Microsoft Docs
description: Obtenga información sobre cómo integrar los registros de Azure Active Directory con SumoLogic mediante Azure Monitor (versión preliminar)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 2c3db9a8-50fa-475a-97d8-f31082af6593
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 2a90e70a9e7951b298be408992ee5e5fc6332131
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2018
ms.locfileid: "45729338"
---
# <a name="integrate-azure-ad-logs-with-sumologic-by-using-azure-monitor-preview"></a>Integración de registros de Azure AD con SumoLogic mediante Azure Monitor (versión preliminar)

En este artículo, se ofrece información sobre cómo integrar los registros de Azure Active Directory (Azure AD) con SumoLogic mediante Azure Monitor. En primer lugar, se enrutan los registros a un centro de eventos de Azure y, a continuación, se integra el centro de eventos con SumoLogic.

## <a name="prerequisites"></a>Requisitos previos

Para usar esta característica, necesita:
* Un centro de eventos de Azure que contenga registros de actividad de Azure AD. Aprenda cómo [transmitir los registros de actividad a un centro de eventos](quickstart-azure-monitor-stream-logs-to-event-hub.md). 
* Una suscripción habilitada para el inicio de sesión único en SumoLogic.

## <a name="steps-to-integrate-azure-ad-logs-with-sumologic"></a>Pasos para integrar los registros de Azure AD con SumoLogic 

1. Primero, haga un [streaming de los registros de Azure AD a Azure Event Hubs (versión preliminar)](quickstart-azure-monitor-stream-logs-to-event-hub.md).
2. Configure la instancia de SumoLogic para [recopilar registros de Azure Active Directory](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Collect_Logs_for_Azure_Active_Directory).
3. [Instale la aplicación SumoLogic de Azure AD](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Install_the_Azure_Active_Directory_App_and_View_the_Dashboards) para usar los paneles preconfigurados que proporcionan análisis en tiempo real de su entorno.

 ![panel](./media/howto-integrate-activity-logs-with-sumologic/overview-dashboard.png)

## <a name="next-steps"></a>Pasos siguientes

* [Interpretación del esquema de registros de auditoría en Azure Monitor](reference-azure-monitor-audit-log-schema.md)
* [Interpretación del esquema de registros de inicio de sesión en Azure Monitor](reference-azure-monitor-sign-ins-log-schema.md)
* [Preguntas frecuentes y problemas conocidos](concept-activity-logs-in-azure-monitor.md#frequently-asked-questions)
