---
title: 'Migración de los datos de caras entre distintas suscripciones: Face API'
titleSuffix: Azure Cognitive Services
description: En esta guía se muestra cómo migrar los datos de caras almacenados de una suscripción de Face API a otra.
services: cognitive-services
author: lewlu
manager: cgronlun
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: lewlu
ms.openlocfilehash: 30ceb0e396597530071c70c4448761d914acb4ac
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2019
ms.locfileid: "59548411"
---
# <a name="migrate-your-face-data-to-a-different-face-subscription"></a>Migración de los datos de caras a una suscripción de Face distinta

En esta guía se muestra cómo migrar datos de caras (como un objeto **PersonGroup** de caras guardado) a una suscripción de Face API distinta mediante la característica Instantánea. Esto le permite evitar tener que crear y entrenar repetidamente un objeto **PersonGroup** o un objeto **FaceList** cuando migre o expanda las operaciones. Por ejemplo, puede que haya creado un objeto **PersonGroup** mediante una suscripción de evaluación gratuita y que ahora quiera migrarlo a la suscripción de pago, o bien puede que necesite sincronizar datos de caras en distintas regiones para realizar una operación empresarial de gran tamaño.

Esta misma estrategia de migración también se aplica a los objetos **LargePersonGroup** y **LargeFaceList**. Si no conoce los conceptos que aparecen en esta guía, consultes las definiciones del [glosario](../Glossary.md). Esta guía usa la biblioteca cliente .NET de Face API con C#.

## <a name="prerequisites"></a>Requisitos previos

