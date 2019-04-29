---
title: Escritura de código para realizar el seguimiento de solicitudes con Azure Application Insights | Microsoft Docs
description: Escriba código para realizar un seguimiento de solicitudes con Application Insights para que pueda obtener perfiles para sus solicitudes.
services: application-insights
documentationcenter: ''
author: cweining
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 08/06/2018
ms.author: cweining
ms.openlocfilehash: 4782e560b580b7f565724dbb35ed9876bffdc256
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60730861"
---
# <a name="write-code-to-track-requests-with-application-insights"></a>Escritura de código para realizar el seguimiento de solicitudes con Application Insights

Para ver los perfiles de la aplicación en la página Rendimiento, Azure Application Insights tiene que realizar un seguimiento de las solicitudes para la aplicación. Application Insights puede realizar automáticamente un seguimiento de solicitudes para las aplicaciones que se basan en marcos de trabajo ya instrumentados. Dos ejemplos son ASP.NET y ASP.NET Core. 

Pero para otras aplicaciones, como los roles de trabajo de Azure Cloud Services y las API sin estado de Service Fabric, es necesario escribir código para indicar a Application Insights dónde empiezan y terminan las solicitudes. Una vez escrito este código, la telemetría de solicitudes se envía a Application Insights. Puede ver la telemetría en la página Rendimiento y los perfiles se recopilan para dichas solicitudes. 

Para realizar un seguimiento de las solicitudes manualmente, haga lo siguiente:

  1. Agregue el código siguiente al principio de la duración de la aplicación:  

        ```csharp
        using Microsoft.ApplicationInsights.Extensibility;
        ...
        // Replace with your own Application Insights instrumentation key.
        TelemetryConfiguration.Active.InstrumentationKey = "00000000-0000-0000-0000-000000000000";
        ```
      Para más información acerca de la configuración de esta clave de instrumentación global, consulte [Using Service Fabric with Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md) (Uso de Service Fabric con Application Insights).  

  1. Con respecto a cualquier parte del código que quiera instrumentar, agregue una instrucción `StartOperation<RequestTelemetry>` **USING** a su alrededor, como se muestra en el ejemplo siguiente:

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
