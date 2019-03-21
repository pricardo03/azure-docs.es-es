---
title: 'Administración de credenciales en Azure Content Moderator: Content Moderator'
titlesuffix: Azure Cognitive Services
description: Administre las credenciales de Content Moderator que necesitará para usar con las API.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 0da6b6b0fef0f998e20789253b2a65c54121532c
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58260016"
---
# <a name="manage-content-moderator-service-credentials"></a>Administración de las credenciales de servicio de Content Moderator

Las credenciales de Content Moderator se crean en las siguientes ubicaciones:

- [Portal de Azure](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator)
- [Herramienta de revisión de Content Moderator](https://contentmoderator.cognitive.microsoft.com/)

En este artículo se explica dónde se pueden encontrar y cómo se relacionan entre sí.

## <a name="the-azure-portal"></a>El Portal de Azure

En el panel de Azure Portal, seleccione su cuenta de Content Moderator. En **Administración de recursos**, seleccione **Claves**. Para copiar la clave, seleccione el icono a la derecha de la clave.

![Claves de Content Moderator en Azure Portal](images/credentials-azure-portal-keys.PNG)

### <a name="use-the-azure-account-with-the-review-tool-and-review-api"></a>Uso de la cuenta de Azure con la herramienta y la API de revisión
Para usar la clave de Azure con las API de revisión, copie la id. de recurso que aparece en la pantalla **Propiedades** de la siguiente captura de pantalla y escríbala en la pantalla de credenciales de la herramienta de revisión en los campos de **Whitelisted Resource Id(s)** (Id de recursos de la lista blanca), tal como se muestra en la siguiente sección **Id. de recurso**. 

> [!NOTE]
> La región de la suscripción de Content Moderator debe coincidir con la región del equipo de revisión para que reconozca el equipo y tenga acceso a los datos de equipo. Por ejemplo, en las imágenes de esta página, la región **West US** **(4)** contiene la suscripción de Azure de Content Moderator y el equipo de revisión.
>
> Después de reemplazar los dos lugares en la herramienta de revisión por la clave y la id. de recurso de la suscripción de Azure, ya no se utilizará su **Trial Ocp-Apim-Subscription-Key** que aparece en la pantalla de credenciales, pero siempre estará disponible.
> La clave de evaluación tiene un límite máximo de 5.000 transacciones al mes a 1 solicitud por segundo (RPS).

![Id. de recurso de Content Moderator en Azure Portal](images/credentials-azure-portal-resourceid.PNG)

### <a name="use-the-azure-account-with-the-workflows-in-the-review-tool"></a>Uso de la cuenta de Azure con los flujos de trabajo en la herramienta de revisión

Para usar la clave de Azure para los flujos de trabajo disponibles en Content Moderator, escríbala de nuevo en el campo **Ocp-Apim-Subscription-Key** de la sección **Configuración del flujo de trabajo**, tal como se muestra en la siguiente sección **Flujos de trabajo**. Pulse en **'+'** para guardar la id. de recurso.

![Credenciales del flujo de trabajo de Content Moderator en la herramienta de revisión](images/credentials-workflow.PNG)

## <a name="the-review-tool"></a>Herramienta de revisión

En la pestaña **Configuración** del panel de la herramienta de revisión, seleccione **Credenciales**.

![Credenciales de Content Moderator en la herramienta de revisión](images/credentials-trial-resource-workflow.PNG)

En la siguiente sección se examina la imagen anterior con más detalle:

### <a name="api"></a>API

En la primera parte se indica el **punto de conexión de API de revisión**, la **id. de equipo** y la **Ocp-Apim-Subscription-Key (clave de evaluación de Content Moderator)** generados como parte de la creación del equipo de revisión. Úselos para llamar a todas las Content Moderator API, incluida la API de revisión.

Tenga en cuenta también su identificador de región para el punto de conexión de API. Por ejemplo, **westus** es la región en "https:\//westus.api.cognitive.microsoft.com/contentmoderator/review/v1.0"

![Clave de Content Moderator en la herramienta de revisión](images/credentials-trialkey.PNG)

### <a name="resource-id"></a>Id. de recurso

Este conjunto de campos se trata en la sección anterior, [Uso de la cuenta de Azure con la herramienta de revisión y la API](credentials.md#use-the-azure-account-with-the-review-tool-and-review-api). Este campo normalmente está en blanco a menos que se agregue el identificador de recursos de Azure, como se explicó en la sección anterior.

### <a name="workflows"></a>Flujos de trabajo

Este conjunto de campos se trata en la sección anterior, [Uso de la cuenta de Azure con los flujos de trabajo en la herramienta de revisión](credentials.md#use-the-azure-account-with-the-workflows-in-the-review-tool). De forma predeterminada, la herramienta de revisión usa su clave de prueba generada automáticamente para ejecutar los flujos de trabajo,y eso es lo que se muestra para comenzar. Los otros dos campos permiten usar listas de términos e imágenes en las operaciones de texto de la pantalla y evaluación de imagen respectivamente.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo usar las credenciales de Content Moderator en los [flujos de trabajo](workflows.md).