- Dos claves de suscripción de Face API (una con los datos existentes y otra a la cual migrar). Siga las instrucciones de [Creación de una cuenta de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) para suscribirse al servicio Face API y obtener la clave.
- La cadena de id. de suscripción de Face API correspondiente a la suscripción de destino (que se encuentra en la hoja **Información general** de Azure Portal). 
- Cualquier edición de [Visual Studio 2015 o 2017](https://www.visualstudio.com/downloads/).


## <a name="create-the-visual-studio-project"></a>Creación del proyecto de Visual Studio

Esta guía usará una aplicación de consola simple para ejecutar la migración de datos de caras. Para realizar una implementación completa, consulte el [ejemplo de instantánea de Face API](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceApiSnapshotSample/FaceApiSnapshotSample) en GitHub.

1. En Visual Studio, cree un proyecto de **Aplicación de consola (.NET Framework)** y asígnele el nombre **FaceApiSnapshotSample**. 
1. Obtenga los paquetes NuGet requeridos. En el Explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione **Administrar paquetes de NuGet**. Haga clic en la pestaña **Examinar** y seleccione **Incluir versión preliminar**; a continuación, busque e instale el paquete siguiente:
    - [Microsoft.Azure.CognitiveServices.Vision.Face 2.3.0-preview](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.2.0-preview)


## <a name="create-face-clients"></a>Creación de clientes de Face

En el método **Main** de *Program.cs*, cree dos instancias de **[FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet)** para las suscripciones de origen y destino. En este ejemplo, usaremos una suscripción de Face de la región de Asia Oriental como origen y una suscripción de Oeste de EE. UU. como destino. Esto mostrará cómo migrar datos de una región de Azure a otra. Si las suscripciones están en distintas regiones, deberá cambiar las cadenas `Endpoint`.

```csharp
var FaceClientEastAsia = new FaceClient(new ApiKeyServiceClientCredentials("<East Asia Subscription Key>"))
    {
        Endpoint = "https://southeastasia.api.cognitive.microsoft.com/>"
    };

var FaceClientWestUS = new FaceClient(new ApiKeyServiceClientCredentials("<West US Subscription Key>"))
    {
        Endpoint = "https://westus.api.cognitive.microsoft.com/"
    };
```

Tendrá que rellenar los valores de las claves de suscripción y las direcciones URL de los puntos de conexión para las suscripciones de origen y destino.


## <a name="prepare-a-persongroup-for-migration"></a>Preparación de un objeto PersonGroup para la migración

Necesita el identificador del objeto **PersonGroup** en la suscripción de origen para migrarlo a la suscripción de destino. Use el método **[PersonGroupOperationsExtensions.ListAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.persongroupoperationsextensions.listasync?view=azure-dotnet)** para recuperar una lista de los objetos **PersonGroup** y, luego, obtenga la propiedad **[PersonGroup.PersonGroupId](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.persongroup.persongroupid?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Vision_Face_Models_PersonGroup_PersonGroupId)**. Este proceso será distinto en función de los objetos **PersonGroup** que tenga. En esta guía, el identificador del objeto **PersonGroup** de origen se almacena en `personGroupId`.

> [!NOTE]
> El [código de ejemplo](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceApiSnapshotSample/FaceApiSnapshotSample) crea y entrena un nuevo objeto **PersonGroup** para la migración pero, en la mayoría de los casos, ya debería tener un objeto **PersonGroup** para usarlo.

## <a name="take-snapshot-of-persongroup"></a>Instantánea de un objeto PersonGroup

Una instantánea es el almacenamiento remoto temporal de ciertos tipos de datos de Face. Funciona como una especie de Portapapeles para copiar datos de una suscripción a otra. En primer lugar, el usuario "toma" una instantánea de los datos de la suscripción de origen y luego la "aplica" a un objeto de datos nuevo en la suscripción de destino.

Use la instancia **FaceClient** de la suscripción de origen para tomar una instantánea del objeto **PersonGroup**, mediante **[TakeAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.snapshotoperationsextensions.takeasync?view=azure-dotnet)** con el identificador del objeto **PersonGroup** y el identificador de la suscripción de destino. Si tiene varias suscripciones de destino, puede agregarlas como entradas de matriz en el tercer parámetro.

```csharp
var takeSnapshotResult = await FaceClientEastAsia.Snapshot.TakeAsync(
    SnapshotObjectType.PersonGroup,
    personGroupId,
    new[] { "<Azure West US Subscription ID>" /* Put other IDs here, if multiple target subscriptions wanted */ });
```

> [!NOTE]
> El proceso de tomar y aplicar instantáneas no interrumpirá ninguna llamada normal al objeto **PersonGroup** (o **FaceList**) de origen o destino. Sin embargo, no se recomienda realizar llamadas simultáneas que cambian el objeto de origen ([llamadas de administración FaceList](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.facelistoperations?view=azure-dotnet) o ["Train" del grupo de personas](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.persongroupoperations?view=azure-dotnet) llamar, por ejemplo), ya que es posible que la operación de instantánea ejecutar antes o después de esas operaciones o pueden producirse errores.

## <a name="retrieve-the-snapshot-id"></a>Recuperación del identificador de la instantánea

La instantánea que toma el método es asincrónica, por lo que deberá esperar su finalización (no se puede cancelar las operaciones de instantánea). En este código, el método `WaitForOperation` supervisa la llamada asincrónica y controla el estado cada 100 ms. Una vez que se completa la operación, podrá recuperar el identificador de una operación. Para obtenerlo, puede analizar el campo `OperationLocation`. 

```csharp
var takeOperationId = Guid.Parse(takeSnapshotResult.OperationLocation.Split('/')[2]);
var operationStatus = await WaitForOperation(FaceClientEastAsia, takeOperationId);
```

Un valor `OperationLocation` típico tendrá este aspecto:

```csharp
"/operations/a63a3bdd-a1db-4d05-87b8-dbad6850062a"
```

El método auxiliar `WaitForOperation` es el siguiente:

```csharp
/// <summary>
/// Waits for the take/apply operation to complete and returns the final operation status.
/// </summary>
/// <returns>The final operation status.</returns>
private static async Task<OperationStatus> WaitForOperation(IFaceClient client, Guid operationId)
{
    OperationStatus operationStatus = null;
    do
    {
        if (operationStatus != null)
        {
            Thread.Sleep(TimeSpan.FromMilliseconds(100));
        }

        // Get the status of the operation.
        operationStatus = await client.Snapshot.GetOperationStatusAsync(operationId);

        Console.WriteLine($"Operation Status: {operationStatus.Status}");
    }
    while (operationStatus.Status != OperationStatusType.Succeeded
            && operationStatus.Status != OperationStatusType.Failed);

    return operationStatus;
}
```

Cuando el estado de la operación se marca como `Succeeded`, puede obtener el identificador de la instantánea si analiza el campo `ResourceLocation` de la instancia **OperationStatus** devuelta.

```csharp
var snapshotId = Guid.Parse(operationStatus.ResourceLocation.Split('/')[2]);
```

Un valor `resourceLocation` típico tendrá este aspecto:

```csharp
"/snapshots/e58b3f08-1e8b-4165-81df-aa9858f233dc"
```

## <a name="apply-snapshot-to-target-subscription"></a>Aplicación de la instantánea en una suscripción de destino

A continuación, cree el nuevo objeto **PersonGroup** en la suscripción de destino mediante un identificador generado aleatoriamente. Luego use la instancia **FaceClient** de la suscripción de destino para aplicar la instantánea en este objeto PersonGroup, pasando el identificador de la instantánea y el identificador de un objeto **PersonGroup** nuevo. 

```csharp
var newPersonGroupId = Guid.NewGuid().ToString();
var applySnapshotResult = await FaceClientWestUS.Snapshot.ApplyAsync(snapshotId, newPersonGroupId);
```


> [!NOTE]
> Un objeto de instantánea solo es válido durante 48 horas. Solo debe tomar una instantánea si pretende usarla para migrar datos poco después.

Una solicitud de aplicación de instantánea devolverá otro identificador de operación. Para obtener este identificador, puede analizar el campo `OperationLocation` de la instancia **applySnapshotResult** devuelta. 

```csharp
var applyOperationId = Guid.Parse(applySnapshotResult.OperationLocation.Split('/')[2]);
```

El proceso de aplicación de la instantánea también es asincrónico, por lo que nuevamente debe usar `WaitForOperation` para esperar que se complete.

```csharp
operationStatus = await WaitForOperation(FaceClientWestUS, applyOperationId);
```

## <a name="test-the-data-migration"></a>Prueba de la migración de datos

Una vez que se aplique la instantánea, el nuevo objeto **PersonGroup** de la suscripción de destino se deberá rellenar con los datos de caras originales. De manera predeterminada, también se copian los resultados del entrenamiento para que el nuevo objeto **PersonGroup** esté preparado para las llamadas de identificación de caras sin que sea necesario un nuevo entrenamiento.

Para probar la migración de datos, puede ejecutar estas operaciones y comparar los resultados que imprimen en la consola.

```csharp
await DisplayPersonGroup(FaceClientEastAsia, personGroupId);
await IdentifyInPersonGroup(FaceClientEastAsia, personGroupId);

await DisplayPersonGroup(FaceClientWestUS, newPersonGroupId);
// No need to retrain the person group before identification,
// training results are copied by snapshot as well.
await IdentifyInPersonGroup(FaceClientWestUS, newPersonGroupId);
```

Use estos métodos auxiliares:

```csharp
private static async Task DisplayPersonGroup(IFaceClient client, string personGroupId)
{
    var personGroup = await client.PersonGroup.GetAsync(personGroupId);
    Console.WriteLine("Person Group:");
    Console.WriteLine(JsonConvert.SerializeObject(personGroup));

    // List persons.
    var persons = await client.PersonGroupPerson.ListAsync(personGroupId);

    foreach (var person in persons)
    {
        Console.WriteLine(JsonConvert.SerializeObject(person));
    }

    Console.WriteLine();
}
```

```csharp
private static async Task IdentifyInPersonGroup(IFaceClient client, string personGroupId)
{
    using (var fileStream = new FileStream("data\\PersonGroup\\Daughter\\Daughter1.jpg", FileMode.Open, FileAccess.Read))
    {
        var detectedFaces = await client.Face.DetectWithStreamAsync(fileStream);

        var result = await client.Face.IdentifyAsync(detectedFaces.Select(face => face.FaceId.Value).ToList(), personGroupId);
        Console.WriteLine("Test identify against PersonGroup");
        Console.WriteLine(JsonConvert.SerializeObject(result));
        Console.WriteLine();
    }
}
```

Ahora puede empezar a usar el nuevo objeto **PersonGroup** en la suscripción de destino. 

Si quiere actualizar nuevamente el objeto **PersonGroup** de destino en el futuro, deberá crear un nuevo objeto **PersonGroup** (con los pasos que aparecen en esta guía) para recibir la instantánea. Una instantánea solo se puede aplicar una sola vez a un objeto **PersonGroup** único.

## <a name="clean-up-resources"></a>Limpieza de recursos

Una vez que termine de migrar los datos de caras, se recomienda que elimine manualmente el objeto de instantánea.

```csharp
await FaceClientEastAsia.Snapshot.DeleteAsync(snapshotId);
```

## <a name="related-topics"></a>Temas relacionados

- [Documentación de referencia de Instantánea (SDK de .NET)](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.snapshotoperations?view=azure-dotnet)
- [Ejemplo de instantánea de Face API](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceApiSnapshotSample/FaceApiSnapshotSample)
- [Incorporación de caras](how-to-add-faces.md)
- [Detección de caras en imágenes](HowtoDetectFacesinImage.md)
- [Identificación de caras en imágenes](HowtoIdentifyFacesinImage.md)
