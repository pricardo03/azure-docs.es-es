---
title: 'Inicio rápido: Comprobación de contenido de texto en Python con Content Moderator'
titleSuffix: Azure Cognitive Services
description: Cómo analizar el contenido de texto de diverso material ofensivo mediante el SDK de Content Moderator para Python
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: pafarley
ms.openlocfilehash: ceaaa9bdb5f6e5c977e0c1a71af080f22d52a5e7
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68561213"
---
# <a name="quickstart-analyze-text-content-for-objectionable-material-in-python"></a>Guía de inicio rápido: Análisis de contenido de textos para detectar material inapropiado en Python

En este artículo se proporciona información y ejemplos de código que le ayudarán a empezar a usar el SDK de Content Moderator para Python. Obtendrá información sobre cómo ejecutar el filtrado y la clasificación basada en términos del contenido de texto con el fin de moderar el material potencialmente ofensivo.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar. 

## <a name="prerequisites"></a>Requisitos previos
- Una clave de suscripción de Content Moderator. Siga las instrucciones de [Creación de una cuenta de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) para suscribirse a Content Moderator y obtener su clave.
- [Python 2.7+ o 3.5+](https://www.python.org/downloads/)
- Herramienta [pip](https://pip.pypa.io/en/stable/installing/)

## <a name="get-the-content-moderator-sdk"></a>Obtención del SDK de Content Moderator

Para instalar el SDK de Python de Content Moderator, abra un símbolo del sistema y ejecute el comando siguiente:

```bash
pip install azure-cognitiveservices-vision-contentmoderator
```

## <a name="import-modules"></a>Importación de módulos

Cree un script de Python denominado _ContentModeratorQS.py_ y agregue el código siguiente para importar los elementos necesarios del SDK. Se incluye el módulo de impresión con sangría para facilitar la lectura de la respuesta JSON.

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/content_moderator_quickstart.py?name=imports)]


## <a name="initialize-variables"></a>Inicialización de variables

A continuación, agregue las variables para la dirección URL del punto de conexión y la clave de suscripción de Content Moderator. Agregue el nombre `CONTENT_MODERATOR_SUBSCRIPTION_KEY` a las variables de entorno con su clave de suscripción como valor. Para la dirección URL del punto de conexión base, agregue `CONTENT_MODERATOR_ENDPOINT` a las variables de entorno con la dirección URL específica de la región como valor, por ejemplo `https://westus.api.cognitive.microsoft.com`. Las claves de suscripción de la evaluación gratuita se generan en la región **westus**.

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/content_moderator_quickstart.py?name=authentication)]

Se moderará una cadena de texto de varias líneas de un archivo. Incluya el archivo [content_moderator_text_moderation.txt](https://github.com/Azure-Samples/cognitive-services-content-moderator-samples/blob/master/documentation-samples/python/content_moderator_text_moderation.txt) en la carpeta raíz local y agregue su nombre de archivo a las variables:

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/content_moderator_quickstart.py?name=textModerationFile)]

## <a name="query-the-moderator-service"></a>Consulta del servicio Moderator

Cree una instancia de **ContentModeratorClient** con la dirección URL del punto de conexión y la clave de suscripción. 

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/content_moderator_quickstart.py?name=client)]

A continuación, use el cliente con la instancia de **TextModerationOperations** de su miembro para llamar a la API de moderación con la función `screen_text`. Consulte la documentación de referencia de **[screen_text](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-contentmoderator/azure.cognitiveservices.vision.contentmoderator.operations.textmoderationoperations?view=azure-python)** para más información sobre cómo llamarla.

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/content_moderator_quickstart.py?name=textModeration)]

## <a name="check-the-printed-response"></a>Comprobación de la respuesta impresa

Ejecute el ejemplo y confirme la respuesta. Tras la finalización correcta, se devuelve una instancia de **Screen**. A continuación se muestra un resultado correcto:

```console
{'auto_corrected_text': '" Is this a garbage email abide@ abed. com, phone: '
                        '6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, '
                        'Redmond, WA 98052. Crap is the profanity here. Is '
                        'this information PII? phone 3144444444\\ n"',
 'classification': {'category1': {'score': 0.00025233393535017967},
                    'category2': {'score': 0.18468093872070312},
                    'category3': {'score': 0.9879999756813049},
                    'review_recommended': True},
 'language': 'eng',
 'normalized_text': '" Is this a garbage email abide@ abed. com, phone: '
                    '6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, '
                    'Redmond, WA 98052. Crap is the profanity here. Is this '
                    'information PII? phone 3144444444\\ n"',
 'original_text': '"Is this a grabage email abcdef@abcd.com, phone: '
                  '6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, '
                  'WA 98052. Crap is the profanity here. Is this information '
                  'PII? phone 3144444444\\n"',
 'pii': {'address': [{'index': 82,
                      'text': '1 Microsoft Way, Redmond, WA 98052'}],
         'email': [{'detected': 'abcdef@abcd.com',
                    'index': 25,
                    'sub_type': 'Regular',
                    'text': 'abcdef@abcd.com'}],
         'ipa': [{'index': 65, 'sub_type': 'IPV4', 'text': '255.255.255.255'}],
         'phone': [{'country_code': 'US', 'index': 49, 'text': '6657789887'},
                   {'country_code': 'US', 'index': 177, 'text': '3144444444'}]},
 'status': {'code': 3000, 'description': 'OK'},
 'terms': [{'index': 118, 'list_id': 0, 'original_index': 118, 'term': 'crap'}],
 'tracking_id': 'b253515c-e713-4316-a016-8397662a3f1a'}
```

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, desarrolló un script Python sencillo que usa el servicio Content Moderator para devolver información pertinente sobre un ejemplo de texto dado. A continuación, conozca lo que significan las distintas marcas y las clasificaciones para que pueda decidir qué datos necesita y el modo en que la aplicación debe controlarlos.

> [!div class="nextstepaction"]
> [Guía para la moderación de texto](text-moderation-api.md)
