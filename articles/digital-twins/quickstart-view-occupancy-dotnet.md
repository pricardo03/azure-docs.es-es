---
title: Búsqueda de habitaciones disponibles con Azure Digital Twins (C#) | Microsoft Docs
description: En esta guía de inicio rápido, ejecutará dos aplicaciones de ejemplo de .NET Core para enviar información de telemetría de movimiento simulado y dióxido de carbono a un espacio de Azure Digital Twins. El objetivo es encontrar habitaciones disponibles con aire fresco desde las API de administración tras el procesamiento calculado en la nube.
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc
ms.date: 10/02/2018
ms.author: alinast
ms.openlocfilehash: e0b47b1322a520ad8b09fd2fe2967e628b5e4e03
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49322963"
---
# <a name="quickstart-find-available-rooms-using-azure-digital-twins"></a>Guía de inicio rápido: Búsqueda de habitaciones disponibles mediante Azure Digital Twins

El servicio Azure Digital Twins le permite recrear una imagen digital de su entorno físico. Así, puede recibir notificaciones mediante eventos en su entorno y personalizar las respuestas a estos. 

En esta guía de inicio rápido se usa [un par de ejemplos de .NET](https://github.com/Azure-Samples/digital-twins-samples-csharp) para digitalizar un edificio de oficinas imaginario, y se muestra cómo encontrar habitaciones disponibles en ese edificio. Con Digital Twins, puede asociar varios sensores con el entorno. Además de la disponibilidad de habitaciones, también puede averiguar si la calidad del aire de la habitación disponible es óptima, con ayuda de un sensor simulado de dióxido de carbono. Una de las aplicaciones de ejemplo generará datos de sensor aleatorios para ayudarle a visualizar este escenario.

## <a name="prerequisites"></a>Requisitos previos

1. Antes de comenzar, si no tiene una cuenta de Azure, [cree una gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

1. Las dos aplicaciones de consola que se ejecutan en esta guía de inicio rápido se escriben con C#. Deberá instalar el [SDK de .NET Core, versión 2.1.403 o superior](https://www.microsoft.com/net/download) en la máquina de desarrollo. Si tiene instalado el SDK de .NET Core, puede comprobar la versión actual de C# en la máquina de desarrollo mediante la ejecución de `dotnet --version` en un símbolo del sistema.

1. Descargue el [proyecto de C# de ejemplo](https://github.com/Azure-Samples/digital-twins-samples-csharp/archive/master.zip) y extraiga el archivo digital-twins-samples-csharp-master.zip. 


## <a name="create-a-digital-twins-instance"></a>Creación de una instancia de Digital Twins

Cree una instancia de Digital Twins en el [portal](https://portal.azure.com) mediante los pasos de esta sección.

[!INCLUDE [create-digital-twins-portal](../../includes/create-digital-twins-portal.md)]

## <a name="set-permissions-for-your-app"></a>Definición de permisos para la aplicación

Esta sección registra la aplicación de ejemplo en Azure Active Directory (AAD), para que pueda acceder a la instancia de Digital Twins. Si ya tiene un registro de aplicación de AAD, puede reutilizarlo en el ejemplo, pero asegúrese de que está configurado como se menciona en esta sección. 

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-permissions.md)]


## <a name="build-application"></a>Compilación de la aplicación

Compile la aplicación de ocupación mediante estos pasos:

1. Abra un símbolo del sistema y vaya a la carpeta donde se extrajeron los archivos de digital-twins-samples-csharp-master.zip.
1. Ejecute `cd occupancy-quickstart/src`.
1. Ejecute `dotnet restore`.
1. Edite *appSettings.json* para actualizar las siguientes variables:
    - *ClientId*: escriba el *identificador de aplicación* de su registro de aplicación de AAD, mencionado en la sección anterior.
    - *Tenant*: escriba el *identificador de directorio* del inquilino de AAD, también indicado en la sección anterior.
    - *BaseUrl*: la *API de administración* de la dirección URL de la instancia de Digital Twins, que estará en el siguiente formato `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/`. Reemplace los marcadores de posición de esta dirección URL por los valores de la instancia de la sección anterior.

## <a name="provision-graph"></a>Aprovisionamiento del gráfico

En este paso se aprovisiona el grafo espacial de Digital Twins con varios espacios, un dispositivo, dos sensores, una función personalizada y una asignación de roles. El grafo espacial se aprovisionará con el archivo [*provisionSample.yaml*](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/provisionSample.yaml).

1. Ejecute `dotnet run ProvisionSample`.
    >[!NOTE]
    >Se usará la herramienta de inicio de sesión de dispositivos de la CLI de Azure para autenticar al usuario en Azure AD. El usuario debe escribir un código determinado para autenticarse mediante la página de [inicio de sesión de Microsoft](https://microsoft.com/devicelogin). Una vez que se escribe el código, siga los pasos para autenticarse. Cada vez que se ejecuta la herramienta se solicita la autenticación del usuario.
    
    >[!TIP]
    > Si al ejecutar este paso recibe el siguiente error, compruebe que las variables se hayan copiado correctamente. 
    > `EXIT: Unexpected error: The input is not a valid Base-64 string ...`


1. El paso de aprovisionamiento puede tardar varios minutos. También se aprovisionará una instancia de IoT Hub dentro de la instancia de Digital Twins y se ejecutará en bucle hasta que IoTHub alcance Status=`Running`.

    ![Ejemplo de aprovisionamiento][4]

1. Al final de la ejecución, copie el valor de `ConnectionString` del dispositivo para usarlo en el ejemplo de simulador de dispositivos. Copie solo la cadena indicada en la imagen siguiente:

    ![Ejemplo de aprovisionamiento][1]

## <a name="send-sensor-data"></a>Envío de datos de sensor

Puede compilar y ejecutar la aplicación de simulación del sensor mediante los pasos siguientes:

1. Abra un nuevo símbolo del sistema y vaya al proyecto que descargó, en la carpeta digital-twins-samples-csharp-master.
1. Ejecute `cd device-connectivity`.
1. Ejecute `dotnet restore`.
1. Edite *appsettings.json* para actualizar *DeviceConnectionString* con el valor de `ConnectionString` anterior.
1. Ejecute `dotnet run` para comenzar a enviar datos del sensor; verá que se envían al servicio Digital Twins como en la imagen siguiente:

     ![Conectividad de dispositivos][2]

1. Vamos a dejar que se ejecute este simulador para que pueda ver los resultados en paralelo con la acción del paso siguiente. En esta ventana se mostrarán los datos del sensor simulado enviados a Digital Twins y en el siguiente paso se consultarán en tiempo real las habitaciones disponibles con aire fresco.

    >[!TIP]
    > Si al ejecutar este paso recibe el siguiente error, asegúrese de que `DeviceConnectionString` se haya copiado correctamente.  
    > `EXIT: Unexpected error: The input is not a valid Base-64 string ...`

## <a name="find-available-spaces-with-fresh-air"></a>Búsqueda de espacios disponibles con aire fresco

El ejemplo del sensor simula valores de datos aleatorios para dos sensores: movimiento y dióxido de carbono. En nuestro ejemplo, los espacios disponibles con aire fresco se definen por la no presencia en la habitación y un nivel de dióxido de carbono por debajo de 1000 ppm. Si no se cumple la condición, el espacio no está disponible o la calidad del aire es deficiente.

1. Abra el símbolo del sistema que se usó para ejecutar el paso de aprovisionamiento anterior.
1. Ejecute `dotnet run GetAvailableAndFreshSpaces`.
1. Examine este símbolo del sistema y el símbolo del sistema de los datos del sensor en paralelo como se describe a continuación. 
1. Un símbolo del sistema envía datos simulados de movimiento y dióxido de carbono a Digital Twins cada 5 segundos. El otro comando lee el gráfico en tiempo real para encontrar las habitaciones disponibles con aire fresco en función de los datos simulados aleatorios. Se mostrará una de estas condiciones casi en tiempo real en función de los datos del sensor que se enviaron por última vez:
    - Habitaciones disponibles con aire fresco.
    - Ocupada o mala calidad del aire de la habitación.

     ![Obtención de espacios disponibles con aire fresco][3]

Para comprender qué ha sucedido en esta guía de inicio rápido y las API que se han llamado, abra [Visual Studio Code](https://code.visualstudio.com/Download) con el proyecto de área de trabajo de código que se encuentra en digital-twins-samples-csharp (consulte el comando a continuación). Los tutoriales profundizan en el código y le enseñan a modificar los datos de configuración, además de mostrarle las API que se han llamado. Para comprender mejor las API de administración, vaya a su página de Digital Twins Swagger `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net//management/swagger`; o bien, si le resulta más cómodo, vaya a [Digital Twins Swagger](https://docs.westcentralus.azuresmartspaces.net/management/swagger). 

```
<path>\occupancy-quickstart\src>code ..\..\digital-twins-samples.code-workspace
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Los tutoriales incluyen información detallada para los responsables de las instalaciones sobre cómo compilar una aplicación a fin de aumentar la productividad de los ocupantes y utilizar el edificio de forma más eficiente.

Si tiene pensado seguir con los tutoriales, no elimine los recursos que se crearon con esta guía de inicio rápido. Si no va a continuar, use los siguientes pasos para eliminar todos los recursos creados con esta guía de inicio rápido:

1. Elimine la carpeta que se creó al descargar el repositorio de ejemplo.
1. En el menú izquierdo de [Azure Portal](http://portal.azure.com), haga clic en **Todos los recursos** y, luego, seleccione su recurso de Digital Twins. En la parte superior del panel **Todos los recursos**, haga clic en **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido se le ha proporcionado una visión general de un escenario sencillo de búsqueda de habitaciones con buenas condiciones de trabajo. Para un análisis más detallado de este escenario, continúe con este tutorial:

> [!div class="nextstepaction"]
> [Tutorial: Implementación de Azure Digital Twins y configuración de un grafo espacial](tutorial-facilities-setup.md)

<!-- Images -->
[1]: media/quickstart-view-occupancy-dotnet/digital-twins-provision-sample.png
[2]: media/quickstart-view-occupancy-dotnet/digital-twins-device-connectivity.png
[3]: media/quickstart-view-occupancy-dotnet/digital-twins-get-available.png
[4]: media/quickstart-view-occupancy-dotnet/digital-twins-provision-sample1.png
