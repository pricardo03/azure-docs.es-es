---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 00cc63f53388ab7bea05a0b55784247f63477684
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67704306"
---
## <a name="validate-that-a-container-is-running"></a>Comprobación de que un contenedor está en ejecución 

Hay varias maneras de comprobar que el contenedor está en ejecución. 

|Solicitud|Propósito|
|--|--|
|`http://localhost:5000/`|El contenedor ofrece una página principal.|
|`http://localhost:5000/status`|Se solicitó con GET, para comprobar que el contenedor está en ejecución sin causar una consulta al punto de conexión. Esta solicitud se puede usar con los [sondeos de ejecución y preparación](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) de Kubernetes.|
|`http://localhost:5000/swagger`|El contenedor cuenta con un completo conjunto de documentación sobre los puntos de conexión y con una característica `Try it now`. Esta característica le permite especificar la configuración en un formulario HTML basado en web y realizar la consulta sin necesidad de escribir código. Una vez que la consulta devuelve resultados, se proporciona un ejemplo del comando CURL para mostrar los encabezados HTTP y el formato de cuerpo requeridos. |

![Página principal del contenedor](./media/cognitive-services-containers-api-documentation/container-webpage.png)
