---
title: Ejecución de varios servicios dependientes con .NET Core y Visual Studio
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.custom: vs-azure
ms.workload: azure-vs
author: zr-msft
ms.author: zarhoads
ms.date: 07/09/2018
ms.topic: tutorial
description: Desarrollo rápido de Kubernetes con contenedores y microservicios en Azure
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores, Helm, service mesh, enrutamiento de service mesh, kubectl, k8s '
ms.openlocfilehash: 1d0b6a7848953bf4c6bf210f37df58d68ac86378
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60707484"
---
# <a name="multi-service-development-with-azure-dev-spaces"></a>Desarrollo de varios servicios con Azure Dev Spaces

En este tutorial, aprenderá cómo puede desarrollar aplicaciones de varios servicios con Azure Dev Spaces, junto con algunas de las ventajas que proporciona Dev Spaces.

## <a name="call-another-container"></a>Llamada a otro contenedor
En esta sección va a crear un segundo servicio `mywebapi` y a hacer que `webfrontend` lo llame. Cada servicio se ejecutará en contenedores diferentes. A continuación, realizará la depuración entre los dos contenedores.

![](media/common/multi-container.png)

### <a name="download-sample-code-for-mywebapi"></a>Descargue el código de ejemplo para *mywebapi*.
Por motivos de tiempo, vamos a descargar código de ejemplo desde un repositorio de GitHub. Vaya a https://github.com/Azure/dev-spaces y seleccione **Clone or Download** (Clonar o descargar) para descargar el repositorio de GitHub. El código de esta sección se encuentra en `samples/dotnetcore/getting-started/mywebapi`.

### <a name="run-mywebapi"></a>Ejecución de *mywebapi*
1. Abra el proyecto `mywebapi` en *otra ventana de Visual Studio*.
1. Seleccione **Azure Dev Spaces** en la lista desplegable de configuración de inicio como hizo anteriormente con el proyecto `webfrontend`. En lugar de crear un nuevo clúster de AKS esta vez, seleccione el mismo que ya ha creado. Al igual que antes, deje el espacio con el valor predeterminado `default` y haga clic en **Aceptar**. En la ventana de salida, puede observar que Visual Studio comienza a preparar este nuevo servicio en el espacio de desarrollo con el fin de acelerar el proceso al comenzar la depuración.
1. Presione F5 y espere a que el servicio se compile e implemente. Sabrá que está listo cuando la barra de estado de Visual Studio se vuelva de color naranja
1. Anote la dirección URL del punto de conexión que se muestra en el panel **Azure Dev Spaces para AKS** en la ventana **Salida**. Se parecerá a esta `http://localhost:<portnumber>`. Puede parecer que el contenedor se ejecuta localmente, pero en realidad lo hace en el espacio de desarrollo de Azure.
2. Cuando `mywebapi` esté listo, abra el explorador en la dirección de localhost y anexe `/api/values` a la dirección URL para invocar la API de GET predeterminada de `ValuesController`. 
3. Si todos los pasos se realizaron correctamente, podrá ver una respuesta del servicio `mywebapi` parecida a esta.

    ![](media/get-started-netcore-visualstudio/WebAPIResponse.png)

### <a name="make-a-request-from-webfrontend-to-mywebapi"></a>Realización de una solicitud de *webfrontend* a *mywebapi*
Vamos ahora a escribir código en `webfrontend` que realiza una solicitud a `mywebapi`. Cambie a la ventana de Visual Studio que tiene el proyecto `webfrontend`. En el archivo `HomeController.cs` *sustituye* el código en el método About por el siguiente código:

   ```csharp
   public async Task<IActionResult> About()
   {
      ViewData["Message"] = "Hello from webfrontend";

      using (var client = new System.Net.Http.HttpClient())
            {
                // Call *mywebapi*, and display its response in the page
                var request = new System.Net.Http.HttpRequestMessage();
                request.RequestUri = new Uri("http://mywebapi/api/values/1");
                if (this.Request.Headers.ContainsKey("azds-route-as"))
                {
                    // Propagate the dev space routing header
                    request.Headers.Add("azds-route-as", this.Request.Headers["azds-route-as"] as IEnumerable<string>);
                }
                var response = await client.SendAsync(request);
                ViewData["Message"] += " and " + await response.Content.ReadAsStringAsync();
            }

      return View();
   }
   ```

