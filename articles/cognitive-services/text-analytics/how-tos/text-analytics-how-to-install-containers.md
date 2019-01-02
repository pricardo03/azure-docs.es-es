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
ms.date: 11/14/2018
ms.author: diberry
ms.openlocfilehash: 11798c3bfd4032ad10c738032a816a2a0488ce67
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53090540"
---
# <a name="install-and-run-containers"></a>Instalación y ejecución de contenedores

La creación de contenedores es un enfoque de distribución de software en el que una aplicación o servicio se empaqueta como una imagen de contenedor. La configuración y las dependencias de la aplicación o del servicio se incluyen en la imagen de contenedor. La imagen de contenedor puede implementarse en un host de contenedor con pocas modificaciones o ninguna. Los contenedores están aislados entre sí y del sistema operativo subyacente, con una superficie menor que una máquina virtual. Se pueden crear instancias de contenedores a partir de las imágenes de contenedor para las tareas a corto plazo y quitarlas cuando ya no se necesiten.

Text Analytics proporciona el siguiente conjunto de contenedores de Docker, cada uno de los cuales contiene un subconjunto de funcionalidades:

| Contenedor| DESCRIPCIÓN |
|----------|-------------|
|Extracción de frases clave | Extrae las frases clave para identificar los puntos principales. Por ejemplo, si el texto de entrada es "La comida estaba deliciosa y el personal era maravilloso", la API devuelve los principales puntos de conversación: "comida" y "personal maravilloso". |
|Detección de idiomas | Hasta 120 idiomas, detecta y notifica en qué idioma está escrito el texto de entrada. El contenedor informa de un código de idioma único para cada documento que se incluye en la solicitud. El código de idioma se empareja con una puntuación que indica la intensidad de esta. |
|Análisis de sentimiento | Analiza el texto sin formato para obtener pistas sobre opiniones positivas o negativas. Esta API devuelve una puntuación de la opción, que oscila entre 0 y 1, con respecto a cada documento, donde 1 es la más positiva. Los modelos de análisis se entrenan previamente con una gran cantidad de cuerpo de texto y tecnologías de idioma natural de Microsoft. Para los [idiomas seleccionados](../language-support.md), la API puede analizar y puntuar cualquier texto sin formato que se proporcione, y devolver los resultados directamente a la aplicación que realiza la llamada. |

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="preparation"></a>Preparación

Debe cumplir los siguientes requisitos previos para poder usar contenedores de Text Analytics:

