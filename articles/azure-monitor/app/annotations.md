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
ms.date: 11/08/2018
ms.author: mbullwin
ms.openlocfilehash: 8536cce8de79f396b5d60058dd41f733d2627c87
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2019
ms.locfileid: "54119142"
---
# <a name="annotations-on-metric-charts-in-application-insights"></a>Anotaciones sobre gráficos de métricas en Application Insights

En las anotaciones sobre gráficos del [Explorador de métricas](../../azure-monitor/app/metrics-explorer.md) se muestra donde ha implementado una nueva compilación u otros eventos importantes. Permiten ver fácilmente si los cambios tuvieron algún efecto en el rendimiento de la aplicación. Se pueden crear automáticamente con el [sistema de compilación de Azure DevOps Services](https://docs.microsoft.com/azure/devops/pipelines/tasks/). También puede crear anotaciones para marcar los eventos que prefiera si los [crea desde PowerShell](#create-annotations-from-powershell).

> [!NOTE]
> En este artículo se refleja la **experiencia de métricas clásicas** en desuso. Las anotaciones solo están disponibles actualmente en la experiencia clásica y en los **[libros](../../azure-monitor/app/usage-workbooks.md)**. Para más información sobre la actual experiencia de métricas, puede consultar [este artículo](../../azure-monitor/platform/metrics-charts.md).

![Ejemplo de anotaciones con correlación visible con el tiempo de respuesta del servidor](./media/annotations/00.png)

## <a name="release-annotations-with-azure-devops-services-build"></a>Anotaciones de la versión con una compilación de Azure DevOps Services

Las anotaciones de la versión son una característica del servicio Azure Pipelines de Azure DevOps Services. 

### <a name="install-the-annotations-extension-one-time"></a>Instalación de la extensión Anotaciones (una vez)
Para poder crear anotaciones de la versión, debe instalar una de las muchas extensiones de Azure DevOps Services disponibles en Visual Studio Marketplace.

1. Inicie sesión en su proyecto de [Azure DevOps Services](https://visualstudio.microsoft.com/vso/).
2. En Visual Studio Marketplace, [obtenga la extensión de anotaciones de la versión](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations) y agréguela a su organización de Azure DevOps Services.

![En la parte superior derecha de página web de Azure DevOps Services, abra Marketplace. Seleccione Azure DevOps Services y, a continuación, en Azure Pipelines, elija Ver más.](./media/annotations/10.png)

Solo deberá hacerlo una vez para su organización de Azure DevOps Services. Las anotaciones de la versión se pueden configurar ahora para cualquier proyecto de su organización. 

### <a name="configure-release-annotations"></a>Configurar anotaciones de versión

Necesita obtener una clave de API separada para cada plantilla de versión de Azure DevOps Services.

1. Inicie sesión en [Microsoft Azure Portal](https://portal.azure.com) y abra el recurso de Application Insights que supervisa su aplicación. (O bien, [cree uno ahora](../../azure-monitor/app/app-insights-overview.md), si aún no lo ha hecho).
2. Abra **Acceso a la API**, **Id. de Application Insights**.
   
    ![En portal.azure.com, abra el recurso de Application Insights y elija Configuración. Abra el Acceso a la API. Copie el identificador de aplicación.](./media/annotations/20.png)

4. En una ventana de explorador independiente, abra (o cree) la plantilla de versión que administra sus implementaciones de Azure DevOps Services. 
   
    Agregue una tarea y seleccione la tarea de anotación de versión de Application Insights en el menú.
   
    Pegue el **Id. de la aplicación** que ha copiado de la hoja Acceso de API.
   
    ![En Azure DevOps Services, abra Versión, seleccione una canalización de versiones y elija Editar. Haga clic en Agregar tarea y seleccione Anotación de versión de Application Insights. Pegue el Id. de Application Insights.](./media/annotations/30.png)
4. Establezca el campo **APIKey** en una variable `$(ApiKey)`.

5. Vuelva a la ventana de Azure, cree una clave de API y realice una copia de esta.
   
    ![En la hoja Acceso a la API de la ventana de Azure, haga clic en Crear clave de API. Proporcione un comentario, compruebe las anotaciones de escritura y haga clic en Generar clave. Copie la clave nueva.](./media/annotations/40.png)

6. Abra la pestaña Configuración de la plantilla de la versión.
   
    Cree una definición de variable para `ApiKey`.
   
    Pegue su clave de API a la definición de la variable de ApiKey.
   
    ![En la ventana de Azure DevOps Services, seleccione la pestaña Configuración y haga clic en Agregar variable. Establezca el nombre en ApiKey y en el Valor, pegue la clave que acaba de generar y haga clic en el icono de bloqueo.](./media/annotations/50.png)
7. Por último, **guarde** la canalización de la versión.


## <a name="view-annotations"></a>Ver anotaciones
Ahora, cuando se utilice la plantilla de versión para implementar una nueva versión, se enviará una anotación a Application Insights. Las anotaciones se mostrarán en los gráficos del Explorador de métricas.

Haga clic en cualquier marcador de anotación para abrir los detalles de la versión, incluidos el solicitante, la rama de control de código fuente, la canalización de la versión, el entorno y mucho más.

![Haga clic en cualquier marcador de anotación de la versión.](./media/annotations/60.png)

## <a name="create-custom-annotations-from-powershell"></a>Crear anotaciones personalizadas desde PowerShell
También puede crear anotaciones desde cualquier proceso que desee (sin usar Azure DevOps Services). 


1. Realice una copia local del [script de PowerShell en GitHub](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).

2. Obtenga el id. de la aplicación y cree una clave de API desde la hoja Acceso a la API.

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