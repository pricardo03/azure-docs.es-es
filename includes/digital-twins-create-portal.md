---
title: archivo de inclusión
description: archivo de inclusión
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: include
ms.date: 12/17/2018
ms.author: dkshir
ms.custom: include file
ms.openlocfilehash: 0c89c8353d410ddd54e8a59f3838c914702b96ca
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66153706"
---
1. Inicie sesión en el [Azure Portal](http://portal.azure.com).

1. En el panel izquierdo, seleccione **Crear un recurso**. Busque **Digital Twins** y seleccione **Digital Twins (preview)** (Digital Twins [versión preliminar]). Haga clic en **Crear** para iniciar el proceso de implementación.

   ![Selecciones para crear una nueva instancia de Digital Twins](./media/create-digital-twins-portal/create-digital-twins.png)

1. En el panel **Digital Twins**, escriba la siguiente información:
   * **Nombre del recurso**: cree un nombre único para la instancia de Digital Twins.
   * **Suscripción**: elija la suscripción que quiere usar para crear esta instancia de Digital Twins. 
   * **Grupo de recursos**: seleccione o cree un [grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) para la instancia de Digital Twins.
   * **Ubicación**: seleccione la ubicación más cercana a sus dispositivos.

     ![Panel de Digital Twins con información especificada](./media/create-digital-twins-portal/create-digital-twins-param.png)

1. Revise la información de Digital Twins y, a continuación, seleccione **Crear**. La instancia de Digital Twins podría tardar unos minutos en crearse. Puede ver el progreso en el panel **Notificaciones**.

1. Abra el panel **Información general** de la instancia de Digital Twins. Observe el vínculo situado bajo **Management API**.

   La dirección URL de **Management API** tiene el formato siguiente: `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/swagger`. Esta dirección URL le lleva a la documentación de la API REST de Azure Digital Twins que se aplica a la instancia. Lea [Uso de Azure Digital Twins Swagger](../articles/digital-twins/how-to-use-swagger.md) para aprender a leer y usar esta documentación de API.

    Modifique la dirección URL de **Management API** para que tenga este formato: `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/`. La aplicación usará la dirección URL modificada como dirección URL base para acceder a la instancia. Copie esta dirección URL modificada en un archivo temporal. La necesitará en la siguiente sección.

    ![API de administración](./media/create-digital-twins-portal/digital-twins-management-api.png)