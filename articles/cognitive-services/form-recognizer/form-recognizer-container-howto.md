---
title: 'Instalación y ejecución del contenedor: Form Recognizer'
titleSuffix: Azure Cognitive Services
description: Aprenda a utilizar el contenedor de Form Recognizer para analizar datos de formularios y tablas.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: overview
ms.date: 05/07/2019
ms.author: pafarley
ms.openlocfilehash: a7159fccc9c4ef232cfca08b173e712e268343ea
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65507804"
---
# <a name="install-and-run-form-recognizer-containers"></a>Instalación y ejecución de contenedores de Form Recognizer
Form Recognizer aplica tecnología de aprendizaje automático para identificar y extraer pares clave-valor de los formularios. Asocia valores y entradas en tabla con ellos y, después, genera datos estructurados que incluyen las relaciones en el fichero original. Puede llamar al modelo personalizado de Form Recognizer mediante una simple API REST para reducir la complejidad e integrarla fácilmente en su proceso de automatización del flujo de trabajo u otra aplicación. Solo se necesitan cinco documentos (o un formulario vacío), por lo que puede obtener resultados de forma rápida, precisa y adaptada a su contenido específico sin necesidad de una pesada intervención manual o una amplia experiencia en ciencia de datos. No requiere el etiquetado ni la anotación de datos.

|Función|Características|
|-|-|
|Form Recognizer| <li>Procesa los archivos de tipo PDF, PNG y JPG.<li>Entrena modelos personalizados con un mínimo de cinco formas del mismo diseño. <li>Extrae información de la tabla y de pares clave-valor. <li>Utiliza la operación RecognizeText de la API Computer Vision de Cognitive Services con el fin de detectar y extraer texto impreso de las imágenes dentro de formularios.<li>No requiere la anotación ni el etiquetado.|

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Debe cumplir los siguientes requisitos previos para poder usar los contenedores de Form Recognizer:

