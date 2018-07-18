---
title: Creación de una cuenta de Cognitive Services APIs en Azure Portal | Microsoft Docs
description: Creación de una cuenta de Microsoft Cognitive Services APIs en Azure Portal.
services: cognitive-services
documentationcenter: ''
author: garyericson
manager: cgronlun
editor: ''
ms.assetid: b6176bb2-3bb6-4ebf-84d1-3598ee6e01c6
ms.service: cognitive-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2018
ms.author: garye
ms.reviewer: gibattag
ms.openlocfilehash: 3697dd0628f0028cb486139e92c032f0d757c6ed
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380887"
---
# <a name="create-a-cognitive-services-apis-account-in-the-azure-portal"></a>Creación de una cuenta de Cognitive Services APIs en Azure Portal

Para usar Microsoft Cognitive Service APIs, primero debe crear una cuenta en Azure Portal.

1. Inicie sesión en el [Azure Portal](http://portal.azure.com).

2. Haga clic en **+ Crear un recurso**.

3. En Azure Marketplace, seleccione **IA y Cognitive Services** y descubra la lista de las API disponibles. Haga clic en **Ver todos** para ver la lista completa de Cognitive Services APIs. Haga clic en la API de su elección para continuar.

    ![Seleccione Cognitive Services APIs.](media/cognitive-services-apis-create-account/select-cognitive-services-apis.png)

4. En la página **Crear**, proporcione la siguiente información:

   - **Nombre de cuenta:** nombre de la cuenta. Se recomienda usar un nombre descriptivo, por ejemplo *AFaceAPIAccount*.

   - **Suscripción:** seleccione una de las suscripciones de Azure disponibles en las que tenga al menos permisos de colaborador.

   - **Tipo de API:** elija la API de Cognitive Services que desee usar. Para obtener más información sobre las diversas API de Cognitive Services APIs disponibles, visite el sitio de [Cognitive Services](https://azure.microsoft.com/services/cognitive-services/).

   - **Plan de tarifa:** el costo de la cuenta de Cognitive Services depende del uso real y de las opciones que elija. Para obtener más información sobre los precios de cada API, consulte las [páginas de precios](https://azure.microsoft.com/pricing/details/cognitive-services/).

   - **Grupo de recursos**: un grupo de recursos es una colección de recursos que comparten el mismo ciclo de vida, los mismos permisos e idénticas directivas. Para obtener más información sobre los grupos de recursos, consulte [Administración de los recursos de Azure a través del Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

   - **Ubicación del grupo de recursos:** esto solo es necesario si la API seleccionada es global (es decir, no está ligada a una ubicación). Sin embargo, si la API es global y no está ligada a una ubicación, debe especificar una ubicación para el grupo de recursos donde residen los metadatos asociados con la cuenta de la API de Cognitive Services. Esta ubicación no afecta a la disponibilidad de runtime de la cuenta. Para obtener más información sobre el grupo de recursos, consulte [Administración de los recursos de Azure a través del Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

   - **Confirmación explícita de las condiciones de los servicios en línea:** con el fin de crear una cuenta, los propietarios de las suscripciones o los colaboradores (tal como se define en [Control de acceso basado en rol de Azure](https://docs.microsoft.com/azure/role-based-access-control/overview)) deben confirmar explícitamente los términos aplicables a Cognitive Services en las [condiciones de los servicios en línea](https://www.microsoft.com/en-us/Licensing/product-licensing/products.aspx). 

     El propietario de la suscripción puede deshabilitar la creación de la cuenta de Cognitive Services para una suscripción o un grupo de recursos específicos a través de las [directivas de Azure](../azure-policy/azure-policy-introduction.md). Para ello, deben seguir el artículo sobre el [uso de Azure Portal para asignar y administrar directivas de recursos](../azure-policy/assign-policy-definition.md), asignar una definición de directiva "Tipos de recursos no permitidos" y especificar **Microsoft.CognitiveServices/accounts** como tipo de recurso de destino.

     Si la creación de la cuenta estuviese deshabilitada, se mostraría el siguiente error en el momento de realizar la operación:

     ![Error de creación de cuenta](media/cognitive-services-apis-create-account/error-message.png)

5. Para anclar la cuenta al panel de Azure Portal, haga clic en **Anclar al panel**.

6. Haga clic en **Crear** para crear la cuenta.

Después de que se haya implementado correctamente la cuenta de Cognitive Services, haga clic en el icono del panel para ver la información de cuenta.

Puede usar la **Dirección URL del extremo** de la sección **Información general** y las claves de la sección **Claves** para empezar a hacer llamadas API en sus aplicaciones.

![Mostrar información de cuenta](media/cognitive-services-apis-create-account/display-account.png)

![Mostrar de claves de cuenta](media/cognitive-services-apis-create-account/account-keys.png)

### <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre todos los servicios disponibles de Microsoft Cognitive Services, consulte [Cognitive Services](https://azure.microsoft.com/services/cognitive-services/).

Para ver las guías de inicio rápido y obtener algunos ejemplos de Cognitive Services APIs:

 - [Inicio rápido en Computer Vision en C#](/computer-vision/quickstarts/csharp.md)
 - [Text Analytics con Python](/text-analytics/quickstarts/python.md)
 - [Face API con JavaScript](/face/quickstarts/javascript.md)
