---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 02/21/2019
ms.openlocfilehash: e80feac7dbf16652cc2e2a6176ed8b2c8c48e35b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60598807"
---
### <a name="running-multiple-containers-on-the-same-host"></a>Ejecución de varios contenedores en el mismo host

Si piensa ejecutar varios contenedores con puertos expuestos, asegúrese de que se ejecute cada contenedor con un puerto diferente. Por ejemplo, ejecute el primer contenedor en el puerto 5000 y el segundo contenedor en el puerto 5001.

Reemplace el `<container-registry>` y `<container-name>` con los valores de los contenedores que utilice. Estos no debe ser el mismo contenedor. Puede hacer que el contenedor de cara y el contenedor de LUIS que se ejecutan en el HOST juntos o puede tener varios contenedores de cara, que se ejecutan. 

Ejecute el primer contenedor en el puerto 5000. 

```bash 
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Ejecute el segundo contenedor en el puerto 5001.


```bash 
docker run --rm -it -p 5001:5000 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Cada contenedor posterior debe estar en un puerto diferente. 
