---
title: 'Actualización del servicio QnA Maker: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Comparta o actualice los servicios de QnA Maker para administrar mejor los recursos.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 03/25/2019
ms.author: diberry
ms.openlocfilehash: df4aa2d6a3c4690fb1fc38b0f4f7d49afccdd657
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2019
ms.locfileid: "69640481"
---
# <a name="share-or-upgrade-your-qna-maker-service"></a>Uso compartido o actualización del servicio de QnA Maker
Comparta o actualice los servicios de QnA Maker para administrar mejor los recursos. 

Puede elegir actualizar componentes individuales de la pila de QnA Maker tras la creación inicial. Consulte los detalles de los componentes dependientes y de la selección de la SKU [aquí](https://aka.ms/qnamaker-docs-capacity).

## <a name="share-existing-services-with-qna-maker"></a>Uso compartido de los servicios existentes con QnA Maker

QnA Maker crea varios recursos de Azure. Con el fin de simplificar la administración y beneficiarse del uso compartido de los costos, use la siguiente tabla para comprender lo que puede y no puede compartir:

|Servicio|Compartir|
|--|--|
|Cognitive Services|X|
|Plan del Servicio de aplicaciones|✔|
|App Service|X|
|Application Insights|✔|
|Servicio de búsqueda|✔|

## <a name="upgrade-qna-maker-management-sku"></a>Actualización de la SKU de administración de QnA Maker

Cuando necesite tener más preguntas y respuestas en Knowledge Base, más allá de su plan actual, actualice el plan de tarifa del servicio de QnA Maker. 

Para actualizar la SKU de administración de QnA Maker:

1. Vaya al recurso QnA Maker en Azure Portal y seleccione **Plan de tarifa**.

    ![Recurso QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource.png)

2. Elija la SKU adecuada y presione **Seleccionar**.

    ![Precios de QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-pricing-page.png)

## <a name="upgrade-app-service"></a>Actualizar App Service

 Cuando Knowledge Base deba atender más solicitudes de la aplicación cliente, actualice el plan de tarifa de App Service.

Puede [escalar](https://docs.microsoft.com/azure/app-service/manage-scale-up) o reducir verticalmente App Service.

1. Vaya al recurso App Service en Azure Portal y seleccione **Escalar verticalmente** o **Reducir verticalmente** según proceda.

    ![Escalado de App Service en QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-scale.png)

## <a name="upgrade-azure-search-service"></a>Actualización del servicio Azure Search

Si planea tener muchas instancias de Knowledge Base, actualice el plan de tarifa del servicio de Azure Search. 

Actualmente, no es posible realizar una actualización en contexto de la SKU de Azure Search. Sin embargo, puede crear un recurso de Azure Search con la SKU deseada, restaurar los datos al nuevo recurso y luego vincularlo a la pila de QnA Maker.

1. Cree un recurso de Azure Search en Azure Portal y elija la SKU deseada.

    ![Recurso Azure Search en QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-new.png)

2. Restaure los índices a partir del recurso Azure Search original en uno nuevo. Vea el código de ejemplo de restauración de copia de seguridad [aquí](https://github.com/pchoudhari/QnAMakerBackupRestore).

3. Una vez restaurados los datos, vaya al nuevo recurso Azure Search, seleccione **Claves** y anote el **Nombre** y la **Clave de administrador**.

    ![Claves de Azure Search en QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-keys.png)

4. Para vincular el nuevo recurso Azure Search a la pila de QnA Maker, vaya a App Service en QnA Maker.

    ![App Service en QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource-list-appservice.png)

5. Seleccione **Configuración de la aplicación** y reemplace los campos **AzureSearchName** y **AzureSearchAdminKey** del paso 3.

    ![Configuración de App Service en QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-settings.png)

6. Reinicie App Service.

    ![Reinicio de App Service en QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Usar QnA Maker API](../Quickstarts/csharp.md)
