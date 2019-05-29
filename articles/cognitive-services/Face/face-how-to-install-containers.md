---
title: Instalación y ejecución de contenedores
titlesuffix: Face - Azure Cognitive Services
description: Descargar, instalar y ejecutar contenedores de cara en este tutorial.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: article
ms.date: 05/28/2019
ms.author: diberry
ms.openlocfilehash: 26ebeb463f42dce06c29a5bd3f69585430a2ee90
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66306637"
---
# <a name="install-and-run-face-containers"></a>Instalar y ejecutar contenedores de cara

Cara de servicios cognitivos Azure proporciona un contenedor de Linux estandarizado para Docker que detecte caras humanas en imágenes. También identifica los atributos, que incluyen los puntos de referencia como narices y los ojos, sexo, edad y otras características faciales predecir a la máquina. Además, la detección de cara puede comprobar si dos caras en el mismo o diferentes imágenes son iguales utilizando una puntuación de confianza. Cara también puede comparar las caras en una base de datos para ver si ya existe una cara similares o idéntica. También, pueden organizar las caras similares en grupos mediante el uso de características compartidas de visuales.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Debe cumplir los siguientes requisitos previos antes de usar los contenedores de Face API.

|Obligatorio|Propósito|
|--|--|
|Motor de Docker| El motor de Docker debe estar instalado en un [equipo host](#the-host-computer). Docker dispone de paquetes que configuran el entorno de Docker en [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) y [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Para conocer los principios básicos de Docker y de los contenedores, consulte [Introducción a Docker](https://docs.docker.com/engine/docker-overview/).<br><br> Docker debe configurarse para permitir que los contenedores se conecten con Azure y envíen datos de facturación a dicho servicio. <br><br> En Windows, Docker también debe configurarse para admitir los contenedores de Linux.<br><br>|
|Conocimientos sobre Docker | Necesita un conocimiento básico de los conceptos de Docker, como registros, repositorios, contenedores y las imágenes de contenedor. También necesita conocimientos de basic `docker` comandos.| 
|Azure `Cognitive Services` recursos |Para usar el contenedor, debe tener:<br><br>Un recurso de Azure Cognitive Services y la clave de facturación asociada y el URI del extremo de facturación. Ambos valores están disponibles en el **Introducción** y **claves** páginas para el recurso. Son necesarios para iniciar el contenedor. Agregar el `face/v1.0` enrutamiento al punto de conexión URI, como se muestra en el siguiente ejemplo BILLING_ENDPOINT_URI: <br><br>**{BILLING_KEY}** : clave de recurso<br><br>**{BILLING_ENDPOINT_URI}** : ejemplo URI de punto de conexión es `https://westus.api.cognitive.microsoft.com/face/v1.0`|

## <a name="request-access-to-the-private-container-registry"></a>Solicitud de acceso al registro de contenedor privado

[!INCLUDE [Request access to private container registry](../../../includes/cognitive-services-containers-request-access.md)]

### <a name="the-host-computer"></a>El equipo host

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]


### <a name="container-requirements-and-recommendations"></a>Recomendaciones y requisitos del contenedor

En la tabla siguiente se describen los núcleos de CPU y memoria mínimos y recomendados para asignar cada contenedor de Face API.

| Contenedor | Mínimo | Recomendada | Transacciones por segundo<br>(Mínimo, máximo)|
|-----------|---------|-------------|--|
|Face | 1 núcleo, 2 GB de memoria | 1 núcleo, 4 GB de memoria |10, 20|

* Cada núcleo debe ser al menos a 2,6 GHz o superior.
* Transacciones por segundo (TPS).

El núcleo y la memoria se corresponden con los valores de `--cpus` y `--memory` que se usan como parte del comando `docker run`.

## <a name="get-the-container-image-with-docker-pull"></a>Obtener la imagen de contenedor con la extracción de docker

Imágenes de contenedor para Face API están disponibles. 

| Contenedor | Repositorio |
|-----------|------------|
| Face | `containerpreview.azurecr.io/microsoft/cognitive-services-face:latest` |

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="docker-pull-for-the-face-container"></a>Docker pull para el contenedor de Face

```
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-face:latest
```

## <a name="use-the-container"></a>Usar el contenedor