En el código de ejemplo anterior se reenvía el encabezado `azds-route-as` de la solicitud entrante a la solicitud saliente. Más adelante verá como esto facilita una experiencia de desarrollo más productiva en escenarios de [trabajo en equipo](team-development-netcore-visualstudio.md).

### <a name="debug-across-multiple-services"></a>Depuración entre varios servicios
1. En este momento, `mywebapi` aún se estará ejecutando con el depurador asociado. Si no es así, presione F5 en el proyecto `mywebapi`.
1. Establezca un punto de interrupción en el método `Get(int id)` en el archivo `Controllers/ValuesController.cs` que controla las solicitudes GET `api/values/{id}`.
1. En el proyecto `webfrontend` donde pegó el código anterior, establezca un punto de interrupción justo antes de que se envíe una solicitud GET a `mywebapi/api/values`.
1. Presione F5 en el proyecto `webfrontend`. Visual Studio abre de nuevo un explorador en el puerto localhost adecuado y se muestra la aplicación web.
1. Haga clic en el vínculo "**Acerca de**" en la parte superior de la página para desencadenar el punto de interrupción del proyecto `webfrontend`. 
1. Presione F10 para continuar. Ahora se desencadena el punto de interrupción del proyecto `mywebapi`.
1. Presione F5 para continuar; volverá al código del proyecto `webfrontend`.
1. Al presionar F5 una vez más, se completa la solicitud y se devuelve una página del explorador. En la aplicación web, la página Acerca de mostrará un mensaje concatenado por los dos servicios: "Hello from webfrontend y Hello from mywebapi".

¡Listo! Ahora tiene una aplicación de varios contenedores donde cada uno se puede desarrollar e implementar por separado.

### <a name="automatic-tracing-for-http-messages"></a>Seguimiento automático de los mensajes HTTP
Puede que haya observado que, aunque *webfrontend* no contiene ningún código especial para imprimir la llamada HTTP que realiza a *mywebapi*, puede ver que HTTP realiza un seguimiento de mensajes en la ventana de salida:
```
// The request from your browser
default.webfrontend.856bb3af715744c6810b.eus.azds.io --gyk-> webfrontend:
   GET /Home/About HTTP/1.1

// *webfrontend* reaching out to *mywebapi*
webfrontend-668b7ddb9f-n5rhj --pu5-> mywebapi:
   GET /api/values/1 HTTP/1.1

// Response from *mywebapi*
webfrontend-668b7ddb9f-n5rhj <-pu5-- mywebapi:
   HTTP/1.1 200 OK
   Hello from mywebapi

// Response from *webfrontend* to your browser
default.webfrontend.856bb3af715744c6810b.eus.azds.io <-gyk-- webfrontend:
   HTTP/1.1 200 OK
   <!DOCTYPE html>
   <html>
   <head>
       <meta charset="utf-8" />
       <meta name="viewport" content="width=device-width, initial-sc...<[TRUNCATED]>
```
Esta es una de las ventajas "gratuitas" que obtendrá de la instrumentación de Dev Spaces. Insertamos los componentes que realizan los seguimientos de las solicitudes HTTP a medida que atraviesan el sistema para que le sea más fácil realizar un seguimiento de llamadas complejas a varios servicios durante el desarrollo.

### <a name="well-done"></a>¡Listo!
Ahora tiene una aplicación de varios contenedores donde cada uno se puede desarrollar e implementar por separado.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre el desarrollo en Dev Spaces](team-development-netcore-visualstudio.md)
