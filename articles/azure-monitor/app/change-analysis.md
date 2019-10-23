---
title: Uso de Application Change Analysis en Azure Monitor para detectar problemas de la aplicación web | Microsoft Docs
description: Use Application Change Analysis en Azure Monitor para solucionar problemas de aplicaciones en sitios activos de Azure App Service.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: cawams
ms.author: cawa
ms.date: 05/07/2019
ms.openlocfilehash: 3805d7b39c25bcb213a1d4f110161dcd00eb3630
ms.sourcegitcommit: 1bd2207c69a0c45076848a094292735faa012d22
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2019
ms.locfileid: "72678248"
---
# <a name="use-application-change-analysis-preview-in-azure-monitor"></a>Uso de Application Change Analysis (versión preliminar) en Azure Monitor

Cuando se produce una interrupción o un problema en un sitio activo, determinar rápidamente la causa raíz es fundamental. Las soluciones de supervisión estándares pueden alertarlo sobre un problema. Incluso pueden indicarle qué componente está presentando el error. Sin embargo, esta alerta no siempre explica inmediatamente la causa del error. Sabrá que su sitio funcionaba hace cinco minutos y que ahora no funciona. ¿Qué cambió en los últimos cinco minutos? Esta es la pregunta que Application Change Analysis está diseñado para responder en Azure Monitor.

A partir de la eficacia de [Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview), Change Analysis proporciona conclusiones sobre los cambios de la aplicación de Azure para aumentar la observación y reducir el tiempo medio para reparación.

