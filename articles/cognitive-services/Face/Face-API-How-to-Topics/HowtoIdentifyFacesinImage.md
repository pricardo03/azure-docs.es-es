---
title: 'Ejemplo: Identificación de caras en imágenes - Face API'
titleSuffix: Azure Cognitive Services
description: Use Face API para identificar caras en imágenes.
services: cognitive-services
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: c61852763353189321b8f98711928e0e8b3a389d
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55208098"
---
# <a name="example-how-to-identify-faces-in-images"></a>Ejemplo: Identificación de caras en imágenes

En esta guía se muestra cómo identificar caras desconocidas con el elemento PersonGroup, que se crea a partir de personas conocidas de antemano. Los ejemplos están escritos en C# con la biblioteca cliente de Face API.

## <a name="concepts"></a>Conceptos

Si no está familiarizado con los siguientes conceptos de esta guía, consulte las definiciones en nuestro [Glosario](../Glossary.md) en cualquier momento:

- Face - Detect
- Face - Identify
- PersonGroup

## <a name="preparation"></a>Preparación

En este ejemplo, se muestra lo siguiente:

- Cómo crear un elemento PersonGroup: este elemento PersonGroup contiene una lista de personas conocidas.
- Cómo asignar caras a cada persona: estas caras se utilizan como línea de base para identificar a las personas. Se recomienda utilizar caras frontales claras, como la foto del documento de identificación. Un buen conjunto de fotos debe contener rostros de la misma persona en diferentes poses, colores de ropa o estilos de cabello.

Para llevar a cabo la demostración de este ejemplo, es necesario preparar un montón de fotos:

- Unas fotos con la cara de la persona. [Haga clic aquí para descargar fotos de ejemplo](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data) para Anna, Bill y Clare.
- Una serie de fotos de prueba, que pueden o no contener las caras de Anna, Bill o Clare utilizadas para probar la identificación. También puede seleccionar algunas imágenes de muestra del vínculo anterior.

## <a name="step-1-authorize-the-api-call"></a>Paso 1: Autorización de la llamada a la API

Cada llamada a Face API requiere una clave de suscripción. Esta clave puede pasarse a través de un parámetro de cadena de consulta o especificarse en la cabecera de la solicitud. Para pasar la clave de suscripción a través de una cadena de consulta, haga referencia a la dirección URL de la solicitud para [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) como ejemplo:
```
https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes]
&subscription-key=<Subscription key>
```

Como alternativa, también puede especificarse la clave de suscripción en el encabezado de la solicitud HTTP: **ocp-apim-subscription-key: &lt;Clave de suscripción&gt;** Al usar una biblioteca cliente, la clave de suscripción se pasa por el constructor de la clase FaceServiceClient. Por ejemplo: 
 
```CSharp 
faceServiceClient = new FaceServiceClient("<Subscription Key>");
```
 
