---
title: Instalación y ejecución de contenedores
titlesuffix: Face - Azure Cognitive Services
description: Cómo descargar, instalar y ejecutar contenedores para Face en este tutorial paso a paso.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: article
ms.date: 03/22/2019
ms.author: diberry
ms.openlocfilehash: 3e6b220e7193c5e683fc8a6c06a6e9e3dd3e3f6e
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58521625"
---
# <a name="install-and-run-face-containers"></a>Instalar y ejecutar contenedores de cara

Face proporciona un contenedor de Linux estandarizado para Docker, denominado Face, que detecta caras humanas en imágenes e identifica atributos, incluidos faciales (como narices y ojos), sexo, edad y otras características faciales previstas por la máquina. Además de la detección, Face puede comprobar si dos caras en la misma o en diferentes imágenes son iguales mediante una puntuación de confianza, o bien comparar caras en una base de datos para ver si ya existe un aspecto similar o una cara idéntica. También puede organizar caras similares en grupos mediante rasgos visuales compartidos.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Debe cumplir los siguientes requisitos previos para poder usar los contenedores de Face API:

|Obligatorio|Propósito|
|--|--|
|Motor de Docker| Necesita que el motor de Docker esté instalado en un [equipo host](#the-host-computer). Docker dispone de paquetes que configuran el entorno de Docker en [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) y [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Para conocer los principios básicos de Docker y de los contenedores, consulte [Introducción a Docker](https://docs.docker.com/engine/docker-overview/).<br><br> Docker debe configurarse para permitir que los contenedores se conecten con Azure y envíen datos de facturación a dicho servicio. <br><br> **En Windows**, Docker también debe estar configurado de forma que admita los contenedores de Linux.<br><br>|
|Conocimientos sobre Docker | Debe tener conocimientos básicos sobre los conceptos de Docker, como los registros, los repositorios, los contenedores y las imágenes de contenedor, así como conocer los comandos `docker` básicos.| 
|Recursos de Face API |Para poder usar el contenedor, debe tener:<br><br>Un recurso de Azure de _Face API_ para obtener la clave de facturación asociada y el URI del punto de conexión de facturación. Ambos valores están disponibles en las páginas de claves y de información general de Face API en Azure Portal y son necesarios para iniciar el contenedor.<br><br>**{BILLING_KEY}**: clave de recurso<br><br>**{BILLING_ENDPOINT_URI}**: el ejemplo de URI de punto de conexión es `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0`|


## <a name="request-access-to-the-private-container-registry"></a>Solicitud de acceso al registro de contenedor privado

[!INCLUDE [Request access to private preview](../../../includes/cognitive-services-containers-request-access.md)]

### <a name="the-host-computer"></a>El equipo host

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]


### <a name="container-requirements-and-recommendations"></a>Recomendaciones y requisitos del contenedor

En la tabla siguiente se describen los núcleos de CPU y memoria mínimos y recomendados para asignar cada contenedor de Face API.

| Contenedor | Mínima | Recomendado | TPS<br>(Mínimo, máximo)|
|-----------|---------|-------------|--|
|Caras | 1 núcleo, 2 GB de memoria | 1 núcleo, 4 GB de memoria |10, 20|

* Cada núcleo debe ser de 2,6 gigahercios (GHz) como mínimo.
* TP - transacciones por segundo

El núcleo y la memoria se corresponden con los valores de `--cpus` y `--memory` que se usan como parte del comando `docker run`.

## <a name="get-the-container-image-with-docker-pull"></a>Obtención de la imagen del contenedor con `docker pull`

Las imágenes de contenedor para Face API están disponibles. 

| Contenedor | Repositorio |
|-----------|------------|
| Caras | `containerpreview.azurecr.io/microsoft/cognitive-services-face:latest` |

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="docker-pull-for-the-face-container"></a>Docker pull para el contenedor de Face

```
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-face:latest
```

## <a name="how-to-use-the-container"></a>Uso del contenedor

