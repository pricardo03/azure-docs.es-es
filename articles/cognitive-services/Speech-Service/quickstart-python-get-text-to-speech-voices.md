---
title: 'Inicio rápido: Lista de texto a voz voces, Python - servicios de voz'
titleSuffix: Azure Cognitive Services
description: En este tutorial, obtendrá información sobre cómo obtener la lista completa de las voces neurales y estándares para una región o punto de conexión mediante Python. Se devuelve la lista como JSON y disponibilidad de voz varía según la región.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/22/2019
ms.author: erhopf
ms.openlocfilehash: 66bda68b1313a7c172e273671bc3a03503d08e0d
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "58876587"
---
# <a name="quickstart-get-the-list-of-text-to-speech-voices-using-python"></a>Inicio rápido: Obtiene la lista de texto a voz voces mediante Python

En este tutorial, obtendrá información sobre cómo obtener la lista completa de las voces neurales y estándares para una región o punto de conexión mediante Python. Se devuelve la lista como JSON y disponibilidad de voz varía según la región. Para obtener una lista de regiones admitidas, consulte [regiones](regions.md).

Esta guía, se requiere un [cuenta de Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con un recurso de servicios de voz. Si no tiene una cuenta, puede usar la [evaluación gratuita](get-started.md) para obtener una clave de suscripción.

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

* Python 2.7.x o 3.x
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) o su editor favorito de código
* Una clave de suscripción de Azure para los servicios de voz

## <a name="create-a-project-and-import-required-modules"></a>Creación de un proyecto e importación de los módulos necesarios

Cree un nuevo proyecto de Python con su IDE o editor favorito. A continuación, copie este fragmento de código en un archivo llamado `get-voices.py`.

```python
import requests
```

> [!NOTE]
> Si no ha usado estos módulos deberá instalarlos antes de ejecutar el programa. Para instalar estos paquetes, ejecute: `pip install requests`.

Las solicitudes se usan para las solicitudes HTTP al servicio de texto a voz.

## <a name="set-the-subscription-key-and-create-a-prompt-for-tts"></a>Establecimiento de la clave de suscripción y creación de un símbolo del sistema para TTS

En las secciones siguientes podrá crear métodos para controlar la autorización, llamar a la API Texto a voz y validar la respuesta. Comencemos por crear una clase. Aquí pondremos nuestros métodos de intercambio de token y llamaremos a la API Texto a voz.

```python
class GetVoices(object):
    def __init__(self, subscription_key):
        self.subscription_key = subscription_key
        self.access_token = None
```

`subscription_key` es la clave única de Azure Portal.

## <a name="get-an-access-token"></a>Obtención de un token de acceso

Este punto de conexión requiere un token de acceso para la autenticación. Para obtener un token de acceso, es necesario un intercambio. En este ejemplo se intercambia su clave de suscripción de servicios de voz para obtener un token de acceso usa el `issueToken` punto de conexión.

En este ejemplo se da por supuesto que la suscripción de servicios de voz está en la región Oeste de Estados Unidos. Si usa una región diferente, actualice el valor de `fetch_token_url`. Para obtener una lista completa, consulte [Regiones](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#rest-apis).

Copie este código en la clase `GetVoices`:

```python
def get_token(self):
    fetch_token_url = "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken"
    headers = {
        'Ocp-Apim-Subscription-Key': self.subscription_key
    }
    response = requests.post(fetch_token_url, headers=headers)
    self.access_token = str(response.text)
```

> [!NOTE]
> Para obtener más información, vea [Autenticación con un token de autenticación](https://docs.microsoft.com/azure/cognitive-services/authentication#authenticate-with-an-authentication-token).

## <a name="make-a-request-and-save-the-response"></a>Realización de solicitud y guardado de la respuesta

Aquí va a generar la solicitud y guardar la lista de voces devueltas. En primer lugar, deberá establecer `base_url` y `path`. En este ejemplo se da por hecho que está utilizando el punto de conexión de Oeste de EE. UU. Si el recurso está registrado en una región diferente, asegúrese de actualizar `base_url`. Para obtener más información, consulte [regiones de servicios de voz](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#text-to-speech).

A continuación, agregue los encabezados necesarios para la solicitud. Por último, deberá realizar una solicitud al servicio. Si la solicitud es correcta y se devuelve un código de 200 estado, la respuesta se escribe en el archivo.

Copie este código en la clase `GetVoices`:

```python
def get_voices(self):
    base_url = 'https://westus.tts.speech.microsoft.com'
    path = '/cognitiveservices/voices/list'
    get_voices_url = base_url + path
    headers = {
        'Authorization': 'Bearer ' + self.access_token
    }
    response = requests.get(get_voices_url, headers=headers)
    if response.status_code == 200:
        with open('voices.json', 'wb') as voices:
            voices.write(response.content)
            print("\nStatus code: " + str(response.status_code) + "\nvoices.json is ready to view.\n")
    else:
        print("\nStatus code: " + str(
            response.status_code) + "\nSomething went wrong. Check your subscription key and headers.\n")
```

## <a name="put-it-all-together"></a>Colocación de todo junto

Ya casi ha terminado. El último paso es crear una instancia de la clase y llamar a las funciones.

```python
if __name__ == "__main__":
    subscription_key = "YOUR_KEY_HERE"
    app = GetVoices(subscription_key)
    app.get_token()
    app.get_voices()
```

## <a name="run-the-sample-app"></a>Ejecutar la aplicación de ejemplo

Eso es todo, ya está listo para ejecutar el ejemplo. Desde la línea de comandos (o sesión de terminal), vaya al directorio del proyecto y ejecute:

```console
python get-voices.py
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Asegúrese de quitar cualquier información confidencial del código fuente de la aplicación de ejemplo como, por ejemplo, las claves de suscripción.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de los ejemplos de Python en GitHub](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/Samples-Http/Python)

## <a name="see-also"></a>Vea también

* [Referencia de Text-to-speech API](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis)
* [Crear fuentes de voz personalizada](how-to-customize-voice-font.md)
* [Grabación de muestras de voz para crear una voz personalizada](record-custom-voice-samples.md)
