---
title: Anotaciones de la versión de Application Insights | Microsoft Docs
description: Agregue marcadores de implementación o compilación a sus gráficos del Explorador de métricas en Application Insights.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 23173e33-d4f2-4528-a730-913a8fd5f02e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: mbullwin
ms.openlocfilehash: 6567d7f2ebaab5bd7b5bc8fb7b5a62970f169161
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66476175"
---
# <a name="annotations-on-metric-charts-in-application-insights"></a>Anotaciones sobre gráficos de métricas en Application Insights

En las anotaciones sobre gráficos del [Explorador de métricas](../../azure-monitor/app/metrics-explorer.md) se muestra donde ha implementado una nueva compilación u otros eventos importantes. Permiten ver fácilmente si los cambios tuvieron algún efecto en el rendimiento de la aplicación. Se pueden crear automáticamente con el [sistema de compilación de Azure DevOps Services](https://docs.microsoft.com/azure/devops/pipelines/tasks/). También puede crear anotaciones para marcar los eventos que prefiera si los crea desde PowerShell.

> [!NOTE]
> En este artículo se refleja la **experiencia de métricas clásicas** en desuso. Las anotaciones solo están disponibles actualmente en la experiencia clásica y en los **[libros](../../azure-monitor/app/usage-workbooks.md)** . Para más información sobre la actual experiencia de métricas, puede consultar [este artículo](../../azure-monitor/platform/metrics-charts.md).

![Ejemplo de las anotaciones](./media/annotations/0-example.png)

## <a name="release-annotations-with-azure-devops-services-build"></a>Anotaciones de la versión con una compilación de Azure DevOps Services

Las anotaciones de la versión son una característica del servicio Azure Pipelines de Azure DevOps Services.

### <a name="install-the-annotations-extension-one-time"></a>Instalación de la extensión Anotaciones (una vez)
Para poder crear anotaciones de la versión, debe instalar una de las muchas extensiones de Azure DevOps Services disponibles en Visual Studio Marketplace.

1. Inicie sesión en su proyecto de [Azure DevOps Services](https://azure.microsoft.com/services/devops/).
2. En Visual Studio Marketplace, [obtenga la extensión de anotaciones de la versión](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations) y agréguela a su organización de Azure DevOps Services.

![Seleccione una organización de DevOps de Azure y, a continuación, instalar.](./media/annotations/1-install.png)

Solo deberá hacerlo una vez para su organización de Azure DevOps Services. Las anotaciones de la versión se pueden configurar ahora para cualquier proyecto de su organización.

### <a name="configure-release-annotations"></a>Configurar anotaciones de versión

Necesita obtener una clave de API separada para cada plantilla de versión de Azure DevOps Services.

1. Inicie sesión en el [portal de Microsoft Azure](https://portal.azure.com) y abra el recurso de Application Insights que supervisa su aplicación. (O bien, [cree uno ahora](../../azure-monitor/app/app-insights-overview.md), si aún no lo ha hecho).
2. Abra el **acceso a la API** pestaña y copie el **Id. de Application Insights**.
   
    ![En portal.azure.com, abra el recurso de Application Insights y elija Configuración. Abra el Acceso a la API. Copie el identificador de aplicación.](./media/annotations/2-app-id.png)

4. En una ventana de explorador independiente, abra (o cree) la plantilla de versión que administra sus implementaciones de Azure DevOps Services.
   
    Agregue una tarea y seleccione la tarea de anotación de versión de Application Insights en el menú.

   ![Haga clic en el signo más para agregar tareas y seleccione anotación de versión de Application Insights. Pegue el identificador de aplicación Insights.](./media/annotations/3-add-task.png)

    Pegar la **Id. de aplicación** que ha copiado en la ficha acceso de API.
   
    ![Pegue el Id. de Application Insights](./media/annotations/4-paste-app-id.png)

5. Vuelva a la ventana de Azure, cree una clave de API y realice una copia de esta.
   
    ![En la ficha acceso de API en la ventana de Azure, haga clic en crear clave de API.](./media/annotations/5-create-api-key.png)

    ![En la pestaña claves de creación de API proporcionan un comentario, compruebe las anotaciones de escritura y haga clic en generar clave. Copie la clave nueva.](./media/annotations/6-create-api-key.png)

6. Abra la pestaña Configuración de la plantilla de la versión.
   
    Cree una definición de variable para `ApiKey`.
   
    Pegue su clave de API a la definición de la variable de ApiKey.
   
    ![En la ventana Servicios de Azure DevOps, seleccione la pestaña Variable y haga clic en Agregar. Establezca el nombre en ApiKey y en el valor, pegue la clave que generó y haga clic en el icono de bloqueo.](./media/annotations/7-paste-api-key.png)
1. Por último, **guarde** la canalización de la versión.


## <a name="view-annotations"></a>Ver anotaciones
Ahora, cuando se utilice la plantilla de versión para implementar una nueva versión, se enviará una anotación a Application Insights. Las anotaciones se mostrarán en los gráficos del Explorador de métricas.

Haga clic en cualquier marcador de anotación (flecha gris claro) para abrir los detalles sobre la versión, incluidos el solicitante, la bifurcación de control de código fuente, versión de canalización, el entorno y mucho más.

![Haga clic en cualquier marcador de anotación de la versión.](./media/annotations/8-release.png)

## <a name="create-custom-annotations-from-powershell"></a>Crear anotaciones personalizadas desde PowerShell
También puede crear anotaciones desde cualquier proceso que desee (sin usar Azure DevOps Services). 


1. Realice una copia local del [script de PowerShell en GitHub](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).

2. Obtenga el identificador de aplicación y cree una clave de API desde la ficha acceso de API.

3. Llame al script de esta forma:

```PS

     .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }
```

El script se puede modificar fácilmente (por ejemplo, para crear anotaciones para el pasado).

## <a name="next-steps"></a>Pasos siguientes

* [Crear elementos de trabajo](../../azure-monitor/app/diagnostic-search.md#create-work-item)
* [Automation con PowerShell](../../azure-monitor/app/powershell.md)
