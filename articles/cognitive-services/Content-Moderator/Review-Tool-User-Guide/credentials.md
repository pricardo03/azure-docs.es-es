---
title: Credenciales en Azure Content Moderator | Microsoft Docs
description: Administre las credenciales de Content Moderator que se van a usar con las API.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/25/2017
ms.author: sajagtap
ms.openlocfilehash: 4531fa4c8bbb7bb9c1daeaaac6f9e7078dae250a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380103"
---
# <a name="manage-credentials"></a>Administración de credenciales

Las credenciales de Content Moderator se crean en las siguientes ubicaciones:

- [Portal de Azure](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator)
- [Herramienta de revisión de Content Moderator](http://contentmoderator.cognitive.microsoft.com/)

En este artículo se explica dónde se pueden encontrar y cómo se relacionan entre sí.

## <a name="the-azure-portal"></a>El Portal de Azure

En el panel de Azure Portal, seleccione su cuenta de Content Moderator. En **Administración de recursos**, seleccione **Claves**. Para copiar la clave, seleccione el icono a la derecha de la clave.

![Claves de Content Moderator en Azure Portal](images/credentials-azure-portal-keys.PNG)

### <a name="how-to-use-your-azure-account-with-the-review-tool"></a>Cómo usar su cuenta de Azure con la herramienta de revisión
Para usar la clave de Azure con las API de revisión, copie la id. de recurso que aparece en la pantalla **Propiedades** de la siguiente captura de pantalla y escríbala en la pantalla de credenciales de la herramienta de revisión en los campos de **Whitelisted Resource Id(s)** (Id de recursos de la lista blanca), tal como se muestra en la siguiente sección **Id. de recurso**. 

Para usar la clave de Azure para los flujos de trabajo disponibles en Content Moderator, escríbala de nuevo en el campo **Ocp-Apim-Subscription-Key** de la sección **Configuración del flujo de trabajo**, tal como se muestra en la siguiente sección **Flujos de trabajo**.

> [!NOTE]
> Después de reemplazar los dos lugares en la herramienta de revisión por la clave y la id. de recurso de la suscripción de Azure, ya no se utilizará su **Trial Ocp-Apim-Subscription-Key** que aparece en la pantalla de credenciales, pero siempre estará disponible.
> La clave de evaluación tiene un límite máximo de 5.000 transacciones al mes a 1 solicitud por segundo (RPS).

![Id. de recurso de Content Moderator en Azure Portal](images/credentials-azure-portal-resourceid.PNG)


## <a name="the-review-tool"></a>Herramienta de revisión

En la pestaña **Configuración** del panel de la herramienta de revisión, seleccione **Credenciales**.

![Credenciales de Content Moderator en la herramienta de revisión](images/credentials-trial-resource-workflow.PNG)

En la siguiente sección se examina la imagen anterior con más detalle:


### <a name="api"></a>API

En la primera parte se indica el **punto de conexión de API de revisión**, la **id. de equipo** y la **Ocp-Apim-Subscription-Key (clave de evaluación de Content Moderator)** generados como parte de la creación del equipo de revisión. Úselos para llamar a todas las Content Moderator API, incluida la API de revisión.

Tenga en cuenta también su identificador de región para el punto de conexión de API. Por ejemplo, **westus** es la región de "https://westus.api.cognitive.microsoft.com/contentmoderator/review/v1.0"

![Clave de Content Moderator en la herramienta de revisión](images/credentials-trialkey.PNG)


### <a name="resource-id"></a>Id. de recurso

Al principio, la segunda parte está vacía sin ninguna id. de recurso. **Para usar su clave de suscripción de Azure con la API de revisión, navegue hasta la pantalla de id. de recurso, tal y como se ha mostrado anteriormente y cópiela en el campo mostrado**. Pulse en **'+'** para guardar la id. de recurso.

> [!NOTE]
> La región de la suscripción de Content Moderator debe coincidir con la región del equipo de revisión para que reconozca el equipo y tenga acceso a los datos de equipo. Por ejemplo, en las imágenes de esta página, la región **West US** **(4)** contiene la suscripción de Azure de Content Moderator y el equipo de revisión.

![Id. de recurso de Content Moderator en la herramienta de revisión](images/credentials-resourceids.PNG)


### <a name="workflows"></a>Flujos de trabajo

La tercera parte muestra la información usada para ejecutar flujos de trabajo. En un principio muestra la clave de evaluación generada automáticamente de forma predeterminada. 

**Actualícela con la clave de Azure cuando obtenga una suscripción de Azure**. Los otros dos campos permiten usar listas de términos e imágenes en las operaciones de texto de la pantalla y evaluación de imagen respectivamente.

![Credenciales del flujo de trabajo de Content Moderator en la herramienta de revisión](images/credentials-workflow.PNG)


## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo usar las credenciales de Content Moderator en los [flujos de trabajo](workflows.md).
