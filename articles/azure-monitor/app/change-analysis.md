---
title: Uso de Application Change Analysis en Azure Monitor para detectar problemas de la aplicación web | Microsoft Docs
description: Use Application Change Analysis en Azure Monitor para solucionar problemas de aplicaciones en sitios activos de Azure App Service.
services: application-insights
author: cawams
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: cawa
ms.openlocfilehash: 3efa26a1eaea8f522d9717efb0de0ec8e1682e0e
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/15/2019
ms.locfileid: "67875162"
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

Si la suscripción incluye varias aplicaciones web, podría resultar ineficaz habilitar el servicio en el nivel de aplicación web. En este caso, siga estas instrucciones alternativas.

### <a name="register-the-change-analysis-resource-provider-for-your-subscription"></a>Registro del proveedor de recursos de Change Analysis para la suscripción

1. Registre la marca de características de Change Analysis (versión preliminar). Dado que la marca de características se encuentra en versión preliminar, es preciso registrarla para que sea visible para la suscripción:

   1. Abra [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/).

      ![Captura de pantalla del cambio de Cloud Shell](./media/change-analysis/cloud-shell.png)

   1. Cambie el tipo de shell a **PowerShell**.

      ![Captura de pantalla del cambio de Cloud Shell](./media/change-analysis/choose-powershell.png)

   1. Ejecute el siguiente comando de PowerShell:

        ``` PowerShell
        Set-AzContext -Subscription <your_subscription_id> #set script execution context to the subscription you are trying to enable
        Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.ChangeAnalysis" -ListAvailable #Check for feature flag availability
        Register-AzureRmProviderFeature -FeatureName PreviewAccess -ProviderNamespace Microsoft.ChangeAnalysis #Register feature flag
        ```

1. Registre el proveedor de recursos de Change Analysis para la suscripción.

   - Vaya a **Suscripciones** y seleccione la suscripción que quiere habilitar en el servicio de cambios. Después seleccione los proveedores de recursos:

        ![Captura de pantalla que muestra cómo registrar al proveedor de recursos de Change Analysis](./media/change-analysis/register-rp.png)

       - Seleccione **Microsoft.ChangeAnalysis**. En la parte superior de la página, seleccione **Registrar**.

       - Después de habilitar al proveedor de recursos, puede establecer una etiqueta oculta en la aplicación web para detectar cambios en el nivel de implementación. Para establecer una etiqueta oculta, siga las instrucciones debajo de la sección **No se puede obtener la información de Change Analysis**.

   - También puede usar un script de PowerShell para registrar el proveedor de recursos:

        ```PowerShell
        Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState #Check if RP is ready for registration

        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.ChangeAnalysis" #Register the Change Analysis RP
        ```

        Para establecer una etiqueta oculta en una aplicación web mediante PowerShell, ejecute el siguiente comando:

        ```powershell
        $webapp=Get-AzWebApp -Name <name_of_your_webapp>
        $tags = $webapp.Tags
        $tags[“hidden-related:diagnostics/changeAnalysisScanEnabled”]=$true
        Set-AzResource -ResourceId <your_webapp_resourceid> -Tag $tag
        ```

     > [!NOTE]
     > Después de agregar la etiqueta oculta, es posible que deba esperar hasta cuatro horas antes de que se reflejen los cambios. Los resultados se retrasan porque Change Analysis solo examina la aplicación web cada cuatro horas. La programación de cuatro horas limita el impacto del examen en el rendimiento.

## <a name="next-steps"></a>Pasos siguientes

- Habilitar Application Insights para [aplicaciones de Azure App Services](azure-web-apps.md).
- Habilitar Application Insights para [aplicaciones hospedadas en IIS en máquina virtual de Azure y conjunto de escalado de máquinas virtuales de Azure](azure-vm-vmss-apps.md).
- Obtenga más información sobre [Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview), que ayuda a la eficacia de Change Analysis.
