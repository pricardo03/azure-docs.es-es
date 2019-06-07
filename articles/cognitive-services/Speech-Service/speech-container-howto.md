---
title: Instalar los contenedores de voz
titleSuffix: Azure Cognitive Services
description: Instalar y ejecutar contenedores de voz. Voz a texto transcribe secuencias de audio a texto en tiempo real que sus aplicaciones, herramientas o dispositivos pueden usar o mostrar. Texto a voz convierte el texto de entrada en voz sintetizada similar a la humana.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: diberry
ms.openlocfilehash: 763e7bc9298eee1ab602968360bbc79a58243e5b
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66752446"
---
# <a name="install-and-run-speech-service-containers"></a>Instalar y ejecutar contenedores de servicios de voz

Contenedores de voz permiten que los clientes creen una arquitectura de aplicaciones de voz que se optimiza para aprovechar las capacidades de nube sólidas y localidad de edge. 

Los contenedores de dos voz son **texto a voz** y **texto a voz**. 

|Función|Características|Más reciente|
|-|-|--|
|Voz a texto| <li>Transcribe continua en tiempo real voz o proceso por lotes las grabaciones de audio en texto con los resultados intermedios.|1.1.1|
|Text-to-Speech| <li>Convierte el texto a una voz que parece natural. con la entrada de texto sin formato o lenguaje de marcado de síntesis de voz (SSML). |1.1.0|

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Debe cumplir los siguientes requisitos previos antes de utilizar contenedores de voz:

