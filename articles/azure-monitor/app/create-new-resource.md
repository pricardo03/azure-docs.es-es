---
title: Creación de un recurso de Azure Application Insights | Microsoft Docs
description: Describe la configuración manual de la supervisión de Application Insights para una nueva aplicación activa.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 878b007e-161c-4e36-8ab2-3d7047d8a92d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: mbullwin
ms.openlocfilehash: 5daf0944212dc4b8040a39e6efbf5bb25f7f39f0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60901819"
---
# <a name="create-an-application-insights-resource"></a>Creación de recursos en Application Insights
Azure Application Insights muestra los datos de la aplicación en un *recurso*de Microsoft Azure. Por tanto, la creación de un nuevo recurso forma parte de la [configuración de Application Insights para supervisar una aplicación nueva][start]. En muchos casos, la creación de un recurso puede realizarla automáticamente el IDE. Pero en algunos casos, creará un recurso manualmente. Por ejemplo, para tener recursos independientes para las compilaciones de desarrollo y producción de la aplicación.

Después de haber creado el recurso, obtiene su clave de instrumentación y la usa para configurar el SDK en la aplicación. La clave de recurso vincula la telemetría al recurso.

## <a name="sign-up-to-microsoft-azure"></a>Suscripción a Microsoft Azure
Si todavía no dispone de una [cuenta Microsoft, obtenga una ahora](https://live.com). (Si usa servicios como Outlook.com, OneDrive, Windows Phone o XBox Live, ya tiene una cuenta Microsoft).

También necesita una suscripción a [Microsoft Azure](https://azure.com). Si su equipo u organización tiene una suscripción a Azure, el propietario puede agregarle a ella mediante su Windows Live ID. Solo se le cobrará por lo que use. El plan básico predeterminado permite hacer un determinado uso experimental de forma gratuita.

Cuando tenga acceso a una suscripción, inicie sesión en Application Insights en [https://portal.azure.com](https://portal.azure.com) y use su Live ID para iniciar sesión.

## <a name="create-an-application-insights-resource"></a>Creación de recursos en Application Insights
En el [portal.azure.com](https://portal.azure.com), agregue un recurso de Application Insights:

![Haga clic en Nuevo, Application Insights.](./media/create-new-resource/01-new.png)

* **Tipo de aplicación** afecta a lo que ve en la hoja de información general y las propiedades disponibles en el [Explorador de métricas de Azure][metrics]. Si no ve el tipo de aplicación, elija General.
* **suscripción** es su cuenta de pago de Azure.
* **grupo de recursos** resulta práctico para administrar las propiedades, como el control de acceso. Si ya ha creado otros recursos de Azure, puede optar por colocar este recurso nuevo en el mismo grupo.
* **ubicación** es donde se guardan los datos.
* **Anclar al panel** coloca un icono de acceso rápido al recurso en la página principal de Azure. Se recomienda su uso.

Cuando se haya creado la aplicación, se abrirá una nueva hoja. En esta hoja podrá ver datos de uso y rendimiento sobre la aplicación. 

Para volver a ella la próxima vez que inicie sesión en Azure, busque el icono de inicio rápido de la aplicación en el panel de inicio (pantalla principal). O bien, haga clic en Examinar para buscar esta opción.

## <a name="copy-the-instrumentation-key"></a>Copia de la clave de instrumentación
La clave de instrumentación identifica al recurso que ha creado. La necesita para proporcionársela al SDK.

![Haga clic en Essentials y elija la clave de instrumentación, CTRL + C](./media/create-new-resource/02-props.png)

## <a name="install-the-sdk-in-your-app"></a>Instalación del SDK en la aplicación
Instale el SDK de Application Insights en su aplicación. Este paso depende en gran medida del tipo de aplicación. 

Use la clave de instrumentación para configurar [el SDK que instala en la aplicación][start].

El SDK incluye módulos estándar que envían telemetría sin tener que escribir código. Para realizar un seguimiento de las acciones del usuario o diagnosticar los problemas con más detalle, [use la API][api] para enviar su propia telemetría.

## <a name="monitor"></a>Visualización de los datos de telemetría
Cierre la hoja de inicio rápido para volver a la hoja de aplicación en el portal de Azure.

Haga clic en el icono de búsqueda para ver la [Búsqueda de diagnóstico][diagnostic], donde aparecen los primeros eventos. 

Si espera más datos, haga clic en **Actualizar** después de unos segundos.

## <a name="creating-a-resource-automatically"></a>Creación de un recurso de forma automática
Puede escribir un [script de PowerShell](../../azure-monitor/app/powershell.md) para crear automáticamente un recurso.

## <a name="next-steps"></a>Pasos siguientes
* [Creación de un panel](../../azure-monitor/app/app-insights-dashboards.md)
* [Búsqueda de diagnóstico](../../azure-monitor/app/diagnostic-search.md)
* [Exploración de métricas](../../azure-monitor/app/metrics-explorer.md)
* [Escribir consultas de Analytics](../../azure-monitor/app/analytics.md)

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[metrics]: ../../azure-monitor/app/metrics-explorer.md
[start]: ../../azure-monitor/app/app-insights-overview.md

