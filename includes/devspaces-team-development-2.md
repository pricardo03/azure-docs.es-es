---
title: archivo de inclusión
description: archivo de inclusión
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
author: DrEsteban
ms.author: stevenry
ms.date: 12/17/2018
ms.topic: include
manager: yuvalm
ms.openlocfilehash: 7dddf7e48913aea9d84e0f8b66c2c9d29449ee6f
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56825631"
---
### <a name="run-the-service"></a>Ejecución del servicio

1. Presione la tecla F5 (o escriba `azds up` en la ventana de terminal) para ejecutar el servicio. El servicio se ejecutará automáticamente en el espacio recién seleccionado _dev/scott_. 
1. Puede confirmar que el servicio se ejecuta en su propio espacio mediante la ejecución de `azds list-up` de nuevo. Observará que se ejecuta una instancia de *mywebapi* en el espacio _dev/scott_ (la versión que se ejecuta en _dev_ sigue ejecutándose, pero no aparece).

    ```
    Name                      DevSpace  Type     Updated  Status
    mywebapi                  scott     Service  3m ago   Running
    mywebapi-bb4f4ddd8-sbfcs  scott     Pod      3m ago   Running
    webfrontend               dev       Service  26m ago  Running
    ```

1. Ejecute `azds list-uris` y tenga en cuenta la dirección URL del punto de acceso para *webfrontend*.

    ```
    Uri                                                                        Status
    -------------------------------------------------------------------------  ---------
    http://localhost:53831 => mywebapi.scott:80                                Tunneled
    http://scott.s.dev.webfrontend.6364744826e042319629.ce.azds.io/  Available
    ```

1. Use la dirección URL con el prefijo*scott.s* para ir a la aplicación. Tenga en cuenta que esta dirección URL se resuelve todavía. Esta dirección URL es única para el espacio _dev/scott_. La dirección URL especial significa que las solicitudes enviadas a la "dirección URL de Scott" intentarán, en primer lugar, enrutar a los servicios del espacio _dev/scott_, pero si se produce un error, realizarán la conmutación por recuperación a los servicios del espacio _dev_.

<!--
TODO: replace 2 & 3 with below once bug#753164 and PR#158827 get pushed to production.

You can confirm that your service is running in its own space by running `azds list-up` again. First, you'll notice an instance of *mywebapi* is now running in the _dev/scott_ space (the version running in _dev_ is still running but it is not listed). If you run `azds list-uris`, you will notice that the access point URL for *webfrontend* is prefixed with the text "scott.s.". This URL is unique to the _dev/scott_ space. The special URL signifies that requests sent to the "Scott URL" will try to first route to services in the _dev/scott_ space, but if that fails, they will fall back to services in the _dev_ space.

```
Name                      DevSpace  Type     Updated  Status
mywebapi                  scott     Service  3m ago   Running
mywebapi-bb4f4ddd8-sbfcs  scott     Pod      3m ago   Running
webfrontend               dev       Service  26m ago  Running
```

```
Uri                                                                        Status
-------------------------------------------------------------------------  ---------
http://localhost:53831 => mywebapi.scott:80                                Tunneled
http://scott.s.dev.webfrontend.6364744826e042319629.ce.azds.io/  Available
```
-->

![](../articles/dev-spaces/media/common/space-routing.png)

Esta característica integrada de Azure Dev Spaces le permite probar el código en un espacio compartido sin que sea necesario que cada desarrollador vuelva a crear la pila completa de servicios en su espacio. Este enrutamiento requiere que el código de la aplicación reenvíe encabezados de propagación, como se ilustra en el paso anterior de esta guía.

### <a name="test-code-in-a-space"></a>Prueba de código en un espacio
Para probar la nueva versión de *mywebapi* junto con *webfrontend*, abra el explorador en la dirección URL del punto de acceso público de *webfrontend* y vaya a la página Acerca de. Debería ver el nuevo mensaje.

Ahora, quite la parte "scott.s." de la dirección URL y actualice el explorador. Debería ver el comportamiento anterior (con la versión de *mywebapi* que se ejecuta en _dev_).

Una vez que tenga un espacio _dev_ que contenga siempre los cambios más recientes, y suponiendo que la aplicación está diseñada para aprovechar las ventajas del enrutamiento basado en espacios de DevSpace, como se describe en esta sección del tutorial, debería resulta fácil ver cómo Dev Spaces puede ayudar en gran medida a probar las nuevas características en el contexto de la aplicación más grande. En lugar de tener que implementar _todos_ los servicios en su espacio privado, puede crear un espacio privado que se derive de _dev_y solo "subir" los servicios en los que realmente esté trabajando. La infraestructura de enrutamiento de Dev Spaces controlará el resto mediante el uso de todos los servicios de su espacio privado que pueda encontrar, mientras se usará por defecto la versión más reciente que se ejecuta en el espacio _dev_. Y aún mejor _varios_ desarrolladores pueden desarrollar activamente servicios diferentes al mismo tiempo en su propio espacio sin interrumpirse entre sí.

### <a name="well-done"></a>¡Listo!
¡Ha completado la guía de introducción! Ha aprendido a:

> [!div class="checklist"]
> * Configurar Azure Dev Spaces con un clúster de Kubernetes administrado en Azure.
> * Desarrollar código de forma iterativa en contenedores.
> * Desarrollar de forma independiente dos servicios distintos y usar la detección de servicios DNS de Kubernetes para realizar una llamada a otro servicio.
> * Desarrollar y probar de forma productiva el código en un entorno de equipo.
> * Establecer una base de referencia de la funcionalidad con el uso de Dev Spaces para probar fácilmente los cambios aislados dentro del contexto de una aplicación de microservicio más grande

Ahora que ha explorado Azure Dev Spaces, [comparta su espacio de desarrollo con un miembro del equipo](../articles/dev-spaces/how-to/share-dev-spaces.md) y ayúdele a ver lo fácil que es colaborar con otros.

## <a name="clean-up"></a>Limpieza
Para eliminar completamente una instancia de Azure Dev Spaces en un clúster, incluidos todos los espacios de desarrollo y los servicios en ejecución dentro de él, utilice el comando `az aks remove-dev-spaces`. Tenga en cuenta que esta acción es irreversible. Puede agregar compatibilidad para Azure Dev Spaces de nuevo en el clúster, pero será como si comienza de nuevo. No se restaurarán los servicios y espacios anteriores.

En el ejemplo siguiente se enumeran los controladores de Azure Dev Spaces en la suscripción activa y, a continuación, se elimina el controlador de Azure Dev Spaces que está asociado con el clúster de AKS "myaks" en el grupo de recursos "myaks-rg".

```cmd
    azds controller list
    az aks remove-dev-spaces --name myaks --resource-group myaks-rg
```