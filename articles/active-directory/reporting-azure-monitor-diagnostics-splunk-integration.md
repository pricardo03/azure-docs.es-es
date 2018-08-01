---
title: Integración de los registros de Azure Active Directory con Splunk mediante Azure Monitor (versión preliminar) | Microsoft Docs
description: Obtenga información sobre cómo integrar los registros de Azure Active Directory con Splunk mediante Azure Monitor (versión preliminar).
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: c4b605b6-6fc0-40dc-bd49-101d03f34665
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: d1dd62d06c7e3ed634795604ce9660694ea073ca
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2018
ms.locfileid: "39240571"
---
# <a name="integrate-azure-active-directory-logs-with-splunk-using-azure-monitor-preview"></a>Integración de los registros de Azure Active Directory con Splunk mediante Azure Monitor (versión preliminar)

En este artículo, se ofrece información sobre cómo integrar los registros de Azure Active Directory con Splunk mediante Azure Monitor. En primer lugar, debe redirigir los registros a un centro de eventos de Azure y después integrarlo con Splunk.

## <a name="prerequisites"></a>Requisitos previos

1. Un centro de eventos de Azure que contenga registros de actividad de Azure AD. Aprenda a [transmitir los registros de actividad al centro de eventos](reporting-azure-monitor-diagnostics-azure-event-hub.md). 
2. Use las siguientes [instrucciones](https://github.com/Microsoft/AzureMonitorAddonForSplunk/blob/master/README.md) para descargar el complemento de Azure Monitor para Splunk y configurar la instancia de Splunk.

## <a name="tutorial"></a>Tutorial 

1. Abra la instancia de Splunk y haga clic en **Data Summary** (Resumen de datos).
    ![Resumen de datos](./media/reporting-azure-monitor-diagnostics-splunk-integration/DataSummary.png "Resumen de datos")

2. Navegue hasta la pestaña **Sourcetypes** (Tipos de orígenes) y seleccione **amal: aadal:audit** ![Pestaña Sourcetypes](./media/reporting-azure-monitor-diagnostics-splunk-integration/sourcetypeaadal.png "Pestaña Sourcetypes")

3. Podrá ver los registros de actividad de Azure AD como se muestra en la figura siguiente.
    ![Registros de actividad](./media/reporting-azure-monitor-diagnostics-splunk-integration/activitylogs.png "Registros de actividad")

> [!NOTE]
> Si no puede instalar un complemento en la instancia de Splunk (por ejemplo, si usa un proxy o se ejecuta en Splunk Cloud), puede reenviar estos eventos al recopilador de eventos de HTTP de Splunk mediante [esta función de Azure desencadenada por nuevos mensajes en el centro de eventos](https://github.com/Microsoft/AzureFunctionforSplunkVS). 
>

## <a name="next-steps"></a>Pasos siguientes

* [Interpretación de un esquema de registros de auditoría en Azure Monitor](reporting-azure-monitor-diagnostics-audit-log-schema.md)
* [Interpretación de un esquema de registros de inicio de sesión en Azure Monitor](reporting-azure-monitor-diagnostics-sign-in-log-schema.md)
* [Preguntas frecuentes y problemas conocidos](reporting-azure-monitor-diagnostics-overview.md#frequently-asked-questions)