---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 03/25/2019
ms.openlocfilehash: 94e95864d8bac2d6dc0ff690a2a8f53bd2db5a40
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58522528"
---
## <a name="validate-container-is-running"></a>Validar contenedor se está ejecutando 

Hay varias maneras de validar el contenedor se está ejecutando: 

|Solicitud|Propósito|
|--|--|
|`http://localhost:5000/`|El contenedor proporciona una página principal.|
|`http://localhost:5000/status`|Se solicitó con GET validar el contenedor se está ejecutando sin causar una consulta de punto de conexión. Esto puede usarse para Kubernetes [sondeos de preparación y ejecución](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/).|
|`http://localhost:5000/swagger`|El contenedor cuenta con un completo conjunto de documentación sobre los puntos de conexión, así como una característica `Try it now`. Esta característica le permite especificar la configuración en un formulario HTML basado en Web y realizar la consulta sin necesidad de escribir código. Una vez que la consulta devuelve resultados, se proporciona un ejemplo del comando CURL para mostrar los encabezados HTTP y el formato de cuerpo requeridos. |

![Página principal del contenedor](./media/cognitive-services-containers-api-documentation/container-webpage.png)
