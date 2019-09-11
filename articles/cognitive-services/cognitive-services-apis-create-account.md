---
title: Creación de un recurso de Cognitive Services en Azure Portal
titleSuffix: Azure Cognitive Services
description: Comience a usar Azure Cognitive Services mediante la creación y suscripción a un recurso en Azure Portal.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 07/16/2019
ms.author: aahi
ms.openlocfilehash: c7db2b4d49e3b9297c32d2e11ffe7c7702c17544
ms.sourcegitcommit: 32242bf7144c98a7d357712e75b1aefcf93a40cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70274671"
---
# <a name="create-a-cognitive-services-resource-using-the-azure-portal"></a>Creación de un recurso de Cognitive Services con Azure Portal

Use este inicio rápido para crear un recurso de Azure Cognitive Services con Azure Portal. Después de crear correctamente un recurso de Cognitive Services, obtendrá un punto de conexión y una clave que puede usar para autenticar las aplicaciones.

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción a Azure válida: [cree una de manera gratuita](https://azure.microsoft.com/free/).

[!INCLUDE [cognitive-services-subscription-types](../../includes/cognitive-services-subscription-types.md)]

## <a name="create-a-new-azure-cognitive-services-resource"></a>Creación de un nuevo recurso de Azure Cognitive Services

Antes de crear un recurso de Cognitive Services, debe tener un grupo de recursos de Azure que contenga el recurso. Al crear un nuevo recurso, puede elegir entre crear un grupo de recursos o usar uno existente. En este artículo se muestra cómo crear un nuevo grupo de recursos.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y haga clic en **+Crear un recurso**.

    ![Seleccione Cognitive Services APIs.](media/cognitive-services-apis-create-account/azurePortalScreenMulti.png)

2. Como se explicó anteriormente, puede crear un recurso de Cognitive Services de dos maneras: mediante un recurso de varios servicios o un recurso de servicio único.

    #### <a name="multi-service-resourcetabmultiservice"></a>[Recurso de varios servicios](#tab/multiservice)

    Para crear un recurso de varios servicios, escriba **Cognitive Services** en la barra de búsqueda.

    ![Búsqueda de Cognitive Services](media/cognitive-services-apis-create-account/azureCogServSearchMulti.png)

    En la página Cognitive Services, seleccione **Crear**.

    ![Creación de una cuenta de Cognitive Services](media/cognitive-services-apis-create-account/azurecogservsearchmulti-2.png)

    #### <a name="single-service-resourcetabsingleservice"></a>[Recurso de servicio único](#tab/singleservice)

    Para ver todos los Cognitive Services disponibles, seleccione **IA y Machine Learning** en **Azure Marketplace**. Si no ve el servicio que le interesa, haga clic en **Ver todo** y desplácese hasta **Cognitive Services**. Haga clic en **Ver más** para ver el catálogo completo de Cognitive Services.

    Una vez que esté en el servicio que le interesa, haga clic en **Crear**.
    
    ![Seleccione Cognitive Services APIs.](media/cognitive-services-apis-create-account/azureMarketplace.png)

    ***
3. En la página **Crear**, proporcione la siguiente información:

    #### <a name="multi-service-resourcetabmultiservice"></a>[Recurso de varios servicios](#tab/multiservice)

    |    |    |
    |--|--|
    | **Nombre** | Nombre descriptivo para el recurso de Cognitive Services. Por ejemplo, *MyCognitiveServicesResource*. |
    | **Suscripción** | Seleccione una de las suscripciones de Azure disponibles. |
    | **Ubicación** | Ubicación de la instancia de Cognitive Services. Las diferentes ubicaciones pueden crear latencias, pero no tienen ningún impacto en la disponibilidad del tiempo de ejecución del recurso. No olvide su ubicación de Azure, ya que puede necesitarla cuando llame a Azure Cognitive Services. |
    | **Plan de tarifa** | Costo de la cuenta de Cognitive Services, que depende del uso y de las opciones que elija. Para obtener más información, consulte los [detalles de los precios](https://azure.microsoft.com/pricing/details/cognitive-services/).
    | **Grupos de recursos** | El grupo de recursos de Azure que contendrá su recurso de Cognitive Services. Puede crear un nuevo grupo o agregarlo a uno ya existente. |

    ![Pantalla de creación de recursos](media/cognitive-services-apis-create-account/resource_create_screen-multi.png)

    Haga clic en **Create**(Crear).

    #### <a name="single-service-resourcetabsingleservice"></a>[Recurso de servicio único](#tab/singleservice)

    |    |    |
    |--|--|
    | **Nombre** | Nombre descriptivo para el recurso de Cognitive Services. Por ejemplo, *TextAnalyticsResource*. |
    | **Suscripción** | Seleccione una de las suscripciones de Azure disponibles. |
    | **Ubicación** | Ubicación de la instancia de Cognitive Services. Las diferentes ubicaciones pueden crear latencias, pero no tienen ningún impacto en la disponibilidad del tiempo de ejecución del recurso. No olvide su ubicación de Azure, ya que puede necesitarla cuando llame a Azure Cognitive Services. |
    | **Plan de tarifa** | Costo de la cuenta de Cognitive Services, que depende del uso y de las opciones que elija. Para obtener más información, consulte los [detalles de los precios](https://azure.microsoft.com/pricing/details/cognitive-services/).
    | **Grupos de recursos** | El grupo de recursos de Azure que contendrá su recurso de Cognitive Services. Puede crear un nuevo grupo o agregarlo a uno ya existente. |

    ![Pantalla de creación de recursos](media/cognitive-services-apis-create-account/resource_create_screen.png)

    Haga clic en **Create**(Crear).

    ***

## <a name="get-the-keys-for-your-resource"></a>Obtención de las claves del recurso

Una vez que el recurso se haya creado correctamente, aparecerá una notificación emergente en la parte superior derecha de la pantalla. En la notificación, haga clic en **Ir al recurso** para ver el recurso de Cognitive Services que creó. 

![Ir al recurso de Cognitive Services](media/cognitive-services-apis-create-account/cog-serv-go-to-resource.png)

En el panel de inicio rápido que se abre, puede acceder al punto de conexión y a la clave.

![Obtención de una clave y un punto de conexión](media/cognitive-services-apis-create-account/get-cog-serv-keys.png)

[!INCLUDE [cognitive-services-environment-variables](../../includes/cognitive-services-environment-variables.md)]

## <a name="pricing-tiers-and-billing"></a>Planes de tarifas y facturación

Los planes de tarifas (y la cantidad que se factura) se basan en el número de transacciones que se envían con la información de autenticación. Cada plan de tarifa especifica lo siguiente:
* el número máximo de transacciones permitidas por segundo (TPS).
* las características de servicio habilitadas en el plan de tarifa.
* El costo de una cantidad predefinida de transacciones. Por encima de esta cantidad se producirá un cargo adicional, tal como se especifica en los [detalles de precios](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) del servicio.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere limpiar y eliminar una suscripción a Cognitive Services, puede eliminar el recurso o grupo de recursos. Al eliminar el grupo de recursos también se eliminan los demás recursos incluidos en el grupo.

Para quitar un grupo de recursos desde Azure Portal:

1. En Azure Portal, expanda el menú de la izquierda para abrir el menú de servicios y elija **Grupos de recursos** para ver una lista con sus grupos de recursos.
2. Busque el grupo de recursos que desea eliminar y haga clic con el botón derecho en el botón Más (... ) situado en la parte derecha de la lista.
3. Seleccione **Eliminar grupo de recursos** y confirme.

## <a name="see-also"></a>Otras referencias

* [Autenticación de solicitudes en Azure Cognitive Services](authentication.md)
* [¿Qué es Azure Cognitive Services?](Welcome.md)
* [Compatibilidad con idiomas naturales](language-support.md)
* [Compatibilidad con contenedores de Docker](cognitive-services-container-support.md)
