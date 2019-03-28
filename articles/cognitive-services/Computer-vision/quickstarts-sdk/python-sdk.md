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
ms.date: 02/28/2019
ms.author: pafarley
ms.openlocfilehash: 23db6f889e2ca4266b7e3566c18cf9a85d4062a8
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58517562"
---
# <a name="azure-cognitive-services-computer-vision-sdk-for-python"></a>SDK de Computer Vision de Azure Cognitive Services para Python

El servicio Computer Vision proporciona a los desarrolladores acceso a algoritmos avanzados para procesar imágenes y devolver información. Los algoritmos de Computer Vision analizan el contenido de una imagen de diferentes formas, en función de las características visuales que le interesen. 

* [Analizar una imagen](#analyze-an-image)
* [Obtener una lista de dominios de sujeto](#get-subject-domain-list)
* [Analizar una imagen por dominio](#analyze-an-image-by-domain)
* [Obtener la descripción textual de una imagen](#get-text-description-of-an-image)
* [Obtener el texto manuscrito de una imagen](#get-text-from-image)
* [Generar miniatura](#generate-thumbnail)

Para más información acerca de este servicio, consulte [¿Qué es Computer Vision?][computervision_docs].

¿Busca más documentación?

* [Documentación de referencia del SDK](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision)
* [Documentación de Computer Vision de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/computer-vision/)

## <a name="prerequisites"></a>Requisitos previos

* [Python 3.6 o posterior][python]
* [Clave de Computer Vision][computervision_resource] gratuita y región asociada. Necesitará estos valores al crear la instancia del objeto de cliente [ComputerVisionAPI][ref_computervisionclient]. Use uno de los métodos siguientes para obtener estos valores. 

### <a name="if-you-dont-have-an-azure-subscription"></a>Si no tiene una suscripción a Azure

Cree una clave gratuita válida durante 7 días con la experiencia **[Probar][computervision_resource]** del servicio Computer Vision. Después de crear la clave, copie la clave y el nombre de la región. Necesitará esta información para [crear el cliente](#create-client).

Después de crear la clave, tenga en cuenta la siguiente información:

* Valor de clave: una cadena de 32 caracteres con el formato de `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx` 
* Región de clave: el subdominio de la dirección URL del punto de conexión, https://**westcentralus**.api.cognitive.microsoft.com

### <a name="if-you-have-an-azure-subscription"></a>Si dispone de una suscripción a Azure

El método más sencillo de crear un recurso en su suscripción es usar el siguiente comando [azure_cli] de la [CLI de Azure]. Este comando crea una clave de Cognitive Services que se puede usar en muchos servicios cognitivos. Deberá elegir el nombre del grupo de recursos _existente_, por ejemplo, "my-cogserv-group" y el nuevo nombre del recurso de Computer Vision, por ejemplo, "my-computer-vision-resource". 

```Bash
RES_REGION=westeurope 
RES_GROUP=<resourcegroup-name>
ACCT_NAME=<computervision-account-name>

az cognitiveservices account create \
    --resource-group $RES_GROUP \
    --name $ACCT_NAME \
    --location $RES_REGION \
    --kind CognitiveServices \
    --sku S0 \
    --yes
```

<!--
## Installation

Install the Azure Cognitive Services Computer Vision SDK with [pip][pip], optionally within a [virtual environment][venv].

### Configure a virtual environment (optional)

Although not required, you can keep your base system and Azure SDK environments isolated from one another if you use a [virtual environment][virtualenv]. Execute the following commands to configure and then enter a virtual environment with [venv][venv], such as `cogsrv-vision-env`:

```Bash
python3 -m venv cogsrv-vision-env
source cogsrv-vision-env/bin/activate
```
-->

### <a name="install-the-sdk"></a>Instalación del SDK

Instale el [paquete][pypi_computervision] del SDK de Computer Vision de Azure Cognitive Services para Python con [pip][pip]:

```Bash
pip install azure-cognitiveservices-vision-computervision
```

## <a name="authentication"></a>Authentication

Una vez creado el recurso de Computer Vision, necesita su **región**y una de sus **claves de cuenta** para crear una instancia del objeto de cliente.

Use estos valores al crear la instancia del objeto de cliente [ComputerVisionAPI][ref_computervisionclient]. 

Por ejemplo, use el terminal de Bash para establecer las variables de entorno:

```Bash
ACCOUNT_REGION=<resourcegroup-name>
ACCT_NAME=<computervision-account-name>
```

### <a name="for-azure-subscription-users-get-credentials-for-key-and-region"></a>Obtención de credenciales de clave y región para los usuarios de la suscripción de Azure

Si no recuerda su región y clave, puede usar el método siguiente para encontrarlas. Si necesita crear una clave y una región, puede usar el método para [titulares de una suscripción de Azure](#if-you-have-an-azure-subscription) o para [usuarios sin una suscripción de Azure](#if-you-dont-have-an-azure-subscription).

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

Obtenga la región y la clave de las variables de entorno [ y ] y, luego, cree el objeto de cliente [ComputerVisionAPIref_computervisionclient].  

```Python
from azure.cognitiveservices.vision.computervision import ComputerVisionAPI
from azure.cognitiveservices.vision.computervision.models import VisualFeatureTypes
from msrest.authentication import CognitiveServicesCredentials

# Get region and key from environment variables
import os
region = os.environ['ACCOUNT_REGION']
key = os.environ['ACCOUNT_KEY']

# Set credentials
credentials = CognitiveServicesCredentials(key)

# Create client
client = ComputerVisionAPI(region, credentials)
```

## <a name="examples"></a>Ejemplos

Necesitará un objeto de cliente [ComputerVisionAPI][ref_computervisionclient] antes de usar una de las siguientes tareas.

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
# type of prediction
domain = "landmarks"

# Public domain image of Eiffel tower
url = "https://images.pexels.com/photos/338515/pexels-photo-338515.jpeg"

# English language response
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
# import models
from azure.cognitiveservices.vision.computervision.models import TextRecognitionMode
from azure.cognitiveservices.vision.computervision.models import TextOperationStatusCodes

url = "https://azurecomcdn.azureedge.net/cvt-1979217d3d0d31c5c87cbd991bccfee2d184b55eeb4081200012bdaf6a65601a/images/shared/cognitive-services-demos/read-text/read-1-thumbnail.png"
mode = TextRecognitionMode.handwritten
raw = True
custom_headers = None
numberOfCharsInOperationId = 36

# Async SDK call
rawHttpResponse = client.recognize_text(url, mode, custom_headers,  raw)

# Get ID from returned headers
operationLocation = rawHttpResponse.headers["Operation-Location"]
idLocation = len(operationLocation) - numberOfCharsInOperationId
operationId = operationLocation[idLocation:]

# SDK call
while result.status in ['NotStarted', 'Running']:
    time.sleep(1)
    result = client.get_text_operation_result(operationId)

# Get data
if result.status == TextOperationStatusCodes.succeeded:

    for line in result.recognition_result.lines:
        print(line.text)
        print(line.bounding_box)
```

### <a name="generate-thumbnail"></a>Generar miniatura

Puede generar una miniatura (JPG) de una imagen con [`generate_thumbnail`][ref_computervisionclient_generate_thumbnail]. La miniatura no necesita tener las mismas proporciones que la imagen original. 

Instale **Pillow** para usar este ejemplo:

```bash
pip install Pillow
``` 

Una vez instalado, use el paquete del siguiente ejemplo de código para generar la imagen en miniatura.

```Python
# Pillow package
from PIL import Image

# IO package to create local image
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

### <a name="more-sample-code"></a>Más código de ejemplo

Hay varios ejemplos de SDK de Computer Vision para Python disponibles en el repositorio de GitHub del SDK. Estos ejemplos proporcionan código de ejemplo de escenarios adicionales que suelen aparecer al trabajar con Computer Vision:

* [recognize_text][recognize-text]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aplicación de etiquetas de contenido a imágenes](../concept-tagging-images.md)

<!-- LINKS -->
[pip]: https://pypi.org/project/pip/
[python]: https://www.python.org/downloads/

[azure_cli]: https://docs.microsoft.com/en-us/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-create
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


[computervision_resource]: https://azure.microsoft.com/en-us/try/cognitive-services/?

[computervision_docs]: https://docs.microsoft.com/azure/cognitive-services/computer-vision/home

[ref_computervisionclient]: https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_analyze_image]: https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_list_models]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_analyze_image_by_domain]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_describe_image]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_recognize_text]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_get_text_operation_result]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_generate_thumbnail]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python


[ref_computervision_model_visualfeatures]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-python

[ref_computervision_model_textoperationstatuscodes]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.textoperationstatuscodes?view=azure-python

[computervision_request_units]:https://azure.microsoft.com/pricing/details/cognitive-services/computer-vision/

[recognize-text]:https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/vision/computer_vision_samples.py

