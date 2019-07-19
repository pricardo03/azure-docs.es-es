---
title: Creación de un conjunto de datos de aprendizaje para un modelo personalizado (Form Recognizer)
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo asegurarse de que el conjunto de datos de aprendizaje está optimizado para entrenar un modelo de Form Recognizer.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: pafarley
ms.openlocfilehash: 643f0d6dd3ee073bd19f8697346689523032ad9f
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67592641"
---
# <a name="build-a-training-data-set-for-a-custom-model"></a>Creación de un conjunto de datos de aprendizaje para un modelo personalizado

Cuando se usa el modelo personalizado de Form Recognizer, puede proporcionar sus propios datos de entrenamiento para que el modelo pueda entrenar según los formularios específicos del sector. Puede entrenar un modelo con cinco formularios rellenados o con un formulario vacío (debe incluir la palabra "empty" [vacío] en el nombre del archivo) más dos formularios rellenados. Incluso si tiene los formularios rellenados suficientes para realizar el entrenamiento, agregar al conjunto de datos de aprendizaje un formulario vacío puede mejorar la precisión del modelo.

## <a name="training-data-tips"></a>Sugerencias sobre los datos de aprendizaje

Es importante usar un conjunto de datos que esté optimizado para el aprendizaje. Use estas sugerencias para asegurarse de obtener los mejores resultados a partir de la operación [Train Model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api/operations/TrainCustomModel) (Entrenar modelo):

* Si es posible, use documentos PDF de texto en lugar de documentos basados en imágenes. Los archivos PDF digitalizados se tratan como imágenes.
* Use un formulario vacío y dos rellenados, si hay disponibles.
* En el caso de los formularios rellenados, use ejemplos en los que estén todos los campos rellenados.
* Use formularios con valores distintos en cada campo.
* Si las imágenes de los formularios son de menor calidad, use un conjunto de datos más grande (con entre 10 y 15 imágenes, por ejemplo).

## <a name="general-input-requirements"></a>Requisitos generales de entrada

Asegúrese de que el conjunto de datos de aprendizaje también cumpla con los requisitos de entrada para todo el contenido de Form Recognizer. 

[!INCLUDE [input requirements](./includes/input-requirements.md)]

## <a name="upload-your-training-data"></a>Carga de los datos de aprendizaje

Una vez que recopila el conjunto de documentos de formularios que usará para el entrenamiento, deberá cargarlo a un contenedor de Azure Blob Storage. Si no sabe cómo crear una cuenta de almacenamiento de Azure con un contenedor, siga el [inicio rápido de Azure Storage para Azure Portal](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal).

### <a name="organize-your-data-in-subfolders-optional"></a>Organización de los datos en subcarpetas (opcional)

De manera predeterminada, la API [Train Model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api/operations/TrainCustomModel) solo usará documentos de formularios que se encuentren en la raíz del contenedor de almacenamiento. Sin embargo, puede realizar el entrenamiento con los datos de las subcarpetas si lo especifica así en la llamada API. Por lo general, el cuerpo de la llamada [Train Model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api/operations/TrainCustomModel) tiene el formato siguiente, donde `<SAS URL>` es la dirección URL de la firma de acceso compartido del contenedor:

```json
{
  "source":"<SAS URL>"
}
```

Si agrega el contenido siguiente al cuerpo de la solicitud, la API se entrenará con los documentos ubicados en las subcarpetas. El campo `"prefix"` es opcional y limitará el conjunto de datos de aprendizaje a los archivos cuyas rutas de acceso empiezan con la cadena determinada. Por lo tanto, un valor de `"Test"`, por ejemplo, hará que la API examine solo los archivos o las carpetas que empiecen con la palabra "Test" (Prueba).

```json
{
  "source": "<SAS URL>",
  "sourceFilter": {
    "prefix": "<prefix string>",
    "includeSubFolders": true
  }
}
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que aprendió a crear un conjunto de datos de aprendizaje, siga un inicio rápido para entrenar a un modelo personalizado de Form Recognizer y empezar a usarlo en los formularios.

* [Inicio rápido: Entrenamiento de un modelo y extracción de datos de formularios mediante cURL](./quickstarts/curl-train-extract.md).
* [Inicio rápido: Entrenamiento de un modelo y extracción de datos de formularios mediante la API REST con Python](./quickstarts/python-train-extract.md)

