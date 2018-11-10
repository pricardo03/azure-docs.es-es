---
title: Desarrollo en equipo para Azure Dev Spaces mediante .NET Core y Visual Studio | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.custom: vs-azure
ms.workload: azure-vs
ms.component: azds-kubernetes
author: iainfoulds
ms.author: iainfou
ms.date: 07/09/2018
ms.topic: tutorial
description: Desarrollo rápido de Kubernetes con contenedores y microservicios en Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores
ms.openlocfilehash: 8bed7801e76e19a391b5b0685c090ca1b2336410
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2018
ms.locfileid: "50979070"
---
# <a name="team-development-with-azure-dev-spaces"></a>Desarrollo en equipo con Azure Dev Spaces

En este tutorial, aprenderá a usar varios espacios de desarrollo para trabajar de forma simultánea en entornos de desarrollo diferentes, manteniendo el trabajo independiente en espacios de desarrollo independientes en el mismo clúster.

## <a name="call-another-container"></a>Llamada a otro contenedor
En esta sección va a crear un segundo servicio `mywebapi` y a hacer que `webfrontend` lo llame. Cada servicio se ejecutará en contenedores diferentes. A continuación, realizará la depuración entre los dos contenedores.

![](media/common/multi-container.png)

### <a name="download-sample-code-for-mywebapi"></a>Descargue el código de ejemplo para *mywebapi*.
Por motivos de tiempo, vamos a descargar código de ejemplo desde un repositorio de GitHub. Vaya a https://github.com/Azure/dev-spaces y seleccione **Clone or Download** (Clonar o descargar) para descargar el repositorio de GitHub. El código de esta sección se encuentra en `samples/dotnetcore/getting-started/mywebapi`.

### <a name="run-mywebapi"></a>Ejecución de *mywebapi*
1. Abra el proyecto `mywebapi` en *otra ventana de Visual Studio*.
1. Seleccione **Azure Dev Spaces** en la lista desplegable de configuración de inicio como hizo anteriormente con el proyecto `webfrontend`. En lugar de crear un nuevo clúster de AKS esta vez, seleccione el mismo que ya ha creado. Al igual que antes, deje el espacio con el valor predeterminado `default` y haga clic en **Aceptar**. En la ventana de salida, puede observar que Visual Studio comienza a preparar este nuevo servicio en el espacio de desarrollo con el fin de acelerar el proceso al comenzar la depuración.
1. Presione F5 y espere a que el servicio se compile e implemente. Sabrá que está listo cuando la barra de estado de Visual Studio se vuelva de color naranja
1. Anote la dirección URL del punto de conexión que se muestra en el panel **Azure Dev Spaces para AKS** en la ventana **Salida**. Se parecerá a esta http://localhost:\<portnumber\>. Puede parecer que el contenedor se ejecuta localmente, pero en realidad lo hace en el espacio de desarrollo de Azure.
2. Cuando `mywebapi` esté listo, abra el explorador en la dirección de localhost y anexe `/api/values` a la dirección URL para invocar la API de GET predeterminada de `ValuesController`. 
3. Si todos los pasos se realizaron correctamente, podrá ver una respuesta del servicio `mywebapi` parecida a esta.

    ![](media/get-started-netcore-visualstudio/WebAPIResponse.png)

### <a name="make-a-request-from-webfrontend-to-mywebapi"></a>Realización de una solicitud de *webfrontend* a *mywebapi*
Vamos ahora a escribir código en `webfrontend` que realiza una solicitud a `mywebapi`. Cambie a la ventana de Visual Studio que tiene el proyecto `webfrontend`. En el archivo `HomeController.cs` *reemplace* el código en el método About por el código siguiente:

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

En el código de ejemplo anterior se reenvía el encabezado `azds-route-as` de la solicitud entrante a la solicitud saliente. Más adelante se verá como el uso de esta clase facilita una experiencia de desarrollo más productiva en escenarios de trabajo en equipo.

### <a name="debug-across-multiple-services"></a>Depuración entre varios servicios
1. En este momento, `mywebapi` aún se estará ejecutando con el depurador asociado. Si no es así, presione F5 en el proyecto `mywebapi`.
1. Establezca un punto de interrupción en el método `Get(int id)` en el archivo `Controllers/ValuesController.cs` que controla las solicitudes GET `api/values/{id}`.
1. En el proyecto `webfrontend` donde pegó el código anterior, establezca un punto de interrupción justo antes de que se envíe una solicitud GET a `mywebapi/api/values`.
1. Presione F5 en el proyecto `webfrontend`. Visual Studio abre de nuevo un explorador en el puerto localhost adecuado y se muestra la aplicación web.
1. Haga clic en el vínculo "**Acerca de**" en la parte superior de la página para desencadenar el punto de interrupción del proyecto `webfrontend`. 
1. Presione F10 para continuar. Ahora se desencadena el punto de interrupción del proyecto `mywebapi`.
1. Presione F5 para continuar; volverá al código del proyecto `webfrontend`.
1. Al presionar F5 una vez más, se completa la solicitud y se devuelve una página del explorador. En la aplicación web, la página Acerca de muestra un mensaje que han concatenado los dos servicios: "Hello from webfrontend and Hello from mywebapi".

