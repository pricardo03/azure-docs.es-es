---
title: Aplicación de Azure Monitor cambiar analysis - detectar los cambios que puedan afectar a problemas de sitios activos/interrupciones con aplicaciones de Azure Monitor cambiar análisis | Microsoft Docs
description: Solucionar problemas de sitios activos de aplicaciones en Azure App Services con el análisis de cambio de aplicación de Azure Monitor
services: application-insights
author: cawams
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: cawa
ms.openlocfilehash: b04bd57c66b52e9a2c14d43c9e89d7c54fc48ae2
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65415853"
---
# <a name="azure-monitor-application-change-analysis-public-preview"></a>Análisis de cambio de aplicación de Azure Monitor (versión preliminar)

Cuando se produce una problema o interrupción de un sitio en directo, determinar rápidamente la causa raíz es fundamental. Estándar de las soluciones de supervisión puede ayudarle a identificar rápidamente que hay un problema y, a menudo incluso qué componente está fallando. Pero esto no siempre da lugar a una explicación de por qué se está produciendo el error inmediata. El sitio de trabajado cinco minutos atrás, ahora ha interrumpido. ¿Qué cambió en los últimos cinco minutos? Se trata de la pregunta que la nueva característica a análisis de cambio de la aplicación está diseñada para responder a Azure Monitor. Mediante la creación de la eficacia de la [recursos de Azure Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview) análisis de cambio de la aplicación proporciona una visión de los cambios de la aplicación de Azure App Service para aumentar la observación y reducir el MTTR (tiempo medio para reparación).

> [!IMPORTANT]
> Análisis de cambio de aplicación de Azure Monitor está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="how-do-i-use-application-change-analysis"></a>¿Cómo se puede usar análisis de cambio de la aplicación?

Análisis de cambio de aplicación de Azure Monitor se integran actualmente en el autoservicio **diagnosticar y resolver problemas** experimentar, que se puede acceder desde el **Introducción** sección de Azure App Service aplicación:

![Información general de la captura de pantalla de Azure App Service con cuadros rojos alrededor del botón información general de la página y diagnosticar y resolver el botón de problemas](./media/change-analysis/change-analysis.png)

### <a name="enable-change-analysis"></a>Habilitar el análisis de cambio

1. Seleccione **disponibilidad y rendimiento**

    ![captura de pantalla de opciones de solución de problemas de rendimiento y disponibilidad](./media/change-analysis/availability-and-performance.png)

2. Haga clic en el **aplicación se bloquea** icono.

   ![Captura de pantalla con el icono de bloqueos de aplicación](./media/change-analysis/application-crashes-tile.png)

3. Para habilitar **análisis cambios** seleccione **habilitar ahora**.

   ![captura de pantalla de opciones de solución de problemas de rendimiento y disponibilidad](./media/change-analysis/application-crashes.png)

4. Para aprovechar las ventajas del perfil de cambiar el conjunto de funcionalidad de análisis **cambiar análisis**, **buscar cambios de código de**, y **siempre** a **en** y seleccione **guardar**.

    ![Captura de pantalla de la interfaz de usuario de análisis de Azure App Service Habilitar cambio](./media/change-analysis/change-analysis-on.png)

    Si **análisis cambios** está habilitada, podrá detectar los cambios de nivel de recurso. Si **buscar cambios de código de** está habilitado, verá también archivos de implementación y los cambios de configuración. Habilitar **siempre** optimizará el rendimiento del examen de cambio, pero puede incurrir en costes adicionales desde la perspectiva de facturación.

5.  Una vez que todo está habilitada, seleccione **diagnosticar y resolver problemas** > **disponibilidad y rendimiento** > **aplicación se bloquea** le permitirá tener acceso a la experiencia de análisis de cambio. El gráfico summerize el tipo de cambios que ha ocurrido con el tiempo, junto con detalles sobre dichos cambios:

     ![Captura de pantalla de vista de diferencias de cambio](./media/change-analysis/change-view.png)

## <a name="troubleshooting"></a>solución de problemas

### <a name="unable-to-fetch-change-analysis-information"></a>No se puede capturar información de análisis de cambio.

Se trata de un problema temporal con la experiencia de incorporación de vista previa actual. La solución consta de la configuración de una etiqueta oculta en la aplicación web y, a continuación, actualizar la página:

   ![Captura de pantalla de la etiqueta de cambio ocultado](./media/change-analysis/hidden-tag.png)

Para establecer la etiqueta oculta mediante PowerShell:

1. Abra Azure Cloud Shell:

    ![Captura de pantalla de cambio de Azure Cloud Shell](./media/change-analysis/cloud-shell.png)

2. Cambie el tipo de shell para PowerShell:

   ![Captura de pantalla de cambio de Azure Cloud Shell](./media/change-analysis/choose-powershell.png)

3. Ejecute el siguiente comando:

```powershell
$webapp=Get-AzWebApp -Name <name_of_your_webapp>
$tags = $webapp.Tags
$tags[“hidden-related:diagnostics/changeAnalysisScanEnabled”]=$true
Set-AzResource -ResourceId <your_webapp_resourceid> -Tag $tag
```

> [!NOTE]
> Una vez que se agrega la etiqueta a la oculta, es posible que todavía deberá inicialmente esperar hasta cuatro horas para que pueda ver los cambios en primer lugar. Esto es debido a la freqeuncy 4 horas que el servicio de análisis de cambio que se utiliza para analizar la aplicación web mientras se limita el impacto en el rendimiento del examen.

## <a name="next-steps"></a>Pasos siguientes

- Mejorar la supervisión de Azure App Services [habilitando las características de Application Insights](azure-web-apps.md) de Azure Monitor.
- Mejorar la comprensión de la [recursos de Azure Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview) que le ayuda a aplicaciones de Azure Monitor power cambiar análisis. 
