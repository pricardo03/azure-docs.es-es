---
title: Instalación y ejecución de contenedores
titlesuffix: Computer Vision - Azure Cognitive Services
description: Cómo descargar, instalar y ejecutar contenedores para Computer Vision en este tutorial paso a paso.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 11/14/2018
ms.author: diberry
ms.openlocfilehash: 9964be07c578cd1cafd93328bffe972483123e03
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53077042"
---
# <a name="install-and-run-containers"></a>Instalación y ejecución de contenedores

La creación de contenedores es un enfoque de distribución de software en el que una aplicación o servicio se empaqueta como una imagen de contenedor. La configuración y las dependencias de la aplicación o del servicio se incluyen en la imagen de contenedor. La imagen de contenedor puede implementarse en un host de contenedor con pocas modificaciones o ninguna. Los contenedores están aislados entre sí y del sistema operativo subyacente, con una superficie menor que una máquina virtual. Se pueden crear instancias de contenedores a partir de las imágenes de contenedor para las tareas a corto plazo y quitarlas cuando ya no se necesiten.

La parte de Reconocer texto de Computer Vision también está disponible como un contenedor de Docker. Le permite detectar y extraer texto impreso de imágenes que muestren diversos objetos con diferentes superficies y fondos, como recibos, pósteres y tarjetas de visita.  
> [!IMPORTANT]
> Actualmente, el contenedor de Reconocer texto solo funciona en inglés.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="preparation"></a>Preparación

Debe cumplir los siguientes requisitos previos para poder usar el contenedor de Reconocer texto:

