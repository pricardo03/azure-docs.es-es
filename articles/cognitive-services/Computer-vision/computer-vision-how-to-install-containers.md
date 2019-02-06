---
title: 'Cómo instalar y ejecutar contenedores: Computer Vision'
titlesuffix: Azure Cognitive Services
description: Cómo descargar, instalar y ejecutar contenedores para Computer Vision en este tutorial paso a paso.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: article
ms.date: 01/29/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 1e7f62d35e9850202b7d55c3c3440ff88413931d
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55473500"
---
# <a name="install-and-run-recognize-text-containers"></a>Instalar y ejecutar contenedores de Reconocer texto

La parte de Reconocer texto de Computer Vision también está disponible como un contenedor de Docker. Le permite detectar y extraer texto impreso de imágenes que muestren diversos objetos con diferentes superficies y fondos, como recibos, pósteres y tarjetas de visita.  
> [!IMPORTANT]
> Actualmente, el contenedor de Reconocer texto solo funciona en inglés.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Debe cumplir los siguientes requisitos previos para poder usar el contenedor de Reconocer texto:

|Obligatorio|Propósito|
|--|--|
|Motor de Docker| Necesita que el motor de Docker esté instalado en un [equipo host](#the-host-computer). Docker dispone de paquetes que configuran el entorno de Docker en [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) y [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Para conocer los principios básicos de Docker y de los contenedores, consulte [Introducción a Docker](https://docs.docker.com/engine/docker-overview/).<br><br> Docker debe configurarse para permitir que los contenedores se conecten con Azure y envíen datos de facturación a dicho servicio. <br><br> **En Windows**, Docker también debe estar configurado de forma que admita los contenedores de Linux.<br><br>|
|Conocimientos sobre Docker | Debe tener conocimientos básicos sobre los conceptos de Docker, como los registros, los repositorios, los contenedores y las imágenes de contenedor, así como conocer los comandos `docker` básicos.| 
|Recurso de Reconocer texto |Para poder usar el contenedor, debe tener:<br><br>Un recurso de Azure de [_Reconocer texto_](vision-api-how-to-topics/howtosubscribe.md) para obtener la clave de facturación asociada y el URI del punto de conexión de facturación. Ambos valores están disponibles en las páginas de claves y de información general de Reconocer texto en Azure Portal y son necesarios para iniciar el contenedor.<br><br>**{BILLING_KEY}**: clave de recurso<br><br>**{BILLING_ENDPOINT_URI}**: el ejemplo de URI de punto de conexión es `https://westus.api.cognitive.microsoft.com/vision/v2.0`|


## <a name="request-access-to-the-private-container-registry"></a>Solicitud de acceso al registro de contenedor privado

[!INCLUDE [Request access to private preview](../../../includes/cognitive-services-containers-request-access.md)]

### <a name="the-host-computer"></a>El equipo host

[!INCLUDE [Request access to private preview](../../../includes/cognitive-services-containers-host-computer.md)]


### <a name="container-requirements-and-recommendations"></a>Recomendaciones y requisitos del contenedor

En la tabla siguiente se describen los núcleos de CPU y memoria mínimos y recomendados para asignar cada contenedor de Reconocer texto.

| Contenedor | Mínima | Recomendado |
|-----------|---------|-------------|
|Reconocer texto|1 núcleo, 8 GB de memoria, 0,5 TPS|2 núcleos, 8 GB de memoria, 1 TPS|

Cada núcleo debe ser de 2,6 gigahercios (GHz) como mínimo.

El núcleo y la memoria se corresponden con los valores de `--cpus` y `--memory` que se usan como parte del comando `docker run`.


## <a name="get-the-container-image-with-docker-pull"></a>Obtención de la imagen del contenedor con `docker pull`

Hay imágenes de contenedor disponibles para Reconocer texto. 

| Contenedor | Repositorio |
|-----------|------------|
|Reconocer texto | `containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:latest` |

Use el comando [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) para descargar una imagen de contenedor.


### <a name="docker-pull-for-the-recognize-text-container"></a>Comando Docker pull para el contenedor Reconocer texto

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-rocognize-text:latest
```

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

## <a name="how-to-use-the-container"></a>Uso del contenedor

Una vez que el contenedor esté en el [equipo host](#the-host-computer), utilice el siguiente proceso para trabajar con el contenedor.

1. [Ejecute el contenedor](#run-the-container-with-docker-run) con la configuración de facturación requerida. Hay más [ejemplos](computer-vision-resource-container-config.md) del comando `docker run` disponibles. 
1. [Consulta del punto de conexión de predicción del contenedor](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>Ejecute el contenedor con `docker run`.

Utilice el comando [docker run](https://docs.docker.com/engine/reference/commandline/run/) para ejecutar el contenedor. El comando usa los parámetros siguientes:

| Marcador de posición | Valor |
|-------------|-------|
|{BILLING_KEY} | Esta clave se usa para iniciar el contenedor y está disponible en página de claves de Reconocer texto en Azure Portal.  |
|{BILLING_ENDPOINT_URI} | Valor del identificador URI del punto de conexión de facturación.|

Reemplace estos parámetros con sus propios valores en el siguiente comando `docker run` de ejemplo.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Este comando:

* Ejecuta un contenedor de reconocimiento desde la imagen de contenedor.
* Asigna un núcleo de CPU y 4 gigabytes (GB) de memoria.
* Expone el puerto TCP 5000 y asigna un seudo-TTY para el contenedor.
* Una vez que se produce la salida, quita automáticamente el contenedor. La imagen del contenedor sigue estando disponible en el equipo host. 

Hay más [ejemplos](./computer-vision-resource-container-config.md#example-docker-run-commands) del comando `docker run` disponibles. 

> [!IMPORTANT]
> Para poder ejecutar el contenedor, las opciones `Eula`, `Billing` y `ApiKey` deben estar especificadas; de lo contrario, el contenedor no se iniciará.  Para obtener más información, vea [Facturación](#billing).

## <a name="query-the-containers-prediction-endpoint"></a>Consulta del punto de conexión de predicción del contenedor

El contenedor proporciona varias API de puntos de conexión de predicción de consultas basadas en REST. 

Utilice el host, https://localhost:5000, con las API de contenedor.

### <a name="asynchronous-text-recognition"></a>Reconocimiento de texto asincrónico

Puede usar las operaciones `POST /vision/v2.0/recognizeText` y `GET /vision/v2.0/textOperations/*{id}*` conjuntamente para reconocer de forma asincrónica texto impreso en una imagen, de manera similar a cómo el servicio de Computer Vision usa esas operaciones de REST correspondientes. Actualmente, el contenedor de Reconocer texto solo reconoce texto impreso, no texto escrito a mano, por lo que dicho contenedor ignora el parámetro `mode` especificado normalmente para la operación del servicio de Computer Vision.

### <a name="synchronous-text-recognition"></a>Reconocimiento de texto sincrónico

Puede usar la operación `POST /vision/v2.0/recognizeTextDirect` para reconocer sincrónicamente texto impreso en una imagen. Dado que esta operación es sincrónica, el cuerpo de solicitud para esta operación es el mismo que para la operación `POST /vision/v2.0/recognizeText`, pero el cuerpo de la respuesta para esta operación es el mismo que el devuelto por la operación `GET /vision/v2.0/textOperations/*{id}*`.

## <a name="stop-the-container"></a>Detención del contenedor

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>solución de problemas

Si ejecuta el contenedor con un [montaje](./computer-vision-resource-container-config.md#mount-settings) de salida y el registro habilitados, el contenedor genera archivos de registro que resultan útiles para solucionar problemas que se producen al iniciar o ejecutar el contenedor. 

## <a name="containers-api-documentation"></a>Documentación de las API del contenedor

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="billing"></a>Facturación

Los contenedores de Reconocer texto envían información de facturación a Azure mediante un recurso de _Reconocer texto_ en la cuenta de Azure. 

Los contenedores de Cognitive Services no tienen licencia para ejecutarse sin estar conectados a Azure para realizar mediciones. Los clientes tienen que habilitar los contenedores para comunicar la información de facturación con el servicio de medición en todo momento. Los contenedores de Cognitive Services no envían los datos del cliente a Microsoft. 

El comando `docker run` utiliza los siguientes argumentos para la facturación:

| Opción | DESCRIPCIÓN |
|--------|-------------|
| `ApiKey` | Clave de API del recurso de _Reconocer texto_ utilizado para realizar un seguimiento de la información de facturación. |
| `Billing` | Punto de conexión del recurso de _Reconocer texto_ que se usa para realizar un seguimiento de la información de facturación.|
| `Eula` | Indica que ha aceptado la licencia del contenedor.<br/>El valor de esta opción debe establecerse en `accept`. |

> [!IMPORTANT]
> Las tres opciones deben especificarse con valores válidos; en caso contrario, no se inicia el contenedor.

Para obtener más información acerca de estas opciones, consulte [Configure containers](./computer-vision-resource-container-config.md) (Configuración de contenedores).

## <a name="summary"></a>Resumen

En este artículo, ha aprendido los conceptos y el flujo de trabajo para la descarga, instalación y ejecución de contenedores de Reconocer texto. En resumen:

* Reconocer texto proporciona un contenedor de Linux para Docker, que encapsula Reconocer texto.
* Las imágenes del contenedor se descargan desde Microsoft Container Registry (MCR) en Azure.
* Las imágenes de contenedor se ejecutan en Docker.
* Puede usar la API REST o el SDK para llamar a operaciones en contenedores de Reconocer texto mediante la especificación del URI del host del contenedor.
* Debe especificar la información de facturación al crear una instancia de un contenedor.

> [!IMPORTANT]
> Los contenedores de Cognitive Services no tienen licencia para ejecutarse sin estar conectados a Azure para realizar mediciones. Los clientes tienen que habilitar los contenedores para comunicar la información de facturación con el servicio de medición en todo momento. Los contenedores de Cognitive Services no envían datos de los clientes (por ejemplo, la imagen o el texto que se está analizando) a Microsoft.

## <a name="next-steps"></a>Pasos siguientes

* Revise [Configure containers](computer-vision-resource-container-config.md) (Configuración de contenedores) para ver las opciones de configuración.
* Revise [Introducción a Computer Vision](Home.md) para obtener más información sobre el reconocimiento de texto escrito a mano e impreso.  
* Consulte [Computer Vision API](//westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) para obtener más información acerca de los métodos que admite el contenedor.
* Consulte [Preguntas más frecuentes (P+F)](FAQ.md) para resolver problemas relacionados con la funcionalidad de Computer Vision.
* Use más [contenedores de Cognitive Services](../cognitive-services-container-support.md)
