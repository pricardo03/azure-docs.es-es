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
ms.openlocfilehash: e0f768b876b49ec006ce98decf121d73d334b6d8
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58439535"
---
### <a name="run-the-service"></a>Ejecución del servicio

Presione la tecla F5 (o escriba `azds up` en la ventana de terminal) para ejecutar el servicio. El servicio se ejecutará automáticamente en el espacio recién seleccionado _dev/scott_. Confirme que el servicio se ejecuta en su propio espacio mediante la ejecución de `azds list-up`:

```cmd
$ azds list-up

Name                      DevSpace  Type     Updated  Status
mywebapi                  scott     Service  3m ago   Running
webfrontend               dev       Service  26m ago  Running
```

Observe que hay una instancia de *mywebapi* en ejecución ahora en el espacio _dev/scott_. La versión que se ejecuta en _dev_ sigue ejecutándose, pero no aparece en la lista.

Enumere las direcciones URL del espacio actual ejecutando `azds list-uris`.

```cmd
$ azds list-uris

Uri                                                                        Status
-------------------------------------------------------------------------  ---------
http://localhost:53831 => mywebapi.scott:80                                Tunneled
http://scott.s.dev.webfrontend.6364744826e042319629.ce.azds.io/  Available
```

Observe que la dirección URL de punto de acceso público de *webfrontend* incluye el prefijo *scott.s*. Esta dirección URL es única para el espacio _dev/scott_. Este prefijo de dirección URL indica el controlador de entrada para enrutar las solicitudes a la versión _dev/scott_ de un servicio. Cuando Dev Spaces administra una solicitud con esta dirección URL, el controlador de entrada intenta primero enrutar la solicitud al servicio *webfrontend* del espacio _dev/scott_. Si se produce un error, la solicitud se enrutará al servicio *webfrontend* del espacio _dev_ como reserva. Observe también que hay una dirección URL de localhost para acceder al servicio a través de localhost mediante la funcionalidad de *reenvío de puerto* de Kubernetes. Para más información acerca de las direcciones URL y el enrutamiento en Azure Dev Spaces, consulte [How Azure Dev Spaces works and is configured](../articles/dev-spaces/how-dev-spaces-works.md) (Funcionamiento y configuración de Azure Dev Spaces).



![Enrutamiento de espacio](../articles/dev-spaces/media/common/Space-Routing.png)

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

Ahora que ha explorado Azure Dev Spaces, [comparta su espacio de desarrollo con un miembro del equipo](../articles/dev-spaces/how-to/share-dev-spaces.md) y comience a colaborar.

## <a name="clean-up"></a>Limpieza
Para eliminar completamente una instancia de Azure Dev Spaces en un clúster, incluidos todos los espacios de desarrollo y los servicios en ejecución dentro de él, utilice el comando `az aks remove-dev-spaces`. Tenga en cuenta que esta acción es irreversible. Puede agregar compatibilidad para Azure Dev Spaces de nuevo en el clúster, pero será como si comienza de nuevo. No se restaurarán los servicios y espacios anteriores.

En el ejemplo siguiente se enumeran los controladores de Azure Dev Spaces en la suscripción activa y, a continuación, se elimina el controlador de Azure Dev Spaces que está asociado con el clúster de AKS "myaks" en el grupo de recursos "myaks-rg".

```cmd
    azds controller list
    az aks remove-dev-spaces --name myaks --resource-group myaks-rg
```