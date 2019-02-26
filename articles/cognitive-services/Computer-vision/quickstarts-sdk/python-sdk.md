---
title: 'Inicio rápido: SDK de Python'
titleSuffix: Azure Cognitive Services
description: En este inicio rápido, obtendrá información sobre cómo usar el SDK de Python para tareas comunes.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 02/15/2019
ms.author: pafarley
ms.openlocfilehash: 3043067f326f782c51be38382070ae0db0e90f4d
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2019
ms.locfileid: "56314179"
---
# <a name="azure-cognitive-services-computer-vision-sdk-for-python"></a>SDK de Computer Vision de Azure Cognitive Services para Python

El servicio Computer Vision proporciona a los desarrolladores acceso a algoritmos avanzados para procesar imágenes y devolver información. Los algoritmos de Computer Vision analizan el contenido de una imagen de diferentes formas, en función de las características visuales que le interesen. Por ejemplo, Computer Vision puede determinar si una imagen incluye contenido explícito o para adultos, buscar todas las caras en una imagen u obtener texto impreso o manuscrito. Este servicio funciona con formatos de imagen muy utilizados, como JPEG y PNG. 

Puede usar Computer Vision en su aplicación para lo siguiente:

- Análisis de imágenes para obtener información
- Extracción de texto en las imágenes
- Generación de miniaturas

¿Busca más documentación?

* [Documentación de referencia del SDK](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision)
* [Documentación de Computer Vision de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/computer-vision/)

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure (cree una [cuenta gratuita][azure_sub])
* Un [recurso de Azure Computer Vision][computervision_resource]
* [Python 3.6 o posterior][python]

Si necesita una cuenta de Computer Vision API, puede crear una con este comando de la [CLI de Azure][azure_cli]:

```Bash
RES_REGION=westeurope 
RES_GROUP=<resourcegroup-name>
ACCT_NAME=<computervision-account-name>

az cognitiveservices account create \
    --resource-group $RES_GROUP \
    --name $ACCT_NAME \
    --location $RES_REGION \
    --kind ComputerVision \
    --sku S1 \
    --yes
```

## <a name="installation"></a>Instalación

Instale el SDK de Computer Vision de Azure Cognitive Services con [pip][pip] y, opcionalmente, en un [entorno virtual][venv].

### <a name="configure-a-virtual-environment-optional"></a>Configuración de un entorno virtual (opcional)

Aunque no es necesario, puede aislar el sistema base de los entornos de SDK de Azure si usa un [entorno virtual][virtualenv]. Ejecute los comandos siguientes para configurar un entorno virtual y acceder a él con [venv][venv], como `cogsrv-vision-env`:

```Bash
python3 -m venv cogsrv-vision-env
source cogsrv-vision-env/bin/activate
```

### <a name="install-the-sdk"></a>Instalación del SDK

Instale el [paquete][pypi_computervision] del SDK de Computer Vision de Azure Cognitive Services para Python con [pip][pip]:

```Bash
pip install azure-cognitiveservices-vision-computervision
```

## <a name="authentication"></a>Authentication

Una vez creado el recurso de Computer Vision, necesita su **región**y una de sus **claves de cuenta** para crear una instancia del objeto de cliente.

Use estos valores al crear la instancia del objeto de cliente [ComputerVisionAPI][ref_computervisionclient]. 

### <a name="get-credentials"></a>Obtener credenciales

Use el fragmento siguiente de la [CLI de Azure][cloud_shell] para rellenar dos variables de entorno con la **región** de la cuenta de Computer Vision y una de sus **claves** (también puede encontrar estos valores en [Azure Portal][azure_portal]). El fragmento de código tiene el formato para el shell de Bash.

```Bash
RES_GROUP=<resourcegroup-name>
ACCT_NAME=<computervision-account-name>

export ACCOUNT_REGION=$(az cognitiveservices account show \
    --resource-group $RES_GROUP \
    --name $ACCT_NAME \
    --query location \
    --output tsv)

export ACCOUNT_KEY=$(az cognitiveservices account keys list \
    --resource-group $RES_GROUP \
    --name $ACCT_NAME \
    --query key1 \
    --output tsv)
```

### <a name="create-client"></a>Crear el cliente

