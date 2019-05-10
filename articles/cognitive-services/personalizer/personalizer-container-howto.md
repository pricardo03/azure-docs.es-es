---
title: 'Instalación y ejecución de contenedores: Personalizer'
titleSuffix: Azure Cognitive Services
description: Cómo descargar, instalar y ejecutar contenedores para Personalizer.
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/09/2019
ms.author: edjez
ms.openlocfilehash: a197531a7c78823271c0a5fa5413b76746f63a9a
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65507030"
---
# <a name="install-and-run-personalizer-containers"></a>Instalación y ejecución de contenedores de Personalizer

El servicio Personalizer tiene los siguientes contenedores: 

|Función|Características|
|-|-|
|Personalizer|Determine la mejor acción del contexto actual de contenido y usuario.|

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para poder usar contenedores del servicio Personalizer, debe cumplir los siguientes requisitos previos:

|Obligatorio|Propósito|
|--|--|
|Motor de Docker| Necesita que el motor de Docker esté instalado en un [equipo host](#the-host-computer). Docker dispone de paquetes que configuran el entorno de Docker en [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) y [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Para conocer los principios básicos de Docker y de los contenedores, consulte [Introducción a Docker](https://docs.docker.com/engine/docker-overview/).<br><br> Docker debe configurarse para permitir que los contenedores se conecten con Azure y envíen datos de facturación a dicho servicio. <br><br> **En Windows**, Docker también debe estar configurado de forma que admita los contenedores de Linux.<br><br>|
|Conocimientos sobre Docker | Debe tener conocimientos básicos sobre los conceptos de Docker, como los registros, los repositorios, los contenedores y las imágenes de contenedor, así como conocer los comandos `docker` básicos.| 
|Recurso del servicio Personalizer |Para usar estos contenedores, debe tener:<br><br>Un recurso de Azure del _servicio Personalizer_ a fin de obtener la clave de facturación asociada y el URI del punto de conexión de facturación. Ambos valores están disponibles en las páginas de claves e información general del servicio Personalizer de Azure Portal, y son necesarios para iniciar el contenedor.<br><br>**{BILLING_KEY}**: clave de recurso<br><br>**{BILLING_ENDPOINT_URI}**: el ejemplo de URI de punto de conexión es `https://westus.api.cognitive.microsoft.com/`|

### <a name="the-host-computer"></a>El equipo host

El **host** es el equipo que ejecuta el contenedor de Docker. Puede ser un equipo del entorno local o un servicio de hospedaje de Docker incluido en Azure, como:

* [Azure Kubernetes Service](https://docs.microsoft.com/aks/index.yml)
* [Azure Container Instances](https://docs.microsoft.com/container-instances/index.yml)
* Un clúster de [Kubernetes](https://kubernetes.io/) implementado en [Azure Stack](https://docs.microsoft.com/azure-stack/index.yml). Para obtener más información, consulte [Implementación de Kubernetes en Azure Stack](https://docs.microsoft.com/azure-stack/user/azure-stack-solution-template-kubernetes-deploy.md).

### <a name="container-requirements-and-recommendations"></a>Recomendaciones y requisitos del contenedor

En la tabla siguiente se describen los núcleos de CPU y memoria mínimos y recomendados para asignar a cada contenedor del servicio Personalizer.

| Contenedor | Mínima | Recomendado |
|-----------|---------|-------------|
|Personalizer | 1 núcleo, 4 GB de memoria | 2 núcleos, 8 GB de memoria |

Cada núcleo debe ser de 2,6 gigahercios (GHz) como mínimo.

El núcleo y la memoria se corresponden con los valores de `--cpus` y `--memory` que se usan como parte del comando `docker run`.

El contenedor debe ser capaz de conectarse a Azure EventHub para poder retransmitir información en las llamadas Rank y Reward al servidor de Personalizer en Azure, y debe poder conectarse a la API de Personalizer para cargar la configuración necesaria y los modelos más recientes de aprendizaje automático.

## <a name="get-the-container-image-with-docker-pull"></a>Obtención de la imagen del contenedor con `docker pull`

Hay disponibles imágenes de contenedor para el servicio Personalizer. 

| Contenedor | Repositorio |
|-----------|------------|
| Personalizer | `mcr.microsoft.com/azure-cognitive-services/personalizer:latest` |

> [!TIP]
> Puede usar el comando [docker images](https://docs.docker.com/engine/reference/commandline/images/) para enumerar las imágenes de contenedor descargadas. Por ejemplo, el comando siguiente muestra el id., el repositorio y la etiqueta de cada imagen de contenedor descargada, con formato de tabla:
>
>  ```Docker
>  docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
>
>  IMAGE ID            REPOSITORY                                                                TAG
>  ebbee78a6baa        mcr.microsoft.com/azure-cognitive-services/personalizer                 latest
>  ``` 

### <a name="docker-pull-for-the-personalizer-service-container"></a>Comando docker pull para el contenedor del servicio Personalizer

```Docker
docker pull mcr.microsoft.com/azure-cognitive-services/personalizer:latest
```

## <a name="how-the-container-works"></a>Cómo funciona el contenedor

El contenedor de Personalizer representa la parte cliente del servicio Personalizer. Cuando se ejecuta el contenedor, obtiene los modelos y las opciones de configuración del servicio Personalizer en la nube de Azure. El servicio de contenedor usa esta información para responder a las solicitudes **rank** y **reward**. El contenedor también envía estas solicitudes al recurso de Personalizer en la nube de Azure. Esta información se emplea entonces para entrenar el modelo de bucle de Personalizer, según la configuración actual de la frecuencia de actualización del modelo. El modelo actualizado se envía de vuelta al contenedor. 

![Arquitectura local del cliente del contenedor de Personalizer](./media/personalizer-container-howto/personalizer-container-architecture.png)

## <a name="how-to-use-the-container"></a>Uso del contenedor

Una vez que el contenedor esté en el [equipo host](#the-host-computer), utilice el siguiente proceso para trabajar con el contenedor.

1. [Ejecute el contenedor](#run-the-container-with-docker-run) con la configuración de facturación requerida. Hay más [ejemplos](personalizer-container-configuration.md#example-docker-run-commands) del comando `docker run` disponibles. 
1. [Consulta del punto de conexión de predicción del contenedor](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>Ejecute el contenedor con `docker run`.

Utilice el comando [docker run](https://docs.docker.com/engine/reference/commandline/run/) para ejecutar cualquiera de los tres contenedores. El comando usa los parámetros siguientes:

| Marcador de posición | Valor |
|-------------|-------|
|{BILLING_KEY} | Esta clave se usa para iniciar el contenedor y está disponible en la página de claves del servicio Personalizer de Azure Portal.  |
|{BILLING_ENDPOINT_URI} | El valor del URI de punto de conexión de facturación está disponible en la página de información general del servicio Personalizer de Azure Portal.|

Reemplace estos parámetros con sus propios valores en el siguiente comando `docker run` de ejemplo.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/personalizer\
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Este comando:

* Ejecuta un contenedor del servicio Personalizer a partir de la imagen de contenedor.
* Asigna un núcleo de CPU y 4 gigabytes (GB) de memoria.
* Expone el puerto TCP 5000 y asigna un seudo-TTY para el contenedor.
* Una vez que se produce la salida, quita automáticamente el contenedor. La imagen del contenedor sigue estando disponible en el equipo host. 

> [!IMPORTANT]
> Para poder ejecutar el contenedor, las opciones `Eula`, `Billing` y `ApiKey` deben estar especificadas; de lo contrario, el contenedor no se iniciará.  Para obtener más información, vea [Facturación](#billing).

### <a name="run-multiple-containers-on-the-same-host"></a>Ejecución de varios contenedores en el mismo host

Si tiene pensado ejecutar varios contenedores con puertos expuestos, asegúrese de ejecutar cada contenedor con un puerto diferente. Por ejemplo, ejecute el primer contenedor en el puerto 5000 y el segundo en el puerto 5001.

Reemplace `<container-registry>` y `<container-name>` por los valores de los contenedores que utilice. Estos contenedores pueden ser diferentes. Puede tener el contenedor de Personalizer y el contenedor de LUIS ejecutándose juntos en el HOST, o puede tener varios contenedores de Personalizer en ejecución. 

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

## <a name="stop-the-container"></a>Detención del contenedor

Para apagar el contenedor, en el entorno de la línea de comandos en la que se ejecuta el contenedor, presione **Ctrl + C**.

## <a name="troubleshoot"></a>Solución de problemas

Si ejecuta el contenedor con un [montaje](personalizer-container-configuration.md#mount-settings) de salida y el registro habilitados, el contenedor genera archivos de registro que resultan útiles para solucionar problemas que se producen al iniciar o ejecutar el contenedor. 

## <a name="container-api-documentation"></a>Documentación de la API del contenedor

El contenedor cuenta con un completo conjunto de documentación sobre los puntos de conexión, así como una característica `Try it now`. Esta característica le permite especificar la configuración en un formulario HTML basado en Web y realizar la consulta sin necesidad de escribir código. Una vez que la consulta devuelve resultados, se proporciona un ejemplo del comando CURL para mostrar los encabezados HTTP y el formato de cuerpo requeridos. 

> [!TIP]
> Consulte la [especificación de OpenAPI](https://swagger.io/docs/specification/about/), donde se describen las operaciones API que se admiten en el contenedor desde el URI relativo `/swagger`. Por ejemplo: 
>
>  ```http
>  http://localhost:5000/swagger
>  ```

## <a name="billing"></a>Facturación

Los contenedores del servicio Personalizer envían información de facturación a Azure mediante un recurso del _servicio Personalizer_ de la cuenta de Azure. 

Los contenedores de Cognitive Services no tienen licencia para ejecutarse sin estar conectados a Azure para realizar mediciones. Los clientes tienen que habilitar los contenedores para comunicar la información de facturación con el servicio de medición en todo momento.  

El comando `docker run` utiliza los siguientes argumentos para la facturación:

| Opción | DESCRIPCIÓN |
|--------|-------------|
| `ApiKey` | La clave de API del recurso del _servicio Personalizer_ usada para realizar el seguimiento de la información de facturación. |
| `Billing` | El punto de conexión del recurso del _servicio Personalizer_ usado para realizar el seguimiento de la información de facturación.|
| `Eula` | Indica que ha aceptado la licencia del contenedor.<br/>El valor de esta opción debe establecerse en `accept`. |

> [!IMPORTANT]
> Las tres opciones deben especificarse con valores válidos; en caso contrario, no se inicia el contenedor.

Para obtener más información acerca de estas opciones, consulte [Configure containers](personalizer-container-configuration.md) (Configuración de contenedores).

## <a name="summary"></a>Resumen

En este artículo, ha aprendido los conceptos y el flujo de trabajo relativos a la descarga, la instalación y la ejecución de contenedores del servicio Personalizer. En resumen:

* El servicio Personalizer ofrece contenedores de Linux para Docker, que encapsulan la personalización.
* Las imágenes del contenedor se descargan desde Microsoft Container Registry (MCR) en Azure.
* Las imágenes de contenedor se ejecutan en Docker.
* Puede usar la API REST o el SDK para llamar a operaciones en contenedores del servicio Personalizer mediante la especificación del URI de host del contenedor.
* Debe especificar la información de facturación al crear una instancia de un contenedor.

> [!IMPORTANT]
> Los contenedores de Cognitive Services no tienen licencia para ejecutarse sin estar conectados a Azure para realizar mediciones. Los clientes tienen que habilitar los contenedores para comunicar la información de facturación con el servicio de medición en todo momento. Los contenedores de Personalizer también enviarán los datos de solicitud al servicio correspondiente en Azure para el entrenamiento en línea, y obtendrán modelos actualizados periódicamente desde Azure.

## <a name="next-steps"></a>Pasos siguientes

[Cómo configurar el contenedor de Docker para el comando `Docker Run`](personalizer-container-configuration.md)