|Obligatorio|Propósito|
|--|--|
|Motor de Docker| Necesita que el motor de Docker esté instalado en un [equipo host](#the-host-computer). Docker dispone de paquetes que configuran el entorno de Docker en [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) y [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Para conocer los principios básicos de Docker y de los contenedores, consulte [Introducción a Docker](https://docs.docker.com/engine/docker-overview/).<br><br> Docker debe configurarse para permitir que los contenedores se conecten con Azure y envíen datos de facturación a dicho servicio. <br><br> **En Windows**, Docker también debe estar configurado de forma que admita los contenedores de Linux.<br><br>|
|Conocimientos sobre Docker | Debe tener conocimientos básicos sobre los conceptos de Docker, como los registros, los repositorios, los contenedores y las imágenes de contenedor, así como conocer los comandos `docker` básicos.|
|Azure CLI| Deberá instalar la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) en el host.|
|Recurso de la API Computer Vision| Para procesar documentos e imágenes digitalizados, se requiere un **recurso de Computer Vision**. Puede acceder a la característica **Reconocer texto** como un recurso de Azure (REST API o SDK) o como un [contenedor](../Computer-vision/computer-vision-how-to-install-containers.md##get-the-container-image-with-docker-pull) `cognitive-services-recognize-text`. Se aplican las tarifas de facturación habituales. <br><br>Debe pasar tanto la clave como el punto de conexión de facturación para el recurso específico de Computer Vision (nube de Azure o contenedor de Cognitive Services). Utilice esta clave y el punto de conexión de facturación como {COMPUTER_VISION_API_KEY} y {COMPUTER_VISION_BILLING_ENDPOINT_URI}.<br><br> Si se usa el **`cognitive-services-recognize-text`contenedor**, asegúrese de que:<br><br>* La clave de Computer Vision para el contenedor de Form Recognizer es la clave especificada en el comando `docker run` de Computer Vision para el contenedor `cognitive-services-recognize-text`.<br>* El punto de conexión de facturación es el punto de conexión del contenedor, como `https://localhost:5000`. Si se utilizan los contenedores Computer Vision y Form Recognizer juntos en el mismo host, no se pueden iniciar con el puerto predeterminado de `5000`.  |  
|Recurso de Form Recognizer |Para usar estos contenedores, debe tener:<br><br>Un recurso de Azure de _Form Recognizer_ para obtener la clave de facturación asociada y el URI del punto de conexión de facturación. Ambos valores están disponibles en las páginas de claves y de información general de **Form Recognizer** en Azure Portal y son necesarios para iniciar el contenedor.<br><br>**{BILLING_KEY}**: clave de recurso<br><br>**{BILLING_ENDPOINT_URI}**: el ejemplo de URI de punto de conexión es `https://westus.api.cognitive.microsoft.com/forms/v1.0`| 

## <a name="request-access-to-the-container-registry"></a>Solicitud de acceso al registro de contenedor

Primero debe completar y enviar el [formulario de solicitud de contenedores de Form Recognizer de Cognitive Services](https://aka.ms/FormRecognizerRequestAccess) para solicitar acceso al contenedor. Esto también le registrará en Computer Vision. No es necesario registrarse para obtener el formulario de solicitud de Computer Vision por separado. 

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

## <a name="the-host-computer"></a>El equipo host

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Recomendaciones y requisitos del contenedor

En la tabla siguiente se describen los núcleos de CPU y de memoria mínimos y recomendados para asignar cada contenedor de Form Recognizer.

| Contenedor | Mínima | Recomendado |
|-----------|---------|-------------|
|cognitive-services-form-recognizer | 2 núcleos, 4 GB de memoria | 4 núcleos, 8 GB de memoria |

* Cada núcleo debe ser de 2,6 gigahercios (GHz) como mínimo.
* TPS: transacciones por segundo

El núcleo y la memoria se corresponden con los valores de `--cpus` y `--memory` que se usan como parte del comando `docker run`.

> [!Note]
> Los valores mínimos y recomendados se basan en los límites de Docker y *no* de los recursos de la máquina host.

## <a name="get-the-container-image-with-docker-pull-command"></a>Obtención de la imagen de contenedor con el comando docker pull

Hay imágenes de contenedor disponibles para Form Recognizer.

| Contenedor | Repositorio |
|-----------|------------|
| cognitive-services-form-recognizer | `containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer:latest` |

Si va a usar el `cognitive-services-recognize-text`[contenedor](../Computer-vision/computer-vision-how-to-install-containers.md##get-the-container-image-with-docker-pull), en lugar del servicio Form Recognizer, asegúrese de utilizar el comando `docker pull` con el nombre de contenedor correcto: 

```
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:latest
```

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]


### <a name="docker-pull-for-the-form-recognizer-container"></a>Extracción de Docker para el contenedor de Form Recognizer

#### <a name="form-recognizer"></a>Form Recognizer

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer:latest
```

## <a name="how-to-use-the-container"></a>Uso del contenedor

Una vez que el contenedor esté en el [equipo host](#the-host-computer), utilice el siguiente proceso para trabajar con el contenedor.

1. [Ejecute el contenedor](#run-the-container-with-docker-run) con la configuración de facturación requerida pero no utilizada. Hay más [ejemplos](form-recognizer-container-configuration.md#example-docker-run-commands) del comando `docker run` disponibles.
1. [Consulta del punto de conexión de predicción del contenedor](#query-the-containers-prediction-endpoint).

## <a name="run-the-container-with-docker-run"></a>Ejecute el contenedor con `docker run`.

Utilice el comando [docker run](https://docs.docker.com/engine/reference/commandline/run/) para ejecutar cualquiera de los tres contenedores. El comando usa los parámetros siguientes:

| Marcador de posición | Valor |
|-------------|-------|
|{BILLING_KEY} | Esta clave se usa para iniciar el contenedor y está disponible en página de claves de Form Recognizer en Azure Portal.  |
|{BILLING_ENDPOINT_URI} | El valor de URI del punto de conexión de facturación está disponible en Azure Portal, en la página de información general de Form Recognizer.|
|{COMPUTER_VISION_API_KEY}| La clave está disponible en la página Claves de la API Computer Vision de Azure Portal.|
|{COMPUTER_VISION_ENDPOINT_URI}|El punto de conexión de facturación. Si está utilizando un recurso de Computer Vision en la nube, el valor del identificador URI está disponible en la página de introducción de la API Computer Vision de Azure Portal. Si utiliza un contenedor `cognitive-services-recognize-text`, use la dirección URL del punto de conexión de facturación pasada al contenedor en el comando `docker run`.|

Reemplace estos parámetros con sus propios valores en el siguiente comando `docker run` de ejemplo.

### <a name="form-recognizer"></a>Form Recognizer

```bash
docker run --rm -it -p 5000:5000 --memory 8g --cpus 2 \
containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer \
--mount type=bind,source=c:\input,target=/input  \
--mount type=bind,source=c:\output,target=/output \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY} \
FormRecognizer:ComputerVisionApiKey={COMPUTER_VISION_API_KEY} \
FormRecognizer:ComputerVisionEndpointUri={COMPUTER_VISION_ENDPOINT_URI}
```

Este comando:

* Ejecuta un contenedor de Form Recognizer desde la imagen de contenedor.
* Asigna dos núcleos de CPU y 8 gigabytes (GB) de memoria.
* Expone el puerto TCP 5000 y asigna un seudo-TTY para el contenedor.
* Una vez que se produce la salida, quita automáticamente el contenedor. La imagen del contenedor sigue estando disponible en el equipo host.
* Monta un volumen /input y uno /output para el contenedor.

[!INCLUDE [Running multiple containers on the same host H2](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]

### <a name="run-separate-containers-as-separate-docker-run-commands"></a>Ejecución de contenedores diferentes como comandos de ejecución de Docker independientes

Para la combinación de Form Recognizer y del reconocedor de texto hospedados localmente en el mismo host, se muestran dos ejemplos de comandos de la CLI de Docker.

Ejecute el primer contenedor en el puerto 5000. 

```bash 
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
FormRecognizer:ComputerVisionApiKey={COMPUTER_VISION_API_KEY} \
FormRecognizer:ComputerVisionEndpointUri={COMPUTER_VISION_ENDPOINT_URI}
```

Ejecute el segundo contenedor en el puerto 5001.


```bash 
docker run --rm -it -p 5001:5000 --memory 4g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text \
Eula=accept \
Billing={COMPUTER_VISION_ENDPOINT_URI} \
ApiKey={COMPUTER_VISION_API_KEY}
```
Cada contenedor posterior debe estar en un puerto diferente. 

### <a name="run-separate-containers-with-docker-compose"></a>Ejecución de contenedores diferentes con Docker Compose

Para la combinación de Form Recognizer y del reconocedor de texto hospedados localmente en el mismo host, se muestra un ejemplo de archivo YAML de Docker Compose. El reconocedor de texto `{COMPUTER_VISION_API_KEY}` debe ser el mismo para los contenedores `formrecognizer` y `ocr`. `{COMPUTER_VISION_ENDPOINT_URI}` solo se utiliza en el contenedor `ocr` porque el contenedor `formrecognizer` utiliza el nombre y el puerto `ocr`. 

```docker
version: '3.3'
services:   
  ocr:
    image: "containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text"
    deploy:
      resources:
        limits:
          cpus: '2'
          memory: 8g
        reservations:
          cpus: '1'
          memory: 4g
    environment:
      eula: accept
      billing: "{COMPUTER_VISION_ENDPOINT_URI}"
      apikey: {COMPUTER_VISION_API_KEY}  

  formrecognizer:
    image: "containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer"
    deploy:
      resources:
        limits:
          cpus: '2'
          memory: 8g
        reservations:
          cpus: '1'
          memory: 4g
    environment:
      eula: accept
      billing: "{BILLING_ENDPOINT_URI}"
      apikey: {BILLING_KEY}
      FormRecognizer__ComputerVisionApiKey: {COMPUTER_VISION_API_KEY}
      FormRecognizer__ComputerVisionEndpointUri: "http://ocr:5000"
      FormRecognizer__SyncProcessTaskCancelLimitInSecs: 75
    links:
      - ocr
    volumes:
      - type: bind
        source: c:\output
        target: /output
      - type: bind
        source: c:\input
        target: /input
    ports:
      - "5000:5000"  
```


> [!IMPORTANT]
> Para poder ejecutar el contenedor, las opciones `Eula`, `Billing` y `ApiKey`, así como las opciones `FormRecognizer:ComputerVisionApiKey` y `FormRecognizer:ComputerVisionEndpointUri`, deben estar especificadas; de lo contrario, el contenedor no se iniciará.  Para obtener más información, vea [Facturación](#billing).

## <a name="query-the-containers-prediction-endpoint"></a>Consulta del punto de conexión de predicción del contenedor

|Contenedor|Punto de conexión|
|--|--|
|form-recognizer|http://localhost:5000


### <a name="form-recognizer"></a>Form Recognizer

El contenedor proporciona las API de punto de conexión de consulta basadas en WebSocket, a las que se accede mediante la [documentación del SDK de servicios de Form Recognizer](https://docs.microsoft.com/azure/cognitive-services/form-recognizer/).

De forma predeterminada, el SDK de Form Recognizer utiliza los servicios en línea. Para usar el contenedor, deberá cambiar el método de inicialización. Consulte los ejemplos siguientes.

#### <a name="for-c"></a>Para C#

Cambie de usar esta llamada de inicialización en la nube de Azure:

```C#
var config = FormRecognizerConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

por esta llamada mediante el punto de conexión de contenedor:

```C#
var config = FormRecognizerConfig.FromEndpoint("ws://localhost:5000/formrecognizer/v1.0-preview/custom", "YourSubscriptionKey");
```

#### <a name="for-python"></a>Para Python

Cambie de usar esta llamada de inicialización en la nube de Azure

```python
formrecognizer_config = formrecognizersdk.FormRecognizerConfig(subscription=formrecognizer_key, region=service_region)
```

por esta llamada mediante el punto de conexión de contenedor:

```python
formrecognizer_config = formrecognizersdk.FormRecognizerConfig(subscription=formrecognizer_key, endpoint="ws://localhost:5000/formrecognizer/v1.0-preview/custom"
```

### <a name="form-recognizer"></a>Form Recognizer

El contenedor proporciona API de punto de conexión de REST, que se pueden encontrar [aquí](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api/operations/AnalyzeWithCustomModel).


[!INCLUDE [Validate container is running - Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]


## <a name="stop-the-container"></a>Detención del contenedor

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>solución de problemas

Al ejecutar el contenedor, este usa **stdout** y **stderr** para generar información útil para solucionar problemas que se producen al inicio o ejecución del contenedor.

## <a name="billing"></a>Facturación

Los contenedores de Form Recognizer envían información de facturación a Azure mediante un recurso de _Form Recognizer_ en la cuenta de Azure.

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Para obtener más información acerca de estas opciones, consulte [Configure containers](form-recognizer-container-configuration.md) (Configuración de contenedores).

## <a name="summary"></a>Resumen

En este artículo, ha aprendido los conceptos y el flujo de trabajo para la descarga, instalación y ejecución de contenedores de Form Recognizer. En resumen:

* Form Recognizer proporciona un contenedor de Linux para Docker.
* Las imágenes de contenedor se descargan desde el registro de contenedor privado de Azure.
* Las imágenes de contenedor se ejecutan en Docker.
* Puede usar la API REST o el SDK para llamar a operaciones en contenedores de Form Recognizer mediante la especificación del URI del host del contenedor.
* Debe especificar la información de facturación al crear una instancia de un contenedor.

> [!IMPORTANT]
>  Los contenedores de Cognitive Services no tienen licencia para ejecutarse sin estar conectados a Azure para realizar mediciones. Los clientes tienen que habilitar los contenedores para comunicar la información de facturación con el servicio de medición en todo momento. Los contenedores de Cognitive Services no envían datos de los clientes (por ejemplo, la imagen o el texto que se está analizando) a Microsoft.

## <a name="next-steps"></a>Pasos siguientes

* Revise [Configure containers](form-recognizer-container-configuration.md) (Configuración de contenedores) para ver las opciones de configuración.
* Uso de [Contenedores de Cognitive Services](../cognitive-services-container-support.md)