**Motor de Docker**: debe tener el motor de Docker instalado localmente. Docker proporciona paquetes que configuran el entorno de Docker en [macOS](https://docs.docker.com/docker-for-mac/), [Linux](https://docs.docker.com/engine/installation/#supported-platforms) y [Windows](https://docs.docker.com/docker-for-windows/). En Windows, Docker debe configurarse para admitir los contenedores de Linux. Los contenedores de Docker también se pueden implementar directamente en [Azure Kubernetes Service](/azure/aks/), [Azure Container Instances](/azure/container-instances/) o en un clúster de [Kubernetes](https://kubernetes.io/) implementado en [Azure Stack](/azure/azure-stack/). Para obtener más información sobre la implementación de Kubernetes en Azure Stack, consulte [Implementación de Kubernetes en Azure Stack](/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy).

Docker debe configurarse para permitir que los contenedores se conecten con Azure y envíen datos de facturación a dicho servicio.

**Familiaridad con Microsoft Container Registry y Docker**: debe tener un conocimiento básico de los conceptos de Microsoft Container Registry y Docker, como los registros, los repositorios, los contenedores y las imágenes de contenedor, así como de los comandos `docker` básicos.  

Para conocer los principios básicos de Docker y de los contenedores, consulte [Introducción a Docker](https://docs.docker.com/engine/docker-overview/).

### <a name="container-requirements-and-recommendations"></a>Recomendaciones y requisitos del contenedor

En la tabla siguiente, se describe el número mínimo y recomendado de núcleos de CPU, por lo menos 2,6 gigahercios (GHz) o con mayor rapidez, y de memoria, en gigabytes (GB), para asignar a cada contenedor de Text Analytics.

| Contenedor | Mínima | Recomendado |
|-----------|---------|-------------|
|Extracción de frases clave | 1 núcleo, 2 GB de memoria | 1 núcleo, 4 GB de memoria |
|Detección de idiomas | 1 núcleo, 2 GB de memoria | 1 núcleo, 4 GB de memoria |
|Análisis de sentimiento | 1 núcleo, 2 GB de memoria | 1 núcleo, 4 GB de memoria |

## <a name="download-container-images-from-microsoft-container-registry"></a>Descarga de imágenes de contenedor de Microsoft Container Registry

Las imágenes de contenedor de Text Analytics están disponibles en Microsoft Container Registry. En la tabla siguiente se enumeran los repositorios disponibles de Microsoft Container Registry para los contenedores de Text Analytics. Cada repositorio contiene una imagen de contenedor, que debe descargarse para ejecutar el contenedor localmente.

| Contenedor | Repositorio |
|-----------|------------|
|Extracción de frases clave | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |
|Detección de idiomas | `mcr.microsoft.com/azure-cognitive-services/language` |
|Análisis de sentimiento | `mcr.microsoft.com/azure-cognitive-services/sentiment` |

Use el comando [docker pull](https://docs.docker.com/engine/reference/commandline/pull/) para descargar una imagen de contenedor desde un repositorio. Por ejemplo, para descargar la imagen de contenedor del contenedor de Extracción de frases clave más reciente desde el repositorio, use el siguiente comando:

```Docker
docker pull mcr.microsoft.com/azure-cognitive-services/keyphrase:latest
```

Para obtener una descripción completa de las etiquetas disponibles para los contenedores de Text Analytics, consulte los siguientes contenedores en Docker Hub:

* [Extracción de frases clave](https://go.microsoft.com/fwlink/?linkid=2018757)
* [Detección de idioma](https://go.microsoft.com/fwlink/?linkid=2018759)
* [Análisis de sentimiento](https://go.microsoft.com/fwlink/?linkid=2018654)

> [!TIP]
> Puede usar el comando [docker images](https://docs.docker.com/engine/reference/commandline/images/) para enumerar las imágenes de contenedor descargadas. Por ejemplo, el comando siguiente muestra el id., el repositorio y la etiqueta de cada imagen de contenedor descargada, con formato de tabla:
>
>  ```Docker
>  docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
>  ```
>

## <a name="instantiate-a-container-from-a-downloaded-container-image"></a>Creación de instancias de contenedores a partir de una imagen de contenedor descargada

Use el comando [docker run](https://docs.docker.com/engine/reference/commandline/run/) para crear una instancia de un contenedor a partir de una imagen de contenedor descargada. Por ejemplo, el siguiente comando:

* Crea una instancia de un contenedor a partir de la imagen de contenedor de Análisis de sentimiento.
* Asigna un núcleo de CPU y 8 gigabytes (GB) de memoria.
* Expone el puerto TCP 5000 y asigna un seudo-TTY para el contenedor.
* Quita automáticamente el contenedor tras la salida.

```Docker
docker run --rm -it -p 5000:5000 --memory 8g --cpus 1 mcr.microsoft.com/azure-cognitive-services/sentiment Eula=accept Billing=https://westus.api.cognitive.microsoft.com/text/analytics/v2.0 ApiKey=0123456789

```

> [!IMPORTANT]
> Las opciones de la línea de comandos `Eula`, `Billing` y `ApiKey` se deben especificar para crear una instancia del contenedor; de lo contrario, este no se iniciará.  Para obtener más información, vea [Facturación](#billing).

Una vez creada la instancia, puede llamar a las operaciones del contenedor mediante el uso del URI del host del contenedor. Por ejemplo, el siguiente URI del host representa el contenedor de Análisis de sentimiento del que se creó una instancia en el ejemplo anterior:

```http
http://localhost:5000/
```

> [!TIP]
> Puede tener acceso a la [especificación OpenAPI](https://swagger.io/docs/specification/about/) (anteriormente, especificación de Swagger), que describe las operaciones admitidas por un contenedor con instancias, desde el URI relativo `/swagger` de ese contenedor. Por ejemplo, el siguiente URI proporciona acceso a la especificación de OpenAPI del contenedor de Análisis de sentimiento del que se creó una instancia en el ejemplo anterior:
>
>  ```http
>  http://localhost:5000/swagger
>  ```

Puede [llamar a las operaciones de API REST](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-call-api) disponibles del contenedor o usar la biblioteca cliente del [SDK de Text Analytics de Azure Cognitive Services](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics) para llamar a dichas operaciones.  
> [!IMPORTANT]
> Debe tener la versión 2.1.0 o posterior del SDK de Text Analytics de Azure Cognitive Services si quiere usar la biblioteca cliente con el contenedor.

La única diferencia entre llamar a una operación determinada del contenedor y llamar a la misma operación desde un servicio correspondiente en Azure es que usará el URI del host del contenedor, en lugar del URI del host de una región de Azure, para llamar a la operación. Por ejemplo, si quisiese usar una instancia de Text Analytics que se ejecutase en la región de Azure Oeste de EE. UU., llamaría a la siguiente operación de API REST:

```http
POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases
```

Si quisiese utilizar un contenedor de Extracción de frases clave que se ejecutase en el equipo local según su configuración predeterminada, llamaría a la siguiente operación de API REST:

```http
POST http://localhost:5000/text/analytics/v2.0/keyPhrases
```

### <a name="billing"></a>Facturación

Los contenedores de Text Analytics envían información de facturación a Azure mediante un recurso correspondiente de Text Analytics en la cuenta de Azure. Los contenedores de Text Analytics usan las siguientes opciones de la línea de comandos para la facturación:

| Opción | DESCRIPCIÓN |
|--------|-------------|
| `ApiKey` | Clave de API del recurso de Text Analytics utilizado para realizar un seguimiento de la información de facturación.<br/>El valor de esta opción debe establecerse en una clave de API para el recurso aprovisionado de Azure de Text Analytics especificado en `Billing`. |
| `Billing` | Punto de conexión del recurso de Text Analytics utilizado para realizar un seguimiento de la información de facturación.<br/>El valor de esta opción debe establecerse en el URI del punto de conexión de un recurso aprovisionado de Azure de Text Analytics.|
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
* Revise la [Introducción a Text Analytics](../overview.md) para obtener más información acerca de la detección de frases clave, la detección de idioma y el análisis de sentimiento.  
* Consulte [Text Analytics API](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) para obtener más información acerca de los métodos que admite el contenedor.
* Consulte [Preguntas más frecuentes (P+F)](../text-analytics-resource-faq.md) para resolver los problemas relacionados con la funcionalidad de Computer Vision.