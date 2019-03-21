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
ms.openlocfilehash: a9f49af54f391b159f8b3d626fffc36635f5e51f
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56821317"
---
# <a name="how-to-move-your-limited-trial-project-to-azure-using-the-customvisionai-site"></a>Cómo mover el proyecto de prueba limitado a Azure mediante el sitio CustomVision.ai


Custom Vision Service está ahora en [versión preliminar de Azure](https://azure.microsoft.com/services/preview/), se acaba el soporte para los proyectos de prueba limitado fuera de Azure. Este documento le mostrará cómo usar el [sitio Web de Custom Vision](https://customvision.ai) para mover el proyecto de prueba limitado va a asociar a un recurso de Azure. 

> [!NOTE]
> Al mover los proyectos de visión personalizada a un recurso de Azure heredar subyacente [permisos]( https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) de ese recurso de Azure. Si otros usuarios de su organización son propietarios del proyecto se encuentra en el recurso de Azure, podrán tener acceso a su proyecto en el [sitio Web de Custom Vision](https://customvision.ai). Del mismo modo, eliminará los proyectos de eliminar los recursos.  


Para obtener una introducción a los conceptos de las suscripciones y recursos de Azure, consulte el [guía para desarrolladores de Azure.](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#manage-your-subscriptions)


## <a name="prerequisites"></a>Requisitos previos

Necesitará una suscripción válida a Azure asociada con la misma cuenta de Microsoft o una cuenta de Azure Active Directory (AAD) que usa para iniciar sesión en el [sitio Web de Custom Vision](https://customvision.ai). 

Si no tienes una cuenta de Azure, [crear una cuenta](https://azure.microsoft.com/free/) de forma gratuita.


## <a name="create-custom-vision-resources-in-the-azure-portal"></a>Crear recursos de visión personalizada en el portal de Azure
Para usar Custom Vision Service con Azure, deberá crear los recursos de Custom Vision entrenamiento y predicción de la [portal Azure](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_customvision#create/Microsoft.CognitiveServicesCustomVision). 

 Para mover el proyecto con este [sitio Web de Custom Vision](https://customvision.ai) experiencia, debe crear los recursos en la región sur de EE. UU, dado que todos los proyectos de prueba limitado se hospedan en el sur de EE. UU. 

Varios proyectos se pueden asociados a un único recurso. Más detalles sobre [precios y límites](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/limits-and-quotas) está disponible. Para seguir usando Custom Vision Service de forma gratuita, puede seleccionar el nivel F0 en Azure portal. 


## <a name="move-your-limited-trial-project-to-an-azure-resource"></a>Mover el proyecto de prueba limitado a un recurso de Azure

1.  En el explorador web, vaya a la [sitio Web de Custom Vision](https://customvision.ai) y seleccione __inicie sesión en__. Abra el proyecto al que desea migrar a una cuenta de Azure. 
2.  Abra la página de configuración para el proyecto haciendo clic en el icono de engranaje en la esquina superior derecha de la pantalla. 

    ![Configuración del proyecto es el icono de engranaje en la parte superior derecha de la página del proyecto.](./media/move-your-project-to-azure/settings-icon.png)


3. Haga clic en __mover a Azure__.

    ![Mover al botón de Azure está en la parte inferior izquierda de la página de configuración del proyecto.](./media/move-your-project-to-azure/move-to-azure.jpg)


4. En la lista desplegable en el __mover a Azure__ botón, seleccione el recurso de Azure que desea mover el proyecto. Haga clic en __mover__. 

5. Si no ve el recurso de Azure que creó anteriormente para Custom Vision Service, es posible en otro directorio. Para mover el proyecto a un recurso en otro directorio, siga las instrucciones siguientes. 

    ![Ventana de migración del proyecto.](./media/move-your-project-to-azure/Project_Migration_Window.jpg)


## <a name="move-project-to-another-azure-directory"></a>Mueva el proyecto a otro directorio de Azure 

> [!NOTE]
> En el portal de Azure y CustomVision.ai, puede seleccionar el directorio en el menú de usuario de la lista desplegable en la esquina superior derecha de la pantalla.   


1. Identifique qué directorio se encuentra el recurso de Azure en. Puede encontrar el directorio enumerado en el nombre de usuario en la parte superior derecha de la barra de menú de Azure portal. 

    ![El directorio aparece en el nombre de usuario en la parte superior derecha de la barra de menú de Azure portal. .](./media/move-your-project-to-azure/identify_directory.jpg)

2. Busque el identificador de recurso del recurso de aprendizaje de visión personalizada. Puede encontrarlo en Azure portal, abra el recurso de aprendizaje de Custom Vision y seleccione "Propiedades" en la sección "Administración de recursos". El identificador de recurso será no existe. 

    ![Encontrar el identificador de recurso en Azure portal, abra el recurso de aprendizaje de Custom Vision y seleccione "Propiedades" en la sección "Administración de recursos".](./media/move-your-project-to-azure/resource_ID_azure_portal.jpg)


3. Como alternativa, puede encontrar el identificador de recurso del recurso Custom Vision directamente en el sitio Web de Custom Vision [página configuración]( https://www.customvision.ai/projects#/settings). Deberá cambiar en el mismo directorio que se encuentra el recurso de Azure en.

    ![Se muestra el identificador de recursos para cada recurso en la página de configuración en el sitio Web de Custom Vision.](./media/move-your-project-to-azure/resource_ID_CVS_portal.jpg)

4. Ahora que tiene el identificador de recursos, vuelva al proyecto Custom Vision que está intentando mover de una versión de prueba limitado a un recurso de Azure. Recordatorio, es posible que deba volver a su directorio original para encontrarlo. Siga las instrucciones proporcionadas [anteriormente](#move-your-limited-trial-project-to-an-azure-resource) para abrir la página Configuración del proyecto y seleccione __mover a Azure__. 


5. En el paso a la ventana de Azure, active la casilla para "Mover a otro directorio de Azure?". Seleccione el directorio que desea mover el proyecto y escriba el identificador de recurso del recurso que se va a mover el proyecto. Haga clic en __mover__. 



5. Recuerde que el proyecto está ahora en un directorio diferente. Para buscar el proyecto, deberá cambiar en el mismo directorio en el portal web de Custom Vision que el proyecto se encuentra en. En el Azure portal y el [sitio Web de Custom Vision](https://customvision.ai), puede seleccionar el directorio en el menú desplegable cuenta en la esquina superior derecha de la pantalla. 

## <a name="next-steps"></a>Pasos siguientes

Ahora, el proyecto se ha movido a un recurso de Azure. Deberá actualizar las claves de entrenamiento y predicción en las aplicaciones que ha escrito.
