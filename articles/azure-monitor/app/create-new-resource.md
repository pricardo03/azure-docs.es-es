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
ms.date: 06/10/2019
ms.author: mbullwin
ms.openlocfilehash: 9da52e5a9dfa3b55431d66ed3162172226f71a40
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67073304"
---
# <a name="create-an-application-insights-resource"></a>Creación de recursos en Application Insights

Azure Application Insights muestra los datos de la aplicación en un *recurso*de Microsoft Azure. Por tanto, la creación de un nuevo recurso forma parte de la [configuración de Application Insights para supervisar una aplicación nueva][start]. Después de haber creado el recurso nuevo, puede obtener su clave de instrumentación y usarla para configurar el SDK de Application Insights. La clave de instrumentación vincula la telemetría al recurso.

## <a name="sign-in-to-microsoft-azure"></a>Iniciar sesión en Microsoft Azure

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="create-an-application-insights-resource"></a>Creación de recursos en Application Insights

Inicie sesión en [Azure Portal](https://portal.azure.com) y cree un recurso de Application Insights:

![En la esquina superior izquierda, haga clic en el signo "+". Seleccionar Herramientas de desarrollo y, a continuación, Application Insights](./media/create-new-resource/new-app-insights.png)

   | Configuración        |  Valor           | DESCRIPCIÓN  |
   | ------------- |:-------------|:-----|
   | **Nombre**      | Nombre único global | Nombre que identifica la aplicación que está supervisando. |
   | **Grupo de recursos**     | myResourceGroup      | Nombre para el grupo de recursos nuevo o existente que hospedará los datos de Application Insights. |
   | **Ubicación** | Este de EE. UU | Elija una ubicación cerca de usted o de donde se hospeda la aplicación. |

Escriba los valores apropiados en los campos obligatorios y, a continuación, seleccione **Revisar y crear**.

![Escriba los valores en los campos obligatorios y, a continuación, seleccione "Revisar y crear".](./media/create-new-resource/review-create.png)

Cuando se haya creado la aplicación, se abrirá un panel nuevo. En este panel podrá ver los datos de uso y rendimiento de la aplicación supervisada. 

## <a name="copy-the-instrumentation-key"></a>Copia de la clave de instrumentación

La clave de instrumentación identifica el recurso con el que quiere asociar los datos de telemetría. Necesitará la copia para agregar la clave de instrumentación al código de la aplicación.

![Hacer clic y copiar la clave de instrumentación](./media/create-new-resource/instrumentation-key.png)

## <a name="install-the-sdk-in-your-app"></a>Instalación del SDK en la aplicación

Instale el SDK de Application Insights en su aplicación. Este paso depende en gran medida del tipo de aplicación.

Use la clave de instrumentación para configurar [el SDK que instala en la aplicación][start].

El SDK incluye módulos estándar que envían telemetría sin tener que escribir código adicional. Para realizar un seguimiento de las acciones del usuario o diagnosticar los problemas con más detalle, [use la API][api] para enviar su propia telemetría.

## <a name="creating-a-resource-automatically"></a>Creación de un recurso de forma automática
Puede escribir un [script de PowerShell](../../azure-monitor/app/powershell.md) para crear automáticamente un recurso.

## <a name="next-steps"></a>Pasos siguientes
* [Búsqueda de diagnóstico](../../azure-monitor/app/diagnostic-search.md)
* [Exploración de métricas](../../azure-monitor/app/metrics-explorer.md)
* [Escribir consultas de Analytics](../../azure-monitor/app/analytics.md)

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[metrics]: ../../azure-monitor/app/metrics-explorer.md
[start]: ../../azure-monitor/app/app-insights-overview.md