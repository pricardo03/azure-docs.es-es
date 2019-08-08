---
title: Creación de un recurso de Cognitive Services en Azure Portal
titlesuffix: Azure Cognitive Services
description: Comience a usar Azure Cognitive Services mediante la creación y suscripción a un recurso en Azure Portal.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 07/16/2019
ms.author: aahi
ms.openlocfilehash: e9616f3014288e4b2580f474c49c646928db5a08
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2019
ms.locfileid: "68334251"
---
# <a name="create-a-cognitive-services-resource-using-the-azure-portal"></a>Creación de un recurso de Cognitive Services con Azure Portal

Use esta guía de inicio rápido para empezar a trabajar con Azure Cognitive Services mediante Azure Portal. Los servicios de Cognitive Services se representan por medio de [recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal) de Azure que se crean en la suscripción de Azure. Después de crear el recurso, use las claves y el punto de conexión generados para autenticar las aplicaciones. 

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure válida: [cree una de forma gratuita](https://azure.microsoft.com/free/).

[!INCLUDE [cognitive-services-subscription-types](../../includes/cognitive-services-subscription-types.md)]

## <a name="create-a-new-azure-cognitive-services-resource"></a>Creación de un nuevo recurso de Azure Cognitive Services

Antes de crear un recurso de Cognitive Services, debe tener un grupo de recursos de Azure que contenga el recurso. Al crear un nuevo recurso, puede elegir entre crear un grupo de recursos o usar uno existente. En este artículo se muestra cómo crear un nuevo grupo de recursos.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y haga clic en **+Crear un recurso**.

    ![Seleccione Cognitive Services APIs.](media/cognitive-services-apis-create-account/azurePortalScreenMulti.png)

2. Puede encontrar los Cognitive Services disponibles con los métodos siguientes:
    * Use la barra de búsqueda y escriba el nombre del servicio al que quiere suscribirse.
        * Para crear un recurso para varios servicios, escriba **Cognitive Services** en la barra de búsqueda y seleccione el recurso **Cognitive Services**.

        ![Búsqueda de Cognitive Services](media/cognitive-services-apis-create-account/azureCogServSearchMulti.png)

    * Para ver todos los Cognitive Services disponibles, seleccione **IA y Machine Learning** en **Azure Marketplace**. Si no ve el servicio que le interesa, haga clic en **Ver todo** y desplácese hasta **Cognitive Services**. Haga clic en **Más** para ver el catálogo completo de Cognitive Services APIs.
    
        ![Seleccione Cognitive Services APIs.](media/cognitive-services-apis-create-account/azureMarketplace.png)

3. En la página **Crear**, proporcione la siguiente información:

    > [!IMPORTANT]
    > No olvide su ubicación de Azure, ya que puede necesitarla cuando llame a Azure Cognitive Services.

    |    |    |
    |--|--|
    | **Nombre** | Nombre descriptivo para el recurso de Cognitive Services. Por ejemplo, *MyCognitiveServicesAccount*. |
    | **Suscripción** | Seleccione una de las suscripciones de Azure disponibles. |
    | **Ubicación** | Ubicación de la instancia de Cognitive Services. Las diferentes ubicaciones pueden crear latencias, pero no tienen ningún impacto en la disponibilidad del tiempo de ejecución del recurso. |
    | **Plan de tarifa** | Costo de la cuenta de Cognitive Services, que depende del uso y de las opciones que elija. Para obtener más información, consulte los [detalles de los precios](https://azure.microsoft.com/pricing/details/cognitive-services/).
    | **Grupos de recursos** | [Grupo de recursos de Azure](https://docs.microsoft.com/azure/architecture/cloud-adoption/governance/resource-consistency/azure-resource-access#what-is-an-azure-resource-group) que contendrá su recurso de Cognitive Services. Puede crear un nuevo grupo o agregarlo a uno ya existente. |

    ![Pantalla de creación de recursos](media/cognitive-services-apis-create-account/resource_create_screen.png)


## <a name="get-the-keys-for-your-resource"></a>Obtención de las claves del recurso

Después de crear el recurso, puede obtener acceso al mismo desde el panel de Azure si lo ancló allí. De lo contrario, puede encontrarlo en la opción **Grupos de recursos**. Después de seleccionar el recurso, puede obtener las claves si selecciona **Claves** en **Administración de recursos**.

[!INCLUDE [cognitive-services-environment-variables](../../includes/cognitive-services-environment-variables.md)]

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
