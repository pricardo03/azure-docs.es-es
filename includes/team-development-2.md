---
title: archivo de inclusión
description: archivo de inclusión
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: e3762744d2d1f021531694b2d9a3f75ca1c3ed49
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55291028"
---
### <a name="run-the-service"></a>Ejecución del servicio

1. Presione la tecla F5 (o escriba `azds up` en la ventana de terminal) para ejecutar el servicio. El servicio se ejecutará automáticamente en el espacio recién seleccionado `default/scott`. 
1. Puede confirmar que el servicio se ejecuta en su propio espacio mediante la ejecución de `azds list-up` de nuevo. En primer lugar, observará que se ejecuta una instancia de `mywebapi` en el espacio `default/scott` (la versión que se ejecuta en `default` sigue ejecutándose, pero no aparece). Si ejecuta `azds list-uris`, observará que la dirección URL del punto de acceso para `webfrontend` tiene como prefijo el texto "scott.s.". Esta dirección URL es única para el espacio `default/scott`. La dirección URL especial significa que las solicitudes enviadas a la "dirección URL de scott" intentarán, en primer lugar, enrutar a los servicios del espacio `default/scott`, pero si se produce un error, realizarán la conmutación por recuperación en los servicios del espacio `default`.

```
Name                      DevSpace  Type     Updated  Status
------------------------  --------  -------  -------  -------
mywebapi                  scott     Service  3m ago   Running
mywebapi-bb4f4ddd8-sbfcs  scott     Pod      3m ago   Running
webfrontend               default   Service  26m ago  Running
```

```
Uri                                                            Status
-------------------------------------------------------------  ---------
http://localhost:53831 => mywebapi.scott:80                    Tunneled
http://webfrontend.6364744826e042319629.canadaeast.aksapp.io/  Available
```

![](../articles/dev-spaces/media/common/space-routing.png)

Esta característica integrada de Azure Dev Spaces le permite probar el código en un espacio compartido sin que sea necesario que cada desarrollador vuelva a crear la pila completa de servicios en su espacio. Este enrutamiento requiere que el código de la aplicación reenvíe encabezados de propagación, como se ilustra en el paso anterior de esta guía.

### <a name="test-code-in-a-space"></a>Prueba de código en un espacio
Para probar la nueva versión de `mywebapi` con `webfrontend`, abra el explorador en la dirección URL del punto de acceso público de `webfrontend` y vaya a la página Acerca de. Debería ver el nuevo mensaje.

Ahora, quite la parte "scott.s." de la dirección URL y actualice el explorador. Debería ver el comportamiento anterior (con la versión de `mywebapi` que se ejecuta en `default`).
