---
title: archivo de inclusión
description: archivo de inclusión
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: 2f2177b289ba9d13ba635bddcd61748e1e8ab0d1
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/14/2018
ms.locfileid: "39063011"
---
### <a name="run-the-service"></a>Ejecución del servicio

1. Presione la tecla F5 (o escriba `azds up` en la ventana de terminal) para ejecutar el servicio. El servicio se ejecutará automáticamente en el espacio recién seleccionado `default/scott`. 
1. Puede confirmar que el servicio se ejecuta en su propio espacio mediante la ejecución de `azds list-up` de nuevo. En primer lugar, observará que se ejecuta una instancia de `mywebapi` en el espacio `default/scott` (la versión que se ejecuta en `default` sigue ejecutándose, pero no aparece). En segundo lugar, la dirección URL del punto de acceso para `webfrontend` tiene como prefijo el texto "scott.s.". Esta dirección URL es única para el espacio `default/scott`. La dirección URL especial significa que las solicitudes enviadas a la "dirección URL de scott" intentarán, en primer lugar, enrutar a los servicios del espacio `default/scott`, pero si se produce un error, realizarán la conmutación por recuperación en los servicios del espacio `default`.

```
Name         Space          Chart              Ports   Updated     Access Points
-----------  --------       -----------------  ------  ----------  -------------
mywebapi     default/scott  mywebapi-0.1.0     80/TCP  15s ago     http://localhost:61466
webfrontend  default        webfrontend-0.1.0  80/TCP  5h ago      http://scott.s.webfrontend-contosodev.1234abcdef.eastus.aksapp.io
```

![](../media/common/space-routing.png)

Esta característica integrada de Azure Dev Spaces le permite probar el código en un espacio compartido sin que sea necesario que cada desarrollador vuelva a crear la pila completa de servicios en su espacio. Este enrutamiento requiere que el código de la aplicación reenvíe encabezados de propagación, como se ilustra en el paso anterior de esta guía.

### <a name="test-code-in-a-space"></a>Prueba de código en un espacio
Para probar la nueva versión de `mywebapi` con `webfrontend`, abra el explorador en la dirección URL del punto de acceso público de `webfrontend` y vaya a la página Acerca de. Debería ver el nuevo mensaje.

Ahora, quite la parte "scott.s." de la dirección URL y actualice el explorador. Debería ver el comportamiento anterior (con la versión de `mywebapi` que se ejecuta en `default`)
