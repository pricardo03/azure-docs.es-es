---
title: 'Inicio rápido: Detección del idioma del texto con Python: Translator Text API'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido, obtendrá información sobre cómo identificar el idioma del texto proporcionado con Python y Translator Text REST API.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 02/21/2019
ms.author: erhopf
ms.openlocfilehash: 6b407f88196867a8cc969a15811a1157e7fde59a
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64922146"
---
# <a name="quickstart-use-the-translator-text-api-to-detect-text-language-using-python"></a>Inicio rápido: Uso de Translator Text API para detectar el idioma del texto con Python

En esta guía de inicio rápido, obtendrá información sobre cómo detectar el idioma del texto proporcionado con Python y Translator Text REST API.

En esta guía de inicio rápido, se requiere una [cuenta de Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con un recurso de Translator Text. Si no tiene una cuenta, puede usar la [evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/) para obtener una clave de suscripción.

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

* Python 2.7.x o 3.x
* Una clave de suscripción de Azure para Translator Text

## <a name="create-a-project-and-import-required-modules"></a>Creación de un proyecto e importación de los módulos necesarios

Cree un nuevo proyecto de Python con su IDE o editor favorito. A continuación, copie este fragmento de código en un archivo llamado `detect.py`.

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

El punto de conexión global de Translator Text se establece como `base_url`. `path` establece la ruta de `detect` e identifica que deseamos usar la versión 3 de la API.

>[!NOTE]
> Para más información sobre los puntos de conexión, las rutas y los parámetros de la solicitud, consulte [Translator Text API 3.0: Detección](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-detect).

```python
base_url = 'https://api.cognitive.microsofttranslator.com'
path = '/detect?api-version=3.0'
constructed_url = base_url + path
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

## <a name="create-a-request-to-detect-text-language"></a>Creación de una solicitud para detectar el idioma del texto

Defina la cadena (o cadenas) para las que desea detectar el idioma:

```python
# You can pass more than one object in body.
body = [{
    'text': 'Salve, mondo!'
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
python detect.py
```

Si desea comparar su código con el nuestro, el ejemplo completo está disponible en [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Python).

## <a name="sample-response"></a>Respuesta de muestra

Busque la abreviatura del país en esta [lista de idiomas](https://docs.microsoft.com/azure/cognitive-services/translator/language-support).

```json
[
    {
        "alternatives": [
            {
                "isTranslationSupported": true,
                "isTransliterationSupported": false,
                "language": "pt",
                "score": 1.0
            },
            {
                "isTranslationSupported": true,
                "isTransliterationSupported": false,
                "language": "en",
                "score": 1.0
            }
        ],
        "isTranslationSupported": true,
        "isTransliterationSupported": false,
        "language": "it",
        "score": 1.0
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
* [Obtener traducciones alternativas](quickstart-python-dictionary.md)
* [Obtener una lista de idiomas admitidos](quickstart-python-languages.md)
* [Determinar la longitud de las oraciones de una entrada](quickstart-python-sentences.md)
