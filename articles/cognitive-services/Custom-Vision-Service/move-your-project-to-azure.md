---
title: Migración de un proyecto de prueba limitada a Azure
titlesuffix: Azure Cognitive Services
description: Aprenda a migrar un proyecto de prueba limitada a Azure.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: anroth
ms.openlocfilehash: 6fac6531ea0a39796de13f95aee33b30dc91f131
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60816518"
---
# <a name="how-to-move-your-limited-trial-project-to-azure"></a>Cómo migrar el proyecto de prueba limitada a Azure

Cuando Custom Vision Service finalice su migración a Azure, se dejarán de respaldar los proyectos de prueba limitada fuera de Azure. En este documento se muestra cómo usar las API de Custom Vision para copiar el proyecto de prueba limitada en un recurso de Azure.

La posibilidad de ver proyectos de prueba limitada en el [sitio web de Custom Vision](https://customvision.ai) finalizó el 25 de marzo de 2019. En este documento se muestra ahora cómo usar las API de Custom Vision con un [script de Python de migración](https://github.com/Azure-Samples/custom-vision-move-project) en GitHub) para duplicar el proyecto en un recurso de Azure.

Para más información, incluidos los plazos clave del proceso de desuso de la prueba limitado, consulte las [notas de la versión](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/release-notes#february-25-2019) o las comunicaciones por correo electrónico enviadas a los propietarios de los proyectos de prueba limitada.

El [script de migración](https://github.com/Azure-Samples/custom-vision-move-project) permite volver a crear un proyecto mediante la descarga y luego la carga de todas las etiquetas, regiones e imágenes de la iteración actual. Le dejará un nuevo proyecto en su nueva suscripción que puede entrenar.

## <a name="prerequisites"></a>Requisitos previos

- Necesitará una suscripción válida a Azure asociada con la cuenta Microsoft o una cuenta de Azure Active Directory (AAD) que quiera usar para iniciar sesión en el [sitio web de Custom Vision](https://customvision.ai). 
    - Si no tiene una cuenta de Azure, [cree una](https://azure.microsoft.com/free/) de forma gratuita.
    - Para una introducción a los conceptos de las suscripciones y recursos de Azure, consulte la [guía para desarrolladores de Azure.](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#manage-your-subscriptions).
-  [Python](https://www.python.org/downloads/)
- [Pip](https://pip.pypa.io/en/stable/installing/)

## <a name="create-custom-vision-resources-in-the-azure-portal"></a>Creación de recursos de Custom Vision en Azure Portal

Para usar Custom Vision Service con Azure, deberá crear recursos de entrenamiento y predicción de Custom Vision en [Azure Portal](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_customvision#create/Microsoft.CognitiveServicesCustomVision). 

Varios proyectos pueden estar asociados a un único recurso. Más información sobre [precios y límites](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/limits-and-quotas). Para seguir usando Custom Vision Service de forma gratuita, puede seleccionar el nivel F0 en Azure Portal. 

> [!NOTE]
> Al mover el proyecto de Custom Vision a un recurso de Azure, hereda los [permisos]( https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) subyacentes de ese recurso de Azure. Si otros usuarios de su organización son propietarios del recurso de Azure donde se encuentra el proyecto, podrán tener acceso a este en el [sitio web de Custom Vision](https://customvision.ai). Del mismo modo, al eliminar los recursos se eliminan los proyectos.  

## <a name="find-your-limited-trial-project-information"></a>Búsqueda de información del proyecto de prueba limitada

Para mover el proyecto, necesitará su _identificador del proyecto_ y _clave entrenamiento_. Si no tiene esta información, visite [https://limitedtrial.customvision.ai/projects](https://limitedtrial.customvision.ai/projects) para obtener el identificador y la clave de cada uno de sus proyectos. 

## <a name="use-the-python-sample-code-to-copy-your-project-to-azure"></a>Uso del código de ejemplo de Python para copiar el proyecto en Azure

Siga las [instrucciones del código de ejemplo](https://github.com/Azure-Samples/custom-vision-move-project), y use su clave e identificador de proyecto de prueba limitada como materiales de "origen" y la clave del nuevo recurso de Azure que creó como "destino".

De forma predeterminada, todos los proyectos de prueba limitada se hospedan en la región Centro y Sur de EE. UU.

## <a name="next-steps"></a>Pasos siguientes

El proyecto se ha movido ahora a un recurso de Azure. Deberá actualizar las claves de entrenamiento y predicción en las aplicaciones que ha escrito.

Para ver el proyecto en el [sitio web de Custom Vision](https://customvision.ai), inicie sesión con la misma cuenta que usó para iniciar sesión en Azure Portal. Si no ve el proyecto, confirme que está en el mismo directorio del [sitio web de Custom Vision](https://customvision.ai) que el directorio donde se encuentran los recursos en Azure Portal. En Azure Portal y CustomVision.ai, puede seleccionar el directorio en el menú de usuario desplegable de la esquina superior derecha de la pantalla.