Una vez que ha rellenado las variables de entorno `ACCOUNT_REGION` y `ACCOUNT_KEY`, puede crear el objeto de cliente [ComputerVisionAPI][ref_computervisionclient].

```Python
from azure.cognitiveservices.vision.computervision import ComputerVisionAPI
from azure.cognitiveservices.vision.computervision.models import VisualFeatureTypes
from msrest.authentication import CognitiveServicesCredentials

import os
region = os.environ['ACCOUNT_REGION']
key = os.environ['ACCOUNT_KEY']

credentials = CognitiveServicesCredentials(key)
client = ComputerVisionAPI(region, credentials)
```

## <a name="usage"></a>Uso

Después de inicializar un objeto de cliente [ComputerVisionAPI][ref_computervisionclient], puede hacer lo siguiente:

* Analizar una imagen: puede analizar una imagen para detectar determinadas características, como caras, colores y etiquetas.   
* Generar miniaturas: cree una imagen JPEG personalizada para usarla como una miniatura de la imagen original.
* Obtener la descripción de una imagen: obtenga una descripción de la imagen basada en el dominio de sujeto. 

Para más información acerca de este servicio, consulte [¿Qué es Computer Vision?][computervision_docs].

## <a name="examples"></a>Ejemplos

Las secciones siguientes proporcionan varios fragmentos de código que abarcan algunas de las tareas más comunes de Computer Vision, entre las que se incluyen las siguientes:

