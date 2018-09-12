---
title: Desarrollo en equipo con Azure Dev Spaces con VS Code | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 07/09/2018
ms.topic: tutorial
description: Desarrollo rápido de Kubernetes con contenedores y microservicios en Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores
manager: douge
ms.openlocfilehash: b4c355c864f83bcd76c310fecb0f26dd3372e760
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162757"
---
# <a name="team-development-with-azure-dev-spaces"></a>Desarrollo en equipo con Azure Dev Spaces

En este tutorial, aprenderá a usar varios espacios de desarrollo para trabajar de forma simultánea en entornos de desarrollo diferentes, manteniendo el trabajo independiente en espacios de desarrollo independientes en el mismo clúster.

## <a name="call-a-service-running-in-a-separate-container"></a>Llamada a un servicio que se ejecuta en un contenedor diferente

En esta sección va a crear un segundo servicio `mywebapi` y a hacer que `webfrontend` lo llame. Cada servicio se ejecutará en contenedores diferentes. A continuación, realizará la depuración entre los dos contenedores.

![](media/common/multi-container.png)

### <a name="open-sample-code-for-mywebapi"></a>Abra el código de ejemplo de *mywebapi*.
Ya debería tener el código de ejemplo de `mywebapi` necesario en esta guía, en una carpeta llamada `samples` (si no es así, vaya a https://github.com/Azure/dev-spaces y seleccione **Clone or Download** (Clonar o descargar) para descargar el repositorio de GitHub). El código de esta sección se encuentra en `samples/nodejs/getting-started/mywebapi`.

### <a name="run-mywebapi"></a>Ejecución de *mywebapi*
1. Abra la carpeta `mywebapi` en una *ventana independiente de VS Code*.
1. Abra la **Paleta de comandos** (mediante el menú **Vista | Paleta de comandos**) y use Autocompletar para escribir y seleccionar este comando: `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`. Este comando no se debe confundir con el comando `azds prep`, que configura el proyecto para la implementación.
1. Presione F5 y espere a que el servicio se compile e implemente. Sabrá que está listo cuando aparezca la barra de depuración de VS Code.
1. Anote la dirección URL del punto de conexión; se parecerá a esta http://localhost:\<portnumber\>. **Sugerencia: La barra de estado de VS Code mostrará una dirección URL en la que se puede hacer clic.** Puede parecer que el contenedor se ejecuta localmente, pero en realidad se ejecuta en el entorno de desarrollo de Azure. La razón de la dirección de localhost es que `mywebapi` no ha definido ningún punto de conexión público y solo se puede acceder a él desde la instancia de Kubernetes. Para mayor comodidad, y para facilitar la interacción con el servicio privado desde la máquina local, Azure Dev Spaces crea un túnel SSH temporal al contenedor que se ejecuta en Azure.
1. Cuando `mywebapi` esté listo, abra el explorador en la dirección de localhost. Verá una respuesta del servicio `mywebapi` ("Hello from mywebapi").


### <a name="make-a-request-from-webfrontend-to-mywebapi"></a>Realización de una solicitud de *webfrontend* a *mywebapi*
Vamos ahora a escribir código en `webfrontend` que realiza una solicitud a `mywebapi`.
1. Cambie a la ventana de VS Code para `webfrontend`.
1. Agregue estas líneas de código en la parte superior de `server.js`:
    ```javascript
    var request = require('request');
    ```

3. *Reemplace* el código por el controlador GET `/api`. Al administrar una solicitud, se realiza a su vez una llamada a `mywebapi` y se devuelven los resultados de ambos servicios.

    ```javascript
    app.get('/api', function (req, res) {
       request({
          uri: 'http://mywebapi',
          headers: {
             /* propagate the dev space routing header */
             'azds-route-as': req.headers['azds-route-as']
          }
       }, function (error, response, body) {
           res.send('Hello from webfrontend and ' + body);
       });
    });
    ```

En el código de ejemplo anterior se reenvía el encabezado `azds-route-as` de la solicitud entrante a la solicitud saliente. Más adelante verá cómo esta función ayuda a los equipos con el desarrollo en colaboración.

### <a name="debug-across-multiple-services"></a>Depuración entre varios servicios
1. En este momento, `mywebapi` aún se estará ejecutando con el depurador asociado. Si no es así, presione F5 en el proyecto `mywebapi`.
1. Establezca un punto de interrupción en el controlador GET `/` predeterminado.
1. En el proyecto `webfrontend`, establezca un punto de interrupción justo antes de que se envíe una solicitud GET a `http://mywebapi`.
1. Presione F5 en el proyecto `webfrontend`.
1. Abra la aplicación web y recorra el código de ambos servicios. La aplicación web debe mostrar un mensaje concatenado por los dos servicios: "Hello from webfrontend and Hello from mywebapi".

¡Listo! Ahora tiene una aplicación de varios contenedores donde cada uno se puede desarrollar e implementar por separado.

## <a name="learn-about-team-development"></a>Aprenda sobre el desarrollo en equipo.

[!INCLUDE [](includes/team-development-1.md)]

Véalo ahora en acción:
1. Vaya a la ventana de VS Code para `mywebapi` y realice una edición de código para el controlador GET `/` predeterminado, por ejemplo:

    ```javascript
    app.get('/', function (req, res) {
        res.send('mywebapi now says something new');
    });
    ```

[!INCLUDE [](includes/team-development-2.md)]

[!INCLUDE [](includes/well-done.md)]

[!INCLUDE [](includes/clean-up.md)]




