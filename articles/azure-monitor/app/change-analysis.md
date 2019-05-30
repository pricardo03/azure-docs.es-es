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
ms.openlocfilehash: 5bd3816e65398283de85b4551a137b3f97db4cc7
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66226327"
---
# <a name="application-change-analysis-public-preview"></a>Análisis de cambio de la aplicación (versión preliminar)

Cuando se produce una problema o interrupción de un sitio en directo, determinar rápidamente la causa raíz es fundamental. Estándar de las soluciones de supervisión puede ayudarle a identificar rápidamente que hay un problema y, a menudo incluso qué componente está fallando. Pero esto no siempre da lugar a una explicación de por qué se está produciendo el error inmediata. El sitio de trabajado cinco minutos atrás, ahora ha interrumpido. ¿Qué cambió en los últimos cinco minutos? Se trata de la pregunta que la nueva característica a análisis de cambio de la aplicación está diseñada para responder a Azure Monitor. Mediante la creación de la eficacia de la [recursos de Azure Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview) análisis de cambio de la aplicación proporciona una visión de los cambios de la aplicación de Azure para aumentar la observación y reducir el MTTR (tiempo medio para reparación).

> [!IMPORTANT]
> Análisis de cambio de aplicación de Azure Monitor está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="overview-of-change-analysis-service"></a>Información general del servicio de análisis de cambios
El servicio de análisis de cambios detecta varios tipos de cambios de nivel de infraestructura completamente a la implementación de la aplicación. Es un proveedor de recursos de Azure de nivel de suscripción que busca en los cambios de recursos en la suscripción y proporciona datos para varias herramientas de diagnóstico ayudar a los usuarios a comprender qué cambios se haya podido provocar problemas.

El siguiente diagrama ilustra la arquitectura del servicio de análisis de cambio: ![Diagrama de arquitectura para cómo obtiene el servicio de análisis de cambio de datos modificados y proporcionar datos a las herramientas de cliente](./media/change-analysis/overview.png)

Actualmente, la herramienta está integrada en los servicios de aplicación web app, diagnosticar y resolver problemas experiencia. Consulte *servicio de análisis de cambio para la aplicación de servicios Web* sección sobre cómo habilitar y ver los cambios realizados en una aplicación web.

### <a name="azure-resource-manager-deployment-changes"></a>Cambios de implementación de Azure Resource Manager
Aprovechamiento de [recursos de Azure Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview) la herramienta de análisis de cambio proporciona un registro histórico de cómo han cambiado los recursos de Azure que hospedan la aplicación con el tiempo. Por ejemplo, si una aplicación web tiene una etiqueta que se agregan a él, el cambio se reflejará en la herramienta de análisis de cambio.
Esta información siempre está disponible mientras el `Microsoft.ChangeAnalysis` proveedor de recursos esté incorporada a la suscripción de Azure.

### <a name="web-application-deployment-and-configuration-changes"></a>Cambios de configuración e implementación de aplicación Web
Herramienta de análisis de cambios captura el estado de implementación y configuración de una aplicación cada 4 horas para calcular las diferencias y presentar lo que ha cambiado. Ejemplos de estos cambios incluyen los cambios de variables de entorno de aplicación, los cambios de regla de configuración de IP, los cambios de identidad de servicio administrada, los cambios de configuración de SSL y así sucesivamente.
A diferencia de los cambios en el Administrador de recursos, este tipo de información de los cambios no estén disponible inmediatamente en la herramienta. Para ver los cambios más recientes, use el botón 'Examen cambia ahora' en la herramienta.

![Captura de pantalla de exploración de los cambios ahora botón diagnosticar y resolver la herramienta de problemas con la integración del análisis de cambio para app service web Apps](./media/change-analysis/scan-changes.png)

### <a name="dependency-changes"></a>Cambios de dependencia
Recursos de las dependencias también podrían ser la causa de problemas. Por ejemplo, si se llama una aplicación web en una caché en Redis, puede verse afectado el rendimiento de la aplicación web por SKU de la memoria caché de Redis. Buscando en el registro DNS de web app cambiar analysis service también presentan las dependencias de información de los cambios para identificar cambios en todos los componentes de una aplicación que puede haber causado problemas.


## <a name="change-analysis-service-for-app-services-web-app"></a>Servicio de análisis de cambio para la aplicación de servicios Web

Análisis de cambio de aplicación de Azure Monitor se integran actualmente en el autoservicio **diagnosticar y resolver problemas** experimentar, que se puede acceder desde el **Introducción** sección de Azure App Service aplicación:

