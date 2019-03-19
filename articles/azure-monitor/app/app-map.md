---
title: Mapa de aplicación en Azure Application Insights | Microsoft Docs
description: Supervisión de topologías de aplicaciones complejas con el mapa de aplicación
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 3bf37fe9-70d7-4229-98d6-4f624d256c36
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/14/2019
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: 777bf38fbc8a90335af79bbd0fb2ce74310bd991
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58006797"
---
# <a name="application-map-triage-distributed-applications"></a>Mapa de aplicación: Evaluación de prioridades de las aplicaciones distribuidas

El mapa de aplicación le ayuda a detectar los cuellos de botella en el rendimiento o las zonas activas con error en todos los componentes de la aplicación distribuida. Cada nodo del mapa representa un componente de aplicación o sus dependencias, e incluye el KPI de mantenimiento y el estado de alerta. Puede hacer clic a través de cualquier componente para ver un diagnóstico más detallado, como los eventos de Application Insights. Si su aplicación usa los servicios de Azure, también puede hacer clic por los diagnósticos de Azure, como las recomendaciones SQL Database Advisor.

## <a name="what-is-a-component"></a>¿Qué es un componente?

Los componentes son partes que se pueden implementar independientemente de su aplicación de microservicios o distribuida. Los equipos de operaciones y los desarrolladores pueden ver el código o acceder a la telemetría que generan estos componentes de la aplicación. 

* Los componentes son diferentes de las dependencias externas "observadas", como SQL, EventHub, etc. a las que su equipo u organización no pueden acceder (código o telemetría).
* Los componentes se ejecutan en cualquier número de instancias de rol, servidor o contenedor.
* Los componentes pueden ser claves de instrumentación de Application Insights independientes (incluso aunque las suscripciones sean diferentes) o diferentes roles que informan a una única clave de instrumentación de Application Insights. La experiencia de mapa de versión preliminar muestra los componentes con independencia de cómo están configurados.

## <a name="composite-application-map"></a>Mapa de aplicación compuesta

Puede ver la topología de aplicación completa a lo largo de varios niveles de componentes de aplicación relacionados. Los componentes podrían ser diferentes recursos de Application Insights o distintos roles de un único recurso. Para encontrar componentes, el mapa de aplicación sigue las llamadas de dependencia HTTP entre los servidores con el SDK de Application Insights instalado. 

Esta experiencia comienza con la detección progresiva de los componentes. La primera vez que carga el mapa de aplicación, se desencadena un conjunto de consultas para detectar los componentes relacionados con este componente. Un botón en la esquina superior izquierda se actualiza con el número de componentes de la aplicación a medida que se detectan. 

Al hacer clic en "Update map components" (Actualizar componentes del mapa), el mapa se actualiza con todos los componentes detectados hasta ese momento. Según la complejidad de la aplicación, esta operación puede tardar un minuto en cargarse.

Si todos los componentes son roles dentro de un único recurso de Application Insights, este paso de detección no es necesario. La carga inicial para este tipo de aplicación tendrá todos sus componentes.

![Captura de pantalla de mapa de aplicación](media/app-map/app-map-001.png)

Uno de los objetivos principales de la experiencia es poder visualizar topologías complejas con cientos de componentes.

Haga clic en cada componente para ver información detallada relacionada e ir a la experiencia de evaluación de prioridades de rendimiento y errores de ese componente.

![Control flotante](media/app-map/application-map-002.png)

### <a name="investigate-failures"></a>Investigar los errores

Seleccione **Investigar los errores** para iniciar el panel de errores.

![Captura de pantalla del botón Investigar los errores](media/app-map/investigate-failures.png)

![Captura de pantalla de la experiencia de errores](media/app-map/failures.png)

### <a name="investigate-performance"></a>Investigar el rendimiento

Para solucionar problemas de rendimiento haga clic en **Investigar el rendimiento**.

![Captura de pantalla del botón Investigar el rendimiento](media/app-map/investigate-performance.png)

![Captura de pantalla de la experiencia de rendimiento](media/app-map/performance.png)

### <a name="go-to-details"></a>Ir a los detalles

Haga clic en **Ir a los detalles** para explorar una experiencia de transacción completa, que puede ofrecer las vistas realizadas en el nivel de la pila de llamadas.

![Captura de pantalla del botón Ir a los detalles](media/app-map/go-to-details.png)

![Captura de pantalla de los detalles de una transacción completa](media/app-map/end-to-end-transaction.png)

### <a name="view-in-analytics"></a>Ver en Analytics

Para consultar e investigar aún más los datos de aplicaciones, haga clic en **Ver en Analytics**.

![Captura de pantalla del botón Ver en Analytics](media/app-map/view-in-analytics.png)

![Captura de pantalla de la experiencia con Analytics](media/app-map/analytics.png)

### <a name="alerts"></a>Alertas

Para ver las alertas activas y las reglas subyacentes que provocan el desencadenamiento de estas, haga clic en **Alertas**.

![Captura de pantalla del botón de alertas](media/app-map/alerts.png)

