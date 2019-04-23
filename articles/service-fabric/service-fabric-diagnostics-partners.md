---
title: Asociados de supervisión de Azure Service Fabric | Microsoft Docs
description: Obtenga información sobre cómo supervisar Azure Service Fabric con soluciones de supervisión de asociados
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/16/2018
ms.author: srrengar
ms.openlocfilehash: c2f953c98e41291951f07556bd0cd441d2793d1d
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59796255"
---
# <a name="azure-service-fabric-monitoring-partners"></a>Asociados de supervisión de Azure Service Fabric

En este artículo se muestra cómo uno mismo puede supervisar sus aplicaciones, clústeres e infraestructura de Service Fabric con una serie de soluciones de asociados. Hemos colaborado con cada uno de los asociados siguientes para crear ofertas integradas para Service Fabric.

## <a name="dynatrace"></a>Dynatrace

La integración con Dynatrace proporciona muchas características incorporadas para supervisar los clústeres de Service Fabric. La instalación de Dynatrace OneAgent en las instancias de VMSS proporciona contadores de rendimiento y una topología de la implementación de Service Fabric hasta el nivel de aplicación. Dynatrace también es una excelente opción para la supervisión local. Consulte más información de las características enumeradas en la [anuncio](https://www.dynatrace.com/news/blog/automatic-end-to-end-service-fabric-monitoring-with-dynatrace/) y [instrucciones](https://www.dynatrace.com/news/blog/automatic-end-to-end-service-fabric-monitoring-with-dynatrace/) para habilitar Dynatrace en el clúster. 

## <a name="datadog"></a>Datadog

Datadog tiene una extensión para VMSS para instancias de Windows y Linux. Con Datadog puede recopilar registros de eventos de Windows y, por tanto, recopilar eventos de la plataforma Service Fabric en Windows. Vea las instrucciones sobre cómo enviar los datos de diagnóstico a Datadog [aquí](https://www.datadoghq.com/blog/azure-monitoring-enhancements/#integrate-with-azure-service-fabric).

## <a name="appdynamics"></a>AppDynamics

La integración de Service Fabric con AppDynamics está en el nivel de aplicación. Al actualizar las variables de entorno y usar paquetes NuGet de AppDynamics, puede enviar telemetría de aplicaciones a AppDynamics. Vea estas [instrucciones](https://docs.appdynamics.com/display/AZURE/Install+AppDynamics+for+Azure+Service+Fabric) para obtener información sobre cómo integrar las aplicaciones .NET de Service Fabric con AppDynamics.

## <a name="new-relic"></a>New Relic

New Relic es otra herramienta de administración del rendimiento de las aplicaciones que se integra bien con aplicaciones de Service Fabric. Puede instalar los paquetes NuGet de New Relic y agregar variables de entorno concretas a los archivos de manifiesto para enviar telemetría de la aplicación a New Relic. Vea estas [instrucciones](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/install-net-agent-azure-service-fabric) para habilitar la telemetría de New Relic para las aplicaciones .NET de Service Fabric.

## <a name="elk"></a>ELK 

La pila de ELK es una colección de tecnologías de código abierto: Elasticsearch, Logstash y Kibana. Si se usan en combinación, es posible recopilar, almacenar y analizar los datos de supervisión y diagnóstico de Service Fabric. Hay un tutorial sobre cómo hacerlo con aplicaciones nativas de Java de Service Fabric [aquí](service-fabric-tutorial-java-elk.md). 

## <a name="humio"></a>Humio

Humio es un servicio de recopilación de registros que puede recopilar registros de las aplicaciones y los eventos de Service Fabric en la nube o local en tiempo real. Además de observación en vivo, Humio ofrece capacidades de análisis y visualización de vanguardia para ver y recopilar información de diagnóstico del. Humio tiene planes de precios más rentables y se compila a escala mientras se mantienen iluminen velocidad rápida. Se integra directamente con los eventos de la plataforma Service Fabric y la telemetría de aplicación. Puede leer más sobre la integración de Service Fabric Humio [aquí](https://github.com/humio/service-fabric-humio).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga [información general sobre la supervisión y el diagnóstico](service-fabric-diagnostics-overview.md) en Azure Service Fabric
* Aprenda a [diagnosticar escenarios comunes](service-fabric-diagnostics-common-scenarios.md) con nuestras herramientas de primera entidad
