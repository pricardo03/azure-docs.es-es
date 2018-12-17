---
title: Escritura de código para realizar el seguimiento de solicitudes con Azure Application Insights | Microsoft Docs
description: Escribir código para realizar un seguimiento de solicitudes con Application Insights para que pueda obtener perfiles para sus solicitudes
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: 20f408d9dd32c3fd7a0e319e4051483e3aa54dd9
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2018
ms.locfileid: "52722129"
---
# <a name="write-code-to-track-requests-with-application-insights"></a>Escritura de código para realizar el seguimiento de solicitudes con Azure Application Insights

Para ver los perfiles de la aplicación en la página Rendimiento, Application Insights tiene que realizar un seguimiento de las solicitudes para la aplicación. Application Insights puede realizar automáticamente un seguimiento de solicitudes para las aplicaciones que se basan en marcos de trabajo que ya estén instrumentados, como ASP.net y ASP.Net Core. Pero para otras aplicaciones, como los roles de Azure Cloud Service y las API sin estado de Service Fabric, es necesario escribir código para indicar a Application Insights dónde empiezan y terminan las solicitudes. Una vez que ha escrito este código, se enviará telemetría de las solicitudes a Application Insights, podrá ver los datos de telemetría en la página Rendimiento y se recopilarán perfiles para dichas solicitudes. 

Estos son los pasos que tiene que seguir para realizar un seguimiento manual de solicitudes:


  1. Agregue el código siguiente al principio de la duración de la aplicación:  

        ```csharp
        using Microsoft.ApplicationInsights.Extensibility;
        ...
        // Replace with your own Application Insights instrumentation key.
        TelemetryConfiguration.Active.InstrumentationKey = "00000000-0000-0000-0000-000000000000";
        ```
      Para más información acerca de la configuración de esta clave de instrumentación global, consulte [Using Service Fabric with Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md) (Uso de Service Fabric con Application Insights).  

  1. Con respecto a cualquier parte del código que desee instrumentar, agregue una instrucción `StartOperation<RequestTelemetry>` **USING** a su alrededor, como se muestra en el ejemplo siguiente:

        ```csharp
        using Microsoft.ApplicationInsights;
        using Microsoft.ApplicationInsights.DataContracts;
        ...
        var client = new TelemetryClient();
        ...
        using (var operation = client.StartOperation<RequestTelemetry>("Insert_Your_Custom_Event_Unique_Name"))
        {
          // ... Code I want to profile.
        }
        ```

        No se permite llamar a `StartOperation<RequestTelemetry>` dentro de otro ámbito de `StartOperation<RequestTelemetry>`. En su lugar, puede usar `StartOperation<DependencyTelemetry>` en el ámbito anidado. Por ejemplo:   
        
        ```csharp
        using (var getDetailsOperation = client.StartOperation<RequestTelemetry>("GetProductDetails"))
        {
        try
        {
          ProductDetail details = new ProductDetail() { Id = productId };
          getDetailsOperation.Telemetry.Properties["ProductId"] = productId.ToString();
        
          // By using DependencyTelemetry, 'GetProductPrice' is correctly linked as part of the 'GetProductDetails' request.
          using (var getPriceOperation = client.StartOperation<DependencyTelemetry>("GetProductPrice"))
          {
              double price = await _priceDataBase.GetAsync(productId);
              if (IsTooCheap(price))
              {
                  throw new PriceTooLowException(productId);
              }
              details.Price = price;
          }
        
          // Similarly, note how 'GetProductReviews' doesn't establish another RequestTelemetry.
          using (var getReviewsOperation = client.StartOperation<DependencyTelemetry>("GetProductReviews"))
          {
              details.Reviews = await _reviewDataBase.GetAsync(productId);
          }
        
          getDetailsOperation.Telemetry.Success = true;
          return details;
        }
        catch(Exception ex)
        {
          getDetailsOperation.Telemetry.Success = false;
        
          // This exception gets linked to the 'GetProductDetails' request telemetry.
          client.TrackException(ex);
          throw;
        }
        }
        ```
