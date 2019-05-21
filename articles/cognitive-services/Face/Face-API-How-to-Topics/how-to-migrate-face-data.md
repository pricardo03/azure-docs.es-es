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
ms.openlocfilehash: 702aed12860c090e83b997e6b56d56e06b416568
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65913798"
---
# <a name="migrate-your-face-data-to-a-different-face-subscription"></a>Migración de los datos de caras a una suscripción de Face distinta

Esta guía muestra cómo mover los datos de cara, por ejemplo, un objeto de grupo de personas guardado con caras, a una suscripción de Azure Cognitive Services Face API diferentes. Para mover los datos, use la característica de instantáneas. Esta forma se evita tener que crear varias veces y entrenar un objeto de grupo de personas o FaceList al mover o expandir sus operaciones. Por ejemplo, quizás crea un objeto de grupo de personas mediante el uso de una suscripción de prueba gratuita y ahora desea migrarla a su suscripción de pago. O bien, es posible que deba sincronizar datos de cara entre regiones para una operación de grandes empresas.

Esta misma estrategia de migración también se aplica a objetos LargePersonGroup y LargeFaceList. Si no está familiarizado con los conceptos de esta guía, consulte sus definiciones en el [se enfrentan a los conceptos de reconocimiento](../concepts/face-recognition.md) guía. Esta guía usa la biblioteca cliente .NET de Face API con C#.

## <a name="prerequisites"></a>Requisitos previos

Se necesitan los siguientes elementos:

