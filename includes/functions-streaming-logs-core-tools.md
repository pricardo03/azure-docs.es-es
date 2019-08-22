---
author: ggailey777
ms.author: glenga
ms.date: 7/24/2019
ms.topic: include
ms.service: azure-functions
ms.openlocfilehash: 1928a8238cd73087e3c199675574dd1395f4d76d
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68881316"
---
#### <a name="built-in-log-streaming"></a>Streaming integrado de registros

Use la opción `logstream` para empezar a recibir registros de streaming de una aplicación de funciones específica que se ejecuta en Azure, como en el ejemplo siguiente:

```bash
func azure functionapp logstream <FunctionAppName>
```

#### <a name="live-metrics-stream"></a>Secuencia de métricas en directo

También puede ver [Live Metrics Stream](../articles/azure-monitor/app/live-stream.md) para la aplicación de funciones en una ventana nueva del explorador si incluye la opción `--browser`, tal como en el ejemplo siguiente:

```bash
func azure functionapp logstream <FunctionAppName> --browser
```
