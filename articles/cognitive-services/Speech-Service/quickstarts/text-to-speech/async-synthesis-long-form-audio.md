---
title: 'Inicio rápido: síntesis asincrónica para audio de formato largo (versión preliminar): servicio de Voz'
titleSuffix: Azure Cognitive Services
description: Use Long Audio API para convertir texto a voz de forma asincrónica y recuperar la salida de audio de un URI proporcionado por el servicio. Esta API REST es ideal para los proveedores de contenido que necesitan convertir archivos de texto de más de 10 000 caracteres o 50 párrafos en voz sintetizada.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: erhopf
ms.openlocfilehash: d308623a323fecd39efa90639da71fb981936fe5
ms.sourcegitcommit: a10074461cf112a00fec7e14ba700435173cd3ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2019
ms.locfileid: "73930573"
---
# <a name="quickstart-asynchronous-synthesis-for-long-form-audio-in-python-preview"></a>Inicio rápido: síntesis asincrónica para audio de formato largo en Python (versión preliminar)

En esta guía de inicio rápido, usará Long Audio API para convertir texto a voz de forma asincrónica y recuperar la salida de audio de un URI proporcionado por el servicio. Esta API REST es ideal para los proveedores de contenido que necesitan convertir archivos de texto de más de 10 000 caracteres o 50 párrafos en voz sintetizada. Para obtener más información, consulte [Long Audio API](../../long-audio-api.md).

