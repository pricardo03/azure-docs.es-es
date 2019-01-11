---
title: Instalación y ejecución de contenedores
titleSuffix: Text Analytics -  Azure Cognitive Services
description: Cómo descargar, instalar y ejecutar contenedores para Text Analytics en este tutorial paso a paso.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 01/02/2019
ms.author: diberry
ms.openlocfilehash: e3b1655207f3baba6ea6e3cf2f00e3540a3602ad
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/02/2019
ms.locfileid: "53969376"
---
# <a name="install-and-run-text-analytics-containers"></a>Instalación y ejecución de contenedores de Text Analytics

Los contenedores de Text Analytics proporcionan un procesamiento avanzado de lenguaje natural sobre texto sin formato, e incluye tres funciones principales: análisis de sentimiento, extracción de frases clave y detección de idioma. Actualmente no se admite la vinculación de entidad en un contenedor. 

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para poder ejecutar cualquiera de los contenedores de Text Analytics, debe tener lo siguiente:

## <a name="preparation"></a>Preparación

Debe cumplir los siguientes requisitos previos para poder usar contenedores de Text Analytics:

|Obligatorio|Propósito|
|--|--|
|Motor de Docker| Necesita que el motor de Docker esté instalado en un [equipo host](#the-host-computer). Docker dispone de paquetes que configuran el entorno de Docker en [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) y [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Para conocer los principios básicos de Docker y de los contenedores, consulte [Introducción a Docker](https://docs.docker.com/engine/docker-overview/).<br><br> Docker debe configurarse para permitir que los contenedores se conecten con Azure y envíen datos de facturación a dicho servicio. <br><br> **En Windows**, Docker también debe estar configurado de forma que admita los contenedores de Linux.<br><br>|
|Conocimientos sobre Docker | Debe tener conocimientos básicos sobre los conceptos de Docker, como los registros, los repositorios, los contenedores y las imágenes de contenedor, así como conocer los comandos `docker` básicos.| 
|Recurso de Text Analytics |Para poder usar el contenedor, debe tener:<br><br>Un recurso de Azure de [_Text Analytics_](text-analytics-how-to-access-key.md) para obtener la clave de facturación asociada y el URI del punto de conexión de facturación. Ambos valores están disponibles en las páginas de claves y de información general de Text Analytics en Azure Portal y son necesarios para iniciar el contenedor.<br><br>**{BILLING_KEY}**: clave de recurso<br><br>**{BILLING_ENDPOINT_URI}**: el ejemplo de URI de punto de conexión es `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0`|

### <a name="the-host-computer"></a>El equipo host

El **host** es el equipo que ejecuta el contenedor de Docker. Puede ser un equipo del entorno local o un servicio de hospedaje de Docker incluido en Azure, como:

* [Azure Kubernetes Service](../../../aks/index.yml)
* [Azure Container Instances](../../../container-instances/index.yml)
* Un clúster de [Kubernetes](https://kubernetes.io/) implementado en [Azure Stack](../../../azure-stack/index.yml). Para obtener más información, consulte [Implementación de Kubernetes en Azure Stack](../../../azure-stack/user/azure-stack-solution-template-kubernetes-deploy.md).


### <a name="container-requirements-and-recommendations"></a>Recomendaciones y requisitos del contenedor

En la tabla siguiente, se describe el número mínimo y recomendado de núcleos de CPU, por lo menos 2,6 gigahercios (GHz) o con mayor rapidez, y de memoria, en gigabytes (GB), para asignar a cada contenedor de Text Analytics.

| Contenedor | Mínima | Recomendado |
|-----------|---------|-------------|
|Extracción de frases clave | 1 núcleo, 2 GB de memoria | 1 núcleo, 4 GB de memoria |
|Detección de idiomas | 1 núcleo, 2 GB de memoria | 1 núcleo, 4 GB de memoria |
|Análisis de sentimiento | 1 núcleo, 2 GB de memoria | 1 núcleo, 4 GB de memoria |

El núcleo y la memoria se corresponden con los valores de `--cpus` y `--memory` que se usan como parte del comando `docker run`.

## <a name="get-the-container-image-with-docker-pull"></a>Obtención de la imagen del contenedor con `docker pull`

Las imágenes de contenedor de Text Analytics están disponibles en Microsoft Container Registry. 

| Contenedor | Repositorio |
|-----------|------------|
|Extracción de frases clave | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |
|Detección de idiomas | `mcr.microsoft.com/azure-cognitive-services/language` |
|Análisis de sentimiento | `mcr.microsoft.com/azure-cognitive-services/sentiment` |

Utilice el comando [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) para descargar la imagen de contenedor del Registro de contenedor de Microsoft.

Para obtener una descripción completa de las etiquetas disponibles para los contenedores de Text Analytics, consulte los siguientes contenedores en Docker Hub:

* [Extracción de frases clave](https://go.microsoft.com/fwlink/?linkid=2018757)
* [Detección de idioma](https://go.microsoft.com/fwlink/?linkid=2018759)
* [Análisis de sentimiento](https://go.microsoft.com/fwlink/?linkid=2018654)


### <a name="docker-pull-for-the-key-phrase-extraction-container"></a>Docker pull para el contenedor de extracción de frases clave

```Docker
docker pull mcr.microsoft.com/azure-cognitive-services/keyphrase:latest
```

### <a name="docker-pull-for-the-language-detection-container"></a>Docker pull para el contenedor de detección de idioma

```Docker
docker pull mcr.microsoft.com/azure-cognitive-services/language:latest
```

### <a name="docker-pull-for-the-sentiment-container"></a>Docker pull para el contenedor de opiniones

```Docker
docker pull mcr.microsoft.com/azure-cognitive-services/sentiment:latest
```

### <a name="listing-the-containers"></a>Enumeración de los contenedores

Puede usar el comando [docker images](https://docs.docker.com/engine/reference/commandline/images/) para enumerar las imágenes de contenedor descargadas. Por ejemplo, el comando siguiente muestra el id., el repositorio y la etiqueta de cada imagen de contenedor descargada, con formato de tabla:

```Docker
docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
```


## <a name="how-to-use-the-container"></a>Uso del contenedor

Una vez que el contenedor esté en el [equipo host](#the-host-computer), utilice el siguiente proceso para trabajar con el contenedor.

1. [Ejecute el contenedor](#run-the-container-with-docker-run) con la configuración de facturación requerida. Hay más [ejemplos](../text-analytics-resource-container-config.md#example-docker-run-commands) del comando `docker run` disponibles. 
1. [Consulta del punto de conexión de predicción del contenedor](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>Ejecute el contenedor con `docker run`.

Utilice el comando [docker run](https://docs.docker.com/engine/reference/commandline/run/) para ejecutar cualquiera de los tres contenedores. El comando usa los parámetros siguientes:

| Marcador de posición | Valor |
|-------------|-------|
|{BILLING_KEY} | Esta clave se usa para iniciar el contenedor y está disponible en página de claves de Text Analytics en Azure Portal.  |
|{BILLING_ENDPOINT_URI} | El valor de URI del punto de conexión de facturación está disponible en Azure Portal, en la página de información general de Text Analytics.|

Reemplace estos parámetros con sus propios valores en el siguiente comando `docker run` de ejemplo.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/keyphrase \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Este comando:

* Ejecuta un contenedor de frases clave desde la imagen de contenedor.
* Asigna un núcleo de CPU y 4 gigabytes (GB) de memoria.
* Expone el puerto TCP 5000 y asigna un seudo-TTY para el contenedor.
* Una vez que se produce la salida, quita automáticamente el contenedor. La imagen del contenedor sigue estando disponible en el equipo host. 

Hay más [ejemplos](../text-analytics-resource-container-config.md#example-docker-run-commands) del comando `docker run` disponibles. 

> [!IMPORTANT]
> Para poder ejecutar el contenedor, las opciones `Eula`, `Billing` y `ApiKey` deben estar especificadas; de lo contrario, el contenedor no se iniciará.  Para obtener más información, vea [Facturación](#billing).

## <a name="query-the-containers-prediction-endpoint"></a>Consulta del punto de conexión de predicción del contenedor

El contenedor proporciona varias API de puntos de conexión de predicción de consultas basadas en REST. 

Utilice el host, https://localhost:5000, con las API de contenedor.

## <a name="stop-the-container"></a>Detención del contenedor

[!INCLUDE [How to stop the container](../../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>solución de problemas

Si ejecuta el contenedor con un [montaje](../text-analytics-resource-container-config.md#mount-settings) de salida y el registro habilitados, el contenedor genera archivos de registro que resultan útiles para solucionar problemas que se producen al iniciar o ejecutar el contenedor. 

## <a name="containers-api-documentation"></a>Documentación de las API del contenedor

El contenedor cuenta con un completo conjunto de documentación sobre los puntos de conexión, así como una característica `Try it now`. Esta característica le permite especificar la configuración en un formulario HTML basado en Web y realizar la consulta sin necesidad de escribir código. Una vez que la consulta devuelve resultados, se proporciona un ejemplo del comando CURL para mostrar los encabezados HTTP y el formato de cuerpo requeridos. 

> [!TIP]
> Consulte la [especificación de OpenAPI](https://swagger.io/docs/specification/about/), donde se describen las operaciones API que se admiten en el contenedor desde el URI relativo `/swagger`. Por ejemplo: 
>
>  ```http
>  http://localhost:5000/swagger
>  ```

## <a name="billing"></a>Facturación

Los contenedores de Text Analytics envían información de facturación a Azure mediante un recurso de _Text Analytics_ en la cuenta de Azure. 

Los contenedores de Cognitive Services no tienen licencia para ejecutarse sin estar conectados a Azure para realizar mediciones. Los clientes tienen que habilitar los contenedores para comunicar la información de facturación con el servicio de medición en todo momento. Los contenedores de Cognitive Services no envían los datos del cliente a Microsoft. 

El comando `docker run` utiliza los siguientes argumentos para la facturación:

| Opción | DESCRIPCIÓN |
|--------|-------------|
| `ApiKey` | Clave de API del recurso de _Text Analytics_ utilizado para realizar un seguimiento de la información de facturación. |
| `Billing` | Punto de conexión del recurso de _Text Analytics_ utilizado para realizar un seguimiento de la información de facturación.|
| `Eula` | Indica que ha aceptado la licencia del contenedor.<br/>El valor de esta opción debe establecerse en `accept`. |

> [!IMPORTANT]
> Las tres opciones deben especificarse con valores válidos; en caso contrario, no se inicia el contenedor.

Para obtener más información acerca de estas opciones, consulte [Configure containers](../text-analytics-resource-container-config.md) (Configuración de contenedores).

## <a name="summary"></a>Resumen

En este artículo, ha aprendido los conceptos y el flujo de trabajo para la descarga, instalación y ejecución de contenedores de Text Analytics. En resumen:

* Text Analytics proporciona tres contenedores de Linux para Docker, que encapsulan la extracción de frases clave, la detección de idioma y el análisis de sentimiento.
* Las imágenes del contenedor se descargan desde Microsoft Container Registry (MCR) en Azure.
* Las imágenes de contenedor se ejecutan en Docker.
* Puede usar la API REST o el SDK para llamar a operaciones en contenedores de Text Analytics mediante la especificación del URI del host del contenedor.
* Debe especificar la información de facturación al crear una instancia de un contenedor.

> [!IMPORTANT]
> Los contenedores de Cognitive Services no tienen licencia para ejecutarse sin estar conectados a Azure para realizar mediciones. Los clientes tienen que habilitar los contenedores para comunicar la información de facturación con el servicio de medición en todo momento. Los contenedores de Cognitive Services no envían datos de los clientes (por ejemplo, la imagen o el texto que se está analizando) a Microsoft.

## <a name="next-steps"></a>Pasos siguientes

* Revise [Configure containers](../text-analytics-resource-container-config.md) (Configuración de contenedores) para ver las opciones de configuración.
* Consulte [Preguntas más frecuentes (P+F)](../text-analytics-resource-faq.md) para resolver problemas relacionados con la funcionalidad.

