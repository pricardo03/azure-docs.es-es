---
title: Compatibilidad con los contenedores
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo los contenedores de Docker pueden obtener Cognitive Services más próximos a los datos.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: article
ms.date: 04/16/2019
ms.author: diberry
ms.openlocfilehash: d84ebcdbd557a56c4c20600be67f26a084d43ed2
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59681818"
---
# <a name="container-support-in-azure-cognitive-services"></a>Compatibilidad con contenedores en Azure Cognitive Services

La compatibilidad con contenedores en Azure Cognitive Services permite a los desarrolladores usar las mismas API enriquecidas que están disponibles en Azure, y permite flexibilidad en cuanto a dónde implementar y hospedar los servicios que vienen con los [contenedores de Docker](https://www.docker.com/what-container). La compatibilidad con contenedores está disponible actualmente en versión preliminar para un subconjunto de Azure Cognitive Services, incluidas las partes de [Computer Vision](Computer-vision/Home.md), [Face](Face/Overview.md), [Text Analytics](text-analytics/overview.md) y [Language Understanding](LUIS/luis-container-howto.md) (LUIS).

La creación de contenedores es un enfoque de distribución de software en el que una aplicación o servicio, incluidas sus dependencias y la configuración, se empaqueta como una imagen de contenedor. La imagen de contenedor puede implementarse en un host de contenedor con pocas o ningunas modificaciones. Los contenedores están aislados entre sí y del sistema operativo subyacente, con una superficie menor que una máquina virtual. Se pueden crear instancias de contenedores a partir de las imágenes de contenedor para las tareas a corto plazo y quitarlas cuando ya no se necesiten.

En el siguiente vídeo se explica cómo se utiliza un contenedor de Cognitive Services.

[![Demostración de contenedores de Cognitive Services](./media/index/containers-video-image.png)](https://azure.microsoft.com/resources/videos/containers-support-of-cognitive-services)

Los servicios [Computer Vision](Computer-vision/Home.md), [Face](Face/Overview.md), [Text Analytics](text-analytics/overview.md) y [Language Understanding (LUIS)](LUIS/what-is-luis.md) están disponibles en [Microsoft Azure](https://azure.microsoft.com). Inicie sesión en [Azure Portal](https://portal.azure.com/) para crear y explorar recursos de Azure para estos servicios.

## <a name="features-and-benefits"></a>Características y ventajas

- **Control sobre datos**: permita que los clientes elijan dónde Cognitive Services procesa los datos. Esto es esencial para los clientes que no pueden enviar datos a la nube, pero necesitan tener acceso a la tecnología de Cognitive Services. Admite la coherencia en entornos híbridos: a través de datos, administración, identidad y seguridad.
- **Control sobre las actualizaciones del modelo**: proporcione flexibilidad a los clientes sobre el control de versiones y la actualización de los modelos implementados en sus soluciones.
- **Arquitectura portátil**: habilite la creación de una arquitectura de aplicación portátil que se pueda implementar en Azure, en el entorno local y en la red perimetral. Los contenedores se pueden implementar directamente en [Azure Kubernetes Service](../aks/index.yml), [Azure Container Instances](../container-instances/index.yml) o en un clúster de [Kubernetes](https://kubernetes.io/) implementado en [Azure Stack](../azure-stack/index.yml). Para obtener más información, consulte [Implementación de Kubernetes en Azure Stack](../azure-stack/user/azure-stack-solution-template-kubernetes-deploy.md).
- **Alto rendimiento y baja latencia**: proporcione a los clientes la capacidad de escalar para los requisitos de alto rendimiento y baja latencia permitiendo que Cognitive Services se ejecute físicamente cerca de sus datos y lógica de aplicación. Los contenedores no limitan las transacciones por segundo (TPS) y se pueden escalar tanto vertical como horizontalmente para controlar la demanda si se proporcionan los recursos de hardware necesarios. 


## <a name="containers-in-azure-cognitive-services"></a>Contenedores en Azure Cognitive Services

Los contenedores de Azure Cognitive Services proporcionan el siguiente conjunto de contenedores de Docker, cada uno de los cuales contiene un subconjunto de funcionalidades de servicios de Azure Cognitive Services:

| Servicio | Plan de tarifa admitido | Contenedor | DESCRIPCIÓN |
|---------|----------|----------|-------------|
|[Computer Vision](Computer-vision/computer-vision-how-to-install-containers.md) |F0, S1|**Reconocer texto** |Extrae texto impreso de imágenes que muestren diversos objetos con diferentes superficies y fondos, como recibos, pósteres y tarjetas de visita.<br/><br/>**Importante:** Actualmente, el contenedor de Reconocer texto solo funciona en inglés.<br>[Solicitar acceso](Computer-vision/computer-vision-how-to-install-containers.md#request-access-to-the-private-container-registry)|
|[Face](Face/face-how-to-install-containers.md) |F0, S0|**Face** |Detecta caras humanas en imágenes e identifica atributos, incluidos faciales (como narices y ojos), sexo, edad y otras características faciales previstas por la máquina. Además de la detección, Face puede comprobar si dos caras en la misma o en diferentes imágenes son iguales mediante una puntuación de confianza, o bien comparar caras en una base de datos para ver si ya existe un aspecto similar o una cara idéntica. También puede organizar caras similares en grupos mediante rasgos visuales compartidos.<br>[Solicitar acceso](Face/face-how-to-install-containers.md#request-access-to-the-private-container-registry) |
|[LUIS](LUIS/luis-container-howto.md) |F0, S0|**LUIS** ([imagen](https://go.microsoft.com/fwlink/?linkid=2043204))|Carga un modelo de Language Understanding entrenado o publicado, lo que también se conoce como aplicación de LUIS, en un contenedor de Docker y proporciona acceso a las predicciones de consulta de los puntos de conexión de la API del contenedor. Puede recopilar registros de consultas en el contenedor y cargarlos de nuevo en el [portal de LUIS](https://www.luis.ai) para mejorar la precisión de predicción de la aplicación.|
|[Text Analytics](text-analytics/how-tos/text-analytics-how-to-install-containers.md) |F0, S|**Extracción de frases clave** ([imagen](https://go.microsoft.com/fwlink/?linkid=2018757)) |Extrae las frases clave para identificar los puntos principales. Por ejemplo, si el texto de entrada es "La comida estaba deliciosa y el personal era maravilloso", la API devuelve los principales puntos de conversación: "comida" y "personal maravilloso". |
|[Text Analytics](text-analytics/how-tos/text-analytics-how-to-install-containers.md)|F0, S|**Detección de idioma** ([imagen](https://go.microsoft.com/fwlink/?linkid=2018759)) |Se detecta el idioma (120 como máximo) en que está escrito el texto de entrada y se usa un código de idioma único para informar acerca de cada documento enviado en la solicitud. El código de idioma se empareja con una puntuación que indica la intensidad de esta. |
|[Text Analytics](text-analytics/how-tos/text-analytics-how-to-install-containers.md)|F0, S|**Análisis de sentimiento** ([imagen](https://go.microsoft.com/fwlink/?linkid=2018654)) |Analiza el texto sin formato para obtener pistas sobre opiniones positivas o negativas. Esta API devuelve una puntuación de la opción, que oscila entre 0 y 1, con respecto a cada documento, donde 1 es la más positiva. Los modelos de análisis se entrenan previamente con una gran cantidad de cuerpo de texto y tecnologías de idioma natural de Microsoft. Para los [idiomas seleccionados](./text-analytics/language-support.md), la API puede analizar y puntuar cualquier texto sin formato que se proporcione, y devolver los resultados directamente a la aplicación que realiza la llamada. |

Además, se admiten los contenedores en Cognitive Services [All-In-One oferta](https://azure.microsoft.com/pricing/details/cognitive-services/). Puede crear un único recurso de All-In-One Cognitive Services y usar la misma clave de facturación para todos los tipos de contenedor que se ha mencionado anteriormente.

## <a name="container-availability-in-azure-cognitive-services"></a>Disponibilidad de contenedores en Azure Cognitive Services

Los contenedores de Azure Cognitive Services están disponibles públicamente a través de su suscripción a Azure y se pueden extraer imágenes de contenedor de Docker desde Microsoft Container Registry o Docker Hub. Puede usar el comando [docker pull](https://docs.docker.com/engine/reference/commandline/pull/) para descargar una imagen de contenedor desde el registro apropiado.

> [!IMPORTANT]
> Actualmente, debe completar un proceso de registro para obtener acceso a los contenedores de [Face](Face/face-how-to-install-containers.md) y [Reconocer texto](Computer-vision/computer-vision-how-to-install-containers.md), en el que debe rellenar y enviar un cuestionario con preguntas acerca de usted, su empresa y el caso de uso para el que desea implementar los contenedores. Una vez que se le concede acceso y se le proporcionan las credenciales, puede extraer las imágenes de contenedor para los contenedores de Face y Reconocer texto a partir de un registro de contenedor privado hospedado por Azure Container Registry.

## <a name="prerequisites"></a>Requisitos previos

Debe cumplir los siguientes requisitos previos para poder utilizar contenedores de Azure Cognitive Services:

**Motor de Docker**: debe tener el motor de Docker instalado localmente. Docker proporciona paquetes que configuran el entorno de Docker en [macOS](https://docs.docker.com/docker-for-mac/), [Linux](https://docs.docker.com/engine/installation/#supported-platforms) y [Windows](https://docs.docker.com/docker-for-windows/). En Windows, Docker debe configurarse para admitir los contenedores de Linux. Los contenedores de Docker también se pueden implementar directamente en [Azure Kubernetes Service](../aks/index.yml) o [Azure Container Instances](../container-instances/index.yml).

Docker debe configurarse para permitir que los contenedores se conecten con Azure y envíen datos de facturación a dicho servicio.

**Familiaridad con Microsoft Container Registry y Docker**: debe tener un conocimiento básico de los conceptos de Microsoft Container Registry y Docker, como los registros, los repositorios, los contenedores y las imágenes de contenedor, así como de los comandos `docker` básicos.

Para conocer los principios básicos de Docker y de los contenedores, consulte [Introducción a Docker](https://docs.docker.com/engine/docker-overview/).

Los contenedores individuales también pueden tener sus propios requisitos, incluidos los requisitos de asignación de memoria y servidor.

## <a name="developer-samples"></a>Ejemplos para desarrolladores

Hay ejemplos para desarrolladores disponibles en nuestro [repositorio de GitHub](https://github.com/Azure-Samples/cognitive-services-containers-samples).

## <a name="next-steps"></a>Pasos siguientes

Instale y explore la funcionalidad proporcionada por los contenedores en Azure Cognitive Services:

* [Install and use Computer Vision containers](Computer-vision/computer-vision-how-to-install-containers.md) (Instalación y uso de contenedores de Computer Vision)
* [Install and use Face containers](Face/face-how-to-install-containers.md) (Instalación y uso de contenedores de Face)
* [Install and use Text Analytics containers](text-analytics/how-tos/text-analytics-how-to-install-containers.md) (Instalación y uso de contenedores de Text Analytics)
* [Install and use Language Understanding (LUIS) containers](LUIS/luis-container-howto.md) [Instalación y utilización de los contenedores de Language Understanding (LUIS)]
