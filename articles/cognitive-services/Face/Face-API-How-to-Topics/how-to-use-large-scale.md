---
title: 'Ejemplo: Uso de la característica a gran escala - Face API'
titleSuffix: Azure Cognitive Services
description: Use la característica a gran escala en Face API.
services: cognitive-services
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: sample
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: e8bbf78da84ddb77ce956e37f91be46e96144991
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46123086"
---
# <a name="example-how-to-use-the-large-scale-feature"></a>Ejemplo: Uso de la característica a gran escala

Esta guía es un artículo avanzado sobre la migración de código para escalar verticalmente desde PersonGroup y FaceList a LargePersonGroup y LargeFaceList, respectivamente.
En esta guía se muestra el proceso de migración y se da por hecho que se conoce el uso básico de PersonGroup y FaceList.
Para familiarizarse con las operaciones básicas, consulte otros tutoriales como [Identificación de caras en imágenes](HowtoIdentifyFacesinImage.md),

Face API ha publicado recientemente dos características para habilitar escenarios a gran escala, LargePersonGroup y LargeFaceList, conocidas como operaciones a gran escala.
LargePersonGroup puede contener hasta 1 000 000 personas, cada una con un máximo de 248 caras, y LargeFaceList puede contener hasta 1 000 000 de caras.

Las operaciones a gran escala son similares a las de PersonGroup y FaceList convencionales, pero tienen algunas diferencias importantes debido a la nueva arquitectura.
En esta guía se muestra el proceso de migración y se da por hecho que se conoce el uso básico de PersonGroup y FaceList.
Los ejemplos están escritos en C# con la biblioteca cliente de Face API.

Para habilitar la búsqueda de caras para Identification y FindSimilar a gran escala, debe incorporar una operación Train para preprocesar LargeFaceList y LargePersonGroup.
El tiempo de entrenamiento varía de unos segundos a media hora según la capacidad real.
Durante el período de aprendizaje, aún se pueden realizar las operaciones Identification y FindSimilar si antes se ha realizado un entrenamiento correcto.
Sin embargo, el inconveniente es que las nuevas personas o caras agregadas no aparecerán en el resultado hasta que se complete una nueva migración posterior al entrenamiento a gran escala.

## <a name="concepts"></a>Conceptos

Si no está familiarizado con los siguiente conceptos de esta guía, encontrará las definiciones en el [glosario](../Glossary.md):

- LargePersonGroup: colección de personas con capacidad para hasta 1 000 000.
- LargeFaceList: colección de caras con capacidad para hasta 1 000 000.
- Train: proceso previo para asegurar el rendimiento de las operaciones Identification/FindSimilar.
- Identification: identificar una o más caras en PersonGroup o LargePersonGroup.
- FindSimilar: buscar caras similares en FaceList o LargeFaceList.

## <a name="step-1-authorize-the-api-call"></a>Paso 1: Autorización de la llamada API

Cuando se utiliza la biblioteca cliente de Face API, la clave de suscripción y el punto de conexión de la suscripción se pasan en el constructor de la clase FaceServiceClient. Por ejemplo: 

```CSharp
string SubscriptionKey = "<Subscription Key>";
// Use your own subscription endpoint corresponding to the subscription key.
string SubscriptionRegion = "https://westcentralus.api.cognitive.microsoft.com/face/v1.0/";
FaceServiceClient FaceServiceClient = new FaceServiceClient(SubscriptionKey, SubscriptionRegion);
```

