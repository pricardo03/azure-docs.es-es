---
title: Fórmulas de contenedores de Docker Compose
titleSuffix: Azure Cognitive Services
description: Aprenda a implementar varios contenedores de Cognitive Services. Este procedimiento muestra cómo coordinar varias imágenes de contenedor de Docker con Docker Compose.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: 8afb7e866bc2a5fefe28a71653c4a2a87fdc7a5b
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445792"
---
# <a name="use-multiple-containers-in-a-private-network-with-docker-compose"></a>Uso de varios contenedores en una red privada con Docker Compose

Aprenda a implementar varios contenedores de Cognitive Services. Este procedimiento muestra cómo coordinar varias imágenes de contenedor de Docker con Docker Compose.

> [Docker Compose](https://docs.docker.com/compose/) es una herramienta para definir y ejecutar aplicaciones de Docker de varios contenedores. Con Compose, se usa un archivo YAML para configurar los servicios de la aplicación. A continuación, con un solo comando, se crean e inician todos los servicios de la configuración.

Cuando procede, puede resultar atractiva la idea de coordinar varias imágenes de contenedor en un único equipo host. En este artículo, reuniremos los servicios Reconocer texto y Form Recognizer.

## <a name="prerequisites"></a>Requisitos previos

Este procedimiento requiere varias herramientas que se deben instalar y ejecutar localmente.

* Use una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
* [Motor de docker](https://www.docker.com/products/docker-engine) y se asegura de que la CLI de Docker funciona en una ventana de consola.
* Un recurso de Azure con el plan de tarifa correcto. No todos los planes de tarifa funcionan con este contenedor:
  * El recurso **Computer Vision** solo con los planes de tarifas estándar o F0.
  * El recurso **Form Recognizer** solo con los planes de tarifas estándar o F0.
  * El recurso **Cognitive Services** con el plan de tarifas S0.

## <a name="request-access-to-the-container-registry"></a>Solicitud de acceso al registro de contenedor

Complete y envíe el [formulario de solicitud de contenedores de Voz de Cognitive Services](https://aka.ms/speechcontainerspreview/) para solicitar acceso al contenedor. 

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

## <a name="docker-compose-file"></a>Archivo de Docker Compose

El archivo YAML define todos los servicios que se van a implementar. Estos servicios se basan en un archivo `DockerFile` o en una imagen de contenedor existente, en este caso vamos a usar dos imágenes de versión preliminar. Copie y pegue el siguiente archivo YAML y guárdelo como *docker-compose.yaml*. Especifique los valores de _apikey_, _facturación_ y _URI de punto de conexión_ adecuados en el archivo _docker-compose.yml_ siguiente.

```yaml
version: '3.7'
services:
  forms:
    image: "containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer"
    environment:
       eula: accept
       billing: # < Your form recognizer billing URL >
       apikey: # < Your form recognizer API key >
       FormRecognizer__ComputerVisionApiKey: # < Your form recognizer API key >
       FormRecognizer__ComputerVisionEndpointUri: # < Your form recognizer URI >
    volumes:
       - type: bind
         source: e:\publicpreview\output
         target: /output
       - type: bind
         source: e:\publicpreview\input
         target: /input
    ports:
      - "5010:5000"

  ocr:
    image: "containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text"
    environment:
      eula: accept
      apikey: # < Your recognize text API key >
      billing: # < Your recognize text billing URL >
    ports:
      - "5021:5000"
```

> [!IMPORTANT]
> Cree los directorios en el equipo host que se especifican bajo el nodo `volumes`. Este es un paso obligatorio, ya que los directorios deben existir antes de intentar montar una imagen con enlaces de volumen.

## <a name="start-the-configured-docker-compose-services"></a>Inicio de los servicios configurados de Docker Compose

Un archivo de Docker Compose permite administrar todos los ciclos de vida del servicio definido; desde iniciar/detener y volver a generar los servicios, ver su estado y transmitir los registros. Abra una interfaz de línea de comandos desde el directorio del proyecto (donde se encuentra el archivo *docker-compose.yaml*).

> [!NOTE]
> Para evitar errores, asegúrese de que la máquina host está compartiendo correctamente las unidades con el **motor de Docker**. Por ejemplo, si *e:\publicpreview* se utiliza como directorio en el archivo *docker-compose.yaml*, comparta la *unidad E* con Docker.

Desde la interfaz de línea de comandos, ejecute el siguiente comando para iniciar (o reiniciar) todos los servicios definidos en *docker-compose.yaml*:

```console
docker-compose up
```

La primera vez que se ejecute el comando `docker-compose up` con esta configuración, **Docker** extraerá las imágenes configuradas en el nodo `services` (las descargará o las montará):

```console
Pulling forms (containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer:)...
latest: Pulling from microsoft/cognitive-services-form-recognizer
743f2d6c1f65: Pull complete
72befba99561: Pull complete
2a40b9192d02: Pull complete
c7715c9d5c33: Pull complete
f0b33959f1c4: Pull complete
b8ab86c6ab26: Pull complete
41940c21ed3c: Pull complete
e3d37dd258d4: Pull complete
cdb5eb761109: Pull complete
fd93b5f95865: Pull complete
ef41dcbc5857: Pull complete
4d05c86a4178: Pull complete
34e811d37201: Pull complete
Pulling ocr (containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:)...
latest: Pulling from microsoft/cognitive-services-recognize-text
f476d66f5408: Already exists
8882c27f669e: Already exists
d9af21273955: Already exists
f5029279ec12: Already exists
1a578849dcd1: Pull complete
45064b1ab0bf: Download complete
4bb846705268: Downloading [=========================================>         ]  187.1MB/222.8MB
c56511552241: Waiting
e91d2aa0f1ad: Downloading [==============================================>    ]  162.2MB/176.1MB
```

Se descargan las imágenes y, a continuación, se inician los servicios de imagen.

```console
Starting docker_ocr_1   ... done
Starting docker_forms_1 ... doneAttaching to docker_ocr_1, docker_forms_1forms_1  | forms_1  | forms_1  | Notice: This Preview is made available to you on the condition that you agree to the Supplemental Terms of Use for Microsoft Azure Previews [https://go.microsoft.com/fwlink/?linkid=2018815], which supplement your agreement [https://go.microsoft.com/fwlink/?linkid=2018657] governing your use of Azure. If you do not have an existing agreement governing your use of Azure, you agree that your agreement governing use of Azure is the Microsoft Online Subscription Agreement [https://go.microsoft.com/fwlink/?linkid=2018755] (which incorporates the Online Services Terms [https://go.microsoft.com/fwlink/?linkid=2018760]). By using the Preview you agree to these terms.
forms_1  | 
forms_1  | 
forms_1  | Using '/input' for reading models and other read-only data.
forms_1  | Using '/output/forms/812d811d1bcc' for writing logs and other output data.
forms_1  | Logging to console.
forms_1  | Submitting metering to 'https://westus2.api.cognitive.microsoft.com/'.
forms_1  | WARNING: No access control enabled!
forms_1  | warn: Microsoft.AspNetCore.Server.Kestrel[0]
forms_1  |       Overriding address(es) 'http://+:80'. Binding to endpoints defined in UseKestrel() instead.
forms_1  | Hosting environment: Production
forms_1  | Content root path: /app/forms
forms_1  | Now listening on: http://0.0.0.0:5000
forms_1  | Application started. Press Ctrl+C to shut down.
ocr_1    | 
ocr_1    | 
ocr_1    | Notice: This Preview is made available to you on the condition that you agree to the Supplemental Terms of Use for Microsoft Azure Previews [https://go.microsoft.com/fwlink/?linkid=2018815], which supplement your agreement [https://go.microsoft.com/fwlink/?linkid=2018657] governing your use of Azure. If you do not have an existing agreement governing your use of Azure, you agree that your agreement governing use of Azure is the Microsoft Online Subscription Agreement [https://go.microsoft.com/fwlink/?linkid=2018755] (which incorporates the Online Services Terms [https://go.microsoft.com/fwlink/?linkid=2018760]). By using the Preview you agree to these terms.
ocr_1    |
ocr_1    | 
ocr_1    | Logging to console.
ocr_1    | Submitting metering to 'https://westcentralus.api.cognitive.microsoft.com/'.
ocr_1    | WARNING: No access control enabled!
ocr_1    | Hosting environment: Production
ocr_1    | Content root path: /
ocr_1    | Now listening on: http://0.0.0.0:5000
ocr_1    | Application started. Press Ctrl+C to shut down.
```

## <a name="verify-the-service-availability"></a>Comprobación de la disponibilidad del servicio

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

La siguiente es una salida de ejemplo:

```
IMAGE ID            REPOSITORY                                                                 TAG
2ce533f88e80        containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer   latest
4be104c126c5        containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text    latest
```

### <a name="test-the-recognize-text-container"></a>Pruebas en el contenedor de Reconocer texto

Abra un explorador en el equipo host y vaya a `localhost` con el puerto especificado en *docker-compose.yaml*; por ejemplo, `http://localhost:5021/swagger/index.html`. Puede usar la característica Probarlo de la API para probar el punto de conexión de Reconocer texto.

![Swagger de Reconocer texto](media/recognize-text-swagger-page.png)

### <a name="test-the-form-recognizer-container"></a>Pruebas del contenedor de Form Recognizer

Abra un explorador en el equipo host y vaya a `localhost` con el puerto especificado en *docker-compose.yaml*; por ejemplo, `http://localhost:5010/swagger/index.html`. Puede usar la característica Probarlo de la API para probar el punto de conexión de Form Recognizer.

![Swagger de Form Recognizer](media/form-recognizer-swagger-page.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Contenedores de Cognitive Services](../cognitive-services-container-support.md)