![Captura de pantalla de la experiencia con Analytics](media/app-map/alerts-view.png)

## <a name="set-cloudrolename"></a>Establecimiento de cloud_RoleName

El mapa de la aplicación usa la propiedad `cloud_RoleName` para identificar los componentes en el mapa. El SDK de Application Insights agrega de forma automática la propiedad `cloud_RoleName` a los componentes emitidos por telemetría. Por ejemplo, el SDK agregará un nombre de sitio web o un nombre de rol de servicio a la propiedad `cloud_RoleName`. Pero hay casos en los que le interesará reemplazar el valor predeterminado. Para reemplazar cloud_RoleName y cambiar lo que se muestra en el mapa de aplicación:

### <a name="net"></a>.NET

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace CustomInitializer.Telemetry
{
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize(ITelemetry telemetry)
        {
            if (string.IsNullOrEmpty(telemetry.Context.Cloud.RoleName))
            {
                //set custom role name here
                telemetry.Context.Cloud.RoleName = "RoleName";
            }
        }
    }
}
```

**Cargue su inicializador**

En ApplicationInsights.config:

```xml
    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="CustomInitializer.Telemetry.MyTelemetryInitializer, CustomInitializer"/>
        ...
      </TelemetryInitializers>
    </ApplicationInsights>
```

Un método alternativo consiste en crear una instancia del inicializador en el código, por ejemplo en Global.aspx.cs:

```csharp
 using Microsoft.ApplicationInsights.Extensibility;
 using CustomInitializer.Telemetry;

    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers.Add(new MyTelemetryInitializer());
    }
```

### <a name="nodejs"></a>Node.js

```javascript
var appInsights = require("applicationinsights");
appInsights.setup('INSTRUMENTATION_KEY').start();
appInsights.defaultClient.context.tags["ai.cloud.role"] = "your role name";
appInsights.defaultClient.context.tags["ai.cloud.roleInstance"] = "your role instance";
```

### <a name="alternate-method-for-nodejs"></a>Método alternativo para Node.js

```javascript
var appInsights = require("applicationinsights");
appInsights.setup('INSTRUMENTATION_KEY').start();

appInsights.defaultClient.addTelemetryProcessor(envelope => {
    envelope.tags["ai.cloud.role"] = "your role name";
    envelope.tags["ai.cloud.roleInstance"] = "your role instance"
});
```

### <a name="java"></a>Java

Si usa Spring Boot con el iniciador de Spring Boot de Application Insights, el único cambio necesario es establecer el nombre personalizado para la aplicación en el archivo application.properties.

`spring.application.name=<name-of-app>`

El iniciador de Spring Boot asignará de forma automática cloudRoleName al valor proporcionado para la propiedad spring.application.name.

Para obtener más información sobre la correlación de Java y cómo configurar cloudRoleName para aplicaciones que no sean de SpringBoot, consulte esta [sección](https://docs.microsoft.com/azure/application-insights/application-insights-correlation#role-name) sobre la correlación.

### <a name="clientbrowser-side-javascript"></a>JavaScript del lado cliente o explorador

```javascript
appInsights.queue.push(() => {
appInsights.context.addTelemetryInitializer((envelope) => {
  envelope.tags["ai.cloud.role"] = "your role name";
  envelope.tags["ai.cloud.roleInstance"] = "your role instance";
});
});
```

Para obtener más información sobre cómo invalidar la propiedad cloud_RoleName con inicializadores de telemetría, vea [Agregar propiedades: ITelemetryInitializer](api-filtering-sampling.md#add-properties-itelemetryinitializer).

## <a name="troubleshooting"></a>solución de problemas

Si tiene dificultades para conseguir que el mapa de aplicación funcione según lo esperado, siga estos pasos:

1. Asegúrese de que usa un SDK oficialmente compatible. Es posible que los SDK no compatibles o de la comunidad no admitan la correlación.

    Consulte este [artículo](https://docs.microsoft.com/azure/application-insights/app-insights-platforms) para obtener una lista de los SDK compatibles.

2. Actualice todos los componentes a la versión más reciente del SDK.

3. Si usa Azure Functions con C#, actualice a [Functions V2](https://docs.microsoft.com/azure/azure-functions/functions-versions).

4. Confirme que [cloud_RoleName](app-map.md#set-cloudrolename) está configurado correctamente.

5. Si falta una dependencia, asegúrese de que se encuentra en la lista de [dependencias recopiladas automáticamente](https://docs.microsoft.com/azure/application-insights/auto-collect-dependencies). De lo contrario, todavía puede realizar su seguimiento de forma manual con una [llamada de seguimiento de dependencia](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackdependency).

## <a name="portal-feedback"></a>Comentarios del portal
Para proporcionar comentarios, use la opción de comentarios.

![Imagen MapLink-1](./media/app-map/14-updated.png)

## <a name="next-steps"></a>Pasos siguientes

* [Descripción de la correlación](https://docs.microsoft.com/azure/application-insights/application-insights-correlation)
