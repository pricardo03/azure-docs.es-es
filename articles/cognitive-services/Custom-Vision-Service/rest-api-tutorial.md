---
title: 'Tutorial: Uso de la API REST de Custom Vision Service'
titlesuffix: Azure Cognitive Services
description: Use la API de REST para crear, entrenar, probar y exportar un modelo de visión personalizado.
services: cognitive-services
author: blackmist
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: tutorial
ms.date: 08/07/2018
ms.author: larryfr
ms.openlocfilehash: e33eb58dd4228bb1093c239bae960f71c0f3788c
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55885004"
---
# <a name="tutorial-use-the-custom-vision-rest-api"></a>Tutorial: Uso de la API REST de Custom Vision

Obtenga información sobre cómo usar la API de REST de Custom Vision para crear, entrenar, probar y exportar un modelo.

La información de este documento muestra cómo utilizar un cliente de REST para trabajar con la API de REST para entrenar Custom Vision Service. Los ejemplos muestran cómo usar la API mediante la utilidad `curl` desde un entorno de bash y `Invoke-WebRequest` desde Windows PowerShell.

> [!div class="checklist"]
> * Obtención de claves
> * Crear un proyecto
> * Creación de etiquetas
> * Adición de imágenes
> * Entrenamiento y prueba del modelo
> * Exportación del modelo

## <a name="prerequisites"></a>Requisitos previos

* Cierta familiaridad básica con la transferencia de estado representacional (REST). En este documento no se detallan aspectos tales como verbos HTTP, JSON u otras cosas que se usa normalmente con REST.

* Ya sea un bash (Bourne Again Shell) con la utilidad [curl](https://curl.haxx.se) o Windows PowerShell 3.0 (o posterior).

* Una cuenta de Custom Vision. Para obtener más información, consulte el documento [Cómo crear un clasificador](getting-started-build-a-classifier.md).

## <a name="get-keys"></a>Obtención de claves

Cuando se usa la API de REST, debe autenticarse con una clave. Al realizar operaciones de administración o entrenamiento, utiliza la __clave de entrenamiento__. Cuando use el modelo para realizar predicciones, use la __clave de predicción__.

Al realizar una solicitud, la clave se envía como encabezado de la solicitud.

Para obtener las claves para la cuenta, visite la [página web de Custom Vision](https://customvision.ai) y seleccione el __icono de engranaje__ en la esquina superior derecha. En la sección __Cuentas__, copie los valores de los campos __Training Key__ (Clave de entrenamiento) y __Prediction Key__ (Clave de predicción).

![Imagen de la interfaz de usuario de claves](./media/rest-api-tutorial/training-prediction-keys.png)

> [!IMPORTANT]
> Dado que las claves se utilizan para autenticar cada solicitud, los ejemplos en este documento suponen que los valores de clave se encuentran en las variables de entorno. Use los siguientes comandos para almacenar las claves en variables de entorno antes de usar cualquier otro fragmento de código en este documento:
>
> ```bash
> read -p "Enter the training key: " TRAININGKEY
> read -p "Enter the prediction key: " PREDICTIONKEY
> ```
>
> ```powershell
> $trainingKey = Read-Host 'Enter the training key'
> $predictionKey = Read-Host 'Enter the prediction key'
> ```

## <a name="create-a-new-project"></a>Creación de un nuevo proyecto

En los ejemplos siguientes se crea un nuevo proyecto denominado `myproject` en la instancia de Custom Vision Service. Este servicio tiene como dominio predeterminado `General`:

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects?name=myproject" -H "Training-Key: $TRAININGKEY" --data-ascii ""
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects?name=myproject" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey" }
$resp.Content
```

La respuesta a esta solicitud es similar al siguiente documento JSON:

```json
{
  "id":"45d1b19b-69b6-4a22-8e7e-d1ca37504686",
  "name":"myproject",
  "description":"",
  "settings":{
    "domainId":"ee85a72c-405e-4adc-bb47-ffa8ca0c9f31",
    "useNegativeSet":true,
    "classificationType":"Multilabel",
    "detectionParameters":null
  },
  "created":"2018-08-10T17:39:02.5633333",
  "lastModified":"2018-08-10T17:39:02.5633333",
  "thumbnailUri":null
}
```

> [!TIP]
> La entrada `id` en la respuesta es el identificador del nuevo proyecto. Esto se usa en otros ejemplos más adelante en este documento.

Para obtener más información sobre esta solicitud, consulte [CreateProject](https://go.microsoft.com/fwlink/?linkid=865446).

### <a name="specific-domains"></a>Dominios específicos

Para crear un proyecto para un dominio específico, puede proporcionar el __Id. del dominio__ como parámetro opcional. Los ejemplos siguientes muestran cómo recuperar una lista de dominios disponibles:

```bash
curl -X GET "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/domains" -H "Training-Key: $TRAININGKEY" --data-ascii ""
```

```powershell
$resp = Invoke-WebRequest -Method 'GET' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/domains" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey" }
$resp.Content
```

La respuesta a esta solicitud es similar al siguiente documento JSON:

```json
[
    {
        "id": "ee85a74c-405e-4adc-bb47-ffa8ca0c9f31",
        "name": "General",
        "type": "Classification",
        "exportable": false,
        "enabled": true
    },
    {
        "id": "c151d5b5-dd07-472a-acc8-15d29dea8518",
        "name": "Food",
        "type": "Classification",
        "exportable": false,
        "enabled": true
    },
    {
        "id": "ca455789-012d-4b50-9fec-5bb63841c793",
        "name": "Landmarks",
        "type": "Classification",
        "exportable": false,
        "enabled": true
    },
    ...
]
```

Para obtener más información sobre esta solicitud, consulte [GetDomains](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a827d).

En el ejemplo siguiente se muestra cómo crear un nuevo proyecto que utiliza el dominio __Landmarks__:

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects?name=myproject&domainId=ca455789-012d-4b50-9fec-5bb63841c793" -H "Training-Key: $TRAININGKEY" --data-ascii ""
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects?name=myproject&domainId=ca455789-012d-4b50-9fec-5bb63841c793" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey" }
$resp.Content
```

