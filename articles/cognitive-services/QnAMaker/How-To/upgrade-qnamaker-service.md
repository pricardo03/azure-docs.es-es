---
title: 'Actualización del servicio QnA Maker: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Cómo actualizar el servicio QnA Maker
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: e8d3713d6729c4e30da9a64a382e9d5a647dfefd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381787"
---
# <a name="upgrade-your-qna-maker-service"></a>Actualización del servicio QnA Maker
Puede elegir actualizar componentes individuales de la pila de QnA Maker tras la creación inicial. Consulte los detalles de los componentes dependientes y de la selección de la SKU [aquí](https://aka.ms/qnamaker-docs-capacity).

## <a name="upgrade-qna-maker-management-sku"></a>Actualización de la SKU de administración de QnA Maker
Para actualizar la SKU de administración de QnA Maker:
1. Vaya al recurso QnA Maker en Azure Portal y seleccione **Plan de tarifa**.

    ![Recurso QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource.png)

2. Elija la SKU adecuada y presione **Seleccionar**.

    ![Precios de QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-pricing-page.png)

## <a name="upgrade-app-service"></a>Actualizar App Service
Puede [escalar](https://docs.microsoft.com/azure/app-service/web-sites-scale) o reducir verticalmente App Service.

1. Vaya al recurso App Service en Azure Portal y seleccione **Escalar verticalmente** o **Reducir verticalmente** según proceda.

    ![Escalado de App Service en QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-scale.png)

## <a name="upgrade-azure-search-service"></a>Actualización del servicio Azure Search
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