> [!NOTE]
> La síntesis asincrónica de audio de formato largo solo se puede usar con [voces neuronales personalizadas](../../how-to-custom-voice.md#custom-neural-voices).

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

* Python 2.7.x o 3.x.
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) o su editor favorito de código.
* Una suscripción a Azure y una clave de suscripción al servicio de Voz. [Cree una cuenta de Azure](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#new-azure-account) y [cree un recurso de voz](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#create-a-speech-resource-in-azure) para obtener la clave. Al crear el recurso de voz, asegúrese de que el plan de tarifa está establecido en **S0** y la ubicación está establecida en una [región compatible](../../regions.md#standard-and-neural-voices).

## <a name="create-a-project-and-import-required-modules"></a>Creación de un proyecto e importación de los módulos necesarios

Cree un nuevo proyecto de Python con su IDE o editor favorito. A continuación, copie este fragmento de código en un archivo llamado `voice_synthesis_client.py`.

```python
import argparse
import json
import ntpath
import urllib3
import requests
import time
from json import dumps, loads, JSONEncoder, JSONDecoder
import pickle

urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
```

> [!NOTE]
> Si no ha usado estos módulos deberá instalarlos antes de ejecutar el programa. Para instalar estos paquetes, ejecute: `pip install requests urllib3`.

Estos módulos se usan para analizar argumentos, construir la solicitud HTTP y llamar a la API REST de audios largos para convertir texto a voz.

## <a name="get-a-list-of-supported-voices"></a>Obtención de una lista de voces compatibles

Este código obtiene una lista de las voces disponibles que puede usar para convertir texto a voz. Agregue este código a `voice_synthesis_client.py`:

```python
parser = argparse.ArgumentParser(description='Cris client tool to submit voice synthesis requests.')
parser.add_argument('--voices', action="store_true", default=False, help='print voice list')
parser.add_argument('-key', action="store", dest="key", required=True, help='the cris subscription key, like ff1eb62d06d34767bda0207acb1da7d7 ')
parser.add_argument('-region', action="store", dest="region", required=True, help='the region information, could be centralindia, canadacentral or uksouth')
args = parser.parse_args()
baseAddress = 'https://%s.cris.ai/api/texttospeech/v3.0-beta1/' % args.region

def getVoices():
    response=requests.get(baseAddress+"voicesynthesis/voices", headers={"Ocp-Apim-Subscription-Key":args.key}, verify=False)
    voices = json.loads(response.text)
    return voices

if args.voices:
    voices = getVoices()
    print("There are %d voices available:" % len(voices))
    for voice in voices:
        print ("Name: %s, Description: %s, Id: %s, Locale: %s, Gender: %s, PublicVoice: %s, Created: %s" % (voice['name'], voice['description'], voice['id'], voice['locale'], voice['gender'], voice['isPublicVoice'], voice['created']))
```

### <a name="test-your-code"></a>Prueba del código

Probemos lo que ha hecho hasta ahora. Ejecute este comando. Para ello, reemplace `<your_key>` por su clave de suscripción de Voz y `<region>` por la región donde se creó el recurso de Voz (por ejemplo: `eastus` o `westus`). Esta información está disponible en la pestaña de **información general** del recurso en [Azure Portal](https://aka.ms/azureportal).

```console
python voice_synthesis_client.py --voices -key <your_key> -region <Region>
```

Debe obtener un resultado con el siguiente aspecto:

```console
There are xx voices available:

Name: Microsoft Server Speech Text to Speech Voice (en-US, xxx), Description: xxx , Id: xxx, Locale: en-US, Gender: Male, PublicVoice: xxx, Created: 2019-07-22T09:38:14Z
Name: Microsoft Server Speech Text to Speech Voice (zh-CN, xxx), Description: xxx , Id: xxx, Locale: zh-CN, Gender: Female, PublicVoice: xxx, Created: 2019-08-26T04:55:39Z
```

## <a name="convert-text-to-speech"></a>Conversión de texto en voz

El siguiente paso es preparar un archivo de texto de entrada. Puede ser un texto sin formato o texto SSML, pero debe tener más de 10 000 caracteres o 50 párrafos. Para obtener una lista completa de los requisitos, consulte [Long Audio API](../../long-audio-api.md).

Después de preparar el archivo de texto, el siguiente paso consiste en agregar código para la síntesis de voz al proyecto. Agregue este código a `voice_synthesis_client.py`:

> [!NOTE]
> De forma predeterminada, la salida de audio se establece en riff-16khz-16bit-mono-pcm. Para obtener más información sobre las salidas de audio compatibles, consulte [Long audio API](../../long-audio-api.md#audio-output-formats).

```python
parser.add_argument('--submit', action="store_true", default=False, help='submit a synthesis request')
parser.add_argument('--concatenateResult', action="store_true", default=False, help='If concatenate result in a single wave file')
parser.add_argument('-file', action="store", dest="file", help='the input text script file path')
parser.add_argument('-voiceId', action="store", nargs='+', dest="voiceId", help='the id of the voice which used to synthesis')
parser.add_argument('-locale', action="store", dest="locale", help='the locale information like zh-CN/en-US')
parser.add_argument('-format', action="store", dest="format", default='riff-16khz-16bit-mono-pcm', help='the output audio format')

def submitSynthesis():
    modelList = args.voiceId
    data={'name': 'simple test', 'description': 'desc...', 'models': json.dumps(modelList), 'locale': args.locale, 'outputformat': args.format}
    if args.concatenateResult:
        properties={'ConcatenateResult': 'true'}
        data['properties'] = json.dumps(properties)
    if args.file is not None:
        scriptfilename=ntpath.basename(args.file)
        files = {'script': (scriptfilename, open(args.file, 'rb'), 'text/plain')}
    response = requests.post(baseAddress+"voicesynthesis", data, headers={"Ocp-Apim-Subscription-Key":args.key}, files=files, verify=False)
    if response.status_code == 202:
        location = response.headers['Operation-Location']
        id = location.split("/")[-1]
        print("Submit synthesis request successful")
        return id
    else:
        print("Submit synthesis request failed")
        print("response.status_code: %d" % response.status_code)
        print("response.text: %s" % response.text)
        return 0

def getSubmittedSynthesis(id):
    response=requests.get(baseAddress+"voicesynthesis/"+id, headers={"Ocp-Apim-Subscription-Key":args.key}, verify=False)
    synthesis = json.loads(response.text)
    return synthesis

if args.submit:
    id = submitSynthesis()
    if (id == 0):
        exit(1)

    while(1):
        print("\r\nChecking status")
        synthesis=getSubmittedSynthesis(id)
        if synthesis['status'] == "Succeeded":
            r = requests.get(synthesis['resultsUrl'])
            filename=id + ".zip"
            with open(filename, 'wb') as f:  
                f.write(r.content)
                print("Succeeded... Result file downloaded : " + filename)
            break
        elif synthesis['status'] == "Failed":
            print("Failed...")
            break
        elif synthesis['status'] == "Running":
            print("Running...")
        elif synthesis['status'] == "NotStarted":
            print("NotStarted...")
        time.sleep(10)
```

### <a name="test-your-code"></a>Prueba del código

Intentaremos realizar una solicitud para sintetizar el texto; usaremos el archivo de entrada como origen. Deberá actualizar algunos detalles en la solicitud siguiente:

* Reemplace `<your_key>` por la clave de suscripción del servicio Speech. Esta información está disponible en la pestaña de **información general** del recurso en [Azure Portal](https://aka.ms/azureportal).
* Reemplace `<region>` por la región donde se creó el recurso de Voz (por ejemplo: `eastus` o `westus`). Esta información está disponible en la pestaña de **información general** del recurso en [Azure Portal](https://aka.ms/azureportal).
* Reemplace `<input>` por la ruta de acceso al archivo de texto que quiere convertir de texto a voz.
* Reemplace `<locale>` por la configuración regional de salida deseada. Para obtener más información, consulte [Compatibilidad de idioma](../../language-support.md#neural-voices).
* Reemplace `<voice_guid>` por la voz deseada para la salida de audio. Use una de las voces devueltas por el método de [obtención de una lista de voces compatibles](#get-a-list-of-supported-voices) o use la lista de voces neuronales que se proporciona en [compatibilidad de idioma](../../language-support.md#neural-voices).

Convierta texto a voz con este comando:

```console
python voice_synthesis_client.py --submit -key <your_key> -region <Region> -file <input> -locale <locale> -voiceId <voice_guid>
```

> [!NOTE]
> "concatenateResult" es un parámetro opcional; si no se proporciona este parámetro, el resultado estará compuesto por varios archivos de onda, uno para cada línea.

Debe obtener un resultado con el siguiente aspecto:

```console
Submit synthesis request successful

Checking status
NotStarted...

Checking status
Running...

Checking status
Running...

Checking status
Succeeded... Result file downloaded : xxxx.zip
```

El resultado proporcionado contiene el texto de entrada y los archivos de salida de audio generados por el servicio. Se descargan como un archivo ZIP.

## <a name="remove-previous-requests"></a>Eliminación de solicitudes anteriores

Hay un límite de 2000 solicitudes para cada suscripción. Por ello, habrá ocasiones en las que necesite quitar las solicitudes enviadas previamente para poder crear otras nuevas. Si no quita las solicitudes existentes, recibirá un error cuando supere las 2000.

Agregue este código a `voice_synthesis_client.py`:

```python
parser.add_argument('--syntheses', action="store_true", default=False, help='print synthesis list')
parser.add_argument('--delete', action="store_true", default=False, help='delete a synthesis request')
parser.add_argument('-synthesisId', action="store", nargs='+', dest="synthesisId", help='the id of the voice synthesis which need to be deleted')

def getSubmittedSyntheses():
    response=requests.get(baseAddress+"voicesynthesis", headers={"Ocp-Apim-Subscription-Key":args.key}, verify=False)
    syntheses = json.loads(response.text)
    return syntheses

def deleteSynthesis(ids):
    for id in ids:
        print("delete voice synthesis %s " % id)
        response = requests.delete(baseAddress+"voicesynthesis/"+id, headers={"Ocp-Apim-Subscription-Key":args.key}, verify=False)
        if (response.status_code == 204):
            print("delete successful")
        else:
            print("delete failed, response.status_code: %d, response.text: %s " % (response.status_code, response.text))

if args.syntheses:
    synthese = getSubmittedSyntheses()
    print("There are %d synthesis requests submitted:" % len(synthese))
    for synthesis in synthese:
        print ("ID : %s , Name : %s, Status : %s " % (synthesis['id'], synthesis['name'], synthesis['status']))

if args.delete:
    deleteSynthesis(args.synthesisId)
```

### <a name="test-your-code"></a>Prueba del código

Ejecute este comando. Para ello, reemplace `<your_key>` por su clave de suscripción de Voz y `<region>` por la región donde se creó el recurso de Voz (por ejemplo: `eastus` o `westus`). Esta información está disponible en la pestaña de **información general** del recurso en [Azure Portal](https://aka.ms/azureportal).

```console
python voice_synthesis_client.py – syntheses -key <your_key> -region <Region>
```

Esto devolverá una lista de síntesis solicitadas. Debe obtener un resultado con el siguiente aspecto:

```console
There are <number> synthesis requests submitted:
ID : xxx , Name : xxx, Status : Succeeded
ID : xxx , Name : xxx, Status : Running
ID : xxx , Name : xxx : Succeeded
```

Ahora usaremos algunos de estos valores para quitar o eliminar solicitudes enviadas previamente. Ejecute este comando. Para ello, reemplace `<your_key>` por su clave de suscripción de Voz y `<region>` por la región donde se creó el recurso de Voz (por ejemplo: `eastus` o `westus`). Esta información está disponible en la pestaña de **información general** del recurso en [Azure Portal](https://aka.ms/azureportal). El `<synthesis_id>` debe ser uno de los valores devueltos en la solicitud anterior.

> [!NOTE]
> Las solicitudes con el estado "En ejecución" o "En espera" no se pueden quitar ni eliminar.

```console
python voice_synthesis_client.py – delete -key <your_key> -region <Region> -synthesisId <synthesis_id>
```

Debe obtener un resultado con el siguiente aspecto:

```console
delete voice synthesis xxx
delete successful
```

## <a name="get-the-full-client"></a>Obtención del cliente completo

El `voice_synthesis_client.py` completo está disponible para su descarga en [GitHub](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Python/voiceclient.py).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre Long Audio API](../../long-audio-api.md)