Una vez que el contenedor esté en el [equipo host](#the-host-computer), utilice el siguiente proceso para trabajar con el contenedor.

1. [Ejecute el contenedor](#run-the-container-with-docker-run) con la configuración de facturación requerida. Hay más [ejemplos](./face-resource-container-config.md#example-docker-run-commands) del comando `docker run` disponibles. 
1. [Consulta del punto de conexión de predicción del contenedor](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>Ejecute el contenedor con `docker run`.

Utilice el comando [docker run](https://docs.docker.com/engine/reference/commandline/run/) para ejecutar cualquiera de los tres contenedores. El comando usa los parámetros siguientes:

| Marcador de posición | Valor |
|-------------|-------|
|{BILLING_KEY} | Esta clave se usa para iniciar el contenedor y está disponible en la página de claves de Face API en Azure Portal.  |
|{BILLING_ENDPOINT_URI} | El valor de URI del punto de conexión de facturación está disponible en Azure Portal, en la página de información general de Face API.|

Reemplace estos parámetros con sus propios valores en el siguiente comando `docker run` de ejemplo.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-face \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Este comando:

* Ejecuta un contenedor de Face desde la imagen de contenedor.
* Asigna un núcleo de CPU y 4 gigabytes (GB) de memoria.
* Expone el puerto TCP 5000 y asigna un seudo-TTY para el contenedor.
* Una vez que se produce la salida, quita automáticamente el contenedor. La imagen del contenedor sigue estando disponible en el equipo host. 

Hay más [ejemplos](./face-resource-container-config.md#example-docker-run-commands) del comando `docker run` disponibles. 

> [!IMPORTANT]
> Para poder ejecutar el contenedor, las opciones `Eula`, `Billing` y `ApiKey` deben estar especificadas; de lo contrario, el contenedor no se iniciará.  Para obtener más información, vea [Facturación](#billing).

[!INCLUDE [Running multiple containers on the same host](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]


## <a name="query-the-containers-prediction-endpoint"></a>Consulta del punto de conexión de predicción del contenedor

El contenedor proporciona varias API de puntos de conexión de predicción de consultas basadas en REST. 

Utilice el host, `https://localhost:5000`, con las API de contenedor.


<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Detención del contenedor

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>solución de problemas

Si ejecuta el contenedor con un [montaje](./face-resource-container-config.md#mount-settings) de salida y el registro habilitados, el contenedor genera archivos de registro que resultan útiles para solucionar problemas que se producen al iniciar o ejecutar el contenedor. 


## <a name="billing"></a>Facturación

Los contenedores de Face API envían información de facturación a Azure mediante un recurso de _Face API_ en la cuenta de Azure. 

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Para obtener más información acerca de estas opciones, consulte [Configure containers](./face-resource-container-config.md) (Configuración de contenedores).

## <a name="summary"></a>Resumen

En este artículo aprendió los conceptos y el flujo de trabajo para la descarga, instalación y ejecución de contenedores de Face API. En resumen:

* Face API proporciona tres contenedores de Linux para Docker, que encapsulan la extracción de frases clave, la detección de idioma y el análisis de sentimiento.
* Las imágenes del contenedor se descargan desde Microsoft Container Registry (MCR) en Azure.
* Las imágenes de contenedor se ejecutan en Docker.
* Puede usar la API REST o el SDK para llamar a operaciones en contenedores de Face API mediante la especificación del URI del host del contenedor.
* Debe especificar la información de facturación al crear una instancia de un contenedor.

> [!IMPORTANT]
> Los contenedores de Cognitive Services no tienen licencia para ejecutarse sin estar conectados a Azure para realizar mediciones. Los clientes tienen que habilitar los contenedores para comunicar la información de facturación con el servicio de medición en todo momento. Los contenedores de Cognitive Services no envían datos de los clientes (por ejemplo, la imagen o el texto que se está analizando) a Microsoft.

## <a name="next-steps"></a>Pasos siguientes

* Revise [Configure containers](face-resource-container-config.md) (Configuración de contenedores) para ver las opciones de configuración.
* Revise [Introducción a Face](Overview.md) para obtener más información acerca de la detección e identificación de caras.  
* Consulte [Face API](//westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) para obtener más información acerca de los métodos que admite el contenedor.
* Consulte [Preguntas más frecuentes (P+F)](FAQ.md) para resolver problemas relacionados con la funcionalidad de Face.
* Usar más [Contenedores de Cognitive Services](../cognitive-services-container-support.md)