## <a name="create-tags"></a>Creación de etiquetas

Para etiquetar imágenes, debe usar un identificador de etiqueta. En el ejemplo siguiente se muestra cómo crear una etiqueta nueva denominada `cat` y obtener un identificador de etiqueta. Reemplace `{projectId}` por el identificador del proyecto. Use el parámetro `name=` para especificar el nombre de la etiqueta:

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/tags?name=cat" -H "Training-Key: $TRAININGKEY" --data-ascii ""
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/tags?name=cat" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey" }
$resp.Content
```

La respuesta a esta solicitud es similar al documento JSON: 

```json
{"id":"ed6f7ab6-5132-47ad-8649-3ec42ee62d43","name":"cat","description":null,"imageCount":0}
```

Guarde el valor `id`, ya que se utiliza al etiquetar imágenes.

Para obtener más información sobre esta solicitud, consulte [CreateTag](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a829d).

## <a name="add-images"></a>Adición de imágenes

Los ejemplos siguientes muestran cómo agregar un archivo desde la dirección URL. Reemplace `{projectId}` por el identificador del proyecto. Reemplace `{tagId}` con el identificador de la etiqueta de la imagen:

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/images/urls" -H "Training-Key: $TRAININGKEY" -H "Content-Type: application/json" --data-ascii '{"images": [{"url": "http://myimages/cat.jpg","tagIds": ["{tagId}"],"regions": [{"tagId": "{tagId}","left": 119.0,"top": 94.0,"width": 240.0,"height": 140.0}]}], "tagIds": ["{tagId}"]}'
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/images/urls" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey"; "Content-Type"="application/json" } `
    -Body '{"images": [{"url": "http://myimages/cat.jpg","tagIds": ["{tagId}"],"regions": [{"tagId": "{tagId}","left": 119.0,"top": 94.0,"width": 240.0,"height": 140.0}]}], "tagIds": ["{tagId}"]}'
$resp.Content
```

La respuesta a esta solicitud es similar al siguiente documento JSON:

```json
{
    "isBatchSuccessful": true,
    "images": [
        {
            "sourceUrl": "http://myimages/cat.jpg",
            "status": "OK",
            "image": {
                "id": "081adaee-a76b-4d94-a70e-e4fd0935a28f",
                "created": "2018-08-13T13:24:22.0815638",
                "width": 640,
                "height": 480,
                "imageUri": "https://linktoimage",
                "thumbnailUri": "https://linktothumbnail",
                "tags": [
                    {
                        "tagId": "ed6f7ab6-5132-47ad-8649-3ec42ee62d43",
                        "tagName": null,
                        "created": "2018-08-13T13:24:22.104936"
                    }
                ],
                "regions": [
                    {
                        "regionId": "40f206a1-3f8a-4de7-a6c3-c7b4643117df",
                        "tagName": null,
                        "created": "2018-08-13T13:24:22.104936",
                        "tagId": "ed6f7ab6-5132-47ad-8649-3ec42ee62d43",
                        "left": 119,
                        "top": 94,
                        "width": 240,
                        "height": 140
                    }
                ]
            }
        }
    ]
}
```

Para obtener más información sobre esta solicitud, consulte [CreateImagesFromUrls](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a8287).

## <a name="train-the-model"></a>Entrenamiento del modelo

En los ejemplos siguientes se muestra cómo entrenar el modelo. Reemplace `{projectId}` por el identificador del proyecto:

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/train" -H "Training-Key: $TRAININGKEY" -H "Content-Type: application/json" --data-ascii ""
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/train" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey"; "Content-Type"="application/json" } 
$resp.Content
```

