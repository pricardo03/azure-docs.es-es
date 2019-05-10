---
title: 'Instalar y ejecutar contenedores: detector de anomalías'
titleSuffix: Azure Cognitive Services
description: Use algoritmos avanzados de la API Detector de anomalías para identificar anomalías en datos de series temporales.
services: cognitive-services
author: aahill
ms.service: cognitive-services
ms.subservice: anomaly-detection
ms.topic: article
ms.date: 05/07/2019
ms.author: aahi
ms.openlocfilehash: cc82e091ae4c033bda7f1d91c9aed36bb081de88
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233615"
---
# <a name="install-and-run-anomaly-detector-containers"></a>Instalar y ejecutar contenedores de Detector de anomalías

El Detector de anomalías tiene el siguiente contenedor: 

|Función|Características|
|-|-|
|Detector de anomalías| <li> Detecta anomalías cuando se producen en tiempo real. <li> Detecta anomalías en todo el conjunto de datos como un lote. <li> Deduce el intervalo normal esperado de los datos. <li> Ajuste de sensibilidad de detección admite anomalías mejor ajustar los datos. |

Para obtener información detallada acerca de las API, consulte:
* [Más información sobre el servicio de API del Detector de anomalías](https://go.microsoft.com/fwlink/?linkid=2080698&clcid=0x409)

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Debe cumplir los siguientes requisitos previos antes de utilizar contenedores Detector de anomalías:

|Obligatorio|Propósito|
|--|--|
|Motor de Docker| Necesita que el motor de Docker esté instalado en un [equipo host](#the-host-computer). Docker dispone de paquetes que configuran el entorno de Docker en [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) y [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Para conocer los principios básicos de Docker y de los contenedores, consulte [Introducción a Docker](https://docs.docker.com/engine/docker-overview/).<br><br> Docker debe configurarse para permitir que los contenedores se conecten con Azure y envíen datos de facturación a dicho servicio. <br><br> **En Windows**, Docker también debe estar configurado de forma que admita los contenedores de Linux.<br><br>|
|Conocimientos sobre Docker | Debe tener conocimientos básicos sobre los conceptos de Docker, como los registros, los repositorios, los contenedores y las imágenes de contenedor, así como conocer los comandos `docker` básicos.| 
|Recursos del Detector de anomalías |Para usar estos contenedores, debe tener:<br><br>Un _Detector de anomalías_ recursos de Azure para obtener la clave de facturación asociada y el URI del extremo de facturación. Ambos valores están disponibles en las páginas de información general sobre el Detector de anomalías y las claves del portal de Azure y es necesario para iniciar el contenedor.<br><br>**{BILLING_KEY}**: clave de recurso<br><br>**{BILLING_ENDPOINT_URI}**: el ejemplo de URI de punto de conexión es `https://westus2.api.cognitive.microsoft.com`|

## <a name="request-access-to-the-container-registry"></a>Solicitud de acceso al registro de contenedor

Primero debe completar y enviar la [formulario de solicitud de contenedor del Detector de anomalías](https://aka.ms/adcontainer) para solicitar acceso al contenedor.

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

## <a name="the-host-computer"></a>El equipo host

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

<!--* [Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/). For instructions of deploying Anomaly Detector module in IoT Edge, see [How to deploy Anomaly Detector module in IoT Edge](how-to-deploy-anomaly-detector-module-in-iot-edge.md).-->

### <a name="container-requirements-and-recommendations"></a>Recomendaciones y requisitos del contenedor

La tabla siguiente describen los núcleos de CPU y memoria para asignar el contenedor del Detector de anomalías mínimos y recomendados.

| Consultas por segundo (consultas por segundo) | Mínimo | Recomendada |
|-----------|---------|-------------|
| 10 QPS | 4 núcleos, 1GB de memoria | 2GB de memoria de 8 núcleos |
| 20 QPS | 8 núcleos, 2GB de memoria | 4GB de memoria de 16 núcleos |

Cada núcleo debe ser de 2,6 gigahercios (GHz) como mínimo.

El núcleo y la memoria se corresponden con los valores de `--cpus` y `--memory` que se usan como parte del comando `docker run`.

## <a name="get-the-container-image-with-docker-pull"></a>Obtención de la imagen del contenedor con `docker pull`

Use el comando [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) para descargar una imagen de contenedor.

| Contenedor | Repositorio |
|-----------|------------|
| cognitive-services-anomaly-detector | `containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector:latest` |

<!--
For a full description of available tags, such as `latest` used in the preceding command, see [anomaly-detector](https://go.microsoft.com/fwlink/?linkid=2083827&clcid=0x409) on Docker Hub.
-->
[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]


### <a name="docker-pull-for-the-anomaly-detector-container"></a>Extracción de docker para el contenedor del Detector de anomalías

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector:latest
```

## <a name="how-to-use-the-container"></a>Uso del contenedor

Una vez que el contenedor esté en el [equipo host](#the-host-computer), utilice el siguiente proceso para trabajar con el contenedor.

1. [Ejecute el contenedor](#run-the-container-with-docker-run) con la configuración de facturación requerida. Hay más [ejemplos](anomaly-detector-container-configuration.md#example-docker-run-commands) del comando `docker run` disponibles. 
1. [Consulta del punto de conexión de predicción del contenedor](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>Ejecute el contenedor con `docker run`.

Utilice el comando [docker run](https://docs.docker.com/engine/reference/commandline/run/) para ejecutar cualquiera de los tres contenedores. El comando usa los parámetros siguientes:

| Marcador de posición | Valor |
|-------------|-------|
|{BILLING_KEY} | Esta clave se usa para iniciar el contenedor y está disponible en página de las claves del Detector de anomalías Azure portal en la.  |
|{BILLING_ENDPOINT_URI} | El valor del identificador URI del punto de conexión facturación está disponible en la página de información general sobre el Detector de anomalías del portal de Azure.|

Reemplace estos parámetros con sus propios valores en el siguiente comando `docker run` de ejemplo.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector:latest \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Este comando:

* Se ejecuta un contenedor del Detector de anomalías de la imagen de contenedor
* Asigna un núcleo de CPU y 4 gigabytes (GB) de memoria.
* Expone el puerto TCP 5000 y asigna un seudo-TTY para el contenedor.
* Una vez que se produce la salida, quita automáticamente el contenedor. La imagen del contenedor sigue estando disponible en el equipo host. 

> [!IMPORTANT]
> Para poder ejecutar el contenedor, las opciones `Eula`, `Billing` y `ApiKey` deben estar especificadas; de lo contrario, el contenedor no se iniciará.  Para obtener más información, vea [Facturación](#billing).

### <a name="running-multiple-containers-on-the-same-host"></a>Ejecución de varios contenedores en el mismo host

Si tiene pensado ejecutar varios contenedores con puertos expuestos, asegúrese de ejecutar cada contenedor con un puerto diferente. Por ejemplo, ejecute el primer contenedor en el puerto 5000 y el segundo en el puerto 5001.

Reemplace `<container-registry>` y `<container-name>` por los valores de los contenedores que utilice. Estos contenedores pueden ser diferentes. Puede hacer que el contenedor del Detector de anomalías y el contenedor de LUIS que se ejecutan en el HOST juntos o puede tener varios contenedores de Detector de anomalías que se ejecutan. 

Ejecute el primer contenedor en el puerto 5000. 

```bash 
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Ejecute el segundo contenedor en el puerto 5001.


```bash 
docker run --rm -it -p 5000:5001 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Cada contenedor sucesivo debe estar en un puerto diferente. 

## <a name="query-the-containers-prediction-endpoint"></a>Consulta del punto de conexión de predicción del contenedor

El contenedor proporciona varias API de puntos de conexión de predicción de consultas basadas en REST. 

Utilice el host, https://localhost:5000, con las API de contenedor.

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Detención del contenedor

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>solución de problemas

Si ejecuta el contenedor con un [montaje](anomaly-detector-container-configuration.md#mount-settings) de salida y el registro habilitados, el contenedor genera archivos de registro que resultan útiles para solucionar problemas que se producen al iniciar o ejecutar el contenedor. 

## <a name="billing"></a>Facturación

El envío de los contenedores del Detector de anomalías información de facturación para Azure, mediante un _Detector de anomalías_ recursos en su cuenta de Azure. 

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Para obtener más información acerca de estas opciones, consulte [Configure containers](anomaly-detector-container-configuration.md) (Configuración de contenedores).

## <a name="summary"></a>Resumen

En este artículo, ha aprendido los conceptos y flujo de trabajo para descargar, instalar y ejecutar contenedores de Detector de anomalías. En resumen:

* Detector de anomalías proporciona un contenedor Linux docker, que encapsula la detección de anomalías con transmisión por secuencias, inferencia de intervalo esperado y el ajuste de sensibilidad de vs de batch.
* Imágenes de contenedor se descargan desde un registro de contenedor de Azure privada dedicada para la versión preliminar de contenedores.
* Las imágenes de contenedor se ejecutan en Docker.
* Puede usar la API de REST o SDK para llamar a operaciones en los contenedores del Detector de anomalías especificando el URI del contenedor del host.
* Debe especificar la información de facturación al crear una instancia de un contenedor.

> [!IMPORTANT]
> Los contenedores de Cognitive Services no tienen licencia para ejecutarse sin estar conectados a Azure para realizar mediciones. Los clientes tienen que habilitar los contenedores para comunicar la información de facturación con el servicio de medición en todo momento. Los contenedores de servicios cognitivos no envían datos de cliente (por ejemplo, el tiempo datos de series que se está analizando) a Microsoft.

## <a name="next-steps"></a>Pasos siguientes

* Revise [Configure containers](anomaly-detector-container-configuration.md) (Configuración de contenedores) para ver las opciones de configuración.
* [Más información sobre el servicio de API del Detector de anomalías](https://go.microsoft.com/fwlink/?linkid=2080698&clcid=0x409)