> [!IMPORTANT]
> Change Analysis se encuentra actualmente en versión preliminar. Esta versión preliminar se proporciona sin un contrato de nivel de servicio. Esta versión no se recomienda para usarse en cargas de trabajo. Es posible que algunas características no sean compatibles o que sus funcionalidades estén limitadas. Para obtener más información, consulte [Términos de uso complementarios para las Versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="overview"></a>Información general

Change Analysis detecta varios tipos de cambios, desde el nivel de infraestructura hasta la implementación de la aplicación. Es un proveedor de recursos de Azure de nivel de suscripción que comprueba los cambios de recursos en la suscripción. Change Analysis proporciona datos para que varias herramientas de diagnóstico ayuden a los usuarios a comprender qué cambios pueden haber causado los problemas.

El siguiente diagrama muestra la arquitectura de Change Analysis:

![Diagrama de la arquitectura del procedimiento mediante el que Change Analysis obtiene los datos de cambio y los envía a las herramientas de cliente](./media/change-analysis/overview.png)

Actualmente Change Analysis se integra con la experiencia **Diagnosticar y solucionar problemas** de la aplicación web de App Service. Para habilitar la detección de cambios y ver los cambios en la aplicación web, consulte la sección *Change Analysis para la característica de aplicaciones web* más adelante en este artículo.

### <a name="azure-resource-manager-deployment-changes"></a>Cambios de implementación de Azure Resource Manager

Mediante [Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview), Change Analysis proporciona un registro histórico de la forma en que los recursos de Azure que hospedan a la aplicación han cambiado con el tiempo. Por ejemplo, Change Analysis puede detectar cambios en las reglas de configuración de IP, las identidades administradas y las reglas de configuración de SSL. Por lo que si se agrega una etiqueta a una aplicación web, Change Analysis refleja el cambio. Esta información está disponible mientras el proveedor de recursos `Microsoft.ChangeAnalysis` está habilitado en la suscripción a Azure.

### <a name="changes-in-web-app-deployment-and-configuration"></a>Cambios en la implementación y configuración de aplicaciones web

Change Analysis captura el estado de implementación y configuración de una aplicación cada 4 horas. Puede detectar, por ejemplo, los cambios en las variables de entorno de aplicación. La herramienta calcula las diferencias y presenta qué ha cambiado. A diferencia de los cambios de Resource Manager, es posible que la información sobre cambios en la implementación de código no esté disponible en la herramienta inmediatamente. Para ver los cambios más recientes en Change Analysis, seleccione **Scan changes now** (Examinar cambios ahora).

![Captura de pantalla del botón "Scan changes now"](./media/change-analysis/scan-changes.png)

### <a name="dependency-changes"></a>Cambios de dependencia

Los cambios realizados en las dependencias de recursos también pueden causar problemas en una aplicación web. Por ejemplo, si llama una aplicación web a una caché de Redis Cache, la SKU la dicha caché podría afectar al rendimiento de la aplicación de web. Para detectar los cambios en las dependencias, Change Analysis comprueba el registro DNS de la aplicación web. De este modo, identifica los cambios en todos los componentes de aplicación que podrían ocasionar problemas.
Actualmente se admiten las siguientes dependencias:
- Web Apps
- Azure Storage
- Azure SQL


## <a name="change-analysis-for-the-web-apps-feature"></a>Change Analysis para la característica de Web Apps

En Azure Monitor, Change Analysis actualmente está integrado en la experiencia de autoservicio **Diagnosticar y solucionar problemas**. Obtenga acceso a esta experiencia desde la pagina **Información general** de la aplicación de App Service.

![Captura de pantalla de los botones "Información general" y "Diagnosticar y solucionar problemas"](./media/change-analysis/change-analysis.png)

### <a name="enable-change-analysis-in-the-diagnose-and-solve-problems-tool"></a>Habilitación de Change Analysis en la herramienta Diagnosticar y solucionar problemas

1. Seleccione **Availability and Performance** (Disponibilidad y rendimiento).

    ![Captura de pantalla de opción de solución de problemas "Disponibilidad y rendimiento"](./media/change-analysis/availability-and-performance.png)

1. Seleccione **Application Changes** (Cambios de la aplicación). Tenga en cuenta que esta característica también está disponible en **Application Crashes** (Bloqueos de la aplicación).

   ![Captura de pantalla del botón "Application Crashes"](./media/change-analysis/application-changes.png)

1. Para habilitar Change Analysis, seleccione **Habilitar ahora**.

   ![Captura de las opciones de "Application Crashes"](./media/change-analysis/enable-changeanalysis.png)

1. Active **Change Analysis** y seleccione **Guardar**.

    ![Captura de pantalla de la interfaz de usuario "Habilitar Change Analysis"](./media/change-analysis/change-analysis-on.png)


1. Para obtener acceso a Change Analysis, seleccione **Diagnosticar y solucionar problemas** > **Availability and Performance (disponibilidad y rendimiento)**  > **Application Crashes (bloqueos de aplicación)** . Verá un gráfico que resume el tipo de cambios a lo largo del tiempo junto con detalles sobre estos cambios:

     ![Captura de pantalla de la vista de diferencias de cambios](./media/change-analysis/change-view.png)


### <a name="enable-change-analysis-at-scale"></a>Habilitación de Change Analysis a gran escala

Si la suscripción incluye varias aplicaciones web, podría resultar ineficaz habilitar el servicio en el nivel de aplicación web. Ejecute el siguiente script para habilitar todas las aplicaciones web de su suscripción.

Requisitos previos:
* Módulo Az de PowerShell. Siga las instrucciones de [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-2.6.0).

Ejecute el siguiente script:

```PowerShell
# Log in to your Azure subscription
Connect-AzAccount

# Get subscription Id
$SubscriptionId = Read-Host -Prompt 'Input your subscription Id'

# Make Feature Flag visible to the subscription
Set-AzContext -SubscriptionId $SubscriptionId

# Register resource provider
Register-AzResourceProvider -ProviderNamespace "Microsoft.ChangeAnalysis"


# Enable each web app
$webapp_list = Get-AzWebApp | Where-Object {$_.kind -eq 'app'}
foreach ($webapp in $webapp_list)
{
    $tags = $webapp.Tags
    $tags[“hidden-related:diagnostics/changeAnalysisScanEnabled”]=$true
    Set-AzResource -ResourceId $webapp.Id -Tag $tags -Force
}

```



## <a name="next-steps"></a>Pasos siguientes

- Habilitar Application Insights para [aplicaciones de Azure App Services](azure-web-apps.md).
- Habilitar Application Insights para [aplicaciones hospedadas en IIS en máquina virtual de Azure y conjunto de escalado de máquinas virtuales de Azure](azure-vm-vmss-apps.md).
- Obtenga más información sobre [Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview), que ayuda a la eficacia de Change Analysis.
