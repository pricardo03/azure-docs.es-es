---
title: 'Inicio rápido: Obtención de una lista de idiomas admitidos con Python: Translator Text API'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido se obtiene una lista de los idiomas admitidos para la traducción, transcripción y búsqueda en diccionarios, y ejemplos mediante Translator Text API con Python.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 02/07/2019
ms.author: erhopf
ms.openlocfilehash: 099485f37cc188307b6c04343ef174740acf07cb
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2019
ms.locfileid: "55891589"
---
# <a name="quickstart-use-the-translator-text-api-to-get-a-list-of-supported-languages-using-python"></a>Inicio rápido: Uso de Translator Text API para obtener una lista de los idiomas compatibles mediante Python

En esta guía de inicio rápido, aprenderá a realizar una solicitud GET que devuelve una lista de idiomas compatibles mediante Python y Translator Text REST API.

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

* Python 2.7.x o 3.x

## <a name="create-a-project-and-import-required-modules"></a>Creación de un proyecto e importación de los módulos necesarios

Cree un nuevo proyecto de Python con su IDE o editor favorito. A continuación, copie este fragmento de código en un archivo llamado `get-languages.py`.

```python
# -*- coding: utf-8 -*-
import os, requests, uuid, json
```

> [!NOTE]
> Si no ha usado estos módulos deberá instalarlos antes de ejecutar el programa. Para instalar estos paquetes, ejecute: `pip install requests uuid`.

El primer comentario le indica al intérprete de Python que debe usar la codificación UTF-8. Después, se importan los módulos necesarios para leer la clave de suscripción desde una variable de entorno, construir la solicitud HTTP, crear un identificador único y controlar la respuesta JSON que devuelve Translator Text API.

## <a name="set-the-base-url-and-path"></a>Establecimiento de la dirección URL base y la ruta de acceso

El punto de conexión global de Translator Text se establece como `base_url`. `path` establece la ruta de `languages` e identifica que deseamos usar la versión 3 de la API.

>[!NOTE]
> Para más información sobre los puntos de conexión, las rutas y los parámetros de la solicitud, consulte [Translator Text API 3.0: Idiomas](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-languages).

```python
base_url = 'https://api.cognitive.microsofttranslator.com'
path = '/languages?api-version=3.0'
constructed_url = base_url + path
```

## <a name="add-headers"></a>Incorporación de encabezados

La solicitud para obtener los idiomas compatibles no requiere autenticación. Establezca `Content-type` como `application/json` y agregue `X-ClientTraceId` para identificar de forma única la solicitud.

Copie este fragmento de código en el proyecto:

```python
headers = {
    'Content-type': 'application/json',
    'X-ClientTraceId': str(uuid.uuid4())
}
```

## <a name="create-a-request-to-get-a-list-of-supported-languages"></a>Crear una solicitud para obtener una lista de idiomas admitidos

Vamos a crear una solicitud GET mediante el módulo `requests`, que usa DOS argumentos: la dirección URL concatenada y los encabezados de solicitud:

```python
request = requests.get(constructed_url, headers=headers)
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
python get-languages.py
```

Si desea comparar su código con el nuestro, el ejemplo completo está disponible en [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Python).

## <a name="sample-response"></a>Respuesta de muestra

Este ejemplo se ha truncado para mostrar un fragmento del resultado:

```json
{
    "translation": {
        "af": {
            "name": "Afrikaans",
            "nativeName": "Afrikaans",
            "dir": "ltr"
        },
        "ar": {
            "name": "Arabic",
            "nativeName": "العربية",
            "dir": "rtl"
        },
        ...
    },
    "transliteration": {
        "ar": {
            "name": "Arabic",
            "nativeName": "العربية",
            "scripts": [
                {
                    "code": "Arab",
                    "name": "Arabic",
                    "nativeName": "العربية",
                    "dir": "rtl",
                    "toScripts": [
                        {
                            "code": "Latn",
                            "name": "Latin",
                            "nativeName": "اللاتينية",
                            "dir": "ltr"
                        }
                    ]
                },
                {
                    "code": "Latn",
                    "name": "Latin",
                    "nativeName": "اللاتينية",
                    "dir": "ltr",
                    "toScripts": [
                        {
                            "code": "Arab",
                            "name": "Arabic",
                            "nativeName": "العربية",
                            "dir": "rtl"
                        }
                    ]
                }
            ]
        },
      ...
    },
    "dictionary": {
        "af": {
            "name": "Afrikaans",
            "nativeName": "Afrikaans",
            "dir": "ltr",
            "translations": [
                {
                    "name": "English",
                    "nativeName": "English",
                    "dir": "ltr",
                    "code": "en"
                }
            ]
        },
        "ar": {
            "name": "Arabic",
            "nativeName": "العربية",
            "dir": "rtl",
            "translations": [
                {
                    "name": "English",
                    "nativeName": "English",
                    "dir": "ltr",
                    "code": "en"
                }
            ]
        },
      ...
    }
}
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
* [Obtener traducciones alternativas](quickstart-python-dictionary.md)
* [Determinar la longitud de las oraciones de una entrada](quickstart-python-sentences.md)
