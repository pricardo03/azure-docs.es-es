---
title: Desarrollo en equipo con Azure Dev Spaces mediante Java y VS Code | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: stepro
ms.author: stephpr
ms.date: 08/01/2018
ms.topic: tutorial
description: Desarrollo rápido de Kubernetes con contenedores y microservicios en Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores
manager: mmontwil
ms.openlocfilehash: b1f05c176c6005a2fda8025b4645813013a30cb3
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47407185"
---
# <a name="team-development-with-azure-dev-spaces"></a>Desarrollo en equipo con Azure Dev Spaces

En este tutorial, aprenderá a usar varios espacios de desarrollo para trabajar de forma simultánea en entornos de desarrollo diferentes, manteniendo el trabajo independiente en espacios de desarrollo independientes en el mismo clúster.

## <a name="call-a-service-running-in-a-separate-container"></a>Llamada a un servicio que se ejecuta en un contenedor diferente

En esta sección, puede crear un segundo servicio `mywebapi` y hacer que `webfrontend` lo llame. Cada servicio se ejecutará en contenedores diferentes. A continuación, realizará la depuración entre los dos contenedores.

![Varios contenedores](media/common/multi-container.png)

### <a name="download-sample-code-for-mywebapi"></a>Descargue el código de ejemplo para *mywebapi*.
Por motivos de tiempo, vamos a descargar código de ejemplo desde un repositorio de GitHub. Vaya a https://github.com/Azure/dev-spaces y seleccione **Clone or Download** (Clonar o descargar) para descargar el repositorio de GitHub. El código de esta sección se encuentra en `samples/java/getting-started/mywebapi`.

### <a name="run-mywebapi"></a>Ejecución de *mywebapi*
1. Abra la carpeta `mywebapi` en una *ventana independiente de VS Code*.
1. Abra la **Paleta de comandos** (mediante el menú **Vista | Paleta de comandos**) y use Autocompletar para escribir y seleccionar este comando: `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`.
1. Presione F5 y espere a que el servicio se compile e implemente. Sabrá que está listo cuando aparezca la barra de depuración de VS Code.
1. La dirección URL del punto de conexión se parecerá a esta http://localhost:\<portnumber\>. **Sugerencia: La barra de estado de VS Code mostrará una dirección URL en la que se puede hacer clic.** Puede parecer que el contenedor se ejecuta localmente, pero en realidad lo hace en el espacio de desarrollo de Azure. La razón de la dirección de localhost es que `mywebapi` no ha definido ningún punto de conexión público y solo se puede acceder a él desde la instancia de Kubernetes. Para mayor comodidad, y para facilitar la interacción con el servicio privado desde la máquina local, Azure Dev Spaces crea un túnel SSH temporal al contenedor que se ejecuta en Azure.
1. Cuando `mywebapi` esté listo, abra el explorador en la dirección de localhost.
1. Si todos los pasos se realizaron correctamente, verá una respuesta del servicio `mywebapi`.

### <a name="make-a-request-from-webfrontend-to-mywebapi"></a>Realización de una solicitud de *webfrontend* a *mywebapi*
Vamos ahora a escribir código en `webfrontend` que realiza una solicitud a `mywebapi`.
1. Cambie a la ventana de VS Code para `webfrontend`.
1. *Agregue* las siguientes instrucciones `import` debajo de la instrucción `package`:

   ```java
   import java.io.*;
   import java.net.*;
   ```
1. *Reemplace* el código para el método de saludo:

    ```java
    @RequestMapping(value = "/greeting", produces = "text/plain")
    public String greeting(@RequestHeader(value = "azds-route-as", required = false) String azdsRouteAs) throws Exception {
        URLConnection conn = new URL("http://mywebapi/").openConnection();
        conn.setRequestProperty("azds-route-as", azdsRouteAs); // propagate dev space routing header
        try (BufferedReader reader = new BufferedReader(new InputStreamReader(conn.getInputStream())))
        {
            return "Hello from webfrontend and " + reader.lines().reduce("\n", String::concat);
        }
    }
    ```

En el código de ejemplo anterior se reenvía el encabezado `azds-route-as` de la solicitud entrante a la solicitud saliente. Más adelante verá cómo esta función ayuda a los equipos con el desarrollo en colaboración.

### <a name="debug-across-multiple-services"></a>Depuración entre varios servicios
1. En este momento, `mywebapi` aún se estará ejecutando con el depurador asociado. Si no es así, presione F5 en el proyecto `mywebapi`.
1. Establezca un punto de interrupción en el método `index()` del proyecto `webapi`.
1. En el proyecto `webfrontend`, establezca un punto de interrupción justo antes de que se envíe una solicitud GET a `mywebapi` en la línea que se inicia con `try`.
1. Presione F5 en el proyecto `webfrontend` (o reinicie el depurador si está en ejecución).
1. Invoque la aplicación web y recorra el código de ambos servicios.
1. En la aplicación web, la página Acerca de muestra un mensaje que han concatenado los dos servicios: "Hello from webfrontend and Hello from mywebapi".

¡Listo! Ahora tiene una aplicación de varios contenedores donde cada uno se puede desarrollar e implementar por separado.

## <a name="learn-about-team-development"></a>Aprenda sobre el desarrollo en equipo.

[!INCLUDE [](../../includes/team-development-1.md)]

Vamos a verlo en acción. Vaya a la ventana de VS Code para `mywebapi` y realice una edición de código para el método `String index()`, por ejemplo:

```java
@RequestMapping(value = "/", produces = "text/plain")
public String index() {
    return "Hello from mywebapi says something new";
}
```

[!INCLUDE [](../../includes/team-development-2.md)]

### <a name="well-done"></a>¡Listo!
¡Ha completado la guía de introducción! Ha aprendido a:

> [!div class="checklist"]
> * Configurar Azure Dev Spaces con un clúster de Kubernetes administrado en Azure.
> * Desarrollar código de forma iterativa en contenedores.
> * Desarrollar de forma independiente dos servicios distintos y usar la detección de servicios DNS de Kubernetes para realizar una llamada a otro servicio.
> * Desarrollar y probar de forma productiva el código en un entorno de equipo.

Ahora que ha explorado Azure Dev Spaces, [comparta su espacio de desarrollo con un miembro del equipo](how-to/share-dev-spaces.md) y ayúdele a ver lo fácil que es colaborar con otros.

## <a name="clean-up"></a>Limpieza
Para eliminar completamente una instancia de Azure Dev Spaces en un clúster, incluidos todos los espacios de desarrollo y los servicios en ejecución dentro de él, utilice el comando `az aks remove-dev-spaces`. Tenga en cuenta que esta acción es irreversible. Puede agregar compatibilidad para Azure Dev Spaces de nuevo en el clúster, pero será como si comienza de nuevo. No se restaurarán los servicios y espacios anteriores.

En el ejemplo siguiente se enumeran los controladores de Azure Dev Spaces en la suscripción activa y, a continuación, se elimina el controlador de Azure Dev Spaces que está asociado con el clúster de AKS "myaks" en el grupo de recursos "myaks-rg".

```cmd
    azds controller list
    az aks remove-dev-spaces --name myaks --resource-group myaks-rg
```