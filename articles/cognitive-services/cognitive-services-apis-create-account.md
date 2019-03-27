---
title: Creación de una cuenta de Cognitive Services en Azure Portal
titlesuffix: Azure Cognitive Services
description: Cómo crear una cuenta de Azure Cognitive Services APIs en Azure portal.
services: cognitive-services
author: garyericson
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 03/22/2019
ms.author: garye
ms.openlocfilehash: 7df429aa848c6f9fb9abe09fbf4357db20fcde6a
ms.sourcegitcommit: fbfe56f6069cba027b749076926317b254df65e5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58472861"
---
# <a name="quickstart-create-a-cognitive-services-account-in-the-azure-portal"></a>Inicio rápido: Creación de una cuenta de Cognitive Services en Azure Portal

En este artículo de inicio rápido, aprenderá cómo suscribirse a Azure Cognitive Services y cómo crear una suscripción de un solo servicio o de varios servicios. Estos servicios se representan mediante los [recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal) de Azure, que le permitirán conectarse a una o varias instancias de Cognitive Services APIs disponibles.

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción válida a Azure. [Cree una cuenta](https://azure.microsoft.com/free/) gratis.

## <a name="create-and-subscribe-to-an-azure-cognitive-services-resource"></a>Crear y suscribirse a un recurso de Azure Cognitive Services

Antes de empezar, es importante saber que hay dos tipos de suscripciones a Azure Cognitive Services. La primera es una suscripción a un servicio único, como Computer Vision o los servicios de voz. Una suscripción a un servicio único está restringida a ese recurso. La segunda es una suscripción a varios servicios de Azure Cognitive Services. Esta suscripción le permite usar una única suscripción para la mayoría de las instancias de Azure Cognitive Services. Esta opción también consolida la facturación. Consulte [Precios de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/) para más información.

>[!WARNING]
> En este momento, estos servicios **no** admiten las claves de varios servicios: QnA Maker, servicios de voz, visión personalizada y Detector de anomalías.

Las siguientes secciones le guiarán por la creación de una suscripción de uno o varios servicios.

### <a name="single-service-subscription"></a>Suscripción a un servicio único

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y haga clic en **+Crear un recurso**.

    ![Seleccione Cognitive Services APIs.](media/cognitive-services-apis-create-account/azurePortalScreen.png)

2. En Azure Marketplace, seleccione **IA y Machine Learning**. Si no ve el servicio que le interesa, haga clic en **Ver todo** para ver el catálogo completo de Cognitive Services APIs.

    ![Seleccione Cognitive Services APIs.](media/cognitive-services-apis-create-account/azureMarketplace.png)

3. En la página **Crear**, proporcione la siguiente información:

    |    |    |
    |--|--|
    | **Nombre** | Nombre descriptivo para el recurso de Cognitive Services. Es recomendable usar un nombre descriptivo, por ejemplo, *MyNameFaceAPIAccount*. |
    | **Suscripción** | Seleccione una de las suscripciones de Azure disponibles. |
    | **Ubicación** | Ubicación de la instancia de Cognitive Services. Las diferentes ubicaciones pueden crear latencias, pero no tienen ningún impacto en la disponibilidad del tiempo de ejecución del recurso. |
    | **Plan de tarifa** | Costo de la cuenta de Cognitive Services, que depende del uso y de las opciones que elija. Para obtener más información, consulte los [detalles de los precios](https://azure.microsoft.com/pricing/details/cognitive-services/).
    | **Grupos de recursos** | [Grupo de recursos de Azure](https://docs.microsoft.com/azure/architecture/cloud-adoption/getting-started/azure-resource-access#what-is-an-azure-resource-group) que contendrá su recurso de Cognitive Services. Puede crear un nuevo grupo o agregarlo a uno ya existente. |

    ![Pantalla de creación de recursos](media/cognitive-services-apis-create-account/resource_create_screen.png)

### <a name="multi-service-subscription"></a>Suscripción a varios servicios

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y haga clic en **+Crear un recurso**.

    ![Seleccione Cognitive Services APIs.](media/cognitive-services-apis-create-account/azurePortalScreenMulti.png)

2. Busque la barra de búsqueda y escriba: **Cognitive Services**.

    ![Búsqueda de Cognitive Services](media/cognitive-services-apis-create-account/azureCogServSearchMulti.png)

3. Seleccione **Cognitive Services**.

    ![Selección de Cognitive Services](media/cognitive-services-apis-create-account/azureMarketplaceMulti.png)

3. En la página **Crear**, proporcione la siguiente información:

    |    |    |
    |--|--|
    | **Nombre** | Nombre descriptivo para el recurso de Cognitive Services. Es recomendable usar un nombre descriptivo, por ejemplo, *MyCognitiveServicesAccount*. |
    | **Suscripción** | Seleccione una de las suscripciones de Azure disponibles. |
    | **Ubicación** | Ubicación de la instancia de Cognitive Services. Las diferentes ubicaciones pueden crear latencias, pero no tienen ningún impacto en la disponibilidad del tiempo de ejecución del recurso. |
    | **Plan de tarifa** | Costo de la cuenta de Cognitive Services, que depende del uso y de las opciones que elija. Para obtener más información, consulte los [detalles de los precios](https://azure.microsoft.com/pricing/details/cognitive-services/).
    | **Grupos de recursos** | [Grupo de recursos de Azure](https://docs.microsoft.com/azure/architecture/cloud-adoption/getting-started/azure-resource-access#what-is-an-azure-resource-group) que contendrá su recurso de Cognitive Services. Puede crear un nuevo grupo o agregarlo a uno ya existente. |

    ![Pantalla de creación de recursos](media/cognitive-services-apis-create-account/resource_create_screen_multi.png)

## <a name="access-your-resource"></a>Obtenga acceso al recurso

> [!NOTE]
> Los propietarios de las suscripciones pueden deshabilitar las opciones de creación de cuentas de Cognitive Services para los grupos de recursos y las suscripciones; para ello, deben aplicar la [directiva de Azure](https://docs.microsoft.com/azure/governance/policy/overview#policy-definition), asignar una definición de directiva "Tipos de recursos no permitidos" y especificar **Microsoft.CognitiveServices/accounts** como el tipo de recurso de destino.

Después de crear el recurso, puede obtener acceso al mismo desde el panel de Azure si lo ancló allí. De lo contrario, puede encontrarlo en la opción **Grupos de recursos**.

Asimismo, en el recurso de Cognitive Services puede usar la dirección URL del punto de conexión y las claves en la sección **Información general**, para poder comenzar a realizar llamadas a la API en las aplicaciones.

![Pantalla de recursos](media/cognitive-services-apis-create-account/resourceScreen.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Autenticación de solicitudes en Azure Cognitive Services](authentication.md)

## <a name="see-also"></a>Vea también

* [Inicio rápido: Extract handwritten text from an image](https://docs.microsoft.com/azure/cognitive-services/computer-vision/quickstarts/csharp-hand-text) (Inicio rápido: Extracción de texto manuscrito de una imagen)
* [Tutorial: Create an app to detect and frame faces in an image](https://docs.microsoft.com/azure/cognitive-services/Face/Tutorials/FaceAPIinCSharpTutorial) (Tutorial: Creación de una aplicación para detectar y enmarcar caras en una imagen)
* [Build a custom search webpage](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/tutorials/custom-search-web-page) (Compilación de una página web de Custom Search)
* [Integrate Language Understanding (LUIS) with a bot using the Bot Framework](https://docs.microsoft.com/azure/cognitive-services/luis/luis-nodejs-tutorial-build-bot-framework-sample) (Integrar Language Understanding (LUIS) con un bot mediante Bot Framework)