Después de que el contenedor está en el [equipo host](#the-host-computer), utilice el siguiente proceso para trabajar con el contenedor.

1. [Ejecute el contenedor](#run-the-container-with-docker-run) con la configuración de facturación. Hay más [ejemplos](./face-resource-container-config.md#example-docker-run-commands) del comando `docker run` disponibles. 
1. [Consulta del punto de conexión de predicción del contenedor](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>Ejecute el contenedor con la ejecución de docker

Utilice el comando [docker run](https://docs.docker.com/engine/reference/commandline/run/) para ejecutar cualquiera de los tres contenedores. El comando utiliza los siguientes parámetros.

| Marcador de posición | Valor |
|-------------|-------|
|{BILLING_KEY} | Esta clave se usa para iniciar el contenedor y está disponible en Azure `Cognitive Services` **claves** página. |
|{BILLING_ENDPOINT_URI} | El valor del identificador URI de extremo facturación está disponible en Azure `Cognitive Services` **Introducción** página. Un ejemplo es `https://westus.api.cognitive.microsoft.com/face/v1.0`.|

Agregar el `face/v1.0` enrutamiento al punto de conexión URI, como se muestra en el ejemplo anterior de BILLING_ENDPOINT_URI. 

Reemplace estos parámetros con sus propios valores en la siguiente `docker run` ejemplo de comando:

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-face \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Este comando:

* Ejecuta un contenedor de cara de la imagen de contenedor.
* Asigna un núcleo de CPU y 4 GB de memoria.
* Expone el puerto TCP 5000 y asigna un pseudo TTY para el contenedor.
* Una vez que se produce la salida, quita automáticamente el contenedor. La imagen del contenedor sigue estando disponible en el equipo host. 

Hay más [ejemplos](./face-resource-container-config.md#example-docker-run-commands) del comando `docker run` disponibles. 

> [!IMPORTANT]
> El `Eula`, `Billing`, y `ApiKey` se deben especificar las opciones para ejecutar el contenedor o no se inicia el contenedor. Para obtener más información, vea [Facturación](#billing).

[!INCLUDE [Running multiple containers on the same host](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]


## <a name="query-the-containers-prediction-endpoint"></a>Consulta del punto de conexión de predicción del contenedor

El contenedor proporciona varias API de puntos de conexión de predicción de consultas basadas en REST. 

Utilice el host, `https://localhost:5000`, con las API de contenedor.


<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Detención del contenedor

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>solución de problemas

Si ejecuta el contenedor con una salida [montar](./face-resource-container-config.md#mount-settings) y el registro está habilitado, el contenedor genera archivos de registro que son útiles para solucionar problemas que se producen al iniciar o al ejecutar el contenedor. 


## <a name="billing"></a>Facturación

Los contenedores de Face API envían información de facturación en Azure mediante el uso de un recurso de Face API en su cuenta de Azure. 

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Para obtener más información acerca de estas opciones, consulte [Configure containers](./face-resource-container-config.md) (Configuración de contenedores).

<!--blogs/samples/video coures -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Resumen

En este artículo, ha aprendido los conceptos y flujo de trabajo acerca de cómo descargar, instalar y ejecutar contenedores de Face API. En resumen:

* Face API proporciona tres contenedores de Linux de Docker que proporcionan análisis de opiniones, detección de idioma y extracción de frases clave.
* Imágenes de contenedor se descargan desde Azure Container Registry.
* Las imágenes de contenedor se ejecutan en Docker.
* Puede usar la API de REST o el SDK para llamar a operaciones en contenedores de Face API especificando el URI del contenedor del host.
* Debe especificar la información de facturación cuando crea una instancia de un contenedor.

> [!IMPORTANT]
> Contenedores de cognitive Services no están una licencia para ejecutar sin estar conectados a Azure para la medición. Los clientes deben habilitar los contenedores comunicar la información de facturación con el servicio de disponibilidad en todo momento. Los contenedores de Cognitive Services no envían datos de los clientes (por ejemplo, la imagen o el texto que se está analizando) a Microsoft.

## <a name="next-steps"></a>Pasos siguientes

* Para opciones de configuración, consulte [configurar contenedores](face-resource-container-config.md).
* Para obtener más información sobre cómo detectar e identificar caras, vea [información general de la cara](Overview.md).
* Para obtener información acerca de los métodos admitidos por el contenedor, consulte el [Face API](//westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).
* Para usar más contenedores de Cognitive Services, consulte [contenedores de Cognitive Services](../cognitive-services-container-support.md).