- Dos Face API claves de suscripción, uno con los datos existentes y otro para migrar a. Para suscribirse al servicio de Face API y obtenga su clave, siga las instrucciones de [crear una cuenta de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).
- La cadena de identificador de suscripción de Face API que se corresponde con la suscripción de destino. Para encontrarlo, seleccione **Introducción** en Azure portal. 
- Cualquier edición de [Visual Studio 2015 o 2017](https://www.visualstudio.com/downloads/).

## <a name="create-the-visual-studio-project"></a>Creación del proyecto de Visual Studio

Esta guía usa una aplicación de consola sencilla para ejecutar la migración de datos de cara. Para una implementación completa, consulte el [snapshot, ejemplo Face API](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceApiSnapshotSample/FaceApiSnapshotSample) en GitHub.

1. En Visual Studio, cree un nuevo proyecto de .NET Framework de aplicación de consola. Asígnele el nombre **FaceApiSnapshotSample**.
1. Obtenga los paquetes NuGet requeridos. Haga clic en el proyecto en el Explorador de soluciones y seleccione **administrar paquetes de NuGet**. Seleccione el **examinar** pestaña y seleccione **incluir versión preliminar**. Buscar e instalar el paquete siguiente:
    - [Microsoft.Azure.CognitiveServices.Vision.Face 2.3.0-preview](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.2.0-preview)

## <a name="create-face-clients"></a>Creación de clientes de Face

En el **Main** método *Program.cs*, cree dos [FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet) instancias para las suscripciones de origen y de destino. Este ejemplo usa una suscripción de cara en la región de Asia oriental como el origen y una suscripción de oeste de Estados Unidos como el destino. En este ejemplo se muestra cómo migrar datos desde una región de Azure a otra. Si las suscripciones se encuentran en regiones distintas, cambie la `Endpoint` cadenas.

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

Rellene los valores de clave de suscripción y direcciones URL de punto de conexión para las suscripciones de origen y de destino.


## <a name="prepare-a-persongroup-for-migration"></a>Preparación de un objeto PersonGroup para la migración

Necesitará el identificador del grupo de personas en su suscripción de origen para migrarlos a la suscripción de destino. Use la [PersonGroupOperationsExtensions.ListAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.persongroupoperationsextensions.listasync?view=azure-dotnet) método para recuperar una lista de los objetos de grupo de personas. A continuación, obtenga el [PersonGroup.PersonGroupId](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.persongroup.persongroupid?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Vision_Face_Models_PersonGroup_PersonGroupId) propiedad. Este proceso tiene un aspecto diferente en función de qué grupo de personas de objetos que tiene. En esta guía, el Id. de grupo de personas de origen se almacena en `personGroupId`.

> [!NOTE]
> El [código de ejemplo](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceApiSnapshotSample/FaceApiSnapshotSample) crea y entrena un nuevo grupo de personas para migrar. En la mayoría de los casos, ya debe tener un grupo de personas para usar.

## <a name="take-a-snapshot-of-a-persongroup"></a>Crear una instantánea de un grupo de personas

Una instantánea es un almacenamiento remoto temporal para determinados tipos de datos de cara. Funciona como una especie de Portapapeles para copiar datos de una suscripción a otra. En primer lugar, tomar una instantánea de los datos en la suscripción de origen. A continuación, se aplica a un nuevo objeto de datos en la suscripción de destino.

Utilice la instancia de FaceClient de la suscripción de origen para tomar una instantánea del grupo de personas. Use [TakeAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.snapshotoperationsextensions.takeasync?view=azure-dotnet) con el identificador de grupo de personas y el identificador de. la suscripción de destino Si tiene varias suscripciones de destino, agréguelos como entradas de la matriz en el tercer parámetro.

```csharp
var takeSnapshotResult = await FaceClientEastAsia.Snapshot.TakeAsync(
    SnapshotObjectType.PersonGroup,
    personGroupId,
    new[] { "<Azure West US Subscription ID>" /* Put other IDs here, if multiple target subscriptions wanted */ });
```

> [!NOTE]
> El proceso de tomar y aplicar las instantáneas no interrumpir las llamadas normales con el origen o destino grupos de personas o lista de caras. No realice llamadas simultáneas que cambian el objeto de origen, como [llamadas de administración FaceList](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.facelistoperations?view=azure-dotnet) o ["Train" del grupo de personas](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.persongroupoperations?view=azure-dotnet) llamar, por ejemplo. La operación de instantánea puede ejecutarse antes o después de esas operaciones o puede producirse errores.

## <a name="retrieve-the-snapshot-id"></a>Recuperar el identificador de la instantánea

El método utilizado para tomar instantáneas es asincrónico, por lo que debe esperar su finalización. No se puede cancelar las operaciones de instantánea. En este código, el `WaitForOperation` método supervisa la llamada asincrónica. Comprueba el estado de cada 100 ms. Una vez finalizada la operación, recuperar el identificador de operación mediante el análisis de la `OperationLocation` campo. 

```csharp
var takeOperationId = Guid.Parse(takeSnapshotResult.OperationLocation.Split('/')[2]);
var operationStatus = await WaitForOperation(FaceClientEastAsia, takeOperationId);
```

Una típica `OperationLocation` valor tiene este aspecto:

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

El estado de la operación después de la muestra `Succeeded`, obtener el identificador de la instantánea mediante el análisis de la `ResourceLocation` campo de la instancia devuelta de OperationStatus.

```csharp
var snapshotId = Guid.Parse(operationStatus.ResourceLocation.Split('/')[2]);
```

Una típica `resourceLocation` valor tiene este aspecto:

```csharp
"/snapshots/e58b3f08-1e8b-4165-81df-aa9858f233dc"
```

## <a name="apply-a-snapshot-to-a-target-subscription"></a>Aplicar una instantánea para una suscripción de destino

A continuación, cree el nuevo grupo de personas en la suscripción de destino mediante el uso de un identificador generado aleatoriamente. Utilizamos la instancia de la suscripción de destino FaceClient para aplicar la instantánea a este grupo de personas. Pasar en la instantánea de identificador y el nuevo identificador de grupo de personas.

```csharp
var newPersonGroupId = Guid.NewGuid().ToString();
var applySnapshotResult = await FaceClientWestUS.Snapshot.ApplyAsync(snapshotId, newPersonGroupId);
```


> [!NOTE]
> Un objeto de instantánea es válido solo durante 48 horas. Solo tomar una instantánea si se va a utilizar para la migración de datos poco después.

Una solicitud de instantánea apply devuelve otro identificador de operación. Para obtener este identificador, analizar el `OperationLocation` campo de la instancia devuelta applySnapshotResult. 

```csharp
var applyOperationId = Guid.Parse(applySnapshotResult.OperationLocation.Split('/')[2]);
```

El proceso de aplicación de instantáneas también es asincrónico, por lo que volver a usar `WaitForOperation` para esperar a que finalice.

```csharp
operationStatus = await WaitForOperation(FaceClientWestUS, applyOperationId);
```

## <a name="test-the-data-migration"></a>Prueba de la migración de datos

Después de aplicar la instantánea, el nuevo grupo de personas en la suscripción de destino se rellena con los datos de la cara original. De forma predeterminada, también se copian los resultados del entrenamiento. El nuevo grupo de personas está listo para las llamadas de identificación de caras sin necesidad de reciclaje.

Para probar la migración de datos, ejecute las siguientes operaciones y comparar los resultados que se imprimen en la consola:

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

Ahora puede usar el nuevo grupo de personas en la suscripción de destino. 

Para actualizar el grupo de personas de destino en el futuro, cree un nuevo grupo de personas para recibir la instantánea. Para ello, siga los pasos descritos en esta guía. Un objeto de grupo de personas solo puede tener una instantánea aplicada solo una vez.

## <a name="clean-up-resources"></a>Limpieza de recursos

Después de migrar datos se enfrentan, elimine manualmente el objeto de instantánea.

```csharp
await FaceClientEastAsia.Snapshot.DeleteAsync(snapshotId);
```

## <a name="next-steps"></a>Pasos siguientes

A continuación, consulte la documentación de referencia de API relevante, exploración de una aplicación de ejemplo que usa la característica de instantáneas, o bien siga una guía de procedimientos para comenzar a usar las otras operaciones de API mencionadas aquí:

- [Documentación de referencia de Instantánea (SDK de .NET)](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.snapshotoperations?view=azure-dotnet)
- [Ejemplo de la instantánea de face API](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceApiSnapshotSample/FaceApiSnapshotSample)
- [Agregar caras](how-to-add-faces.md)
- [Detectar caras en una imagen](HowtoDetectFacesinImage.md)
- [Identificación de caras en una imagen](HowtoIdentifyFacesinImage.md)
