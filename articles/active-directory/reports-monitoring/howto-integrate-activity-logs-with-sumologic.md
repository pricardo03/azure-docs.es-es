---
title: Transmisión de los registros de Azure Active Directory a SumoLogic mediante Azure Monitor (versión preliminar) | Microsoft Docs
description: Aprenda a integrar los registros de Azure Active Directory con SumoLogic mediante Azure Monitor (versión preliminar).
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
ms.openlocfilehash: 256b5d925bd924cbfdd08e9fb6600ea848fd2eb3
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55195637"
---
# <a name="integrate-azure-active-directory-logs-with-sumologic-using-azure-monitor-preview"></a>Integración de los registros de Azure Active Directory con SumoLogic mediante Azure Monitor (versión preliminar)

En este artículo, aprenderá a integrar los registros de Azure Active Directory (Azure AD) con SumoLogic mediante Azure Monitor. En primer lugar, se enrutan los registros a un centro de eventos de Azure y, a continuación, se integra el centro de eventos con SumoLogic.

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
* [Preguntas frecuentes y problemas conocidos](concept-activity-logs-azure-monitor.md#frequently-asked-questions)