La clave de la suscripción con el punto de conexión correspondiente se puede obtener desde la página Marketplace de Azure Portal.
Consulte [Suscripciones](https://azure.microsoft.com/services/cognitive-services/directory/vision/).

## <a name="step-2-code-migration-in-action"></a>Paso 2: Migración del código en acción

Esta sección se centra solo en la migración de la implementación de PersonGroup/FaceList a LargePersonGroup/LargeFaceList.
Aunque LargePersonGroup/LargeFaceList difieren de PersonGroup/FaceList en el diseño y la implementación interna, las interfaces de la API son similares por motivos de compatibilidad con versiones anteriores.

No se admite la migración de datos; en su lugar, tendrá que volver a crear la colección LargePersonGroup/LargeFaceList.

## <a name="step-21-migrate-persongroup-to-largepersongroup"></a>Paso 2.1: Migración de PersonGroup a LargePersonGroup

La migración de PersonGroup a LargePersonGroup es muy sencilla porque comparten exactamente las mismas operaciones en el nivel de grupo.

Para la implementación de PersonGroup/Person, solo es necesario cambiar las rutas de acceso de las API o la clase/módulo del SDK a LargePersonGroup y LargePersonGroup Person.

En cuanto a la migración de los datos, consulte [Incorporación de caras](how-to-add-faces.md) para más información.

## <a name="step-22-migrate-facelist-to-largefacelist"></a>Paso 2.2: Migración de FaceList a LargeFaceList

| API de FaceList | API de LargeFaceList |
|:---:|:---:|
| Crear | Crear |
| Eliminar | Eliminar |
| Obtener | Obtener |
| Enumerar | Enumerar |
| Actualizar | Actualizar |
| - | Train |
| - | Get Training Status |

La tabla anterior es una comparación de las operaciones en el nivel de lista entre FaceList y LargeFaceList.
Tal y como se muestra, LargeFaceList incluye nuevas operaciones (Train y Get Training Status) en comparación con FaceList.
El entrenamiento de LargeFaceList es una condición previa de la operación [FindSimilar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), mientras que no es necesario para FaceList.
El fragmento de código siguiente es una función auxiliar para esperar el entrenamiento de LargeFaceList.

```CSharp
/// <summary>
/// Helper function to train LargeFaceList and wait for finish.
/// </summary>
/// <remarks>
/// The time interval can be adjusted considering the following factors:
/// - The training time which depends on the capacity of the LargeFaceList.
/// - The acceptable latency for getting the training status.
/// - The call frequency and cost.
///
/// Estimated training time for LargeFaceList in different scale:
/// -     1,000 faces cost about  1 to  2 seconds.
/// -    10,000 faces cost about  5 to 10 seconds.
/// -   100,000 faces cost about  1 to  2 minutes.
/// - 1,000,000 faces cost about 10 to 30 minutes.
/// </remarks>
/// <param name="largeFaceListId">The Id of the LargeFaceList for training.</param>
/// <param name="timeIntervalInMilliseconds">The time interval for getting training status in milliseconds.</param>
/// <returns>A task of waiting for LargeFaceList training finish.</returns>
private static async Task TrainLargeFaceList(
    string largeFaceListId,
    int timeIntervalInMilliseconds = 1000)
{
    // Trigger a train call.
    await FaceServiceClient.TrainLargeFaceListAsync(largeFaceListId);

    // Wait for training finish.
    while (true)
    {
        Task.Delay(timeIntervalInMilliseconds).Wait();
        var status = await FaceServiceClient.GetLargeFaceListTrainingStatusAsync(largeFaceListId);

        if (status.Status == Status.Running)
        {
            continue;
        }
        else if (status.Status == Status.Succeeded)
        {
            break;
        }
        else
        {
            throw new Exception("The train operation is failed!");
        }
    }
}
```

Anteriormente, un uso típico de FaceList con adición de caras y FindSimilar sería:

```CSharp
// Create a FaceList.
const string FaceListId = "myfacelistid_001";
const string FaceListName = "MyFaceListDisplayName";
const string ImageDir = @"/path/to/FaceList/images";
FaceServiceClient.CreateFaceListAsync(FaceListId, FaceListName).Wait();

// Add Faces to the FaceList.
Parallel.ForEach(
    Directory.GetFiles(ImageDir, "*.jpg"),
    async imagePath =>
        {
            using (Stream stream = File.OpenRead(imagePath))
            {
                await FaceServiceClient.AddFaceToFaceListAsync(FaceListId, stream);
            }
        });

// Perform FindSimilar.
const string QueryImagePath = @"/path/to/query/image";
var results = new List<SimilarPersistedFace[]>();
using (Stream stream = File.OpenRead(QueryImagePath))
{
    var faces = FaceServiceClient.DetectAsync(stream).Result;
    foreach (var face in faces)
    {
        results.Add(await FaceServiceClient.FindSimilarAsync(face.FaceId, FaceListId, 20));
    }
}
```

Al migrar a LargeFaceList, se convertirá en lo siguiente:

```CSharp
// Create a LargeFaceList.
const string LargeFaceListId = "mylargefacelistid_001";
const string LargeFaceListName = "MyLargeFaceListDisplayName";
const string ImageDir = @"/path/to/FaceList/images";
FaceServiceClient.CreateLargeFaceListAsync(LargeFaceListId, LargeFaceListName).Wait();

// Add Faces to the LargeFaceList.
Parallel.ForEach(
    Directory.GetFiles(ImageDir, "*.jpg"),
    async imagePath =>
        {
            using (Stream stream = File.OpenRead(imagePath))
            {
                await FaceServiceClient.AddFaceToLargeFaceListAsync(LargeFaceListId, stream);
            }
        });

// Train() is newly added operation for LargeFaceList.
// Must call it before FindSimilarAsync() to ensure the newly added faces searchable.
await TrainLargeFaceList(LargeFaceListId);

// Perform FindSimilar.
const string QueryImagePath = @"/path/to/query/image";
var results = new List<SimilarPersistedFace[]>();
using (Stream stream = File.OpenRead(QueryImagePath))
{
    var faces = FaceServiceClient.DetectAsync(stream).Result;
    foreach (var face in faces)
    {
        results.Add(await FaceServiceClient.FindSimilarAsync(face.FaceId, largeFaceListId: LargeFaceListId));
    }
}
```

Tal y como se mostró anteriormente, la administración de datos y la parte FindSimilar son prácticamente iguales.
La única excepción es que debe realizarse una operación de entrenamiento totalmente nueva en LargeFaceList para que FindSimilar funcione.

## <a name="step-3-train-suggestions"></a>Paso 3: Entrenamiento de sugerencias

Aunque la operación Train acelera las operaciones [FindSimilar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) e [Identification](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), el tiempo de entrenamiento sufre especialmente cuando se aplica a gran escala.
En la tabla siguiente se indica el tiempo de entrenamiento estimado de las diferentes escalas:

| Escala (caras o personas) | Tiempo de entrenamiento estimado |
|:---:|:---:|
| 1000 | 1-2 s |
| 10 000 | 5-10 s |
| 100 000 | 1 - 2 min |
| 1 000 000 | 10 - 30 min |

Para aprovechar mejor la característica a gran escala, se recomienda tener en cuenta algunas estrategias.

## <a name="step-31-customize-time-interval"></a>Paso 3.1: Personalización del intervalo de tiempo

Tal y como se muestra en `TrainLargeFaceList()`, hay un valor de `timeIntervalInMilliseconds` para retrasar el proceso de comprobación del estado del entrenamiento infinito.
Cuando LargeFaceList tiene más caras, el uso de un intervalo mayor reduce el número de llamadas y el costo.
El intervalo de tiempo debe personalizarse según la capacidad prevista de LargeFaceList.

La misma estrategia se aplica también a LargePersonGroup.
Por ejemplo, al entrenar una colección LargePersonGroup con 1 000 000 de personas, el valor e `timeIntervalInMilliseconds` podría ser 60 000 (también conocido como intervalo de 1 minuto).

## <a name="step-32-small-scale-buffer"></a>Paso 3.2: Búfer a pequeña escala

Las búsquedas de personas o caras en LargePersonGroup/LargeFaceList solo se pueden realizar después del entrenamiento.
En un escenario dinámico, se agregan constantemente nuevas personas y caras, que deben poder buscarse inmediatamente, aunque el entrenamiento podría tardar más tiempo del deseado.
Para mitigar este problema, puede usar una colección LargePersonGroup/LargeFaceList a pequeña escala adicional como búfer solo para las entradas recién agregadas.
El entrenamiento de este búfer es más rápido porque su tamaño es mucho menor, y las búsquedas inmediatas en este búfer temporal deberían funcionar.
Para usar este búfer en combinación con el entrenamiento en las colecciones LargePersonGroup/LargeFaceList maestras, ejecute el entrenamiento de las colecciones maestras a intervalos más largos, por ejemplo, a media noche o diariamente.

Flujo de trabajo de ejemplo:
1. Cree una colección LargePersonGroup/LargeFaceList maestra (colección maestra) y una colección LargePersonGroup/LargeFaceList búfer (colección búfer). La colección búfer es solo para las personas o caras recién agregadas.
1. Agregue las nuevas personas o caras a la colección maestra y a la colección búfer.
1. Entrene solo la colección búfer a intervalos más cortos para garantizar que las entradas recién agregadas surtan efecto.
1. Llame a Identification/FindSimilar en la colección maestra y la colección búfer, y combine los resultados.
1. Cuando el tamaño de la colección búfer aumente hasta cierto punto o a una hora de inactividad del sistema, cree una nueva colección búfer y entrene la colección maestra.
1. Elimine la colección búfer antigua después de que termine el entrenamiento de la colección maestra.

## <a name="step-33-standalone-training"></a>Paso 3.3: Entrenamiento independiente

Si es aceptable una latencia relativamente larga, no es necesario desencadenar la operación de entrenamiento justo después de agregar datos nuevos.
En su lugar, la operación de entrenamiento se puede separar de la lógica principal y desencadenar periódicamente.
Esta estrategia es adecuada para escenarios dinámicos con una latencia aceptable y se puede aplicar a escenarios estáticos para reducir aún más la frecuencia del entrenamiento.

Supongamos que hay una función `TrainLargePersonGroup` similar a `TrainLargeFaceList`.
Una implementación típica de entrenamiento independiente en LargePersonGroup mediante la invocación de la clase [`Timer`](https://msdn.microsoft.com/library/system.timers.timer(v=vs.110).aspx)
en `System.Timers` sería:

```CSharp
private static void Main()
{
    // Create a LargePersonGroup.
    const string LargePersonGroupId = "mylargepersongroupid_001";
    const string LargePersonGroupName = "MyLargePersonGroupDisplayName";
    FaceServiceClient.CreateLargePersonGroupAsync(LargePersonGroupId, LargePersonGroupName).Wait();

    // Setup a standalone training at regular intervals.
    const int TimeIntervalForStatus = 1000 * 60; // 1 minute interval for getting training status.
    const double TimeIntervalForTrain = 1000 * 60 * 60; // 1 hour interval for training.
    var trainTimer = new Timer(TimeIntervalForTrain);
    trainTimer.Elapsed += (sender, args) => TrainTimerOnElapsed(LargePersonGroupId, TimeIntervalForStatus);
    trainTimer.AutoReset = true;
    trainTimer.Enabled = true;

    // Other operations like creating persons, adding faces and Identification except for Train.
    // ...
}

private static void TrainTimerOnElapsed(string largePersonGroupId, int timeIntervalInMilliseconds)
{
    TrainLargePersonGroup(largePersonGroupId, timeIntervalInMilliseconds).Wait();
}
```

Para más información sobre la administración de datos y las implementaciones relativas a la identificación, consulte [Incorporación de caras](how-to-add-faces.md) y [Identificación de caras en imágenes](HowtoIdentifyFacesinImage.md).

## <a name="summary"></a>Resumen

En esta guía, ha aprendido a migrar el código existente de PersonGroup/FaceList (no los datos) a LargePersonGroup/LargeFaceList:

- LargePersonGroup y LargeFaceList funcionan de forma similar a PersonGroup/FaceList, salvo que LargeFaceList requiere la operación de entrenamiento.
- Adopte la estrategia de entrenamiento apropiada para la actualización de datos dinámicos en un conjunto de datos a gran escala.

## <a name="related-topics"></a>Temas relacionados

- [Identificación de caras en imágenes](HowtoIdentifyFacesinImage.md)
- [Incorporación de caras](how-to-add-faces.md)