La clave de la suscripción con el punto de conexión correspondiente se puede obtener desde la página Marketplace de Azure Portal. Consulte [Suscripciones](https://azure.microsoft.com/try/cognitive-services/).

## <a name="step-2-create-the-persongroup"></a>Paso 2: Creación del elemento PersonGroup

En este paso, hemos creado un elemento PersonGroup denominado "MyFriends" que contiene tres personas: Anna, Bill y Clare. Cada persona tiene varias caras registrados. Las caras deben detectarse en las imágenes. Después de todos estos pasos, tiene un elemento PersonGroup similar al de la siguiente imagen:

![HowToIdentify1](../Images/group.image.1.jpg)

### <a name="21-define-people-for-the-persongroup"></a>2.1 Definir personas para el elemento PersonGroup
Una persona es una unidad básica de identificación. Una persona puede tener una o más caras conocidas registradas. Sin embargo, un elemento PersonGroup es una colección de personas, y cada persona se define dentro de una determinada entidad PersonGroup. La identificación se realiza en un elemento PersonGroup. Por lo tanto, la tarea consiste en crear un elemento PersonGroup y, después, crear personas en él, como Anna, Bill y Clare.

En primer lugar, debe crear un nuevo elemento PersonGroup. Esto se ejecuta mediante el uso de [PersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API. La API de la biblioteca de cliente correspondiente es el método CreatePersonGroupAsync para la clase FaceServiceClient. El identificador de grupo especificado crear el grupo es único para cada suscripción: también se puede obtener, actualizar o eliminar elementos PersonGroup con otras PersonGroup API. Cuando se define un grupo, las personas pueden define dentro de él mediante [PersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API. El método de la biblioteca cliente es CreatePersonAsync. Puedes agregarle una cara a cada persona después de su creación.

```CSharp 
// Create an empty PersonGroup
string personGroupId = "myfriends";
await faceServiceClient.CreatePersonGroupAsync(personGroupId, "My Friends");
 
// Define Anna
CreatePersonResult friend1 = await faceServiceClient.CreatePersonAsync(
    // Id of the PersonGroup that the person belonged to
    personGroupId,    
    // Name of the person
    "Anna"            
);
 
// Define Bill and Clare in the same way
```
### <a name="step2-2"></a> 2.2 Detectar caras y registrarlas en la persona correcta
La detección se realiza enviando una petición web "POST" a [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API con el archivo de imagen en el cuerpo de la solicitud HTTP. Cuando se utiliza la biblioteca cliente, la detección de caras se realiza mediante el método DetectAsync para la clase FaceServiceClient.

Para cada cara detectada puede llamar a [PersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) para agregarla a la persona correcta.

El siguiente código demuestra el proceso de cómo detectar una cara en una imagen y agregarla a una persona:
```CSharp 
// Directory contains image files of Anna
const string friend1ImageDir = @"D:\Pictures\MyFriends\Anna\";
 
foreach (string imagePath in Directory.GetFiles(friend1ImageDir, "*.jpg"))
{
    using (Stream s = File.OpenRead(imagePath))
    {
        // Detect faces in the image and add to Anna
        await faceServiceClient.AddPersonFaceAsync(
            personGroupId, friend1.PersonId, s);
    }
}
// Do the same for Bill and Clare
``` 
Observe que si la imagen contiene más de una cara, solo se agrega la cara más grande. Puede agregar otras caras a la persona pasando una cadena en el formato "targetFace = left, top, width, height" al parámetro de consulta targetFace de [PersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) API, o mediante el parámetro opcional targetFace del método AddPersonFaceAsync para agregar otras caras. A cada cara que se agrega a la persona se le dará un identificador de cara persistente único, que se puede utilizar en [PersonGroup Person - Delete Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523e) y [Face - Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

## <a name="step-3-train-the-persongroup"></a>Paso 3: Entrenamiento del elemento PersonGroup

El elemento PersonGroup debe entrenarse antes de que se pueda realizar una identificación con ella. Por otra parte, tiene que volverse a entrenar después de agregar o quitar a una persona, o si cualquier persona tiene su cara registrada editada. El entrenamiento se realiza mediante [PersonGroup – Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) API. Cuando se utiliza la biblioteca cliente, simplemente es una llamada al método TrainPersonGroupAsync:
 
```CSharp 
await faceServiceClient.TrainPersonGroupAsync(personGroupId);
```
 
El entrenamiento es un proceso asincrónico. Es posible que no se haya terminado incluso después de que se haya devuelto el método TrainPersonGroupAsync. Puede que necesite consultar el estado de entrenamiento mediante [PersonGroup - Get Training Status](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395247) API o el método GetPersonGroupTrainingStatusAsync de la biblioteca cliente. El siguiente código demuestra una lógica simple de esperar a que termine el entrenamiento de PersonGroup:
 
```CSharp 
TrainingStatus trainingStatus = null;
while(true)
{
    trainingStatus = await faceServiceClient.GetPersonGroupTrainingStatusAsync(personGroupId);
 
    if (trainingStatus.Status != Status.Running)
    {
        break;
    }
 
    await Task.Delay(1000);
} 
``` 

## <a name="step-4-identify-a-face-against-a-defined-persongroup"></a>Paso 4: Identificación de una cara con un elemento PersonGroup definido

Al realizar identificaciones, Face API puede calcular la similitud de una cara de prueba entre todas las caras de un grupo y devuelve a la persona o personas más comparables para esa cara de prueba. Esto se realiza a través de [Face - Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) API o del método IdentifyAsync de la biblioteca cliente.

La cara de prueba debe detectarse usando los pasos anteriores, y después el identificador de la cara se pasa a la API de identificación como segundo argumento. Se pueden identificar varios identificadores de cara a la vez y el resultado contendrá todos los resultados de identificación. De forma predeterminada, el identificador devuelve solo una persona que coincida mejor con la cara de la prueba. Si lo prefiere, puede especificar el parámetro opcional maxNumOfCandidatesReturned para que el identificador devuelva más candidatos.

El siguiente código demuestra el proceso de identificación:

```CSharp 
string testImageFile = @"D:\Pictures\test_img1.jpg";

using (Stream s = File.OpenRead(testImageFile))
{
    var faces = await faceServiceClient.DetectAsync(s);
    var faceIds = faces.Select(face => face.FaceId).ToArray();
 
    var results = await faceServiceClient.IdentifyAsync(personGroupId, faceIds);
    foreach (var identifyResult in results)
    {
        Console.WriteLine("Result of face: {0}", identifyResult.FaceId);
        if (identifyResult.Candidates.Length == 0)
        {
            Console.WriteLine("No one identified");
        }
        else
        {
            // Get top 1 among all candidates returned
            var candidateId = identifyResult.Candidates[0].PersonId;
            var person = await faceServiceClient.GetPersonAsync(personGroupId, candidateId);
            Console.WriteLine("Identified as {0}", person.Name);
        }
    }
}
``` 

Cuando haya terminado los pasos, puede intentar identificar las diferentes caras y ver si las caras de Anna, Bill o Clare se pueden identificar correctamente, de acuerdo con las imágenes cargadas para la detección de caras. Consulte los siguientes ejemplos:

![HowToIdentify2](../Images/identificationResult.1.jpg )

## <a name="step-5-request-for-large-scale"></a>Paso 5: Solicitud a gran escala

Como se sabe, un elemento PersonGroup puede albergar hasta 10 000 personas debido a la limitación del diseño anterior.
Para más información sobre escenarios de hasta un millón de escalas, consulte [Uso de la característica a gran escala](how-to-use-large-scale.md).

## <a name="summary"></a>Resumen

En esta guía, ha aprendido el proceso de creación de un elemento PersonGroup e identificar una persona. Este es un rápido recordatorio de las características anteriormente explicadas y demostradas:

- Detección de caras con [Face - Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d) API
- Creación de PersonGroups con [PersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API
- Creación de personas con [PersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API
- Entrenamiento de un elemento PersonGroup con [PersonGroup – Train](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) API
- Identificación de caras desconocidas en el elemento PersonGroup con [Face - Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) API

## <a name="related-topics"></a>Temas relacionados

- [Detección de caras en imágenes](HowtoDetectFacesinImage.md)
- [Incorporación de caras](how-to-add-faces.md)
- [Uso de la característica a gran escala](how-to-use-large-scale.md)
