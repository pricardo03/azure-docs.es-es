---
title: Compatibilidad con los contenedores
titleSuffix: Azure Cognitive Services
description: Aprenda cómo crear un recurso de instancia de contenedor de Azure.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 7/3/2019
ms.author: dapine
ms.openlocfilehash: 38addf4651373ba0f4df411325218a255c835508
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2019
ms.locfileid: "67717152"
---
## <a name="create-an-azure-container-instance-resource"></a>Crear un recurso de instancia de contenedor de Azure

1. Vaya a la página [Crear](https://ms.portal.azure.com/#create/Microsoft.ContainerInstances) de las instancias de contenedor.

2. En la pestaña **Datos básicos**, escriba la siguientes información:

    |Configuración|Valor|
    |--|--|
    |Subscription|Seleccione su suscripción.|
    |Grupos de recursos|Seleccione el grupo de recursos disponible o cree uno nuevo, como `cognitive-services`.|
    |Nombre del contenedor|Escriba un nombre como `cognitive-container-instance`. Este nombre debe estar en minúsculas.|
    |Location|Seleccione una región para la implementación.|
    |Tipo de imagen|`Public`|
    |Nombre de la imagen|Escriba la ubicación del contenedor de Cognitive Services. La ubicación puede ser la misma que se usa en el comando `docker pull`; consulte las [imágenes y los repositorios de contenedor](../../cognitive-services-container-support.md#container-repositories-and-images) para ver los nombres de imagen disponibles y su repositorio correspondiente.|
    |Tipo de SO|`Linux`|
    |Size|Seleccione el tamaño de las recomendaciones sugeridas para su contenedor específico de Cognitive Services:<br>2 núcleos de CPU<br>4 GB

3. En la pestaña **Redes**, escriba los siguientes detalles:

    |Configuración|Valor|
    |--|--|
    |Puertos|Establezca el puerto TCP en `5000`. Expone el contenedor en el puerto 5000.|

4. En la pestaña **Opciones avanzadas**, escriba las **Variables de entorno** necesarias para la [configuración de la facturación](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers#billing-arguments) del recurso de ACI:

    | Clave | Valor |
    |--|--|
    |`apikey`|Se copia desde la página **Claves** del recurso de Text Analytics. Es una cadena de 32 caracteres alfanuméricos sin espacios ni guiones, `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
    |`billing`|Se copia desde la página **Información general** del recurso de Text Analytics. Ejemplo: `https://northeurope.api.cognitive.microsoft.com/text/analytics/v2.0`|
    |`eula`|`accept`|

1. Haga clic en **Revisar y crear**.
1. Después de realizar correctamente la validación, haga clic en **Crear** para finalizar el proceso de creación.
1. Cuando el recurso se implemente con éxito, estará listo.