---
title: Implementación de Azure Digital Twins | Microsoft Docs
description: Aprenda a implementar una instancia de Azure Digital Twins y a configurar los recursos espaciales mediante los pasos de este tutorial.
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: tutorial
ms.date: 10/15/2018
ms.author: dkshir
ms.openlocfilehash: 9ca8a9de9a286d4b14dae4a822f3e9baf4747c60
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49363340"
---
# <a name="tutorial-deploy-azure-digital-twins-and-configure-a-spatial-graph"></a>Tutorial: Implementación de Azure Digital Twins y configuración de un grafo espacial

El servicio Azure Digital Twins permite unir personas, lugares y dispositivos en un sistema espacial coherente. En esta serie de tutoriales se muestra cómo usar Azure Digital Twins para detectar la ocupación de la sala con condiciones óptimas de temperatura y calidad del aire. Estos tutoriales le guiarán a través de una aplicación de consola .NET para crear un escenario de un edificio de oficinas de varias plantas y varias salas en cada planta. Las salas contienen dispositivos, con sensores conectados, que detectan movimiento, la temperatura ambiente y la calidad del aire. Aprenderá a replicar las entidades y áreas físicas en el edificio como objetos digitales mediante el servicio Digital Twins. Simulará los eventos de los dispositivos mediante otra aplicación de consola. Luego, aprenderá a supervisar los eventos procedentes de estas áreas físicas y las entidades casi en tiempo real. Los administradores de las oficinas pueden usar esta información para ayudar a los empleados que trabajan en el edificio a reservar salas de reuniones con condiciones óptimas. Los administradores de las instalaciones de la oficina pueden usar la configuración no solo para averiguar las tendencias de uso de las salas, sino también para supervisar las condiciones laborales, con el fin de ajustar el mantenimiento.

En el primer tutorial de esta serie, aprenderá a:

> [!div class="checklist"]
> * Implementar Digital Twins
> * Conceder permisos a una aplicación
> * Modificar la aplicación de ejemplo de Digital Twins
> * Aprovisionar un edificio


Estos tutoriales utilizan y modifican los mismos ejemplos que usa la [guía de inicio rápido para buscar salas disponibles](quickstart-view-occupancy-dotnet.md) para proporcionar una cobertura más detallada y exhaustiva de los conceptos.


## <a name="prerequisites"></a>Requisitos previos

- Una suscripción de Azure. Si no tiene una cuenta de Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

- Los ejemplos de Digital Twins que se usan en estos tutoriales están escritos en C#. Para compilar y ejecutar el ejemplo, asegúrese de que está instalada en el equipo de desarrollo la [versión 2.1.403 del SDK de .NET Core, o cualquier versión superior](https://www.microsoft.com/net/download). Para comprobar que está instalada la versión correcta, ejecute `dotnet --version` en una ventana de comandos.

- [Visual Studio Code](https://code.visualstudio.com/) para explorar el código de ejemplo. 

<a id="deploy" />

## <a name="deploy-digital-twins"></a>Implementación de Digital Twins

Use los pasos de esta sección para crear una nueva instancia del servicio Digital Twins. No se puede crear más de una instancia por suscripción; si ya hay una en ejecución, vaya a la siguiente sección. 

[!INCLUDE [create-digital-twins-portal](../../includes/create-digital-twins-portal.md)]


<a id="permissions" />

## <a name="grant-permissions-to-your-app"></a>Concesión de permisos a una aplicación

Digital Twins usa [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) para controlar el [acceso de lectura y escritura](../active-directory/develop/v1-permissions-and-consent.md) al servicio. Todas las aplicaciones que necesita para conectarse a una instancia de gemelos Digital, deben estar registradas en Azure Active Directory. Los pasos de esta sección muestran cómo crear una aplicación de ejemplo.

Si ya tiene el *registro de una aplicación*, puede reutilizarlo para el ejemplo. Sin embargo, en esta sección tiene la posibilidad de asegurarse de que el registro de la aplicación está configurado correctamente.

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-permissions.md)]


## <a name="configure-digital-twins-sample"></a>Configuración del ejemplo de Digital Twins

En esta sección recorrerá una aplicación de Digital Twins que se comunica con las [API REST de Digital Twins](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index). 

### <a name="download-the-sample"></a>Descarga del ejemplo
Si ya tiene los ejemplos descargados para la [guía de inicio rápido para la búsqueda de salas disponibles](quickstart-view-occupancy-dotnet.md), puede omitir estos pasos.