* [Analizar una imagen](#analyze-an-image)
* [Obtener una lista de dominios de sujeto](#get-subject-domain-list)
* [Analizar una imagen por dominio](#analyze-an-image-by-domain)
* [Obtener la descripción textual de una imagen](#get-text-description-of-an-image)
* [Obtener el texto manuscrito de una imagen](#get-text-from-image)
* [Generar miniatura](#generate-thumbnail)

### <a name="analyze-an-image"></a>Análisis de una imagen

Puede analizar una imagen para detectar determinadas características con [`analyze_image`][ref_computervisionclient_analyze_image]. Use la propiedad [`visual_features`][ref_computervision_model_visualfeatures] para establecer los tipos de análisis que realizar en la imagen. Los valores habituales son `VisualFeatureTypes.tags` y `VisualFeatureTypes.description`.

```Python
url = "https://upload.wikimedia.org/wikipedia/commons/thumb/1/12/Broadway_and_Times_Square_by_night.jpg/450px-Broadway_and_Times_Square_by_night.jpg"

image_analysis = client.analyze_image(url,visual_features=[VisualFeatureTypes.tags])

for tag in image_analysis.tags:
    print(tag)
```

### <a name="get-subject-domain-list"></a>Obtener una lista de dominios de sujeto

Revise los dominios de sujeto utilizados para analizar la imagen con [`list_models`][ref_computervisionclient_list_models]. Estos nombres de dominio se usan al [analizar una imagen por dominio](#analyze-an-image-by-domain). Un ejemplo de un dominio es `landmarks`.

```Python
models = client.list_models()

for x in models.models_property:
    print(x)
```

### <a name="analyze-an-image-by-domain"></a>Analizar una imagen por dominio

Puede analizar una imagen por dominio de sujeto con [`analyze_image_by_domain`][ref_computervisionclient_analyze_image_by_domain]. Obtenga la [lista de dominios de sujeto admitidos](#get-subject-domain-list) para usar el nombre de dominio correcto.  

```Python
domain = "landmarks"
url = "https://upload.wikimedia.org/wikipedia/commons/thumb/1/12/Broadway_and_Times_Square_by_night.jpg/450px-Broadway_and_Times_Square_by_night.jpg"
language = "en"

analysis = client.analyze_image_by_domain(domain, url, language)

for landmark in analysis.result["landmarks"]:
    print(landmark["name"])
    print(landmark["confidence"])
```

### <a name="get-text-description-of-an-image"></a>Obtener la descripción textual de una imagen

Puede obtener una descripción de texto basada en un idioma de una imagen con [`describe_image`][ref_computervisionclient_describe_image]. Solicite varias descripciones con la propiedad `max_description` para realizar un análisis de texto para determinar palabras clave asociadas con la imagen. `a train crossing a bridge over a body of water`, `a large bridge over a body of water` y `a train crossing a bridge over a large body of water` son ejemplos de una descripción de texto de la siguiente imagen.

```Python
domain = "landmarks"
url = "http://www.public-domain-photos.com/free-stock-photos-4/travel/san-francisco/golden-gate-bridge-in-san-francisco.jpg"
language = "en"
max_descriptions = 3

analysis = client.describe_image(url, max_descriptions, language)

for caption in analysis.captions:
    print(caption.text)
    print(caption.confidence)
```

### <a name="get-text-from-image"></a>Obtener el texto de la imagen

Puede obtener cualquier texto manuscrito o impreso de una imagen. Esto requiere dos llamadas al SDK: [`recognize_text`][ref_computervisionclient_recognize_text] y [`get_text_operation_result`][ref_computervisionclient_get_text_operation_result]. La llamada a recognize_text es asincrónica. En los resultados de la llamada a get_text_operation_result, deberá comprobar si la primera llamada finalizó con [`TextOperationStatusCodes`][ref_computervision_model_textoperationstatuscodes] antes de extraer los datos de texto. Los resultados incluyen el texto, así como las coordenadas del cuadro de límite para el texto. 

```Python
url = "https://azurecomcdn.azureedge.net/cvt-1979217d3d0d31c5c87cbd991bccfee2d184b55eeb4081200012bdaf6a65601a/images/shared/cognitive-services-demos/read-text/read-1-thumbnail.png"
mode = TextRecognitionMode.handwritten
raw = True
custom_headers = None
numberOfCharsInOperationId = 36

# SDK call
rawHttpResponse = client.recognize_text(url, mode, custom_headers,  raw)

# Get ID from returned headers
operationLocation = rawHttpResponse.headers["Operation-Location"]
idLocation = len(operationLocation) - numberOfCharsInOperationId
operationId = operationLocation[idLocation:]

# SDK call
result = client.get_text_operation_result(operationId)

# Get data
if result.status == TextOperationStatusCodes.succeeded:

    for line in result.recognition_result.lines:
        print(line.text)
        print(line.bounding_box)
```

### <a name="generate-thumbnail"></a>Generar miniatura

Puede generar una miniatura (JPG) de una imagen con [`generate_thumbnail`][ref_computervisionclient_generate_thumbnail]. La miniatura no necesita tener las mismas proporciones que la imagen original. 

En este ejemplo se usa el paquete [Pillow][pypi_pillow] para guardar localmente la nueva imagen en miniatura.

```Python
from PIL import Image
import io

width = 50
height = 50
url = "http://www.public-domain-photos.com/free-stock-photos-4/travel/san-francisco/golden-gate-bridge-in-san-francisco.jpg"

thumbnail = client.generate_thumbnail(width, height, url)

for x in thumbnail:
    image = Image.open(io.BytesIO(x))

image.save('thumbnail.jpg')
```

## <a name="troubleshooting"></a>solución de problemas

### <a name="general"></a>General

Al interactuar con el objeto de cliente [ComputerVisionAPI][ref_computervisionclient] mediante el SDK de Python, la clase [`ComputerVisionErrorException`][ref_computervision_computervisionerrorexception] se usa para devolver errores. Los errores devueltos por el servicio se corresponden con los mismos códigos de estado HTTP devueltos para las solicitudes de API de REST.

Por ejemplo, si intenta analizar una imagen con una clave no válida, se devuelve un error `401`. En el siguiente fragmento, el [error][ref_httpfailure] se controla correctamente al detectar la excepción y mostrar información adicional sobre el error.

```Python

domain = "landmarks"
url = "http://www.public-domain-photos.com/free-stock-photos-4/travel/san-francisco/golden-gate-bridge-in-san-francisco.jpg"
language = "en"
max_descriptions = 3

try:
    analysis = client.describe_image(url, max_descriptions, language)

    for caption in analysis.captions:
        print(caption.text)
        print(caption.confidence)
except HTTPFailure as e:
    if e.status_code == 401:
        print("Error unauthorized. Make sure your key and region are correct.")
    else:
        raise
```

### <a name="handle-transient-errors-with-retries"></a>Control de errores transitorios con reintentos

Mientras trabaja con el cliente [ComputerVisionAPI][ref_computervisionclient], puede encontrar errores transitorios producidos por [límites de frecuencia][computervision_request_units] aplicados por el servicio u otros problemas transitorios, como interrupciones de red. Para obtener información sobre cómo controlar estos tipos de errores, consulte la sección sobre el [patrón Retry][azure_pattern_retry] en la guía de patrones de diseño en la nube y el [patrón Circuit Breaker][azure_pattern_circuit_breaker] relacionado.

## <a name="next-steps"></a>Pasos siguientes

### <a name="more-sample-code"></a>Más código de ejemplo

Hay varios ejemplos de SDK de Computer Vision para Python disponibles en el repositorio de GitHub del SDK. Estos ejemplos proporcionan código de ejemplo de escenarios adicionales que suelen aparecer al trabajar con Computer Vision:

* [recognize_text][recognize-text]

### <a name="additional-documentation"></a>Documentación adicional

Para obtener documentación ampliada sobre el servicio Computer Vision, consulte la [documentación de Azure Computer Vision][computervision_docs] en docs.microsoft.com.

<!-- LINKS -->
[pip]: https://pypi.org/project/pip/
[python]: https://www.python.org/downloads/

[azure_cli]: https://docs.microsoft.com/cli/azure
[azure_pattern_circuit_breaker]: https://docs.microsoft.com/azure/architecture/patterns/circuit-breaker
[azure_pattern_retry]: https://docs.microsoft.com/azure/architecture/patterns/retry
[azure_portal]: https://portal.azure.com
[azure_sub]: https://azure.microsoft.com/free/

[cloud_shell]: https://docs.microsoft.com/azure/cloud-shell/overview

[venv]: https://docs.python.org/3/library/venv.html
[virtualenv]: https://virtualenv.pypa.io

[source_code]: https://github.com/Azure/azure-sdk-for-python/tree/master/azure-cognitiveservices-vision-computervision

[pypi_computervision]:https://pypi.org/project/azure-cognitiveservices-vision-computervision/
[pypi_pillow]:https://pypi.org/project/Pillow/

[ref_computervision_sdk]: https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision?view=azure-python
[ref_computervision_computervisionerrorexception]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.computervisionerrorexception?view=azure-python
[ref_httpfailure]: https://docs.microsoft.com/python/api/msrest/msrest.exceptions.httpoperationerror?view=azure-python


[computervision_resource]: https://docs.microsoft.com/azure/cognitive-services/computer-vision/vision-api-how-to-topics/howtosubscribe

[computervision_docs]: https://docs.microsoft.com/azure/cognitive-services/computer-vision/home

[ref_computervisionclient]: https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionapi?view=azure-python


[ref_computervisionclient_analyze_image]: https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionapi?view=azure-python#analyze-image-url--visual-features-none--details-none--language--en---custom-headers-none--raw-false----operation-config-
[ref_computervisionclient_list_models]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionapi?view=azure-python#list-models-custom-headers-none--raw-false----operation-config-
[ref_computervisionclient_analyze_image_by_domain]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionapi?view=azure-python#analyze-image-by-domain-model--url--language--en---custom-headers-none--raw-false----operation-config-
[ref_computervisionclient_describe_image]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionapi?view=azure-python#describe-image-url--max-candidates--1---language--en---custom-headers-none--raw-false----operation-config-
[ref_computervisionclient_recognize_text]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionapi?view=azure-python#recognize-text-url--mode--custom-headers-none--raw-false----operation-config-
[ref_computervisionclient_get_text_operation_result]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionapi?view=azure-python#get-text-operation-result-operation-id--custom-headers-none--raw-false----operation-config-
[ref_computervisionclient_generate_thumbnail]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionapi?view=azure-python#generate-thumbnail-width--height--url--smart-cropping-false--custom-headers-none--raw-false--callback-none----operation-config-


[ref_computervision_model_visualfeatures]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-python

[ref_computervision_model_textoperationstatuscodes]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.textoperationstatuscodes?view=azure-python

[computervision_request_units]:https://azure.microsoft.com/pricing/details/cognitive-services/computer-vision/

[recognize-text]:https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/vision/computer_vision_samples.py

