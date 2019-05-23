---
title: Ejecución de varios servicios dependientes con Java y VS Code
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: DrEsteban
ms.author: stevenry
ms.date: 11/21/2018
ms.topic: tutorial
description: Desarrollo rápido de Kubernetes con contenedores y microservicios en Azure
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores, Helm, service mesh, enrutamiento de service mesh, kubectl, k8s '
manager: yuvalm
ms.openlocfilehash: a5afd093e0f961d048681465850419c5e8712557
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2019
ms.locfileid: "65800387"
---
# <a name="multi-service-development-with-azure-dev-spaces"></a>Desarrollo de varios servicios con Azure Dev Spaces

En este tutorial, aprenderá cómo puede desarrollar aplicaciones de varios servicios con Azure Dev Spaces, junto con algunas de las ventajas que proporciona Dev Spaces.

## <a name="call-a-service-running-in-a-separate-container"></a>Llamada a un servicio que se ejecuta en un contenedor diferente

En esta sección, puede crear un segundo servicio `mywebapi` y hacer que `webfrontend` lo llame. Cada servicio se ejecutará en contenedores diferentes. A continuación, realizará la depuración entre los dos contenedores.

![Varios contenedores](media/common/multi-container.png)

### <a name="download-sample-code-for-mywebapi"></a>Descargue el código de ejemplo para *mywebapi*.
Por motivos de tiempo, vamos a descargar código de ejemplo desde un repositorio de GitHub. Vaya a https://github.com/Azure/dev-spaces y seleccione **Clone or Download** (Clonar o descargar) para descargar el repositorio de GitHub. El código de esta sección se encuentra en `samples/java/getting-started/mywebapi`.

### <a name="run-mywebapi"></a>Ejecución de *mywebapi*
1. Abra la carpeta `mywebapi` en una *ventana independiente de VS Code*.
1. Abra la **Paleta de comandos** (mediante el menú **Vista | Paleta de comandos**) y use Autocompletar para escribir y seleccionar este comando: `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`.
1. Presione F5 y espere a que el servicio se compile e implemente. Sabrá que está listo cuando aparezca un mensaje similar al de abajo en la consola de depuración:

    ```cmd
    2019-03-11 17:02:35.935  INFO 216 --- [           main] com.ms.sample.mywebapi.Application       : Started Application in 8.164 seconds (JVM running for 9.272)
    ```

1. La dirección URL del punto de conexión se parecerá a esta `http://localhost:<portnumber>`. **Sugerencia: La barra de estado de VS Code mostrará una dirección URL en la que se puede hacer clic.** Puede parecer que el contenedor se ejecuta localmente, pero en realidad lo hace en el espacio de desarrollo de Azure. La razón de la dirección de localhost es que `mywebapi` no ha definido ningún punto de conexión público y solo se puede acceder a él desde la instancia de Kubernetes. Para mayor comodidad, y para facilitar la interacción con el servicio privado desde la máquina local, Azure Dev Spaces crea un túnel SSH temporal al contenedor que se ejecuta en Azure.
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
1. En la aplicación web, la página Acerca de mostrará un mensaje concatenado por los dos servicios: "Hello from webfrontend y Hello from mywebapi".

### <a name="automatic-tracing-for-http-messages"></a>Seguimiento automático de los mensajes HTTP
Puede que haya observado que, aunque *webfrontend* no contiene ningún código especial para imprimir la llamada HTTP que realiza a *mywebapi*, puede ver que HTTP realiza un seguimiento de mensajes en la ventana de salida:
```
// The request from your browser
default.webfrontend.856bb3af715744c6810b.eus.azds.io --ytv-> webfrontend:8080:
   GET /greeting?_=1544503627515 HTTP/1.1

// *webfrontend* reaching out to *mywebapi*
webfrontend --ve4-> mywebapi:
   GET / HTTP/1.1

// Response from *mywebapi*
webfrontend <-ve4-- mywebapi:
   HTTP/1.1 200
   Hello from mywebapi

// Response from *webfrontend* to your browser
default.webfrontend.856bb3af715744c6810b.eus.azds.io <-ytv-- webfrontend:8080:
   HTTP/1.1 200
   Hello from webfrontend and
   Hello from mywebapi
```
Esta es una de las ventajas "gratuitas" que obtendrá de la instrumentación de Dev Spaces. Insertamos los componentes que realizan los seguimientos de las solicitudes HTTP a medida que atraviesan el sistema para que le sea más fácil realizar un seguimiento de llamadas complejas a varios servicios durante el desarrollo.

### <a name="well-done"></a>¡Listo!
Ahora tiene una aplicación de varios contenedores donde cada uno se puede desarrollar e implementar por separado.


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre el desarrollo en Dev Spaces](team-development-java.md)