**Motor de Docker**: el motor de Docker debe estar instalado en el entorno local. Docker proporciona paquetes que configuran el entorno de Docker en [macOS](https://docs.docker.com/docker-for-mac/), [Linux](https://docs.docker.com/engine/installation/#supported-platforms) y [Windows](https://docs.docker.com/docker-for-windows/). En Windows, Docker debe configurarse para admitir los contenedores de Linux. Los contenedores de Docker también se pueden implementar directamente en [Azure Kubernetes Service](/azure/aks/), [Azure Container Instances](/azure/container-instances/) o en un clúster de [Kubernetes](https://kubernetes.io/) implementado en [Azure Stack](/azure/azure-stack/). Para obtener más información sobre la implementación de Kubernetes en Azure Stack, consulte [Implementación de Kubernetes en Azure Stack](/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy).

Docker debe configurarse para permitir que los contenedores se conecten con Azure y envíen datos de facturación a dicho servicio.

**Conocimientos sobre Microsoft Container Registry y Docker**: debe tener conocimientos básicos sobre los conceptos de Microsoft Container Registry y Docker, como los registros, los repositorios, los contenedores y las imágenes de contenedor, así como sobre los comandos `docker` básicos.  

Para conocer los principios básicos de Docker y de los contenedores, consulte [Introducción a Docker](https://docs.docker.com/engine/docker-overview/).

### <a name="container-requirements-and-recommendations"></a>Recomendaciones y requisitos del contenedor

El contenedor de Reconocer texto requiere un mínimo de 1 núcleo de CPU, con una frecuencia de 2,6 gigahercios (GHz) o mayor, y 8 gigabytes (GB) de memoria asignada, pero se recomienda al menos 2 núcleos de CPU y 8 GB de memoria asignada.

## <a name="request-access-to-the-private-container-registry"></a>Solicitud de acceso al registro de contenedor privado

Primero debe completar y enviar el [formulario de solicitud de contenedores de Vision de Cognitive Services](https://aka.ms/VisionContainersPreview) para solicitar acceso al contenedor de Reconocer texto. El formulario solicita información acerca del usuario y de su empresa, así como del escenario de usuario para el que se va a usar el contenedor. Una vez enviado, el equipo de Azure Cognitive Services revisa el formulario para asegurarse de que cumple los criterios de acceso al registro de contenedor privado.

> [!IMPORTANT]
> Debe usar una dirección de correo electrónico asociada con una cuenta de Microsoft (MSA) o de Azure Active Directory (Azure AD) en el formulario.

Si se aprueba la solicitud, recibirá un correo electrónico con instrucciones que describen cómo obtener las credenciales y tener acceso al registro de contenedor privado.

## <a name="create-a-computer-vision-resource-on-azure"></a>Creación de un recurso de Computer Vision en Azure

Debe crear un recurso de Computer Vision en Azure si quiere usar el contenedor de Reconocer texto. Después de crear el recurso, utilice la clave de suscripción y la dirección URL del punto de conexión desde el recurso para crear una instancia del contenedor. Para obtener más información sobre cómo crear una instancia de un contenedor, consulte [Creación de instancias de contenedor a partir de una imagen de contenedor descargada](#instantiate-a-container-from-a-downloaded-container-image).

Realice los pasos siguientes para crear y recuperar información de un recurso de Azure:

1. Cree un recurso de Azure en Azure Portal.  
   Si quiere usar el contenedor de Reconocer texto, primero debe crear un recurso correspondiente de Computer Vision en Azure Portal. Para más información, consulte [Inicio rápido: Crear una cuenta de Cognitive Services en Azure Portal](../cognitive-services-apis-create-account.md).

1. Obtenga la dirección URL del punto de conexión y la clave de suscripción para el recurso de Azure.  
   Una vez creado el recurso de Azure, debe usar la clave de suscripción y la dirección URL del punto de conexión de ese recurso para crear una instancia del contenedor correspondiente de Reconocer texto. Puede copiar la dirección URL del punto de conexión y la clave de suscripción de las páginas Inicio rápido y Claves, respectivamente, del recurso de Computer Vision en Azure Portal.

## <a name="log-in-to-the-private-container-registry"></a>Inicio de sesión en el registro de contenedor privado

Hay varias formas de autenticarse con el registro de contenedor privado de contenedores de Cognitive Services, pero el método recomendado de la línea de comandos es mediante el uso de la [CLI de Docker](https://docs.docker.com/engine/reference/commandline/cli/).

Use el comando [docker login](https://docs.docker.com/engine/reference/commandline/login/), como se muestra en el ejemplo siguiente, para iniciar sesión en `containerpreview.azurecr.io`, el registro de contenedor privado de los contenedores de Cognitive Services. Reemplace *\<username\>* por el nombre de usuario y *\<password\>* por la contraseña proporcionada en las credenciales que recibió del equipo de Azure Cognitive Services.

```docker
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Si ha protegido las credenciales en un archivo de texto, puede concatenar el contenido de dicho archivo de texto, mediante el comando `cat`, en el comando `docker login`, tal como se muestra en el ejemplo siguiente. Reemplace *\<passwordFile\>* por la ruta de acceso y el nombre del archivo de texto que contiene la contraseña, y *\<username\>* por el nombre de usuario proporcionado en las credenciales.

```docker
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```

## <a name="download-container-images-from-the-private-container-registry"></a>Descarga de imágenes de contenedor del registro de contenedor privado

La imagen de contenedor del contenedor de Reconocer texto está disponible en un registro de contenedor privado de Docker, denominado `containerpreview.azurecr.io`, en Azure Container Registry. Debe descargar la imagen de contenedor para el contenedor de Reconocer texto desde el repositorio para ejecutar el contenedor localmente.

Use el comando [docker pull](https://docs.docker.com/engine/reference/commandline/pull/) para descargar una imagen de contenedor desde el repositorio. Por ejemplo, para descargar la imagen de contenedor del contenedor de Reconocer texto más reciente desde el repositorio, use el siguiente comando:

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:latest
```

Para obtener una descripción completa de las etiquetas disponibles para el contenedor de Reconocer texto, consulte [Reconocer texto](https://go.microsoft.com/fwlink/?linkid=2018655) en Docker Hub.

> [!TIP]
> Puede usar el comando [docker images](https://docs.docker.com/engine/reference/commandline/images/) para enumerar las imágenes de contenedor descargadas. Por ejemplo, el comando siguiente muestra el id., el repositorio y la etiqueta de cada imagen de contenedor descargada, con formato de tabla:
>
>  ```Docker
>  docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
>  ```
>

## <a name="instantiate-a-container-from-a-downloaded-container-image"></a>Creación de instancias de contenedores a partir de una imagen de contenedor descargada

Use el comando [docker run](https://docs.docker.com/engine/reference/commandline/run/) para crear una instancia de un contenedor a partir de una imagen de contenedor descargada. Por ejemplo, el siguiente comando:

* Crea una instancia de un contenedor a partir de la imagen de contenedor de Reconocer texto.
* Asigna dos núcleos de CPU y 8 gigabytes (GB) de memoria.
* Expone el puerto TCP 5000 y asigna un seudo-TTY para el contenedor.
* Quita automáticamente el contenedor tras la salida.

```docker
docker run --rm -it -p 5000:5000 --memory 8g --cpus 2 containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text Eula=accept Billing=https://westus.api.cognitive.microsoft.com/vision/v2.0 ApiKey=0123456789
```

Una vez creada la instancia, puede llamar a las operaciones del contenedor mediante el uso del URI del host del contenedor. Por ejemplo, el siguiente URI del host representa el contenedor de Reconocer texto del que se creó una instancia en el ejemplo anterior:

```http
http://localhost:5000/
```

> [!IMPORTANT]
> Puede tener acceso a la [especificación OpenAPI](https://swagger.io/docs/specification/about/) (anteriormente, especificación de Swagger), que describe las operaciones admitidas por un contenedor con instancias, desde el URI relativo `/swagger` de ese contenedor. Por ejemplo, el siguiente URI proporciona acceso a la especificación de OpenAPI del contenedor de Reconocer texto del que se creó una instancia en el ejemplo anterior:
>
>  ```http
>  http://localhost:5000/swagger
>  ```

Puede [llamar a las operaciones de API REST](https://docs.microsoft.com/azure/cognitive-services/computer-vision/vision-api-how-to-topics/howtocallvisionapi) disponibles del contenedor para reconocimiento de texto sincrónico o asincrónico o usar la biblioteca cliente del [SDK de Computer Vision de Azure Cognitive Services](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision) para llamar a dichas operaciones.  
> [!IMPORTANT]
> Debe tener la versión 3.2.0 o posterior del SDK de Computer Vision de Azure Cognitive Services si quiere usar la biblioteca cliente con el contenedor.

### <a name="asynchronous-text-recognition"></a>Reconocimiento de texto asincrónico

Puede usar las operaciones `POST /vision/v2.0/recognizeText` y `GET /vision/v2.0/textOperations/*{id}*` conjuntamente para reconocer de forma asincrónica texto impreso en una imagen, de manera similar a cómo el servicio de Computer Vision usa esas operaciones de REST correspondientes. Actualmente, el contenedor de Reconocer texto solo reconoce texto impreso, no texto escrito a mano, por lo que dicho contenedor ignora el parámetro `mode` especificado normalmente para la operación del servicio de Computer Vision.

### <a name="synchronous-text-recognition"></a>Reconocimiento de texto sincrónico

Puede usar la operación `POST /vision/v2.0/recognizeTextDirect` para reconocer sincrónicamente texto impreso en una imagen. Dado que esta operación es sincrónica, el cuerpo de solicitud para esta operación es el mismo que para la operación `POST /vision/v2.0/recognizeText`, pero el cuerpo de la respuesta para esta operación es el mismo que el devuelto por la operación `GET /vision/v2.0/textOperations/*{id}*`.

### <a name="billing"></a>Facturación

El contenedor de Reconocer texto envía información de facturación a Azure mediante un recurso correspondiente de Computer Vision en la cuenta de Azure. El contenedor de Reconocer texto usa las siguientes opciones para la facturación:

| Opción | DESCRIPCIÓN |
|--------|-------------|
| `ApiKey` | Clave de API del recurso de Computer Vision utilizado para realizar un seguimiento de la información de facturación.<br/>El valor de esta opción debe establecerse en una clave de API para el recurso aprovisionado de Azure de Computer Vision especificado en `Billing`. |
| `Billing` | Punto de conexión del recurso de Computer Vision utilizado para realizar un seguimiento de la información de facturación.<br/>El valor de esta opción debe establecerse en el URI del punto de conexión de un recurso aprovisionado de Azure de Computer Vision.|
| `Eula` | Indica que ha aceptado la licencia del contenedor.<br/>El valor de esta opción debe establecerse en `accept`. |

> [!IMPORTANT]
> Las tres opciones deben especificarse con valores válidos; en caso contrario, no se inicia el contenedor.

Para obtener más información acerca de estas opciones, consulte [Configure containers](computer-vision-resource-container-config.md) (Configuración de contenedores).

## <a name="summary"></a>Resumen

En este artículo, ha aprendido los conceptos y el flujo de trabajo para la descarga, instalación y ejecución de contenedores de Computer Vision. En resumen:

* Computer Vision proporciona uno de los contenedores de Linux para Docker para detectar y extraer texto impreso.
* Las imágenes de contenedor se descargan desde un registro de contenedor privado de Azure.
* Las imágenes de contenedor se ejecutan en Docker.
* Puede usar la API REST o el SDK para llamar a operaciones en contenedores de Computer Vision mediante la especificación del URI del host del contenedor.
* Debe especificar la información de facturación al crear una instancia de un contenedor.

> [!IMPORTANT]
> Los contenedores de Cognitive Services no tienen licencia para ejecutarse sin estar conectados a Azure para realizar mediciones. Los clientes tienen que habilitar los contenedores para comunicar la información de facturación con el servicio de medición en todo momento. Los contenedores de Cognitive Services no envían datos de los clientes (por ejemplo, la imagen o el texto que se está analizando) a Microsoft.

## <a name="next-steps"></a>Pasos siguientes

* Revise [Configure containers](computer-vision-resource-container-config.md) (Configuración de contenedores) para ver las opciones de configuración.
* Revise [Introducción a Computer Vision](Home.md) para obtener más información sobre el reconocimiento de texto escrito a mano e impreso.  
* Consulte [Computer Vision API](//westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) para obtener más información acerca de los métodos que admite el contenedor.
* Consulte [Preguntas más frecuentes (P+F)](FAQ.md) para resolver problemas relacionados con la funcionalidad de Computer Vision.
