---
title: Resistencia y recuperación ante desastres de Azure App Configuration | Microsoft Docs
description: Información general sobre cómo implementar la resistencia y la recuperación ante desastres con App Configuration.
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.service: azure-app-configuration
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 05/29/2019
ms.author: yegu
ms.openlocfilehash: 39e7a62899a7d1d6feb5bfd3b45ad4adc3c996a0
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66394074"
---
# <a name="resiliency-and-disaster-recovery"></a>Resistencia y recuperación ante desastres

Actualmente Azure App Configuration es un servicio regional. Cada almacén de configuración se crea en una región de Azure determinada. Una interrupción en toda la región afectará a todos los almacenes de esa región. App Configuration no ofrece conmutación por error automática en otra región. En este artículo se proporcionan instrucciones generales sobre cómo puede usar varios almacenes de configuración entre regiones de Azure para aumentar la resistencia geográfica de la aplicación.

## <a name="high-availability-architecture"></a>Arquitectura de alta disponibilidad

Para alcanzar la redundancia entre regiones, debe crear varios almacenes de configuración de aplicaciones en distintas regiones. Con esta configuración, la aplicación tendrá al menos un almacén de configuración adicional al que recurrir cuando el almacén principal deje de estar accesible. A continuación hay un diagrama que ilustra la topología entre la aplicación y sus almacenes de configuración principal y secundario.

![Almacenes con redundancia geográfica.](./media/geo-redundant-app-configuration-stores.png)

La aplicación cargará su configuración desde los almacenes principal y secundario en paralelo. Con ello, se aumenta la posibilidad de obtener correctamente los datos de configuración de modo significativo. Usted es responsable de mantener sincronizados los datos de ambos. En las siguientes secciones se explica cómo puede crear resistencia geográfica en la aplicación.

## <a name="failover-between-configuration-stores"></a>Conmutación por error entre los almacenes de configuración

Técnicamente, la aplicación no va a ejecutar una conmutación por error. Va a intentar recuperar el mismo conjunto de datos de configuración de dos almacenes de configuración de aplicaciones al mismo tiempo. El código se debe organizar de modo que se cargue primero desde el almacén secundario y, luego, desde el almacén principal. Este enfoque garantiza que los datos de configuración del almacén principal tengan prioridad siempre que estén disponibles. En el siguiente fragmento de código se muestra cómo implementar esto en .NET Core.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            var settings = config.Build();
            config.AddAzureAppConfiguration(settings["ConnectionString_SecondaryStore"], optional: true)
                  .AddAzureAppConfiguration(settings["ConnectionString_PrimaryStore"], optional: true);
        })
        .UseStartup<Startup>();
    }
```

Observe el parámetro `optional` pasado a la función `AddAzureAppConfiguration`. Cuando se establece en `true`, este parámetro evitará que la aplicación se detenga si la función no puede cargar los datos de configuración.

## <a name="synchronization-between-configuration-stores"></a>Sincronización entre los almacenes de configuración

Es importante que los almacenes de configuración con redundancia geográfica tengan todos el mismo conjunto de datos. Puede usar la función **Exportar** de App Configuration para copiar datos del almacén principal al secundario a petición. Esta función está disponible mediante Azure Portal y la CLI.

En Azure Portal, puede insertar un cambio en otro almacén de configuración mediante estos pasos:

1. Vaya a la pestaña **Import/Export**, seleccione **Exportar**, seleccione **App Configuration** como el servicio de **destino** y haga clic en **Seleccionar un recurso**.

2. En la nueva hoja que se ha abierto, especifique la suscripción, el grupo de recursos, el nombre del recurso del almacén secundario y, luego, haga clic en **Aplicar**.

3. La interfaz de usuario se actualizará para que pueda elegir qué datos de configuración quiere exportar al almacén secundario. Puede dejar el valor de tiempo predeterminada tal cual y establecer **De la etiqueta** y **Para etiquetar** en el mismo valor. Haga clic en **Aplicar**.

4. Repita los pasos anteriores con todos los cambios de configuración.

Este proceso de exportación se puede automatizar con la CLI de Azure. El comando siguiente muestra cómo exportar un único cambio de configuración del almacén principal al secundario.

    az appconfig kv export --destination appconfig --name {PrimaryStore} --label {Label} --dest-name {SecondaryStore} --dest-label {Label}

## <a name="next-steps"></a>Pasos siguientes

En este artículo, aprendió a mejorar la aplicación para lograr resistencia geográfica en tiempo de ejecución para App Configuration. Como alternativa, puede insertar datos de configuración de App Configuration en tiempo de compilación o implementación. Para más información, consulte [Integración con una canalización de CI/CD](./integrate-ci-cd-pipeline.md).

