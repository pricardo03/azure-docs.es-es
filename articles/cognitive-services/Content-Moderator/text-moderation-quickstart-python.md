---
title: 'Inicio rápido: Comprobación de contenido de texto en Python con Content Moderator'
titlesuffix: Azure Cognitive Services
description: Cómo analizar el contenido de texto de diverso material ofensivo mediante el SDK de Content Moderator para Python
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: quickstart
ms.date: 01/10/2019
ms.author: pafarley
ms.openlocfilehash: 20920a04c5b85c9eede7d7b249dd6e0548308240
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55883413"
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

```
pip install azure-cognitiveservices-vision-contentmoderator
```

## <a name="import-modules"></a>Importación de módulos

Cree un script de Python denominado _ContentModeratorQS.py_ y agregue el código siguiente para importar los elementos necesarios del SDK.

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/text-moderation-quickstart-python.py?range=1-10)]

También importe la función "impresión con sangría" para controlar la salida final.

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/text-moderation-quickstart-python.py?range=12)]


## <a name="initialize-variables"></a>Inicialización de variables

A continuación, agregue las variables para la dirección URL del punto de conexión y la clave de suscripción de Content Moderator. Tendrá que reemplazar `<your subscription key>` por el valor de la clave. También es posible que tenga que cambiar el valor de `endpoint_url` para usar el identificador de región que corresponde a la clave de suscripción. Las claves de suscripción de la evaluación gratuita se generan en la región **westus**.

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/text-moderation-quickstart-python.py?range=14-16)]


Para simplificar, analizará el texto directamente desde el script. Defina una cadena nueva de contenido de texto para moderar:

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/text-moderation-quickstart-python.py?range=18-21)]


## <a name="query-the-moderator-service"></a>Consulta del servicio Moderator

Cree una instancia de **ContentModeratorClient** con la dirección URL del punto de conexión y la clave de suscripción. Luego, use la instancia de **TextModerationOperations** del miembro para llamar a la API de moderación. Consulte la documentación de referencia de **[screen_text](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-contentmoderator/azure.cognitiveservices.vision.contentmoderator.operations.textmoderationoperations?view=azure-python)** para más información sobre cómo llamarla.

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/text-moderation-quickstart-python.py?range=23-36)]

## <a name="print-the-response"></a>Impresión de la respuesta

Por último, compruebe que la llamada se completó correctamente y que se devolvió una instancia de **Screen**. Imprima los datos devueltos en la consola.

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/text-moderation-quickstart-python.py?range=38-39)]


El texto de ejemplo usado en esta guía de inicio rápido genera la siguiente salida:

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