1. Descargue los [ejemplos de .NET de Digital Twins](https://github.com/Azure-Samples/digital-twins-samples-csharp/archive/master.zip). 
2. Extraiga el contenido de la carpeta ZIP en el equipo. 

### <a name="explore-the-sample"></a>Exploración del ejemplo
En la carpeta de ejemplo, abra el archivo **_digital-twins-samples-csharp\digital-twins-samples.code-workspace_**  en Visual Studio Code. Esta solución contiene dos proyectos: 

1. El ejemplo de aprovisionamiento **_occupancy-quickstart_** le permite configurar y aprovisionar un [grafo de inteligencia espacial](concepts-objectmodel-spatialgraph.md#graph), que es la imagen digitalizada de los espacios físicos y los recursos que contienen. Usa un [modelo de objetos](concepts-objectmodel-spatialgraph.md#model) que define los objetos de un edificio inteligente. Para obtener una lista completa de los objetos y las API REST de Digital Twins, visite [esta documentación de la API REST](https://docs.westcentralus.azuresmartspaces.net/management/swagger) o la dirección URL de **API Management** que se creó para [su instancia](#deploy).

   Para explorar el ejemplo para ver cómo se comunica con su instancia de Digital Twins, puede comenzar con la carpeta **_src\actions_**. Los archivos de esta carpeta implementan los comandos que va a utilizar en estos tutoriales:
    - el archivo *provisionSample.cs* muestra cómo aprovisionar el grafo espacial,
    - el archivo *getSpaces.cs* obtiene información acerca de los espacios aprovisionados,
    - el archivo *getAvailableAndFreshSpaces.cs* obtiene los resultados de una función personalizada llamada función definida por el usuario, y
    - el archivo *createEndpoints.cs* crea puntos de conexión que interactúan con otros servicios.

1. La **_conectividad de los dispositivos_** del ejemplo de simulación simula los datos del sensor y los envía a la instancia de IoT Hub aprovisionada para la instancia de Digital Twin. Este ejemplo lo va a usar en [el tutorial después de que haya aprovisionado su grafo espacial](tutorial-facilities-udf.md#simulate). Los identificadores de sensores y dispositivos que se usan para configurar este ejemplo deben ser los mismos que usará para aprovisionar su grafo.

### <a name="configure-the-provisioning-sample"></a>Configuración del ejemplo de aprovisionamiento
1. Abra una ventana de comandos y vaya al ejemplo descargado. Ejecute el siguiente comando:

    ```cmd/sh
    cd occupancy-quickstart/src
    ```

1. Restaure las dependencias en el proyecto de ejemplo mediante la ejecución de este comando:

    ```cmd/sh
    dotnet restore
    ```

1. En Visual Studio Code, abra el archivo **_appSettings.json_** que pertenece al proyecto **occupancy-quickstart** y actualice los siguientes valores:
    1. *ClientId*: escriba el **identificador de aplicación** de su registro de aplicación de AAD, mencionado en la sección para [establecer los permisos de la aplicación](#permissions).
    1. *Tenant*: escriba el **identificador de directorio** de su [inquilino de AAD](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant), que también se ha mencionado en la sección para [establecer los permisos de la aplicación](#permissions).
    1. *BaseUrl*: escriba la dirección URL de la instancia de Digital Twins. Para obtener esta dirección URL, sustituya los marcadores de posición de esta dirección URL por los valores de su instancia: **https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/**. Esta dirección URL también se puede obtener mediante la modificación de la dirección URL de **Management API** desde [la sección de implementación](#deploy) y reemplace **swagger /** por **api/v1.0/**.

1. Vea una lista de las características de Digital Twins que puede explorar utilizando el ejemplo, para lo que debe ejecutar el comando siguiente.

    ```cmd/sh
    dotnet run
    ```

<a id="provision-spaces" />

## <a name="understand-provisioning-process"></a>Información acerca del proceso de aprovisionamiento
En esta sección se muestra la forma en que el ejemplo aprovisiona un grafo espacial de un edificio. 

En Visual Studio Code, vaya a la carpeta **_occupancy-quickstart\src\actions_** y abra el archivo *provisionSample.cs*. Anote la siguiente función:

```csharp
public static async Task<IEnumerable<ProvisionResults.Space>> ProvisionSample(HttpClient httpClient, ILogger logger)
{
    IEnumerable<SpaceDescription> spaceCreateDescriptions;
    using (var r = new StreamReader("actions/provisionSample.yaml"))
    {
        spaceCreateDescriptions = await GetProvisionSampleTopology(r);
    }

    var results = await CreateSpaces(httpClient, logger, spaceCreateDescriptions, Guid.Empty);

    Console.WriteLine($"Completed Provisioning: {JsonConvert.SerializeObject(results, Formatting.Indented)}");

    return results;
}

```

Esta función usa el archivo *provisionSample.yaml* en la misma carpeta. Abra este archivo y anote la jerarquía de un edificio de oficinas: la *ubicación*, la *planta*, el *área*y las *salas*. Todos estos espacios físicos pueden contener *dispositivos* y *sensores*. Cada entrada tiene un valor de `type` predefinido, por ejemplo, *Floor* o *Room*. 

El archivo *yaml* del ejemplo muestra un grafo que mediante el modelo de objetos `Default` de Digital Twins. Este modelo proporciona nombres genéricos para la mayoría de los tipos (por ejemplo, Temperatura para SensorDataType, Mapa para SpaceBlobType) y tipos de espacios (por ejemplo, Sala con los subtipos FocusRoom, ConferenceRoom, etc.), lo que es suficiente para un edificio. Si tuviera que crear un grafo espacial para una ubicación de un tipo diferente, como por ejemplo una fábrica, es posible que necesite un otro modelo de objetos. Para averiguar los modelos que están disponibles para su uso, ejecute el comando `dotnet run GetOntologies` en la línea de comandos del ejemplo de aprovisionamiento. Para más información acerca de los grafos espaciales y los modelos de objetos, lea [Understanding Digital Twins object model and spatial intelligence graph](concepts-objectmodel-spatialgraph.md) (Información acerca de los modelos de objetos y el grafo de inteligencia espacial de Digital). 

### <a name="modify-sample-spatial-graph"></a>Modificación del espacial de ejemplo
El archivo *provisionSample.yaml* contiene los siguientes nodos:

- **resources**: el nodo `resources` crea un recurso de IoT Hub para comunicarse con los dispositivos en la instalación. Una instancia de IoT Hub en el nodo raíz del grafo, puede comunicarse con todos los dispositivos y sensores del grafo.  

- **spaces**: en el modelo de objetos de Digital Twins `spaces` representa las ubicaciones físicas. Cada espacio tiene un valor de `Type`, por ejemplo, *Region*, *Venue* o *Customer*, y un nombre descriptivo en `Name`. Los espacios pueden pertenecer a otros espacios, lo que crea una estructura jerárquica. El archivo *provisionSample.yaml* tiene un grafo espacial de un edificio imaginario. Observe el anidamiento lógico de los espacios del tipo `Floor` dentro de `Venue`, `Area` en una planta y los nodos de `Room` en un área. 

- **devices**: los espacios pueden contener `devices`, que son entidades físicas o virtuales que administran varios sensores. Por ejemplo, un dispositivo puede ser el teléfono de un usuario, un pod de un sensor de Raspberry Pi, una puerta de enlace, etc. En el edificio imaginario del ejemplo, observe que la sala llamada *Focus Room* contiene un dispositivo *Raspberry Pi 3 A1*. Cada nodo de dispositivo se identifica mediante un valor de `hardwareId` único, que está codificado de forma rígida en el ejemplo. Para configurar este ejemplo para una producción real, reemplácelos por los valores de su configuración.  

- **sensors**: un dispositivo puede contener varios `sensors`, que pueden detectar y registrar cambios físicos, como temperatura, movimiento, nivel de batería, et. Cada nodo de sensor se identifica de forma única mediante `hardwareId`, que aquí está codificado de forma rígida. Para una aplicación real, reemplácelos por los identificadores únicos de los sensores de su configuración. El archivo *provisionSample.yaml* tiene dos sensores para registrar los valores de *Motion* y *CarbonDioxide*. Para agregar otro sensor para registrar el valor de *Temperature*, agregue las líneas siguientes debajo de las líneas del sensor CarbonDioxide. Tenga en cuenta que en el archivo *provisionSample.yaml* aparecen como líneas con comentarios; para que dejen de ser comentarios no tiene más que quitar el carácter '#' que hay al principio de cada línea. 

    ```yaml
            - dataType: Temperature
              hardwareId: SAMPLE_SENSOR_TEMPERATURE
    ```
    > [!NOTE]
    > Asegúrese de que las claves `dataType` y `hardwareId` se alinean con las instrucciones anteriores a este fragmento de código. Asegúrese también de que el editor no reemplaza los espacios por caracteres de tabulación. 

Guarde el archivo *provisionSample.yaml* y ciérrelo. En el siguiente tutorial, agregará más información a este archivo y, después, aprovisionará la creación del edificio del ejemplo de Azure Digital Twins.


## <a name="clean-up-resources"></a>Limpieza de recursos

Si desea dejar de explorar Azure Digital Twins aquí, elimine los recursos creados en este tutorial:

1. En el menú izquierdo de [Azure Portal](http://portal.azure.com), haga clic en **Todos los recursos**, seleccione el grupo de recurso de Digital Twins y haga clic en **Eliminar**.
2. Si lo necesita, también puede eliminar la aplicación de ejemplo de su equipo de trabajo. 


## <a name="next-steps"></a>Pasos siguientes

Pase al siguiente tutorial de la serie, donde aprenderá a implementar una lógica personalizada para supervisar las condiciones del edificio de ejemplo. 
> [!div class="nextstepaction"]
> [Tutorial: Aprovisionamiento del edificio y supervisión de las condiciones de trabajo](tutorial-facilities-udf.md)