¡Listo! Ahora tiene una aplicación de varios contenedores donde cada uno se puede desarrollar e implementar por separado.

## <a name="learn-about-team-development"></a>Aprenda sobre el desarrollo en equipo.

Hasta ahora, ha ejecutado el código de la aplicación como si fuera el único desarrollador que trabaja en la aplicación. En esta sección, aprenderá cómo Azure Dev Spaces simplifica el desarrollo en equipo:
* Habilite un equipo de desarrolladores para trabajar en el mismo entorno, mediante el trabajo en un espacio de desarrollo compartido o en espacios de desarrollo distintos, lo que sea necesario.
* Permitirá que cada desarrollador efectúe una iteración en su código de forma aislada y sin peligro de interrumpir a otros desarrolladores.
* Probará el código de un extremo a otro, antes de confirmarlo, sin tener que crear bocetos o dependencias simuladas.

### <a name="challenges-with-developing-microservices"></a>Dificultades con el desarrollo de microservicios
Por el momento, la aplicación de ejemplo no es muy compleja. Pero en el trabajo de desarrollo del mundo real, las dificultades pronto surgen a medida que se agregan más servicios y crece el equipo de desarrollo.

Imagínese trabajando en un servicio que interactúa con otras docenas de servicios.

- Puede ser poco realista ejecutarlo todo localmente para el desarrollo. Es posible que la máquina de desarrollo no tenga recursos suficientes para ejecutar la aplicación completa. O bien, quizás la aplicación tenga puntos de conexión que deben estar accesibles públicamente (por ejemplo, la aplicación responde a un webhook desde una aplicación SaaS).
- Puede intentar ejecutar solo los servicios de los que depende, pero esto significa que debe conocer el cierre completo de dependencias (por ejemplo, las dependencias de dependencias). O bien sea cuestión de no saber fácilmente cómo compilar y ejecutar las dependencias porque no ha trabajado con ellas.
- Algunos desarrolladores lo que hacen es simular o realizar bocetos de muchas de sus dependencias de servicios. Este sistema puede funcionar a veces, pero administrar esos bocetos puede ocuparles pronto todo el trabajo de desarrollo. Además, puede ocurrir que el entorno de desarrollo no se parezca al de producción, y que aparezcan errores imperceptibles.
- A eso se suma la dificultad de hacer cualquier tipo de prueba de un extremo a otro. Las pruebas de integración solo pueden suceder de forma realista tras una confirmación, lo que significa que verá problemas más adelante en el ciclo de desarrollo.

    ![](media/common/microservices-challenges.png)

### <a name="work-in-a-shared-dev-space"></a>Trabajo en un espacio de desarrollo compartido
Con Azure Dev Spaces se puede configurar un espacio de desarrollo *compartido* en Azure. Cada desarrollador puede centrarse en su parte de la aplicación y desarrollar de forma iterativa *código de confirmación previa* en un espacio de desarrollo que ya contiene los restantes servicios y recursos en la nube de los que dependen sus escenarios. Las dependencias siempre están actualizadas y los desarrolladores trabajan de una manera que refleja el entorno de producción.

### <a name="work-in-your-own-space"></a>Trabajar en su propio espacio de trabajo
A medida que desarrolla código para su servicio, y antes de estar preparado para insertarlo en el repositorio, el código estará normalmente en buenas condiciones. Aún estará dándole forma, probándolo y experimentando con soluciones de manera iterativa. Azure Dev Spaces proporciona el concepto de un **espacio**, que le permite trabajar de forma aislada sin miedo a que le interrumpan los miembros de su equipo.

Para asegurarse de que los servicios `webfrontend` y `mywebapi` se ejecutan **en el espacio de desarrollo `default`**, siga estos pasos.
1. Cierre las sesiones de depuración o de F5 en ambos servicios, pero mantenga abierto el proyecto en sus ventanas de Visual Studio.
2. Cambie a la ventana de Visual Studio con el proyecto `mywebapi` y presione Ctrl+F5 para ejecutar el servicio sin el depurador asociado.
3. Cambie a la ventana de Visual Studio con el proyecto `webfrontend` y presione Ctrl+F5 para ejecutarlo también.

