---
title: 'Inicio rápido: Comprobación de contenido de texto en C# con Content Moderator'
titleSuffix: Azure Cognitive Services
description: Cómo analizar el contenido de texto de diverso material ofensivo mediante el SDK de Content Moderator para .NET
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: pafarley
ms.openlocfilehash: 40f73a6aabd3631bf2a2f8f1406d2ec9abd55e51
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2019
ms.locfileid: "72757228"
---
# <a name="quickstart-analyze-text-content-for-objectionable-material-in-c"></a>Inicio rápido: Análisis de contenido de textos para detectar material inapropiado en C#

En este artículo se proporciona información y ejemplos de código que le ayudarán a empezar a usar el [SDK de Content Moderator para .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/). Obtendrá información sobre cómo ejecutar el filtrado y la clasificación basada en términos del contenido de texto con el fin de moderar el material potencialmente ofensivo.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar. 

## <a name="prerequisites"></a>Requisitos previos
- Una clave de suscripción de Content Moderator. Siga las instrucciones de [Creación de una cuenta de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) para suscribirse a Content Moderator y obtener su clave.
- Cualquier edición de [Visual Studio 2015 o 2017](https://www.visualstudio.com/downloads/).

> [!NOTE]
> Esta guía usa una suscripción de Content Moderator gratuita. Para información sobre lo que se proporciona con cada nivel de suscripción, consulte la página [Precios y límites](https://azure.microsoft.com/pricing/details/cognitive-services/content-moderator/).

## <a name="create-the-visual-studio-project"></a>Creación del proyecto de Visual Studio

1. En Visual Studio, cree un nuevo proyecto de **Aplicación de consola (.NET Framework)** y asígnele el nombre **TextModeration**. 
1. Si hay otros proyectos en la solución, seleccione este como proyecto de inicio único.
1. Obtenga el paquete NuGet requerido. En el Explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione **Administrar paquetes de NuGet**. A continuación, busque e instale el **paquete `Microsoft.Azure.CognitiveServices.ContentModerator`** . Como alternativa, puede ejecutar el siguiente comando desde el directorio de la solución:

```console
dotnet add package Microsoft.Azure.CognitiveServices.ContentModerator
```

## <a name="add-text-moderation-code"></a>Incorporación de código de moderación de texto

A continuación, copiará y pegará el código de esta guía en el proyecto para implementar un escenario básico de moderación de contenido.

### <a name="include-namespaces"></a>Inclusión de espacios de nombres

Agregue las siguientes instrucciones `using` al principio del archivo *Program.cs*.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/text-moderation-quickstart-dotnet.cs?range=1-8)]

### <a name="create-the-content-moderator-client"></a>Creación del cliente de Content Moderator

Agregue el código siguiente al archivo *Program.cs* para crear un cliente de Content Moderator para su suscripción. Agregue el código junto con la clase **Program**, en el mismo espacio de nombres. Tendrá que actualizar los campos **AzureRegion** y **CMSubscriptionKey** con los valores de su identificador de región y clave de suscripción.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/text-moderation-quickstart-dotnet.cs?range=54-77)]

### <a name="set-up-input-and-output-targets"></a>Configuración de los destinos de entrada y salida

Agregue los siguientes campos estáticos a la clase **Program** en _Program.cs_. Estos campos especifican los archivos para el contenido de texto de entrada y el contenido JSON de salida.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/text-moderation-quickstart-dotnet.cs?range=15-19)]

Tendrá que crear el archivo *TextFile.txt* de entrada y actualizar su ruta de acceso (las rutas de acceso relativas se refieren al directorio de ejecución). Abra _TextFile.txt_ y agregue el texto que se va a moderar. En esta guía de inicio rápido se usa el siguiente texto de ejemplo:

```
Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.
These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 
0800 820 3300. Also, 999-99-9999 looks like a social security number (SSN).
```

### <a name="load-the-input-text"></a>Carga del texto de entrada

Agregue el siguiente código al método **Main**. El método **ScreenText** constituye la operación esencial. Sus parámetros especifican qué operaciones de moderación de contenido se realizarán. En este ejemplo, el método se configura para:
- Detectar posibles blasfemias en el texto.
- Normalizar el texto y corregir los errores tipográficos automáticamente.
- Detectar datos personales, como números de teléfono de Estados Unidos y Reino Unido, direcciones de correo electrónico y direcciones postales de Estados Unidos.
- Usar modelos basados en aprendizaje automático para clasificar el texto en tres categorías.

Si desea más información sobre lo que hacen estas operaciones, siga el vínculo de la sección [Pasos siguientes](#next-steps).

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/text-moderation-quickstart-dotnet.cs?range=23-48)]

## <a name="run-the-program"></a>Ejecución del programa

El programa escribirá los datos de cadena de JSON en el archivo _TextModerationOutput.txt_. El texto de ejemplo utilizado en esta guía de inicio rápido provoca la siguiente salida:

```json
Autocorrect typos, check for matching terms, check for personal data, and classify.
{
"OriginalText": "\"Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number (SSN).\"",
"NormalizedText": "\" Is this a garbage or crap email abide@ abed. com, phone: 6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, Redmond, WA 98052. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number ( SSN) . \"",
"AutoCorrectedText": "\" Is this a garbage or crap email abide@ abed. com, phone: 6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, Redmond, WA 98052. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number ( SSN) . \"",
"Misrepresentation": null,
    
"Classification": {
        "Category1": {
        "Score": 1.5113095059859916E-06
        },
        "Category2": {
        "Score": 0.12747249007225037
        },
        "Category3": {
        "Score": 0.98799997568130493
        },
        "ReviewRecommended": true
  },
  "Status": {
    "Code": 3000,
    "Description": "OK",
    "Exception": null
  },
  "PII": {
        "Email": [
            {
            "Detected": "abcdef@abcd.com",
            "SubType": "Regular",
            "Text": "abcdef@abcd.com",
            "Index": 33
            }
            ],
        "IPA": [
            {
            "SubType": "IPV4",
            "Text": "255.255.255.255",
            "Index": 73
            }
            ],
        "Phone": [
            {
            "CountryCode": "US",
            "Text": "6657789887",
            "Index": 57
            },
            {
            "CountryCode": "US",
            "Text": "870 608 4000",
            "Index": 211
            },
            {
            "CountryCode": "UK",
            "Text": "+44 870 608 4000",
            "Index": 207
            },
            {
            "CountryCode": "UK",
            "Text": "0344 800 2400",
            "Index": 227
            },
            {
        "CountryCode": "UK",
            "Text": "0800 820 3300",
            "Index": 244
            }
            ],
         "Address": [{
             "Text": "1 Microsoft Way, Redmond, WA 98052",
             "Index": 89
                }]
    },
  "Language": "eng",
  "Terms": [
    {
        "Index": 22,
        "OriginalIndex": 22,
        "ListId": 0,
        "Term": "crap"
    }
  ],
  "TrackingId": "9392c53c-d11a-441d-a874-eb2b93d978d3"
}
```

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha desarrollado una aplicación .NET simple que usa el servicio Content Moderator para devolver información pertinente sobre un ejemplo de texto dado. A continuación, conozca lo que significan las distintas marcas y las clasificaciones para que pueda decidir qué datos necesita y el modo en que la aplicación debe controlarlos.

> [!div class="nextstepaction"]
> [Guía para la moderación de texto](text-moderation-api.md)
