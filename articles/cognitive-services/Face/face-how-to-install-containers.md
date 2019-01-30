---
title: Instalación y ejecución de contenedores
titlesuffix: Face - Azure Cognitive Services
description: Cómo descargar, instalar y ejecutar contenedores para Face en este tutorial paso a paso.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 01/22/2019
ms.author: diberry
ms.openlocfilehash: 310311c22be25960b15f20d573624f50b0f618b1
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54474819"
---
# <a name="install-and-run-containers"></a>Instalación y ejecución de contenedores

La creación de contenedores es un enfoque de distribución de software en el que una aplicación o servicio se empaqueta como una imagen de contenedor. La configuración y las dependencias de la aplicación o del servicio se incluyen en la imagen de contenedor. La imagen de contenedor puede implementarse en un host de contenedor con pocas modificaciones o ninguna. Los contenedores están aislados entre sí y del sistema operativo subyacente, con una superficie menor que una máquina virtual. Se pueden crear instancias de contenedores a partir de las imágenes de contenedor para las tareas a corto plazo y quitarlas cuando ya no se necesiten.

Face proporciona un contenedor de Linux estandarizado para Docker, denominado Face, que detecta caras humanas en imágenes e identifica atributos, incluidos faciales (como narices y ojos), sexo, edad y otras características faciales previstas por la máquina. Además de la detección, Face puede comprobar si dos caras en la misma o en diferentes imágenes son iguales mediante una puntuación de confianza, o bien comparar caras en una base de datos para ver si ya existe un aspecto similar o una cara idéntica. También puede organizar caras similares en grupos mediante rasgos visuales compartidos.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="preparation"></a>Preparación

Debe cumplir los siguientes requisitos previos para poder usar el contenedor de Face:

