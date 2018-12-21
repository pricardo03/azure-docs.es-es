---
title: 'Inicio rápido: Obtención de traducciones alternativas con Python: Translator Text API'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido, aprenderá a buscar traducciones alternativas y ejemplos de uso para un texto específico con Python y Translator Text REST API.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 10/21/2018
ms.author: erhopf
ms.openlocfilehash: ab8a6d5b6d4bb52863e77b844a4008bb80d840be
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2018
ms.locfileid: "52993958"
---
# <a name="quickstart-use-the-translator-text-api-to-get-alternate-translations-using-python"></a>Inicio rápido: Uso de Translator Text API para obtener traducciones alternativas con Python

En esta guía de inicio rápido, aprenderá a buscar traducciones alternativas y ejemplos de uso para un texto específico con Python y Translator Text REST API.

En esta guía de inicio rápido, se requiere una [cuenta de Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con un recurso de Translator Text. Si no tiene una cuenta, puede usar la [evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/) para obtener una clave de suscripción.

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

* Python 2.7.x o 3.x
* Una clave de suscripción de Azure para Translator Text

## <a name="create-a-project-and-import-required-modules"></a>Creación de un proyecto e importación de los módulos necesarios

Cree un nuevo proyecto de Python con su IDE o editor favorito. A continuación, copie este fragmento de código en un archivo llamado `dictionary-lookup.py`.

```python
# -*- coding: utf-8 -*-
import os, requests, uuid, json
```

> [!NOTE]
> Si no ha usado estos módulos deberá instalarlos antes de ejecutar el programa. Para instalar estos paquetes, ejecute: `pip install requests uuid`.

El primer comentario le indica al intérprete de Python que debe usar la codificación UTF-8. Después, se importan los módulos necesarios para leer la clave de suscripción desde una variable de entorno, construir la solicitud HTTP, crear un identificador único y controlar la respuesta JSON que devuelve Translator Text API.

## <a name="set-the-subscription-key-base-url-and-path"></a>Establecimiento de la clave de suscripción, la dirección URL base y la ruta de acceso

En este ejemplo se intenta leer la clave de suscripción de Translator Text desde la variable de entorno `TRANSLATOR_TEXT_KEY`. Si no está familiarizado con las variables de entorno, puede establecer `subscriptionKey` como una cadena y convertir en comentario la instrucción condicional.

Copie este código en el proyecto:

```python
# Checks to see if the Translator Text subscription key is available
# as an environment variable. If you are setting your subscription key as a
# string, then comment these lines out.
if 'TRANSLATOR_TEXT_KEY' in os.environ:
    subscriptionKey = os.environ['TRANSLATOR_TEXT_KEY']
else:
    print('Environment variable for TRANSLATOR_TEXT_KEY is not set.')
    exit()
# If you want to set your subscription key as a string, uncomment the line
# below and add your subscription key.
#subscriptionKey = 'put_your_key_here'
```

Actualmente, hay un punto de conexión disponible para Translator Text y está establecido como `base_url`. `path` establece la ruta de `dictionary/lookup` e identifica que deseamos usar la versión 3 de la API.

`params` se utiliza para establecer los idiomas de entrada y salida. En este ejemplo, se van a usar inglés y español: `en` y `es`.

>[!NOTE]
> Para más información sobre los puntos de conexión, las rutas y los parámetros de la solicitud, consulte [Translator Text API 3.0: Búsqueda de diccionario](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-dictionary-lookup).

```python
base_url = 'https://api.cognitive.microsofttranslator.com'
path = '/dictionary/lookup?api-version=3.0'
params = '&from=en&to=es';
constructed_url = base_url + path + params
```

## <a name="add-headers"></a>Incorporación de encabezados

La manera más fácil de autenticar una solicitud es pasar la clave de suscripción como un encabezado `Ocp-Apim-Subscription-Key`, que es el que se usa en este ejemplo. O bien, puede intercambiar la clave de suscripción para un token de acceso y pasar este token como un encabezado `Authorization` para validar la solicitud. Para más información, consulte [Autenticación](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

Copie este fragmento de código en el proyecto:

```python
headers = {
    'Ocp-Apim-Subscription-Key': subscriptionKey,
    'Content-type': 'application/json',
    'X-ClientTraceId': str(uuid.uuid4())
}
```

## <a name="create-a-request-to-find-alternate-translations"></a>Creación de una solicitud para buscar traducciones alternativas

Defina la cadena (o cadenas) para las que desea encontrar traducciones:

```python
# You can pass more than one object in body.
body = [{
    'text': 'Elephants'
}]
```

A continuación, vamos a crear una solicitud POST mediante el módulo `requests`, que usa tres argumentos: la dirección URL concatenada, los encabezados de solicitud y el cuerpo de solicitud:

```python
request = requests.post(constructed_url, headers=headers, json=body)
response = request.json()
```

## <a name="print-the-response"></a>Impresión de la respuesta

El último paso es imprimir los resultados. Este fragmento de código adorna los resultados mediante una clasificación de las claves, el establecimiento de una sangría y la declaración de separadores de elementos y de claves.

```python
print(json.dumps(response, sort_keys=True, indent=4, ensure_ascii=False, separators=(',', ': ')))
```

## <a name="put-it-all-together"></a>Colocación de todo junto

Eso es todo, ha creado un sencillo programa que llama a Translator Text API y devuelve una respuesta JSON. Ahora es el momento de ejecutar el programa:

```console
python dictionary-lookup.py
```

Si desea comparar su código con el nuestro, el ejemplo completo está disponible en [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Python).

## <a name="sample-response"></a>Respuesta de muestra

```json
[
    {
        "displaySource": "elephants",
        "normalizedSource": "elephants",
        "translations": [
            {
                "backTranslations": [
                    {
                        "displayText": "elephants",
                        "frequencyCount": 1207,
                        "normalizedText": "elephants",
                        "numExamples": 5
                    }
                ],
                "confidence": 1.0,
                "displayTarget": "elefantes",
                "normalizedTarget": "elefantes",
                "posTag": "NOUN",
                "prefixWord": ""
            }
        ]
    }
]
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Si ha codificado de forma rígida la clave de suscripción en el programa, asegúrese de quitarla cuando haya terminado con esta guía de inicio rápido.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Explorar ejemplos de Python en GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Python)

## <a name="see-also"></a>Otras referencias

Aprenda a usar Translator Text API para:

* [Traducir texto](quickstart-python-translate.md)
* [Transliterar texto](quickstart-python-transliterate.md)
* [Identificar el idioma de entrada](quickstart-python-detect.md)
* [Obtener una lista de idiomas admitidos](quickstart-python-languages.md)
* [Determinar la longitud de las oraciones de una entrada](quickstart-python-sentences.md)
