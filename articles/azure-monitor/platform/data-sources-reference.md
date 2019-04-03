---
title: Orígenes de datos de supervisión en Azure
description: Obtenga información sobre todos los orígenes de datos de supervisión disponibles hoy en Azure.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/12/2018
ms.author: johnkem
ms.subservice: ''
ms.openlocfilehash: c703f735f59a8425c2a14641781f482a6e2d1c78
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2019
ms.locfileid: "58848536"
---
# <a name="consume-monitoring-data-from-azure"></a>Consume monitoring data from Azure (Consumo de datos de supervisión de Azure)

En toda la plataforma Azure, reunimos datos de supervisión en un solo lugar con la canalización de Azure Monitor. Sin embargo, a nivel práctico, sepa que hoy no todos los datos de supervisión están disponibles aún en dicha canalización. En este artículo, resumiremos las diversas formas en que puede tener acceso, mediante programación, a los datos de supervisión de los servicios de Azure.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="options-for-data-consumption"></a>Opciones para el consumo de datos

| Tipo de datos | Categoría | Servicios admitidos | Métodos de acceso |
| --- | --- | --- | --- |
| Métricas en el nivel de plataforma de Azure Monitor | Métricas | [Consulte la lista aquí](metrics-supported.md) | <ul><li>**API REST:** [API Metric de Azure Monitor](https://docs.microsoft.com/rest/api/monitor/metrics)</li><li>**Storage Blob o centro de eventos:** [Configuración de diagnóstico](diagnostic-logs-overview.md#diagnostic-settings)</li></ul> |
| Métricas de SO invitado de proceso (p. ej., contadores de rendimiento) | Métricas | Máquinas virtuales [Windows](/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines) y Linux (v2), [Cloud Services](../../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md), [Service Fabric](../../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md) | <ul><li>**Tabla o blob de almacenamiento:** [diagnósticos de Azure para Windows o Linux](diagnostics-extension-to-storage.md)</li><li>**Centro de eventos:** [diagnósticos de Azure para Windows](diagnostics-extension-stream-event-hubs.md)</li></ul> |
| Métricas personalizadas o de la aplicación | Métricas | Cualquier aplicación instrumentada con Application Insights | <ul><li>**API REST:** [API REST de Application Insights](https://dev.applicationinsights.io/reference)</li></ul> |
| Métricas de almacenamiento | Métricas | Azure Storage | <ul><li>**Tabla de almacenamiento:** [Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/storage-analytics)</li></ul> |
| Datos de facturación | Métricas | Todos los servicios de Azure | <ul><li>**API REST:** [API RateCard y de uso de recursos de Azure](../../billing/billing-usage-rate-card-overview.md)</li></ul> |
| Registro de actividad | Eventos | Todos los servicios de Azure | <ul><li>**API REST:** [API de eventos de Azure Monitor](https://docs.microsoft.com/rest/api/monitor/eventcategories)</li><li>**Storage Blob o centro de eventos:** [Perfil de registro](activity-logs-overview.md#export-the-activity-log-with-a-log-profile)</li></ul> |
| Registros de diagnóstico de Azure Monitor | Eventos | [Consulte la lista aquí](diagnostic-logs-schema.md) | <ul><li>**Storage Blob o centro de eventos:** [Configuración de diagnóstico](diagnostic-logs-overview.md#diagnostic-settings)</li></ul> |
| Registros de SO invitado de proceso (p. ej., IIS, ETW, syslogs) | Eventos | Máquinas virtuales [Windows](/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines) y Linux (v2), [Cloud Services](../../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md), [Service Fabric](../../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md) | <ul><li>**Tabla o blob de almacenamiento:** [diagnósticos de Azure para Windows o Linux](diagnostics-extension-to-storage.md)</li><li>**Centro de eventos:** [diagnósticos de Azure para Windows](diagnostics-extension-stream-event-hubs.md)</li></ul> |
| Registros de App Service | Eventos | App Services | <ul><li>**Almacenamiento de blobs, tabla o archivo:** [diagnósticos de la aplicación web](../../app-service/troubleshoot-diagnostic-logs.md)</li></ul> |
| Registros de almacenamiento | Eventos | Azure Storage | <ul><li>**Tabla de almacenamiento:** [Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/storage-analytics)</li></ul> |
| Alertas de Security Center | Eventos | Azure Security Center | <ul><li>**API REST:** [alertas de seguridad](https://msdn.microsoft.com/library/mt704050.aspx)</li></ul> |
| Informes de Active Directory | Eventos | Azure Active Directory | <ul><li>**API REST:** [Graph API de Azure Active Directory](../../active-directory/reports-monitoring/concept-reporting-api.md)</li></ul> |
| Estado del recurso de Security Center | Status | [Todos los recursos admitidos](https://msdn.microsoft.com/library/mt704041.aspx#Anchor_1) | <ul><li>**API REST:** [estados de seguridad](https://msdn.microsoft.com/library/mt704041.aspx)</li></ul> |
| Estado de los recursos | Status | Servicios admitidos | <ul><li>**API REST:** [API REST de Resource Health](https://azure.microsoft.com/blog/reduce-troubleshooting-time-with-azure-resource-health/)</li></ul> |
| Alertas de métricas de Azure Monitor | Notificaciones | [Consulte la lista aquí](metrics-supported.md) | <ul><li>**Webhook:** [alertas de métricas de Azure](alerts-webhooks.md)</li></ul> |
| Alertas de registro de actividad de Azure Monitor | Notificaciones | Todos los servicios de Azure | <ul><li>**Webhook:** alertas de registro de actividad de Azure</li></ul> |
| Notificaciones de escalado automático | Notificaciones | [Consulte la lista aquí](autoscale-overview.md#supported-services-for-autoscale) | <ul><li>**Webhook:** [esquema de carga de webhook de notificación de escalado automático](autoscale-webhook-email.md#autoscale-notification-webhook-payload-schema)</li></ul> |
| Alertas de consulta de búsqueda de registros | Notificaciones | Registros de Azure Monitor | <ul><li>**Webhook:** [acciones de webhook para reglas de alertas de registro](alerts-log-webhook.md)</li></ul> |
| Alertas de métricas de Application Insights | Notificaciones | Application Insights | <ul><li>**Webhook:** [alertas de Application Insights](../../azure-monitor/app/alerts.md)</li></ul> |
| Pruebas web de Application Insights | Notificaciones | Application Insights | <ul><li>**Webhook:** [alertas de Application Insights](../../azure-monitor/app/alerts.md)</li></ul> |

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre las [métricas de Azure Monitor](data-platform.md)
- Más información sobre [el registro de actividad de Azure](activity-logs-overview.md)
- Más información sobre los [registros de diagnóstico de Azure](diagnostic-logs-overview.md)