**Motor de Docker**: debe tener el motor de Docker instalado localmente. Docker proporciona paquetes que configuran el entorno de Docker en [macOS](https://docs.docker.com/docker-for-mac/), [Linux](https://docs.docker.com/engine/installation/#supported-platforms) y [Windows](https://docs.docker.com/docker-for-windows/). En Windows, Docker debe configurarse para admitir los contenedores de Linux. Los contenedores de Docker también se pueden implementar directamente en [Azure Kubernetes Service](../../aks/index.yml), [Azure Container Instances](../../container-instances/index.yml) o en un clúster de [Kubernetes](https://kubernetes.io/) implementado en [Azure Stack](../../azure-stack/index.yml). Para obtener más información sobre la implementación de Kubernetes en Azure Stack, consulte [Implementación de Kubernetes en Azure Stack](../../azure-stack/user/azure-stack-solution-template-kubernetes-deploy.md).

Docker debe configurarse para permitir que los contenedores se conecten con Azure y envíen datos de facturación a dicho servicio.

**Familiaridad con Microsoft Container Registry y Docker**: debe tener un conocimiento básico de los conceptos de Microsoft Container Registry y Docker, como los registros, los repositorios, los contenedores y las imágenes de contenedor, así como de los comandos `docker` básicos.  

Para conocer los principios básicos de Docker y de los contenedores, consulte [Introducción a Docker](https://docs.docker.com/engine/docker-overview/).

### <a name="container-requirements-and-recommendations"></a>Recomendaciones y requisitos del contenedor

El contenedor de Face requiere como mínimo 1 núcleo de CPU, con una frecuencia de 2,6 gigahercios (GHz) o mayor, y 4 gigabytes (GB) de memoria asignada, pero se recomienda tener al menos 2 núcleos de CPU y 6 GB de memoria asignada.

## <a name="request-access-to-the-private-container-registry"></a>Solicitud de acceso al registro de contenedor privado

Primero debe completar y enviar el [formulario de solicitud de contenedores de Vision de Cognitive Services](https://aka.ms/VisionContainersPreview) para solicitar acceso al contenedor de Face. El formulario solicita información acerca del usuario y de su empresa, así como del escenario de usuario para el que se va a usar el contenedor. Una vez enviado, el equipo de Azure Cognitive Services revisa el formulario para asegurarse de que cumple los criterios de acceso al registro de contenedor privado.

> [!IMPORTANT]
> Debe usar una dirección de correo electrónico asociada con una cuenta de Microsoft (MSA) o de Azure Active Directory (Azure AD) en el formulario.

Si se aprueba la solicitud, recibirá un correo electrónico con instrucciones que describen cómo obtener las credenciales y tener acceso al registro de contenedor privado.

## <a name="create-a-face-resource-on-azure"></a>Creación de un recurso de Face en Azure

Si quiere usar el contenedor de Face, debe crear un recurso de Face en Azure. Después de crear el recurso, utilice la clave de suscripción y la dirección URL del punto de conexión desde el recurso para crear una instancia del contenedor. Para obtener más información sobre cómo crear una instancia de un contenedor, consulte [Creación de instancias de contenedor a partir de una imagen de contenedor descargada](#instantiate-a-container-from-a-downloaded-container-image).

Siga los pasos a continuación para crear y recuperar información de un recurso de Face:

1. Cree un recurso de Face en Azure Portal.  
   Si quiere usar el contenedor de Face, primero debe crear un recurso correspondiente de Face en Azure Portal. Para más información, consulte [Inicio rápido: Creación de una cuenta de Cognitive Services en Azure Portal](../cognitive-services-apis-create-account.md).

1. Obtenga la dirección URL del punto de conexión y la clave de suscripción para el recurso de Azure.  
   Una vez creado el recurso de Azure, debe usar la clave de suscripción y la dirección URL del punto de conexión de ese recurso para crear una instancia del contenedor correspondiente de Face. Puede copiar la dirección URL del punto de conexión y la clave de suscripción de las páginas Inicio rápido y Claves, respectivamente, del recurso de Face en Azure Portal.

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

La imagen de contenedor del contenedor de Face está disponible en un registro de contenedor privado de Docker, denominado `containerpreview.azurecr.io`, en Azure Container Registry. Debe descargar la imagen de contenedor para el contenedor de Face desde el repositorio para ejecutar el contenedor localmente.

Use el comando [docker pull](https://docs.docker.com/engine/reference/commandline/pull/) para descargar una imagen de contenedor desde el repositorio. Por ejemplo, para descargar la imagen de contenedor del contenedor de Face más reciente desde el repositorio, use el siguiente comando:

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-face:latest
```

Para obtener una descripción completa de las etiquetas disponibles para el contenedor de Face, consulte [Reconocer texto](https://go.microsoft.com/fwlink/?linkid=2018655) en Docker Hub.

> [!TIP]
> Puede usar el comando [docker images](https://docs.docker.com/engine/reference/commandline/images/) para enumerar las imágenes de contenedor descargadas. Por ejemplo, el comando siguiente muestra el id., el repositorio y la etiqueta de cada imagen de contenedor descargada, con formato de tabla:
>
>  ```Docker
>  docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
>  ```
>

## <a name="instantiate-a-container-from-a-downloaded-container-image"></a>Creación de instancias de contenedores a partir de una imagen de contenedor descargada

Use el comando [docker run](https://docs.docker.com/engine/reference/commandline/run/) para crear una instancia de un contenedor a partir de una imagen de contenedor descargada. Por ejemplo, el siguiente comando:

* Crea una instancia de un contenedor a partir de la imagen de contenedor de Face.
* Asigna dos núcleos de CPU y 6 gigabytes (GB) de memoria.
* Expone el puerto TCP 5000 y asigna un seudo-TTY para el contenedor.
* Quita automáticamente el contenedor tras la salida.

```Docker
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789
```

> [!IMPORTANT]
> Las `Eula`, `Billing` y `ApiKey` se deben especificar para crear una instancia del contenedor; de lo contrario, este no se iniciará.  Para obtener más información, vea [Facturación](#billing).

Una vez creada la instancia, puede llamar a las operaciones del contenedor mediante el uso del URI del host del contenedor. Por ejemplo, el siguiente URI del host representa el contenedor de Face del que se creó una instancia en el ejemplo anterior:

```http
http://localhost:5000/
```

> [!TIP]
> Puede tener acceso a la [especificación OpenAPI](https://swagger.io/docs/specification/about/) (anteriormente, especificación de Swagger), que describe las operaciones admitidas por un contenedor con instancias, desde el URI relativo `/swagger` de ese contenedor. Por ejemplo, el siguiente URI proporciona acceso a la especificación de OpenAPI del contenedor de Face del que se creó una instancia en el ejemplo anterior:
>
>  ```http
>  http://localhost:5000/swagger
>  ```

Puede [llamar a las operaciones de API REST](https://docs.microsoft.com/azure/cognitive-services/face/face-api-how-to-topics/howtodetectfacesinimage) disponibles del contenedor o usar la biblioteca cliente de la [biblioteca cliente de Face de Azure Cognitive Services](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/) para llamar a dichas operaciones.  
> [!IMPORTANT]
> Debe tener la versión 2.0 o posterior de la biblioteca cliente de Face de Azure Cognitive Services si quiere usar la biblioteca cliente con el contenedor.

La única diferencia entre llamar a una operación determinada del contenedor y llamar a la misma operación desde un servicio correspondiente en Azure es que usará el URI del host del contenedor, en lugar del URI del host de una región de Azure, para llamar a la operación. Por ejemplo, si quisiese usar una instancia de Face que se ejecutase en la región de Azure Oeste de EE. UU. para detectar caras, llamaría a la siguiente operación de API REST:

```http
POST https://westus.api.cognitive.microsoft.com/face/v1.0/detect
```

Si quisiese utilizar un contenedor de Face que se ejecutase en el equipo local según su configuración predeterminada para detectar caras, llamaría a la siguiente operación de API REST:

```http
POST http://localhost:5000/face/v1.0/detect
```

### <a name="billing"></a>Facturación

El contenedor de Face envía información de facturación a Azure mediante un recurso correspondiente de Face en la cuenta de Azure. Los contenedores de Face usan las siguientes opciones de la línea de comandos para la facturación:

| Opción | DESCRIPCIÓN |
|--------|-------------|
| `ApiKey` | Clave de API del recurso de Face utilizado para realizar un seguimiento de la información de facturación.<br/>El valor de esta opción debe establecerse en una clave de API para el recurso aprovisionado de Azure de Face especificado en `Billing`. |
| `Billing` | Punto de conexión del recurso de Face utilizado para realizar un seguimiento de la información de facturación.<br/>El valor de esta opción debe establecerse en el URI del punto de conexión de un recurso aprovisionado de Azure de Face.|
| `Eula` | Indica que ha aceptado la licencia del contenedor.<br/>El valor de esta opción debe establecerse en `accept`. |

> [!IMPORTANT]
> Las tres opciones deben especificarse con valores válidos; en caso contrario, no se inicia el contenedor.

Para obtener más información acerca de estas opciones, consulte [Configure containers](face-resource-container-config.md) (Configuración de contenedores).

## <a name="summary"></a>Resumen

En este artículo, ha aprendido los conceptos y el flujo de trabajo para la descarga, instalación y ejecución de contenedores de Face. En resumen:

* Face proporciona un contenedor de Linux para Docker, denominado Face, para detectar o identificar caras mediante una base de datos de personas.
* Las imágenes de contenedor se descargan desde un registro de contenedor privado de Azure.
* Las imágenes de contenedor se ejecutan en Docker.
* Puede usar la API REST o el SDK para llamar a operaciones en contenedores de Face mediante la especificación del URI del host del contenedor.
* Debe especificar la información de facturación al crear una instancia de un contenedor.

> [!IMPORTANT]
> Los contenedores de Cognitive Services no tienen licencia para ejecutarse sin estar conectados a Azure para realizar mediciones. Los clientes tienen que habilitar los contenedores para comunicar la información de facturación con el servicio de medición en todo momento. Los contenedores de Cognitive Services no envían datos de los clientes (por ejemplo, la imagen o el texto que se está analizando) a Microsoft.

## <a name="next-steps"></a>Pasos siguientes

* Revise [Configure containers](face-resource-container-config.md) (Configuración de contenedores) para ver las opciones de configuración.
* Revise [Introducción a Face](Overview.md) para obtener más información acerca de la detección e identificación de caras.  
* Consulte [Face API](//westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) para obtener más información acerca de los métodos que admite el contenedor.
* Consulte [Preguntas más frecuentes (P+F)](FAQ.md) para resolver problemas relacionados con la funcionalidad de Face.
* Usar más [Contenedores de Cognitive Services](../cognitive-services-container-support.md)