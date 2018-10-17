---
title: 'Ejemplo: Agregar caras - Face API'
titleSuffix: Azure Cognitive Services
description: Use Face API para agregar caras en imágenes.
services: cognitive-services
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: sample
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: fb5d03e2cb3c11daf7a94966fda46345ee910ded
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46125109"
---
# <a name="example-how-to-add-faces"></a>Ejemplo: Incorporación de caras

En esta guía se muestra el procedimiento recomendado para agregar un gran número de personas y caras a un elemento PersonGroup.
Esta misma estrategia se aplica también a FaceList y LargePersonGroup.
Los ejemplos están escritos en C# con la biblioteca cliente de Face API.

## <a name="step-1-initialization"></a>Paso 1: Inicialización

Se declaran varias variables y se implementa una función auxiliar para programar las solicitudes.

- `PersonCount` es el número total de personas.
- `CallLimitPerSecond` es el número máximo de llamadas por segundo según el nivel de suscripción.
- `_timeStampQueue` es una cola para registrar las marcas de tiempo de solicitud.
- `await WaitCallLimitPerSecondAsync()` esperará hasta que sea válido para enviar la solicitud siguiente.

```CSharp
const int PersonCount = 10000;
const int CallLimitPerSecond = 10;
static Queue<DateTime> _timeStampQueue = new Queue<DateTime>(CallLimitPerSecond);

static async Task WaitCallLimitPerSecondAsync()
{
    Monitor.Enter(_timeStampQueue);
    try
    {
        if (_timeStampQueue.Count >= CallLimitPerSecond)
        {
            TimeSpan timeInterval = DateTime.UtcNow - _timeStampQueue.Peek();
            if (timeInterval < TimeSpan.FromSeconds(1))
            {
                await Task.Delay(TimeSpan.FromSeconds(1) - timeInterval);
            }
            _timeStampQueue.Dequeue();
        }
        _timeStampQueue.Enqueue(DateTime.UtcNow);
    }
    finally
    {
        Monitor.Exit(_timeStampQueue);
    }
}
```

## <a name="step-2-authorize-the-api-call"></a>Paso 2: Autorización de la llamada API

Al usar una biblioteca cliente, la clave de suscripción se pasa a través del constructor de la clase FaceServiceClient. Por ejemplo: 

```CSharp
FaceServiceClient faceServiceClient = new FaceServiceClient("<Subscription Key>");
```

La clave de la suscripción con el punto de conexión correspondiente se puede obtener desde la página Marketplace de Azure Portal. Consulte [Suscripciones](https://www.microsoft.com/cognitive-services/en-us/sign-up).

## <a name="step-3-create-the-persongroup"></a>Paso 3: Creación del elemento PersonGroup

Se crea un elemento PersonGroup denominado "MyPersonGroup" para guardar las personas.
El tiempo de solicitud se pone en cola en `_timeStampQueue` para garantizar la validación general.

```CSharp
const string personGroupId = "mypersongroupid";
const string personGroupName = "MyPersonGroup";
_timeStampQueue.Enqueue(DateTime.UtcNow);
await faceServiceClient.CreatePersonGroupAsync(personGroupId, personGroupName);
```

## <a name="step-4-create-the-persons-to-the-persongroup"></a>Paso 4: Creación de las personas de PersonGroup

Las personas se crean de forma simultánea y se aplica también `await WaitCallLimitPerSecondAsync()` para evitar que se supere el límite de llamadas.

```CSharp
CreatePersonResult[] persons = new CreatePersonResult[PersonCount];
Parallel.For(0, PersonCount, async i =>
{
    await WaitCallLimitPerSecondAsync();

    string personName = $"PersonName#{i}";
    persons[i] = await faceServiceClient.CreatePersonAsync(personGroupId, personName);
});
```

## <a name="step-5-add-faces-to-the-persons"></a>Paso 5: Adición de caras a las personas

Cuando se agregan caras a diferentes personas se procesan de manera simultánea, mientras que cuando es para una persona específica el proceso es secuencial.
Una vez más, se invoca `await WaitCallLimitPerSecondAsync()` para garantizar que la frecuencia de solicitud está dentro del ámbito de limitación.

```CSharp
Parallel.For(0, PersonCount, async i =>
{
    Guid personId = persons[i].PersonId;
    string personImageDir = @"/path/to/person/i/images";

    foreach (string imagePath in Directory.GetFiles(personImageDir, "*.jpg"))
    {
        await WaitCallLimitPerSecondAsync();

        using (Stream stream = File.OpenRead(imagePath))
        {
            await faceServiceClient.AddPersonFaceAsync(personGroupId, personId, stream);
        }
    }
});
```

## <a name="summary"></a>Resumen

En esta guía, ha conocido el proceso de creación de un elemento PersonGroup con un gran número de personas y caras. Varios recordatorios:

- Esta estrategia se aplica también a FaceList y LargePersonGroup.
- La adición o eliminación de caras a distintos elementos FaceLists o Persons en LargePersonGroup se puede procesar a la vez.
- Las mismas operaciones para un elemento FaceList o Person específico de LargePersonGroup se deben realizar de manera secuencial.
- Para mantener la simplicidad, en esta guía se omite el control de posibles excepciones. Si quiere mejorar la solidez, se debe aplicar la directiva de reintentos adecuada.

Este es un rápido recordatorio de las características anteriormente explicadas y demostradas:

- Creación de PersonGroups con [PersonGroup - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API
- Creación de personas con [PersonGroup Person - Create](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API
- Adición de caras a personas mediante la API [PersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b)

## <a name="related-topics"></a>Temas relacionados

- [Identificación de caras en imágenes](HowtoIdentifyFacesinImage.md)
- [Detección de caras en imágenes](HowtoDetectFacesinImage.md)
- [Uso de la característica a gran escala](how-to-use-large-scale.md)