![Información general de la captura de pantalla de Azure App Service con cuadros rojos alrededor del botón información general de la página y diagnosticar y resolver el botón de problemas](./media/change-analysis/change-analysis.png)

### <a name="enable-change-analysis-in-diagnose-and-solve-problems-tool"></a>Habilitar el análisis de cambio de diagnosticar y resolver la herramienta de problemas

1. Seleccione **disponibilidad y rendimiento**

    ![captura de pantalla de opciones de solución de problemas de rendimiento y disponibilidad](./media/change-analysis/availability-and-performance.png)

2. Haga clic en el **aplicación se bloquea** icono.

   ![Captura de pantalla con el icono de bloqueos de aplicación](./media/change-analysis/application-crashes-tile.png)

3. Para habilitar **análisis cambios** seleccione **habilitar ahora**.

   ![captura de pantalla de opciones de solución de problemas de rendimiento y disponibilidad](./media/change-analysis/application-crashes.png)

4. Para aprovechar las ventajas del perfil de cambiar el conjunto de funcionalidad de análisis **cambiar análisis**, **buscar cambios de código de**, y **siempre** a **en** y seleccione **guardar**.

    ![Captura de pantalla de la interfaz de usuario de análisis de Azure App Service Habilitar cambio](./media/change-analysis/change-analysis-on.png)

    Si **análisis cambios** está habilitada, podrá detectar los cambios de nivel de recurso. Si **buscar cambios de código de** está habilitado, verá también archivos de implementación y los cambios de configuración. Habilitar **siempre** optimizará el rendimiento del examen de cambio, pero puede incurrir en costes adicionales desde la perspectiva de facturación.

5.  Una vez que todo está habilitada, seleccione **diagnosticar y resolver problemas** > **disponibilidad y rendimiento** > **aplicación se bloquea** le permitirá tener acceso a la experiencia de análisis de cambio. El gráfico mostrará un resumen del tipo de cambios que ha ocurrido con el tiempo, junto con detalles sobre dichos cambios:

     ![Captura de pantalla de vista de diferencias de cambio](./media/change-analysis/change-view.png)


### <a name="enable-change-analysis-service-at-scale"></a>Habilitar el servicio de análisis de cambio a escala
Si tiene una gran cantidad de aplicaciones web en su suscripción, habilitar el servicio en cada nivel de la aplicación web será ineficaz. Estas son algunas instrucciones de incorporación alternativo.

#### <a name="registering-change-analysis-resource-provider-for-your-subscription"></a>Registrar proveedor de recursos de análisis de cambio de su suscripción

1. Registrar la marca de características de vista previa de análisis de cambios

    Puesto que esta característica está en versión preliminar, deberá registrar en primer lugar la marca de característica para que sea visible para la suscripción.
    - Abra [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/).

    ![Captura de pantalla de cambio de Azure Cloud Shell](./media/change-analysis/cloud-shell.png)

    - Cambie el tipo de shell para PowerShell:

    ![Captura de pantalla de cambio de Azure Cloud Shell](./media/change-analysis/choose-powershell.png)

    - Ejecute el siguiente comando de PowerShell:

    ``` PowerShell

    Set-AzContext -Subscription <your_subscription_id> #set script execution context to the subscription you are trying to onboard
    Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.ChangeAnalysis" -ListAvailable #Check for feature flag availability
    Register-AzureRmProviderFeature -FeatureName PreviewAccess -ProviderNamespace Microsoft.ChangeAnalysis #Register feature flag

    ```

2. Registrar proveedor de recursos de análisis de cambio de la suscripción

    - Vaya a las suscripciones, seleccione la suscripción que desea incorporar el servicio de cambio y luego haga clic en proveedores de recursos:

        ![Captura de pantalla para el registro de RP de análisis de cambio de la hoja de suscripciones](./media/change-analysis/register-rp.png)

    - Seleccione *Microsoft.ChangeAnalysis* y haga clic en *registrar* en la parte superior de la página.

    - Una vez que el proveedor de recursos está incorporada, siga las instrucciones del *no se puede obtener la información de análisis cambiar* a continuación para establecer la etiqueta oculta en la aplicación web para habilitar la implementación de nivel de detección de cambios en la aplicación web.

3. Como alternativa al paso 2 anterior, puede registrarse para el proveedor de recursos mediante un script de PowerShell:

    ```PowerShell
    Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState #Check if RP is ready for registration

    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.ChangeAnalysis" #Register the Change Analysis RP
    ```

4. Para establecer una etiqueta oculta en una aplicación web con PowerShell, ejecute el siguiente comando:

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
