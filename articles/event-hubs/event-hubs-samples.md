---
title: Ejemplos de Azure Event Hubs | Microsoft Docs
description: En este artículo se proporciona una lista de los ejemplos de Azure Event Hubs que se encuentran en GitHub.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.custom: seodec18
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 1c1198733fb56303d328ee97152442d25dbe945a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60343484"
---
# <a name="git-repositories-with-samples-for-azure-event-hubs"></a>Repositorios GIT con ejemplos para Azure Event Hubs 
Puede encontrar ejemplos de Event Hubs en [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples). Estos ejemplos muestran características clave de [Azure Event Hubs](/azure/event-hubs/). En este artículo se categorizan y describen los ejemplos disponibles, con vínculos a cada uno.

## <a name="net-samples"></a>Ejemplos de .NET

| Nombre del ejemplo | DESCRIPCIÓN | 
| ----------- | ----------- | 
| [SampleSender](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) | En este ejemplo se muestra cómo escribir una aplicación de consola de .NET Core que envía un conjunto de eventos a un centro de eventos. |
| [SampleEHReceiver](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) | En este ejemplo se muestra cómo escribir una aplicación de consola de .NET Core que recibe un conjunto de eventos de un centro de eventos mediante la biblioteca de host de procesador de eventos.  | 

## <a name="java-samples"></a>Ejemplos de Java

| Nombre del ejemplo | DESCRIPCIÓN | 
| ----------- | ----------- | 
| [SendBatch](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SendBatch)  | En este ejemplo se muestra cómo ingerir lotes de eventos en su centro de eventos. | 
| [SimpleSend](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SimpleSend) | En este ejemplo se muestra cómo ingerir eventos en su centro de eventos. |
| [AdvanceSendOptions](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/AdvancedSendOptions) | En este ejemplo se muestran las diversas opciones disponibles con Event Hubs para ingerir eventos. |
| [ReceiveByDateTime](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/ReceiveByDateTime) | En este ejemplo se muestra cómo recibir eventos desde una partición de un centro de eventos mediante un desplazamiento de fecha y hora específico. |
| [ReceiveUsingOffset](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/ReceiveUsingOffset) | En este ejemplo se muestra cómo recibir eventos desde una partición de un centro de eventos mediante un desplazamiento de datos específico. |  
| [ReceiveUsingSequenceNumber](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/ReceiveUsingSequenceNumber) | En este ejemplo se muestra cómo puede recibir eventos desde particiones de un centro de eventos mediante un número de secuencia. |   
| [EventProcessorSample](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/EventProcessorSample) |En este ejemplo se muestra cómo recibir eventos desde un centro de eventos mediante el host del procesador de eventos, que proporciona la selección automática de la partición y la conmutación por error entre los diversos receptores simultáneos. | 
| [AutoScaleOnIngress](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Benchmarks/AutoScaleOnIngress) | En este ejemplo se muestra cómo un centro de eventos puede escalarse verticalmente de forma automática con cargas elevadas. En el ejemplo se envían eventos a una velocidad que simplemente supera la velocidad configurada de un centro de eventos, lo cual provoca que este se escale verticalmente. | 
| [IngressBenchmark](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Benchmarks/IngressBenchmark) | Este ejemplo permite medir la velocidad de entrada. | 

## <a name="python-samples"></a>Ejemplos de Python
Puede encontrar ejemplos de Python para Azure Event Hubs en el repositorio de GitHub [azure-event-hubs-python](https://github.com/Azure/azure-event-hubs-python/tree/master/examples).

## <a name="nodejs-samples"></a>Ejemplos de Node.js
Puede encontrar ejemplos de Node.js para Azure Event Hubs en el repositorio de GitHub [azure-sdk-for-js](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs/samples).

## <a name="go-samples"></a>Ejemplos de Go
Puede encontrar ejemplos de Go para Azure Event Hubs en el repositorio de GitHub [azure-event-hubs-go](https://github.com/Azure/azure-event-hubs-go/tree/master/_examples).

## <a name="azure-cli-samples"></a>Ejemplos de la CLI de Azure
Puede encontrar ejemplos de la CLI de Azure para Azure Event Hubs en el repositorio de GitHub [azure-event-hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples/Management/CLI).

## <a name="azure-powershell-samples"></a>Ejemplos de Azure PowerShell
Puede encontrar ejemplos de Azure PowerShell para Azure Event Hubs en el repositorio de GitHub [azure-event-hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples/Management/PowerShell).
 
## <a name="apache-kafka-samples"></a>Ejemplos de Apache Kafka
Puede encontrar ejemplos de Event Hubs para la característica de Apache Kafka en el repositorio [azure-event-hubs-for-kafka](https://github.com/Azure/azure-event-hubs-for-kafka) de GitHub.

## <a name="next-steps"></a>Pasos siguientes
Si desea conocer más información acerca de Event Hubs, consulte los siguientes artículos:

- [Información general de Event Hubs](event-hubs-what-is-event-hubs.md)
- [Características de Event Hubs](event-hubs-features.md)
- [Preguntas más frecuentes sobre Event Hubs](event-hubs-faq.md)