> [!Note]
> En ocasiones es necesario actualizar el explorador después de que la página web se muestra inicialmente a continuación de una operación Ctrl+F5.

Cualquiera que abra la dirección URL pública y vaya a la aplicación web, invocará la ruta de acceso al código que ha escrito, que recorrerá ambos servicios mediante el espacio predeterminado `default`. Ahora, suponga que quiere seguir desarrollando `mywebapi`; ¿cómo podría hacerlo sin interrumpir a otros desarrolladores que usan el espacio de desarrollo? Para ello, configurará su propio espacio.

### <a name="create-a-new-dev-space"></a>Creación de un nuevo espacio de desarrollo
En Visual Studio, puede crear espacios adicionales que se usarán al presionar F5 o Ctrl+F5 para su servicio. Puede llamar a un espacio como quiera y puede ser flexible sobre lo que significa (p. ej. `sprint4` o `demo`).

Siga estos pasos para crear un nuevo espacio:
1. Cambie a la ventana de Visual Studio con el proyecto `mywebapi`.
2. Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** y seleccione **Propiedades**.
3. Seleccione la pestaña **Depurar** a la izquierda para mostrar la configuración de Azure Dev Spaces.
4. Desde aquí, puede cambiar o crear el clúster o el espacio que se usará al presionar F5 o Ctrl+F5. *Asegúrese de que el espacio de Azure Dev Spaces que creó anteriormente está seleccionado*.
5. En la lista desplegable de espacios, seleccione **<Create New Space…>** (Crear nuevo espacio).

    ![](media/get-started-netcore-visualstudio/Settings.png)

6. En el cuadro de diálogo **Add Space** (Agregar espacio), configure el espacio principal como **predeterminado** y escriba el nombre del nuevo espacio. Puede usar su nombre (por ejemplo, "scott") para el nuevo espacio de forma que sus compañeros puedan identificar fácilmente el espacio en el que está trabajando. Haga clic en **OK**.

    ![](media/get-started-netcore-visualstudio/AddSpace.png)

7. Ahora verá el clúster de AKS y el nuevo espacio seleccionados en la página de propiedades del proyecto.

    ![](media/get-started-netcore-visualstudio/Settings2.png)

### <a name="update-code-for-mywebapi"></a>Actualización del código de *mywebapi*

1. En el proyecto `mywebapi`, realice un cambio de código al método `string Get(int id)` en el archivo `Controllers/ValuesController.cs` de la manera siguiente:
 
    ```csharp
    [HttpGet("{id}")]
    public string Get(int id)
    {
        return "mywebapi now says something new";
    }
    ```

2. Establezca un punto de interrupción en este bloque de código actualizado (quizás ya tenga uno establecido desde antes).
3. Presione F5 para iniciar el servicio `mywebapi`. El servicio se iniciará en su clúster mediante el espacio seleccionado, que en este caso es `scott`.

Este es un diagrama que le ayudará a comprender cómo funcionan los distintos espacios. La ruta de acceso morada muestra una solicitud a través del espacio `default`, que es la ruta de acceso predeterminada que se usa si no hay ningún espacio antepuesto a la dirección URL. La ruta de acceso rosa muestra una solicitud a través del espacio `default/scott`.

![](media/common/Space-Routing.png)

Esta funcionalidad integrada de Azure Dev Spaces le permite probar el código de un extremo a otro de un entorno compartido sin necesidad de que cada desarrollador vuelva a crear la pila completa de servicios en su espacio. Este enrutamiento requiere el reenvío de encabezados de propagación en el código de aplicación, como se ilustra en el paso anterior de esta guía.

### <a name="test-code-running-in-the-defaultscott-space"></a>Prueba del código que se ejecuta en el espacio `default/scott`
Para probar la nueva versión de `mywebapi` junto con `webfrontend`, abra el explorador en la dirección URL del punto de acceso público de `webfrontend` (por ejemplo, http://webfrontend.123456abcdef.eastus.aksapp.io) y vaya a la página Acerca de. Verá el mensaje original "Hello from webfrontend and Hello from mywebapi".

Ahora, agregue la parte "scott.s." a la dirección URL para que ponga algo parecido a http://scott.s.webfrontend.123456abcdef.eastus.aksapp.io y actualice el explorador. Se llegará al punto de interrupción establecido en el proyecto `mywebapi`. Presione F5 para continuar; en el explorador debería ver el nuevo mensaje "Hello from webfrontend and mywebapi now says something new". El motivo es que la ruta de acceso al código actualizado en `mywebapi` se ejecuta en el espacio `default/scott`.

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
