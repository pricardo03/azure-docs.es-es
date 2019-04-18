---
title: Mover un proyecto de prueba limitado a Azure
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre cómo mover un proyecto de prueba limitado a Azure.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: anroth
ms.openlocfilehash: 6fac6531ea0a39796de13f95aee33b30dc91f131
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59274457"
---
# <a name="how-to-move-your-limited-trial-project-to-azure"></a>Cómo mover el proyecto de prueba limitado a Azure

Custom Vision Service finaliza su migración a Azure, finalizará el soporte técnico para los proyectos de prueba limitado fuera de Azure. Este documento le mostrará cómo usar las API de Custom Vision para copiar el proyecto de prueba limitada en un recurso de Azure.

Soporte técnico para ver los proyectos de prueba limitada en el [sitio Web de Custom Vision](https://customvision.ai) finalizó el 25 de marzo de 2019. Ahora este documento muestra cómo usar las API de visión personalizada con un [script de python de migración](https://github.com/Azure-Samples/custom-vision-move-project) en GitHub) para duplicar el proyecto para un recurso de Azure.

Para más detalles, incluidos los plazos de clave en el proceso de degradación de prueba limitado, consulte el [notas de la versión](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/release-notes#february-25-2019) o a las comunicaciones de correo electrónico enviadas a los propietarios de los proyectos de prueba limitados.

El [script de migración](https://github.com/Azure-Samples/custom-vision-move-project) permite volver a crear un proyecto mediante la descarga y, a continuación, cargar todas las etiquetas, regiones y las imágenes de la iteración actual. Dejará un nuevo proyecto en su nueva suscripción que, a continuación, puede entrenar.

## <a name="prerequisites"></a>Requisitos previos

- Necesitará una suscripción válida a Azure asociada con la cuenta de Microsoft o una cuenta de Azure Active Directory (AAD) que desea usar para iniciar sesión en el [sitio Web de Custom Vision](https://customvision.ai). 
    - Si no tienes una cuenta de Azure, [crear una cuenta](https://azure.microsoft.com/free/) de forma gratuita.
    - Para obtener una introducción a los conceptos de las suscripciones y recursos de Azure, consulte el [guía para desarrolladores de Azure.](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#manage-your-subscriptions).
-  [Python](https://www.python.org/downloads/)
- [Pip](https://pip.pypa.io/en/stable/installing/)

## <a name="create-custom-vision-resources-in-the-azure-portal"></a>Crear recursos de visión personalizada en el portal de Azure

Para usar Custom Vision Service con Azure, deberá crear los recursos de Custom Vision entrenamiento y predicción de la [portal Azure](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_customvision#create/Microsoft.CognitiveServicesCustomVision). 

Varios proyectos se pueden asociados a un único recurso. Más detalles sobre [precios y límites](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/limits-and-quotas) está disponible. Para seguir usando Custom Vision Service de forma gratuita, puede seleccionar el nivel F0 en Azure portal. 

> [!NOTE]
> Al mover el proyecto de visión personalizada a un recurso de Azure, hereda subyacente [permisos]( https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) de ese recurso de Azure. Si otros usuarios de su organización son propietarios del proyecto se encuentra en el recurso de Azure, podrán tener acceso a su proyecto en el [sitio Web de Custom Vision](https://customvision.ai). Del mismo modo, eliminará los proyectos de eliminar los recursos.  

## <a name="find-your-limited-trial-project-information"></a>Buscar la información del proyecto de prueba limitado

Para mover el proyecto, necesitará el _identificador del proyecto_ y _clave entrenamiento_ para el proyecto está intentando migrar. Si no tiene esta información, visite [ https://limitedtrial.customvision.ai/projects ](https://limitedtrial.customvision.ai/projects) para obtener el identificador y la clave para cada uno de sus proyectos. 

## <a name="use-the-python-sample-code-to-copy-your-project-to-azure"></a>Utilice el código de ejemplo de Python para copiar el proyecto en Azure

Siga el [instrucciones de código de ejemplo](https://github.com/Azure-Samples/custom-vision-move-project), con su clave de prueba limitada y el identificador del proyecto como los materiales de "origen" y la clave desde el nuevo recurso de Azure que creó como el "destino".

De forma predeterminada, todos los proyectos de prueba limitado se hospedan en la región South Central US Azure.

## <a name="next-steps"></a>Pasos siguientes

Ahora, el proyecto se ha movido a un recurso de Azure. Deberá actualizar las claves de entrenamiento y predicción en las aplicaciones que ha escrito.

Para ver el proyecto en el [sitio Web de Custom Vision](https://customvision.ai), inicie sesión con la misma cuenta que usó para iniciar sesión en el portal de Azure. Si no ve el proyecto, confirme que está en el mismo directorio en el [sitio Web de Custom Vision](https://customvision.ai) como el directorio donde se encuentran los recursos en Azure portal. En el portal de Azure y CustomVision.ai, puede seleccionar el directorio en el menú de usuario de la lista desplegable en la esquina superior derecha de la pantalla.