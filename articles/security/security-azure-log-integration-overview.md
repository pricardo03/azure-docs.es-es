---
title: Integración de registros de recursos de Azure en sistemas SIEM | Microsoft Docs
description: Aprenda sobre Azure Log Integration, sus principales funcionalidades y cómo funciona.
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TerryLanfear
ms.assetid: 9c1346e1-baf8-4975-b2f2-42ae05b2dc0a
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/14/2019
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: 93ed7620636535d45791a657d012a9c7056be09d
ms.sourcegitcommit: 70471c4febc7835e643207420e515b6436235d29
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2019
ms.locfileid: "54303197"
---
# <a name="introduction-to-azure-log-integration"></a>Introducción a Azure Log Integration

>[!IMPORTANT]
> La característica Azure Log Integration dejará de utilizarse el 01/06/2019. Las descargas de AzLog se deshabilitaron el 27 de junio de 2018. Para obtener orientación sobre cómo avanzar, consulte el artículo [Use Azure monitor to integrate with SIEM tools](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/) (Uso de Azure Monitor para realizar la integración con herramientas SIEM) 

Azure Log Integration se encuentra disponible para simplificar la tarea de integración de los registros de Azure con el sistema local de Administración de eventos e información de seguridad (SIEM).

 El método recomendado para la integración de registros de Azure consiste en utilizar conectores de su proveedor de SIEM. Azure Monitor ofrece la capacidad de transmitir los registros a los centros de eventos, y el proveedor de SIEM puede escribir conectores para integrar más registros desde el centro de eventos en SIEM.  Para obtener una descripción de cómo funciona esto, siga las instrucciones descritas en [Flujo de datos de supervisión para centros de eventos de datos](../azure-monitor/platform/stream-monitoring-data-event-hubs.md). En el artículo también se enumeran las instancias de SIEM para las que ya están disponibles los conectores directos de Azure.  

> [!IMPORTANT]
> Si su interés principal está en la recopilación de registros de máquina virtual, la mayoría de los proveedores de SIEM incluyen esta opción en su solución. El conector del proveedor de SIEM se usa siempre como alternativa preferida.

La documentación sobre la característica Azure Log Integration se mantendrá hasta que la característica quede en desuso.

Siga leyendo para obtener más información sobre la característica Azure Log Integration:

Azure Log Integration recopila eventos de Windows de los registros del Visor de eventos de Windows, los [registros de actividad de Azure](../azure-monitor/platform/activity-logs-overview.md), las [alertas de Azure Security Center](../security-center/security-center-intro.md) y los [registros de Azure Diagnostics](../azure-monitor/platform/diagnostic-logs-overview.md) de los recursos de Azure. La integración ayuda a la solución SIEM a proporcionar un panel unificado para todos sus recursos, tanto locales como en la nube. Puede usar un panel para recibir, agregar, correlacionar y analizar las alertas de los eventos de seguridad.

> [!NOTE]
> Actualmente, Azure Log Integration solo admite nubes comerciales de Azure y de Azure Government. No se admiten otras nubes.

![Proceso de Azure Log Integration][1]

## <a name="what-logs-can-i-integrate"></a>¿Qué registros se pueden integrar?

Azure genera gran cantidad de registros para cada servicio. Los registros representan tres tipos de registro:

* **Registros de control y administración**: ofrecen visibilidad sobre las operaciones CREATE, UPDATE y DELETE de [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). Los registros de actividad de Azure son un ejemplo de este tipo de registro.
* **Registros del plano de datos**: proporcionan visibilidad sobre los eventos que se producen cuando se usa un recurso de Azure. Un ejemplo de este tipo de registro son los canales **Sistema**, **Seguridad** y **Aplicación** del Visor de eventos de Windows en una máquina virtual Windows. Otro ejemplo es el registro de Azure Diagnostics, que se configura mediante Azure Monitor.
* **Eventos procesados**: proporcionan información sobre las alertas y los eventos analizados procesados por el usuario. Un ejemplo de este tipo son las alertas de Azure Security Center. Azure Security Center procesa y analiza la suscripción para proporcionar alertas significativas para su estado de seguridad actual.

Integración de registro de Azure admite ArcSight, QRadar y Splunk. Consulte a su proveedor de SIEM para determinar si tienen un conector nativo. No use Azure Log Integration si hay un conector nativo.

Si no hay ninguna otra opción disponible, considere la posibilidad de usar Azure Log Integration. En la tabla siguiente se incluye nuestras recomendaciones:

|SIEM | El cliente ya usa el integrador de registros de Azure | El cliente está investigando las opciones de integración de SIEM|
|---------|--------------------------|-------------------------------------------|
|**Splunk** | Inicie la migración al [complemento Azure Monitor para Splunk](https://splunkbase.splunk.com/app/3534/). | Use el [conector de Splunk](https://splunkbase.splunk.com/app/3534/). |
|**QRadar** | Migre o comience a usar el conector de QRadar documentado en la última sección de [Flujo de datos de supervisión de Azure a un centro de eventos para que lo consuma una herramienta externa](../azure-monitor/platform/stream-monitoring-data-event-hubs.md). | Use el conector de QRadar documentado en la última sección de [Flujo de datos de supervisión de Azure a un centro de eventos para que lo consuma una herramienta externa](../azure-monitor/platform/stream-monitoring-data-event-hubs.md). |
|**ArcSight** | Siga usando el integrador de registros de Azure hasta que haya disponible un conector y, a continuación, migre a la solución basada en el conector.  | Como alternativa, considere la posibilidad de usar Azure Log Analytics. No se incorpore a Azure Log Integration, a menos que esté dispuesto a pasar por el proceso de migración cuando el conector esté disponible. |

> [!NOTE]
> Aunque Azure Log Integration es una solución gratuita, hay costos de almacenamiento de Azure asociados con el almacenamiento de la información de archivos de registro.

Si necesita ayuda, puede crear una [solicitud de soporte técnico](../azure-supportability/how-to-create-azure-support-request.md). Para el servicio, seleccione **Log Integration**.

## <a name="next-steps"></a>Pasos siguientes

Este artículo es una introducción a Azure Log Integration. Para más información al respecto y los tipos de registros admitidos, consulte los artículos siguientes:

* [Introducción a Azure Log Integration](security-azure-log-integration-get-started.md). Este tutorial le guía por los pasos de instalación de Azure Log Integration. También se describe cómo integrar registros de almacenamiento de Azure Diagnostics para Windows (WAD), registros de actividad de Azure, alertas de Azure Security Center y registros de auditoría de Azure Active Directory.
* [Preguntas frecuentes sobre Azure Log Integration (P+F)](security-azure-log-integration-faq.md). Estas preguntas frecuentes responden a las preguntas sobre Azure Log Integration.
* Obtenga más información sobre cómo [transmitir datos de supervisión de Azure a un centro de eventos para que lo consuma una herramienta externa](../azure-monitor/platform/stream-monitoring-data-event-hubs.md).

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