|Obligatorio|Propósito|
|--|--|
|Motor de Docker| Necesita que el motor de Docker esté instalado en un [equipo host](#the-host-computer). Docker dispone de paquetes que configuran el entorno de Docker en [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) y [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Para conocer los principios básicos de Docker y de los contenedores, consulte [Introducción a Docker](https://docs.docker.com/engine/docker-overview/).<br><br> Docker debe configurarse para permitir que los contenedores se conecten con Azure y envíen datos de facturación a dicho servicio. <br><br> **En Windows**, Docker también debe estar configurado de forma que admita los contenedores de Linux.<br><br>|
|Conocimientos sobre Docker | Debe tener conocimientos básicos sobre los conceptos de Docker, como los registros, los repositorios, los contenedores y las imágenes de contenedor, así como conocer los comandos `docker` básicos.| 
|Recursos de voz |Para usar estos contenedores, debe tener:<br><br>Un _voz_ recursos de Azure para obtener la clave de facturación asociada y el URI del extremo de facturación. Ambos valores están disponibles en el portal de Azure **voz** páginas de información general y las claves y son necesarios para iniciar el contenedor.<br><br>**{BILLING_KEY}** : clave de recurso<br><br>**{BILLING_ENDPOINT_URI}** : el ejemplo de URI de punto de conexión es `https://westus.api.cognitive.microsoft.com/sts/v1.0`|

## <a name="request-access-to-the-container-registry"></a>Solicitud de acceso al registro de contenedor

Primero debe completar y enviar la [formulario de solicitud de contenedores de Cognitive Services Speech](https://aka.ms/speechcontainerspreview/) para solicitar acceso al contenedor. 

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

## <a name="the-host-computer"></a>El equipo host

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="advanced-vector-extension-support"></a>Compatibilidad con las extensiones de Vector avanzada

El **host** es el equipo que ejecuta el contenedor de Docker. El host debe admitir [extensiones de Vector avanzadas](https://en.wikipedia.org/wiki/Advanced_Vector_Extensions#CPUs_with_AVX2) (AVX2). Puede comprobar esta compatibilidad en hosts de Linux con el siguiente comando: 

```console
grep -q avx2 /proc/cpuinfo && echo AVX2 supported || echo No AVX2 support detected
```

### <a name="container-requirements-and-recommendations"></a>Recomendaciones y requisitos del contenedor

La tabla siguiente describen los núcleos de CPU y memoria para asignar cada contenedor de voz mínimos y recomendados.

| Contenedor | Mínima | Recomendado |
|-----------|---------|-------------|
|cognitive-services-speech-to-text | 2 núcleos<br>2 GB de memoria  | 4 núcleos<br>4 GB de memoria  |
|cognitive-services-text-to-speech | 1 núcleo, 0,5 GB de memoria| 2 núcleos, 1 GB de memoria |

* Cada núcleo debe ser de 2,6 gigahercios (GHz) como mínimo.


El núcleo y la memoria se corresponden con los valores de `--cpus` y `--memory` que se usan como parte del comando `docker run`.

**Tenga en cuenta**; Mínimos y recomendados se basan en los límites de Docker, *no* los recursos del equipo host. Por ejemplo, algunas partes de los contenedores de voz a texto memoria mapa de un modelo de lenguaje grandes y es _recomienda_ que se ajusta todo el archivo en memoria, que es de 4 a 6 GB adicionales. Además, la primera ejecución de cualquier contenedor puede tardar más, dado que los modelos que se va a se paginan en la memoria.

## <a name="get-the-container-image-with-docker-pull"></a>Obtención de la imagen del contenedor con `docker pull`

Imágenes de contenedor para voz están disponibles. 

| Contenedor | Repositorio |
|-----------|------------|
| cognitive-services-speech-to-text | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text:latest` |
| cognitive-services-text-to-speech | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech:latest` |

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="language-locale-is-in-container-tag"></a>Configuración regional de idioma se encuentra en la etiqueta de contenedor

El `latest` etiqueta extrae la `en-us` configuración regional y `jessarus` voz. 

#### <a name="speech-to-text-locales"></a>Conversión de voz en las configuraciones regionales de texto

Todas las etiquetas, excepto para `latest` tienen el formato siguiente, donde el `<culture>` indica el contenedor de configuración regional:

```
<major>.<minor>.<patch>-<platform>-<culture>-<prerelease>
```

La etiqueta a la siguiente es un ejemplo del formato:

```
1.0.0-amd64-en-us-preview
```

En la tabla siguiente se enumera las configuraciones regionales admitidas para **texto a voz** en el 1.1.1 versión del contenedor:

|Configuración regional de idioma|`Tags`|
|--|--|
|Chino|`zh-cn`|
|English |`en-us`<br>`en-gb`<br>`en-au`<br>`en-in`|
|Francés |`fr-ca`<br>`fr-fr`|
|Alemán|`de-de`|
|Italiano|`it-it`|
|Japonés|`ja-jp`|
|Coreano|`ko-kr`|
|Portugués|`pt-br`|
|Español|`es-es`<br>`es-mx`|


#### <a name="text-to-speech-locales"></a>Configuraciones regionales de texto a voz

Todas las etiquetas, excepto para `latest` tienen el formato siguiente, donde el `<culture>` indica la configuración regional y la `<voice>` indica la voz del contenedor:

```
<major>.<minor>.<patch>-<platform>-<culture>-<voice>-<prerelease>
```

La etiqueta a la siguiente es un ejemplo del formato:

```
1.0.0-amd64-en-us-jessarus-preview
```

En la tabla siguiente se enumera las configuraciones regionales admitidas para **texto a voz** en la 1.1.0 versión del contenedor:

|Configuración regional de idioma|`Tags`|Voces admitidas|
|--|--|--|
|Chino|`zh-cn`|huihuirus<br>kangkang-apollo<br>yaoyao-apollo|
|English |`en-au`|Catherine<br>hayleyrus|
|English |`en-gb`|george-apollo<br>hazelrus<br>susan-apollo|
|English |`en-in`|heera-apollo<br>priyarus<br>Ravi apollo<br>|
|English |`en-us`|jessarus<br>benjaminrus<br>jessa24krus<br>zirarus<br>guy24krus|
|Francés|`fr-ca`|caroline<br>harmonierus|
|Francés|`fr-fr`|hortenserus<br>julie-apollo<br>paul-apollo|
|Alemán|`de-de`|hedda<br>heddarus<br>Stefan apollo|
|Italiano|`it-it`|cosimo apollo<br>luciarus|
|Japonés|`ja-jp`|ayumi-apollo<br>harukarus<br>ichiro-apollo|
|Coreano|`ko-kr`|heamirus|
|Portugués|`pt-br`|daniel-apollo<br>heloisarus|
|Español|`es-es`|elenarus<br>laura-apollo<br>pablo-apollo<br>|
|Español|`es-mx`|hildarus<br>raul-apollo|

### <a name="docker-pull-for-the-speech-containers"></a>Extracción de docker para los contenedores de voz

#### <a name="speech-to-text"></a>Voz a texto

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text:latest
```

#### <a name="text-to-speech"></a>Texto a voz

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech:latest
```

## <a name="how-to-use-the-container"></a>Uso del contenedor

Una vez que el contenedor esté en el [equipo host](#the-host-computer), utilice el siguiente proceso para trabajar con el contenedor.

1. [Ejecute el contenedor](#run-the-container-with-docker-run) con la configuración de facturación requerida pero no utilizada. Hay más [ejemplos](speech-container-configuration.md#example-docker-run-commands) del comando `docker run` disponibles. 
1. [Consulta del punto de conexión de predicción del contenedor](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>Ejecute el contenedor con `docker run`.

Utilice el comando [docker run](https://docs.docker.com/engine/reference/commandline/run/) para ejecutar cualquiera de los tres contenedores. El comando usa los parámetros siguientes:

**Durante la versión preliminar**, la configuración de facturación debe ser válida para iniciar el contenedor, pero no se factura el uso.

| Marcador de posición | Valor |
|-------------|-------|
|{BILLING_KEY} | Esta clave se usa para iniciar el contenedor y está disponible en la página de claves de voz del portal de Azure.  |
|{BILLING_ENDPOINT_URI} | El valor del identificador URI del punto de conexión facturación está disponible en la página de introducción a la voz del portal de Azure.|

Reemplace estos parámetros con sus propios valores en el siguiente comando `docker run` de ejemplo.

### <a name="text-to-speech"></a>Texto a voz

```bash
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY} 
```

### <a name="speech-to-text"></a>Voz a texto

```bash
docker run --rm -it -p 5000:5000 --memory 2g --cpus 2 \
containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY} 
```

Este comando:

* Se ejecuta un contenedor de voz desde la imagen de contenedor
* Asigna 2 núcleos de CPU y 2 gigabytes (GB) de memoria
* Expone el puerto TCP 5000 y asigna un seudo-TTY para el contenedor.
* Una vez que se produce la salida, quita automáticamente el contenedor. La imagen del contenedor sigue estando disponible en el equipo host. 

> [!IMPORTANT]
> Para poder ejecutar el contenedor, las opciones `Eula`, `Billing` y `ApiKey` deben estar especificadas; de lo contrario, el contenedor no se iniciará.  Para obtener más información, vea [Facturación](#billing).

## <a name="query-the-containers-prediction-endpoint"></a>Consulta del punto de conexión de predicción del contenedor

|Contenedor|Punto de conexión|
|--|--|
|Voz a texto|ws://localhost:5000/speech/recognition/dictation/cognitiveservices/v1|
|Texto a voz|http://localhost:5000/speech/synthesize/cognitiveservices/v1|

### <a name="speech-to-text"></a>Voz a texto

El contenedor proporciona el punto de conexión de consulta basada en websocket API, que se accede mediante el [Speech SDK](index.yml).

De forma predeterminada, el SDK de voz usa servicios de voz en línea. Para usar el contenedor, deberá cambiar el método de inicialización. Consulte los ejemplos siguientes.

#### <a name="for-c"></a>Para C#

Cambie de usar esta llamada de inicialización en la nube de Azure:

```C#
var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

por esta llamada mediante el punto de conexión de contenedor:

```C#
var config = SpeechConfig.FromEndpoint("ws://localhost:5000/speech/recognition/dictation/cognitiveservices/v1", "YourSubscriptionKey");
```

#### <a name="for-python"></a>Para Python

Cambie de usar esta llamada de inicialización en la nube de Azure

```python
speech_config = speechsdk.SpeechConfig(subscription=speech_key, region=service_region)
```

por esta llamada mediante el punto de conexión de contenedor:

```python
speech_config = speechsdk.SpeechConfig(subscription=speech_key, endpoint="ws://localhost:5000/speech/recognition/dictation/cognitiveservices/v1")
```

### <a name="text-to-speech"></a>Texto a voz

El contenedor proporciona el punto de conexión REST API que pueden encontrarse [aquí](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-text-to-speech) y puede encontrar ejemplos de [aquí](https://azure.microsoft.com/resources/samples/cognitive-speech-tts/).


[!INCLUDE [Validate container is running - Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]


## <a name="stop-the-container"></a>Detención del contenedor

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>solución de problemas

Al ejecutar el contenedor, este usa **stdout** y **stderr** para generar información útil para solucionar problemas que se producen al inicio o ejecución del contenedor. 

## <a name="billing"></a>Facturación

El envío de contenedores de voz, información de facturación para Azure, mediante un _voz_ recursos en su cuenta de Azure. 

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Para obtener más información acerca de estas opciones, consulte [Configure containers](speech-container-configuration.md) (Configuración de contenedores).

<!--blogs/samples/video coures -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Resumen

En este artículo, ha aprendido los conceptos y flujo de trabajo para descargar, instalar y ejecutar contenedores de voz. En resumen:

* Voz proporciona dos contenedores de Linux docker, que encapsula la conversión de voz en texto y texto a voz.
* Las imágenes de contenedor se descargan desde el registro de contenedor privado de Azure.
* Las imágenes de contenedor se ejecutan en Docker.
* Puede usar la API de REST o SDK para llamar a operaciones en contenedores de voz especificando el URI del contenedor del host.
* Debe especificar la información de facturación al crear una instancia de un contenedor.

> [!IMPORTANT]
>  Los contenedores de Cognitive Services no tienen licencia para ejecutarse sin estar conectados a Azure para realizar mediciones. Los clientes tienen que habilitar los contenedores para comunicar la información de facturación con el servicio de medición en todo momento. Los contenedores de Cognitive Services no envían datos de los clientes (por ejemplo, la imagen o el texto que se está analizando) a Microsoft.

## <a name="next-steps"></a>Pasos siguientes

* Revise [Configure containers](speech-container-configuration.md) (Configuración de contenedores) para ver las opciones de configuración.
* Uso de [Contenedores de Cognitive Services](../cognitive-services-container-support.md)
