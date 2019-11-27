---
title: Consulta del punto de conexión del contenedor Conversión de voz a texto
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 11/15/2019
ms.author: dapine
ms.openlocfilehash: 2d96385f2d2d34d161739c55228220cf28871048
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2019
ms.locfileid: "74132594"
---
### <a name="speech-to-text-or-custom-speech-to-text"></a>Conversión de voz a texto o Conversión de voz a texto personalizada

El contenedor proporciona las API de punto de conexión de consulta basadas en WebSocket, a las que se accede mediante el [SDK de Voz](../index.md). De forma predeterminada, el SDK de Voz usa servicios de voz en línea. Para usar el contenedor, deberá cambiar el método de inicialización.

> [!TIP]
> Al usar el SDK de Voz con los contenedores, no es necesario proporcionar la [clave de suscripción del recurso de Voz de Azure o un token de portador de autenticación](../rest-speech-to-text.md#authentication).

Consulte los ejemplos siguientes.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Cambie de usar esta llamada de inicialización en la nube de Azure:

```csharp
var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

por esta llamada mediante el [host](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.fromhost?view=azure-dotnet) de contenedor:

```csharp
var config = SpeechConfig.FromHost(
    new Uri("ws://localhost:5000"));
```
# <a name="pythontabpython"></a>[Python](#tab/python)

Cambie de usar esta llamada de inicialización en la nube de Azure:

```python
speech_config = speechsdk.SpeechConfig(
    subscription=speech_key, region=service_region)
```

por esta llamada mediante el [host](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python) de contenedor:

```python
speech_config = speechsdk.SpeechConfig(
    host="ws://localhost:5000")
```

***