La respuesta a esta solicitud es similar al siguiente documento JSON:

```json
{
    "id": "23de09d6-42a1-413e-839e-8db6ee6d3496",
    "name": "Iteration 1",
    "isDefault": false,
    "status": "Training",
    "created": "2018-08-10T17:39:02.5766667",
    "lastModified": "2018-08-16T17:15:07.5250661",
    "projectId": "45d1b19b-69b8-4b22-8e7e-d1ca37504686",
    "exportable": false,
    "domainId": null
}
```

Guarde el valor `id`, ya que se utiliza para probar y exportar el modelo.

Para obtener más información, consulte [TrainProject](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a8294).

## <a name="test-the-model"></a>Prueba del modelo

En los ejemplos siguientes se muestra cómo realizar una prueba del modelo. Reemplace `{projectId}` por el identificador del proyecto. Reemplace `{iterationId}` con el identificador devuelto desde el entrenamiento del modelo. Reemplace `https://linktotestimage` por la ruta a la imagen de prueba.

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/quicktest/url?iterationId={iterationId}" -H "Training-Key: $TRAININGKEY" -H "Content-Type: application/json" --data-ascii '{"url":"https://linktotestimage"}'
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/quicktest/url?iterationId={iterationId}" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey"; "Content-Type"="application/json" } `
    -Body '{"url":"https://linktotestimage"}'
$resp.Content
```

La respuesta a esta solicitud es similar al siguiente documento JSON:

```json
{
    "id": "369b010b-2a92-4f48-a918-4c1a0af91888",
    "project": "45d1b19b-69b8-4b22-8e7e-d1ca37504686",
    "iteration": "23de09d6-42a1-413e-839e-8db6ee6d3496",
    "created": "2018-08-16T17:39:20.7944508Z",
    "predictions": [
        {
            "probability": 0.8390652,
            "tagId": "ed6f7ab6-5132-47ad-8649-3ec42ee62d43",
            "tagName": "cat"
        }
    ]
}
```

Para obtener más información, consulte [QuickTestImageUrl](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a828d).

## <a name="export-the-model"></a>Exportación del modelo

La exportación de un modelo es un proceso de dos pasos. En primer lugar, debe especificar el formato de modelo y, a continuación, solicitar la dirección URL para el modelo exportado.

### <a name="request-a-model-export"></a>Solicitud de la exportación de un modelo

En los ejemplos siguientes se muestra cómo exportar un modelo `coreml`. Reemplace `{projectId}` por el identificador del proyecto. Reemplace `{iterationId}` con el identificador devuelto desde el entrenamiento del modelo.

```bash
curl -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/iterations/{iterationId}/export?platform=coreml" -H "Training-Key: $TRAININGKEY" -H "Content-Type: application/json" --data-ascii ''
```

```powershell
$resp = Invoke-WebRequest -Method 'POST' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/iterations/{iterationId}/export?platform=coreml" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey"; "Content-Type"="application/json" }
$resp.Content
```

La respuesta a esta solicitud es similar al siguiente documento JSON:

```json
{
    "platform": "CoreML",
    "status": "Exporting",
    "downloadUri": null,
    "flavor": null
}
```

Para obtener más información, consulte [ExportIteration](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a829b).

### <a name="download-the-exported-model"></a>Descarga del modelo exportado

En los ejemplos siguientes se muestra cómo recuperar la dirección URL del modelo exportado. Reemplace `{projectId}` por el identificador del proyecto. Reemplace `{iterationId}` con el identificador devuelto desde el entrenamiento del modelo.

```bash
curl -X GET "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/iterations/{iterationId}/export" -H "Training-Key: $TRAININGKEY" -H "Content-Type: application/json" --data-ascii ''
```

```powershell
$resp = Invoke-WebRequest -Method 'GET' `
    -Uri "https://southcentralus.api.cognitive.microsoft.com/customvision/v2.0/Training/projects/{projectId}/iterations/{iterationId}/export" `
    -UseBasicParsing `
    -Headers @{ "Training-Key"="$trainingKey"; "Content-Type"="application/json" }
$resp.Content
```

La respuesta a esta solicitud es similar al siguiente documento JSON:

```json
[
    {
        "platform": "CoreML",
        "status": "Done",
        "downloadUri": "https://linktoexportedmodel",
        "flavor": null
    }
]
```

Para obtener más información, consulte [GetExports](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d0e77c63c39c4259a298830c15188310/operations/5a59953940d86a0f3c7a829a).
