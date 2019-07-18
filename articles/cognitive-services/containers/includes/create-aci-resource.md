---
title: Compatibilidad con los contenedores
titleSuffix: Azure Cognitive Services
description: Cómo crear un recurso de instancia de contenedor de Azure (ACI).
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: b24a78873c3220b969fc548d2553e465e80773cd
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67455070"
---
## <a name="create-an-azure-container-instance-aci-resource"></a>Crear un recurso de instancia de contenedor de Azure (ACI)

1. Vaya a la página [Create](https://ms.portal.azure.com/#create/Microsoft.ContainerInstances) (Crear) de las instancias de contenedor.

2. En la pestaña **Basics** (Aspectos básicos), escriba los siguientes detalles:

    |Configuración|Valor|
    |--|--|
    |Subscription|Seleccione su suscripción.|
    |Grupos de recursos|Seleccione el grupo de recursos disponible o cree uno nuevo, como `cognitive-services`.|
    |Nombre del contenedor|Escriba un nombre como `cognitive-container-instance`. Este nombre debe estar en minúsculas.|
    |Location|Seleccione una región para la implementación.|
    |Tipo de imagen|`Public`|
    |Nombre de la imagen|Escriba la ubicación del contenedor de Cognitive Services. Esta puede ser la misma ubicación que usó en el comando `docker pull`, _como_: <br>`mcr.microsoft.com/azure-cognitive-services/sentiment`|
    |Tipo de SO|`Linux`|
    |Size|Cambie el tamaño de las recomendaciones sugeridas del contenedor específico de Cognitive Services:<br>2 núcleos<br>4 GB

3. En la pestaña **Networking** (Redes), escriba los siguientes detalles:

    |Configuración|Valor|
    |--|--|
    |Puertos|Establezca el puerto TCP en `5000`. Expone el contenedor en el puerto 5000.|

4. En la pestaña **Advanced** (Opciones avanzadas), escriba los siguientes detalles para pasar a través del contenedor de la [configuración de facturación requerida](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers#billing-arguments) al recurso de ACI:

    |Clave de la página de opciones avanzadas|Valor de la página de opciones avanzadas|
    |--|--|
    |`apikey`|Copiado desde la página **Claves** del recurso de Text Analytics. Es una cadena de 32 caracteres alfanuméricos sin espacios ni guiones, `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
    |`billing`|Copiado desde la página **Introducción** del recurso de Text Analytics. Ejemplo: `https://northeurope.api.cognitive.microsoft.com/text/analytics/v2.0`|
    |`eula`|`accept`|

1. Haga clic en **Review and Create** (Revisar y crear).
1. Después de realizar correctamente la validación, haga clic en **Create** (Crear) para finalizar el proceso de creación.
1. Cuando el recurso se implemente con éxito, estará listo para su uso.